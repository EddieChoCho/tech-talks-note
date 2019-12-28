# Java Streams vs Reactive Streams: Which, When, How, and Why? 
### Speaker: Venkat Subramaniam

## Introducing Java Stream
### Why methods are moved from Collections to Streams?
* Lazy evaluation
    * Java(old): imperative + OOP
    * Java(today): imperative + Functional + OOP
    * imperative  -> tell me what and how
    * declarative -> tell me what but not how.
    * functional: declarative + higher-order function
    * external iteration v.s. internal iteration

* Before java 8, the structure of sequential code was very different from the structure of concurrent code.
* With Stream, the structure of sequential code is the same as the structure of concurrent code.(e.g. parallelStream(), parallel())

* What is functional programming(from Venkat Subramaniam's point of view): functional composition + lazy evaluation

* Stream is not d data structure, it is an abstraction of functions.
    * Bucket(List/Set) v.s. Pipeline(Stream)

* Limitations of Java Steam
    * Single use only(IllegalStateException: stream has already been operated upon or closed.)
    * Single pipeline - a single terminal operation
    * How to handle exceptions in stream -> good luck
	    * Scala and Haskell has interfaces to handle exceptions

## Reactive programming
* Elastic
	* How many threads can you create? (Bad questions) 
	* How many threads should you create?
		* computation intensive tasks: <= number of cores
		* IO intensive tasks: <= "number of cores" / "1-blocking factor", 0 <= blocking factor < 1
* Message-driven
	* Do not expose your database instead export your data.
* Responsive 
	* Infinite-scrolling
* Resilience
	* Handle Failure gracefully -> Failure is a first-class citizen

### Dataflow Computing

* e.g. Serverless
* It builds on functional composition and lazy evaluation and takes the abstraction event further.

### Java Stream v.s. Reactive Stream

| Java Stream                   | Reactive Stream                                                     | 
| ----------------------------- | ------------------------------------------------------------------- | 
| Pipeline                      | Pipeline                                                            | 
| Push data                     | Push data                                                           | 
| Lazy evaluation               | Lazy evaluation                                                     | 
| Zero, one or more data        | Zero, one or more data                                              | 
| One channel for data          | 3 channel(data, error, complete)                                    | 
| Hard to handle the exceptions | Error is like data, it will be deal with downstream(circuit breaks) | 
| Sequential v.s. Parallel      | Sync v.s. Async, Nonblocking backpressure                           |
| Single pipeline               | Multiple subscribers(share or not)                                  |

| Reactive Stream API | Java 9 Reactive Stream API |
| ------------------- | -------------------------- |
| Publisher           | Publisher                  |
| Subscriber          | Subscriber                 |
| Subscription        | Subscription               |
| Processor           | Processor                  |

## References
* [Java Streams vs Reactive Streams: Which, When, How, and Why? by Venkat Subramaniam](https://youtu.be/kG2SEcl1aMM)
