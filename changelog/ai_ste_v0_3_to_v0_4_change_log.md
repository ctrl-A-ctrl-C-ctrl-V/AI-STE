# AI-STE v0.3.0 → v0.4.0: Critique and Change Log

## 1. How this review was done

*   **Source file:** `ai_ste_standard_v3_0.md`.
*   **Reference:** ASD-STE100 Issue 9 (January 2025). The official PDF at asd-ste100.org **blocked automated download**. I could not read the primary text. I checked structure and rules against published summaries and tools that encode Issue 9 (nine sections, 53 rules, 20/25-word limits, no *-ing* verb forms, active voice, no phrasal verbs, safety instructions in Section 7).
*   **Consequence:** Section-level claims are well supported. **Exact ASD rule numbers are unverified.** The revised file therefore cites ASD at the section-topic level only (Appendix A). Check rule numbers against the official PDF before you cite them.
*   **Self-consistency audit:** I ran a script on v0.3.0 and on v0.4.0. It checks whether definitions use words that the dictionary bans, whether definitions contain *-ing* words, and whether the examples follow the rules.

## 2. Critique summary (most serious first)

| # | Severity | Finding |
| :--- | :--- | :--- |
| 1 | **Critical** | The standard violates itself. The script found **15 of 42** dictionary entries whose definition uses a word from an *Unapproved Synonyms* column (for example, HALT is defined with "stop"). It found **7** definitions with *-ing* words (for example, "containing"), which Rule 3.3 bans. |
| 2 | **Critical** | Rule 1.1 says "use only approved words", but the dictionary has 42 entries. The examples need words that are not in it (*contains*, *include*, *save*, *admin*, *email*, *account*, *profile*). The rule cannot be followed. |
| 3 | **High** | The document claims to mirror ASD-STE100 "Sections 1 through 8". ASD-STE100 has **nine** sections. The AI-specific Section 9 collides with ASD Section 9 (Writing practices). Section 7 is mislabeled as "Warnings, Cautions, and Notes". |
| 4 | **High** | Section 9 and Section 10 say "exactly three fields" and then show four. Rule 6.1 and Rule 1.3 refer to an `AGENT CONTEXT` that no template contains. |
| 5 | **High** | The reason for Rule 7.1 is technically wrong. An autoregressive model reads the whole prompt before it generates, so a trailing prohibition does not fail "because the output token sequence has already initiated". |
| 6 | **High** | Several example "Approved" sentences change the meaning, use unlisted words, or break another rule (see Section 3, items R-6 to R-22). |
| 7 | **Medium** | Rules 4.2 (one instruction per sentence) and 4.3 (`IF / THEN / ELSE` in one sentence) conflict. |
| 8 | **Medium** | The text presents human-cognition claims ("analogous cognitive constraints") and numeric limits as established. No evidence is given. |
| 9 | **Medium** | The notation is ambiguous: `[brackets]` mean a runtime variable, a template placeholder, and a declared term. |
| 10 | **Medium** | The architecture has gaps: no INPUT field in a SKILL, no error handling, no AGENT assignment in a TASK, no handling of untrusted DATA, no conformance test. |
| 11 | **Low** | The Rule 2.1 example says "6 nouns" and has 8. |

## 3. Change log (each change with its reason)

*Type: **Error** = factually wrong or self-contradicting. **Gap** = missing content. **Align** = closer to ASD-STE100. **Clarity** = ambiguity removed. **Overclaim** = claim without support.*

### Document overview

