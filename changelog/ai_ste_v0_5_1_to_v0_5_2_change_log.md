# AI-STE Changelog

All notable changes to the AI-STE standard are documented in this file.

Format: each release has a **Summary**, a **Changed** section (modifications to existing content), an **Added** section (new content), a **Fixed** section (corrections to errors), and a **Removed** section (content that was deleted). Changes within each section are grouped by area: Document Overview, Part 1 (rules by section number), Part 2 (architecture), Part 3 (dictionary), Part 4 (conformance), and Appendices.

The version number scheme follows Semantic Versioning. A change that breaks references to rule numbers or template field order increments the minor version. A change that only corrects errors or adds content without restructuring increments the patch version. A change that restructures the rule set or numbering increments the major version.

For the full per-change rationale, see `ai_ste_v4_to_v5_change_log.md` (v4.0 → v0.5.1) and `ai_ste_v3_to_v4_change_log.md` (v3.0 → v4.0).

---

## [0.5.2] — 2026-10-10

**Summary:** Presentation change only. No rule was added, changed, or removed. Rule tags (`[ASD]`, `[ASD+]`, `[DEV]`, `[AI]`) were removed from every rule heading and from rule body text. A single reference table that lists all 67 rules with their tags was added after the existing tag legend in the Document Overview. This makes the tags easy to scan in one place and removes repetitive annotation from the rule text.

No linter results changed. The standard passes at 0 errors and 3 warnings (unchanged from v0.5.1).

### Changed

**Document Overview**
- Sentence "Deliberate differences are tagged `[DEV]` or `[ASD+]`" updated to "Deliberate differences are listed with tag `[DEV]` or `[ASD+]` in the Rule Tag Reference table" to direct the reader to the new table.

**Part 1: Writing Rules — all 67 rule headings**
- Removed the inline tag suffix `` `[TAG]` `` from each of the 67 rule headings (Rules 1.1–1.15, 2.1–2.2, 3.1–3.7, 4.1–4.5, 5.1–5.6, 6.1–6.8, 7.1–7.6, 8.1–8.10, 9.1–9.8).

**Part 1: Rule 5.4 body text**
- Removed inline tag from the label `*Additional AI-STE requirements \`[AI]\`:*`, which is now `*Additional AI-STE requirements:*`.

**Part 1: Rule 8.3 body text**
- Removed the mid-sentence inline tag `` `[AI]` `` that preceded "A text in parentheses must not contain an instruction or a `GUARDRAIL`." The sentence now reads without interruption.

**Part 1: Rule 8.6 body text**
- Removed the leading inline tag `` `[AI]` `` from the paragraph "Count each of these also as one word: a variable…". The paragraph now begins with the word "Count".

**Part 4: Section 15.3 heading**
- Removed `` `[AI]` `` from the heading "### 15.3 Validation of the Heuristics".

### Added

**Document Overview — Rule Tag Reference table**
- Added a new section "## Rule Tag Reference" immediately after the existing Rule Tags legend.
- The table has 67 data rows, one for each rule, grouped into nine sections by bold section header rows (Section 1 to Section 9).
- Columns: Rule number, Title, Tag.
- The table is the single authoritative location of all rule tags from this version forward.

---

## [0.5.1] — 2026-10-10

**Summary:** Corrected three errors found by the automated self-hosting lint test (`ai_ste_lint.py lint --fences` run against the standard's own fenced examples). All three errors were violations of the declare-before-use rule (Rule 13 / Rule 1.5): a declared term was used before it was declared, or was used without being declared at all. No rule text was changed.

The standard passes at 0 errors and 3 warnings after this release. The 3 warnings are expected: they arise from SKILL and PLAN examples that reference terms declared in the AGENT block, which the linter cannot resolve across separate fenced blocks. In a real project those terms would be in a shared `DEFINITIONS` file.

### Fixed

**Part 2: Section 10 — Agent Architecture, template field order**
- `DEFINITIONS` was listed after `ROLE`, `GOAL`, and `CONTEXT` in the AGENT template.
- A declared term used in `ROLE`, `GOAL`, or `CONTEXT` was therefore used before it was declared, which violates Rule 13 (declare before use).
- Fixed: `DEFINITIONS` is now the first optional field in the template, before `ROLE`.
- The prose description was updated to state: "`DEFINITIONS` comes first, so that each declared term is declared before its first use (Section 13)."

**Part 2: Section 10 — Agent Architecture, example field order**
- The `Invoice_Checker` AGENT example had `ROLE`, `GOAL`, `CONTEXT`, and then `DEFINITIONS`.
- `ROLE: Invoice_Validator` and `GOAL: VALIDATE each Stripe_Invoice…` both referenced `Invoice_Validator` and `Stripe_Invoice` before those terms were declared.
- Fixed: `DEFINITIONS` was moved to the first position in the example, before `ROLE`.

**Part 3: Section 13 — Technical Terms Protocol, Rule 9.1 example**
- The Rule 9.1 example declared `TECHNICAL_VERB: PROVISION = To add a User_Profile to the TOOL.` but `User_Profile` appeared in the definition without being declared.
- Fixed: a `TECHNICAL_NOUN: User_Profile = The data that tell the TOOL about one USER.` declaration was added before the `PROVISION` declaration.

**Part 3: Section 14.6 — Standard Technical Nouns**
- The Section 13 example used `HTTP` (in the definition of `POST`) but `HTTP` was not listed in the standard technical nouns table (14.6).
- Fixed: `HTTP` was added to table 14.6 with the definition "A set of rules for the exchange of DATA between programs on a network."

---

## Prior versions

| Version label used in files | Renamed to | Change log file |
| :--- | :--- | :--- |
| v5.0 | v0.5.0 | See `ai_ste_v4_to_v5_change_log.md` |
| v4.0 | v0.4.0 | See `ai_ste_v4_to_v5_change_log.md` (starting context) and `ai_ste_v3_to_v4_change_log.md` |
| v3.0 | v0.3.0 | See `ai_ste_v3_to_v4_change_log.md` |

v0.5.0 was the first version verified against the primary ASD-STE100 Issue 9 PDF. It introduced the full 53-rule alignment, vocabulary source codes, Appendix A (rule-by-rule mapping), and Appendix B (vocabulary conflicts). It also replaced HALT with STOP, REPORT (as verb) with TELL, and THEN/ELSE with paired IF sentences. See `ai_ste_v4_to_v5_change_log.md` for the complete record.
