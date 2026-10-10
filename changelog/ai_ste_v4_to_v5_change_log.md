# AI-STE v0.4.0 → v0.5.0: Critique and Change Log

## 1. How this review was done

*   **Primary source:** `ASD-STE100_ISSUE9.pdf` (Issue 9, 2025-01-15, 434 pages). The earlier review (v3.0 → v0.4.0) could not read it. All earlier statements about ASD rules were therefore inferred. This review checks them.
*   **What I read:** The general introduction, the table of contents, and Part 1 (Writing rules, Sections 1 to 9, 53 rules). I read Sections 2 to 9 in full. I read Section 1 in compressed form (rule statements and key examples). I also parsed the Part 2 dictionary word list (875 approved words and 1,274 non-approved words) so that I could check every AI-STE term against it.
*   **Checks run on the files** (scripts, not judgment):
    1.  A word-by-word check of the examples against the real ASD-STE100 approved word list.
    2.  A check of whether a dictionary definition uses a word that the dictionary prohibits.
    3.  A check of "-ing" words, pronouns, modal verbs, semicolons, and sentence length in the examples.
    4.  A comparison of every prohibited word and every Layer 2 word with its ASD-STE100 status and meaning.
*   **Copyright:** ASD limits redistribution of the standard. The revised file paraphrases rules in new words, uses its own examples, and does not copy the dictionary.
*   **Not verified:** Whether any AI-STE rule improves LLM results. ASD-STE100 makes no such claim. Section 15.3 of the standard describes a test.

## 2. Critique summary: verified problems in v0.4.0 (most serious first)

| # | Severity | Finding in v0.4.0 | Evidence in ASD-STE100 Issue 9 |
| :--- | :--- | :--- | :--- |
| 1 | **High** | Rule 2.1 limits "three **nouns**". | Rule 2.1 limits a multi-word noun to three **words**. Adjectives count. |
| 2 | **High** | Rule 7.2 says that putting a safety instruction before the step "is the practice of ASD-STE100" (tagged `[ASD]`). | Section 7 does not state where a safety instruction goes. Rule 7.2 only says to start the instruction with a command or condition. |
| 3 | **High** | Rule 7 has no rule to explain the risk. | Rule 7.3 requires an explanation of the risk or result. |
| 4 | **High** | The standard uses **REPORT as a verb**, uses a verb as a noun ("the PARSE action", "VALIDATE returns an ERROR"), and bans STOP and TELL. | REPORT is approved as a noun only (Rule 1.2). Technical verbs are not nouns (Rule 1.13). STOP and TELL are approved verbs. HALT is not in the dictionary. |
| 5 | **High** | Layer 2 reuses words that ASD-STE100 approves with a **different meaning** (AGENT, TOOL, ERROR) and v0.4.0 does not say so. | AGENT = a material (for example, a cleaning agent). TOOL = a physical object. ERROR = a difference from the correct value. |
| 6 | **Medium** | The `THEN` keyword conflicts with an approved meaning, and `ELSE` is not approved. | THEN is approved with the meaning "immediately after in time or sequence". ELSE is not in the dictionary. Rule 5.4 uses a condition, a comma, and an instruction. |
| 7 | **Medium** | v0.4.0 prohibits 154 single words. **43 of them are approved in ASD-STE100** (for example *do*, *start*, *stop*, *tell*, *get*, *send*, *when*, *unless*). The standard banned *do* and then used `DO NOT` as its prohibition syntax. | Rule 1.3: an approved word keeps its approved meaning. |
| 8 | **Medium** | The "-ing" rule has an exception for STRING. It does not mention the real exceptions. | Rule 3.5 limits the *-ing* form of a **verb**. It allows a technical noun and a modifier in a technical noun. |
| 9 | **Medium** | The pronoun rule bans *this*, *these*, and *those* even before a noun. | Rule 4.5 **encourages** a demonstrative adjective before a noun. |
| 10 | **Medium** | v0.4.0 says "Every example marked Approved follows the rules." The word check found **39 tokens** in the normative examples that are not Layer 1 or Layer 2 words. About half are real problems (for example *actor*, *bill*, *buffer*, *customer*, *identifier*, *issues*, *personal*, *session*, *size*, *summary*). The rest are template placeholders and inflected forms of AI-STE verbs. | Rule 1.1 and the dictionary. |
| 11 | **Medium** | Many ASD rules are missing: 1.4 to 1.14, 2.2, 3.1, 3.3, 3.4, 4.3 to 4.5, 5.5 limits, 6.1, 6.2, 6.4 to 6.6, 7.3, 8.2 to 8.7, 9.1, 9.2, and 9.4. | Part 1 of the standard. |
| 12 | **Medium** | The word count method says "do not count literal payload" (zero words). | Rule 8.6 counts quoted text as **one** word. |
| 13 | **Low** | v0.4.0 tags Rule 1.4 (meaning outranks vocabulary) as AI-specific and Rule 8.3 (Latin abbreviations) as an ASD rule. | Rule 1.4 is ASD Rule 9.1. Latin abbreviations are a general **recommendation** (GR-6), not a rule. |
| 14 | **Low** | The notes example "accepts JSON only" gives a limit. | Rule 5.5: a note has no instruction, no requirement, and no limit. |
| 15 | **Low** | v0.4.0 uses `ACTION`, `INSTEAD`, and `REQUIRED`. | ASD-STE100 does not approve *action* (use STEP), *instead* (use ALTERNATIVE), or *require* (use NECESSARY). |
| 16 | **Low** | v0.4.0 requires Layer 1 words in declared definitions (Section 12) but its own Section 13 definitions do not follow that rule. | The ASD-STE100 dictionary also says that its definition text is not in STE. |

