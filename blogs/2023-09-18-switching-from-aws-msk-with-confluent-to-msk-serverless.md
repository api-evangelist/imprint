---
title: "Switching from AWS MSK with Confluent to MSK Serverless with Segmentio"
url: "https://tech.imprint.co/aws-msk-serverless-segment/"
date: "2023-09-18"
feed_url: "https://tech.imprint.co/index.xml"
---
Context Imprint uses Kafka for streaming realtime data into our data warehouse, along with asynchronous processing. This data includes updates to database entities, external events, webhook payloads, audit logs, and important internal metrics. Before this migration, we used AWS Managed Streaming for Kafka (MSK), and Confluent’s Kafka Go library.
