# AI-STE: Simplified Technical English for AI Orchestration (v0.2.0)

## Introduction

The Artificial Intelligence Simplified Technical English (AI-STE) standard is adapted from ASD-STE100. It is designed exclusively for writing prompts, system instructions, and orchestration logic for Large Language Models (LLMs) and autonomous agents. The goal is to enforce deterministic execution, eliminate semantic ambiguity, and map human language directly to computational logic. Version 2.0 incorporates advanced strictures to prevent hallucination caused by pronoun resolution, gerunds, and trailing constraints.

## Part 1: Core Writing Rules

These rules govern the syntax and structure used to write instructions for any AI PLAN, SKILL, or AGENT.

**1. Sentence Structure and Length**

* **Length limits:** Restrict instructional sentences (prompts) to a maximum of 20 words. Restrict descriptive sentences (system CONTEXT) to a maximum of 25 words.
* **Single instructions:** Write only one instruction per sentence.
  * *Unapproved:* "Retrieve the user data and format it as JSON before you validate the output."
  * *Approved:* "RETRIEVE the DATA. FORMAT the DATA to the JSON SCHEMA. VALIDATE the OUTPUT."
* **Conditional logic:** Use standard IF/THEN/ELSE formatting for branching instructions. Place the condition before the action.
  * *Approved:* "IF the DATA is NULL, THEN HALT the SKILL."

**2. Verbs and Voice**

* **Active voice only:** Do not use the passive voice. The subject (the AGENT or SKILL) must perform the action.
  * *Unapproved:* "The API must be called by the agent."
  * *Approved:* "The AGENT EXECUTES the TOOL."
* **Imperative mood:** Use the imperative mood (command form) for all direct instructions.
  * *Approved:* "PARSE the JSON STRING."
* **The Ban on "-ing" Words:** Do not use present participles or gerunds (words ending in "-ing"). These blur the line between nouns and verbs.
  * *Unapproved:* "Analyzing the DATA..." or "For testing the TOOL..."
  * *Approved:* "ANALYZE the DATA." or "To TEST the TOOL..."
* **No phrasal verbs:** Do not use verbs combined with prepositions (e.g., "find out", "look up"). Use single, approved verbs.

**3. Nouns and Pronouns**

* **Pronoun Elimination:** Do not use pronouns (it, they, this, these, them). Explicitly repeat the noun or variable in every sentence to prevent resolution hallucinations.
  * *Unapproved:* "PARSE the JSON STRING. VALIDATE it against the SCHEMA."
  * *Approved:* "PARSE the JSON STRING. VALIDATE the JSON STRING against the SCHEMA."
* **Noun Cluster Restrictions:** Do not string together more than three nouns. Use prepositions to separate complex descriptions.
  * *Unapproved:* "User account database connection string."
  * *Approved:* "Connection STRING for the user account database."

**4. Formatting and Guardrails**

* **Guardrails (Warnings/Cautions):** All negative constraints or safety GUARDRAILs must precede the ACTION instructions they apply to, never after. AI models execute sequentially; constraints placed at the end are often ignored.
  * *Unapproved:* "FORMAT the DATA to JSON. DO NOT include nested objects."
  * *Approved:* "GUARDRAIL: DO NOT include nested objects. FORMAT the DATA to JSON."
* **Capitalization:** Write all approved dictionary words in ALL CAPS when used in system prompts.
* **Variables:** Enclose all dynamic variables or parameters in brackets `[like_this]` or standard code block backticks.
* **Lists:** Use vertical numbered lists for sequential processes. Use bulleted lists for non-sequential parameters.

## Part 2: Structural Frameworks

To maintain consistency, all AI orchestrations must use the following structural definitions. 

* **AGENT:** An autonomous entity. Must be defined by exactly three fields:
  1. `ROLE`: A two-word noun phrase (e.g., "Data Validator").
  2. `GOAL`: A single sentence defining the ultimate objective.
  3. `SKILLSET`: A bulleted list of approved SKILLs the AGENT can EXECUTE.
* **SKILL:** A deterministic capability. Must be defined by:
  1. `TRIGGER`: The specific event or INPUT that starts the SKILL.
  2. `ACTION`: The sequential TASKs the SKILL EXECUTES.
  3. `OUTPUT`: The exact SCHEMA of the returned DATA.
* **PLAN:** A routing document. Must be defined by:
  1. `WORKFLOW`: A numbered list of TASKs.
  2. `DEPENDENCY`: A list of TASKs that must complete before others start.

## Part 3: Extensibility and Custom Domains

The core AI-STE dictionary is fixed. However, AI systems interact with infinite domain-specific environments. Users can define custom vocabulary using the following protocol:

* **DOMAIN_NOUNS:** You can define Technical Names (e.g., *Stripe_API*, *User_Profile*) within the CONTEXT of an AGENT. 
* **API_VERBS:** You can define Technical Verbs (e.g., *POST*, *ENCRYPT*) if a specific TOOL requires actions outside the core dictionary.
* **Rule of definition:** All custom `DOMAIN_NOUNS` and `API_VERBS` must be explicitly defined in the AGENT CONTEXT, must have a single strict meaning, and must not duplicate the meaning of core AI-STE vocabulary.

