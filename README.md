
# 1. Routing exercise

The context of this exercise is a fictional messaging system.

Consumers can subscribe to messages of a given type.
When a message type is routable, the consumers can also subscribe to a subset of the messages, identified by a `ContentPattern`.

A `MessageRoutingContent` can be generated for every routable message. This object contains the routable components of the message.

If a consumer subscribes using the pattern `{ "A", "*" }`, it will receive the messages `{ "A", "100" }` and `{ "A", "200" }`, but not `{ "B", "100" }`.

The `MessageRouter` is used to identity the consumers for a specified message.

The messaging system is expected to handle 1000+ consumers, 5000+ subscriptions per consumer and up to 2000+ subscriptions per consumer for a given message type.

## A. Implement SubscriptionIndex

- Make `MessageRouterTests` tests pass
- Add missing tests
- Analyze and try to improve `MessageRouter.GetConsumers` performance
    - Document your approach and the tools you used
    - Include screenshots of your analysis or your intermediate steps if appropriate

## B. Replace IRoutableMessage by attributes

- Remove IRoutableMessage and replace it with two attributes:
    - `[RoutableMessage]` to identify routables messages
    - `[RoutableContent(X)]` to the identify the properties that are part of the routing content
- Update the existing routable messages
- Make existing tests pass

For example the `PriceUpdated` message should be changed to:
```cs
[RoutableMessage]
public class PriceUpdated : IMessage
{
    [RoutableContent(1)] // First part of the routable content
    public string ExchangeCode { get; set; }
    [RoutableContent(2)] // Second part of the routable content
    public string Symbol { get; set; }
    public double Value { get; set; }
}
```

## C. Implement MessageQueue

- Make `MessageQueueTests` tests pass
- Make `MessageQueue` thread safe
