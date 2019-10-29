# ChoCho's Bizarre Adventure of Event-Sourcing
A leisure study journey from 2017 to 2019

### Speaker: Eddie Cho

### Why I like to giving this talk
* Fragmented study time
* The gap between concept and practices
    * Which are the good examples/documents let I could follow?
        * The example/documents provided by exports are not in Java...
        * The famous projects in Java are frameworks which I am not familiar with.
        * Study many things at the same time is not effective.         
* Could not find a good example of Java + Spring + RDMBS
* I spent much more time than I thought to fill the gap.

### Core Concepts of Event Sourcing
#### What is Event Sourcing
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

#### Issues could be solved by Event Sourcing
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
* RDBMS Design 1: Op lock with version column
    * TBD
* RDBMS Design 2: Id + parent unique constrain
    * TBD
#### Snapshot 
* TBD
### Microservice + Event Sourcing
* TBD