## 3. Change log (each change with its reason)

*Type: **Error** = wrong or self-contradicting. **Gap** = missing ASD rule or content. **Align** = closer to ASD-STE100. **Clarity** = ambiguity removed. **Transparency** = a deviation is now visible.*

### 3.1 Document level

| ID | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- |
| G-1 | Version 0.4.0 → 0.5.0. Rules 1.1 to 9.4 now use the **same numbers and topics as ASD-STE100**. AI-specific rules continue the numbering (for example, 5.6). | Align | The earlier goal of "structural parity" was not met. In v0.4.0, rule numbers and some section titles differed from ASD-STE100 (for example, the section of the pronoun rule and the sentence-length rule). The numbering change breaks references to v0.4.0 rule numbers, so this is a major version. |
| G-2 | Overview: exact issue date (2025-01-15), aviation origin, and the statement that STE is not for use alone. | Align | Taken from the general introduction. v0.4.0 gave an approximate date. |
| G-3 | Attribution: EU trademark, no copy of the dictionary or examples, and the ASD statement on LLMs (translation only). | Gap | v0.4.0 omitted the trademark and the redistribution limit. Without the LLM note, a reader could think that ASD supports LLM instruction use. ASD makes no such claim. |
| G-4 | New section "How AI-STE Relates to ASD-STE100". | Transparency | Shows the numbering rule, the tags, and where Appendix B lists conflicts. |
| G-5 | Scope: Section 14 definitions are reference text. Definitions that an AGENT must read (Section 13) must follow AI-STE. | Error | Finding 16. This follows the ASD-STE100 dictionary practice. |
| G-6 | Example vocabulary rule now allows technical nouns that ASD-STE100 itself uses (for example, *password*). | Clarity | I checked that ASD-STE100 uses *password* in an STE example sentence. |

### 3.2 Section 1: Words

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| W-1 | 1.1 | Split into Rules 1.1 to 1.4 (layers, part of speech, approved meaning, verb forms). Layer 2 words that ASD does not approve are technical nouns and technical verbs. | Gap | ASD Rules 1.1 to 1.4. |
| W-2 | 1.1 example | Added a Rule 1.2 example: REPORT → TELL. | Error | Finding 4. v0.4.0 used REPORT as a verb. |
| W-3 | (absent) | Added Rules 1.5 to 1.14: technical nouns (1.5 to 1.11), technical verbs (1.12, 1.13), spelling (1.14). Each has an AI example where useful. | Gap | Finding 11. v0.4.0 had one vague rule (1.3) for what ASD covers in ten rules. |
| W-4 | 1.2 (synonyms) | Replaced by Rule 1.11 (same technical noun for the same item) and Rule 1.15 (prohibited alternatives). | Align | The "one word, one meaning" idea is not one ASD rule. ASD achieves it with the dictionary and Rule 1.11. v0.4.0 presented it as if it were ASD Rule 1.2. |
| W-5 | 1.3 `DOMAIN_NOUN`, `API_VERB` | Renamed `TECHNICAL_NOUN`, `TECHNICAL_VERB`. | Align | Issue 9 uses these terms. "API verb" is also wrong for verbs that have no API. |
| W-6 | 1.4 "Meaning outranks vocabulary" `[AI]` | Moved to Rule 9.1 and tagged `[ASD]`. | Error | Finding 13. It is ASD Rule 9.1. |
| W-7 | (absent) | New Rule 1.15: prohibition applies to the **meaning** of the Layer 2 word. † marks words that ASD approves. | Clarity | Finding 7. A blanket ban on *do* contradicted `DO NOT`. A ban on *when* would also remove a normal use of time. |

