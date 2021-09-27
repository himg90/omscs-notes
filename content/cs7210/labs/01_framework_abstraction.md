![dslabs_abstract_interface](/cs7210/labs/dslabs_abstract_interface.png)

* The system consists of a number of `node`s which have `addresse`s
* A `Node` can `send()` a `message` to another `node`. The receiving `node` has to `handle()` the message. 
* Some `nodes` expose a `client` interface.  An external user can interact with the distributed system by `send()`ing  `commands`and gettting `results` back.
* `Application`: Some nodes (server nodes) have contain an `application` object. An example of an application would be key-value in-memory store. Application is what the users of the distributed system want to access. An application takes a `command` as input and produces `result` as output.
* Communication is one way i.e. when a node is sending a message, it is not expecting a reply message. Request/Response semantics are implemented at client-server level.