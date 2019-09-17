# ZGC for Future LINE HBase 

### Speaker: Shinya Yoshida   
2019/09/05

## Garbage Collection

* Stop The World(STW)
  * World == Application 
  * Application can do nothing while in STW states.

* HBase and GC
  * Hbase = NoSQL running on JVM
  * JVM pasuse time(STW) by GC will lead to long response time of HBase.
  * Since HBase is used as persistent store in Line Messaging, the response time of HBase is important.

### How to collect garbage
#### 1. Finding garbage (non referenced objects)
  * Mark-compact algorithm
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
### GC in Java
| GC Algorithms                                   |How to collect            |
| ----------------------------------------------- |-------------------------:|
| Young GC Sequential/Parallel                    |Copy                      |
| Old GC Sequential/Parallel                      |Sweep&Compact             |
| Old GC Concurrent Mark&Sweep (To be removed)    |Sweep Only(No Compaction) |
| G1GC                                            |Copy                      |
| ZGC(Java11, Experimental)                       |Copy                      |
| Shenandoah(Java12, Experimental)                |Copy                      |


## Evaluate HBase with ZGC

## Applying ZGC to production HBase

## Futher reading
* [Memory Management in Java: Mark Sweep Compact Copy algorithm](https://iq.opengenus.org/memory-management-in-java-mark-sweep-compact-copy/)
