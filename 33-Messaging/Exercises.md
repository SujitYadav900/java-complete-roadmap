# Messaging Exercises

> Practice the messaging concepts from Module 33. Use Spring Boot with RabbitMQ (Docker) and optionally Kafka.

## 1. Beginner Exercises

1. Start RabbitMQ with Docker and verify the management UI.
2. Send a text message with `RabbitTemplate` and receive it with `@RabbitListener`.
3. Explain the roles of producer, broker, and consumer for this flow.

## 2. Routing Exercises

1. Create a topic exchange with two bindings: `order.created` and `order.#`.
2. Publish `order.created` and `order.shipped`; note which queues receive each.
3. Add a fanout exchange and confirm all bound queues receive every message.

## 3. Reliability Exercises

1. Make your queues/exchanges durable.
2. Simulate a failing consumer; watch retries and confirm messages land in a DLQ.
3. Set `default-requeue-rejected: false` and verify behavior.

## 4. Kafka Exercises

1. Start Kafka (Docker) and produce/consume with `kafkaTemplate`.
2. Create a consumer group with two instances and observe partition assignment.
3. Send events with keys and verify per-key ordering.

## 5. Offset Exercises

1. Switch to manual commits (`enable-auto-commit: false`).
2. Crash a consumer mid-processing and verify messages are redelivered.
3. Inspect consumer lag via the Kafka CLI.

## 6. Challenge: Idempotent order consumer

1. Publish `OrderEvent`s with unique event IDs.
2. Build a deduplication store and a consumer that skips duplicates.
3. Replay the same event and prove it processes only once.

## 7. Challenge: Event pipeline

1. Model an "order placed" pipeline: orders topic → inventory consumer + notifications consumer.
2. Add bounded retries and a DLQ for the notification consumer.
3. Simulate a duplicate and a failure; verify both are handled correctly.

> **You've mastered messaging.** Move to **[34 — Testing](../34-Testing/README.md)** — verify everything you've built.