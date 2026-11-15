# Evaluation and results

I evaluate the project at three levels: whether the workflow preserves its invariants, whether a response reaches its intended destination, and whether that response helps the person with the actual task. A pass at one level does not establish the others.

## Reference guide

The guide acceptance record dated **26 October 2026**, with a closure record on **27 October**, reports:

| Measure | Recorded result | Meaning |
| --- | --- | --- |
| Guide content | 325 cards, 8 topics, 196 source tasks | Content scope, not a count of users or completed administrative tasks. |
| Selected card coverage | 48 of 48 selected cards | Composite delivery/assessment coverage of the selected set. |
| Original illustration coverage | 58 of 58 originals | 57 photos and one original within a separate PDF. |
| Acceptance method | Operator → application/outbox/sender delivery and self-assessment | The agreed scope of guide acceptance. |

Native Telegram UI observation and independent human usability validation were not included in that acceptance. It does not validate every guide statement or establish personalized AI quality.

A [machine-readable summary](../evidence/guide-acceptance-summary.json) preserves these aggregate figures and their scope without publishing raw recordings.

## Conversational assistance

The latest reviewed status records assistance as **not accepted**. Recorded model conversations include successful, partial and failed results. An automated judge's PASS is kept alongside manual findings, rather than replacing them.

Examples of what the evaluation found:

| Finding | Engineering response | Remaining question |
| --- | --- | --- |
| A corrected user fact was not reliably reflected in the next answer. | Retain corrections in task state and exercise them in repeat conversations. | Does the resulting next step address the original need? |
| An answer referred to an image the user had not been shown. | Bind instruction and illustration context to delivery evidence. | Is the delivered illustration relevant and understandable in the client? |
| Older banking material was stated too categorically. | Add source checks and explicit qualification of historical material. | Is each material claim supported and appropriate to the current situation? |
| A judge passed an answer that manual review found long and partly irrelevant. | Preserve both assessments and repeat affected scenarios. | Does the response help a person act without unnecessary steps? |

These are findings from specific recorded versions. A local fix is not reported as a validated product improvement until the affected behaviour is checked again.

## Engineering checks

The implementation contains unit, integration and security coverage for intake, outbox processing, task state, tenant isolation, source withdrawal, model accounting and delivery. Ruff and strict mypy complement behavioural checks.

Large historical test totals span different versions and scopes, so this case study does not combine them into a single current pass count. The recorded model conversations and historical application checks were not rerun to prepare this public repository.

## Next acceptance work

Resolve the remaining semantic failures and review the affected conversations again; observe the native Telegram experience; and demonstrate production backup restoration and recovery. Product readiness, usage, retention and latency improvements are not established by the results above.

## Evidence provenance

This case study was prepared on **15 November 2026** from private implementation snapshot `a05309faf2eb5ec0806ef0cb9f0d886c828a0211`.

The aggregate guide results come from `reports/product/layer1_close_20261026/acceptance-decision.json` and `reports/product/layer1_verify_20261027/RESULT.md`. Assistance status follows `docs/PROJECT_STATUS.md` and the recorded presentation rechecks dated 29 October 2026. Older overview documents describe an earlier, pre-implementation stage of assistance; the later execution records determine the stage described here.

The public summary contains aggregate results and architectural descriptions. Application source, raw conversations, private operational records and credentials remain private.

[Architecture](architecture.md) · [Engineering decisions](decisions.md) · [Project overview](../README.md)
