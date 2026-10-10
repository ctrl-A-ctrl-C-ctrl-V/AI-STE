# AI-STE Changelog — v0.5.0 and v0.5.1

This file documents every change between v0.4.0 and v0.5.0, and between v0.5.0 and v0.5.1. Changes are derived from the file diff and verified against the v4.0 → v5.0 change log (`ai_ste_v4_to_v5_change_log.md`), which gives the full rationale for each decision.

---

## [0.5.1] — 2026-10-10

**Summary:** Patch release. Three errors found by the automated self-hosting lint test were corrected. No rule text was added, changed, or removed.

### Fixed

**Part 2 — Section 10: Agent Architecture**

*Template field order*
- `DEFINITIONS` was the fifth field in the AGENT template, after `ROLE`, `GOAL`, and `CONTEXT`.
- A declared term used in `ROLE`, `GOAL`, or `CONTEXT` was therefore used before it was declared, which violates Section 13 (declare before first use).
- Fix: `DEFINITIONS` is now the first field in the template, before `ROLE`.
- The prose description was updated to state: "`DEFINITIONS` comes first, so that each declared term is declared before its first use (Section 13)."

*Example field order*
- The `Invoice_Checker` AGENT example used the same wrong field order: `ROLE`, `GOAL`, `CONTEXT`, then `DEFINITIONS`.
- `ROLE: Invoice_Validator` and `GOAL: VALIDATE each Stripe_Invoice…` both referenced `Invoice_Validator` and `Stripe_Invoice` before those terms were declared.
- Fix: `DEFINITIONS` moved to first position in the example.

**Part 3 — Section 13: Technical Terms Protocol**

*Rule 9.1 example: undeclared noun in a definition*
- The Rule 9.1 example declared `TECHNICAL_VERB: PROVISION = To add a User_Profile to the TOOL.` The noun `User_Profile` appeared in the definition text without being declared first.
- Fix: added `TECHNICAL_NOUN: User_Profile = The data that tell the TOOL about one USER.` before the `PROVISION` declaration.

**Part 3 — Section 14.6: Standard Technical Nouns**

*HTTP missing from the dictionary*
- The Section 13 example used `HTTP` in the definition of the declared verb `POST`, but `HTTP` was not listed anywhere in the standard.
- Fix: `HTTP` added to Section 14.6 with the definition "A set of rules for the exchange of DATA between programs on a network."

---

## [0.5.0] — 2026-10-10

**Summary:** Major revision. This is the first version verified rule-by-rule against the primary ASD-STE100 Issue 9 PDF (2025-01-15). Every structural claim about ASD-STE100 that was inferred in v0.4.0 is now checked against the source. The rule numbering was restructured so that rules 1.1 to 9.4 match the numbers and topics of ASD-STE100. New rules fill the gaps. The dictionary was rebuilt with a Source column, a Prohibited Alternatives column (replacing the Unapproved Synonyms column), and the addition of an ASD-conflict marker (†). A new Appendix B documents vocabulary conflicts with ASD-STE100.

Breaking changes: rule numbers changed throughout Part 1. Templates changed field names (`ACTION` → `STEPS`, `REQUIRED` → `NECESSARY`). The conditional syntax changed (`IF/THEN/ELSE` → paired `IF` sentences). Documents written to v0.4.0 must be reviewed before use with v0.5.0.

### Changed

**Document Overview**

- Issue date of ASD-STE100 corrected from "January 2025" to "2025-01-15".
- Origin of ASD-STE100 changed from "written for aerospace maintenance documentation" to "came from a request of the aviation industry in the late 1970s."
- Attribution expanded: ASD-STE100 is a registered European Union trademark of ASD; AI-STE does not copy the ASD-STE100 dictionary or its examples; ASD-STE100 is not for use alone; ASD makes no claim that LLMs follow STE instructions better than they follow plain prose.
- "The limits in this document (for example, 20 and 25 words per sentence, and the pronoun ban)" shortened to "the limits in this document (for example, the pronoun ban)" to avoid overstating which limits are heuristics.
- Scope of rules: commentary now explicitly includes Section 14 reference definitions, which are written for human readers and are not in STE.
- Scope of rules: "Every example marked *Approved* follows the rules" now notes that approved examples may also use technical nouns that ASD-STE100 itself uses (for example, *password*).
- Literal payload reference changed from "Rule 9.3" to "Rule 9.8" (rule was renumbered).
- `[ASD]` tag description changed from "Follows the intent of ASD-STE100" to "Follows ASD-STE100."
- Document Map: Part 3 description changed from "Extensibility protocol and core dictionary" to "Technical terms and the AI-STE core dictionary."
- Document Map: Appendices row changed from "Appendix A | Mapping to ASD-STE100, with deviations" to "Appendices | A, B | Rule mapping and vocabulary conflicts."

