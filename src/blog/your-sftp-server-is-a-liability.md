---
layout: post.njk
title: Your SFTP server is a liability, not a feature
description: SFTP became the default for B2B file transfer because it was good enough. Good enough is no longer good enough.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

Somewhere in your infrastructure, there is probably an SFTP server nobody wants to own.

It was set up years ago for a specific partner integration. Then another partner needed the same thing. Then a customer asked for it. At some point it became the default answer to "how do we exchange files with external parties?" It has been running ever since.

If you ask who is responsible for it, you will get a short silence followed by an answer that involves IT operations and possibly a specific engineer who set it up and has since changed roles.

This is not a rare situation. SFTP as a default file exchange mechanism is nearly universal in industries that move sensitive data between organizations: financial services, healthcare, insurance, logistics, legal. It became the default because it solved a real problem in a standardized way, it works across organizations without requiring both sides to adopt the same software, and it was good enough.

Good enough is no longer good enough.

## What SFTP actually gives you

SFTP is a file transfer protocol. It is encrypted in transit. It supports key-based authentication. It works. For moving a file from point A to point B, it does its job.

What it does not give you:

**Usable audit trails.** A standard SFTP server logs connections and file operations to a log file. That log tells you when a connection was made and what files were accessed. It does not tell you why, whether the transfer was expected, whether the volume was normal, or whether the file that arrived was the file that was supposed to arrive.

**Schema or content validation.** SFTP moves bytes. It does not know whether the CSV that landed has the expected columns, whether a required field is missing, or whether a format change on the sender's side has silently broken the downstream process that reads the file.

**Credential management.** In practice, SFTP credentials get shared over email, stored in spreadsheets, and rotated infrequently or never. A partner organization rotates their key and the integration breaks. A former employee still has access to a customer's SFTP drop because the offboarding process did not include reviewing external service accounts.

**Notifications.** When a file does not arrive, SFTP does not tell you. Someone either notices downstream when a process fails, or a human on the partner's side emails to ask whether the file landed. Neither is a monitoring strategy.

**Approval workflows.** There is no policy layer between "someone can write to this directory" and "this write just executed." If a partner uploads the wrong file, or an attacker who has compromised a partner's credentials pushes a malicious payload, SFTP will accept it.

## The operational reality

SFTP servers accumulate. An organization that has been running one for a few years typically has:

- Multiple user accounts, some of which belong to partners who are no longer active
- Credentials that have never been rotated
- A directory structure that was organized by one person and is understood by fewer than three people
- No alerting on failed authentication attempts
- Log files that are either not forwarded to a SIEM or are forwarded but not monitored
- No documentation of which partner uses which directory or what the expected file format is

This is not negligence. It is the natural result of treating SFTP as a utility. Utilities accumulate technical debt because nobody is assigned to maintain them as products.

The security implications are real. SFTP servers that are internet-facing and poorly monitored are a known target. Credential stuffing, brute force, and compromised partner credentials are all vectors that appear regularly in security incident reports.

The operational implications are also real but less visible. When a file fails to arrive, it takes time to diagnose whether the problem is on the sender's side, the receiver's side, or somewhere in the network. When a partner changes their file format without warning, the downstream process breaks silently. When a transfer is needed at a new cadence, someone has to coordinate the change over email.

## What replacing it actually requires

The alternative to SFTP as infrastructure is not a different protocol. It is a different product model.

A transfer system that does not create liability needs:

- An explicit registry of who is allowed to send what, to where, and on what schedule
- Validation at ingestion: does the file match the expected schema, size range, and content rules?
- Notifications when a transfer is late, fails, or deviates from expected volume
- Credentials managed by reference, scoped to specific transfer configurations, and rotated without touching the partner-facing interface
- Audit logs written to a destination the transfer system cannot modify
- A clear ownership model: every transfer configuration has a named owner and a defined review cadence

This is not a longer feature list. It is a different framing. SFTP was designed as a protocol. A transfer system designed for B2B data exchange in 2026 should be designed as infrastructure: observable, governed, and auditable by default.

The SFTP server will probably keep running until something breaks badly enough to force a replacement. The question is whether the replacement is another SFTP server with slightly better configuration, or something built to the right spec from the start.

If you are thinking about what that looks like for your organization, please [reach out](#talk-to-sales).
