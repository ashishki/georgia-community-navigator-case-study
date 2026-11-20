# Quality and evaluation

The reference guide and personalized AI assistance are working product features. I evaluate them at three levels: workflow correctness, delivery and usefulness to the person asking for help.

## What I check

| Level | Main questions | Methods |
| --- | --- | --- |
| Workflow | Do duplicates, restarts and corrections preserve the right task state? | Unit and integration tests for intake, claims, memory, recovery and accounting. |
| Delivery and privacy | Is the response bound to the intended tenant, current state and valid sources? | Security and delivery checks, including part receipts, withdrawal and deletion boundaries. |
| Answer usefulness | Does the response address the actual question and give a relevant next step? | Recorded conversations, automated judging and manual review against source material. |

Ruff and strict mypy complement the behavioural checks. A mechanically correct workflow and a well-formed model response are useful foundations; conversation review checks whether the resulting explanation helps with the task.

## Lessons from development

| Risk found during development | Engineering response | Evaluation focus |
| --- | --- | --- |
| A corrected fact is lost in a follow-up. | Retain corrections in versioned task state. | Follow-up answers reflect the current situation. |
| An answer assumes an instruction or image was delivered. | Bind proposed state and illustration context to delivery evidence. | Generated, delivered and user-completed steps stay distinct. |
| Older material sounds more authoritative than its evidence supports. | Preserve source versions, validity checks and claim qualifications. | Material claims match the available source and its scope. |
| A judge passes an answer that is long or partly irrelevant. | Keep manual review alongside model judging. | Responses address the question without unnecessary steps. |

These lessons explain why source checks, task memory and semantic review are part of the architecture rather than optional additions to prompting.

## Recorded conversation

On 19 November 2026, a fixed eight-turn run on build `3f6cf5a3` used synthetic questions and recorded Telegram delivery. The two tax replies below both passed review for correctness, usefulness and tone, and both were recorded within a minute. Of the other six turns, one returned a service message and five had no recorded reply.

<details>
<summary>Original Russian transcript</summary>

**User**

> Мне пришло начисление 232 лари за октябрь по малому бизнесу. В банке вижу списание 160 лари за этот налог, а в личном кабинете налоговой ещё не смотрела. Платить сегодня все 232 или только разницу?

**Assistant**

> За месяц начислено 232 лари. Банк списал 160 лари, но по банковской выписке ещё нельзя определить доплату. Какая сумма из этого платежа зачислена в бюджет по этому начислению в вашей лицевой карточке RS?

**User**

> Вошла в кабинет. В карточке за октябрь налог 232 лари, из тех 160 лари туда зачислены 150, ещё 10 была банковская комиссия. Зачётов и старых долгов нет. Сколько осталось доплатить за октябрь?

**Assistant**

> За этот месяц осталось доплатить 82 лари. За месяц начислено 232 лари. В бюджет уже зачислено 150 лари. Сумма зачётов за месяц — 0 лари.

</details>

The README images summarize this transcript in English. The test evaluated the assistant's response and recorded delivery for the specified inputs.

## Historical guide delivery record

The guide record dated **26 October 2026**, with closure on **27 October**, contains composite delivery and self-assessment coverage of **48 selected cards and 58 original illustrations**. The guide corpus contains **325 cards across 8 topics**.

Those figures describe that selected historical coverage. Native Telegram UI observation and independent human usability testing were outside its acceptance method. The [aggregate evidence summary](../evidence/guide-acceptance-summary.json) retains that scope.

## Status and provenance

The current product description includes the author's **15 November 2026** update that both modes work. The earlier guide acceptance and assistance development records describe their own dated versions; their historical verdicts are retained in the evidence summary rather than used as the current product status.

Implementation descriptions were checked against private snapshot `a05309faf2eb5ec0806ef0cb9f0d886c828a0211`. The evidence summary identifies the original records. Public content includes aggregate findings and engineering reasoning; application source and raw operational material remain private.

[Architecture](architecture.md) · [Engineering decisions](decisions.md) · [Project overview](../README.md)
