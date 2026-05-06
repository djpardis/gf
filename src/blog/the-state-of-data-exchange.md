---
layout: post.njk
tags: post
title: The state of data exchange
description: Data exchange is integral to every business partnership. Yet data exchange practices are highly manual, prone to data leaks, difficult to validate, inherently impossible to monitor, and costly to audit. In this talk, we present an overview of the variety of methods enterprises use to share and transfer data.
date: 2023-04-04
author: Pardis Noorzad
permalink: /blog/the-state-of-data-exchange/
toc:
  - id: the-abstract
    label: The abstract
  - id: the-talk
    label: The talk
  - id: companies-build-solutions-for-data-exchange-today
    label: Companies build solutions for data exchange today.
  - id: but-challenges-still-remain
    label: But challenges still remain.
  - id: so-what-does-a-good-solution-look-like
    label: So what does a good solution look like?
  - id: conclusion
    label: Conclusion
---
A recording of [the talk](https://www.datacouncil.ai/talks/the-state-of-cross-company-data-exchange) is available on YouTube.

{% youtube %}Np0kTZlbRO4{% endyoutube %}

Below you can find the full transcript. I would love to hear about your experiences with data exchange. Please [reach out](#talk-to-sales)!

## The abstract

Data exchange is integral to every business partnership. Yet data exchange practices are highly manual, prone to data leaks, difficult to validate, inherently impossible to monitor, and costly to audit. In this talk, we present an overview of the variety of methods enterprises use to share and transfer data. We talk about some of the challenges companies continue to face along the vectors of security, simplicity, and speed. We conclude by enumerating the properties of a good solution.

## The talk

It's great to be here today, thank you for coming to this talk! I'm Pardis, I started a company called [General Folders](https://medium.com/u/47bc7b8be3d8?source=post_page-----31049fa229f0--------------------------------). And today I want to talk about how businesses exchange data, which is something I've been focused on for a while now!

### Business partners exchange data for a variety of reasons.

It might be surprising to many, as it was for me, but they do this quite often.

One of the scenarios is SaaS vendors sending data to their customers. SaaS vendors offer some sort of service to their customer's customers. The customers' data then ends up in the vendor's warehouse, and so the vendors need to make that data available to their customers.

Another scenario is when a company is doing market research and needs to incorporate market data into their analysis. They need to talk to data vendors to purchase a particular data set. The procurement process usually involves a data exploration and assessment aspect and the transaction could be a one-time transfer or require recurring updates.

Yet another scenario is when a healthcare, transportation, or energy company is responsible for sending data to the local government on a regular basis.

For AI SaaS evaluation, companies usually want to assess whether a new tool would work for their uses cases. Before buying, they usually transfer data to the vendor's warehouse and ask that the vendor demonstrates the effectiveness of the new tool on real customer data.

In M&A, the acquirer needs to run their own diligence on the company they want to acquire. They usually ask for certain data sets and if the deal goes through, a full transfer of data assets.

Sometimes businesses need to collaborate on the intersection of their respective customers. For example, a makeup brand (i.e., the supplier) needs to share data with all retailers that carry the brand to find the marketing channels that work best in each cohort.

Yet in other cases, businesses don't even know the overlap of their customers and that's what they need to find out. We'll get into more details about these applications later.

Okay, enough about use cases — we can go on forever.

## Companies build solutions for data exchange today.

Let's go through some of these solutions and how they work. I've had a lot of conversations with companies about this topic and this list is a kind of summary of those conversations.

The goal is not to judge these methods, or even to assess whether a team made the right call to go with a certain approach — a lot of that depends on infrastructure limitations and deadlines. The goal is to enumerate some of the inevitably great many methods companies use to exchange data with their business partners.

### Send or receive data as an Excel file or CSV by Gmail, Slack, Dropbox, or over SFTP.

One of the most common ways to exchange data is by setting up an SFTP server, uploading a CSV file, then sharing a link. Some use [Amazon Transfer Family](https://aws.amazon.com/aws-transfer-family/) and managed workflows to move data in and out of S3 over SFTP. Others use [Airflow](https://airflow.apache.org/), [Dagster](https://dagster.io/integrations/dagster-ssh-sftp), or [cron jobs](https://en.wikipedia.org/wiki/Cron) to set up and manage pipelines.

![SFTP Data Exchange](/assets/images/blog/sftp-exchange.png)

The data provider manages only half the pipeline. The rest of the pipeline is up to the data consumer.

What happens in this scenario is that the data provider ensures that the data is securely moved from S3 to the SFTP server. It is then up to the data consumer to decide how to handle the downstream data. They need to build their own pipelines.

### Transfer data by sharing AWS or database credentials.

Companies use DB replication tools to implement one-time or recurring syncs to and from customer data stores. You can use [Fivetran](http://fivetran.com/) or [Airbyte](https://airbyte.com/), or even [Debezium](https://debezium.io/blog/2018/07/19/advantages-of-log-based-change-data-capture/) to build your own log-based CDC replication, should a log be available.

You might say, at this point, that sharing database credentials with business partners is not a secure solution. And this is true. But it happens quite often as businesses need to set up reliable end-to-end cross-company pipelines— particularly when one partner is short on staff.

It should be noted that AWS lets you create [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) to securely provide a business partner read or write access to your S3 buckets. This helps avoid sharing credentials in most cases.

![Database Credentials Sharing](/assets/images/blog/database-credentials.png)

### Make data available by exposing an API.

As a SaaS vendor, when you're relatively sure you know what data a lot of your customers want access to, you can make that data available by exposing an API.

There are a lot of benefits to this: 1) consumers can pick and choose the data they need and access it when they need it, 2) there are standards for APIs that everyone can implement, like REST or GraphQL, 3) APIs are DB-independent. 4) APIs are open and not tied to a vendor, 5) the data contract is implicit in an API 6) APIs are easily testable, by both sides of the transaction. These are all properties of good data transfer technology.