*New subsection added: "How AI-STE Relates to ASD-STE100"*
- States that rule numbers 1.1 to 9.4 match ASD-STE100 Issue 9, that AI-specific rules extend the section numbering, and that deliberate differences are tagged `[DEV]` or `[ASD+]`.

**Part 1 — Section 1: Words**

Rules 1.1 to 1.4 of v0.4.0 (Approved Words, One Word One Meaning, Technical Terms, Meaning Outranks Vocabulary) replaced by Rules 1.1 to 1.15 aligned to ASD-STE100:

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 1.1 Approved Words | 1.1 Which Words You Can Use | Table simplified to two columns (Layer / Source). Priority column removed. Clarification added: a Layer 2 word that ASD does not approve is a technical noun or technical verb; the AI-STE meaning overrides ASD where they conflict. |
| 1.2 One Word, One Meaning | 1.11 One Technical Noun for One Item | Renamed and relocated to match ASD Rule 1.11. Example changed from RETRIEVE synonyms to VALIDATE/STORE of Stripe_Invoice. |
| 1.2 example | 1.2 Part of Speech | Rule 1.2 is now the part-of-speech rule. Example changed from FORMAT/format ambiguity to REPORT (noun only in ASD) → TELL. |
| 1.3 Technical Terms | 1.5, 1.12 Technical Nouns / Technical Verbs | Split into two rules matching ASD Rules 1.5 and 1.12. Keywords changed from `DOMAIN_NOUN`/`API_VERB` to `TECHNICAL_NOUN`/`TECHNICAL_VERB`. |
| 1.4 Meaning Outranks Vocabulary | 9.1 Use a Different Sentence Construction | Moved to Section 9, matching ASD Rule 9.1. |

Rules 1.3, 1.4, 1.6, 1.7, 1.8, 1.9, 1.10, 1.13, 1.14 added (adopted from ASD Rules 1.3, 1.4, 1.6–1.14).

Rule 1.15 (Prohibited Alternatives) added as an AI-specific rule. Clarifies that the prohibition applies only to the meaning of the Layer 2 word, not to all uses of the word. `†` marks words that ASD-STE100 approves.

**Part 1 — Section 2: Multi-Word Nouns**

- Section renamed from "Noun Clusters" to "Multi-Word Nouns."
- Rule 2.1 renamed from "Noun Cluster Limit" to "Maximum Three Words." The limit changed from "three nouns" to "three words" (correct reading of ASD Rule 2.1; adjectives count). Unapproved example changed from "(8 nouns)" to "(8 words)". Approved example changed from "The size limit of the MEMORY buffer for one user session" to "The MEMORY limit for one USER" (removed `size`, which ASD does not approve).
- Rule 2.2 (Long Technical Nouns) added, adopted from ASD Rule 2.2.

