# JMS (Java Message Service)

## 1. Introduction

**JMS** is Java's standard API for messaging with brokers like ActiveMQ, Artemis, and IBM MQ. It supports **point-to-point** (queues) and **publish/subscribe** (topics) models through `ConnectionFactory`, `Queue/Topic`, and `MessageProducer`/`MessageConsumer` objects.

## 2. Why This Topic Is Important

- JMS is the classic Java messaging API and appears in legacy and enterprise systems.
- Understanding JMS's objects and acknowledge modes clarifies all Java messaging.
- Spring's `JmsTemplate` and `@JmsListener` build directly on it.

## 3. Real-World Analogy

JMS is like a **standardized mail system**: `ConnectionFactory` is the post office branch, a `Queue` is a specific mailbox, a `MessageProducer` is your pen and envelope, and a `MessageConsumer` is the person checking their mailbox. The JMS spec guarantees that different post offices (brokers) follow the same rules.

## 4. Theory

### Core objects

| Object | Role |
|--------|------|
| `ConnectionFactory` | Creates connections to the broker |
| `Connection` / `Session` | Connection and transaction context |
| `Destination` | `Queue` (P2P) or `Topic` (pub/sub) |
| `MessageProducer` | Sends messages |
| `MessageConsumer` | Receives messages |
| `Message` | Payload + headers (`TextMessage`, `ObjectMessage`, `BytesMessage`) |

### Acknowledge modes

- `AUTO_ACKNOWLEDGE` — ack on receipt.
- `CLIENT_ACKNOWLEDGE` — app calls `message.acknowledge()`.
- `DUPS_OK_ACKNOWLEDGE` — lazy acks, may redeliver.

### Spring JMS

`JmsTemplate.convertAndSend(destination, object)` and `@JmsListener(destination = "...")` simplify everything.

## 5. Syntax

```java
// Plain JMS send
Connection conn = factory.createConnection();
Session session = conn.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(session.createQueue("orders"));
TextMessage msg = session.createTextMessage("Hello");
producer.send(msg);
```

```java
// Spring style
jmsTemplate.convertAndSend("orders", order);

@JmsListener(destination = "orders")
public void handle(Order order) { ... }
```

## 6. Flow Explanation

1. Create a connection and session from the `ConnectionFactory`.
2. Create a producer bound to a destination (queue or topic).
3. Send a message.
4. A consumer bound to the same destination receives it.
5. Depending on the ack mode, the app acknowledges — or the broker redelivers.

## 7. Memory Representation

```
ConnectionFactory
  └─ Connection ── Session ── Producer ──▶ Queue/Topic
                                 Consumer ◀───┘
```

## 8. Simple Example

```java
public class Sender {
    public void send(JmsTemplate jms, String body) {
        jms.convertAndSend("greetings", body);
    }
}
```

## 9. Intermediate Example

```java
@JmsListener(destination = "orders")
public void process(Order order) {
    try {
        orderService.process(order);
        // AUTO_ACKNOWLEDGE acknowledges on return
    } catch (Exception e) {
        // throw -> redelivery
    }
}
```

## 10. Advanced Example

```java
// Client-acknowledge with manual control
@JmsListener(destination = "critical.tasks", ackMode = "CLIENT")
public void handle(Message message, Session session) throws JMSException {
    try {
        process(message);
        message.acknowledge();          // explicit ack
    } catch (Exception e) {
        // no ack -> redelivery
    }
}
```

## 11. Output

```text
Received: Hello
Order 123 processed
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Closing connection per message | Terrible performance | Reuse via a pool/Spring |
| Forgetting to acknowledge | Redelivery loops | Use AUTO or ack explicitly |
| Blocking `receive()` in UI threads | Frozen app | Use listeners or async receive |
| Sending non-serializable objects | Errors | Use JSON/text or Serializable |

## 13. Best Practices

- Use Spring's `JmsTemplate` + `@JmsListener` for real apps.
- Make consumers idempotent (redeliveries happen).
- Use transactions for message + DB consistency.
- Configure a DLQ for poison messages.
- Use `ObjectMessage`/JSON serialization, not raw `BytesMessage` for business data.

## 14. Interview Questions

**Q: What is JMS?**
Java's standard messaging API supporting point-to-point queues and pub/sub topics.

**Q: What are the acknowledge modes?**
AUTO (ack on receipt), CLIENT (app acknowledges), DUPS_OK (lazy, may redeliver).

**Q: How does Spring simplify JMS?**
`JmsTemplate` for sending and `@JmsListener` for consuming, hiding connections/sessions/producers.

## 15. Practice Exercises

1. Send a text message with `JmsTemplate` and receive it with `@JmsListener`.
2. Switch between queue and topic destinations and note behavior.
3. Simulate a failing consumer and observe redelivery.

## 16. Coding Challenges

1. Build a send/receive flow that processes an `Order` object (JSON) end to end.
2. Add a DLQ: messages that fail N times land in a dead-letter queue.

## 17. Summary

- JMS standardizes Java messaging over queues and topics.
- Objects: ConnectionFactory → Connection → Session → Producer/Consumer.
- Ack modes control redelivery; prefer at-least-once + idempotency.
- Spring's JmsTemplate/@JmsListener simplify it.
- Next, the message broker RabbitMQ in **RabbitMQ.md**.