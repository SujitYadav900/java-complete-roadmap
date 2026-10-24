# RabbitMQ

## 1. Introduction

**RabbitMQ** is a battle-tested message broker implementing AMQP. Producers publish to **exchanges**; exchanges route to **queues** via **bindings**; consumers read from queues. Its flexible routing makes it ideal for work queues, pub/sub, and RPC patterns.

## 2. Why This Topic Is Important

- RabbitMQ is the most common broker in Java shops and interview questions.
- Its exchange/queue model appears across messaging platforms.
- Spring AMQP integrates it beautifully with `RabbitTemplate` and `@RabbitListener`.

## 3. Real-World Analogy

RabbitMQ is like a **post office sorting room**. The exchange is the mail sorter: letters arrive (publish) and the sorter decides which delivery routes (queues) to drop them into, based on the routing key (the address). Couriers (consumers) empty each route's mailbag.

## 4. Theory

### Components

| Component | Role |
|-----------|------|
| **Producer** | Publishes to an exchange |
| **Exchange** | Routes messages using rules |
| **Binding** | Links an exchange to a queue |
| **Queue** | Stores messages for consumers |
| **Consumer** | Reads and processes queue messages |
| **DLQ** | Dead-letter queue for failed messages |

### Exchange types

| Type | Routing |
|------|---------|
| **Direct** | Routes by exact routing key |
| **Topic** | Routes by pattern (`order.*`, `order.created`) |
| **Fanout** | Broadcasts to all bound queues |
| **Headers** | Routes by header values |

### Spring AMQP

- `spring-boot-starter-amqp` dependency.
- `RabbitTemplate.convertAndSend(exchange, routingKey, message)`.
- `@RabbitListener(queues = "...")` consumes.

## 5. Syntax

```java
// Producer
rabbitTemplate.convertAndSend("orders.exchange", "order.created", order);

// Consumer
@RabbitListener(queues = "orders.queue")
public void onOrder(Order order) {
    inventoryService.reserve(order);
}
```

```yaml
# application.yml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```

## 6. Flow Explanation

1. Producer publishes to an exchange with a routing key.
2. The exchange applies its type + bindings to route the message.
3. The bound queue stores it until a consumer is ready.
4. The consumer receives and acknowledges it.
5. Without an ack, RabbitMQ redelivers.

## 7. Memory Representation

```
producer ─▶ orders.exchange (topic)
                ├─ binding "order.created" ─▶ orders.queue ─▶ inventory consumer
                └─ binding "order.#"        ─▶ analytics.queue ─▶ analytics consumer
```

## 8. Simple Example

```java
rabbitTemplate.convertAndSend("default", "my.queue", "Hello Rabbit");
```

```java
@RabbitListener(queues = "my.queue")
public void listen(String msg) {
    System.out.println("Got: " + msg);
}
```

## 9. Intermediate Example

```java
@Configuration
public class RabbitConfig {

    @Bean Queue ordersQueue() {
        return new Queue("orders.queue", true);   // durable
    }

    @Bean TopicExchange ordersExchange() {
        return new TopicExchange("orders.exchange");
    }

    @Bean Binding binding() {
        return BindingBuilder.bind(ordersQueue())
                .to(ordersExchange())
                .with("order.*");
    }
}
```

## 10. Advanced Example

```java
// Retry + DLQ
@RabbitListener(queues = "orders.queue")
public void onOrder(Order order) {
    try {
        process(order);
    } catch (Exception e) {
        throw new AmqpRejectAndDontRequeueException(e);  // to DLQ after retries
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
        default-requeue-rejected: false
```

## 11. Output

```text
Consumed order 1 from orders.queue
Consumed order 2 from orders.queue
Failed order 3 after 3 attempts -> dead.letter.queue
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Publishing to a queue name directly | Misses exchange routing | Publish to an exchange |
| Non-durable queues | Messages lost on restart | Durable queues/exchanges |
| No DLQ | Poison messages block the queue | Configure a DLQ |
| Long-running consumers | Consumers pile up | Use concurrency/prefetch |

## 13. Best Practices

- Design exchanges/queues explicitly (don't rely on defaults).
- Use topic exchanges for flexible routing.
- Configure retries with max attempts, then a DLQ.
- Set a sensible prefetch count for consumers.
- Monitor queue depth and consumer lag.

## 14. Interview Questions

**Q: What is the difference between an exchange and a queue?**
An exchange routes messages (no storage); a queue stores messages for consumers.

**Q: What are the exchange types?**
Direct (exact key), topic (pattern), fanout (broadcast), headers.

**Q: How do you prevent message loss in RabbitMQ?**
Durable queues + `persistent` delivery mode + publisher confirms + consumer acks.

## 15. Practice Exercises

1. Set up RabbitMQ (Docker) and send/receive with Spring AMQP.
2. Create a topic exchange with two bindings and route different keys to different queues.
3. Add retry + a DLQ and observe a failing message.

## 16. Coding Challenges

1. Build a work queue: 10 messages, 2 concurrent consumers, each message consumed once.
2. Implement fanout: one message broadcast to three different queues.

## 17. Summary

- RabbitMQ routes via exchanges → bindings → queues.
- Exchange types: direct, topic, fanout, headers.
- Use Spring AMQP: `RabbitTemplate` + `@RabbitListener`.
- Retries + DLQ keep pipelines robust.
- Next, event streaming with **Kafka.md**.