**Part 1 — Section 3: Verbs**

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 3.1 Imperative Mood | Moved to 5.3 Imperative Form | Relocated to match ASD placement in Section 5. |
| 3.2 Permitted Verb Forms | 3.2 Permitted Tenses | Minor wording change; "progressive or perfect forms" → "progressive or perfect tenses." |
| 3.3 Active Voice | 3.6 Active Voice | Renumbered. Rule split: "In an instruction, the implied subject is the AGENT" removed (it contradicted the imperative mood rule). Passive voice permitted in descriptive text when the agent is unknown. Example: "agent unknown" changed from "actor" (unapproved) to "agent." |
| 3.4 No "-ing" Verb Forms | 3.5 "-ing" Forms | Renumbered. Rule narrowed: applies to the *-ing* form of a verb only (not to all *-ing* words). STRING exception replaced with general statement "a word that is not a verb form and only ends in *-ing* is not affected." |
| 3.5 No Modal Verbs | Merged into 5.3 Imperative Form | Replaced by the ASD-aligned Rule 5.3, which specifies which modals ASD approves (*can*, *will*, *must*) and which it does not (*should*, *may*, *would*). |
| 3.6 No Nominalizations | 3.7 Use a Verb to Describe an Action | Renamed and renumbered to match ASD Rule 3.7. |

Rules 3.1 (Verb Forms), 3.3 (Past Participle as an Adjective), 3.4 (No Auxiliary Verbs) added, adopted from ASD Rules 3.1, 3.3, 3.4.

**Part 1 — Section 4: Sentences**

Section 4 now holds ASD Rules 4.1–4.5 (sentence clarity, word omission, vertical lists, connecting words, articles). In v0.4.0 these were scattered or absent.

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 4.1 Sentence Length | 4.1 Short and Clear Sentences | Sentence length limits moved to Rules 5.1 and 6.3 (matching ASD placement). Rule 4.1 now points to those rules. |
| 4.2 One Instruction per Sentence | 5.2 One Instruction in Each Sentence | Moved to Section 5. Exception added: two actions at the same time may appear in one sentence (ASD Rule 5.2). |
| 4.3 Conditional Syntax `[ASD+]` | 5.4 Condition First | Redesigned. `IF/THEN/ELSE` removed (`THEN` has a different ASD meaning; `ELSE` is not approved). Replaced with condition-first syntax using `IF` and a comma. Each branch is its own `IF` sentence with an exclusive condition. |
| 4.4 Do Not Omit Words | 4.2 Do Not Omit Words | Renumbered. Example changed: removed REPORT and THEN (not approved as verb/keyword). |

Rules 4.3 (Vertical Lists), 4.4 (Connecting Words), 4.5 (Articles and Demonstrative Adjectives) added, adopted from ASD Rules 4.3–4.5.

**Part 1 — Section 5: Procedural Writing**

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 5.1 Numbered Steps | Merged into 5.2 | Content absorbed into Rule 5.2. |
| 5.2 Name Every Result | 5.6 Name Every Result | Renumbered from 5.2 to 5.6 to free numbers for ASD rules. |
| 5.3 Notes | 5.5 Notes | Expanded significantly. Now specifies: no instruction, no requirement, no limit, no imperative form; must follow the step it explains; 25-word sentence limit; includes the "read steps without notes" test. Example corrected: old example "NOTE: The SKILL Parse_Data accepts JSON only" gave a limit, which a NOTE must not do. |

Rule 5.1 (Maximum 20 Words) added. Specifies that GUARDRAIL and CAUTION also follow the 20-word limit.
Rule 5.2 (One Instruction in Each Sentence) added with the simultaneous-action exception.
Rule 5.3 (Imperative Form) added from v0.4.0 Rule 3.1, expanded with ASD modal guidance.
Rule 5.4 (Condition First) redesigned from v0.4.0 Rule 4.3 (see Section 4 above).
Rule 5.5 (Notes) expanded from v0.4.0 Rule 5.3 (see above).

**Part 1 — Section 6: Descriptive Writing**

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 6.1 Context Framing `[AI]` | 6.7 Where Descriptive Text Appears `[AI]` | Renumbered. Tightened: a NOTE is no longer permitted in the CONTEXT field. |
| 6.2 Separate Fact from Command `[AI]` | 6.8 Separate Fact from Command `[ASD]` | Retag from `[AI]` to `[ASD]`: ASD-STE100 Section 6 states that descriptive writing does not use the imperative form. |
| 6.3 Paragraph Limit `[ASD]` | Split into 6.5 and 6.6 | "One topic" → Rule 6.5. "Six sentences" → Rule 6.6. |

Rules 6.1 (Give Information Gradually), 6.2 (Key Words and Key Phrases), 6.3 (Maximum 25 Words), 6.4 (Paragraphs) added, adopted from ASD Rules 6.1–6.4.

