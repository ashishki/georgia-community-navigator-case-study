# Georgia Community Navigator

**A Telegram assistant that helps newcomers in Georgia find practical instructions, ask follow-up questions and resume administrative tasks with source-backed guidance.**

**Python · FastAPI · PostgreSQL · Redis Streams · LLM integration · Docker**

Built end to end by [Artem Shishkin](https://github.com/ashishki). The product combines an illustrated reference guide with personalized AI assistance.

[Architecture](docs/architecture.md) · [Engineering decisions](docs/decisions.md) · [Quality and evaluation](docs/evaluation.md)

## Why it matters

Administrative information is spread across government websites, practical guides and community discussions. Finding a page is only part of the work: people still need to understand which procedure applies, what to do next and how to continue when their situation changes.

Georgia Community Navigator brings that journey into one conversation. Shared knowledge gives people a consistent starting point; personalized assistance connects it to their situation; task memory supports continuity. Together, they turn fragmented information into a practical service people can return to as they work through a procedure.

## My role

I owned product definition, user journeys, architecture, the Python backend, Telegram workflows, knowledge organization, and quality and recovery checks. I designed the integration between the guide, AI assistance, task memory and external delivery.

## What the product enables

| User need | Product response |
| --- | --- |
| Find a clear starting point | An illustrated guide with **325 cards across 8 topics**, source links and supporting files. |
| Understand a specific situation | AI assistance that asks clarifying questions and explains the relevant next step using source material. |
| Continue without starting over | Task memory retains corrections, unresolved questions and progress across interactions. |
| Understand where guidance comes from | Source references and version checks connect explanations to their supporting material. |

The guide works without model calls. Personalized assistance builds on shared knowledge and task context, with explicit limits and accounting for model work.

## Stack and why

| Technology | Purpose and rationale |
| --- | --- |
| Python 3.12+ and FastAPI | Async services alongside retrieval and evaluation code. |
| PostgreSQL, SQLAlchemy, Alembic | Transactions, tenant-scoped records and schema migrations. |
| Redis Streams | Consumer groups, acknowledgement and pending-job recovery. |
| HTTPX, JSON Schema, DeepSeek | Explicit provider calls, validated contracts and dispatch accounting. |
| Docker and Docker Compose | Containerized services and isolated optional research tooling. |
| pytest, Ruff, strict mypy | Behavioural checks, consistency and type checking. |

## How the pieces work together

![Guide and AI assistance share task memory and source checks, with durable processing and delivery for recovery](assets/architecture.svg)

Both modes use a common foundation for context, evidence and delivery. PostgreSQL keeps durable state; Redis Streams moves work. Models interpret and explain; application code controls permissions, state transitions, dispatch and limits.

[Follow a request through the architecture →](docs/architecture.md)

## Key engineering decisions

1. **Database-owned workflows:** queue redelivery must not blindly repeat external side effects.
2. **Explicit progress and evidence:** generating an instruction does not mean it was delivered, completed or remains valid.
3. **Accounting before dispatch:** an ambiguous provider timeout may still represent a billable request.

[Read the alternatives and tradeoffs →](docs/decisions.md)

## Quality approach

I check workflow correctness, delivery and answer usefulness separately. The project combines unit, integration and security tests with recorded conversations, automated judging and manual review.

[Read about quality checks and development lessons →](docs/evaluation.md)

## About this repository

This public case study presents the product, architecture and engineering decisions. The application implementation is private.
