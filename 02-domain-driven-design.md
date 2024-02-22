<a href="http://www.reactivemanifesto.org/"> <img style="border: 0; position: fixed; right: 0; top:0; z-index: 9000" src="//d379ifj7s9wntv.cloudfront.net/reactivemanifesto/images/ribbons/we-are-reactive-black-right.png"> </a>
<br/><br/><br/>

# Reactive Architecture: Domain Driven Design

> Domain Driven Design is a technique commonly used to build Reactive Systems. This course will introduce the core elements of Domain Driven Design. It will also explain how those elements relate to Reactive Systems.

### Introduction

#### The book

Domain Driven Design originates from a book called _"Domain
Driven Design - tackling complexity in the heart of software - written by
Eric Evans"_. One of the key goals in the book is to try to create a
software implementation that is based on an evolving model that is understood by the domain experts.

#### The Goal

One of the key goals is to take a large system, or a large domain rather, and to break it into smaller and smaller pieces. Another goal is to build a model that the domain experts can understand.

Microservices, have a similar goal: they need to be separated along clear boundaries.

DDD give us a set of guidelines and a set of techniques that we can use to try to help us break larger domains into smaller domains. Because of that we can take that logic and apply it to microservices to come out with something similar.

#### Domains

A domain, by dictionary definition, is a sphere of knowledge. A specific area of knowledge. In the context of software, it refers to a business or idea that we are trying to model.

Experts in the domain are therefore people who understand that business or that idea not necessarily the software.

> _Note: The model is not the software. The model represents our understanding of the domain. The software is the implementation of the model._

#### Ubiquitous Language

Communication between developers and domain experts requires a common language. This language is what we call the ubiquitous language.

Terminology in the ubiquitous language comes from the domain experts.
- Words originate in the domain and are used in the software.
- Avoid taking software terms and introducing them into the domain.

Domain experts and software developers should be able to have a conversation without resorting to software terms.

### Decomposing the Domain

#### Large Domains

- Business domains are often large and complicated
- They contain many ideas, actions, and rules that interact in complex ways
- Trying to model a large domain can become problematic

#### Subdomains

Large domains may be separated into subdomains. Those subdomains are created by grouping related actions, ideas and rules.

Some concepts/models may exist in multiple subdomains
- shared concepts may not be identical initially
- they may also evolve differently
- avoid the temptation to abstract

<u>Each subdomain essentially ends up having its own ubiquitous language and model. The language and model for a sub domain is what we call a bounded context.</u>

#### Understanding bounded contexts

From one bounded context to the next the meaning of a word may change dramatically.

> e.g.: In a restaurant if we're talking to a server, the term order has a very specific meaning. When you talk to the server it means one thing (customers -> restaurant). However if you talk to somebody else in the restaurant, for example, the person who manages inventory, then order means something different (restaurant -> supplier). 

We also need to observe how the details of the model change.

> e.g.: In a restaurant, when the kitchen is preparing an Order they don't care about prices. But when a customer pays for the Order, price is critical. It's the same Order, but the relevant details of that Order change.

#### How to determine bounded contexts

<u>There is no universal answer, but there are some guidelines.</u>

- consider human culture and interaction.
    - different areas of the domain that are handled by different groups of people, suggests a natural division.
- Look for changes in Ubiquitous language
    - If the language or meaning of that language changes it may suggest a new context
- Look for varying on unnecessary information
    -  Employee ID is very important in an employee but it's meaningless in a customer
- Strongly separated bounded contexts will usually result in smooth workflows 
    - an awkward workflow may signal a misunderstanding of the domain
    - if a bounded context has too many dependencies it may be overcomplicated. You may have drawn your lines incorrectly. That's an area where you want to explore in a little more detail and try and figure out if you've done something wrong

#### Event First Domain Driven Design

Traditionally domain driven design focused on the objects within the domain.

> e.g: cook, reservation, customer, etc

In more recent times the techniques have evolved a little bit and now we talked a lot about <u>event first domain driven design which places the focus on the activities or events that occur in the domain</u>. Rather than looking at those objects we look at what is happening to those objects or what they are doing.

> e.g: "Customer makes a reservation", "Server places an order", "Food is served to the customer"

Using event first domain driven design we start by defining the activities, then group those activities to find logical system boundaries. The technique called "event storming" may help you to find those events.

##### Subject-Verb-Object Notation

The purpose of this notation is to give us a consistent way to phrase our activities or our events in the domain.

> e.g.: Hosts checks current reservations
>
> Our **subject** is "Host", our **verb** is "checks" and our **object** is "Reservation". We have the subject first, the object last, and then the verb in the middle.

The subject represents whoever is doing  a particular action/activity. The verb is the action being done. The object is the target of that action.

When defining these activities, it is important to think about how the business would operate in the absence of software. We want to be careful not to allow the implementation of the software, to leak into our domain model.

##### Direct vs Indirect Objects

