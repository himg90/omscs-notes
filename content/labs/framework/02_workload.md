---
title: "02 Workload"

---
# Workload

```java
abstract class Workload {
	Pair<Command, Result> nextCommandAndResult()
	Command nextCommand()
	boolean hasNext()
	boolean hasResults()
}
```

* A workload is an ordered collection of commands and expected results. 
* Results are optional but commands are must.
* The test framework initializes a `Workload` and maps it to a client node.
* The framework executes one command at a time from the Workload on the client.
* If results are present in the workload, after every command, the result received from the client is compared against the _expected_ result stored in the Workload.



### StandardWorkload

* It's a concrete implementation of Workload.

* It adds two capabilities to Workload

  * __Repetition__: The workload ( i.e. the command in workload) can be executed specified number of times.

  * __Patterns as Commands__: Instead of providing exact list of command objects, it can be provided with a string patterns and it transforms them into strings. These strings are then converted to Command and Result objects by __Parser__. Parser is nothing but a function that takes strings as input and converts them to Command and Result Objects.

    ```
                                                                          
       String_Pattern                                                     
       "GET:foo_%i"                                                       
             |                                                            
             v                                                            
    +-------------------+                                                 
    | Standard Workload |                                                 
    |  doReplacements() |                                                 
    +-------------------+                                                 
             |                                                            
             v                   +--------------+                         
        String_Command  -------> |Parser        |------->  GET{           
        GET:foo_01               |KVStoreParser |          key : "foo_01"}
                                 +--------------+                         
    ```
