# Observability and resiliency pattern in the cloud

### Speaker: Sara Gerion  
2019/09/20

## Logging 
* Logging gives us information related to the errors and states of the running code application.

### Useful Strategies
* Json format
	* More context(service name, environment, host, log level, data center)
	* Filtering(by context)& browsing
* Sample logging
	* Useful operational information
	* Cost effective

## CloudWatch Metrics
* Divided by Namespaces
	* AWS/EC2
	* AWS/DynamoDB
* Available metrics per service
	* EC2: CPUUtilization
	* DynamoDB: ThrottledRequests, ConsumedReadCapacityUnits
* Filterable by Dimension(s)
	* EC2: InstanceId, InstanceType
	* DynamoDB: TableName
	
## Metrics
Metrics give us information related to the state of the underlying infrastructure via discrete values collected a specific amount of time

### Useful Strategies
* Capture missing data
	* Missing logs
	* Incorrect number of running resources
	
* HTTP requests
	* Latency of inbound & outbound requests
	* Http status 
	* Missing http status(error on a TCP layer)
	
* Study with your data(is this a metric or the context of a metric)
	* Better metrics filtering
	* Less data pollution

## Metrics & Alert

### Useful Strategies
* Missing data
	* Notify if service unavailable

* Definition of different alert type
	* Custom recipients and custom medium based on severity
	* Better escalation
	* Better visibility for stakeholders

## Cases study of AWS
* Multi AV replication
* Load balancing & data replication
* Health checks of regional stacks

## Common vulnerabilities 
Open System Interconnection(OSI) model

|    Level     | Protocols  | Attack types                                 |
| -----------  | ---------- | -------------------------------------------- |
| Application  |  HTTP, DNS | HTTP floods, cache busting, DNS query floods |
| Presentation |  TLS       | TLS abuse                                    |
| Session      |            |                                              |
| Transport    |  TCP       | SYN floods                                   |
| Network      |  UDP       | UDP reflection                               |
| Data Link    |            |                                              |
| Physical     |            |                                              |


## Common attack types
### Infrastructure Layer
* UDP reflection attack
	* UDP request packet with our service's IP as origin
	* An intermediate server generates a bigger response
	* If intermediate server is DNS server, response up to 54x bigger
	* Server capacity under stress
	
* TCP SYN flood attack
	* Attacker keeps on establishing connections with target
	* Initial handshake never finalized
	* Our sever runs out capacity to accept new TCP connections

### Application Layer
* HTTP flood
	* Attacker keeps on sending HTTP requests pretending to be the API consumer
* Cache busting
	* Variations in the query string to circumvent CDN caching
* DNS query flood(+ cache busting)
	* Well-formed DNS queries sent to exhaust the resources of a DNS server
	* Random subdomain to bypass caching
* TLS abuse
	* Sending unintelligible data or perpetually renegotiates the encryption method

## Service at the edge 
* Route 53: DNS query floods mitigated
	* DNS service
	* Shuffle sharding

* CloudFront: HTTP floods mitigated
	* CDN (Content Delivery Network) service
	* Caching of HTTP content
	* Less HTTP and TCP connections to origin
	* Accepts only well formed connections
   SYN floods, UDP reflection, TLS floods mitigated

* WAF(Web Application Firewall)
	* Integrated with CloudFront
	* Set of rules to protect your origin
	* You can observe, analyze and deny requests with malicious intent

## Observability patterns
* Logging 
* Metrics 
* Traceability
* Monitors
* Alerts

## Resiliency patterns
* Redundancy of computing units
* Replication of data
* Load balancing
* DNS routing
* Delivery at the edge

## Useful questions during design phase
* What can go wrong?
* How should my system react in case of that specific failure?
* Who should be notified, and how?
* How can we limit the impact for customers?





	