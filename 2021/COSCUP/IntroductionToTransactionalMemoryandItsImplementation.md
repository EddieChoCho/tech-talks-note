# Introduction to Transactional Memory and Its Implementation
### Speaker: Mars Cheng
2021/07/31

## Why Transactional Memory?
* To do concurrent programming with threads, synchronization is necessary
	* Lock
		* lock simple in the beginning, but becomes frustrating in the end
	* Lock-less with atomic instructions
		* looks powerful in the beginning, but realizes its difficulty very soon
		* convoluted atomic instructions mixed with normal logic
		* hard to make sure the correctness if you are not an expert
	* transactional memory
		* looks very simple and powerful in the beginning, but need more experience
		* x64/ARM have supported TM for a while, not to mention software solutions
		* More and more real world applications using this tech, give it a try!!

## Weakness of Locks
* Deadlock
* Priority inversion
* High contention on non-partitionable data structures
* Blocking when acquiring a lock
* Overhead when doing acquiring/release lock

## Recap - Lock
* exclusion(limited users can enter the critical section)
* need to determine the types of locks when programming
* idea is simple
* easy to use in the beginning
* can be applied to all resources
* mature APIs
* mature debug tools

## Let's Take a Step Back

* Why do we do synchronization?
	* To make sure the behavior of a block of codes correctly

* What's wrong with locks?
	* It's a kind of pessimistic with might waste a lot of opportunities for performance

## How about being more Optimistic?
* The idea of Optimistic Concurrency Control(OCC) was proposed by H.T Kung:
	* Being
		* start to do housekeeping
	* Modify
		* read and write
	* Validate
		* check conflicts happen or not
	* Commit or Rollback
		* No conflict found, commit the result
		* Conflict happen, resolve it(usually abort and try again)

## After several decades...
* x86 Transactional Synchronization Extension(TSX) arrived in mainstream machines
	```
		start:
			xbegin abort_handler //begin

			mov %eax， count
			inc %eax             //modify
			mov count， %eax
                                 //valide
			xend                 //commit
		abort_handler:
			jmp  start           //rollback
	```

## Let's play with TSX
* https://github.com/reborn2266/STM-Toy

## The Strength of TM
* Simple and elegant in concept
	* Consider how to deal with 2 code blocks that need to be synchronized?
* No deadlock since there is no lock used
* Can compose transaction much easier
	* No ABBA deadlock

## Many Questions ran into my Brain
* There is no free lunch...
* How does HW do that?
	* how to detect conflict?
	* when to validate? really in single xend instruction?
	* how to do rollback with different design goals?
		* fairness
		* livelock prevention
		* stravation avoidance
		* max throughput
		* ...
* What are the limitations and best practices of TM?
	* I/O can't perform in a transaction?

## Hardware Transactional Memory(HTM)
* Speculative values in a transaction must be buffered and remain unseen by other threads until commit time.
* Large buffers are used to store speculative values while avoiding write propagation through the underlying `cache coherence` protocol
* When caches are used, the system my introduce the risk of `false conflicts` due to the use of cache line granularity

## Let's take a look at software transactional memory(STM)
* https://github.com/reborn2266/STM-Toy

## Important Design Choices
* We can learn how TM systems work by studying STM
* Get deeper understanding of what HTM described in manual

* Conflict handling
	* definition of a conflict
	* conflict detection
	* conflict resolution

* version managrement
	* eager - write to memory directly so need to maintain an `undo-log`
	* lazy - write to some buffer first so need to maintain s `redo-log`

## The weakness of TM
* Race conditions still happen if programmers don't specify transactions in proper code blocks
* Starvation, which is obviously when conflicts appear frequently
* Hard to debug(how can you debug HW behavior?)
* Not many applications
* No standards(No API)
* Can't protect resources like I/O easily
* Don't  work well with current SW tools, lock gdb
* Reference: Why The Grass May Not Be Greener On the Other Side: A Comparison of Locking v.s. Transactional Memory

## The Future is Now
* Suitable for some applications
	* Complex data structures need fine-grain granularity
	* Update-heavy workloads using large non-partitionable data structures such as high diameter unstructured graphs
	* e.g., Twemache: Twitter Memcached(in memory cache/data structure), SAP-HANA: B+Tree/Delta Storage

## References
* [Transactional Memory 2nd](https://www.amazon.com/Transactional-Synthesis-Lectures-Computer-Architecture/dp/1608452352)
* [STM-Toy](https://github.com/reborn2266/STM-Toy)
* [x86 RTM manual]
* [GCC TM Tutorial]
* https://hackmd.io/@coscup/ryHfQ6vAu/%2F%40coscup%2FHkDJm6PAd
