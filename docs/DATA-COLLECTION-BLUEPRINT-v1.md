# Data Collection Blueprint v1.0

**Project:** TextLab  
**Status:** PROPOSED operational plan; public-source candidates are not approved for training until individually audited.  
**Version date:** 2026-11-09  
**Scope:** Indonesian-language AI-text provenance dataset; document and sentence level.

## 1. Purpose and exit criteria

This blueprint turns Dataset Specification & Benchmark Protocol v1.0 into repeatable collection procedures. It does not authorize full-scale collection or final model training by itself.

The pilot (200–500 documents) must demonstrate that:
1. Every record has traceable provenance and a documented usage basis.
2. Human contributors gave informed consent for the specified use.
3. AI generations can be reproduced or audited from prompts and generation metadata.
4. Human edits are recorded as a version history, not guessed from the final text.
5. Annotators can apply the label guide consistently and mark insufficient evidence UNCERTAIN.
6. Exact/near duplicates and linked derivatives are grouped before splitting.
7. Author and generator holdouts can be enforced without leakage.
8. Records can be exported and validated against the JSON Schema in `schemas/dataset-records-v1.schema.json`.

**Stop condition:** If provenance, consent, label agreement, source rights, or group-based splitting cannot be demonstrated, pause scaling and revise the process.

## 2. Non-negotiable rules

- Public availability is not proof of reuse permission or human authorship.
- A text is not HUMAN solely because it was found on a website, in a corpus, or in a publication.
- Do not infer labels from another AI detector.
- Do not label by writing style, grammar, formality, or detector score.
- Do not invent editing history after the fact. Unknown history remains unknown.
- Preserve the original text. Normalized text is a separate derived field used for matching only.
- Store author identity separately from text records; use pseudonymous IDs.
- Do not collect sensitive personal information or private communications unless specifically necessary, consented to, and approved.
- Do not publish participant text or raw prompts containing personal data by default.
- Keep test/challenge labels and records access-controlled until the benchmark protocol is locked.
- A license decision applies to a particular version, source, and intended use, not automatically to an entire website or corpus.

## 3. Collection streams

### Stream A — controlled human writing (primary HUMAN source)

Recruit consenting adults who can provide writing that they created themselves. Record how the text was produced, whether AI or another person contributed, and whether the contributor has authority to grant the requested use.

Preferred collection methods:
- Write a new response to a standardized topic/prompt during collection.
- Submit an existing text only when the contributor confirms its origin and has the right to submit it.
- Collect a small number of different genres and lengths from each contributor.
- Avoid requiring participants to disclose private or sensitive experiences.

A human submission with unverified AI assistance is not automatically HUMAN. It may be MIXED or UNCERTAIN depending on the evidence and annotation policy.

### Stream B — controlled AI generation (primary AI source)

Generate text from documented prompts using multiple provider/model families. Save the exact prompt, model identifier, provider, date/time, settings, response identifier when available, and raw response. Do not post-edit the raw AI output before saving it.

For each generation, keep a paired human response to the same topic where practical. Pairing is for controlled comparisons; both records must remain in the same `group_id` for split purposes.

### Stream C — controlled human editing of AI output (primary MIXED source)

Use a saved AI output as a starting version. Ask a consenting editor to revise it naturally, without a requirement to preserve a fixed percentage of the AI text. Save every version and a change log. Annotators must use the actual change history and text evidence, not assume every edit makes the whole document MIXED.

### Stream D — public datasets (supplementary; restricted admission)

Public corpora can help assess genre coverage, language variation, and preprocessing, but are not automatically valid labeled training examples. Admit an item only after source-specific provenance, license/terms, privacy, and derivative/group checks.

## 4. Candidate public sources and current gate status

The statuses below are **screening decisions**, not legal opinions. The source's license and terms must be checked again at the exact version/download date. Do not bulk-download or use for training before the gate is passed.