### Pull data by implementing an API.

As a SaaS vendor partner, you can implement an API to pull your data from vendors. It turns out that's a whole lot of work. APIs and schemas evolve a lot. The good news is that you can also use tools like [Fivetran](https://www.fivetran.com/), [Airbyte](https://airbyte.com/), [Meltano](https://meltano.com/), or [Rudderstack](https://www.rudderstack.com/) that handle this for you.

![API Implementation](/assets/images/blog/api-implementation.png)

### Send data via an API.

Moving data out of the warehouse and into operational systems like [MailChimp](https://mailchimp.com/en-gb/) and [Braze](https://www.braze.com/) is useful. These systems provide some sort of value on top of raw data. This is made possible by implementing the API they expose to send data. Given the work involved in maintaining that code, it's simpler to use "reverse ETL" or "data activation" tools like [Census](https://www.getcensus.com/reverse-etl), [Hightouch](https://hightouch.com/), and [Rudderstack](https://www.rudderstack.com/).

### Securely share data using Snowflake, Redshift, Azure Data Share, and GCP Datashare.

When transferring data is not necessary, sharing data is way more efficient. However, sharing works in very specific scenarios.

You can share data for *read purposes only* across accounts (which can span regions but of course needs to be on the same cloud), on both Snowflake and Redshift. The great thing about it is that it's instant access. There's no data copies or data movement. It's a view of existing data and so any changes are captured instantaneously by the view. Because data never leaves your servers, you also get straightforward access to usage metrics. You can restrict or revoke access at any time.

![Data Sharing Platforms](/assets/images/blog/data-sharing-platforms.png)

With [AWS Redshift](https://aws.amazon.com/blogs/big-data/announcing-amazon-redshift-data-sharing-preview/), you can create data shares and share it with internal and external customers.

Companies like Stripe use Snowflake and Redshift data sharing capabilities to send all of their customers' up-to-date data, avoiding an API integration. Of course, the API is still available for those that need it. Salesforce CDP also uses this "zero-copy" or "zero-ETL" strategy to data sharing to make their data available to Snowflake customers.

### Make data available on a marketplace.

With [AWS Data Exchange](https://aws.amazon.com/data-exchange/), data vendors can provide easy and secure access to their data, with the ability to reach AWS customers. Consumers can get read access via Redshift sharing capabilities or write pipelines to export the data to S3 and use it from there.

![Marketplace](/assets/images/blog/marketplace.png)

With [Snowflake Marketplace](https://www.snowflake.com/en/data-cloud/marketplace/), data vendors can make their data available to Snowflake customers. With Snowflake data sharing, consumers get read access to the data instantly without the need for ELT integrations.

### Collaborate on overlapping data with data clean rooms.

Sometimes, all that business partners want is to collaborate on overlapping data without the need to fully exchange copies of data. This makes contracts easier and the whole procurement process faster.

![Data Clean Rooms](/assets/images/blog/data-clean-rooms.png)

Clean rooms allow business partners to collaborate on and analyze data in a secure environment, without having to share or reveal user-level data. For example, clean rooms can aid two business partners discover their shared customers. Further, clean rooms sometimes facilitate collaborations even before any sort of partnership contracts are signed.

With [AWS Clean Rooms](https://aws.amazon.com/clean-rooms/) you can invite any AWS customer to collaborate, select datasets, and configure restrictions. You can analyze data with up to 4 parties in a single collaboration. You can set minimum aggregation thresholds while allowing collaborators to run their queries.

Similarly, [Snowflake](https://www.snowflake.com/blog/data-clean-room-explained/), [InfoSum](https://www.infosum.com/), and [LiveRamp](https://business.pinterest.com/blog/pinterest-liveramp-pilot-data-clean-room/) all offer some flavor of clean rooms. Some companies also [implement their own](https://clearcode.cc/blog/data-clean-room/). For example, Disney, Unilever, Hershey's have all built out their own clean rooms to be able to collaborate with marketers and retailers.

## But challenges still remain.

### Data exchange via Excel or CSV loses valuable type information.

CSV suppresses type information that needs to be inferred later on when data is being uploaded back to the database or warehouse.

### Data validation is manual in most cases.

This is an issue on both sides of the transaction. This is not an issue for API-based integration. And it's one of the main areas of strength for APIs.

Everywhere else, there is always the risk that your business partner might mistakenly send you data they should not be sending. And this is usually PII (personal identifiable information) or PHI (protected health information) where you don't have the necessary contracts set into place, but also data for other parts of the business you may not need.

When you talk to data practitioners, you'll see that they've been on the receiving side of a lot of PII data that customers just send by accident.

### Not all data providers expose an API.

Building and managing APIs is not easy. Keeping the API up-to-date and backwards compatible is also time consuming and requires a major commitment from the API provider. We can't expect a partner API to be available for every data set we need from a business partner.

### Not all vendor APIs are implemented by major integration companies.

Connectors for all possible APIs under the sun just doesn't exist. As a data consumer, implementing APIs and keeping the integration code up-to-date is very time consuming. Hence the need for integration companies in the first place.

### Some data consumers are not staffed adequately.

Data consumers are sometimes not staffed appropriately to manage the data they receive. This is especially an issue if data is sent via SFTP.

SFTP data exchange only handles one half of the transaction.

### Data silos make cross-company sharing difficult.

Business partners don't always use the same data warehouse. Not everyone is on Snowflake. This makes the zero-copy strategy harder to implement across organizations.

### Clean rooms are not always an option.

Clean rooms are designed for very specific use cases. There's also a data readiness requirement involved in using clean rooms that might not be met across all business partners.

## So what does a good solution look like?

Here are some properties we think a good solution should have.

### End-to-end secure data pipelines.

A good solution is an end-to-end pipeline that can be set up by business partners quickly and safely. Everything from infrastructure requirements to access control should just work.

### Validation and monitoring should be automatic.

A good solution should be able to tell if a column is likely to be PII, or if it matches the data type in the agreed upon schema. A good solution should be able to alert you if data volumes are lower than expected, or if data is arriving late, or if data is coming from an unexpected source.

### Transfers should be auditable.

A good solution should be able to report on all data that has been transferred between two parties. This is useful for compliance and for understanding the value of the data exchange.

### Data should be accessible to both parties.

A good solution should be able to make data accessible to both parties. This is useful for debugging and for understanding the data that is being exchanged.

### The solution should be warehouse-agnostic.

A good solution should be able to work with any data warehouse. This is useful for business partners that don't use the same data warehouse.

## Conclusion

Data exchange is integral to every business partnership. Yet data exchange practices are highly manual, prone to data leaks, difficult to validate, inherently impossible to monitor, and costly to audit.

We've presented an overview of the variety of methods enterprises use to share and transfer data. We've talked about some of the challenges companies continue to face along the vectors of security, simplicity, and speed.

If you're interested in learning more about how General Folders can help you with your data exchange needs, please [reach out](#talk-to-sales)!
