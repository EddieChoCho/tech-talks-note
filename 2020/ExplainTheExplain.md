# Explain the Explain
### Speaker: Maria Colgan
2020/10/23

## Additional information under the execution plan
* Operations: 
	* produces roles(full table scan), consume roles(sort/group by), produces and consume roles(hash join),

* Predicate information: Shows that how optimizer using the predicates
	* Access predicate: Where clause predicate used for data retrieval
		* The start and stop keys for an index.
		* If rowids are passed to a table scan.
	* FIlter predicate: Where clause predicate that is not used for data retrieval but to eliminate uninteresting rows once the data is found.

* Note Section: Details on Optimizer features used
	* Rule Based Optimizer(RBO, seldom used today)
	* Dynamic Sampling
	* Outlines
	* SQL Profiles or plan baselines
	* Adaptive Plans
	* Hints(Starting in 19c)

## How to generate an execution plan
* EXPLAIN PLAN command: Displays an execution plan for a SQL statement without actually executing the statement.
* V$SQL_PLAN: A dictionary view introduced in Oracle 9i that shows the execution plan for a SQL statement that has been compiled into a cursor in the cursor cache.
* EXPLAIN PLAN v.s. V$SQL_PLAN: EXPLAIN PLAN does not check the bind value but V$SQL_PLAN does.

### EXPLAIN PLAN command & dbms_xplan.display function
* TBD...
### Generate & display plan for last SQL statements executed in session
* TBD...

## What is Cardinality Estimate?

## Checking Cardinality Estimates for Parallel Execution
## Solutions to Incorrect Cardinality Estimates
## How many Access Methods does the Optimizer have?
1. Full Table Scan
	* Read all rows from table & filters based on where clause predicates.
	* Used when no index, DOP set dtc.
	
2. Table Access by Rowid
	* Rowid- datafile. data block & location of the row in that block.
	* Used if row id supplied by index or directly in a where clause.
	
3. Index Unique Scan
	* Only one row returned.
	* Used if a UNIQUE or PRIMARY KEY constraint that guarantees that only a single row is accessed.
	* e.g. SELECT id, name FROM customer WHERE id IN (100, 200, 100000);
	
4. Index Range Scan
	* Access adjacent index entries return ROWIDs used with quality on non-unique(column/index) or rande predicate on unique indexes.
	* e.g. SELECT id, name FROM customer WHERE id BETWEEN 100 AND 150;
	
5. Index Skip Scan (less common)
	* Skip the leading edge(column) of the index & use the rest.
	* Typically done when the first column in that index doesn't have a high number of distinct values but the subsequent columns do.
	
6. Full Index Scan
	* Scans all leaf blocks but only enough branch blocks to find 1st leaf.
	* Used when all columns are in index & order by clause matches index or SMJ.
	* We've already got the data in a sorted format and we don't need to do an additional sort operation for the "order by".
	
7. Fast Full Index Scan
	* Scans all blocks in the index used to replace a Full Table Scan when all columns are in the index.
	* Using multi-block IO & can go parallel.
	
8. Index Joins(less common)
	* Hash join of several indexes that together contain all needed columns.
	* Won't eliminate a sort operation.
	* It occurs if we've got an incredibly wide table that has multiple indexes on it.
	
9. Bitmap Index
	* Uses a bitmap for key values and a mapping function that converts each bit position to a rowid.
	* Can efficiently merge indexes that correspond to several conditions in a WHERE clause.
	* Used frequently in data warehousing.

### Exercise
* Common access path issues.
	* Uses a table scan instead of index.
	* Picks wrong index.

## How many Join Methods does the Optimizer have?
1. Nested Loops Joins
	* It's normally chosen when we're joining two small tables.
	* For every row in the outer table, Oracle assesses all the rows in the inner table.
	* Useful when joining small subsets of data and there is an efficient way to access the second table(index lookup).
	* Normally it's an index access on that inner table.
	
2. Hash Joins
	* The smaller of two tables is scan and resulting rows are used to build a hash table on the join key in memory.
	* The larger table is then scan, join column of the resulting rows are hashed and the values used to probe the hash table of find the matching rows.
	* Useful for larger tables & if equality predicate.
	
3. Sort Merge Join
	* Consists of two steps:
		1. Sort join operation: Both the inputs are sorted on the join key.
		2. Merge join operation: The sorted list are merged together.
	* Useful when the join condition between two tables is an inequality condition.
	
* TBD...

## References
* [Explain the Explain by Maria Colgan](https://youtu.be/OpPiJ2P5iPs)


