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
		* avoid using -Djava.util.concurrent.ForkJoinPool.common.paralleism=100
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

* //TBD...








