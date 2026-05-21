---
layout: post.njk
title: Agents need transfer tools, not more scripts
description: AI agents will need to move data between systems, but the safe pattern is deterministic transfer tools with permissions, logs, and approvals.
date: 2026-05-15
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

AI agents are good at turning intent into code. That is useful, but it is the wrong abstraction for operational data transfer.

Consider a common request: "Move this customer's weekly loan tape from their S3 bucket into our Postgres database, validate the columns, retry transient failures, and alert us if the schema changes."

An agent can probably write a script that does this once. The hard part is not the first successful run. The hard part is everything that makes the transfer safe to run again:

- credential storage and rotation
- source and destination permissions
- schema inspection
- idempotency
- retries and backoff
- checkpoints
- partial failure handling
- logs and metrics
- approval policy
- audit history

This is why agents should not directly improvise data movement. They should call deterministic transfer tools.

The industry is already moving toward tool-based agents. Anthropic introduced the [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) to standardize how AI systems connect to external tools and data sources. Airbyte's [AI agents work](https://docs.airbyte.com/ai-agents/get-started/what-is-airbyte-agents) also points in the same direction: agents need structured access to connectors, context, and actions.

For data transfer, the tool boundary needs to be especially strict. A good agent interface should expose operations like:

- `test_connection`
- `inspect_schema`
- `plan_transfer`
- `validate_transfer`
- `create_transfer`
- `run_transfer`
- `get_transfer_status`
- `get_transfer_logs`
- `explain_failure`
- `retry_transfer`

Those tools should be deterministic backend operations, not prompts. `inspect_schema` should connect with a scoped credential and return a typed result. `validate_transfer` should check that the source exists, the destination is writable, required mappings are present, and the run is allowed by policy. `run_transfer` should enqueue a job for a worker. `retry_transfer` should respect retry limits and approval rules.

The agent's job is to translate messy intent into a structured plan:

```text
source: customer_s3.loan_tapes
destination: internal_postgres.loan_tapes_raw
schedule: Mondays at 06:00 UTC
validation: required columns, file freshness, row count
policy: require approval before first production run
```

The transfer system's job is to enforce that plan through code.

This separation is the difference between "an agent wrote a script" and "an agent used infrastructure." The first is flexible but hard to govern. The second is narrower but much safer.

The right architecture is a control plane and a data plane. The control plane stores transfer definitions, permissions, credential references, schedules, logs, and audit events. The data plane runs workers that move data and report progress. Agents interact with the control plane. Workers do the dangerous work.

That model lets agents help without giving them unchecked access to credentials, networks, and production databases. It also gives humans a record they can inspect later.

The future of agentic data transfer should not be a new pile of generated scripts. It should be agents calling safe transfer primitives that were built to be observed, retried, audited, and controlled.

If you are thinking through safe data transfer tools for agents, please [reach out](#talk-to-sales).

