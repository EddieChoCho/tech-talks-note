# ChoCho's Bizarre Adventure of Event-Sourcing
A leisure study journey from 2017 to 2019

### Speaker: Eddie Cho

### Why I like to giving this talk
* The gap between concept and practices
    * Which are the good examples/documents I could follow?
        * The example/documents provided by experts are not in Java...
        * The famous projects in Java are frameworks which I am not familiar with.
        * Study many things at the same time is not effective.         
* Could not find a good example of Java + Spring + RDMBS
* Fragmented study time
* I spent much more time than I thought to fill the gap.

### Core Concepts of Event Sourcing
#### What is Event Sourcing [1]
* Event 
    * An event is something that has happened in the past.
    * Conceptually, it should be Immutable.
* Event Sourcing
    * Storing facts.
    * Reading information from the facts for different perceptions.
    
* The Issues of CRUD
    * The history could be lost
    * Writing logs is the logic which different from the major business logic.
        * Could not have single source of truth
    
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

#### Issues could be solved by Event Sourcing [1]
* No audit log issue, Information would not be lost.
* Structure changes more often than behavior  => we could update the read model easily without migration script.
* Single source of truth
* Allows you to time travel (Historic states)
* Easier to analyze and fix bugs.
* Could keep all data for analysis

#### Different types of Event Store
* Eventstore.org
    * TBD 
* NoSQL Database
    * TBD
* Message Queue
    * TBD
* RDBMS
    * Might not be the bast choice 
* Other options    

### Event Sourcing in Practice
#### Event Store Design
* RDBMS Design 1: Op lock with version column [4]
    * TBD
* RDBMS Design 2: Id + parent unique constrain
    * TBD
* RDBMS Design 2: Id + parent unique constrain
    * TBD
##### Questions to Ask When We Design an Event Store
* What is the size of your domain?
* What is the existed design of your system?
* Where does new events come from?
* How often will events be created?
    
#### Snapshot 
* Snapshots should not be places in line in the Event Log.[1]
* Snapshots should have their own table.[4]
* Memento pattern better insulates the domain over time as the structure of the domain objects change.[4]
* Frequency of creating snapshots. <= Depends on your requirements[3]
* You might not need it

#### CQRS [1,2]
* CQRS is required when we implement Event Sourcing.[1]
* Event Sourcing is not required when we implement CQRS.[1]


### Microservice + Event Sourcing
* TBD
#### Microservice + Event Sourcing: Async Communication [5,6]
#### Microservice + Event Sourcing: CQRS [5,6,8]
#### Microservice + Event Sourcing: Transaction [8]
* Monolithic service + RDBMS
    * ACID, 2PC 
* Saga Pattern
    * Avoid Distributed Transaction
    * event driven + local transaction
    * 
    

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
