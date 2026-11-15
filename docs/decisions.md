# Three engineering decisions

I built the product around three boundaries: durable workflow state, evidence and task context, and model-call accounting. Each addresses a failure that a successful API response alone would miss.

## 1. Use PostgreSQL as workflow authority and Redis as transport

**Problem.** Telegram can deliver an update again, workers can restart and a process can fail between committing a task and publishing a job. Retrying everything risks duplicate model charges or duplicate messages.

**Choice.** Store intake, turn ordering, jobs and an outbox transactionally. Publish committed jobs to Redis Streams. Require a durable turn claim before processing, then store the final response and per-part delivery receipts.

**Why this shape.** PostgreSQL can enforce tenant-scoped uniqueness and commit related state together. Redis consumer groups are useful for distributing and recovering work, but queue acknowledgement cannot describe whether an external model or Telegram request happened.

**Alternative.** A webhook that calls the model and sends the answer inline is simpler for an early demonstration. An in-memory queue is also easy to start. Neither preserves enough state to distinguish a fresh request from an uncertain external side effect after a crash.

**Tradeoff.** The outbox, processing claims and delivery parts require more schema and recovery code. PostgreSQL and Redis still do not form one transaction; idempotent handling closes that gap. For Telegram, ambiguous outcomes are retained rather than claiming exactly-once delivery.

**Verification focus.** Atomic intake, duplicate updates, rollback, tenant boundaries, stale workers and partial delivery. Some tests use SQLite fixtures; PostgreSQL-specific guarantees need their own database checks.

## 2. Model task progress and evidence validity explicitly

**Problem.** A fluent answer can use an old source, forget a corrected fact or imply that the user completed a step merely because the assistant described it. A previously generated answer can become unsuitable before it is sent or reused.

**Choice.** Retain a versioned task with facts, unresolved needs and source references. Distinguish proposed steps, delivery provenance and user-reported completion. Recheck source validity where material is reused, including the final sender path. Keep the guide available without invoking a model.

**Why this shape.** Conversation text is useful context, but it does not replace an explicit record of what is known, pending, delivered or withdrawn. The model can propose interpretation and wording; application code controls the corresponding state and admission rules.

**Alternative.** Replaying the whole chat and asking a model to infer progress reduces initial implementation work. It also makes corrections, stale evidence and completion claims harder to inspect and reproduce.

**Tradeoff.** Explicit contracts and source dependencies make schema evolution and invalidation more involved. They reduce particular failure modes; answer relevance and the strength of each claim still need semantic review alongside the mechanical checks.

**Verification focus.** Corrected facts, return after expiry, source withdrawal across multiple consumers, pending versus delivered state and answer review against the available evidence.

## 3. Admit and account for model work before sending it

**Problem.** A provider timeout does not prove a request was unbilled. Hidden retries or several small calls can also exceed the intended turn budget.

**Choice.** Reserve an upper-bound cost before dispatch, bind reservations to the intended role and model, and record usage and outcomes. Configured bounded profiles enforce spending and call limits; unknown attempts remain visible rather than releasing capacity as if nothing happened. The direct DeepSeek adapter uses explicit single-attempt transport.

**Why this shape.** The dispatch boundary is the last place to stop an unintended billable action. Recording both the reserved maximum and the observed result supports recovery without rewriting an uncertain event as a success or a free failure.

**Alternative.** Inspecting a provider dashboard afterwards is simpler, but cannot stop a turn before it exceeds a local policy or decide whether an interrupted request is safe to repeat. Unrestricted SDK retries also obscure the number of external attempts.

**Tradeoff.** Conservative reservations can stop work even when some budget probably remains. Accounting must be reconciled when actual usage becomes known. Policies are configurable: a separately selected tracked mode can remove monetary caps while retaining other accounting and execution controls.

**Verification focus.** Reservation identity, role/model binding, limits before HTTP dispatch, recorded usage and ambiguous outcomes. Low cost is not a substitute for answer quality; evaluation checks usefulness separately.

## What these decisions enable

The guide and AI assistance can share knowledge and context while keeping task progress, external side effects and spending explicit. This gives the product a foundation for ongoing source updates and new workflows without relying on a model to reconstruct operational state from chat history.

[Architecture](architecture.md) · [Quality and evaluation](evaluation.md) · [Project overview](../README.md)
