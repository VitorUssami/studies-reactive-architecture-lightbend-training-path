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

#### Subject-Verb-Object Notation

The purpose of this notation is to give us a consistent way to phrase our activities or our events in the domain.

> e.g.: Hosts checks current reservations
>
> Our **subject** is "Host", our **verb** is "checks" and our **object** is "Reservation". We have the subject first, the object last, and then the verb in the middle.

The subject represents whoever is doing  a particular action/activity. The verb is the action being done. The object is the target of that action.

When defining these activities, it is important to think about how the business would operate in the absence of software. We want to be careful not to allow the implementation of the software, to leak into our domain model.


### Maintaining Purity

Once we've separated our bounded context into nice clean boundaries we have a bit of a job ahead of us which is maintaining those clear boundaries, maintaining the purity of those bounded contexts.

There are some techniques that allow us to do that.

#### Anti-Corruption Layer (ACL)

![](/images/02/acl.png)

- Each bounded context may have domain concepts that are unique
- Concepts are not always compatible from one context to the next
- Anti-Corruption Layers are introduced to translate these concepts
- An Anti-Corruption Layer will prevent Bounded Contexts from leaking into each other.
- Anti-Corruption Layer help the bounded context to stand alone


How is that anti-corruption layer implemented? A common way to implement it is as an abstract interface. The abstract interface represents sort of a pure domain representation of the data. Then we have implementation of that interface which does the necessary translation.

#### Anti-Corruption for Legacy System

![](/images/02/acl-legacy.png)

- Sometimes you have to interface with a legacy system
- The domain of legacy system may be messy or unclear
- An Anti-Corruption Layer keeps your bounded context pure. It prevents your domain from having to deal with that mess
- It prevents your domain from dealing with the mess og the legacy system
- Anti-Corruption Layer may be implemented in the Legacy System, or in the bounded context (or both)

#### Context Maps

![](/images/02/context-maps.png)

- Context Maps are a way of visualizing Bounded Contexts and the relationships between them
- Bounded Contexts are drawn as simple shapes
- Lines connect the Bounded COntexts to indicate relationships
- Lines may be labelled to indicate the nature of the relationship