### 3.3 Section 2: Multi-word nouns

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| N-1 | 2.1 | "Three nouns" → "three words". Approved example: "The MEMORY limit for one USER." | Error | Finding 1. The v0.4.0 approved example also used *size*, which ASD does not approve. |
| N-2 | (absent) | Added Rule 2.2 (long technical nouns). | Gap | ASD Rule 2.2. It fits with `DEFINITIONS`. |
| N-3 | 2.1 (title "Noun Clusters") | Title → "Multi-Word Nouns". | Align | The Issue 9 title. |

### 3.4 Section 3: Verbs

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| V-1 | 3.1 (imperative) | Moved to Rule 5.3. | Align | ASD puts the imperative in Rule 5.3. |
| V-2 | 3.2 (tenses) | Kept as Rule 3.2. Added Rules 3.1, 3.3, 3.4. | Gap | Finding 11. ASD Rules 3.1, 3.3 (past participle as adjective), and 3.4 (no auxiliary-verb constructions) were missing. |
| V-3 | 3.3 (active voice) | Now Rule 3.6. | Align | Same content. |
| V-4 | 3.4 (no "-ing") | Now Rule 3.5. The rule now limits *-ing* **verb forms** and allows a technical noun and a modifier in a technical noun. The STRING exception is replaced by a note. | Error | Finding 8. STRING is not a verb form. |
| V-5 | 3.5 (modals) | Moved to Rule 5.3 as `[ASD+]`. Added the ASD list of approved modals (*can*, *will*, *must*) and the limit on *must* before an imperative. | Align | v0.4.0 did not state which modals ASD approves. ASD Rule 5.3 limits *must*. |
| V-6 | 3.6 (nominalization) | Now Rule 3.7. | Align | Same content. |

### 3.5 Section 4: Sentences

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| S-1 | 4.1 (20/25 words) | Limits moved to Rules 5.1 and 6.3. Rule 4.1 now points to them. Added: safety instructions follow the 20-word limit, and a note has a limit of 25 words. | Align | ASD places the limits in Sections 5 and 6. v0.4.0 missed the limits for safety instructions and notes. |
| S-2 | 4.2 (one instruction) | Moved to Rule 5.2. Added the exception for two actions at the same time. | Gap | ASD Rule 5.2 has the exception. v0.4.0 had a stricter rule without a reason. |
| S-3 | 4.3 (`IF/THEN/ELSE`) | Moved to Rule 5.4: condition, comma, instruction. Removed `THEN` and `ELSE`. Each branch has its own `IF` with a complete, exclusive condition. No nested `IF`. | Error | Findings 6. THEN has a different approved meaning. ELSE is not approved. A separate `ELSE` line can attach to the wrong `IF` in a long prompt. |
| S-4 | 4.4 (do not omit words) | Now Rule 4.2. It also covers the subject and contractions. The v0.4.0 claim "keep articles" is now Rule 4.5 with the ASD exception for general statements. | Error | ASD Rules 4.2 and 4.5. |
| S-5 | (absent) | Added Rules 4.3 (vertical lists), 4.4 (connecting words), 4.5 (articles and demonstrative adjectives). | Gap | Finding 11. |

### 3.6 Section 5: Procedural writing

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| P-1 | 5.1 (numbered steps) | Merged into Rule 5.2. | Align | ASD Rule 5.2 covers the sequence of steps. |
| P-2 | 5.2 (name every result) | Now Rule 5.6 `[AI]`. | Align | Frees Rule 5.2 for the ASD rule. |
| P-3 | 5.3 (notes) | Now Rule 5.5. A note has no instruction, requirement, or limit, no imperative, and a 25-word limit. Added the "read the steps without the notes" test. | Gap | Finding 14. |
| P-4 | 5.3 example | "The SKILL Parse_Data accepts JSON only" → "The `[user_data]` is NULL for a new USER." | Error | The old note gave a limit. |

### 3.7 Section 6: Descriptive writing

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| D-1 | 6.1 (context only) | Now Rule 6.7 `[AI]`. No `NOTE` in `CONTEXT`. | Align | ASD notes appear in procedures. ASD allows a note in a description only for illustrations and tables. |
| D-2 | 6.2 (fact or command) | Now Rule 6.8, tagged `[ASD]`. | Error | ASD Section 6 states that descriptive writing does not use the imperative. v0.4.0 tagged it `[AI]`. |
| D-3 | 6.3 (paragraph limit) | Split into Rules 6.5 and 6.6. Added 6.1, 6.2, 6.4. | Gap | Finding 11. |

