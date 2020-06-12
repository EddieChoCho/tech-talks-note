# Java Concurrent Animated-2020
### Speaker: Victor J Grazi
2020/06/02

### 6 states of a thread
* NEW
    * A thread that has not yet started is in this state.
* RUNNABLE
    * A thread executing in the Java virtual machine is in this state.
* BLOCKED
    * A thread that is blocked waiting for a monitor lock is in this state.
* WAITING
    * A thread that is waiting indefinitely for another thread to perform a particular action is in this state.
* TIMED_WAITING
    * A thread that is waiting for another thread to perform an action for up to a specified waiting time is in this state.
* TERMINATED
    * A thread that has exited is in this state.

### synchronized
* One thread executing inside the synchronized block at the same time. All other threads attempting to enter the synchronized block are blocked(Non-Runnable/Blocked state) until the thread inside the synchronized block exits the block.
* When one thread exit the synchronized block, the thread scheduler will select another thread(block state thread) randomly to be in the runnable state.
* object.wait()
	* When calling wait for the object locked by the synchronized block inside the synchronized block, the state of the running thread will become waiting.
	
* object.notify()
	* When calling notify method, the thread in waiting state will be changed to block state. (Select one waiting thread randomly and change it to block state?)
	* When one running thread exit the synchronized block, the thread scheduler will select another thread(block state thread) randomly to be in the runnable state.
	
* object.notifyAll()
	* Wakes up(waiting -> block) all of the waiting threads
	* Each one can test it condition, if the condition is fulfilled -> works on it's condition. If the condition is not fulfilled -> go back to wait.
		
* Multiple condition + wait 
	* When we call wait, we are essentially saying that we are waiting for any condition.
	* Once the wait has been notified, it has to check all the conditions.

### ReentrantLock
* One thread executing inside the ReentrantLock block at the same time. All other threads attempting to enter the ReentrantLock block are blocked(`Waiting State`) until the thread inside the synchronized block exits the block.
* lock.lockInterruptibly()
	* When I lock interruptibly, I am new enable to interrupt any waiting thread. It will throw an InterruptedException
* lock.newCondition()
	* Can create condition on the fly
	* Condition condition = lock.newCondition();
	* condition.wait();

### Semaphore
* Can declare a number of locks that can be hold concurrently.
* All other threads attempting to enter the Semaphore block are blocked(`Wating State`).
* semaphore.tryAcquire()
	* If it can not be acquired, then return false
	* Can set time for tryAcquire. e.g. semaphore.tryAcquire(3, TimeUnit.SECONDS)

### ReadWriteLock (ReentrantReadWriteLock)
* If you have a lot of readers and you do not want readers read in the middle of the update.
* We want to lock the data only when there are writers going on. But when there is no writers, we want all readers access the data concurrently.
* And if there is any reader access in the lock, we do not want allow any writers in util the readers done(those writers will be in the waiting state).
* If there is any writer access in the lock, we do not want allow any readers in util the writers done(those readers will be in the waiting state).
* When there is any writer waiting to acquire the lock, the new readers can not acquire to the lock, those reader will be waiting state.

### StampedLock
* TBD...
	* faster?
	* tryOptimisticRead

### Executors
* Pooled threads
	* Can reuse the threads
* executor.execute(Runnable)
	* execute immediately
* executor.submit(Runnable)
	* execute immediately 
	* return Future

#### Saturation Policy
* rejection
* caller runs
* etc...

### CyclicBarrier
* TBD...

### CountDownLatch
* TBD...

### Phaser
* TBD...

### BlockingQueue
* TBD...

### TransferQueue
* TBD...

### CompletableFuture
* completableFuture.supplyAsync
	* return value
* completableFuture.runAsync
	* return null
* completableFuture.get
* completableFuture.anyOf
	* return value of any task has been completed
* completableFuture.allOf
	* return null after all tasks have been completed
* completableFuture.join
* completableFuture.getNow(backupValue)

### CompletionService
* completionService.submit(Callable/Runnable)

### AtomicInteger

## References
* [Jnation 2020 - Aeminium](https://youtu.be/ngKmzr04Yug?t=18895)
* [JavaConcurrentAnimatedReboot](https://github.com/vgrazi/JavaConcurrentAnimatedReboot)


