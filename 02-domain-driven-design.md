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