### 3.8 Section 7: Safety instructions

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| F-1 | 7.1 (levels) | Added the ASD definitions of warning and caution, the rule that the higher level wins, and the statement that ASD allows other words if Rules 7.1 to 7.3 are obeyed. | Clarity | This is the ASD basis for the words `GUARDRAIL` and `CAUTION`. |
| F-2 | 7.2 (placement, `[ASD]`) | Moved to Rule 7.4 and tagged `[AI]`. The false claim about ASD practice is removed. | Error | Finding 2. I read Section 7 and searched Part 1 for a placement rule. There is none. |
| F-3 | (absent) | Added Rule 7.3 (explain the risk). All guardrail examples now have a risk sentence. | Gap | Finding 3. An explanation also tells the model why the limit exists. |
| F-4 | 7.3 (syntax) | Now Rule 7.2 (start with a clear command or condition). | Align | ASD Rule 7.2. |
| F-5 | `INSTEAD:` | `ALTERNATIVE:` (Rule 7.5). | Error | *Instead* is not approved. *Alternative* is. |
| F-6 | 7.2 (enforcement) | Own Rule 7.6. | Clarity | A separate rule is easier to find and to test. |

### 3.9 Section 8: Punctuation and word count

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| M-1 | 4.1 (word count) | Replaced by Rules 8.4 to 8.7 (ASD). AI additions: a variable, a declared term, and a payload block count as one word. | Error | Finding 12. |
| M-2 | 8.1, 8.2, 8.4 | Now Rules 8.8, 8.9, 8.10. `DATA block` is now the dictionary term `DATA_BLOCK`. | Clarity | v0.4.0 used "DATA block" with no dictionary entry. |
| M-3 | 8.3 (semicolons, Latin abbreviations) | Semicolons → Rule 8.1 `[ASD]`. Latin abbreviations → Rule 9.6 `[AI]`. | Error | Finding 13. |
| M-4 | (absent) | Added Rules 8.2, 8.3, 8.5. Parentheses must not contain an instruction or `GUARDRAIL`. | Gap | ASD rules. The parentheses limit closes a way to hide an instruction. |

