---
layout: post.njk
title: What comes after Fivetran
description: Data movement is expanding beyond ELT into hybrid deployment, reverse ETL, custom connectors, and cross-company operational workflows.
date: 2026-05-15
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

Fivetran helped define a category: managed connectors that move data from operational systems into a warehouse.

That was a major abstraction. Teams did not want to maintain hundreds of brittle extract jobs, OAuth flows, API pagination rules, schema drift handlers, and warehouse load routines. Fivetran turned much of that into managed infrastructure.

But the market is moving beyond one-way ELT.

Fivetran's [Hybrid Deployment](https://www.fivetran.com/press/fivetran-launches-hybrid-deployment-enabling-enterprises-to-run-pipelines-in-any-environment-on-a-unified-platform) is a useful signal. It separates the control plane from the data plane: customers can manage pipelines centrally while running data movement inside their own environment. That is not just a deployment option. It is an architectural admission that the place where data moves matters.

Fivetran's planned [Census acquisition](https://www.fivetran.com/press/fivetran-signs-agreement-to-acquire-census-delivering-the-first-end-to-end-data-movement-platform-for-the-ai-era) is another signal. Reverse ETL brings warehouse-modeled data back into operational tools. The story becomes less "load everything into the warehouse" and more "move governed data across the stack."

That still leaves a different class of movement underserved: cross-company operational transfer.

These workflows do not look like clean SaaS-to-warehouse syncs:

- a mortgage company receives recurring loan files from a partner
- an energy company exchanges field data with contractors
- a B2B software company onboards a customer's database export
- a vendor sends a weekly report that must land in an internal system
- a partner changes a file format and the receiving workflow breaks

The technical requirements overlap with ELT, but the product model is different. In ELT, the central object is often a connector syncing into a destination. In cross-company transfer, the central object is the business relationship and the handoff: who requested the data, who owns the source, who owns the destination, what format was expected, when it was due, whether it arrived, and what happened when it failed.

The data may not be headed to a warehouse. It may be moving from S3 to Postgres, from Postgres to S3, from a vendor export into an operational database, or from one company's system into another company's controlled environment. The job is less about analytical freshness and more about operational trust.

That points to a different architecture:

- a control plane for connections, transfer definitions, schedules, permissions, and audit history
- a data plane for worker-based movement
- connector interfaces for files, databases, APIs, and object stores
- validation before and after transfer
- retries, checkpoints, and failure classification
- agent-callable tools for inspection, planning, and troubleshooting

Fivetran's direction shows that data movement is becoming broader, more hybrid, and more bidirectional. But the next frontier is not only "more connectors." It is more context around why data is moving and who is accountable for it.

Warehouses are still important. SaaS connectors are still important. Reverse ETL is useful. But many businesses still run critical operations through SFTP folders, vendor portals, emailed exports, and scripts nobody wants to own.

What comes after Fivetran may not be a larger ELT platform. It may be a transfer layer for the messy boundary between companies, systems, customers, vendors, and agents.

If you are working on cross-company data movement, please [reach out](#talk-to-sales).

