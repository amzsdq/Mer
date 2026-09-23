# Reference Base

Status: SEED_REFERENCE_SET
Last refreshed: 2026-09-23

This file is not authority for Mer runtime behavior. It is a source of hypotheses, invariants, failure modes, and adverse-test ideas.

## Internal empirical sources
- amzsdq/tEST
  - relay lead-time tests
  - WRITE_OK / STATE_OK / WAKE_OK / WORK_OK separation
  - recurring RRULE self-shift / missed-wake / fallback work
- amzsdq/workwork
  - immediate prearm
  - runtime-boundary / productive-window / completion-envelope separation
  - close reserve estimation
  - overlap / shadow-successor handoff

## Authoritative implementation references

### Temporal
Reference: https://docs.temporal.io/
Relevant ideas:
- durable execution and resume-after-failure
- history-backed continuation
- worker/task-queue performance tuning
Translation candidates:
- durable state should be reconstructable independently of one transient invocation
- latency optimization must not erase recovery semantics

### Kubernetes Lease / leader election
References:
- https://kubernetes.io/docs/concepts/architecture/leases/
- https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/
Relevant ideas:
- one active holder
- renewable lease / expiry
- holder identity
- optimistic concurrency / version fencing
Translation candidates:
- overlap experiments require explicit single-authority fencing
- liveness and authority are separate concerns

### AWS Step Functions
Reference:
- https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html
Relevant ideas:
- explicit retry vs catch/fallback paths
- retry parameters are policy, not proof of success
- stage-aware recovery
Translation candidates:
- classify failure before retrying
- retries need bounded rules and alternate recovery paths

### Cloudflare Durable Objects Alarms
Reference:
- https://developers.cloudflare.com/durable-objects/api/alarms/
Relevant ideas:
- future wake as durable scheduled state
- at-least-once alarm delivery
- automatic retry with bounded backoff
- one current alarm per object, reschedule for next event
Translation candidates:
- scheduler acceptance and later execution are distinct evidence
- recurring/fallback wake paths must be tested under missed/failure conditions

## Academic references

### Chandra & Toueg (1996)
Unreliable failure detectors for reliable distributed systems.
Journal of the ACM 43:225-267.
Relevant ideas:
- failure detection is characterized by completeness and accuracy rather than perfect knowledge
- false suspicions and delayed detection are fundamental design concerns
Translation candidates:
- Mer watchdog/liveness logic should explicitly measure false-positive and missed-detection behavior
- timeout/overdue is evidence, not automatically proof of death

## Use rule
For each new hypothesis:
1. cite the relevant entries from this file or add new references;
2. state what property is transferable;
3. state what assumption is not transferable to ChatGPT Automations;
4. define the Mer experiment that would falsify the translated hypothesis.


## Prompt representation and format references

### OpenAI prompt engineering / reasoning guidance
References:
- https://developers.openai.com/api/docs/guides/prompt-engineering
- https://developers.openai.com/api/docs/guides/reasoning-best-practices
- https://developers.openai.com/api/docs/guides/latest-model
Relevant ideas:
- high-authority instructions and clear roles matter;
- reasoning models benefit from simple/direct prompts;
- Markdown headings, lists, XML tags and delimiters can clarify boundaries;
- current model guidance recommends starting with Markdown and evaluating alternatives;
- build evals rather than assuming prompt details generalize.
Translation candidates:
- F1_MARKDOWN_DIRECT is the baseline prior, not the automatic winner;
- evaluate section delimiters and syntax on the actual automation runtime.

### OpenAI Structured Outputs
References:
- https://developers.openai.com/api/docs/guides/structured-outputs
- https://openai.com/index/introducing-structured-outputs-in-the-api/
Relevant ideas:
- JSON Schema with constrained output mechanisms can guarantee schema adherence in supported API contexts.
Non-transferable:
- this does NOT establish that a JSON-encoded input prompt has stronger behavioral instruction force in ChatGPT Automations.
Translation:
- keep input-representation experiments separate from output-schema enforcement.

### Anthropic prompting best practices
Reference:
- https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-templates-and-variables
Relevant idea:
- XML tags can help separate instruction/context/constraint boundaries.
Non-transferable:
- Claude-specific preferences do not prove a GPT/Automation optimum.

### Lee & Lee (2025)
The Impact of Prompt Formats on the Robustness of LLMs.
DOI: 10.32431/kace.2025.28.12.001
Reference:
- https://journal.kace.re.kr/_PR/view/?aidx=48056&bidx=4325
Relevant finding:
- no universal best format across Markdown, JSON, YAML, XML; model/task/language interactions matter.
Translation:
- use model/runtime-specific controlled comparisons.

### Tam et al. (2024)
Let Me Speak Freely? A Study On The Impact Of Format Restrictions On Large Language Model Performance.
EMNLP Industry 2024.
Reference:
- https://aclanthology.org/2024.emnlp-industry.91/
Relevant finding:
- stricter structured-output constraints can degrade reasoning.
Translation:
- measure task quality, not only formatting/adherence.

### Lee, D'Antoni & Berg-Kirkpatrick (2026)
The Format Tax.
Reference:
- https://arxiv.org/abs/2604.03616
Relevant finding:
- format-requesting instructions can impose a reasoning/writing cost in some models/settings; decoupling can recover performance.
Translation:
- syntactic ceremony is a cost to measure, not a free reliability improvement.

### Lepagnol et al. (2026)
Format Matters: A Critical Evaluation of Output Formats for Prompting LLMs in SLU and NER.
DOI: 10.63317/3osjjdr778fh
Reference:
- https://aclanthology.org/2026.lrec-1.593/
Relevant finding:
- JSON/XML/key-value performance varied materially across model/task; a small development slice can select candidates efficiently.
Translation:
- micro-eval all representations, then canary only the top two.

### Berkeley Function-Calling Leaderboard V4 prompt variation
Reference:
- https://gorilla.cs.berkeley.edu/blogs/17_bfcl_v4_prompt_variation.html
Relevant finding:
- function-document representation affected tool-use accuracy; JSON performed strongly in that tested schema-oriented setting.
Translation:
- JSON/KV deserves testing for machine-like constants and tool-oriented instructions, but not automatic promotion for nuanced behavioral rules.
