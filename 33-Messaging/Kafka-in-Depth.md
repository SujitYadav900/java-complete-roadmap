# Kafka in Depth

## 1. Introduction

Beyond the basics, production Kafka requires understanding **partitioning, offsets, consumer-group rebalancing, retention, and reliability**. This topic details how Kafka actually works and how to operate it correctly.

## 2. Why This Topic Is Important

- Misunderstanding partitions/offsets causes ordering bugs and lost events.
- Consumer-lag monitoring is the #1 Kafka operational task.
- Senior interviews test exactly-once vs at-least-once and retention.

## 3. Real-World Analogy

A Kafka topic is a **warehouse with labeled shelves** (partitions). Each shelf stores boxes (events) in numbered order (offsets). A delivery team (consumer group) splits the shelves among workers. If a worker quits, the team rebalances and the shelves are re-assigned — nobody re-labels boxes; they just note where to continue reading.

## 4. Theory

### Partitions in detail

- A topic has 1..N partitions; each is an ordered, immutable log.
- Producers pick a partition: key-based (`hash % N`) or round-robin.
- Partitions enable both **throughput** (parallelism) and **ordering** (same key → same partition).

### Offsets & commit

- Each consumer group tracks an **offset per partition** = next event to read.
- **auto-commit** (default interval) can lose events on crash → prefer manual commit after processing.
- `enable.auto.commit=false` + `commitSync()`/`commitAsync()` after success.

### Consumer group rebalance

- When a member joins/leaves, the group rebalances and partitions are reassigned.
- Rebalances can duplicate processing (events re-read) → idempotency required.

### Reliability settings

- `acks=all` — wait for all replicas (no data loss).
- `enable.idempotence=true` — no duplicate writes from a producer.
- Replication factor ≥ 3; retention by time/size (e.g., 7 days).

## 5. Syntax

```yaml
spring:
  kafka:
    producer:
      acks: all
      properties:
        enable.idempotence: true
    consumer:
      enable-auto-commit: false
      group-id: inventory
```

## 6. Flow Explanation

1. Producer picks a partition for each event (key hash or round-robin).
2. Brokers append events to partition logs and replicate them.
3. Consumers commit offsets after successful processing.
4. On member join/leave, the group rebalances and re-assigns partitions.
5. Lag = latest produced offset − consumer's committed offset.

## 7. Memory Representation

```
Partition log (append-only):
  offset 0: event A
  offset 1: event B
  offset 2: event C     <-- consumer's committed offset (next = 2)
  offset 3: event D     <-- latest (lag = 3 - 2 = 1)
```

## 8. Simple Example

```java
@KafkaListener(topics = "orders", groupId = "inventory")
public void onOrder(Order order) {
    process(order);
}
```

## 9. Intermediate Example

```java
// Manual commit after processing
@KafkaListener(topics = "orders", groupId = "inventory")
public void onOrder(Order order, Acknowledgment ack) {
    try {
        process(order);
        ack.acknowledge();       // only now advance the offset
    } catch (Exception e) {
        // no ack -> redelivery
    }
}
```

## 10. Advanced Example

```java
// Keyed ordering + idempotent side effect
kafkaTemplate.send("orders", order.getCustomerId(), order);

@KafkaListener(topics = "orders", groupId = "billing")
public void onOrder(Order order) {
    // Deduplicate by event id
    if (deduplicationStore.alreadySeen(order.getEventId())) {
        return;
    }
    billingService.charge(order);
    deduplicationStore.record(order.getEventId());
}
```

## 11. Output

```text
lag for group billing: topic orders partition 2, lag = 12
lag for group billing: topic orders partition 0, lag = 0
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Auto-commit before processing | Events lost on crash | Commit after processing |
| 1 partition | No parallelism | Partition by throughput |
| No deduplication | Duplicate charges | Idempotent consumers |
| `acks=0/1` in production | Message loss | `acks=all` |
| Ignoring lag | Backlog grows silently | Alert on lag |

## 13. Best Practices

- Choose partition count = expected consumer parallelism × growth.
- Manual offset commit after successful processing.
- Make consumers idempotent with a deduplication store.
- Replication factor ≥ 3; monitor under-replicated partitions.
- Set sensible retention and size limits.

## 14. Interview Questions

**Q: What is consumer lag?**
The difference between the latest produced offset and the consumer group's committed offset — a measure of processing backlog.

**Q: How do you achieve exactly-once in Kafka?**
With idempotent producers, transactional producers, and consumers that deduplicate — or by designing consumers to be idempotent and accepting at-least-once.

**Q: What triggers a rebalance?**
A consumer joining or leaving the group, or a partition/topic change; partitions get reassigned, which may cause duplicate processing.

## 15. Practice Exercises

1. Inspect partitions and offsets via the Kafka CLI (`kafka-consumer-groups`).
2. Switch to manual commits and verify offsets advance only after processing.
3. Add a consumer to a group and observe the rebalance.

## 16. Coding Challenges

1. Build an idempotent consumer that deduplicates by event ID and demonstrate duplicate events are ignored.
2. Simulate consumer lag by producing faster than a slow consumer, then measure the lag.

## 17. Summary

- Partitions provide parallelism and per-key ordering.
- Offsets track consumption; commit after processing.
- Rebalances re-assign partitions — expect duplicates, so be idempotent.
- Reliability: `acks=all`, idempotent producers, replication ≥ 3.
- Monitor consumer lag.
- Next, production messaging patterns in **Messaging-Best-Practices.md**.