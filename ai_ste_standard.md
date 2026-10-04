# AI-STE: Simplified Technical English for AI Orchestration (v0.3.0)

## Document Overview & Structural Alignment

This specification adapts the **ASD-STE100 (Issue 9)** standard for Large Language Model (LLM) prompts, autonomous system instructions, and multi-agent orchestration files. 

ASD-STE100 was developed for human technicians operating under low bandwidth or non-native language constraints. LLMs suffer from analogous cognitive constraints: attention degradation across long contexts, semantic drift from multi-meaning words, coreference resolution errors from pronouns, and execution sequence errors from trailing guardrails.

To maintain structural parity with ASD-STE100, this document mirrors the ASD-STE100 chapter architecture (Sections 1 through 8) and introduces AI-specific architectural frameworks (Sections 9 through 11).

---

# PART 1: WRITING RULES

## Section 1: Words

### Rule 1.1: Approved Words
Use only approved words from the AI-STE Dictionary (Part 3) or explicitly defined technical terms. Use each word only as its assigned part of speech.

*   *Unapproved:* "FORMAT the DATA into a JSON format." (*FORMAT used as both verb and noun*)
*   *Approved:* "FORMAT the DATA to the JSON SCHEMA." (*FORMAT as verb; SCHEMA as noun*)

### Rule 1.2: One Word, One Meaning
Do not use synonyms. Do not use different words to describe the same object, state, or action.

*   *Unapproved:* "Retrieve the record, pull the metadata, and fetch the logs."
*   *Approved:* "RETRIEVE the RECORD. RETRIEVE the METADATA. RETRIEVE the LOGS."

### Rule 1.3: Technical Nouns and Verbs (Domain Extensibility)
When interacting with domain-specific APIs or databases, you may define custom `DOMAIN_NOUNS` (e.g., *Stripe_Customer*) and `API_VERBS` (e.g., *POST*, *ENCRYPT*). These must be declared in the AGENT CONTEXT prior to execution (see Section 12).

---

## Section 2: Multi-Word Nouns and Pronouns

### Rule 2.1: Noun Cluster Restrictions
Do not string together more than three nouns in sequence. Use prepositions to separate complex noun phrases. LLM attention mechanisms frequently misassociate modifier nouns in long clusters.

*   *Unapproved:* "User session context history memory buffer size limit." (6 nouns)
*   *Approved:* "Limit on the buffer SIZE for the MEMORY of the user session."

### Rule 2.2: Pronoun Elimination
Do not use pronouns (*it*, *they*, *this*, *these*, *them*, *its*). Always repeat the specific noun or variable name. Pronoun resolution is a primary cause of context drift and hallucination in multi-turn conversations.

*   *Unapproved:* "PARSE the JSON STRING. If **it** contains an ERROR, send **it** to the admin."
*   *Approved:* "PARSE the JSON STRING. IF the JSON STRING contains an ERROR, ROUTE the JSON STRING to the admin."

---

## Section 3: Verbs

### Rule 3.1: Imperative Mood
Use the imperative mood (direct command) for all instructional prompts.

*   *Unapproved:* "The AGENT should proceed to validate the string."
*   *Approved:* "VALIDATE the STRING."

### Rule 3.2: Active Voice Only
The subject performing the action must always precede the verb. Do not use passive constructions.

*   *Unapproved:* "The API is called by the SKILL."
*   *Approved:* "The SKILL CALLS the API."

### Rule 3.3: Prohibition of "-ing" Words (Gerunds and Participles)
Do not use present participles or gerunds (words ending in *-ing*). Words ending in *-ing* create syntactic ambiguity between nouns, adjectives, and verbs.

*   *Unapproved:* "When processing the DATA, start checking for errors."
*   *Approved:* "To PROCESS the DATA, VALIDATE the DATA for an ERROR."

### Rule 3.4: No Phrasal Verbs
Do not combine a verb with a preposition to create a functional meaning (e.g., *look up*, *carry out*, *set up*). Use single, explicit approved verbs.

*   *Unapproved:* "Look up the account and set up the profile."
*   *Approved:* "RETRIEVE the account. GENERATE the profile."

---

## Section 4: Sentences

### Rule 4.1: Sentence Length Limits
*   **Instructional Sentences (Prompts):** Maximum **20 words**.
*   **Descriptive Sentences (Context/Framing):** Maximum **25 words**.

### Rule 4.2: Single Instruction Per Sentence
Express only one command per sentence. Split multi-action sentences into sequential single-action statements.