**Part 1 — Section 7: Safety Instructions**

Section renamed from "Guardrails and Safety Instructions" to "Safety Instructions (Guardrails)."

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 7.1 Instruction Levels `[ASD]` | 7.1 Identify the Level of Risk `[ASD]` | GUARDRAIL description changed from "a prohibition for an action that causes a security, privacy, safety, or irreversible-data risk" to "risk of injury, a security or privacy breach, or a loss of DATA that cannot be reversed." CAUTION description changed from "reversible damage or a poor OUTPUT" to "damage to objects (DATA, TOOLs, or systems) that can be reversed." Rule added: when both levels occur together, use GUARDRAIL. |
| 7.2 Place the Guardrail Before the Action `[ASD]` | 7.4 Place the Safety Instruction Before the Step `[AI]` | Retag from `[ASD]` to `[AI]`: ASD-STE100 does not specify where a safety instruction goes. The false claim "this is the practice of ASD-STE100" was removed. Reason corrected: an LLM reads the whole prompt before it generates, so a trailing prohibition does not fail because the output has already started. |
| 7.3 Guardrail Syntax `[AI]` | 7.2 Start with a Clear Command or Condition `[ASD]` | Renumbered to match ASD Rule 7.2. Simplified: `DO NOT <ACTION>` → `DO NOT <step>`. |

Rule 7.3 (Explain the Risk) added, adopted from ASD Rule 7.3. All guardrail examples now include a risk sentence.
Rule 7.5 (Give an Alternative) added. `INSTEAD:` keyword replaced by `ALTERNATIVE:` (`instead` is not an approved word; `alternative` is).
Rule 7.6 (Enforce Critical Limits Outside the Prompt) added as a standalone rule.

**Part 1 — Section 8: Punctuation and Word Count**

Section renamed from "Punctuation, Variables, and Formatting" to "Punctuation and Word Count."

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 8.1 Notation `[AI]` | 8.8 Notation `[AI]` | Renumbered. Declared term reference changed from "Rule 1.3" to "Section 13." Wording: "would look like" → "look like." |
| 8.2 All-Caps Keywords `[AI]` | 8.9 All-Caps Keywords `[AI]` | Renumbered. |
| 8.3 Punctuation `[ASD]` | Split: 8.1 No Semicolons `[ASD]` and 9.6 Latin Abbreviations `[AI]` | Semicolons → Rule 8.1 (adopted from ASD Rule 8.1). Latin abbreviations → Rule 9.6 (adopted from ASD General Recommendation GR-6, not a rule). |
| 8.4 Delimit Untrusted Data `[AI]` | 8.10 Delimit Untrusted Data `[AI]` | Renumbered. Block keywords renamed from `BEGIN_DATA`/`END_DATA` to `BEGIN_DATA_BLOCK`/`END_DATA_BLOCK`. Noun `DATA block` replaced by dictionary term `DATA_BLOCK`. Reference to enforcement changed from "Rule 7.2" to "Rule 7.6." |

Rules 8.2 (Hyphens), 8.3 (Parentheses), 8.4 (Colon in a Vertical List), 8.5 (Text in Parentheses), 8.6 (Count as One Word), 8.7 (Hyphenated Words) added, adopted from ASD Rules 8.2–8.7. Rule 8.6 AI extension: variables, declared terms, and payload blocks each count as one word. (v0.4.0 said payload counted as zero words, which contradicted ASD Rule 8.6.)

**Part 1 — Section 9: Writing Practices**

| v0.4.0 rule | v0.5.0 rule | Change |
| :--- | :--- | :--- |
| 9.1 No Pronouns `[ASD+]` | 9.5 Pronouns `[ASD+]` | Renumbered. Pronoun list changed: *he*, *she* moved to Rule 9.7 (gender-neutral language, GR-7). *that* as a pronoun no longer listed separately; note that demonstrative adjective *this*/*these* before a noun is now permitted (ASD Rule 4.5). Added: *you*/*we* permitted if exactly one reader or writer exists. Example corrected: removed "THEN REPORT" and "PARSE action" (verb as noun). |
| 9.2 No Phrasal Verbs `[ASD]` | 9.3 No Phrasal Verbs `[ASD]` | Renumbered. Minor wording: "form a new meaning" → "make a new meaning." |
| 9.3 Literal Payload `[AI]` | 9.8 Literal Payload `[AI]` | Renumbered. `DATA block` → `DATA_BLOCK`. |

