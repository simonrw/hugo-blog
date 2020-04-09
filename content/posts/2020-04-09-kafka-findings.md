---
title: Kafka findings
Authors: Simon Walker
date: 2020-04-09 09:28:12
tags:
- kafka
summary: How does Kafka distribute its messages to consumers?
---

# Outline

How does Kafka distribute its messages?

I wanted to use Kafka in two ways:

1. to behave like a FIFO queue where work can be parallelised by spinning up multiple worker processes such that they consume messages once from the topic in a first-come first-served way, and
2. to split the work deterministically between workers.

The second use case is where Kafka excels over other pub/sub messaging systems (e.g. AMQP) as messages can be assigned to partitions to ensure consistency, however for my greater understanding I wanted to know if Kafka could be used as a drop-in replacement for e.g. Rabbitmq.

## Experiment

* Set up a Kafka broker (single one for now) and try to consume messages from a topic.
* Play with the combination of partition number and group id to test the desired results.


| Partition # | Consumer # | Group ID distribution | Observations                                                  |
| --          | --         | --                    | --                                                            |
| 1           | 1          | -                     | All messages are delivered to the single consumer             |
| 1           | N          | same                  | All messages are delivered to a single consumer in that group |
| 1           | N          | different             | All messages are sent to _each_ consumer                      |
| N           | 1          | -                     | All messages are delivered to the single consumer             |
| N           | N          | same                  | Messages are distributed between consumers                    |
| N           | N          | different             | All messages are sent to _each_ consumer                      |


## Message keys

Messages can be assigned _keys_ which ensure predictable routing. This is desired as Kafka guaruntees consistent ordering _within a partition_. If the use case relates to relationships between consistent sources of data and consistent ordering is required then a deterministic key should be assigned to each message.

For example given an event-sourced model of bank accounts, we model the changes to an account as a stream of information. In order to calculate the balance at any one point, a stream of changes ("deltas") must be summed. To do this accurately, the consumer that is aggregating the deltas must get all of the messages for that account. We can therefore give each message a key that relates to that bank account.

# Summary

1. For use case 1 (distributing messages evenly to a pool of consumers, distributing load):
    * the topic must contain multiple partitions
    * each consumer must have a unique group id
2. For use case 2 above:
    * the topic must contain multiple partitions
    * each consumer must have a unique group id
    * message keys provide predictable routing

A helper environment can be found [here](https://github.com/mindriot101/kafka-partition-testing-src.git) which sets up a Kafka environment with `docker-compose`.
