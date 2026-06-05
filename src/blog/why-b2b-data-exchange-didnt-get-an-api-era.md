---
layout: post.njk
title: Why B2B data exchange didn't get an API era
description: APIs transformed how internal systems talk to each other. Business-to-business data exchange is still largely files, SFTP, and email. Here's why the API playbook didn't transfer.
date: 2026-06-05
author: Pardis Noorzad
draft: true
eleventyExcludeFromCollections: true
permalink: false
---

The API economy changed software development. Internal systems that used to exchange data through database dumps, scheduled jobs, and brittle custom integrations now talk to each other through REST and GraphQL endpoints, event streams, and webhooks. Developer experience improved. Integrations became composable. A generation of infrastructure companies was built on the assumption that connecting systems via API was the default.

That transformation happened inside organizations and between software products. It did not happen between companies exchanging operational data.

Banks still receive loan tapes as flat files over SFTP. Healthcare systems still exchange patient records as HL7 or CSV drops. Insurers still process claims data from files delivered to a shared directory. Logistics companies still reconcile shipment data from spreadsheet exports. None of this happened because the people running these systems did not know APIs existed.

## The trust problem

An API requires that both sides trust the same interface, the same authentication model, and the same versioning contract.

Inside a single organization, you can mandate that. Engineering leadership can require that internal services use REST with OAuth 2.0 and semantic versioning. If a service breaks the contract, the same organization absorbs the cost and fixes it.

Between companies, you cannot mandate anything. You can request, negotiate, and contractually require — but the other organization makes its own technical decisions, uses its own identity systems, and is not subordinate to your engineering standards. If your API requires a specific authentication flow that their system does not support, you either build an adapter or you fall back to something both sides can agree on.

Files are the lowest common denominator that both sides can usually agree on. Every system that has ever been built can produce a CSV. Every system can consume a CSV if someone writes the parser. The file format is not elegant, but it does not require the other party to implement your API client.

## The compliance problem

Regulated industries face an additional constraint. The data exchanged between companies is often subject to audit requirements: who sent it, when, in what form, and whether the recipient processed it correctly.

APIs can satisfy these requirements, but doing so requires explicit engineering effort on both sides. You need logging of every request and response, immutable records of what was exchanged, and an audit trail that survives system failures. Most API implementations do not have this out of the box. Building it requires agreement between the two parties on what to log and how.

File-based transfer has its own audit challenges, but the record is simpler: the file either arrived or it did not, and a log entry exists for the transfer event. Compliance teams in regulated industries are comfortable with this model. They have been using it for decades, have audit procedures built around it, and are cautious about changing something that passes an audit.

## The stability problem

APIs change. Endpoints get deprecated. Response schemas evolve. Authentication protocols get updated. Managing API versioning is a known engineering problem, and good API providers handle it gracefully — but it still requires the consuming side to update when a breaking change ships.

For a company that receives a weekly loan tape from thirty different partners, the prospect of managing thirty different API versioning cycles is not appealing. A CSV format that has not changed in eight years is operationally stable in a way that a versioned API is not. Organizations that have been burned by a partner API breaking a critical pipeline have strong institutional incentives to prefer something boring and stable.

## The network effect of incumbency

File-based transfer is also self-reinforcing. Every organization that has set up SFTP infrastructure has a partner population that knows how to connect to it. Switching to an API-first model requires that partners also switch. Large enterprise partners have their own technology timelines, their own compliance requirements, and their own change management processes. Unilaterally changing the data exchange interface is a negotiation, not a decision.

This is why the API era did not arrive for B2B data exchange the same way it arrived for internal systems. Internal system migrations are hard. Partner ecosystem migrations are harder. The coordination cost is paid by everyone simultaneously, and there is rarely a central party with enough leverage to mandate it.

## What is actually changing

The picture is not static. Several forces are pushing toward more structured interfaces at the B2B boundary.

Modern data platforms are standardizing on object storage interfaces like S3-compatible APIs. Partners that used to require raw SFTP increasingly accept S3 endpoints, which brings more structure than a filesystem path without requiring a custom API.

Industry consortiums in healthcare, finance, and logistics have been developing standardized data exchange formats: FHIR for healthcare interoperability, FIX for financial markets, EDI standards for logistics. These are not REST APIs, but they are structured interfaces that reduce the negotiation overhead of one-off file formats.

AI-native workflows are creating a new pressure. When an AI agent needs to orchestrate a data transfer, it works best with a structured, machine-readable interface — not a semi-documented SFTP directory with an unspecified file naming convention. As more data workflows involve AI components, the operational cost of unstructured transfer interfaces will become more visible.

The companies building the next layer of B2B data infrastructure are not asking whether files should be replaced by APIs. They are building systems that accept files as inputs and provide API-level control over what happens to them: validation, transformation, scheduling, monitoring, and audit. The interface that partners use stays familiar; the infrastructure that processes the exchange becomes programmable.

That is probably the realistic path. Not replacing the file, but wrapping it in something built to be governed.

If you are working on the data exchange infrastructure problem, please [reach out](#talk-to-sales).
