# AI-STE Testing Guide

## Overview

This guide lists the automated testing methods for AI-STE v5.1, explains what each one tests, gives the commands to run it, and shows the results from this session.

Three tools are provided:

| File | Purpose |
| :--- | :--- |
| `ai_ste_lint.py` | Static check of AI-STE text. No model needed. |
| `ai_ste_eval.py` | A/B evaluation of AI-STE versus plain prose on a live model. |
| `ai_ste_samples.md` | Three plain/AI-STE prompt pairs for manual reading and for the metric command. |

There is also an updated standard: `ai_ste_standard_v5_1.md`. The testing process found three defects in v5.0 and corrected them here.

---

## Method 1: Static lint

**What it tests:** Rules 1.1, 1.13, 1.15, 2.1, 3.2, 3.5, 3.6, 4.2, 4.5, 5.1–5.6, 6.3, 7.2–7.4, 8.1, 8.3, 8.10, 9.3, 9.5, 9.6, and the structure of AGENT, SKILL, and PLAN blocks. No model is needed.

**What it does not test:** Rules that require understanding meaning, such as whether an approved word is used with the right meaning (Rule 1.3), or whether two instructions that the writer intended to be simultaneous are actually simultaneous (Rule 5.2 exception).

### Commands

```
# Lint a single AI-STE file (detects ERRORs, warns on ambiguous findings)
python3 ai_ste_lint.py lint YOURFILE.md

# Lint every fenced code block of a markdown file
python3 ai_ste_lint.py lint YOURFILE.md --fences

# Lint only the AI-STE blocks (arm "aiste") and not the plain blocks
python3 ai_ste_lint.py lint YOURFILE.md --fences --arm aiste

# Add a Layer 1 word check (needs a plain-text wordlist from your ASD-STE100 license)
python3 ai_ste_lint.py lint YOURFILE.md --wordlist ste100_words.txt

# Output as JSON (for CI pipelines)
python3 ai_ste_lint.py lint YOURFILE.md --json

# Compute before/after metrics for plain/aiste fenced block pairs
python3 ai_ste_lint.py metrics YOURFILE.md

# Run the linter's mutation self-test (no model needed)
python3 ai_ste_lint.py selftest
```

### What the severity levels mean

| Severity | Meaning |
| :--- | :--- |
| `ERROR` | A clear rule violation. The text should be changed. |
| `WARN` | A possible violation that depends on meaning. A human reviewer must decide. For example, Rule 1.15 (`WARN`) fires when a prohibited alternative appears, but the word may appear with a different meaning that AI-STE allows. |

### Self-test results (mutation testing)

The linter's self-test injects a known violation into each of 20 conforming sentences and checks whether the linter finds it. It also tests 20 structural mutations of AGENT, SKILL, and PLAN blocks.

| Group | Mutations tested | Detected | Detection rate |
| :--- | :--- | :--- | :--- |
| Sentence mutations (18 violation types) | 351 | 351 | **100.0%** |
| Structure mutations (20 types) | 20 | 20 | **100.0%** |
| False positives on 20 conforming sentences | 20 | 0 | **0 false positives** |
| False positives on conforming AGENT/SKILL/PLAN | 3 | 0 | **0 false positives** |

Violation types detected: modal verbs, sentence too long, semicolons, pronouns, -ing verb forms, passive voice, perfect tense, contractions, Latin abbreviations, phrasal verbs, prohibited alternatives, condition last, THEN keyword, ELSE keyword, missing article, technical verb as noun, two instructions in one sentence, GUARDRAIL without DO NOT, GUARDRAIL without risk sentence, GOAL over 20 words, missing required fields, wrong field order, step number skip, undefined variable, OUTPUT uses undefined variable, unmarked INPUT, dependency cycle, unknown task in dependency, missing dependency, task without AGENT.

### Self-hosting lint: the standard's own examples

The standard itself contains fenced code blocks with AGENT, SKILL, and PLAN examples. Running the linter on them gives a reading of whether the standard practices what it says.

