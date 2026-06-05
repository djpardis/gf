---
layout: post.njk
title: Transfer failure is a business failure
description: When a data transfer fails, the technical problem is usually the smallest part of it. The real cost is downstream — in delayed settlements, broken processes, and strained partner relationships.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

A loan tape does not arrive on Friday morning.

Somewhere downstream, a reconciliation job is waiting for it. The job eventually times out. An analyst notices on Monday. They file a support ticket. IT checks the SFTP server logs. The file was never uploaded. Someone emails the partner. The partner says they sent it — they have a sent confirmation in their system. Back-and-forth follows. By Tuesday, the root cause is identified: a firewall change on the partner's side blocked the outbound connection. The file is resent. The reconciliation runs two days late. A settlement is delayed. A downstream reporting deadline is missed.

No data was lost. No system was compromised. The transfer simply failed silently, and the failure propagated through every process that depended on it.

## The failure mode nobody instruments

Data transfer failures are a business continuity problem, but they are rarely treated as one.

Engineering teams instrument applications. They add health checks to services, set up uptime monitoring for APIs, track error rates in observability dashboards, and get paged when a database becomes unreachable. The investment in detecting and responding to system failures has grown substantially in the past decade.

Transfer failures get much less attention. A file that was expected and did not arrive does not generate an exception. A transfer job that exits cleanly after writing zero bytes looks like a success at the infrastructure level. A partner that changes their file format without notice produces a file that lands correctly but fails silently when the consuming system cannot parse it.

The failure is real, but it happens in a gap between systems. No single system is responsible for knowing that a specific file was supposed to arrive at a specific time, and none of the systems on either side notices its absence.

## The downstream cost is larger than the transfer cost

Transfer failures are deceptively cheap to fix in isolation. The file is resent. The job reruns. The issue is closed.

The downstream cost is what accumulates. In financial services, a delayed settlement can trigger penalties, credit implications, or regulatory scrutiny. In healthcare, a delayed record transfer can affect care coordination or claims processing timelines. In logistics, a delayed shipment data feed means decisions are made on stale information. In any B2B context, a pattern of transfer failures damages the business relationship — partners lose confidence in the reliability of the integration, and reliability conversations that should be routine become escalations.

These costs are hard to see in a post-mortem because they are distributed across teams and time. The file transfer failure closes when the file is resent. The downstream delay closes when the reconciliation reruns. The partner conversation closes when someone from account management smooths it over. Nobody adds up the total cost of the original transfer failure.

## Reliability requires making expectations explicit

The root cause of most silent transfer failures is that expectations are implicit.

The reconciliation job knows it needs a file, but the transfer system does not know the job exists. The partner knows they send a file every Friday, but there is no system that knows a file is expected from that partner every Friday and can notice when Friday passes without one.

Making expectations explicit means the transfer system — not the downstream job, not the analyst, not the account manager — is responsible for knowing what is expected and alerting when it does not happen.

This requires treating transfers as scheduled commitments, not fire-and-forget events. A transfer definition should include:

- **Expected cadence:** daily, weekly, on the first business day of the month, triggered by an upstream event
- **Expected window:** the transfer should arrive by 06:00 UTC; alert if it has not arrived by 07:00
- **Expected shape:** the file should have these columns, this approximate row count range, this encoding
- **Escalation policy:** who gets notified first, who gets notified if the first alert is not acknowledged

With these defined, a transfer system can distinguish between "the file arrived and looks correct," "the file arrived but failed validation," "the file has not arrived yet," and "the file is past its expected window and no one has acknowledged the alert." Those four states have very different implications and require different responses.

## Failure classification matters

Not all transfer failures are equal, and the response should match the type of failure.

A transient network error during a transfer is different from a persistent authentication failure. A file that arrived with the wrong column count is different from a file that arrived with the right structure but suspicious content. A transfer that is two hours late is different from a transfer that has not run in three days.

A transfer system that classifies failures by type enables proportionate responses: automatic retry for transient errors, immediate alert for authentication failures, validation report for schema mismatches, and escalating notifications for overdue transfers.

Without classification, all failures look the same in a log file. The response is always "check the logs," and the logs always require interpretation.

## The business continuity framing

Data transfer reliability should be treated the same way as any other operational dependency: with explicit SLAs, monitoring, and incident response processes.

If a database becomes unavailable, there is a runbook. Someone gets paged. The incident is tracked. A post-mortem happens. Transfer failures that affect business-critical processes deserve the same treatment. The transfer window, the escalation chain, and the acceptable recovery time are business decisions, not engineering details.

Organizations that make this shift — from treating transfer failures as IT incidents to treating them as business continuity events — end up with faster recovery times, clearer partner SLAs, and better visibility into the operational risk embedded in their data exchange workflows.

The file that did not arrive is not just a file. It is everything that was supposed to happen after it did.

If you are thinking about how to build more reliable data transfer processes, please [reach out](#talk-to-sales).