Rule 9.1 (Use a Different Sentence Construction) added, adopted from ASD Rule 9.1. This was v0.4.0 Rule 1.4, retag from `[AI]` to `[ASD]`.
Rule 9.2 (Use Each Approved Word Correctly) added, adopted from ASD Rule 9.2.
Rule 9.4 (Consistent Style) added, adopted from ASD Rule 9.4.
Rule 9.6 (Latin Abbreviations) added `[AI]` (adopted from ASD GR-6, promoted from recommendation to rule).
Rule 9.7 (Gender-Neutral Language) added, adopted from ASD GR-7.

General Recommendations section added at the end of Part 1, noting AI-STE-specific guidance for GR-1, GR-2, GR-5, and GR-8.

**Part 2 — Sections 10, 11, 12: Architecture**

| Area | v0.4.0 | v0.5.0 | Change |
| :--- | :--- | :--- | :--- |
| AGENT field table | `REQUIRED` / `OPTIONAL` | `NECESSARY` / `OPTIONAL` | `REQUIRED` is not an approved adjective in ASD-STE100; NECESSARY is. |
| AGENT ROLE limit | "maximum three nouns" | "maximum three words" | Correct reading of Rule 2.1. |
| AGENT GUARDRAIL placeholder | `DO NOT <ACTION>` | `DO NOT <step>` | ACTION is a noun; the placeholder now uses STEP, which matches the verb Rule 3.7. |
| AGENT example role | `Invoice validator` | `Invoice_Validator` | Declared term notation (capital first letter, underscores). |
| AGENT example GOAL | "REPORT each ERROR" | "TELL the USER about each ERROR" | REPORT is a noun in ASD-STE100. |
| AGENT example CONTEXT | "receives" | "gets" | Consistent with approved vocabulary. |
| AGENT example declarations | `DOMAIN_NOUN:` | `TECHNICAL_NOUN:` | Keyword change (see Section 13). |
| AGENT example GUARDRAIL | "card number" | "password" | *Card number* is two words that ASD does not approve. *Password* is used by ASD-STE100 itself. Risk sentence added to each GUARDRAIL. |
| AGENT example DATA block | `DATA block` | `DATA_BLOCK` | Dictionary term (see Rule 8.10). |
| SKILL field name | `ACTION` | `STEPS` | ACTION is the structural noun for a single step; the field name is STEPS. |
| SKILL field table | `REQUIRED` | `NECESSARY` | Same as AGENT. |
| SKILL TRIGGER | "contains" | "has" | Minor wording. |
| SKILL INPUT marking | `REQUIRED` / `OPTIONAL` | `NECESSARY` / `OPTIONAL` | Same as AGENT. |
| SKILL ON_ERROR | "REPORT the ERROR to the USER" | "TELL the USER about the ERROR" | REPORT is a noun. |
| PLAN TASK field | `ACTION` | `STEP` | Matches SKILL field rename. |
| PLAN ON_ERROR | "HALT the PLAN. REPORT the ERROR" | "STOP the PLAN. TELL the USER about the ERROR" | HALT replaced by STOP; REPORT (noun) replaced by TELL. |
| PLAN field table | Added | `PLAN`, `WORKFLOW`, `ON_ERROR` NECESSARY; `DEPENDENCY` NECESSARY when needed. | Table was absent in v0.4.0. |
| PLAN note on GUARDRAIL/DEPENDENCY | Prose | Removed | Content now in field table. |

**Part 3 — Section 13: Technical Terms Protocol**