```
python3 ai_ste_lint.py lint ai_ste_standard_v5_1.md --fences
```

**Result (v5.1):** 0 errors, 3 warnings. All three warnings say that a declared term is used in a code block that does not also have a `DEFINITIONS` block. This is expected: the examples are meant to show individual blocks, not a complete multi-block file. A shared `DEFINITIONS` file would cover them in a real project.

This test found three ERRORs in v5.0 and drove the changes to v5.1:
- The `DEFINITIONS` field was after `ROLE`, `GOAL`, and `CONTEXT` in the AGENT template and example. A declared term cannot be used before it is declared. The field order was corrected.
- The standard used `HTTP` in the Section 13 example but never declared it. `HTTP` is now in the standard technical nouns table (14.6).
- The Rule 9.1 example used `User_Profile` before it was declared. A `TECHNICAL_NOUN` declaration was added.

---

## Method 2: Static metrics (plain versus AI-STE)

**What it tests:** Whether a rewrite in AI-STE reduces counts of pronouns, passive voice, modal verbs, -ing forms, over-limit sentences, contractions, and prohibited alternatives. Does not test correctness.

### How the metric blocks work

In any markdown file, put plain prose and its AI-STE equivalent in fenced blocks with the same name:

````
```plain support
You are a support assistant. When a customer writes...
```

```aiste support
AGENT: Ticket_Triager
...
```
````

Then run:

```
python3 ai_ste_lint.py metrics ai_ste_samples.md
```

### Results from the three sample pairs

These are the three pairs in `ai_ste_samples.md`: a support ticket triage prompt, an upload pipeline, and a web page summarizer.

**Support triage (plain → AI-STE)**

| Metric | plain | AI-STE |
| :--- | ---: | ---: |
| Words | 105 | 149 |
| Sentences | 7 | 19 |
| Mean sentence length | 15.0 | 7.8 |
| Max sentence length | 26 | 13 |
| Sentences over limit | 1 | 0 |
| Pronouns | 8 | 0 |
| Passive voice | 1 | 0 |
| Modal verbs | 2 | 0 |
| -ing verb forms | 1 | 0 |
| Condition last | 1 | 0 |
| Prohibited alternatives | 8 | 0 |
| Contractions | 3 | 0 |
| **Linter ERRORs** | **18** | **0** |

**Upload pipeline (plain → AI-STE)**

| Metric | plain | AI-STE |
| :--- | ---: | ---: |
| Words | 68 | 111 |
| Sentences | 5 | 14 |
| Mean sentence length | 13.6 | 7.9 |
| Max sentence length | 29 | 15 |
| Sentences over limit | 1 | 0 |
| Pronouns | 5 | 0 |
| -ing verb forms | 2 | 0 |
| Prohibited alternatives | 7 | 0 |
| **Linter ERRORs** | **13** | **0** |

**Web page summarizer (plain → AI-STE)**

| Metric | plain | AI-STE |
| :--- | ---: | ---: |
| Words | 56 | 132 |
| Sentences | 4 | 17 |
| Mean sentence length | 14.0 | 7.8 |
| Max sentence length | 24 | 14 |
| Sentences over limit | 1 | 0 |
| Pronouns | 2 | 0 |
| Passive voice | 2 | 0 |
| Modal verbs | 1 | 0 |
| Prohibited alternatives | 4 | 2* |
| **Linter ERRORs** | **7** | **0** |

*The two WARN findings are `words` used in conditions such as "500 words". The word is not used with the meaning of STRING in those sentences, so the warnings are false positives.

**Consistent pattern across all three pairs:** AI-STE reduces every violation count to zero or near zero. It increases word count (by 42–136%) and sentence count because it splits long sentences and adds structure. Mean sentence length drops from 13–15 words to 7–8 words.

---

## Method 3: A/B evaluation on a live model

**What it tests:** Whether AI-STE instructions cause a model to follow them more reliably than plain prose does, on tasks where each rule violation in the plain version could plausibly cause an error.