*   *Unapproved:* "ANALYZE the INPUT, EXTRACT the email, and GENERATE a confirmation message."
*   *Approved:* "ANALYZE the INPUT. EXTRACT the email. GENERATE a confirmation message."

### Rule 4.3: Conditional Logic Syntax
Structure all conditional logic using strict `IF / THEN / ELSE` formatting. Place the condition before the required action.

*   *Approved:* "IF the DATA is NULL, THEN HALT the EXECUTE operation. ELSE ROUTE the DATA to the SKILL."

---

## Section 5: Procedural Writing (Workflows & Tasks)

### Rule 5.1: Sequential Numbering
Write step-by-step instructions as vertical numbered lists. Do not write sequential procedures in paragraph form.

### Rule 5.2: Explicit Task Completion
Every procedural step must declare its completion output before the next step begins.

*   *Approved:*
    1. RETRIEVE the user record. Save the result as `[user_data]`.
    2. VALIDATE `[user_data]` against the SCHEMA.

---

## Section 6: Descriptive Writing (System Context)

### Rule 6.1: Context Framing
Use descriptive writing exclusively in the `CONTEXT` field of an AGENT definition. Keep descriptive explanations factual, objective, and compliant with sentence length limits.

### Rule 6.2: Separation of Fact from Command
Descriptive sentences must state facts. Instructional sentences must start with imperative verbs. Do not mix descriptive facts and commands in the same sentence.

---

## Section 7: Guardrails and Safety Instructions

*(Equivalent to ASD-STE100 Section 7: Warnings, Cautions, and Notes)*

### Rule 7.1: Pre-Action Guardrail Placement
All negative constraints, safety boundaries, and `GUARDRAILS` must precede the actions to which they apply. LLMs execute auto-regressive generation; trailing constraints fail because the output token sequence has already initiated.

*   *Unapproved:* "GENERATE a summary report. DO NOT include personal identifier DATA."
*   *Approved:* 
    *   "GUARDRAIL: DO NOT include personal identifier DATA."
    *   "ACTION: GENERATE a summary report."

### Rule 7.2: Absolute Guardrail Syntax
Guardrails must use strict negative imperative syntax: `GUARDRAIL: DO NOT [UNAPPROVED_ACTION]`.

---

## Section 8: Punctuation, Variables, and Formatting

### Rule 8.1: System Variable Notation
Enclose all dynamic parameters, runtime variables, and user data fields in brackets `[like_this]` or inline code backticks `` `like_this` ``.

### Rule 8.2: All-Caps Keyword Formatting
Write approved dictionary terms and structural keywords in ALL CAPS when authoring system prompts to distinguish control structure from literal text payload.

---

# PART 2: STRUCTURAL ARCHITECTURE (AI-SPECIFIC CHAPTERS)

## Section 9: Agent Architecture

An `AGENT` is an autonomous entity capable of decision-making and tool execution. Every `AGENT` document must contain exactly three fields formatted as follows:

```
AGENT: [Agent_Name]
ROLE: [Two-Word Noun Phrase]
GOAL: [Single sentence <= 20 words defining objective]
SKILLSET:
  - [Approved SKILL 1]
  - [Approved SKILL 2]
```

---

## Section 10: Skill Architecture

A `SKILL` is a deterministic, modular capability. Every `SKILL` document must contain exactly three fields:

```
SKILL: [Skill_Name]
TRIGGER: [Specific event or INPUT state]
ACTION:
  1. [Imperative instruction 1]
  2. [Imperative instruction 2]
OUTPUT: [Defined SCHEMA or DATA type]
```

---

## Section 11: Plan & Workflow Architecture

A `PLAN` routes execution across multiple agents or steps. Every `PLAN` document must contain:

```
PLAN: [Plan_Name]
WORKFLOW:
  1. TASK 1: [Imperative action]
  2. TASK 2: [Imperative action]
DEPENDENCY:
  - TASK 2 REQUIRES TASK 1 completion.
```

---

# PART 3: THE APPROVED DICTIONARY & EXTENSIBILITY

## Section 12: Vocabulary Extensibility Protocol

When a domain requires words outside the dictionary:
1.  Declare `DOMAIN_NOUNS` under a `DEFINITIONS` header.
2.  Declare `API_VERBS` under the same header.
3.  Ensure no custom word duplicates a core dictionary word.

*Example:*
```
DEFINITIONS:
  - DOMAIN_NOUN: [Stripe_Invoice] = The billing record from the payment gateway.
  - API_VERB: [POST] = To transmit data to an HTTP endpoint.
```