- Section renamed from "Vocabulary Extensibility Protocol" to "Technical Terms Protocol."
- Introduction expanded: ASD-STE100 does not list technical nouns and technical verbs; each domain declares its own.
- Step 1: `DOMAIN_NOUN`/`API_VERB` keywords replaced by `TECHNICAL_NOUN`/`TECHNICAL_VERB`.
- Steps added: select a term of not more than three words (Rule 1.9); do not use slang or jargon (Rule 1.10); use only Layer 1 and Layer 2 words in the definition; do not declare a prohibited alternative; write a declared verb in ALL CAPS; do not use a declared noun as a verb or a declared verb as a noun.
- Example: `DOMAIN_NOUN: Stripe_Invoice = A bill that the Stripe service issues to one customer` → `TECHNICAL_NOUN: Stripe_Invoice = The data that tell a person the quantity to pay to Stripe` (removed *bill*, which ASD does not approve; removed *customer*, which was a prohibited alternative to USER).
- Example: `API_VERB: POST = To transmit data to an HTTP endpoint` → `TECHNICAL_VERB: POST = To supply DATA to an API with the HTTP POST method` (removed *transmit*, a prohibited alternative to ROUTE; used approved vocabulary).

**Part 3 — Section 14: Core AI-STE Dictionary**

| Change | Detail |
| :--- | :--- |
| Introductory notes | Added: definitions in Section 14 are reference text for humans and are not in STE; source codes (ASD, ASD\*, TV, TN) explained; † marker explained. |
| Column names | "Unapproved Synonyms" → "Prohibited Alternatives." "Approved Word" unchanged. "Source" column added. |
| HALT removed | Replaced by STOP (ASD-approved verb). HALT is not in the ASD-STE100 dictionary. |
| REPORT removed | REPORT is approved in ASD-STE100 as a noun only. Replaced by TELL for delivering information to a person. |
| RETURN removed | "VALIDATE returns an ERROR" (verb as noun) corrected in Rule content. |
| STOP added | ASD-approved verb: "To cause the end of a TASK, a PLAN, or a step." |
| TELL added | ASD-approved verb: "To supply facts, as a STRING, to a person." |
| WAIT redefined | "To delay an operation until an event occurs" → "To stop work on a step while a different event occurs" (removed *delay* and *operation*, which are prohibited/unapproved). |
| COMPARE redefined | "To state how two values differ" → "To examine two values for differences." |
| ASK, COMPARE, WAIT, STOP, TELL marked ASD | These are approved ASD-STE100 verbs. |
| EXECUTE, EXTRACT, FORMAT, GENERATE, PARSE, RETRIEVE, ROUTE, STORE, VALIDATE, ANALYZE marked TV | Technical verbs not in the ASD general dictionary. |
| ACTION removed from 14.3 | ACTION was used as a field name. Replaced by STEP (single instruction) and STEPS (field name). |
| STEP and STEPS added to 14.3 | STEP = one instruction in a SKILL or TASK (ASD-approved). STEPS = field name for the numbered list in a SKILL. |
| ALTERNATIVE added to 14.3 | Replaced `INSTEAD:` keyword in Rule 7.5. |
| DATA_BLOCK added to 14.3 | Replaced the informal "DATA block" phrase. |
| REQUIRED removed from 14.4 | REQUIRED is not an approved ASD-STE100 adjective. |
| NECESSARY added to 14.4 | ASD-approved adjective: "That must be present for a SKILL to complete." |
| OPTIONAL redefined | "Not REQUIRED" → "Not NECESSARY." |
| 14.4 note added | TRUE, FALSE, NULL, and VALID are treated as state names (analogous to colors in ASD Rule 1.5). |
| 14.5 conjunctions note | "Use in conditions only (Rule 4.3)" → "Use in conditions only (Rule 5.4)." "THEN and ELSE are not AI-STE keywords" added. IF/THEN/ELSE entry replaced by four separate entries for AND, IF, NOT, OR. |
| Prohibited column | † added to single words that ASD-STE100 approves. For example, *get*†, *read*†, *send*†, *tell*†. |
| Source column | Each entry now shows ASD (compatible meaning), ASD\* (different meaning, see Appendix B), TV (technical verb), or TN (technical noun). |

**Part 4 — Section 15: Conformance**

