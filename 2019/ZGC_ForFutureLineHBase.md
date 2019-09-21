# ZGC for Future LINE HBase 

### Speaker: Shinya Yoshida   
2019/09/05

## Garbage Collection

* Stop The World(STW)
  * World == Application 
  * Application can do nothing while in STW states.

* HBase and GC
  * Hbase = NoSQL running on JVM
  * JVM pause time(STW) by GC will lead to long response time of HBase.
  * Since HBase is used as persistent store in Line Messaging, the response time of HBase is important.

### How to collect garbage
#### 1. Finding garbage (Mark: to identify all live (reachable) objects)
* First, Garbage Collection algorithm defines some specific objects as Garbage Collection Roots (GC Roots)[1]. For example:
     * Local variable and input parameters of the currently executing methods
     * Active threads
     * Static field of the loaded classes
     * JNI references
 
![alt text](https://iq.opengenus.org/content/images/2018/05/gc_mark.png)

* Key points regarding the marking phase are[1]:
    * When the application threads are temporarily stopped so that the JVM can indulge in housekeeping activities is called a safe point resulting in a Stop The World pause.
    * Safe points can be triggered for different reasons but garbage collection is by far the most common reason for a safe point to be introduced.
    * The duration of this pause does not depend on the total number of objects in heap nor or the size of the heap
    * The duration of this pause depends on the number of alive objects.
    * Increasing the size of the heap does not directly affect the duration of the marking phase.
 
#### 2. Collect garbage, and defrag heep space.
##### 2-1. Sweep/Compaction: Remove and defrag
![alt text](https://iq.opengenus.org/content/images/2018/05/gc_compact.png)
* Fragmentation will be solved in the last step, but the address will also be changed.
##### 2-2. Copy(Relocate)
![alt text](https://iq.opengenus.org/content/images/2018/05/gc_copy_opengenus.png)
* Move live objects to another space 
* Do either while or do after marking.
* Need another memory region

### How to use heap spaces (This topic is skipped.)

## How to choose GC
* Know GC's properties (pros/cons)
* Consider your application's properties and hardware's properties
### GC in Java
| GC Algorithms                                   |How to collect            |
| ----------------------------------------------- |-------------------------:|
| Young GC Sequential/Parallel                    |Copy                      |
| Old GC Sequential/Parallel                      |Sweep&Compact             |
| Old GC Concurrent Mark&Sweep (To be removed)    |Sweep Only(No Compaction) |
| G1GC                                            |Copy                      |
| ZGC(Java11, Experimental)                       |Copy                      |
| Shenandoah(Java12, Experimental)                |Copy                      |

### GC with running application

STW will be occur while GC for two reason:
1. Soundness(Must): Never collect living object
2. Completeness(Option): Collect all garbage

STW duration depends on algorithm:
1. How long, often
2. Scaling factor(e.g. heap size, number of living object, number of threads)

### GC's properties
1. Response time(STW duration)/Throughput
2. How many CUP cores are required for best performance
3. Heap size
4. Live object ratio for best performance
5. Object sizes
6. And so on...

## Evaluate HBase with ZGC

## Applying ZGC to production HBase

## Further reading
* [1] [Memory Management in Java: Mark Sweep Compact Copy algorithm](https://iq.opengenus.org/memory-management-in-java-mark-sweep-compact-copy/)