---

## Section 13: Core AI-STE Approved Dictionary

### 13.1 Verbs (Actions)

| Approved Word | Definition for AI Context | Unapproved Synonyms |
| :--- | :--- | :--- |
| **ANALYZE** | To process DATA to identify patterns, sentiment, or entities. | Think about, evaluate, assess |
| **EXECUTE** | To run a script, TOOL, or defined SKILL. | Run, do, perform, start |
| **EXTRACT** | To pull targeted DATA from a larger CONTEXT. | Pull, grab, find, scrape |
| **FORMAT** | To change the structural presentation of DATA. | Change, convert, make into |
| **GENERATE** | To create text, code, or conceptual OUTPUT from a PROMPT. | Make, write, produce, draft |
| **HALT** | To stop an operation immediately. | Stop, quit, end, kill, abort |
| **PARSE** | To read structured DATA and EXTRACT components. | Read, process, interpret |
| **RETRIEVE** | To fetch DATA from a database, TOOL, or search engine. | Get, fetch, download, look up |
| **ROUTE** | To send DATA or a TASK to a specific AGENT or SKILL. | Send, pass, hand off, give |
| **VALIDATE** | To verify that DATA meets a REQUIRED SCHEMA. | Check, make sure, confirm |

### 13.2 Nouns (Data and Objects)

| Approved Word | Definition for AI Context | Unapproved Synonyms |
| :--- | :--- | :--- |
| **CONTEXT** | Background instructions or retrieved DATA framing a prompt. | Background, situation, setting |
| **DATA** | Raw text, numbers, JSON, or code processed by an AGENT. | Information, info, details |
| **ERROR** | A state where a SKILL, TOOL, or VALIDATE action fails. | Mistake, bug, glitch, fault |
| **MEMORY** | The stored record of previous turns in a session. | History, past, recall, transcript |
| **PARAMETER** | A defined variable REQUIRED to EXECUTE a SKILL. | Setting, argument, option |
| **PROMPT** | The formulated STRING sent to an LLM. | Question, query, command |
| **SCHEMA** | The defined structure that DATA must adhere to. | Format (as noun), layout |
| **STRING** | A sequence of alphanumeric characters. | Text, words, sentence |
| **TOOL** | An external script, API, or search function. | Plugin, extension, utility |

### 13.3 Nouns (Structural System Entities)

*These nouns are strictly reserved for defining system architecture:*

*   **AGENT**: Autonomous entity executing a PLAN.
*   **DEPENDENCY**: A TASK that must finish before another starts.
*   **GOAL**: The objective of an AGENT.
*   **GUARDRAIL**: A negative constraint or safety instruction.
*   **INPUT**: The DATA provided to a SKILL or AGENT.
*   **OUTPUT**: The DATA returned by a SKILL or AGENT.
*   **PLAN**: Routing document containing a WORKFLOW.
*   **ROLE**: Persona assigned to an AGENT.
*   **SKILL**: Defined, deterministic capability.
*   **SKILLSET**: The collection of SKILLs an AGENT possesses.
*   **TASK**: Unit of work within a WORKFLOW.
*   **TRIGGER**: Event that initiates a SKILL.
*   **WORKFLOW**: Sequential list of TASKs.

### 13.4 Adjectives (States and Conditions)

| Approved Word | Definition for AI Context | Unapproved Synonyms |
| :--- | :--- | :--- |
| **FALSE** | Negative Boolean state. | Wrong, incorrect, no |
| **NULL** | State of containing no DATA or value. | Empty, blank, missing |
| **OPTIONAL** | Not REQUIRED for execution. | Unnecessary, voluntary |
| **REQUIRED** | Mandatory for execution. | Needed, mandatory, crucial |
| **TRUE** | Positive Boolean state. | Right, correct, yes |
| **VALID** | Conforming to the expected SCHEMA or PARAMETER. | Good, clean, proper |

### 13.5 Conjunctions and Operators (Logic Gates)

| Approved Word | Definition for AI Context | Unapproved Synonyms |
| :--- | :--- | :--- |
| **AND** | Requires all conditions to evaluate to TRUE. | Plus, also, as well as |
| **IF / THEN / ELSE** | Required syntax for conditional branching. | When, in case, otherwise |
| **NOT** | Reverses the Boolean state of the following condition. | Doesn't, isn't, non- |
| **OR** | Requires at least one condition to evaluate to TRUE. | Alternatively, either |