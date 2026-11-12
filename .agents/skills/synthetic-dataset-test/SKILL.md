---
name: synthetic-dataset-test
description: Plans and executes synthetic dataset procedure tests for Lexora, including JSON Schema, sentence offsets, editing lineage, deduplication, and leakage-safe group splits.
---

# Synthetic Dataset Procedure Test

## Purpose

Prove that Lexora's dataset pipeline preserves structural validity, text offsets, provenance lineage, deduplication integrity, and leakage-safe group boundaries before collecting real pilot data.

## Preconditions

- Read the active dataset schema, annotation rules, editing-history template, and roadmap.
- If required specifications are missing from the repository, stop and report the missing source. Do not reconstruct requirements from memory without marking them as provisional and obtaining approval.
- Use synthetic fixtures only. Do not use real participant text or public corpora in this phase.

## Test design

For each component, include:
- Valid case.
- Invalid case.
- Boundary case.
- Adversarial or ambiguous case.
- Expected result and explicit failure reason.

### 1. JSON Schema

- Validate the schema itself with a standards-compliant JSON Schema validator.
- Validate valid fixtures and invalid fixtures.
- Check required fields, types, enums, nullability, nested structures, and additional-property policy.
- Record validator name/version and command.
- A successful parse is not the same as formal JSON Schema validation.

### 2. Sentence offsets

- Define the offset convention explicitly (for example, zero-based, end-exclusive Unicode code-point indices).
- Check that each span selects the exact sentence substring from the original document.
- Cover punctuation, abbreviations, quotation marks, emoji, combining characters, Indonesian diacritics, CRLF/LF, tabs, repeated whitespace, and empty text.
- Reject out-of-range, overlapping, or mismatched offsets unless the specification explicitly allows them.

### 3. Editing history and lineage

- Model immutable versions with stable IDs and explicit parent/version links.
- Test missing parent, cyclic lineage, duplicate version ID, out-of-order timestamps, and content hash mismatch.
- Verify that every transformation event can be traced to its input and output version.
- Do not infer provenance from textual similarity alone.

### 4. Deduplication

- Test exact duplicates and normalized exact duplicates separately.
- Test near-duplicates, paraphrases, shared boilerplate, and same-topic independently written texts.
- Avoid merging solely because two texts share topic or common phrases.
- Preserve source references and deduplication decisions for audit.

### 5. Group-based split

- Build groups before assigning split labels.
- Keep pairs, derivatives, paraphrases, and editing versions in the same group.
- Check that no group_id or related lineage crosses train/validation/test.
- Keep held-out writers/generators out of training where the evaluation design requires it.
- Select thresholds on validation only; never tune against test results.

## Reporting

For every run, record:
- Test ID and date.
- Code/schema version.
- Fixture set and expected outcome.
- Exact command and environment.
- Passed, failed, skipped counts.
- Failure details and severity.
- Decision: pass, revise, or blocked.

Do not mark a component Verified until the tests have actually run and the evidence is committed. A test plan or fixture file alone is not a passing test.
