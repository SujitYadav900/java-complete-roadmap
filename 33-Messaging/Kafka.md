# Kafka

## 1. Introduction

**Apache Kafka** is a distributed event-streaming platform built for **high-throughput, durable, replayable** data. Unlike RabbitMQ (a message broker that removes consumed messages), Kafka retains events in a log so consumers can read, replay, and re-process them.

## 2. Why This Topic Is Important

- Kafka powers real-time pipelines: clickstreams, metrics, CDC, microservice events.
- It is the standard for event-driven architectures and data platforms.
- Interviewers ask how Kafka differs from RabbitMQ and how consumers scale.

## 3. Real-World Analogy

Kafka is like a **newspaper archive**. Every edition (event) is printed once and kept forever in the archive (topic). Subscribers (consumers) read editions at their own pace — new subscribers can even start from the first edition ever printed (replay). A message broker like RabbitMQ, by contrast, is like a **courier who delivers your mail and then throws it away**.

## 4. Theory

### Key concepts

| Concept | Meaning |
|---------|---------|
| **Topic** | A named, ordered log of events |
| **Partition** | A shard of a topic; events ordered within it |
| **Producer** | Publishes events to a topic |
| **Consumer** | Reads events from partitions |
| **Consumer group** | A set of consumers sharing a topic's partitions |
| **Offset** | Position of a consumer in a partition |
| **Broker** | A Kafka server storing partitions |

### Ordering & scaling

- Ordering is guaranteed **within a partition**, not across a topic.
- A key routes events to the same partition (`key.hash % partitions`), preserving order per key.
- Consumer groups scale: each partition is consumed by exactly one group member.

### Kafka vs RabbitMQ

| | Kafka | RabbitMQ |
|---|-------|----------|
| Model | Log / event stream | Queue / routing |
| Retention | Events kept (replayable) | Consumed and removed |
| Throughput | Very high | High |
| Best for | Event streams, analytics | Work queues, RPC, pub/sub |

## 5. Syntax

```java
// Producer (spring-kafka)
kafkaTemplate.send("orders", order.getId(), order);

// Consumer
@KafkaListener(topics = "orders", groupId = "inventory")
public void onOrder(Order order) { ... }
```

```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    consumer:
      group-id: inventory
      auto-offset-reset: earliest
```

## 6. Flow Explanation

1. A producer publishes an event to a topic.
2. Kafka appends it to the appropriate partition's log.
3. Consumers in a group are assigned partitions.
4. A consumer reads events from its partitions at its own offset.
5. Events are retained (configurable), so consumers can replay from the beginning.

## 7. Memory Representation

```
Topic "orders" (3 partitions)
Partition 0: [o1, o4, o7]
Partition 1: [o2, o5, o8]
Partition 2: [o3, o6, o9]

Consumer group "inventory" (2 members)
  member A -> partitions 0, 1
  member B -> partition 2
```

## 8. Simple Example

```java
kafkaTemplate.send("greetings", "Hello Kafka");
```

```java
@KafkaListener(topics = "greetings", groupId = "app")
public void listen(String msg) {
    System.out.println("Got: " + msg);
}
```

## 9. Intermediate Example

```java
// Preserve per-order ordering via the key
kafkaTemplate.send("orders", order.getId(), order);   // same id -> same partition

@KafkaListener(topics = "orders", groupId = "inventory")
public void onOrder(Order order) {
    inventoryService.reserve(order);
}
```

## 10. Advanced Example

```java
// Partitioned consumer with explicit assignment info
@KafkaListener(topics = "orders", groupId = "inventory")
public void onOrder(Order order, @Header(KafkaHeaders.RECEIVED_PARTITION) int partition,
                    @Header(KafkaHeaders.OFFSET) long offset) {
    log.info("order {} from partition {} at offset {}", order.getId(), partition, offset);
}
```

## 11. Output

```text
order 1 from partition 0 at offset 3
order 2 from partition 1 at offset 1
order 3 from partition 2 at offset 0
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Assuming topic-wide ordering | Unexpected order across keys | Key to partitions; accept per-key order |
| Too few partitions | Limited parallelism | Size partitions to throughput needs |
| Consumer commits too early | Events lost on crash | Commit after processing |
| No idempotency | Duplicate side effects | Idempotent consumers / keys |

## 13. Best Practices

- Choose a partition key that matches your ordering needs (e.g., order ID).
- Use consumer groups for parallelism; keep one consumer per partition max.
- Set `auto-offset-reset: earliest`/`latest` deliberately.
- Enable idempotent producers and handle duplicates in consumers.
- Monitor consumer lag (the lag between produced and consumed offsets).

## 14. Interview Questions

**Q: What is a Kafka partition and why does ordering matter per partition?**
A partition is an ordered shard of a topic; Kafka only guarantees order within a partition, so you choose keys that route related events together.

**Q: How do consumers scale in Kafka?**
Consumers in a group split the partitions; each partition is processed by exactly one consumer in the group, so adding members increases parallelism up to the partition count.

**Q: Kafka vs RabbitMQ?**
Kafka is a durable, replayable event log for high-throughput streams; RabbitMQ is a broker that routes and removes messages — better for classic queues.

## 15. Practice Exercises

1. Run Kafka (Docker) and send/receive with `kafkaTemplate`.
2. Create a consumer group and run two instances; watch partitions split.
3. Send events with a key and observe they land in the same partition.

## 16. Coding Challenges

1. Produce 100 events with 5 keys and verify per-key ordering in a consumer.
2. Change a consumer group's `auto-offset-reset` and observe replay from earliest vs latest.

## 17. Summary

- Kafka is a distributed, durable event log — events can be replayed.
- Topics → partitions → offsets; ordering is per-partition.
- Consumer groups distribute partitions across members.
- Keys control partitioning and ordering.
- Next, dive deeper into **Kafka-in-Depth.md**.