# Where is my Cache? Architectural Patterns for Caching Microservices
### Speaker: Rafał Leszko
2020/06/02

# Caching Architectural Patterns
## Embedded
* Java Collection is not a Cache
	* No Eviction Policies
	* No Max Size Limit (OurOfMemoryError)
	* No Statistics
	* No built-in Cache Loaders
	* No Expiration Time
	* No Notification Mechanism
* Use Libraries for Cache
	* e.g Guava Cache
* Caching Application Layer
	* Be Careful, Spring uses ConcurrentHasMap by default! (better change to other cache e.g. Guava)
	* TBD...
* Communication issues, can not cooperate with load balancer
### Embedded Distributed
* Hazelcasr Cluster
* Spring with Hazelcast
* How to make Hazelcast nodes discover themselves automatical
	* Hazelcast Discovery Plugins
	* TODO: check implementation of plugins
### Pros 
* Simple configuration/ deployment
* Low-latency data access
* No separate Ops Team needed
### Cons
* No flexible management(scaling, backup)
* Limited to the language of your services
* Data collocated with applications

## Client-Server
* Need separate Management
    * Backups
    * (auto)scaling
    * security
* Communication through protocol, the languages between application and cache server can be different
### Client-Server(Could)
* Underline Could Provider
* Region
* Virtual Private Cloud(VPC)
### Pros
* Data separate from applications
* Separate management(scaling, backup)
* Programming-language agnostic
### Cons
* Separate Ops effort
* Higher latency
* Server network requires adjustment(same region, same vpc)

## Sidecar
* k8s
* Similar to Embedded
	* The same physical machine
	* The same resource pool
	* Scales up and down together
	* No discovery needed(always localhost)
* Similar to Client-Server
	* Different programming language
	* Uses cache client to connect
	* Clear isolation between app and cache
### Pros
* Simple configuration
* Programming-language agnostic
* Low latency
* Some isolation of data and applications
### Cons
* Limited to container-based environments
* Not flexible management(scaling, backup)
* Data collocated with application PODS

## Reverse Proxy
* Nginx -> load balancer + cache
* Application is not aware of cache
* Nginx Reverse Proxy Cache Issues
	* Only for HTTP
	* No distributed
	* No High Availability
	* Data stored on the disk (limited options)
### Reverse Proxy Sider
* TBD...
### Pros
* Configuration-based(no need to change applications)
* Programming-language agnostic
* Consistent with containers and microservice world
### Cons
* Difficult cache invalidation
* No Mature solutions yet
* Protocol-based(e.g. works only with HTTP)

## Summary

    application-aware?
	    ├── yes ── lot of data? security restriction?
	    │                       │
        │                       ├── yes ── cloud?
	    │                       │           ├── yes ── Cloud 
	    │                       │           │
	    │                       │           └── no ── Client-Server
	    │                       │
	    │                       └── no ── language-agnostic? containers?
	    │                                       │
	    │                                       ├── yes ── Sidecar
	    │                                       │
	    │                                       └── no ── Embedded(Distributed)
	    │
	    └── no ── containers?
                    │
                    ├── yes ── Revers Proxy Sidecar
                    │
                    └── no ── Revers Proxy
				
## Resources
* [Hazelcast Sidecar Container Pattern](https://hazelcast.com/blog/hazelcast-sidecar-container-pattern/)
* [Hazelcast Reverse Proxy Sidecar Caching Prototype](https://github.com/leszko/caching-injector)
* [Caching Best Practices](https://vladmihalcea.com/caching-best-practices/)
* [NGINX HTTP Reverse Proxy Caching](https://www.nginx.com/resources/videos/best-practices-for-caching/)

## References
* [Jnation 2020 - Aeminium](https://youtu.be/ngKmzr04Yug?t=8149)
* [Where is my cache? Architectural patterns for caching microservices by example](https://youtu.be/TQXRAZwtWfc)