| Candidate | What it may contribute | Current status | Main limitation / gate |
|---|---|---|---|
| [IndoNLU benchmark](https://github.com/IndoNLP/indonlu) and [Hugging Face dataset card](https://huggingface.co/datasets/indonlp/indonlu) | Indonesian language/genre examples; pipeline smoke tests; possible source-linked analysis | **CONDITIONAL / audit per task** | HF card reports MIT for benchmark datasets, but the benchmark mixes sources and tasks. Original source rights, source attribution, and text-level AI provenance are not established uniformly. Do not assign HUMAN solely from this corpus. |
| [Indo4B access via IndoNLU](https://github.com/IndoNLP/indonlu) | Background language coverage or pretraining research if separately authorized | **PENDING** | Large corpus, source composition and source-level rights need review. It is not a provenance-labeled AI-detection dataset. The project notes exclusions related to Twitter developer policy; do not reintroduce restricted data. |
| [Leipzig Corpora Collection](https://wortschatz.uni-leipzig.de/en/download) | Language/genre coverage and preprocessing experiments | **PENDING** | Mixed news, web, and Wikipedia-derived material. Verify the specific corpus version's license, terms, attribution requirements, and source composition. Not a default HUMAN label source. |
| [OSCAR](https://huggingface.co/datasets/oscar) | Optional robustness/coverage analysis after review | **PENDING / not pilot priority** | Common Crawl-derived web data has heterogeneous sources, duplicates, uncertain authorship/editing histories, and potentially problematic content. Dataset-card license metadata does not automatically clear every underlying work for every intended use. |
| [CC-100](https://data.statmt.org/cc-100/) | Optional coverage research after review | **PENDING / not pilot priority** | Web-crawl origin and source-level rights/provenance require review; no reliable AI-origin labels by default. |
| [Wikipedia dumps](https://dumps.wikimedia.org/) | A source for language/encyclopedic genre tests only if compliant | **CONDITIONAL** | Wikipedia text is generally under CC BY-SA with attribution/share-alike conditions, but article revision history does not prove no AI assistance. Verify applicable terms and keep derivative/version groups together. |
| [dataset_indonesia on Hugging Face](https://huggingface.co/datasets/sansaks/dataset_indonesia) | Not admitted in pilot | **BLOCKED pending license clarification** | Dataset card declares license `other`; upstream provenance and component licenses must be resolved before use. |
| Public news collections listed in [Indonesian NLP resources](https://github.com/kmkurn/id-nlp-resource) | Possible article genre coverage | **PENDING / do not ingest yet** | Access, copyright, redistribution, scraping terms, and source-level provenance must be verified. A citation or research download is not permission to republish or train. |

### Public-source audit checklist

For every candidate and version, record:
- exact dataset name, version/revision, download date, URL, file checksum;
- publisher/maintainer and underlying source(s);
- explicit license text/URL and any additional terms;
- whether commercial use, modification, redistribution, and model training are allowed or unclear;
- attribution/share-alike/notice obligations;
- personal-data and sensitive-content risks;
- provenance strength for each proposed label;
- deduplication/grouping method;
- decision: `APPROVED`, `CONDITIONAL`, `PENDING_REVIEW`, or `REJECTED`;
- reviewer, date, rationale, and re-review trigger.

When terms are ambiguous, do not silently interpret ambiguity as permission. Exclude the source from training until resolved.

## 5. AI generator plan

### Proposed provider families

Use at least three independent provider families if budget and access permit. Candidate families for the pilot:

| Family | Official model reference | Proposed role | Status |
|---|---|---|---|
| OpenAI | [API model catalog](https://developers.openai.com/api/docs/models) | Mainstream general-purpose generation | Candidate; choose exact API model ID at collection time |
| Google Gemini | [Gemini API models](https://ai.google.dev/gemini-api/docs/models) | Independent provider family | Candidate; choose a stable model ID where available |
| DeepSeek | [API model list](https://api-docs.deepseek.com/api/list-models/) | Independent provider family | Candidate; verify the exact returned model ID and alias behavior |
| Open-weight model served locally or by a documented host | [Hugging Face model hub](https://huggingface.co/models) | Optional local/open-weight comparison | Candidate; license and runtime need per-model review |

This is a provider shortlist, not a frozen model roster. Before collection, record exact model IDs and provider terms. Never record only a mutable alias such as `latest`. If a provider exposes only a moving alias, save the response metadata, date, and all available version identifiers and mark reproducibility as limited.

### Generator selection rules

- Select 3 provider families for the first pilot; do not generate with every available model merely to increase volume.
- Use at least one generator family only in the challenge split once the training roster is chosen.
- Rotate generator holdout across later benchmark versions; a single held-out model is not evidence of broad generalization.
- Do not use the same generated answer as both a training item and a test/challenge item, including paraphrases or edits.
- Log safety refusals, truncations, API errors, and retries; do not silently replace a failed sample.
- Record provider terms and data-handling settings before sending prompts. Do not send participant-identifying information to a generation API.
- Keep temperature, top-p, system instructions, output-length limits, locale/language instruction, and all other available generation parameters fixed within each planned condition. If a parameter is unsupported, record it as unavailable rather than assuming a default.
- A model update can change text style. Treat materially changed model versions as a new generator group.

### Required generation record

`provider`, `model_id_exact`, `model_version_or_snapshot`, `api_endpoint`, `generation_timestamp_utc`, `prompt_id`, `prompt_text`, `system_instruction`, `parameters`, `response_id`, `raw_output_sha256`, `retry_number`, `status`, and `terms_review_id`.

## 6. Topic and prompt design

Use a topic bank with balanced genres and a common task structure. Each topic receives a stable `topic_id`. Each prompt version receives a stable `prompt_id`. Avoid prompts that require personal disclosure or reproduce copyrighted passages.

Prompt conditions:
1. **Neutral response:** explain a topic to a general Indonesian reader.
2. **Argumentative:** present a position, two reasons, and a counterargument.
3. **Instructional:** explain a safe, ordinary process in ordered steps.
4. **Narrative:** write a fictional or low-stakes everyday scene; no personal claim is requested.
5. **Casual:** write a short message or forum-style explanation.
6. **Academic/educational:** explain a concept for a student, with no fabricated citations or sources.

Human and AI participants should receive the same task intent and topic wherever feasible. Do not give the human one topic and the AI an unrelated one, then let topic differences become a shortcut for the detector.

Prompt wording must not demand that a model imitate a named living writer or a real participant. Do not instruct a model to deliberately add mistakes to appear human.

## 7. Human writing collection form

Use `templates/human-writing-consent-form.md` as the starting form. The form must explain:
- project purpose and that the text may be used to develop/evaluate AI-text-origin detection;
- what text and process metadata are collected;
- whether the text may be stored, edited, annotated, and used in model training;
- that public release of the raw text is a separate optional permission;
- retention, withdrawal limits, contact route, and whether compensation exists;
- that the contributor should not submit confidential, third-party, or sensitive personal information.

Consent to contribute to research/training is not the same as consent to publish the text publicly. Keep those permissions separate.

## 8. Editing history

Use `templates/editing-history.template.json`. Preserve each version as an immutable record with:
- `version_id`, `parent_version_id`, timestamp;
- actor type (`HUMAN`, `AI`, `UNKNOWN`);
- action (`GENERATE`, `EDIT`, `COPYEDIT`, `REWRITE`, `ACCEPT`, `REJECT`);
- description of changes and reason, if known;
- tool/provider/model metadata if AI was used;
- consent/provenance reference;
- diff or changed-span reference when available.

Never claim an edit is “mechanical” or “substantive” without a reasonable description of what changed. The annotator decides the label under the guideline, not the collection operator.

## 9. Annotation workflow

1. Ingest raw source and preserve it read-only.
2. Check consent/source rights and provenance completeness.
3. Assign `document_id`, `group_id`, and `pair_id` if applicable.
4. Run exact and near-duplicate detection before split.
5. Build a version graph for all edits, paraphrases, and source derivatives.
6. Annotate provenance spans from documented history.
7. Derive sentence labels using `docs/ANNOTATION-GUIDELINES-v1.md`.
8. Double-annotate at least 20% of pilot documents, with at least two annotators on that subset.
9. Adjudicate all disagreements in the audit subset and record reasons.
10. Validate JSON schema and metadata constraints.
11. Split by groups only after grouping and duplicate review.
12. Audit split boundaries, writer overlap, generator overlap, topic/prompt leakage, and source overlap.
13. Freeze the pilot manifest and record a version/hash before any benchmark run.

For a 200-document pilot, 40 double-annotated documents is a reasonable initial operational sample. If there are too few items in a label/genre cell to assess agreement meaningfully, report the limitation rather than claiming reliable agreement.

## 10. Dedupe and group rules

### Exact duplicate
- Calculate SHA-256 of the raw text.
- Also calculate a normalized comparison hash after Unicode normalization, whitespace normalization, and conservative case handling.
- Do not replace raw text with normalized text.

### Near duplicate / derivative
Use multiple signals, not one similarity score alone:
- character or token n-gram similarity;
- MinHash/LSH or another scalable candidate search;
- embedding similarity as a candidate finder only;
- source URL, document ID, revision history, pair ID, and edit ancestry;
- manual review for borderline pairs.

Do not automatically delete two texts merely because they share a topic or common phrase. Link confirmed derivatives into the same `group_id`; document the decision. If uncertain whether two texts are derivatives, use a conservative group or keep them out of the strict benchmark until reviewed.

### Grouping constraints
All of the following stay in one split:
- a document and its edited versions;
- AI original and human-edited descendants;
- paraphrases or translations derived from the same source;
- human/AI paired responses tied to one controlled experiment;
- records sharing a source document or revision lineage;
- repeated outputs from the same prompt-run cluster when they could leak template content.

Use separate fields for `writer_id_pseudo`, `generator_family`, `prompt_id`, and `group_id`; one field must not stand in for all grouping relationships.

## 11. Split design

Keep the previously agreed overall target of 70/15/15, group-based. For the pilot, do not force exact percentages if doing so breaks group integrity or makes the held-out sets meaningless.

Suggested pilot split:
- 70% training groups;
- 15% validation groups;
- 10% in-distribution test groups;
- 5% challenge groups.

This is a pilot allocation only. The existing v1.0 benchmark examples for 5,000–10,000 documents remain the scale-up planning targets. Record actual counts by document, sentence, group, class, genre, writer, and generator.

Challenge set policy:
- reserve at least one generator family not used in training/validation;
- hold out writers who have no records in training/validation;
- include controlled examples from relevant genres;
- keep labels inaccessible to model selection until evaluation;
- do not tune thresholds or prompts on challenge results.

If the pilot is too small to support all holdouts, prioritize a valid group split and treat challenge results as a process test, not a performance claim.

## 12. Data schema and JSON

The authoritative machine-readable contract is `schemas/dataset-records-v1.schema.json`. The initial design uses a document record containing provenance and group metadata plus sentence records containing exact offsets and labels. Editing history is represented as a version graph. All IDs are stable and pseudonymous.

Schema rules:
- offsets are zero-based, half-open `[start_char, end_char)`, measured in Unicode code points against the exact stored `text` field;
- `sentence.text` must equal the corresponding slice of the original document under the declared offset convention;
- retain both raw text and a separate normalized comparison representation;
- missing values are explicit `null` or documented enums, never silently inferred;
- every training-eligible document has `review_status=APPROVED`;
- `UNCERTAIN` records remain available for auditing but are excluded from the three-class training target unless a later experiment explicitly defines an uncertainty task.

## 13. Pilot quality gates

A pilot passes only if:
- 100% of training candidates have source/provenance fields completed;
- 100% have a documented source/consent usage decision;
- 0 known exact duplicates cross splits;
- 0 known version/derivative groups cross splits;
- 0 writer overlap between train/validation and the strict writer-holdout set;
- 0 generator-family overlap for the selected held-out generator;
- at least 20% of pilot documents are double-annotated;
- disagreement and adjudication logs exist;
- agreement metrics are reported with class counts and caveats;
- all JSON records validate;
- all sentence offsets pass automated slice checks;
- every exclusion and label change has a reason;
- no unresolved license or consent issue remains in the training subset.

A pass does not prove the detector is accurate. It proves the collection and labeling process is sufficiently controlled to justify the next experiment.

## 14. Repository artifacts

- `docs/DATA-COLLECTION-BLUEPRINT-v1.md` — this operational plan.
- `docs/ANNOTATION-GUIDELINES-v1.md` — label policy and examples.
- `docs/PROMPT-BANK-v1.md` — topic and prompt bank.
- `templates/human-writing-consent-form.md` — participant-facing draft.
- `templates/editing-history.template.json` — editing/version metadata template.
- `schemas/dataset-records-v1.schema.json` — machine-readable JSON Schema.
- `docs/DECISION-LOG.md` — decision history; changes are appended.

## 15. Source references reviewed for this draft

- IndoNLU repository and data description: https://github.com/IndoNLP/indonlu
- IndoNLU Hugging Face dataset card/license field: https://huggingface.co/datasets/indonlp/indonlu
- Indonesian NLP resource list (corpus discovery, not license clearance): https://github.com/kmkurn/id-nlp-resource
- Leipzig Corpora Collection: https://wortschatz.uni-leipzig.de/en/download
- OSCAR dataset card: https://huggingface.co/datasets/oscar
- CC-100 source page: https://data.statmt.org/cc-100/
- Wikipedia dumps: https://dumps.wikimedia.org/
- Candidate Indonesian corpus with `other` license field: https://huggingface.co/datasets/sansaks/dataset_indonesia
- OpenAI model catalog: https://developers.openai.com/api/docs/models
- Gemini model catalog: https://ai.google.dev/gemini-api/docs/models
- DeepSeek API model list: https://api-docs.deepseek.com/api/list-models/

**Important:** these references are discovery sources. They do not replace the per-version license, provenance, and terms audit described above.
