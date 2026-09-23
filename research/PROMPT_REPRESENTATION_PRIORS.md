# Prompt Representation Prior Art and Test Matrix

Status: PRIOR_RESEARCH_COMPLETE / MER_TEST_PENDING
Date: 2026-09-23

## Research question

For stable behavioral invariants injected into a ChatGPT Automation prompt, which representation yields the best combination of:
- instruction adherence;
- resistance to omission/misinterpretation;
- reasoning/task quality;
- token/control overhead;
- editability and version-sync safety?

This is distinct from output-format enforcement. JSON Schema / Structured Outputs constrain generated outputs in API contexts; Mer is testing how the *input instruction representation* affects behavior inside the automation prompt.

## Prior art

### OpenAI prompt engineering guidance
Sources:
- https://developers.openai.com/api/docs/guides/prompt-engineering
- https://developers.openai.com/api/docs/guides/reasoning-best-practices
- https://developers.openai.com/api/docs/guides/latest-model

Observed guidance:
- use high-authority message/instruction channels for behavioral instructions;
- keep reasoning-model prompts simple and direct;
- use delimiters, Markdown headings, and XML where they clarify logical boundaries;
- current model guidance recommends Markdown as a starting delimiter and says XML can also perform well;
- build evals because optimal prompt details may differ across models/snapshots.

Transferable:
- representation should be evaluated empirically on the actual model/runtime;
- semantic clarity and section boundaries are legitimate variables;
- concise Markdown is a strong baseline, not a proven universal winner.

Not transferable:
- API developer-message hierarchy is not identical to ChatGPT Automation prompt injection;
- Structured Outputs guarantees concern generated output schemas, not behavioral instruction precedence in this automation runtime.

### Anthropic prompting guidance
Source:
- https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-templates-and-variables

Observed guidance:
- consistent XML tags can reduce ambiguity when instructions, context, examples, and variable inputs are mixed.

Transferable:
- explicit section boundaries may reduce cross-section confusion.

Not transferable:
- Claude-specific training/preferences do not establish the best format for GPT/ChatGPT Automations.

### Lee & Lee (2025), The Impact of Prompt Formats on the Robustness of LLMs
DOI: 10.32431/kace.2025.28.12.001
Source:
- https://journal.kace.re.kr/_PR/view/?aidx=48056&bidx=4325

Finding:
- no universally best format across Markdown, JSON, YAML, XML;
- high-capability models were less format-sensitive than some smaller models;
- language/task/model interactions matter.

Translation:
- Mer should not promote JSON/XML/Markdown by reputation; use controlled model/runtime-specific evals.

### Tam et al. (EMNLP Industry 2024), Let Me Speak Freely?
Source:
- https://aclanthology.org/2024.emnlp-industry.91/

Finding:
- structured output restrictions can degrade reasoning performance; stricter format constraints can impose a capability cost.

Translation:
- do not assume more syntactic rigidity is free;
- measure downstream useful-work quality as well as rule adherence.

Non-transferable:
- this primarily studies output restrictions, not stable input-rule representation.

### Lee, D'Antoni & Berg-Kirkpatrick (2026), The Format Tax
Source:
- https://arxiv.org/abs/2604.03616

Finding:
- format-requesting instructions can themselves contribute to performance degradation;
- decoupling reasoning from formatting recovers performance in tested settings;
- recent closed models showed less tax than many open models.

Translation:
- syntactic ceremony may consume capacity/tokens without improving compliance;
- include a task-quality and token-cost metric.

### Lepagnol et al. (LREC 2026), Format Matters
DOI: 10.63317/3osjjdr778fh
Source:
- https://aclanthology.org/2026.lrec-1.593/

Finding:
- JSON/XML/key-value output-format performance varies materially by model/dataset;
- small development-slice selection can identify a good format without assuming a universal winner.

Translation:
- use a compact discriminating benchmark first, then promote the best candidates to longer relay trials.

### Berkeley Function-Calling Leaderboard V4 prompt variation
Source:
- https://gorilla.cs.berkeley.edu/blogs/17_bfcl_v4_prompt_variation.html

Finding:
- function-document representation affected tool-use performance; JSON was generally strongest in the tested function-document setting.

Translation:
- machine-like structured representations can help when the task itself is schema/tool oriented.

Non-transferable:
- function specifications are not identical to behavioral invariants.

## Candidate representations

Keep semantic content, ordering, authority channel, model, task, scheduler policy, and dynamic GitHub state fixed. Change only representation.

F1_MARKDOWN_DIRECT
- concise headings + bullets + imperative natural language.
- Current strong baseline based on OpenAI guidance.

F2_FLAT_KV_DSL
- terse hardcoded key/value and enum-like rules, e.g.:
  CONTINUATION_IDENTITY=SAME_AUTOMATION
  SCHEDULER_MODE=RECURRING_RRULE_HOURLY
  STATE_AUTHORITY=status/program.json
  EARLY_STOP_POLICY=ONLY_TERMINAL_OR_NO_SAFE_ADMISSIBLE_WORK
