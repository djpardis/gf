---
layout: post.njk
title: The agent-first data transfer layer doesn't exist yet
description: MCP gave agents a way to call tools. Airbyte and Fivetran gave agents access to internal pipelines. Nobody has built the agent-first layer for B2B operational data transfer.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

The agent tooling stack has filled in fast. MCP reached 97 million downloads by early 2026 and is now supported by every major AI development platform. Airbyte launched an MCP endpoint and agent SDK in May 2026, giving agents authenticated access to a pre-indexed context store across 50-plus SaaS connectors. Fivetran shipped an MCP server for managing pipeline state and triggering syncs via natural language. AgentDrop built encrypted agent-to-agent file transfer as an MCP server. AEX (Agent Exchange Protocol) is working on an open protocol for agent-to-agent file handoff with cryptographic identity and pluggable content scanning.

A lot of the agent data infrastructure problem has been worked on. One part has not.

## What has been built

The current tools roughly divide into three categories.

**Agent-to-tool access for internal data.** Airbyte Agents and Fivetran MCP let agents interact with data pipelines and SaaS systems inside an organization. An agent can query a pre-indexed context store across Salesforce, HubSpot, and Jira records. It can ask whether a pipeline sync succeeded, trigger a resync, or update a connector schedule. These tools give agents programmatic access to internal infrastructure.

**Agent-to-agent file exchange for development workflows.** AgentDrop and AEX address a specific problem that MCP does not: agents running in different hosts or on different machines need to hand files to each other. A Claude Code agent produces an artifact and needs to pass it to a Cursor agent. These tools provide encrypted, identity-aware file transfer at the agent level. The use case is primarily developer tooling and multi-agent pipelines.

**Protocol coordination.** A2A (Google's Agent-to-Agent protocol, announced April 2025) handles agent-to-agent task delegation and coordination across vendor boundaries. MCP handles agent-to-tool access. Together they are starting to define the communication stack for multi-agent systems.

## What has not been built

None of these tools address B2B operational data transfer.

Consider what a B2B data transfer workflow actually involves:

- A mortgage company expects a loan tape from a partner every Monday at 06:00
- The file must conform to an agreed schema: specific columns, specific date formats, specific encoding
- Before the file lands in production, it should be validated and require approval
- The transfer must be logged immutably for compliance purposes
- If the file does not arrive by 08:00, the right people should be notified
- If the schema changes, the receiving system should detect that and halt rather than silently ingest bad data
- Every access to the file should be tied to a named principal with a defined scope
- The business relationship — who requested this, who owns it, what the SLA is — should be a first-class object

None of the current agent tools model this. Airbyte Agents gives an agent access to internal SaaS data; it does not model partner relationships, approval policies, or compliance audit trails for cross-company data exchange. Fivetran MCP manages ELT pipeline state; it does not handle the B2B handoff layer. AgentDrop and AEX handle agent-to-agent file passing in developer workflows; they are not built for regulated operational data exchange between business entities.

## Why the gap exists

The agent tooling ecosystem built outward from where engineers spend time: internal systems, SaaS integrations, developer workflows. These are high-visibility problems with large developer audiences and fast feedback loops.

B2B operational data transfer is a different problem space. The stakeholders are often in operations, compliance, and partnerships, not engineering. The failure modes are business continuity failures, not software bugs. The requirements — audit trails, approval workflows, schema enforcement, partner identity management — come from legal and compliance, not product.

The existing MFT and managed file transfer vendors addressed some of these requirements, but in products designed before the agent era. Their architectures do not expose the tool interfaces an agent needs: `inspect_schema`, `validate_transfer`, `plan_transfer`, `run_transfer`, `get_transfer_status`, `explain_failure`. They were built to be operated by humans in a UI, not called deterministically by an agent.

## What agent-first B2B data transfer requires

An agent interacting with a B2B data transfer system needs a different set of guarantees than it needs when calling a SaaS API or triggering an ELT sync.

**Determinism.** When an agent calls `run_transfer`, it should not be improvising. The transfer should execute a defined, pre-validated configuration — not generate a script on the fly. The agent's job is to translate intent into a structured transfer definition. The system's job is to enforce that definition reliably.

**Policy enforcement that the agent cannot override.** The agent should not be able to bypass approval requirements, skip validation, or write to a destination it has not been explicitly authorized to reach. The policy layer should sit below the agent's reach, not inside its prompt.

**Auditable identity.** Every transfer operation should be associated with a named principal — the agent, acting on behalf of a user or an automated workflow — with a logged scope and authorization chain. If a transfer was triggered by an agent, the audit log should reflect that, including what the agent was authorized to do and what it actually did.

**Business relationship context.** The system should model the business relationship as a first-class object: who the partner is, what data they are authorized to send or receive, what the expected format is, what the schedule is, and what the SLA is. An agent querying transfer status should be able to ask "is anything overdue with partner X" and get a meaningful answer.

**Failure with explanation.** When a transfer fails, the agent should receive a structured explanation: was it a connection failure, a credential error, a schema mismatch, a policy violation, or something else? `explain_failure` should return something an agent can act on — retry, alert a human, request a schema update — not a raw log.

## The protocol layer and the operational layer

One useful distinction from the current agent ecosystem is the separation between the protocol layer and the operational layer.

MCP defines how agents call tools. A2A defines how agents coordinate with other agents. These are protocol decisions. They do not define what the tools do.

For B2B data transfer, the interesting work is in the operational layer: what tools the transfer system exposes, what guarantees those tools carry, how identity and policy are enforced, and how the business relationship context is maintained across transfer operations.

The protocol layer is largely settled — MCP is the dominant agent-to-tool interface, and an agent-first transfer system should expose its tools via MCP. The operational layer for B2B data transfer has not been built to the spec that agents require.

## What this means in practice

The companies building agent-first workflows that touch partner data are going to encounter this gap. An agent that can orchestrate an internal data pipeline but cannot safely interact with a B2B data handoff is not a complete workflow.

The transfer layer for cross-company operational data needs to be rebuilt for the agent era. Not as a wrapper around SFTP with an MCP server bolted on, but as infrastructure designed from the start with agent-callable tools, policy enforcement below the agent layer, business relationship modeling, and audit that satisfies compliance requirements for regulated data.

That is the transfer layer that does not exist yet.

If you are building agent workflows that involve B2B data exchange, please [reach out](#talk-to-sales).
