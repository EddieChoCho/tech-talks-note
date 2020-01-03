# Parallel and Asynchronous Programming with Streams and CompletableFuture 
### Speaker: Venkat Subramaniam

## Parallel v.s. Asynchronous:
* when to use parallel: have a collections of data and want to deal with it parallel
* when to use asynchronous: divide and conquer, completableFuture

## Parallel Streams
* Collection pipeline pattern
* From imperative to functional
* Benefits of the pipeline pattern
* Parallel as a master switch
    * Structure of sequential code is same as parallel code
	* Since it is easy to switch to parallel, we need to think twice before switch it.
* Sequential execution
	* Note last call wins:
	```
    .parallel()
	.sequential()
	.parallel()
	.sequential()
    ```
	
* Order of execution
* Controlling the order
	* forEachOrdered() -> does not means it executed sequential
	* findFist
	* other terminal operations...
* Parallel and filter
* Parallel and map
* Parallel and reduce
	* Use reduce with parallel will execute extra times than sequential?
	* Should use identity value(e.g 0) instead of the non-identity value(e.g. 21)
	```
    .reduce(0, (total, e) -> add(total, e)) + 21;
    ```
* How many threads?
	* Formula to decide number of threads
	* Default number of threads
	* Configuring number of threads JVM wide
		* avoid using -Djava.util.concurrent.ForkJoinPool.common.parallelism=100
			* this will changing the JVM level common pool
	* Configuring programmatically
	```
    ForkJoinPool pool = new ForkJoinPool(5);
    pool.submit(() -> stream.forEach(e->{}));
    ```	
* Parallel does not always mean fast
	* How to decide to make parallel
		* When parallel makes no sense: 
		* //TBD...
    * Laziness
        * Stream does not execute a function on a collection of data
        * Stream instead executes a collection of functions on a piece of data
        * FindFirst -> will not go through all data
    * Laziness + Parallel?
            * Laziness -> try to avoid extra works, minimize the work you do
            * Parallel -> don't mind wasting more resources with the hope of getting the result faster

## CompletableFuture 
* Three different states: result, rejected or pending
* Two channel, data and error
* Failure/error is like data, beautiful to compose
	```
	func.then(f1).catch(e1).then(f2).catch(e2)
	//data channel: f1-f2-...
	//error channel: e1-e2-...
	```
* Stages, one stage completes and another stage may run
* gte v.s. getNow
* Thread of execution: If the CompletableFuture finished it's work and main is also free, the result of the task would be handled by main(e.g. future.thenAccept)
* Changing the pool: 
* thenAccept is like forEach, thenApply is like map and thenCompose is like flatMap
* Main Difference between CompletableFuture and Stream:
    * Stream is a zero or more stream of operation
    * CompletableFuture has only one sequence goes through, then it will be completed or canceled
* Question to ask when dealing exceptions with CompletableFutures
    * Are we dealing with the exception and recovering from it? -> return value
    * Or we dealing with the exception with no hope of recovery? -> don't return value
* What if the complete never happen?
    * Never do anything without timeout
    * Succeed on timeout
    * Failure on timeout
    * Java9: future.completeOnTimeout
    * Java9: future.orTimeout
* combine
    * futureA.thenCombine(futureB, (a, b) -> a + b).thenAccept(result -> sout(result));
    * futureA.thenCombine(futureB, Integer::sum.thenAccept(result -> sout(result));
* compose
    * thenCompose