## Part 4: The Core AI-STE Approved Dictionary

This dictionary enforces the "one word, one meaning, one part of speech" rule.

### 1. Verbs (Actions)

| Approved Word | Definition for AI Context | Unapproved Synonyms | 
| ----- | ----- | ----- | 
| **ANALYZE** | To process DATA to identify patterns, sentiment, or specific entities. | Think about, evaluate, assess | 
| **EXECUTE** | To run a script, TOOL, or defined SKILL. | Run, do, perform, start | 
| **EXTRACT** | To pull specific, targeted DATA from a larger CONTEXT. | Pull, grab, find, scrape | 
| **FORMAT** | To change the structural presentation of DATA. | Change, convert, make into | 
| **GENERATE** | To create novel text, code, or conceptual OUTPUT based on a PROMPT. | Make, write, produce, draft | 
| **HALT** | To stop an operation immediately. | Stop, quit, end, kill, abort | 
| **PARSE** | To read structured DATA and EXTRACT components. | Read, process, interpret | 
| **RETRIEVE** | To fetch DATA from a database, TOOL, or search engine. | Get, fetch, download, look up | 
| **ROUTE** | To send DATA or a TASK to a specific AGENT or SKILL. | Send, pass, hand off, give | 
| **VALIDATE** | To verify that an OUTPUT meets a REQUIRED SCHEMA or PARAMETER. | Check, make sure, confirm | 

### 2. Nouns (Data and Objects)

| Approved Word | Definition for AI Context | Unapproved Synonyms | 
| ----- | ----- | ----- | 
| **CONTEXT** | Background system instructions or retrieved DATA framing a generation. | Background, situation, setting | 
| **DATA** | Raw text, numbers, JSON, or code processed by an AGENT. | Information, info, details | 
| **ERROR** | A state where a SKILL, TOOL, or VALIDATE action fails. | Mistake, bug, glitch, fault | 
| **MEMORY** | The stored historical record of previous turns in a session. | History, past, recall, transcript | 
| **PARAMETER** | A specific, defined variable REQUIRED to EXECUTE a SKILL. | Setting, argument, option | 
| **PROMPT** | The formulated STRING sent to a Large Language Model. | Question, query, command | 
| **SCHEMA** | The defined structure that DATA must adhere to (e.g., JSON). | Format (as a noun), layout, template | 
| **STRING** | A sequence of alphanumeric characters treated as text. | Text, words, sentence | 
| **TOOL** | An external script, API, or search function called by a SKILL. | Plugin, extension, utility, app | 

### 3. Nouns (Structural Entities)

*These nouns are strictly reserved for defining the architecture of the AI system.*

* **AGENT**: An autonomous entity executing a PLAN.
* **DEPENDENCY**: A TASK that must finish before another begins.
* **GOAL**: The ultimate objective of an AGENT.
* **GUARDRAIL**: A negative constraint or safety instruction.
* **INPUT**: The DATA provided to a SKILL or AGENT.
* **OUTPUT**: The DATA returned by a SKILL or AGENT.
* **PLAN**: A routing document containing a WORKFLOW.
* **ROLE**: The persona assigned to an AGENT.
* **SKILL**: A defined, deterministic capability.
* **SKILLSET**: The collection of SKILLs an AGENT possesses.
* **TASK**: A single unit of work within a WORKFLOW.
* **TRIGGER**: The event that initiates a SKILL.
* **WORKFLOW**: A sequential list of TASKs.

### 4. Adjectives (States and Conditions)

| Approved Word | Definition for AI Context | Unapproved Synonyms | 
| ----- | ----- | ----- | 
| **FALSE** | The negative Boolean state. | Wrong, incorrect, no | 
| **NULL** | The state of containing no DATA or value. | Empty, blank, missing | 
| **OPTIONAL** | Not REQUIRED for the execution of a TASK or SKILL. | Unnecessary, voluntary | 
| **REQUIRED** | Mandatory for the execution of a TASK or SKILL. | Needed, mandatory, crucial | 
| **TRUE** | The positive Boolean state. | Right, correct, yes | 
| **VALID** | Conforming to the expected SCHEMA or PARAMETER. | Good, clean, proper | 

### 5. Conjunctions and Operators (Logic Gates)

| Approved Word | Definition for AI Context | Unapproved Synonyms | 
| ----- | ----- | ----- | 
| **AND** | Requires all connected conditions to evaluate to TRUE. | Plus, also, as well as | 
| **IF / THEN / ELSE** | The required syntax for conditional branching. | When, in case, otherwise | 
| **NOT** | Reverses the Boolean state of the following condition. | Doesn't, isn't, non- | 
| **OR** | Requires at least one connected condition to evaluate to TRUE. | Alternatively, either | 