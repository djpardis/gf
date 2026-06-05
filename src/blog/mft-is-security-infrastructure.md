---
layout: post.njk
title: MFT is security infrastructure
description: Managed file transfer sits at the perimeter, holds credentials for both sides of a business relationship, and processes regulated data at scale. It should be treated like security infrastructure. It usually isn't.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

In May 2023, a SQL injection vulnerability in Progress Software's MOVEit Transfer was exploited by the Cl0p ransomware group. Over the following weeks, hundreds of organizations disclosed breaches: US federal agencies, major pension funds, banks, insurers, and healthcare systems. Estimates put the total number of affected organizations above 2,000.

In January 2023, Fortra disclosed a zero-day in GoAnywhere MFT. Attackers used it to exfiltrate data from over 130 organizations before a patch was available.

In 2021, Accellion's legacy File Transfer Appliance was exploited in a similar pattern. The victims included law firms, universities, government agencies, and a major grocery chain.

These are not three isolated incidents. They are the same incident, recurring. Attackers go after managed file transfer products specifically, because the attack surface is extremely high-value: MFT sits at the perimeter, holds credentials for multiple external parties, processes data that crosses company lines, and is routinely under-monitored.

## Why the transfer layer is a target

Managed file transfer occupies a structurally unusual position. Unlike most internal systems, MFT has to be reachable from outside your network — because the whole point is moving data to and from external partners. That means it has a public-facing attack surface almost by definition.

It also holds credentials for both sides of a relationship. To pull a file from a partner's SFTP server, your MFT system needs a username and a private key or password for that server. To deliver a file to a customer's S3 bucket, it needs a cloud credential with write access. A compromised MFT system is not just a file server. It is a credential store for dozens or hundreds of external relationships.

The data passing through is often the most sensitive in the organization. Healthcare organizations transfer patient records. Financial services firms transfer loan tapes, settlement files, and custodial reports. Government contractors transfer regulated documents. Insurers transfer claims data. This is not log noise. It is the data that attackers want.

And yet: most MFT deployments are treated like utilities. They run until something breaks. Patching is slow. Monitoring is sparse. Access reviews rarely happen. The team that owns it is often IT operations, not security. Nobody has looked at the authentication logs in months.

## The category was designed for compliance, not security

The original MFT pitch was not security. It was compliance.

HIPAA required audit logs for data containing protected health information. PCI DSS required encryption in transit and at rest for payment data. SOC 2 required evidence of access controls. MFT vendors added logging dashboards, encryption checkboxes, and compliance report exports. The product got bought because it helped an organization pass an audit, not because it was hardened against attack.

That framing left a large gap. Compliance logging tells you that a transfer happened. It does not tell you whether the transfer was expected, whether the volume was normal, whether the source IP was unusual, or whether credentials were used in an unexpected sequence. A compliant MFT deployment can be thoroughly compromised and generate a clean audit report.

## What treating MFT as security infrastructure actually means

Security infrastructure gets a different operational treatment than utility software. Firewalls get patched on an emergency basis when critical CVEs drop. Identity providers get access reviews on a defined cadence. Authentication events go to a SIEM. Anomalies get alerts.

MFT deserves the same treatment, for the same reasons. That means:

**Patching and exposure reduction.** MFT products that are internet-facing should be patched as quickly as any perimeter device. Legacy on-premise appliances that vendors no longer actively develop should be retired. Exposure surface should be minimized: restrict source IPs, use allowlisting, close unused ports.

**Credential scoping and rotation.** Every external connection should use a dedicated, scoped credential — not a shared account. Credentials should be rotated on a defined schedule and revoked immediately when a partner relationship changes. The MFT system should not hold cleartext secrets; it should reference a secrets manager.

**Anomaly detection on transfer behavior.** Baseline what normal looks like: typical transfer volumes, source IPs, file sizes, transfer times. Alert when something deviates significantly. An attacker staging an exfiltration often looks like a large unexpected transfer to an unusual destination. That is detectable if you are looking.

**Immutable audit logs.** Logs should be written to a destination the MFT system itself cannot modify or delete. If an attacker compromises the transfer system and can also delete its logs, you have no forensic record.

**Policy-enforced approvals.** High-sensitivity transfers — first run to a new destination, unusually large volumes, transfers to external parties not in the approved list — should require human approval before executing. This is not just a compliance checkbox. It is a control that limits the blast radius of a compromised or misconfigured system.

## The product gap

The major MFT vendors built their products in an era when the threat model was different. The compliance requirements drove the product roadmap. The result is software with strong reporting features and weak operational security posture.

A transfer system built today for the threat environment that exists today would look different. It would be designed from the start as an observable, policy-aware transport layer: every transfer logged to an append-only store, credentials managed by reference not by value, anomaly detection built into the monitoring layer, approval workflows for sensitive operations, and a deployment model that reduces perimeter exposure rather than requiring it.

The MFT breaches are going to keep happening as long as organizations treat file transfer as a convenience tool and attackers treat it as a high-value target. Closing that gap starts with being honest about what MFT actually is: not a file delivery utility, but a trust boundary between your organization and everyone outside it.

If you are thinking about how to build a more secure transfer layer, please [reach out](#talk-to-sales).