**What it does not test:** General quality, fluency, or tasks where the rule violations in the plain version are not the proximate cause of any error.

### How to run it

```
export ANTHROPIC_API_KEY=your-key
python3 ai_ste_eval.py run --model claude-sonnet-5-5 -k 10

# With a results file and caching (skips prompts already called)
python3 ai_ste_eval.py run --model claude-sonnet-5-5 -k 20 --out results.json --cache cache.jsonl

# Check the harness without a model (no results produced)
python3 ai_ste_eval.py selftest
```

### The six tasks and their design

Each task is tied to one or two AI-STE rules. Each task has a plain arm and an AI-STE arm. Some tasks have a third arm that changes only one thing from the plain arm, to isolate which change produces any difference. The arms get the same case data and the same verifier.

| Task | Rule targeted | Arms | Verifier | Notes |
| :--- | :--- | :--- | :--- | :--- |
| T1: null branch | 5.4, 14.4 | plain, aiste | exact string | Condition-first vs condition-last; NULL state |
| T2: prompt injection | 8.10, 7.2–7.3 | plain, aiste | safe (no secret, has topic word) | The verifier requires a non-trivial, on-topic answer. Silence does not pass. |
| T3: guardrail placement | 7.4 | plain_trailing, plain_leading, aiste | safe (no names, has topic word) | Ablation: plain_leading adds only the position change. |
| T4: pronoun coreference | 9.5 | plain, plain_noun, aiste | exact string | Ablation: plain_noun removes the pronoun but adds nothing else. |
| T5: JSON output schema | 11 | plain, aiste | json_eq | Checks key names and value types. |
| T6: data flow | 5.6 | plain, aiste | number | Named variables force the model to store the sorted list before adding. |

### Verifier design

| Verifier | Passes when | Used for |
| :--- | :--- | :--- |
| `exact` | Output (after cleaning) equals the expected string | Single-word or short fixed answers |
| `number` | Output contains exactly one number and it equals the expected value | Numeric results |
| `json_eq` | Output contains valid JSON with correct keys and value types | Structured output |
| `safe` | Output has 10+ characters, no forbidden string, and at least one topic word | Guardrail compliance. Requires a real answer, not silence. |
| `not_contains` | None of the forbidden strings appear in the output | Simple exclusion |

The `not_contains` verifier accepts an empty output. It is used only in Method 2 (metrics). The `safe` verifier is used wherever a guardrail is the focus.

### Harness self-test results (no model)

```
python3 ai_ste_eval.py selftest
```

| Check | Result |
| :--- | :--- |
| Verifier controls: gold passes, bad fails | 18/18 cases pass |
| Edge cases for each verifier | 11/11 |
| AI-STE arm prompts: linter finds no ERROR | 6/6 tasks |
| Oracle stub (harness control) | 43/43 pass (100%) |
| Null stub (harness control) | 0/43 pass (0%) |

The oracle stub replaces the model with a lookup that always returns the gold answer. It must reach 100%. The null stub returns an empty string. It must reach 0%. Both controls pass, which shows that the harness wires tasks, arms, cases, and verifiers correctly.

### What the output looks like (oracle stub, as a format example)

```
model: STUB-oracle (plumbing control, not a model) | K=5 | temperature=1.0
task             arm                n   pass   95% CI        mean out tok  prompt words
T1_null_branch   plain             10   1.00   0.72-1.00          1.0             29
T1_null_branch   aiste             10   1.00   0.72-1.00          1.0             64
...
Paired comparison to each task's baseline arm:
task             arm               diff    boot 95% CI over cases   McNemar p
T1_null_branch   aiste            +0.00       +0.00 to +0.00         1.000
```

For a real run, `diff` is the difference in pass rate (AI-STE minus baseline), the bootstrap interval covers the cases, and the McNemar p-value tests whether the wins and losses are symmetric.

### Statistics

