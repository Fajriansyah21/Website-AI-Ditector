# Development Workflow: Google Stitch + Google Antigravity

**Status:** workflow decision recorded; not yet validated through an implemented feature.  
**Date recorded:** 2026-11-09

## 1. Tool responsibilities

### Google Stitch — UI/UX and design exploration

Use Stitch for:
- exploring information architecture and screen layouts;
- generating and iterating high-fidelity UI concepts;
- defining visual tokens, typography, spacing, colors, and component states;
- documenting the intended responsive behavior;
- exporting or handing off the selected design and design rules.

Stitch output is a design artifact, not proof that the website's interactions, security, accessibility, or responsive behavior work correctly.

Official references:
- Stitch introduction and UI-to-code workflow: https://developers.googleblog.com/en/stitch-a-new-way-to-design-uis/
- Stitch design system and export workflow: https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-ai-ui-design/
- DESIGN.md format: https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/

### Google Antigravity — implementation and verification

Use Antigravity for:
- inspecting the repository and existing architecture before changing code;
- turning an approved design into maintainable frontend components;
- implementing routes, state, forms, validation, loading/error/empty states, and responsive behavior;
- integrating APIs and backend services only after their contracts are defined;
- running the project, tests, linters, and browser checks;
- reviewing diffs and reporting what was actually verified.

Antigravity agents can operate across editor, terminal, and browser. Agent output must still be reviewed; an agent's statement that a task is complete is not itself a test result.

Official references:
- Antigravity IDE overview: https://antigravity.google/docs/ide/overview
- Antigravity agent capabilities: https://antigravity.google/docs/agent/
- Google Developers introduction: https://developers.googleblog.com/en/build-with-google-antigravity-our-new-agentic-development-platform/

## 2. Standard feature workflow

1. **Specify:** define user problem, user flow, acceptance criteria, data needs, failure states, and privacy/security implications.
2. **Design in Stitch:** produce screen concepts, component states, responsive behavior, and a concise design-system handoff. Export the selected result rather than treating every generated variation as approved.
3. **Review design:** check visual hierarchy, readability, keyboard and screen-reader needs, contrast, mobile layouts, and consistency with existing screens.
4. **Implement in Antigravity:** inspect existing files first; implement in small, reviewable changes. Do not replace architecture or add dependencies without a reason.
5. **Verify:** run the app and relevant automated checks. Test normal, loading, empty, invalid, error, and narrow-screen states. For data-sensitive features, also test authorization, retention, and failure paths.
6. **Review diff:** inspect changed files, secrets, dependency changes, console errors, and accidental unrelated edits.
7. **Record:** update documentation only for decisions or observed results. Mark each item as designed, implemented, or verified; never describe an untested feature as working.
8. **Commit to GitHub:** use a clear commit message. Keep implementation changes and unrelated documentation edits separate when practical.

## 3. Definition of done for a UI feature

A feature is not complete just because the screenshot resembles the Stitch design. At minimum:
- agreed acceptance criteria are met;
- layouts are checked at mobile and desktop widths;
- interactive controls have working behavior and visible feedback;
- loading, empty, validation, and error states exist where relevant;
- keyboard navigation and accessible names are checked;
- no obvious console/runtime errors remain;
- relevant tests/checks are run and their actual results recorded;
- design deviations are documented with a reason;
- the Git diff is reviewed.

The exact test suite and browser/device matrix will be selected after the app stack is decided.

## 4. TextLab design handoff checklist

For each screen, hand off:
- screen name and intended user task;
- Stitch project/share/export reference;
- viewport(s) and responsive rules;
- design tokens or DESIGN.md, if available;
- components and variants;
- content hierarchy and copy;
- interaction states: default, hover/focus, disabled, loading, success, empty, validation error, server error;
- navigation and route behavior;
- accessibility notes;
- unresolved questions and intentional deviations.

## 5. TextLab screen sequence (proposed)

This is a planning sequence, not a claim that screens exist:
1. Detector landing/input screen.
2. Detector results with sentence highlights and document summary.
3. Humanizer input, style selection, and before/after comparison.
4. DOCX Metadata Cleaner upload, review, and download flow.
5. Account/profile screens for personal writing profiles.
6. Privacy, consent, quota, and access/advertising states if the monetization design is approved.

Do not finalize advertising UI until policy and technical eligibility are verified.

## 6. Source-of-truth rules

- GitHub repository: decisions, technical specifications, test evidence, and implementation history.
- Stitch: source design workspace and exported design artifacts.
- Antigravity workspace: application source code and local development/test workflow.
- If a design changes after implementation begins, record the change and review its impact rather than silently allowing design and code to drift.
- Do not store API keys, access tokens, private user text, consent identity records, or other secrets in GitHub.
- Generated code must be reviewed; preserve the ability to explain and maintain it.

## 7. Current status

- Tool choice: recorded.
- Stitch project link and exported design artifacts: not yet supplied/recorded in this repository.
- Frontend framework and backend stack: not yet finalized in this workflow document.
- Application implementation: not yet verified.
- Automated UI tests: not yet configured or run.

This workflow should be revised after the first real screen is designed, implemented, and tested.
