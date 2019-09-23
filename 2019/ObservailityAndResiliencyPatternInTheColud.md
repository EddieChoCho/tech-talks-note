# Observability and resiliency pattern in the cloud

### Speaker: Sara Gerion  
2019/09/20

## Logging 
* Logging gives us information related to the errors and states of the running code application.

### Useful Strategies
* Json format
	* More context(service name, environment, host, log level, datacenter)
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
	
//TBD...
	