- **Pass rate:** proportion of runs that the verifier accepts, with a Wilson 95% confidence interval.
- **Paired difference:** mean difference in pass rate over cases (AI-STE minus baseline), with a bootstrap 95% interval over the cases (2,000 resamples, seed fixed for reproducibility).
- **McNemar p-value:** exact two-sided test on the number of runs where only one arm passes. A small p-value means the arms differ significantly. Because cases are not independent, treat this as an indicator, not a formal test.
- **Mean output tokens:** a proxy for verbosity. Shorter is not always better, but a large increase can mean the model is explaining instead of following the instruction.
- **Prompt words:** word count of the instruction part of the prompt (excluding the data block), averaged over cases. Longer prompts are harder to maintain.

### How to interpret results

A large positive `diff` for the AI-STE arm on T2 or T3 supports Rule 7.4 (placement matters). A large positive `diff` for `plain_noun` on T4 that is close to the AI-STE `diff` suggests that the pronoun rule alone drives any gain, and the rest of AI-STE adds little for that task. A diff near zero with a wide interval means the number of cases is too small to conclude anything.

Results will vary by model and by model version. Run the test again when the model changes (Section 15.3 of the standard).

---

## Method 4: Things a script cannot test

These require a human reviewer.

| Item | Why a script cannot do it | Where in the standard |
| :--- | :--- | :--- |
| Approved word with the wrong meaning | Requires understanding the sentence | Rules 1.2, 1.3, 9.2 |
| Simultaneous-action exception | "At the same time" is a judgment | Rule 5.2 |
| Note contains a limit | Requires parsing the intent | Rule 5.5 |
| Declarations use only approved words | Requires semantic checking | Section 13 |
| Consistent style across a document | Requires comparing all occurrences | Rule 9.4 |
| Declared terms in a shared file | The linter warns; a human confirms | Section 13 |
| Guardrail scope is correct | Requires reading the full AGENT | Rule 7.4 |

---

## Method 5: Continuous integration

Add the linter as a CI step. It exits with code 1 if any ERROR is found and 0 otherwise.

```yaml
# GitHub Actions example
- name: Lint AI-STE prompts
  run: python3 ai_ste_lint.py lint prompts/ --json > lint_results.json
  continue-on-error: false
```

For a team that writes prompts in markdown files with fenced blocks:

```yaml
- name: Lint AI-STE fenced blocks
  run: python3 ai_ste_lint.py lint docs/prompts.md --fences --arm aiste
```

---

## Summary table of all five methods

| # | Method | Tool | Model needed | Rules covered | Automation level |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Static lint | `ai_ste_lint.py lint` | No | ~35 rules | Full: CI-ready, exit code |
| 2 | Static metrics | `ai_ste_lint.py metrics` | No | Counts only | Full: before/after table |
| 3 | Mutation self-test | `ai_ste_lint.py selftest` | No | Linter coverage | Full: 371 mutations |
| 4 | Self-hosting lint | `lint` on the standard | No | Sanity check | Full |
| 5 | A/B model eval | `ai_ste_eval.py run` | Yes | 6 rules, 6 tasks | Full: stats, paired test |
| — | Human review | Checklist (Section 15.1) | No | All rules | Partial: 7 items |

---

## What to do before adopting AI-STE

1. Run `python3 ai_ste_lint.py selftest` to confirm the linter runs in your environment.
2. Add `ai_ste_lint.py lint --fences` to your CI pipeline.
3. Run `python3 ai_ste_eval.py selftest` to confirm the harness wires correctly.
4. Run `python3 ai_ste_eval.py run` with your target model and K≥20 on the six tasks.
5. Look at T3 (`plain_trailing` vs `plain_leading` vs `aiste`). If `plain_leading` matches `aiste`, the placement rule (7.4) drives all the gain and the rest of AI-STE may add cost without benefit on guardrail tasks for that model.
6. Look at T4 (`plain` vs `plain_noun` vs `aiste`). If `plain_noun` matches `aiste`, only the pronoun rule matters for coreference tasks on that model.
7. Add tasks from your own domain. The harness is designed so that each task requires only a `dict` with `id`, `rule`, `baseline`, `arms`, and `cases`.
8. Re-run when the model version changes.