| ID | v0.3.0 location | Change in v0.4.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| O-1 | Overview, para. 3 | "Sections 1 through 8" replaced by nine sections that follow the nine ASD topics. AI-specific content moved to Sections 10–15. | Error | ASD-STE100 has nine sections. The old claim was false, and the old Section 9 collided with ASD Section 9. |
| O-2 | Overview, para. 2 | Replaced "low bandwidth or non-native language constraints" with "aerospace maintenance documentation… non-native speakers". | Error | ASD-STE100 came from aerospace maintenance writing. "Low bandwidth" does not describe it. |
| O-3 | Overview, para. 2 | "LLMs suffer from analogous cognitive constraints" replaced by a list of four concrete failure modes. | Overclaim | The old sentence compared an LLM to human cognition without evidence. The failure modes are observable and testable. |
| O-4 | (absent) | Added "Status and Attribution": independent adaptation, no ASD endorsement, no claim of ASD compliance, ASD owns the copyright. | Gap | The document used the ASD-STE100 name and structure with no statement on status. A reader could think AI-STE is official or conformant. |
| O-5 | (absent) | Added a statement that the numeric limits are heuristics, and a pointer to Section 15.3. | Overclaim | The 20/25-word limits and the pronoun ban come from human technical writing. Nobody has shown that they hold for every LLM. |
| O-6 | (absent) | Added "Scope of the Rules": the rules apply to normative text. Commentary and literal payload are exempt. | Clarity | Without it, the document itself (which uses *-ing* words, pronouns, and long sentences in its prose) would be non-conformant. |
| O-7 | (absent) | Added Rule Tags `[ASD]`, `[ASD+]`, `[DEV]`, `[AI]` and a document map. | Clarity | The reader could not see which rules follow ASD-STE100 and which are new or stricter. |

### Part 1: Writing rules

