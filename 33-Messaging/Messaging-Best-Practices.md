# Messaging Best Practices

## 1. Introduction

Production messaging systems share a set of hard-won rules: consumers must be **idempotent**, failures must go to a **dead-letter queue**, ordering must be **explicit**, and the whole pipeline needs **observability**. This topic consolidates those patterns.

## 2. Why This Topic Is Important

- At-least-once delivery means duplicates are normal — not handling them causes double payments, double emails, double stock.
- A misconfigured DLQ silently drops business-critical events.
- Interviewers probe exactly these production concerns.

## 3. Real-World Analogy

Message processing is like a **bank clerk processing deposit slips**: if the clerk isn't sure a slip was processed (no ack), the bank re-sends it (redelivery). The clerk must recognize and ignore duplicates (idempotency), and slips that can't be processed go to a special tray (DLQ) rather than the shredder.

## 4. Theory

### The golden rules

1. **Idempotency** — processing a message twice must equal processing it once.
2. **Acks** — acknowledge only after successful processing.
3. **DLQ** — failed messages go to a dead-letter queue, not to the void.
4. **Ordering** — order only where needed (per key), and say so.
5. **Backpressure & lag** — monitor queue depth and consumer lag.
6. **Retries** — bounded retries with exponential backoff, then DLQ.

### Idempotency techniques

| Technique | How |
|-----------|-----|
| Deduplication store | Record processed event IDs, skip duplicates |
| Natural keys | e.g., idempotency key from the client |
| Database constraints | Unique index on event ID |
| Conditional writes | `UPDATE ... WHERE status = 'NEW'` |

### Payload design

- Version the schema (`"v": 1`).
- Keep messages small and self-contained.
- Include a unique event ID and a timestamp.

## 5. Syntax

```java
@RabbitListener(queues = "payments.queue")
public void onPayment(PaymentEvent event) {
    if (dedupStore.alreadySeen(event.getEventId())) {
        log.info("Duplicate event {} skipped", event.getEventId());
        return;
    }
    paymentService.process(event);
    dedupStore.record(event.getEventId());
}
```

## 6. Flow Explanation

1. Producer sends an event with a unique ID.
2. Consumer receives it (possibly more than once).
3. A deduplication store filters duplicates.
4. Business logic runs; on success the message is acknowledged.
5. On repeated failure, the message moves to the DLQ for inspection.

## 7. Memory Representation

```
producer ─▶ queue ─▶ consumer (dedup? no → process → ack)
                              │ fail ×3
                              ▼
                           DLQ ─▶ operator/alert
```

## 8. Simple Example

```java
@RabbitListener(queues = "notifications.queue")
public void sendEmail(NotificationEvent e) {
    emailService.send(e);
}
```

## 9. Intermediate Example

```java
// Bounded retries then DLQ (Spring AMQP)
@RabbitListener(queues = "notifications.queue")
public void sendEmail(NotificationEvent e) {
    try {
        emailService.send(e);
    } catch (Exception ex) {
        throw new AmqpRejectAndDontRequeueException(ex);
    }
}
```

```yaml
spring:
  rabbitmq:
    listener:
      simple:
        retry:
          enabled: true
          max-attempts: 3
          initial-interval: 1s
          multiplier: 2
        default-requeue-rejected: false
```

## 10. Advanced Example

```java
// Event with an idempotency key + versioned payload
public class OrderEvent {
    private String eventId;      // unique
    private String version;      // "1.0"
    private Long orderId;
    private String action;       // CREATED, SHIPPED
    private Instant occurredAt;
}

@KafkaListener(topics = "orders", groupId = "shipping")
public void onOrder(OrderEvent event) {
    if (!dedupStore.claim(event.getEventId())) return;   // atomic dedup
    shippingService.dispatch(event);
}
```

## 11. Output

```text
Processing event 42 (new)
Processing event 42 (duplicate, skipped)
event 42 failed 3 times → moved to dead.letter.queue
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No deduplication | Duplicate side effects | Idempotency store |
| Ack before processing | Events lost on crash | Ack after success |
| No DLQ | Failures vanish | Configure a DLQ |
| Infinite retries | Queue blocked forever | Bounded retries |
| Unversioned payloads | Consumers break on schema change | Version events |

## 13. Best Practices

- Design every consumer to be idempotent.
- Ack after processing, not on receipt.
- Bound retries with backoff, then DLQ + alerting.
- Include event IDs, versions, and timestamps.
- Monitor queue depth, lag, and DLQ count.

## 14. Interview Questions

**Q: Why must consumers be idempotent?**
Most systems deliver at-least-once — messages can be redelivered on crashes or rebalances, so processing must tolerate duplicates.

**Q: What is a dead-letter queue?**
A queue where messages that repeatedly fail are placed for manual inspection instead of blocking or vanishing.

**Q: How do you deduplicate messages?**
Store processed event IDs (DB unique constraint or key-value store) and skip events already seen.

## 15. Practice Exercises

1. Add a deduplication store and replay the same event twice.
2. Configure bounded retries and a DLQ; send a failing message and watch it land there.
3. Version a payload and make two consumer versions handle it.

## 16. Coding Challenges

1. Build a payment consumer that is provably idempotent (double delivery = one charge).
2. Set up monitoring that alerts when consumer lag exceeds a threshold.

## 17. Summary

- At-least-once means duplicates — build idempotent consumers.
- Ack after success; retry with bounds; route failures to a DLQ.
- Version payloads and include event IDs.
- Monitor queue depth and consumer lag.
- Messaging completes your distributed toolkit — next, **Module 34 — Testing**.