### 3.10 Section 9: Writing practices

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| X-1 | 9.1 (pronouns) | Now Rule 9.5. It bans pronoun use only. A demonstrative adjective before a noun is allowed. Added the *you*/*we* rule. | Error | Finding 9. |
| X-2 | 9.1 example | "IF the PARSE action returns an ERROR" → "IF `[parse_result]` is an ERROR". "Administrator" removed. | Error | Finding 4 (verb as noun) and Rule 1.1. |
| X-3 | 9.2 (phrasal verbs) | Now Rule 9.3. | Align | ASD Rule 9.3. |
| X-4 | 9.3 (payload) | Now Rule 9.8. | Align | Frees the ASD numbers. |
| X-5 | (absent) | Added Rules 9.2, 9.4, 9.7 and the list of general recommendations (GR-1, 2, 5, 8). | Gap | Finding 11. Rule 9.4 (consistent style) fits LLM use directly. |

### 3.11 Parts 2 and 3: Architecture and dictionary

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| A-1 | `ACTION` field | `STEPS` (SKILL) and `STEP` (TASK). Added STEP to the dictionary. | Align | Finding 15. |
| A-2 | `REQUIRED` | `NECESSARY` in all field tables and `INPUT` lists. | Align | Finding 15. |
| A-3 | `ROLE` "maximum three nouns" | "Maximum three words". Example role is now a declared term. | Error | Finding 1. The v0.4.0 example used undeclared technical nouns. |
| A-4 | Examples | `REPORT` → `TELL`, `HALT` → `STOP`, "summary" removed, "card number" → "password", every guardrail has a risk sentence. | Error | Findings 4, 3, 10. |
| A-5 | Section 12 → 13 numbering | Part 3 starts with the "Technical Terms Protocol" (Section 13). | Align | Uses the Issue 9 terms. |
| T-1 | Section 12 | Declared definitions must use Layer 1, Layer 2, or declared words. Rules 1.9, 1.10, 1.7, 1.13 apply. | Gap | ASD Rules 1.5 to 1.13. |
| T-2 | Section 13 columns | Added a **Source** column (ASD, ASD\*, TV, TN). | Transparency | The reader sees which words ASD approves. |
| T-3 | "Unapproved Synonyms" | Renamed "Prohibited Alternatives". † marks ASD-approved words. Removed *do*, *start*, *make sure*, *end*, *procedure*, *operation* where the ban was too broad. | Error | Finding 7. |
| T-4 | HALT | → STOP (ASD approved: "cause the end of…"). | Align | Rule 1.12: do not use a technical verb if an approved verb gives the meaning. |
| T-5 | REPORT | → TELL. | Error | Finding 4. |
| T-6 | RETURN | Removed. OUTPUT uses "supplies". VALIDATE: "the result is an ERROR". | Error | Finding 4. "VALIDATE returns" used a verb as a noun. |
| T-7 | COMPARE, WAIT | Use the ASD-STE100 meanings. | Align | v0.4.0 definitions differed from the approved meanings. |
| T-8 | AGENT, TOOL, ERROR | Marked ASD\*. Appendix B explains each override and gives a rename option. | Transparency | Finding 5. |
| T-9 | THEN, ELSE | Removed from 14.5. | Align | Finding 6. |
| T-10 | 13.4 | Note: TRUE, FALSE, NULL, VALID are state names, like colors in ASD Rule 1.5. | Clarity | They are adjectives, and ASD does not allow technical adjectives except colors. |
| T-11 | Definitions | Removed all prohibited words from definitions (OPTIONAL, TELL, NOTE, JSON, STOP, WAIT). | Error | The audit found these in my first v5 draft. Final result: 0 violations. |

### 3.12 Part 4 and appendices

| ID | v0.4.0 | Change in v0.5.0 | Type | Reason |
| :--- | :--- | :--- | :--- | :--- |
| Q-1 | Checklist (15.1) | Added items for Rules 1.7 and 1.13, the 20/25-word limits for each sentence type, and the risk sentence (7.3). | Gap | New rules. |
| Q-2 | 15.2 | A script can check Item 1 with the ASD-STE100 word list. | Clarity | I did this check in this review. |
| Q-3 | Appendix A | Topic level → **rule-by-rule** (53 rows) with status. | Gap | The primary source made this possible. |
| Q-4 | (absent) | Added Appendix B, the vocabulary conflict register. | Transparency | Finding 5. |

## 4. Kept unchanged on purpose

*   The 20-word and 25-word limits, the single-instruction rule, and the imperative form. They match ASD-STE100.
*   The words `GUARDRAIL` and `CAUTION`. ASD-STE100 allows other words if Rules 7.1 to 7.3 are obeyed.
*   Placement of guardrails before the step. Only the tag (`[AI]`) and the reason changed.
*   The pronoun ban. It is stricter than ASD-STE100 (a recommendation, not a rule) and is tagged.
*   The ALL CAPS convention, the notation rules, and `DATA_BLOCK` for untrusted data. They have no ASD equivalent and are tagged `[AI]`.
*   The AGENT, SKILL, and PLAN architecture, and the validation procedure in Section 15.3.

## 5. Audit results

| Check | v0.4.0 | v0.5.0 |
| :--- | :--- | :--- |
| Definitions that use a prohibited word | 0 (after the v3→v4 fix) | 0 |
| Prohibited words that ASD-STE100 approves, with no marking | 43 of 154 | 0 unmarked (each is marked † and limited to its meaning) |
| Words in normative examples that are not Layer 1 or Layer 2 | 39 tokens | 9 tokens. All are inflected AI-STE verbs, the declared verb PROVISION, the label "description", the tokenized term DATA_BLOCK, and *password* (used by ASD-STE100). |
| Modal verbs in instructions | 0 | 0 (the 6 flags are descriptive sentences, where ASD allows *can*) |
| ASD rules with the same number and topic | Not tested | 53 of 53 |

## 6. Open items for you

1.  **Decide on AGENT, TOOL, and ERROR.** AI-STE overrides three ASD-approved meanings. If you need strict ASD-STE100 parity, rename them (for example, `AI_AGENT`). I kept the common names because they are standard in LLM tooling.
2.  **Test the pronoun ban and the 20/25-word limits** on your target model (Section 15.3). ASD-STE100 does not give evidence for LLMs.
3.  **Ask STEMG about licensing** before you distribute AI-STE with or next to the ASD-STE100 dictionary. AI-STE does not copy the dictionary. Layer 1 refers to it.
4.  **Section 1 reading depth.** I read Section 1 in compressed form. Check the wording of Rules 1.5 to 1.14 in the PDF before you quote them.
5.  **Next step if you want it:** build the Section 15.2 checker with the licensed word list.
