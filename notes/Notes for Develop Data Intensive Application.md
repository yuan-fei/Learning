## Questions
1. SStable delete
## Chapter 1: Reliability, Scalability, Maintainability
* Reliability: fault-tolerant, resilient
	* fault: “A fault is usually defined as one component of the system deviating from its spec”
		* hardware, software, human
* Scalability: a system’s ability to cope with increased load
	* Load parameters: QPS, R/W rate, concurrency #, cache hits
	* Describing load: throughput for batch systems, percentiles for describe response time for online system
	* Cope with load: scale up/out, elastic
* Maintainability
	* operability: make life easy
		* visibility for system's health
	* simplicity: manage complexity
		* “One of the best tools we have for removing accidental complexity is abstraction”
	* evolvability

## Chapter 2: Data Models and query languages
* NoSQL: driving forces
	* greater scalability: large datasets or high write throughput
	* free and open source
	* Specialized query operations that are not well supported by the relational model
	* schema-less, dynamic and expressive data model
* Modeling
	* Relational
		* Good support for many-to-1 and many-to-many through joins
		* schema-on-write
	* Document: self contained data
		* Good for 1-to-many: self contained, tree representation
			* schema flexibility: schema-on-read
			* good locality when
				* doc not too large or deep
				* less writes/updates
		* Bad for many-to-1 and many-to-many
			* no support for join
			* limited support for relationships between one document and another
		* Network (Graph): modeling vertices and edges
			* Model
				* Property graph

				~~~SQL
				CREATE TABLE vertices (
					vertex_id   integer PRIMARY KEY,
					properties  json
				);
					
					
				CREATE TABLE edges (
					edge_id     integer PRIMARY KEY,
					tail_vertex integer REFERENCES vertices (vertex_id),
					head_vertex integer REFERENCES vertices (vertex_id),
					label       text,
					properties  json
				);
				
				
				CREATE INDEX edges_tails ON edges (tail_vertex);
				CREATE INDEX edges_heads ON edges (head_vertex);
				~~~

				* Triplet-store (used in RDF):
	
				~~~
				(subject, predicate, object)
				(lucy, age, 33)
				~~~
			
			* Query Language
				* Cypher: declarative query language for property graphs model like Neo4j

				~~~SQL
				MATCH
				(person) -[:BORN_IN]->  () -[:WITHIN*0..]-> (us:Location {name:'United States'}),
				(person) -[:LIVES_IN]-> () -[:WITHIN*0..]-> (eu:Location {name:'Europe'})
				RETURN person.name
				~~~
			
				* SPARQL: declarative query language for triple-stores using the RDF data model

				~~~SQL
				SELECT ?personName WHERE {
				?person :name ?personName.
				?person :bornIn  / :within* / :name "United States".
				?person :livesIn / :within* / :name "Europe".
				}
				~~~
				
			* Datalog: prolog for graph data query
				* `predicate(subject, object)`
* Query Language
	* SQL: declarative, hide query engine implementation such as query optimization (parallelization)...
	* Mapreduce: functional (non-declarative nor imperative)
	* Mongodb implements both MR and declarative query (pipeline)

## Chapter 3: Storage and Retrieval 
![SSTable](pics/ddia/SSTable.png)
## To Reading list
