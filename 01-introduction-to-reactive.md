<a href="http://www.reactivemanifesto.org/"> <img style="border: 0; position: fixed; right: 0; top:0; z-index: 9000" src="//d379ifj7s9wntv.cloudfront.net/reactivemanifesto/images/ribbons/we-are-reactive-black-right.png"> </a>
<br/><br/><br/>

# Reactive Architecture: Introduction to Reactive Systems

> This course teaches the core principles behind Reactive Architecture. It introduces learners to why we need Reactive Systems, and what problems they are trying to solve. It also contrasts Reactive Architectures with Reactive Programming, showing how they relate, and how they are different.

### The Reactive Manifesto

[The Reactive Manifesto](Thttps://reactivemanifesto.org/) was Jonas Bonér, Dave Farley, Roland Kuhn, and Martin Thompson.

- Created in response to companies trying to cope with changes in software landscape
- Similar patterns for solving similar problems.
- <u>The reactive manifesto tries to put these common ideias into a single set of principles</u>


### The reactive principles [🔗](https://www.reactiveprinciples.org/)

![Image ilustrating reactive principles. Responsive, Resilient, Elastic and Message Driven](/images/01/reactive-traits.svg 'Image ilustrating reactive principles. Responsive, Resilient, Elastic and Message Driven')


#### Responsive

> A reactive system consistently responds in a timely fashion.

- Responsiveness is the cornerstone of usability
- Systems must respond in a fast and consistent fashion if at all possible
- Responsive systems build user confidence

#### Resilient

> A reactive system remains responsive, even when failures occur.

- Resilience provides responsiveness, despite failures
- Achieved thought replication, isolation, containment, delegation
    - Replication: means multiple copies
    - Isolation: means services can function on their own. They don't have external dependencies
    - Containment: If a failure occurs it's not propagated to because it is isolated
    - Delegation: recovery is managed by an external component
- Failures are isolated to a single component
- Recovery is delegated to an external component 

#### Elastic

> A reactive system remains responsive, despite changes to system load.

- Elasticity provides responsiveness, despite increases (or decreases) in load
- Implies zero contention and no central bottlenecks
- Predictive auto scaling techniques can be used to support elasticity
- Scaling up provides responsiveness during peak, while scaling down improves cost effectiveness


#### Message Driven

> A reactive system is built on a foundation of async, non-blocking messages.

- Responsiveness, Resilience, Elasticity are supported by a Message Driven Architecture
- Message are asynchronous and non-blocking
- Provides Loose coupling, isolation, location transparency
- Resources are consumed only while active

