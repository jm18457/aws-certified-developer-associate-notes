# SQS

## Communication

- Multiple application inevitably need to communicate with one another.
- Two patterns of application communication:
  - Synchrnous: buying <=> shipping
    - Problems: sudden spike in traffic
  - Async / Event based: buying => queue => shipping

## Overview

- Producers send message to SQS queue 
- Consumers poll messages then process and delete it
- Queue decouples your application and allows custom scaling of queue service independant of application
- Oldest offering
- Fully managed service, used to decouple applications
- Unlimited thoughput, unlimited number of messages in queue
- Default retention of messages: 4 days, max 14 days
- Low latency
- Limitation of 256 kb per message sent
- Can have duplicate messages
- Ordering is not guaranteed

## Producing Messages

- Using SendMessage API
- Persisted until consumer deletes it

## Long polling


## SQS with ASG

- SQS Queue polled by ASG ec2. Cloudwatch metric - Queue Length => CloudWatch Alarm => Trigger ASG

## Message Visibility Timeout

- After message is polled by a consumter it becomes invisible to other consumers.
- By default the message visibility timeout is 30 seconds.
- If message takes longer than 30 seconds to be processed it will become visible in SQS queue again and it will processed twice.
- If visibility timeout is high and consumer crashes re processing will take time.
- If visibility timeout is too low we may get duplicates

## Dead Letter Queues

## Delay Queues

- Delay a message so consumers don't see it immediately.
- Default is 0.
- Can set default, per send message etc.
- Example: Set 15 minutes. Producer sends message, it will take 15 minutes before consumer can even see it.

## FIFO Queues

- First in first out (ordered)
- Consumers read messages the same order as producer sends them.
- Limited thoughput: 300 msg/s with batching 3000 msg/s

## SQS vs SNS vs Kinesis

- SQS:
  - Consumer pull data
  - Data is deleted after being consumed
  - Can have as many consumers as we want
  - No ordering
  - Individual delay message capability
- SNS:
  - Push data to many subscribers
  - Up to 10_000_000 subscribers
  - Data is not persisted
  - Integrated with SQS for fan out pattern
- Kinesis:
  - Consumers pull data
  - As many consumers as we want
  - Meant for real time big data

## Amazon MQ

- SQS, SNS cloud-native propriety protocols from AWS.
- Amazon MQ = managed Apache ActiveMQ
- Has both SQS and SNS features.
- When migrating to the cloud instead of re-enginnering the application to use SQS and SNS use Amazon MQ.