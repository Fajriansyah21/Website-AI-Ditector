# Database Schema v1.0

**Status:** Proposed logical schema for pilot; not a migration or deployed database.  
**Purpose:** preserve provenance, annotations, edit lineage, source permissions, and split integrity.

## Design rules

- Keep participant identity/contact information in a separate restricted system. Dataset tables use pseudonymous IDs only.
- Raw text is immutable after ingestion. Corrections create a new version or a documented correction event.
- Use stable IDs and foreign keys. Never use row order as identity.
- Store timestamps in UTC; store source locale/timezone separately only when needed.
- Keep source review, consent, and annotation history auditable.
- A record with unresolved consent/license/provenance flags cannot enter the training export.
- Use the JSON Schema at `schemas/dataset-records-v1.schema.json` as the export contract. The relational model and JSON export are related but not identical.

## Logical tables

### 1. `documents`

One row per collected text/document.

| Column | Type (suggested) | Constraint / purpose |
|---|---|---|
| `document_id` | UUID or stable text ID | Primary key |
| `group_id` | UUID/text | Required grouping key for leakage-safe split |
| `pair_id` | UUID/text nullable | Links controlled human/AI paired responses |
| `raw_text` | TEXT | Immutable source text |
| `raw_text_sha256` | CHAR(64) | Hash of exact raw text |
| `normalized_match_hash` | CHAR(64) nullable | Duplicate matching only |
| `language` | VARCHAR | `id` for this project |
| `genre` | VARCHAR | Controlled vocabulary from JSON Schema |
| `document_label` | VARCHAR | HUMAN / AI / MIXED / UNCERTAIN |
| `source_type` | VARCHAR | controlled_human / controlled_ai / controlled_mixed / public_dataset / other |
| `source_review_id` | FK nullable | Source/license/consent decision |
| `provenance_status` | VARCHAR | COMPLETE / PARTIAL / UNKNOWN |
| `writer_id_pseudo` | VARCHAR nullable | Pseudonymous writer ID; never real name |
| `topic_id` | FK nullable | Topic bank reference |
| `prompt_id` | FK nullable | Prompt registry reference |
| `generator_run_id` | FK nullable | AI generation record |
| `split` | VARCHAR | train / validation / test_in_distribution / test_challenge / unassigned |
| `review_status` | VARCHAR | APPROVED / PENDING_REVIEW / REJECTED / NEEDS_ADJUDICATION |
| `created_at_utc` | TIMESTAMP | Ingestion timestamp |
| `schema_version` | VARCHAR | Export schema version |

**Indexes:** `raw_text_sha256`, `normalized_match_hash`, `group_id`, `pair_id`, `split`, `review_status`.

### 2. `sentence_annotations`

One row per sentence per annotation version/annotator.

| Column | Type (suggested) | Constraint / purpose |
|---|---|---|
| `sentence_id` | UUID/text | Primary key |
| `document_id` | FK | Parent document |
| `sentence_index` | INTEGER | Zero-based order |
| `start_char`, `end_char` | INTEGER | Zero-based half-open offsets into exact raw text |
| `sentence_text` | TEXT | Must match the slice of raw text at these offsets |
| `annotator_id_pseudo` | VARCHAR | Pseudonymous annotator ID |
| `guideline_version` | VARCHAR | Annotation rules version |
| `label` | VARCHAR | HUMAN / AI / MIXED / UNCERTAIN |
| `confidence` | VARCHAR | HIGH / MEDIUM / LOW / NOT_ASSESSED |
| `evidence_refs` | JSON or child table | References to provenance evidence |
| `rationale` | TEXT nullable | Required for uncertain/disputed cases |
| `annotation_status` | VARCHAR | UNANNOTATED / ANNOTATED / DOUBLE_ANNOTATED / ADJUDICATED |
| `annotated_at_utc` | TIMESTAMP | Audit timestamp |

**Constraints:** `start_char >= 0`, `end_char > start_char`; unique `(document_id, sentence_index, annotator_id_pseudo, guideline_version)`.

### 3. `provenance_spans`

Optional but recommended for span-level origin evidence.

| Column | Type | Purpose |
|---|---|---|
| `span_id` | UUID/text PK | Stable span ID |
| `document_id` | FK | Parent document |
| `start_char`, `end_char` | INTEGER | Half-open offsets |
| `origin_label` | VARCHAR | HUMAN / AI / UNKNOWN |
| `contribution_type` | VARCHAR | SUBSTANTIVE / MECHANICAL / UNKNOWN |
| `evidence_ref` | FK/text | Provenance/version evidence |
| `annotator_id_pseudo` | VARCHAR | Who annotated the span |
| `guideline_version` | VARCHAR | Rules version |

Spans may overlap only when the annotation tool explicitly supports layered annotations. Otherwise require non-overlapping spans or document the overlap policy.

### 4. `editing_versions`

One immutable row per version in a text's version graph.

