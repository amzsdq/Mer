# O8 official scheduled-task documentation gap — 2026-09-24

Purpose: bounded authoritative-reference check for the same-canonical dispatch-topology question.

Source checked: OpenAI Help Center scheduled-tasks / ChatGPT Work documentation surfaced on 2026-09-24. The official material documents recurring scheduled work, eligible cadence, exact scheduling, task lifecycle and sharing, but the searched material did not establish whether a recurrence of the SAME scheduled task may execute concurrently with a still-running prior invocation or is serialized/deferred.

Decision: do not infer overlap or serialization from product documentation. Treat provider concurrency semantics as an empirical Mer question. B03's exact target observation remains the discriminator.

Counterevidence discipline: absence of a concurrency statement in the bounded documentation search is not evidence that serialization occurs. It only prevents promotion of either topology from documentation alone.

Relevant official source family: OpenAI Help Center, Scheduled Tasks in ChatGPT / ChatGPT Work and Codex. This note records the bounded search result, not a claim of exhaustive documentation coverage.
