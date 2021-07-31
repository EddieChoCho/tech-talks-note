# Go memory allocation and deallocation
### Speaker: Gaston Chiu
2021/07/31
## Background Knowledge
* Heap and Stack Example
	* Stack is fixed size linear data struct and only handles the memory will be released after the function finished.
	* The memory allocated on heap is slow. Therefore handling the life cycle is difficult and tricky. There are a lots of things we can optimize.

* Escape Analysis
	* Go decide the object should go to heap or stack through the escape analysis.

* Garbage collection(Mark and Sweep)
	* The mechanism that decide the object should be released is called garbage collection.(In real life it will be triggered automatically)

* Heap struct in go
	* System calls(mmap) are very expensive. We pre-allocate an area of memory.

* There is a gap between requested memory and used memory.

* Garbage collection(Scavenging)
	* After a memory is marked as unused, it will be released to OS.
* Memory life cycle
	```
	  [OS] ──request──> [Unused] ──new object──> [Used]
	   ^                  │ ^                      │
	   │                  │ │                      │
	   └──────────────────┘ └──────────────────────┘
	```


## Thread-cache Malloc(TCMalloc)
* Large allocate 
	* Large object allocation(>32KB)
	* Allocate on heap directly
	* Required lock

* Small allocation
	* Small object allocation(<=32KB)
	* There are about 70 different classes of span from 8 bytes to 32 KB.
	* Round up to the class size
	* No lock required in the small allocation.

* Mache benefit
	* Assume most allocations are small object
	* Align with Go m:n scheduler
	* Provide better locality
	* Reduce fragmentation

* The mcache has no space(small allocation)
	* Ask a new span from mcentral.
	* All the mcaches share the same mcentral.
	* Require lock when asking for a new span from mcentral.

* mcentral
	* Partial: partial of the span is in-used
	* Full: the span is full.

* The central list has no space(small allocation)
	* When mcentral has no space, we can allocate a new span on heap.

## Garbage collection
* Mark and Sweep
	* 2 * GC percentage / 100 *current memory size(in default GC percentage = 100)
	* Every few minutes

* Sweep phase
	* Sweep concurrently in a background goroutine.
	* When the gorotine needs another space on heap, it first attempts to reclaim that much memory by sweeping.(Before ask more from mheap or mcentral)

* Pacing
	* Pacing calculates how fast the next garbage collection trigger.

* Heap fragmentation
	* The heap memory will be fragmented because of GC.
	* The fragmentation reduces the efficiency.

* Scavenging
	* Free and release the physical page backing mapping memory to OS.
	* Reduce the page level fragmentation and resident set size.
	* Trigger scavenging
		* Background scavenging
		* Heap growth
		
	* How many memory we should retain
		* C * (next_gc_goal/last_next_gc_goal) * last_heap_inuse
			* (C = 1.1 in 1.16 version)
			
	* At what rate is memory scavenged
		* The goal is achieving the goal before the next collection trigger.(We record the interval between each trigger in pacing)
		
	* How to handle new allocation on heap
		* Best-fit: the memory allocated at the closest fragmentation
			* This should provide the least fragmentation
			
		* First-fit: the partition is allocated which is first sufficient.
			* Scavenging from the highest base address first.

* Recap
	```
	                 ┌─────────────────────────────────────┐
                     │                           TCMalloc  │
                     │                                     │
	  [OS] ──request─┼─> [Unused] ──new object──> [Used]   │
	   ^             │    │ ^                      │       │
	   │             └────┼─┼──────────────────────│───────┘
	   │                  │ │                      │
	   └──────────────────┘ └──────────────────────┘
	```

## References
* [Smarter Scavenging](https://go.googlesource.com/proposal/+/master/design/30333-smarter-scavenging.md)
* [Deep understanding go memory allocation](https://programmer.group/deep-understanding-go-memory-allocation.html)
* [Evolving the Go Memory Manager's RAM and CPU Efficiency](https://youtu.be/S_1YfTfuWmo)
* [Tcmalloc](https://google.github.io/tcmalloc/design.html)
* [GC in go](https://slides.com/jalex-chang/gc-in-go)
* https://hackmd.io/@coscup/HyHyr6PR_/%2F%40coscup%2FH1FpVTvA_