- minimal prose.

F3_JSON_INVARIANTS
- same rules as a JSON object with explicit keys/enums/booleans.
- no claim that JSON itself has higher instruction authority.

F4_XML_SECTIONS
- same semantics encoded with descriptive XML tags/attributes.
- tests explicit boundary structure.

F5_HYBRID_MINIMAL
- key constants/identifiers as flat key-value fields;
- behavioral semantics as concise Markdown imperatives;
- dynamic references as paths/IDs.
- candidate motivated by separating machine-like constants from natural-language behavior.

Do not add YAML initially unless the first four/five candidates are inconclusive; avoid format proliferation.

## Metrics

Primary:
- invariant_adherence_rate on adversarial/ambiguous fixtures;
- critical_violation_count;
- successful full-task completion rate.

Secondary:
- bootstrap/control tokens or character count proxy;
- interpretation/repair turns;
- useful_work_sec / wake;
- control_overhead_sec;
- prompt-sync/edit errors;
- omission rate when one rule competes with task pressure.

Safety floor:
- zero unauthorized repo writes;
- zero replacement-automation creation under same-continuation fixture;
- no inferred WAKE_OK from WRITE_OK/STATE_OK.

## Controlled fixtures

Use deterministic or near-deterministic fixtures that exercise the invariant without changing actual production state when possible:
1. authority conflict fixture;
2. tempting early-stop fixture;
3. scheduler identity/replacement temptation fixture;
4. evidence-layer confusion fixture;
5. dynamic-state-vs-stable-rule fixture;
6. prompt-version mismatch fixture.

Each format gets the same fixtures and semantic content.

## Hypotheses

H-FORMAT-0 (null):
Representation does not materially change adherence/task quality on the current high-capability model; choose the cheapest/readable format.

H-FORMAT-1:
Concise Markdown/direct natural language provides the best overall adherence-to-overhead ratio for behavioral invariants.

H-FORMAT-2:
Flat key/value DSL improves exact constant/enum compliance but may weaken nuanced behavioral rules.

H-FORMAT-3:
JSON improves schema-like constant extraction but does not necessarily improve behavioral obedience, and may add token/syntax cost.

H-FORMAT-4:
XML improves section-boundary discrimination when mixed instruction/context is the dominant failure mode, at additional token cost.

H-FORMAT-5:
Hybrid minimal representation outperforms homogeneous formats by using machine-like syntax for constants and direct natural language for behavior.

## Test strategy

Phase A: micro-eval
- repeated fixtures, same semantics, representation only variable;
- randomize/counterbalance candidate order where possible;
- collect adherence + quality + overhead.

Phase B: relay shadow/canary
- top two candidates only;
- run equivalent non-destructive relay research steps;
- compare real wake behavior and control overhead.

Phase C: production candidate
- promote only if difference is reproducible and operationally material;
- if statistically/operationally tied, choose simpler/shorter/readable candidate.

## Rejection rule
A format is rejected if it:
- increases critical invariant violations;
- materially degrades task quality/useful work;
- adds overhead without measurable adherence benefit;
- causes more rollout/sync errors.

No format is promoted because an official guide recommends it; official guidance establishes a prior, not Mer truth.


### Native protocol syntax vs invented prompt DSL

#### RFC 5545 iCalendar recurrence syntax
Source:
- https://www.rfc-editor.org/rfc/rfc5545.html

Relevant fact:
- `RRULE:FREQ=HOURLY` is not merely a key/value-looking prompt convention; it is canonical iCalendar recurrence syntax defined by RFC 5545.
- `FREQ` is a required recurrence rule part and `HOURLY` is a standardized value.

Hypothesis implication:
- when the downstream scheduler/tool itself consumes VEVENT/RRULE syntax, embedding the exact native fragment in a stable invariant may reduce semantic translation between prompt intent and tool payload compared with an invented alias such as `SCHEDULER_MODE=HOURLY`.

Counter-hypothesis:
- native syntax may be too low-level to express behavioral conditions such as when to update, when not to update, ownership, or recovery logic;
- adding protocol syntax may create false confidence while not improving behavioral adherence;
- model/tool interfaces can still require a natural-language decision before emitting the native payload.

Therefore test these separately:
- N1_NATURAL_LANGUAGE: “preserve an hourly recurring RRULE”
- N2_INVENTED_DSL: `RECURRENCE=HOURLY`
- N3_NATIVE_FRAGMENT: `RRULE:FREQ=HOURLY`
- N4_HYBRID_NATIVE: concise behavioral sentence + exact native fragment

Keep actual scheduler behavior identical; vary only lexical/representation form.

Primary metrics:
- exact recurrence preservation;
- accidental one-shot conversion;
- wrong DTSTART/recurrence mutation;
- scheduler payload repair count;
- control-token/character cost;
- task-quality/utilization side effects.

Promotion rule:
- prefer native syntax only if it reproducibly reduces scheduler representation errors or control overhead without increasing behavioral mistakes.