When we're using Subject-Verb-Object notation sometimes we get into a
situation where it seems like maybe there's more than one object in the
sentence. 

> e.g.: Bartender collects Payment for a Drink Order

In that case the subjects clear, that's the bartender. The verb is collect but then we have payment for a drink order. Is payment the object? Or is the order the object? That's where it becomes unclear.

The truth is that they both represent objects in the sentence. There's what's called a **direct object** which is our payment. And there's an
**indirect object** which is our drink order.

#### Maintaining Purity

Once we've separated our bounded context into nice clean boundaries we have a bit of a job ahead of us which is maintaining those clear boundaries, maintaining the purity of those bounded contexts.

There are some techniques that allow us to do that.

##### Anti-Corruption Layer (ACL)

![](/images/02/acl.png)

- Each bounded context may have domain concepts that are unique
- Concepts are not always compatible from one context to the next
- Anti-Corruption Layers are introduced to translate these concepts
- An Anti-Corruption Layer will prevent Bounded Contexts from leaking into each other.
- Anti-Corruption Layer help the bounded context to stand alone


How is that anti-corruption layer implemented? A common way to implement it is as an abstract interface. The abstract interface represents sort of a pure domain representation of the data. Then we have implementation of that interface which does the necessary translation.

##### Anti-Corruption for Legacy System

![](/images/02/acl-legacy.png)

- Sometimes you have to interface with a legacy system
- The domain of legacy system may be messy or unclear
- An Anti-Corruption Layer keeps your bounded context pure. It prevents your domain from having to deal with that mess
- It prevents your domain from dealing with the mess og the legacy system
- Anti-Corruption Layer may be implemented in the Legacy System, or in the bounded context (or both)

###### Context Maps

![](/images/02/context-maps.png)

- Context Maps are a way of visualizing Bounded Contexts and the relationships between them
- Bounded Contexts are drawn as simple shapes
- Lines connect the Bounded COntexts to indicate relationships
- Lines may be labelled to indicate the nature of the relationship

### Domain Building Blocks

#### Domain Activities

##### Commands

A command is a type of activity that occurs in the domain. It represents a request to perform an action. The action has not yet happened and it can be rejected.

 Commands are typically delivered to a specific destination. And it causes a change to the state of the domain. After the command has been completed, the domain won't be in the same state that it was prior to issuing that command.

> e.g.: Add an item to an order, pay a bill, prepare a meal

Those are all requests to perform an action.

##### Events

Events are another activity in the domain. An event is an activity that occurs in the domain but now it represents an action that happened in the past. 

Because the action already completed, they can not be rejected. You can choose to ignore it. You can choose to do nothing with it. But you can't say that it never happened. 

Events are often broadcast to many  destinations.

An event records a change to the state of the domain. Often the result of a command.

> e.g.: An item was added to an order, A bill was paid, a meal was prepared

Events are always worded as past tense. You can see that it's an event.

Again the critical thing here is that events happened in the past. They are indisputable. You cannot argue with the
fact that it has already happened.

##### Queries

Queries are the final type of activity in the domain. They represent a request for information about the domain.

Because they are a query, a response is always expected. It's usually delivered to a specific destination.

Queries should not alter the state of the domain.

> e.g.: Ge the details of an order, check if a bill has been paid.

##### Commands, Events and Queries in reactive systems

![](/images/02/commands-evts-queries.png)

> Commands: Make a reservation
>
> Events: Reservation made
>
> Queries: Get reservation


In a reactive system, the commands, events and queries represent the messages.
 
They basically form the API of a bounded context or a microservice.

#### Ubiquitous Language To Code

As we begin to flesh out the bounded context, we're going to need to define activities for that context. When we define those activities, one of the things that we're going to want to do is we're gonna want to focus on the ubiquitous language. 

So we're going to use terms that you would find when speaking to the domain experts and we will express these activities.

However when we translate those activities into code we're gonna want to
maintain that same language.

For example, if we have an activity such as "Open an Order." This is a command that gets issued in the domain. When we translate that command into code we are going to name the class or object that represents the command using the ubiquitous language. So it would get translated into something like "OpenOrder."

"OpenOrder" is the name of the class that represents the "Open an Order" command that we found in the domain.

#### Domain Objects

##### Values Objects

![](/images/02/value-objects.png)

A value object is defined by its attributes. Two value objects are equivalent if their attributes are the same.

Value objects are immutable. In addition to state, value objects can contain business logic.

##### Entities

![](/images/02/entities.png)

An entity is defined by a unique identity(i.e. an ID or a key). It may change its attributes, its mutable, but not its identity.

If the identity changes, it is a new entity, regardless of its attributes.

Entities are the single source of truth for a particular id. Entities can also contain business logic.

##### Aggregates