- Checklist: 8 items → 9 items. Item 1 adds part-of-speech check (Rule 1.2). Item 2 changed from "no unapproved synonym" to "no prohibited alternative" (Rule 1.15). Item 3 expanded: NOTE sentences also have a 25-word limit (Rule 5.5). Item 4 adds the simultaneous-action exception. Item 5 pronoun rule reference changed from 9.1 to 9.5. Item 6 adds Rules 3.2, 3.4. Item 7 added (no technical verb as noun, no technical noun as verb). Item 8 adds Rule 7.3 (risk sentence). Item 9 replaces "REQUIRED" with "NECESSARY."
- Section 15.2: automated checks now note that a script can check Item 1 using the ASD-STE100 word list. Reference for "meaning of each word" changed from Rule 1.4 to Rule 9.1.

### Added

**Document Overview**
- Subsection "How AI-STE Relates to ASD-STE100" added.

**Part 1**
- Rules 1.3, 1.4, 1.6, 1.7, 1.8, 1.9, 1.10, 1.13, 1.14 (adopted from ASD Rules 1.3–1.14).
- Rule 1.15 Prohibited Alternatives (AI-specific).
- Rule 2.2 Long Technical Nouns (adopted from ASD Rule 2.2).
- Rules 3.1, 3.3, 3.4 (adopted from ASD Rules 3.1, 3.3, 3.4).
- Rules 4.3, 4.4, 4.5 (adopted from ASD Rules 4.3–4.5).
- Rules 5.1, 5.2, 5.3 (adopted from ASD Rules 5.1–5.3).
- Rules 6.1, 6.2, 6.3, 6.4 (adopted from ASD Rules 6.1–6.4).
- Rules 7.3 Explain the Risk, 7.5 Give an Alternative, 7.6 Enforce Critical Limits Outside the Prompt.
- Rules 8.2, 8.3, 8.4, 8.5, 8.6, 8.7 (adopted from ASD Rules 8.2–8.7).
- Rules 9.1, 9.2, 9.4 (adopted from ASD Rules 9.1, 9.2, 9.4).
- Rules 9.6 Latin Abbreviations, 9.7 Gender-Neutral Language, 9.8 Literal Payload.
- General Recommendations section.

**Part 3**
- Dictionary entries: STOP, TELL, ALTERNATIVE, DATA_BLOCK, NECESSARY, STEP, STEPS.
- Source column in all dictionary tables.
- Introductory notes in Section 14.
- Section 14.4 note on state names.
- Section 14.5 note on THEN/ELSE.

**Appendices**
- Appendix A rebuilt rule-by-rule (53 rows) from the ASD-STE100 Issue 9 PDF. Previous Appendix A was topic-level only and was based on published summaries, not the primary source.
- Appendix B added: vocabulary conflicts between AI-STE Layer 2 words and ASD-STE100 approved meanings. Lists AGENT, TOOL, ERROR (meanings override ASD), REPORT (noun only in ASD, not used as verb), THEN (not used as keyword), ELSE (not approved).

### Removed

**Part 1**
- Rule 1.2 One Word, One Meaning (content moved to Rule 1.11 and Rule 1.15).
- Rule 4.3 Conditional Syntax `IF/THEN/ELSE` (replaced by Rule 5.4 and paired `IF` sentences).
- Rule 5.1 Numbered Steps (merged into Rule 5.2).
- Rule 9.3 Literal Payload (renumbered to 9.8).

**Part 3 — Dictionary**
- HALT (not in ASD-STE100 dictionary; replaced by STOP).
- REPORT as a verb (approved only as noun in ASD-STE100; replaced by TELL).
- RETURN (content absorbed into VALIDATE definition and SKILL template).
- ACTION as a structural noun (replaced by STEP and STEPS).
- REQUIRED as an adjective (replaced by NECESSARY).
- `IF / THEN / ELSE` as a single dictionary entry (replaced by individual entries for IF, AND, OR, NOT; THEN and ELSE removed as AI-STE keywords).
- "Unapproved Synonyms" column (replaced by "Prohibited Alternatives" with † markers).

**Appendices**
- Previous Appendix A (topic-level mapping based on inferred rules) replaced by the rule-by-rule Appendix A.
