# SNS

## Overview

- Send one message to many receiver.
- PUB / SUB pattern
- Event Producer sents message to one SNS topic.
- As many event receivers (subscriptions) as we want to listen to the SNS topic notifications.
- Up to 10_000_000 subscriptions per topic.
- 100,000 topics limit
- Topic publish (SDK): 
  - create a topic
  - create subscription
  - publish to topic
- Direct publich (mobile apps sdk):
  - Create platform application
  - platform endpoint
  - publish to platform endpoint


## Fan Out pattern

- Push once in SNS, receive in all SQS queues that are subscribers.
- Fully decouploed no data loss
- SQS Allows for: data persistance, delayed processing and retries of work
- Ability yo add more SQS subscribers over time
- SNS cannot send messages to SQS FIFO queues (AWS limitation)
- Application example:
  - S3 bucket object create. For same combination of event and prefix you can have only one event rule in s3.
  - If you want to send same s3 event to multiple queues you need to use fan-out.
  - S3 object create => s3 => sns topic => multiple queues, lambda etc.