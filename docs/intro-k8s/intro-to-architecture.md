# Intro to Architecture

## Monolithic architecture

Monolithic architecture is an architecture where multiple components or features with low cohesion is build together into a single platform.

### Three Tier Architecture

The three tier architecture is one of the most common architecture around, test and proven to work.

In three tier architecture, we can split the software into 3 different tier, presentational, Application and Data layer.

The Presentational tier is what you see. Typically HTML, CSS and Javascript in a browser.

The application tier will be a server that does the CRUD operation and also handles business logics hidden from the user.

Data tier will be a DBMS that stores persisted data such as MongoDB, PostgresSQL, MySQL.

This Architecture have historically work pretty well, allow separation of individual technology which allows changing or upgrading individual tier without affecting the other as long as you can keep the interface.

### Advantage

- Simplicity, everything is in one code base, there is no confusion of where the code is.
- Group people with same strength to work together
- Code in each tier works independent of each other, we can deploy each tier individually
- Simple authentication architecture with JWT and Cookie

### Monolithic Hell

There is a lot of advantage to go with a monolithic architecture. Overall, I feel this as is a good architecture to start with.

With Monolithic application, the growth and increase of functionality result in a large code based, complexity, and intertwining of logic. This make each increment harder and harder till we might reach a point that adding any additional feature going to become very challenging.

Say we have a shop. We have a collection of inventory and a collection or purchase system. The purchase system will have to check if there are available inventory, which means the purchase system will have to reach out to the inventory db and get the information. If we want to upgrade the inventory system, any change could also break the purchase system. This is an example of 2 system. In many enterprises, it will involve multiple systems to make a business logic happen, one system might need to reach out to multiple collections/tables before can handle a business logic. This intertwining gives rise to insane complexity, making development difficult and time consuming and prone to bugs.

### Disadvantage of Monolithic

- Complex and slow development
- To scale the DB, say lots of ppl doing browsing of items and purchase and shipping require a lot less computing power, we must scale the whole application but not part of it
- If one system cause an error, the entire system might go down.

## Service-Oriented Architecture(SOA)

> Service-oriented architecture (SOA) is a style of software design where services are provided to the other components by application components, through a communication protocol over a network - [Wikipedia](<https://en.wikipedia.org/wiki/Service-oriented_architecture#:~:text=Service%2Doriented%20architecture%20(SOA),communication%20protocol%20over%20a%20network.>)

SOA aims to break a monolithic software into smaller services that each handle a smaller business domain. One application will work with multiple system.

SOA had many push back because of its initial choice of protocols and dependencies. Using SOAP requires XML and fix data schema which cause tight coupling of system. It also doesn't specify how we should separate database and model. We can still share data model and databases.

> Many of the problems laid at the door of SOA are actually problems with things like communication protocols (e.g., SOAP), vendor middleware, a lack of guidance about service granularity, or the wrong guidance on picking places to split your system. A cynic might suggest that vendors co-opted (and in some cases drove) the SOA movement as a way to sell more products, and those selfsame products in the end undermined the goal of SOA. - Sam Newman

## Microservices

Micro-services is a way of implementing SOA.

One of the most notable company that brings early success using Micro-services is Amazon.

```text
1. All teams will henceforth expose their data and functionality through service interfaces.
2. Teams must communicate with each other through these interfaces.
3. There will be no other form of interprocess communication allowed: no direct linking, no direct reads of another team’s data store, no shared-memory model, no back-doors whatsoever. The only communication allowed is via service interface calls over the network.
4. It doesn’t matter what technology they use. HTTP, Corba, Pubsub, custom protocols — doesn’t matter.
5. All service interfaces, without exception, must be designed from the ground up to be externalizable. That is to say, the team must plan and design to be able to expose the interface to developers in the outside world. No exceptions.
6. Anyone who doesn’t do this will be fired.
7. Thank you; have a nice day!
- mandate by Jeff Bezos in 2002
```

### Anatomy of a micro-services

- Each service should be small, allow deployment to happen individually

- Each Service can be scale individually
- Every service should represent a business domain
- Each service should have its individual database. Only the service access its database.

### Advantages

- Small maintainable services: code change and onboarding new member will also be easy
- Fault tolerance: If a service goes down, other services can still other services to function.
- Ease of adding new services: adding, replacing, upgrading a small isolated service is a lot easier.
- Fast: a smaller code means test run faster, code likely to run faster, time to debug any issues will also be faster

### Disadvantages

The major disadvantage of micro-services comes with the complexity

- Complex setup, Docker, k8s, Kafka, RabbitMQ, ...
  - Complex communication: What happen if a service breaks down. How can a service retrieve missed messages if the service was down?
- Service Discovery and Modeling: too many services might cause the service to get lost, is also hard to add any additional feature. Should the feature belongs to which existing service or new service can sometimes be hard to decide.
- Authentication: separation of services might require a more complex Authentication strategy

## Conway's Law

> Any organization that designs a system will produce a design whose structure is a copy of the organization's communication structure. -  Melvin E. Conway

In a long run, a team work will most likely be effective if they structure their code based on the structure of the team.

In a three tier architecture, we split Frontend Developers, Backend Developers and DBA then (Frontend) <=interface=> (Backend) <=interface=> (Database).

In micro-services, it will probably make sense for us to group our team into cross-functional team with each team handling a particular business domain.

## Containers

While is not required to use containers for micro-services but given the complexity involve in managing every service’s environment for difference services, using container gels very well with micro-services.

## Kubernetes

One of the best ways to manage and scale containers is using Kubernetes. Kubernetes can help us scale, load balance, add and remove new services and also bring up services if it fails.
