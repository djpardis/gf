---
layout: post.njk
title: The vendor onboarding tax
description: Every time a company onboards a new data partner, it pays a hidden tax in IT tickets, credential emails, schema negotiations, and delayed first transfers. Nobody measures it. Everyone pays it.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

The first data transfer with a new partner rarely happens on the day anyone wants it to.

A new vendor relationship is signed. Somewhere in the contract is a clause about data exchange: the vendor will send a weekly file, or the customer needs to provide an export, or both sides will sync a dataset on a defined schedule. Someone from IT is looped in. Emails start moving.

What follows is a negotiation that nobody formally owns and that takes much longer than anyone expects.

## What the onboarding process actually looks like

The sequence is roughly:

1. Someone asks how the other side wants to receive data. The answer is usually SFTP, S3, or email, depending on how old the relationship is.

2. Credentials are exchanged. This happens over email, or a ticketing system, or occasionally a shared document. The credential is a username and password, or a private key, or an access key pair. It arrives in a format that needs to be reformatted before it can be used. There is back-and-forth.

3. Someone tests the connection. It fails. The reason is usually a firewall rule, an IP allowlist that needs to be updated, or a directory permission that was not set correctly.

4. A file format is negotiated. One side has a CSV with a specific column layout. The other side's system expects a different column order, different date formats, or a different encoding. This is discovered when the first test file fails validation, which itself requires that the receiving side has validation, which it often does not.

5. A schedule is agreed on. This happens in a calendar invite or an email thread, not in a system. There is no automation. Someone on one side has a cron job or a manual reminder. Someone on the other side has a support ticket that fires if they do not receive the file by a certain time.

6. The first successful transfer happens. At this point, the process has taken anywhere from two weeks to three months, depending on the complexity of the integration and the responsiveness of the teams involved.

7. The configuration is documented somewhere informal — a Confluence page, a README in a repository, a comment in the cron job — and then slowly diverges from reality as things change on either side.

## The cost nobody adds up

Each of these steps has a cost. IT time to create and manage credentials. Engineering time to debug connection issues. Operations time to negotiate schemas and validate files. Management time to follow up when something is late.

None of this cost appears on a line item. It is spread across multiple teams, absorbed into support queues and on-call rotations, and attributed to the overhead of running a business. Companies that do a lot of partner integrations do not calculate how much they spend on onboarding each one. They should.

The costs compound in less visible ways too. When onboarding is slow, partnerships start later than they should. When onboarding is manual, errors happen — wrong credentials, wrong directories, wrong formats — and each error adds another round of coordination. When onboarding is undocumented, the knowledge lives in whoever managed the original integration, and when that person leaves, the knowledge leaves with them.

## Why this has not been solved

The tooling for internal data pipelines has improved dramatically over the past decade. ELT platforms, orchestration tools, and data catalogs have made it significantly easier to connect internal systems and keep them in sync. But these tools are designed for connections within an organization's control plane. They assume you own both ends.

Cross-company data exchange is structurally different. You do not control the other side. You cannot install your preferred agent on a partner's server. You cannot require that a vendor adopt your API format. You have to meet the other party where they are — and where they are is usually SFTP, S3, or email, configured differently for every relationship.

The result is a long tail of bespoke integrations, each assembled by hand, each slightly different, each requiring manual intervention when something changes.

## What a better onboarding process looks like

The core problem is that partner onboarding involves too many steps that require human coordination for things that could be handled by software.

Credential exchange should not happen over email. A partner should be able to provision access through a self-service interface, generate scoped credentials, and configure their connection without filing a ticket. The receiving side should be notified automatically when a new connection is established.

Format negotiation should happen at the system level, not through email threads. If a partner's file does not match the expected schema, the system should tell them specifically what is wrong and what the expected format is, not rely on someone to check an inbox and reply.

Schedules should be first-class objects in the transfer system. When a file is expected at a specific time, the system should know that and alert when it does not arrive, rather than requiring a person to notice.

Documentation should be a byproduct of configuration, not a separate task. The transfer definition — who sends what, in what format, on what schedule, to what destination — should be the record. It should not need a Confluence page to interpret it.

The vendor onboarding tax is not inevitable. It is a product gap. The companies that eliminate it will have faster partner integrations, fewer operational errors, and lower support costs. That is a durable competitive advantage in any industry where data exchange with external parties is a core workflow.

If you are thinking about how to reduce onboarding friction for your data partnerships, please [reach out](#talk-to-sales).