![](/images/02/aggregates.png)

 An aggregate is a collection of domain objects bound to a root Entity. The root Entity is called the **Aggregate Root**.

 Objects in an aggregate can be treated as a single unit. The access to objects in the aggregate must go though the Aggregate Root.

 Transactions should not span multiple aggregate roots.

 ##### Determining the aggregate roots

 ![](/images/02/aggregates-01.png)

 Choosing an aggregate root is not always straightforward. The aggregate root can be different from on context to the next.

 Some context may require multiple aggregate roots. It's not common, it's far more common to see a single aggregate root per bounded context but it's not always the case.

 So some questions to consider:
 - Is the entity involved in most of the operations in that bounded
context? 
- If you delete the entity, does it require you to delete other entities?
- Will a single transaction span multiple entities?


#### Object-Field Notation

As we continue with our analysis of the Orders context, our next step is to determine what objects are present. This usually involves looking at the Commands, Events, and Queries that we defined previously and determining what information we will need in our Bounded Context order to execute them.

We will continue to move our representation of these things to look more like code. Our objects will be represented using the following notation:

_Object(fields)_

Based on this you will be able to see which fields make up an individual object. For example:

_Order(orderId, orderItems, tableNumber, serverId)_

Here you can see that our Order object consists of the fields orderId, orderItems, tableNumber, and serverId

We will represent more primitive objects by excluding their fields.

_Primitive_

For example we could have something like:

_SpecialInstructions_

This represents a primitive object that does not contain any special fields. In our code it would likely be represented by something like a string in this case.

#### Domain Abstractions

##### Services

![](/images/02/services.png)

Business logic doesn't always fit with an entity or value object. The logic can be encapsulated by a Service.

Services have some special criteria: They should be stateless. And services are
often used to abstract away something like an anti-corruption layer.

It's important to note that creating systems that use too many services, it leads to an anemic domain. Look for a missing domain object before resorting to a service. Make sure that we're not missing an entity or a value object. What we don't want is we don't want
services to be doing all the work. We want services to typically be fairly thin layers over a very specific piece of business logic rather than something that just does everything.

##### Factories

![](/images/02/factories.png)

When we go to create a new domain object, often entities or aggregate roots, the logic to construct those domain objects may not be trivial. May require access to external resources (databases, files, rest api, etc).

A factory allows us to abstract away that logic. It's again usually implemented as a domain interface with one or more concrete implementations.

##### Repositories

![](/images/02/repositories.png)

Repositories are similar to factories but instead of abstracting away creation they abstract away the retrieving of existing objects. Factories are used to get new objects. Repositories are used to get or modify existing objects. 

They often operate as abstraction layers over top of databases but they can also work with files, REST API, etc.

> Note: A repository does not automatically imply a database.

Factories and repositories are related. For this reason, they are often combined. A repository may end up with all of the Create, Read, Update, Delete operations. We don't
bother with factories in that case.


#### Hexagonal Architecture

![](/images/02/hexagonal-architecture.png)

A particular technique that is often combined with domain-driven design is called hexagonal architecture. Hexagonal architecture is not directly related to domain driven design. You can use domain driven design without using hexagonal architecture however it is very compatible with domain driven design. 

It's also known as ports and adapters. It was the idea of ports and adapters and the
hexagonal architecture was proposed by Alistair Cockburn. It's an alternative to the layered or n-tier architecture.

The the idea here is that the domain is core. It is absolutely the most important thing in your application therefore it should be at the center. It becomes the architectural focus.

Ports are exposed as an API for the domain. Infrastructure contains adapters that map to the ports.  And those adapters communicate with those ports, they communicate through
that API. 

![](/images/02/hexagonal-architecture-2.png)

Here we've kind of combined the onion and the hexagon into a drawing. You see the domain at the center. Outside of that domain we have another layer which is the API, sometimes called the services layer. Then outside of that we have an infrastructure layer. 

The idea here is the domain is the center of the onion, the API provides the ports and it's another layer, and then the infrastructure provides the adaptors which communicate (in and out) with the ports. 

The outer layers are allowed to depend on the inner layers but the reverse is not true. Inner layers have no knowledge of outer layers.

Hexagonal architecture ensures a proper separation of infrastructure from domain. You can guarantee that the domain has no knowledge of the infrastructure. It doesn't know about the database. It doesn't know about the user interface. It doesn't know about any of those things because it's not allowed. Those rules prevent that so it prevents the concerns about databases, user interfaces, things like that, from bleeding into the domain.

This can be enforced with packages, or even project structure in the application.

What this does is it allows your domain to be portable. It means that it becomes
much more easy to swap out pieces of your infrastructure without having to affect your domain.


#### Domain Driven Design & Hexagonal Architecture Links

- [Domain Driven Design: Tackling Complexity in the Heart of Software](https://domainlanguage.com/ddd/)
- [DDD Reference](http://domainlanguage.com/ddd/reference/)
- [Implementing Domain Driven Design](http://www.informit.com/store/implementing-domain-driven-design-9780133039894)
- [Domain Driven Design Distilled](http://www.informit.com/store/domain-driven-design-distilled-9780134434988)
- [Hexagonal Architecture](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))