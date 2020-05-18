# Domain Entities and Value Objects and Aggregates, Oh My!

Working in a DDD ([domain-driven design](https://domainlanguage.com/ddd/)) codebase has taught me many a lesson about these three concepts:
- A *domain entity* is an object that has a distinct identity and exists through time (or as described by Eric Evans, "has a thread of continuity").
- A *value object* is an objects that has no identity and whose equality is determined by the equality of its attributes (not a distinct id).
    - Simply put (and taken from [this Microsoft article](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/microservice-domain-model)), "You care about _what_ they are, not _who_ they are".
    - It should be *immutable* and in order to change a property value, you should re-construct a new instance of the value object or else you may risk incurring side effects by changing property values of that object.
- An *aggregate root* is a cluster of domain objects that can be treated as a single unit.
    - Any changes you want to make to its entities should be made through the aggregate root. This way, you can ensure the states of your aggregate and its entities are always valid.

***

#### Example

Taken from the aforementioned Microsoft article, consider an e-commerce website that takes orders.
- `Order`s have `OrderItem`s that hold information about the item... in that order.
- `Order`s also have associated addresses, each of which cannot exist outside of an order (say this website doesn't store addresses outside of orders).

Microsoft has a [clear visual](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/microservice-domain-model#the-aggregate-root-or-root-entity-pattern) describing this example.

Your code might look something like:
```
public class Order // Aggregate Root
{
    public List<OrderItem> Items { get; private set; } // See note below
    public Address OrderAddress { get; private set; }
}

public class OrderItem	// Domain Entity
{
    public int Id { get; private set; }
    public int Quantity { get; private set; }
    public decimal Price { get; private set; }
}

public class Address // Value Object
{
    public string Street { get; private set; }
    public string City { get; private set; }
}
```
Note, this is not proper usage of DDD - the public getter `List<OrderItem> Items` allows for external manipulation of this list. Will explain in a future TIL.

***

Sources
- [Evans Classification by Martin Fowler](https://martinfowler.com/bliki/EvansClassification.htmlhttps://martinfowler.com/bliki/EvansClassification.html)
- [DDD Aggregate by Martin Fowler](https://martinfowler.com/bliki/DDD_Aggregate.html)
- [Implementing Value Objects by Microsoft](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/implement-value-objects)

