# ChoCho's Bizarre Adventure of Event-Sourcing
A leisure study journey from 2017 to 2019

### Speaker: Eddie Cho
2019/11/12

## Why I like to give this talk
* The gap between concept and practices
    * Which are the good examples/documents I could follow?
        * The example/documents provided by experts are not in Java...
        * The famous projects in Java are frameworks which I am not familiar with.
        * Study many things at the same time is not effective.         
* Could not find a good example of Java + Spring + RDMBS
* Fragmented study time
* I spent much more time than I thought to fill the gap.

## Core Concepts of Event Sourcing
### What is Event Sourcing [1]
* Event 
    * An event is something that has happened in the past.
    * Conceptually, it should be Immutable.
* Event Sourcing
    * Storing facts.
    * Reading information from the facts for different perceptions.
* Event Stream
    * An Event Stream is a sequence of events of the same aggregate.
    * Each event is a fact that had happened, so we do not need extra code to write logs.
    * Could trace the history
* Event Store
    * The database for storing events.
* Projection
    * The code goes over a series of events and produces some format(Read Model) of transient state of it.
    * System could have various projection logic with various read model.
    * Allows you to time travel.
    * If the projection process is very time consuming, caching read model might be a good idea.
    * Events are still the single source of truth of the system.
    
#### The Issues of CRUD
* The history could be lost
* Writing logs logic is different from the major business logic.
* Logs + data -> Could not have single source of truth

#### Issues could be solved by Event Sourcing [1]
* No audit log issue, Information would not be lost.
* Structure changes more often than behavior  => we could update the read model easily without migration script.
* Single source of truth
* Allows you to time travel (Historic states)
* Easier to analyze and fix bugs.
* Could keep all data for analysis    

### Event Sourcing in Practice
#### How to Define Events
* Event Storming  (DDD)

#### Different types of Event Store
* Eventstore.org
* NoSQL Database (e.g. MongoDB) [12,13]
    * Flexible data model 
    * Not the best option for apps with complex transactions, but we only need the read and create operations for event sourcing.
* Message Queue
* RDBMS
    * Might not be the bast choice 
* Other options

#### Event Store Designs (RDBMS)
* Design 1: Optimistic lock with version column [4]
    * Aggregate Table							

    |  Column Name  | Column Type |
    | --------------|-------------|
    | Id (PK)       | bigint      |
    | Type          | varchar     |
    | Version       | bigint      |

    * Event Table
    
    |  Column Name  | Column Type |
    | --------------|-------------|
    | Aggregste (FK)| bigint      | 
    | Content       | blob(json)  |
    | Version       | bigint      |

* Design 2: Id + parent unique constrain
    * Event Table
    
    |  Column Name  | Column Type |
    | --------------|-------------|
    | Id (PK)       | bigint      | 
    | Content       | blob(json)  |
    | Parent (FK)   | bigint      |
    
* Design 3: Use normal columns instead of json 

##### Questions to Ask When We Want to Design/Choose an Event Store
* What is the size of your domain? => Json v.s. Columns
* What is the existed design of your system? => RDBMS, NoSQL... 
* Where does those event come from? => Need Optimistic lock?
* How often will events be created?  => RDBMS, NoSQL... 
    
#### Snapshot 
* Snapshots should not be places in line in the Event Log.[1]
* Snapshots should have their own table.[4]
* Memento pattern better insulates the domain over time as the structure of the domain objects change.[4]
* Frequency of creating snapshots. <= Depends on your requirements[3]
* You might not need it

#### CQRS [1,2]
* CQS (Command Query Separation - by Bertrand Meyer)
    * Command: void return type method, it is allowed to mutate state 
    * Query: non-void return type. it is not allowed to mutate state
* CQRS (Command Query Responsibility Segregation)
	* Command Component 
	* Query Component
	
* Queries are very easy to scale because almost all queries can be eventually consistent.
* CQRS is required when we implement event sourcing.
* Event sourcing is not required when we implement CQRS.

### Event Sourcing + Microservice

* Caution! These patterns are fit to be implemented with event sourcing. But they also could be implemented isolatedly.
* Patterns
    * Event-Driven Async Communication
    * CQRS
    * Choreography-based Saga Pattern

#### Event Sourcing + Microservice: Async Communication [5,6]
* Issues of communications with services through http request
    * Tight coupling, services need to know each other, need know how to communicate with each other.
    * Resilience Issue, multiple points of failures, if one of the services is crashed, user could not make the request anymore.
    * The more services were want through by the request, the longer response time will be.

* Event-Driven [11]
    * A system(or component) sends event messages to notify other systems(or component) of a change in its domain.
    
* Event-Driven + Asynchronous Communication + Server Push
    * Shorter response time could be gained by separating the command and the query to two request.
    * Loose coupling, services do not communicate to each other, they just need to communicate with the event bus(message queue).  
    * Resilience, user can still make requests if some of the services are crashed(not the entry one).
    * Server push could provide better user experience without wasting resources.  
    
#### Event Sourcing + Microservice: CQRS [5,6,8,10]
* CQRS allows you to separate the load from reads and writes allowing you to scale each independently.[10]

#### Event Sourcing + Microservice: Transaction [8]
* Monolithic service + RDBMS
    * ACID
* Distribute transaction, Two-phase commit protocol
    * Synchronous communication 
    * Resilience issue, Multiple points of failures.
    * Response time issue
* Saga Pattern
    * Avoid Distributed Transaction
    * Event driven + local transactions
    * Event sourcing -> no need for rollback, just add an event to the event store atomically.

### References
* [1][Greg Young - CQRS and Event Sourcing](https://youtu.be/JHGkaShoyNs)
* [2][The Many Meanings of Event-Driven Architecture • Martin Fowler](https://youtu.be/STKCRSUsyP0)
* [3][Event Sourcing You are doing it wrong by David Schmitz](https://youtu.be/GzrZworHpIk)
* [4][Building an Event Storage - CQRS](https://cqrs.wordpress.com/documents/building-event-storage/)
* [5][When Microservices Meet Event Sourcing](https://youtu.be/cISNDnwlSgw)
* [6][Sebastian Daschner: Video Course — Event Sourcing, Distributed Systems & CQRS](https://blog.sebastian-daschner.com/entries/event_sourcing_cqrs_video_course)
* [7][Event Sourcing in Practice - Benjamin Reitzammer & Johannes Seitz]()
* [8][Microservice Patterns - Saga Pattern by Chris Richardson](https://ookami86.github.io/event-sourcing-in-practice/)
* [9][Episode 370: Chris Richardson on Microservice Patterns](https://www.se-radio.net/2019/06/episode-370-chris-richardson-on-microservice-patterns/)
* [10][CQRS - Martin Fowler](https://martinfowler.com/bliki/CQRS.html)
* [11][What do you mean by “Event-Driven”? - Martin Fowler](https://martinfowler.com/articles/201701-event-driven.html)
* [12][MongoDB and MySQL Compared](https://www.mongodb.com/compare/mongodb-mysql)
* [13][MongoDB vs MySQL Comparison: Which Database is Better?](https://hackernoon.com/mongodb-vs-mysql-comparison-which-database-is-better-e714b699c38b)
* [14][Event Store](https://eventstore.org/)
* [15][Greg Young — A Decade of DDD, CQRS, Event Sourcing](https://youtu.be/LDW0QWie21s)