| Column | Type | Purpose |
|---|---|---|
| `version_id` | UUID/text PK | Version ID |
| `document_id` | FK | Logical document |
| `parent_version_id` | FK nullable | Previous version; forms lineage |
| `actor_type` | VARCHAR | HUMAN / AI / UNKNOWN |
| `action` | VARCHAR | GENERATE / EDIT / COPYEDIT / REWRITE / ACCEPT / REJECT |
| `description` | TEXT nullable | What changed |
| `reason` | TEXT nullable | Why, if known |
| `text_sha256` | CHAR(64) | Exact version hash |
| `tool_name`, `provider`, `model_id_exact` | VARCHAR nullable | Tool/generator trace |
| `prompt_id`, `response_id` | VARCHAR nullable | Generation linkage |
| `created_at_utc` | TIMESTAMP | Event time |
| `diff_reference` | TEXT nullable | Restricted diff/artifact reference |

### 5. `source_reviews`

One row per dataset/source/consent review.

| Column | Type | Purpose |
|---|---|---|
| `source_review_id` | UUID/text PK | Review ID |
| `source_name` | TEXT | Dataset, publication, participant submission stream, or provider |
| `source_version` | TEXT nullable | Exact version/revision |
| `source_url` | TEXT nullable | Canonical source |
| `license_name`, `license_url` | TEXT nullable | Explicit license evidence |
| `terms_review_status` | VARCHAR | APPROVED / CONDITIONAL / PENDING_REVIEW / REJECTED |
| `provenance_review_status` | VARCHAR | APPROVED / CONDITIONAL / PENDING_REVIEW / REJECTED |
| `privacy_review_status` | VARCHAR | APPROVED / CONDITIONAL / PENDING_REVIEW / REJECTED |
| `decision_rationale` | TEXT | Why source passed/failed |
| `reviewer_id_pseudo` | VARCHAR | Reviewer |
| `reviewed_at_utc` | TIMESTAMP | Review date |
| `evidence_refs` | JSON/text | License snapshot, card, terms, checksum |

### 6. `generator_runs`

One row per API/local generation attempt, including failures.

| Column | Type | Purpose |
|---|---|---|
| `generator_run_id` | UUID/text PK | Stable run ID |
| `provider` | VARCHAR | Provider/family |
| `model_id_exact` | VARCHAR | Exact ID returned/used |
| `model_version_or_snapshot` | VARCHAR nullable | Snapshot/version if exposed |
| `prompt_id` | FK | Prompt registry |
| `prompt_sha256` | CHAR(64) | Exact rendered prompt hash |
| `parameters_json` | JSON | Explicit parameters; note unsupported/unavailable values separately |
| `response_id` | VARCHAR nullable | Provider response ID |
| `output_document_id` | FK nullable | Linked document if successful |
| `status` | VARCHAR | SUCCESS / REFUSAL / TRUNCATED / API_ERROR / REVIEW_REQUIRED |
| `retry_number` | INTEGER | Attempt number |
| `generated_at_utc` | TIMESTAMP | Time |
| `terms_review_id` | FK | Provider terms/data-handling review |

### 7. `prompt_registry`

One row per immutable prompt version.

Required fields: `prompt_id` PK, `prompt_version`, `topic_id`, `genre`, `task_condition`, `template_text`, `language`, `target_min_words`, `target_max_words`, `created_at_utc`, `approved_by_pseudo`, `notes`.

### 8. `annotation_events`

Append-only audit events for label changes, adjudication, exclusions, and corrections.

Required fields: `event_id` PK, `document_id` FK, `sentence_id` FK nullable, `actor_id_pseudo`, `event_type`, `previous_value_json`, `new_value_json`, `reason`, `guideline_version`, `created_at_utc`.

### 9. Restricted participant/consent mapping

Keep names/contact details, if collected at all, in a separate restricted store. Dataset tables should contain only `writer_id_pseudo` and `consent_id_pseudo`. Consent records must track:
- participant code;
- age eligibility confirmation (do not store full birth date unless necessary);
- consent version and timestamp;
- internal research/training consent;
- separate public-release consent;
- withdrawal status and effective date;
- retention/deletion status.

Do not place real names, email addresses, phone numbers, signatures, or identity documents in the dataset export.

## JSON export relationship

The JSON Schema describes the portable export record. Relational tables can normalize many-to-one/many-to-many relationships, while the export nests source review, provenance, editing history, and sentence annotations for convenience. The export process must preserve IDs, exact raw text, offsets, group relationships, source review status, and audit trail references.

## Integrity checks before export

1. Every `document_id` is unique.
2. Every sentence references an existing document.
3. Every sentence's `sentence_text` equals the raw-text slice using the agreed Unicode code-point offset convention.
4. Every training-eligible document has approved source/consent review and adequate provenance.
5. Every document has a `group_id`; related versions/pairs share a group.
6. No duplicate or derivative group crosses splits.
7. A held-out writer/generator does not appear in train/validation.
8. Every model-generated record references a generator run and prompt version.
9. Every annotation change is auditable.
10. The export validates against the current JSON Schema.

**Implementation note:** SQL dialect, physical database engine, migrations, and access-control configuration are not selected yet. This document is a logical schema only.