| ID | v0.3.0 rule | Change in v0.4.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| R-1 | 1.1 | Added a three-layer vocabulary (ASD-STE100 general words → AI-STE core → declared terms). Unapproved-column words stay banned in every layer. | Error | A 42-word dictionary cannot support "use only approved words". ASD-STE100 itself has about 900 words. The layers make the rule possible and keep the AI-specific restrictions. |
| R-2 | 1.1 example | Kept the FORMAT example. Added JSON to the standard technical nouns (14.6). | Gap | The example used JSON, which v0.3.0 never declared. |
| R-3 | 1.2 example | New example: "Fetch the DATA. Pull the CONTEXT. Get the MEMORY." → "RETRIEVE…" for all three. | Error | The old "Approved" line used RECORD, METADATA, and LOGS. None of them is in the dictionary. |
| R-4 | 1.3 | Reference changed from "AGENT CONTEXT" to the `DEFINITIONS` block (Section 13). | Error | No template had an `AGENT CONTEXT`. The old reference pointed to nothing. |
| R-5 | (absent) | Added Rule 1.4, "Meaning Outranks Vocabulary". | Gap | The old Rule 3.4 example replaced "set up" with GENERATE, which changes the meaning. The standard had no rule to prevent that. |
| R-6 | 2.1 example | Corrected "6 nouns" to 8. Rewrote the "Approved" form with no unlisted words and no mixed capitalization. | Error | The script counted 8. The old "Approved" form capitalized SIZE and MEMORY at random, which broke Rule 8.2. |
| R-7 | 2.1 reason | Replaced "LLM attention mechanisms frequently misassociate…" with "it is not clear which noun modifies which". | Overclaim | The old sentence made an unsupported claim about attention mechanisms. |
| R-8 | 2.2 (Pronoun Elimination) | Moved to Rule 9.1. Section 2 now covers noun clusters only. | Align | ASD-STE100 treats pronouns under writing practices, not under noun clusters. |
| R-9 | 2.2 | Pronoun list extended (*he*, *she*, *those*, *which*). Marked `[ASD+]`: ASD-STE100 permits a pronoun with a clear reference. | Clarity | The old rule did not say that it is stricter than ASD-STE100. The old list also missed common pronouns. |
| R-10 | 2.2 example | Rewrote: "send it to the admin" → "REPORT the ERROR to the USER". Fixed lowercase "If". | Error | The old "Approved" line used *admin* (unlisted), used ROUTE for a human (ROUTE is defined for an AGENT or SKILL), and mixed `IF` and `If`. |
| R-11 | 3.1 | Kept. Added tag `[ASD]`. | Align | The rule is sound. |
| R-12 | (absent) | Added Rule 3.2, Permitted Verb Forms. | Gap | The core of ASD-STE100 Section 3 is a closed list of allowed forms. v0.3.0 had no such list. |
| R-13 | 3.2 | Rule renumbered to 3.3. The passive voice is now permitted in a descriptive sentence if the actor is unknown. | Align | ASD-STE100 permits this case. A total ban without a reason is a deviation that was not marked. |
| R-14 | 3.2 | Removed "the subject must always precede the verb". Added: the implied subject of an instruction is the AGENT. | Error | An imperative has no written subject. The old sentence contradicted Rule 3.1. |
| R-15 | 3.2 example | "The SKILL CALLS the API" → "The SKILL EXECUTES the TOOL". | Error | CALL and API were not in the dictionary. |
| R-16 | 3.3 | Rule renumbered to 3.4. Added an exception for approved nouns that end in *-ing*. | Error | STRING ends in *-ing*, so the dictionary broke its own rule. ASD-STE100 also permits *-ing* in a technical noun. |
| R-17 | 3.3 example | Replaced "To PROCESS the DATA, VALIDATE…" with a numbered PARSE → VALIDATE example. | Error | The old "Approved" example used PROCESS. PROCESS is an unapproved synonym of PARSE. |
| R-18 | 3.4 (Phrasal Verbs) | Moved to Rule 9.2. | Align | ASD-STE100 treats phrasal verbs under writing practices. |
| R-19 | 3.4 example | "RETRIEVE the account. GENERATE the profile." → "RETRIEVE the DATA. VALIDATE the DATA." | Error | GENERATE does not mean "set up". *Account* and *profile* were unlisted. "Carry out the check" now maps to VALIDATE. |
| R-20 | (absent) | Added Rule 3.5, No Modal Verbs. | Gap | The old Rule 3.1 example used "should". A modal verb hides whether an action is required. |
| R-21 | (absent) | Added Rule 3.6, No Nominalizations. | Gap | ASD-STE100 contains this rule. A noun that hides a verb ("perform a validation") makes the action less direct. |
| R-22 | 4.1 | Added a word-count method. | Gap | Limits of 20 and 25 words cannot be checked without a counting method. Variables, declared terms, and code needed a decision. |
| R-23 | 4.2 example | Used only dictionary words and the variable notation (`[user_id]`). | Error | The old example used *email* and *confirmation message*. |
| R-24 | 4.3 | `IF/THEN/ELSE` rewritten: one instruction per branch, `ELSE` on the next line, no nested `IF`, and operators only inside conditions. | Error | The old rule put two instructions in one sentence, which contradicted Rule 4.2. The old `ELSE` sentence was a fragment. |
| R-25 | 4.3 example | "HALT the EXECUTE operation" → "HALT the TASK". | Error | EXECUTE is a verb. The old line used it as a noun or adjective, which broke Rule 1.1. |
| R-26 | (absent) | Added Rule 4.4, Do Not Omit Words (articles, no contractions). | Gap | ASD-STE100 has this rule. LLM instructions in terse style ("validate JSON, report error if invalid") are ambiguous. |
| R-27 | 5.1 | Kept. | Align | The rule is sound. |
| R-28 | 5.2 | Reworded: a step that creates a result for a later step must `STORE` it in a named variable. | Clarity | The old rule ("declare its completion output") was vague. The old example used "Save", which is not an approved verb. |
| R-29 | (absent) | Added Rule 5.3, Notes. | Gap | ASD-STE100 uses notes for information only. v0.3.0 claimed to cover "Notes" but did not define one. |
| R-30 | 6.1 | Descriptive text allowed in `CONTEXT` or in a `NOTE`. | Clarity | The old rule needed a `CONTEXT` field that the AGENT template did not have. |
| R-31 | (absent) | Added Rule 6.3, paragraph limit (one topic, six sentences). | Align | ASD-STE100 has a paragraph limit for descriptive text. |
| R-32 | 7 (heading) | "Warnings, Cautions, and Notes" replaced by "Safety Instructions" with a mapping table (GUARDRAIL ≈ WARNING, CAUTION ≈ CAUTION). | Error | ASD-STE100 Section 7 covers warnings and cautions. v0.3.0 defined only GUARDRAIL. |
| R-33 | 7.1 reason | Rewrote the reason. Position reduces missed limits and fits step-by-step execution. It is not a guarantee. | Error | The old claim ("the output token sequence has already initiated") is wrong for a prompt that the model reads in full before it generates. |
| R-34 | (absent) | Added enforcement outside the prompt for critical guardrails. | Gap | A text instruction is not a security boundary. A critical limit needs a permission, a filter, or validation code. |
| R-35 | (absent) | Added the guardrail scope rule (before a `TASK`, or in the `AGENT` header). | Gap | v0.3.0 did not say how far a guardrail reaches. |
| R-36 | 7.2 | `[UNAPPROVED_ACTION]` → `<ACTION>`. Added the `INSTEAD:` line for a permitted alternative. | Error | The placeholder said "unapproved action", which has the wrong meaning. A prohibition with no alternative leaves the model without a path. |
| R-37 | 7.1 example | Removed the `ACTION:` label that no template defined. Added "in the OUTPUT" for scope. | Clarity | The label belonged to no architecture. |
| R-38 | 8.1 | Three distinct notations (runtime variable, template placeholder, declared term). | Clarity | One bracket style had three meanings. |
| R-39 | 8.2 | Added the lowercase convention for Layer 1 words and a note on the unproven effect. | Overclaim | The claim that caps "distinguish control structure" had no support. |
| R-40 | (absent) | Added Rule 8.3: no semicolons, no Latin abbreviations. | Align | These are ASD-STE100 punctuation and style rules. v0.3.0 itself used "e.g." in Rule 1.3. |
| R-41 | (absent) | Added Rule 8.4, Delimit Untrusted Data. | Gap | An orchestration standard needs a rule on user-supplied and retrieved text that carries instructions (prompt injection). |
| R-42 | (absent) | Added Rule 9.3, Literal Payload. | Gap | Without it, a quoted OUTPUT example or user DATA would break the rules. |

