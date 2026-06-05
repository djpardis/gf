---
layout: post.njk
title: Zero trust for data movement
description: Zero trust transformed network access and identity. The same principles — verify every request, least-privilege access, full audit — haven't been applied to data movement yet.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

Zero trust started as a network security model. The original idea, articulated by John Kindervag at Forrester around 2010, was simple: stop assuming that traffic inside the network perimeter is safe. Verify every request. Grant least-privilege access. Assume breach.

The industry took about a decade to make it operational. BeyondCorp at Google showed what zero trust looked like in practice for network access. Cloud identity providers built policy engines that could enforce least-privilege access dynamically. NIST published a formal architecture document in 2020. Now zero trust is a standard design principle for enterprise identity and network access.

It has not been applied to data movement.

## What zero trust says and what data movement currently does

The core principles of zero trust are:

1. Never implicitly trust a principal — verify identity on every request, regardless of network location
2. Grant least-privilege access — the minimum access required for the specific operation, not broad access to a system
3. Assume breach — behave as if an attacker already has some foothold, and design controls accordingly
4. Inspect and log everything — full visibility into what happened, who did it, and when

Apply these to data movement and the gap becomes clear.

**Implicit trust is the default.** A credential that can connect to an SFTP directory can typically read everything in that directory, write anything to it, and connect as many times as it wants. There is no concept of "this credential is authorized to transfer file X from source Y to destination Z on Tuesdays." The credential grants access to the system; the system trusts the credential.

**Privilege is broad by design.** File transfer integrations are typically configured with service accounts or access keys that have the minimum permissions needed to connect — but "minimum to connect" is often still far broader than the specific transfer being authorized. A service account with read access to a partner's S3 prefix can read any file in that prefix, not just the ones that were supposed to be transferred this week.

**Breach is not assumed.** If an attacker obtains a partner's SFTP credential, they can connect to your SFTP server and access anything that credential can access. There is typically no anomaly detection comparing the connection behavior against a baseline, no policy requiring that connections come from known IP ranges, and no approval gate for operations that deviate from the expected pattern.

**Logging is incomplete.** Transfer logs usually record that a connection happened and what files were accessed. They do not record the full context of why the transfer was authorized, whether it was expected, or whether it conforms to the policy that was supposed to govern it.

## What zero trust applied to data movement looks like

**Transfer-scoped authorization.** Instead of granting a credential access to a system, grant it access to a specific transfer. The authorization is "entity A is allowed to deliver file format F to destination D on schedule S, through connection C." Any operation outside that scope — accessing a different directory, uploading at an unexpected time, transferring an unexpected volume — fails and alerts.

**Just-in-time credentials.** Rather than issuing long-lived credentials that persist across many transfer operations, generate short-lived credentials scoped to a specific transfer execution. The credential expires after the transfer completes or after a short window. This limits the blast radius if a credential is compromised: it can only be used for the transfer it was issued for, and only briefly.

**Behavioral baselines and anomaly detection.** A transfer system that has been running for weeks has a behavioral baseline: typical transfer times, typical file sizes, typical connection sources. Deviations from that baseline — large unexpected transfers, connections from new IP ranges, files that are much larger or smaller than usual — should trigger alerts. This is the "assume breach" principle applied: the system behaves as if a credential might be compromised, and looks for evidence.

**Approval gates for sensitive operations.** Certain operations should require explicit authorization before they execute: the first transfer to a new destination, transfers above a volume threshold, transfers that contain certain data classifications. A zero trust model for data movement makes these approval requirements explicit and enforced, not advisory.

**Immutable, full-context audit logs.** Every transfer operation should produce a log entry that includes not just what happened, but the full authorization context: who was authorized to perform this transfer, what policy governed it, whether the transfer matched expectations, and what the outcome was. The logs should be written to a store that the transfer system cannot modify.

## Why this matters now

Two forces are making the zero trust gap in data movement more urgent.

The first is the attack surface. MFT breaches — MOVEit, GoAnywhere, Accellion — demonstrate that attackers specifically target the data transfer layer because it concentrates credentials and sensitive data from many organizations in a single, often under-monitored system. Applying zero trust principles to that layer reduces the blast radius when, not if, a transfer system or a partner credential is compromised.

The second is the agent surface. As AI agents take on more operational data tasks — moving files, triggering transfers, accessing partner systems — the implicit trust model becomes untenable. An AI agent acting on behalf of a company needs the same kind of constrained, auditable, least-privilege access that a human operator should have. The infrastructure to provide that does not currently exist for most data movement tooling.

Zero trust for network access took a decade to go from concept to standard practice. The path was: articulate the model clearly, show what it looks like in production, build tooling that makes compliance tractable, and make the default infrastructure zero-trust-aligned.

The same path applies to data movement. The model is clear. The production examples are starting to emerge. The tooling gap is where the work is.

If you are thinking about how to apply zero trust principles to your data transfer infrastructure, please [reach out](#talk-to-sales).