### Part 2: Architecture

| ID | v0.3.0 | Change in v0.4.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| A-1 | Section 9 | Renumbered to Section 10. | Align | Frees Section 9 for ASD "Writing practices". |
| A-2 | Section 9 | "Exactly three fields" corrected. REQUIRED and OPTIONAL fields are listed. | Error | The template showed four fields. |
| A-3 | Section 9 | Added `CONTEXT`, `DEFINITIONS`, and repeatable `GUARDRAIL` fields. `GUARDRAIL` precedes `SKILLSET`. | Gap | Rules 6.1 and 7.2 needed these fields. |
| A-4 | Section 9 | ROLE: "Two-Word Noun Phrase" → "noun phrase, maximum three nouns". | Clarity | The two-word limit was arbitrary and conflicted with Rule 2.1. |
| A-5 | Section 9 | Added a complete AGENT example. | Gap | There was no example to show the rules working together. |
| A-6 | Section 10 | Renumbered to Section 11. "Exactly three fields" corrected. | Error | The template showed four fields. |
| A-7 | Section 10 | "Deterministic" → "defined, bounded". | Overclaim | An LLM skill does not give the same OUTPUT each time. |
| A-8 | Section 10 | Added `INPUT` and `ON_ERROR` fields. | Gap | The dictionary defines PARAMETER and ERROR, but no field used them. |
| A-9 | Section 10 | Added a complete SKILL example. | Gap | See A-5. |
| A-10 | Section 11 | Renumbered to Section 12. A `TASK` now names its `AGENT` and its `ACTION` on separate lines. | Gap | A PLAN "routes across agents" but had no AGENT field. |
| A-11 | Section 11 | "TASK 2 REQUIRES TASK 1 completion" → "TASK_2 WAITS for the OUTPUT of TASK_1". Added `WAIT`. | Error | REQUIRES was not in the dictionary (REQUIRED is an adjective), and *completion* was a nominalization. |
| A-12 | Section 11 | Added `ON_ERROR` and an example. | Gap | See A-8. |

### Part 3: Dictionary and extensibility

| ID | v0.3.0 | Change in v0.4.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| D-1 | Section 12 | Renumbered to Section 13. Definitions must use approved words or earlier declared terms. Added the naming rule and the file scope. | Gap | The old example defined terms with unapproved words (*transmit*, *endpoint*, *gateway*). |
| D-2 | Section 12 example | Brackets removed from `Stripe_Invoice` and `POST`. New definitions use approved words. | Clarity | See R-38. |
| D-3 | 13.1 (10 verbs) | Rewrote all 10 definitions. Removed words from every *Unapproved* column. Examples: HALT "stop"→"cease", ROUTE "send"→"deliver", RETRIEVE "fetch"→"obtain". | Error | Audit finding 1. 15 definitions used a banned word. |
| D-4 | 13.1 | Added ASK, COMPARE, REPORT, RETURN, STORE, WAIT. | Gap | The examples and templates need these actions. v0.3.0 had none for a human, for a stored result, or for a delay. |
| D-5 | 13.1 | PARSE and EXTRACT redefined: PARSE splits a STRING into the parts of a SCHEMA. EXTRACT takes a defined part. | Clarity | The old PARSE definition contained EXTRACT. The two verbs overlapped. |
| D-6 | 13.1 | VALIDATE: "VALIDATE returns an ERROR if the DATA does not match." | Gap | The link between VALIDATE and ERROR was implied, not stated. Skills need it for error handling. |
| D-7 | 13.1 | Added CALL and INVOKE to the unapproved synonyms of EXECUTE. | Clarity | v0.3.0 used CALL in an "Approved" example. The synonym list now covers it. |
| D-8 | 13.2 | Rewrote definitions without banned words (CONTEXT, DATA, ERROR, PROMPT, STRING, and others). Removed "alphanumeric" from STRING. | Error | CONTEXT used "Background". DATA used "text". "Alphanumeric" wrongly excluded punctuation. |
| D-9 | 13.2 | Added USER and VARIABLE. | Gap | The new rules and examples need them. |
| D-10 | 13.2 | PARAMETER vs INPUT: PARAMETER is "a named value that a SKILL requires as INPUT". | Clarity | The two nouns overlapped. |
| D-11 | 13.3 | Converted from bullets to a table with an *Unapproved Synonyms* column. Added ACTION, CAUTION, DEFINITIONS, NOTE, ON_ERROR. Removed "-ing" words. | Gap | The old section had no synonym column, so the audit could not enforce the one-meaning rule on these nouns. |
| D-12 | 13.4 | Added EMPTY. NULL now says "A VARIABLE that has no value. An EMPTY STRING is not NULL." | Clarity | The old table listed *empty* and *blank* as synonyms of NULL. Those are different states in code. |
| D-13 | 13.4 | Rewrote NULL, VALID, and others without "-ing" words and without banned words. | Error | "Containing" and "Conforming" are *-ing* words. |
| D-14 | 13.5 | AND and OR rewritten without "evaluate" (an unapproved synonym of ANALYZE). OR is stated as inclusive. | Error | Audit finding 1. |
| D-15 | 13.5 | Added "Use in conditions only". | Clarity | Matches Rule 4.3. |
| D-16 | (absent) | Added 14.6 (API, JSON, LLM). | Gap | v0.3.0 used these nouns without a declaration. |
| D-17 | 13.x ban lists | Removed ordinary words from my first draft of the ban lists (for example *tool*, *rule*, *customer*, *operation*). | Clarity | A re-run of the audit showed that my own first draft banned words that definitions needed. The final v0.4.0 audit shows zero violations. |

### Part 4 and appendix

| ID | v0.3.0 | Change in v0.4.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| C-1 | (absent) | Added Section 15.1, an author checklist. | Gap | v0.3.0 had no test for a conforming text. |
| C-2 | (absent) | Added Section 15.2, automated checks. | Gap | It states which rules a script can check and which need a human. |
| C-3 | (absent) | Added Section 15.3, validation of the heuristics on the target model. | Overclaim | See O-5. |
| C-4 | (absent) | Added Appendix A, the ASD-STE100 mapping. | Gap | It makes each deviation visible. |

## 4. Kept unchanged on purpose

*   Imperative mood, 20/25-word limits, and one instruction per sentence. These match ASD-STE100.
*   The guardrail-before-action rule. Only the reason changed.
*   ALL CAPS for dictionary words. Only the claim about its effect changed.
*   The AGENT / SKILL / PLAN idea and the extensibility idea.

## 5. Open items for you

1.  **Check ASD rule numbers.** I could not read the primary PDF.
2.  **Replace the pronoun ban if you want ASD parity.** Rule 9.1 is stricter than ASD-STE100 on purpose. Run the test in Section 15.3 before you keep it.
3.  **Layer 1 words.** Rule 1.1 refers to the ASD-STE100 general dictionary. Obtain it from ASD and check that no Layer 1 word conflicts with an *Unapproved* column.
4.  **Version number.** I chose v0.4.0 because templates and section numbers changed in a way that breaks old documents.
