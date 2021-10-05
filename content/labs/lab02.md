## Wait-Notify Semantics of Java

> Java has a builtin wait mechanism that enable threads to become inactive while waiting for signals. The class java.lang.Object defines three methods, wait(), notify(), and notifyAll(), to facilitate this.
> A thread that calls wait() on any object becomes inactive until another thread calls notify() **on that object**. In order to call either wait() or notify the calling thread **must first obtain the lock on that object**. In other words, the calling thread must call wait() or notify() from inside a synchronized block.

source: [Thread Signaling by Jakob Jenkov](http://tutorials.jenkov.com/java-concurrency/thread-signaling.html)

## Synchronized Instance Methods in Java

> A synchronized instance method in Java is synchronized on the instance (object) owning the method. Thus, each instance has its synchronized methods synchronized on a different object: **the owning instance**.
> Only one thread per instance can execute inside a synchronized instance method. If more than one instance exist, then one thread at a time can execute inside a synchronized instance method per instance. **One thread per instance**.
> This is true across all synchronized instance methods for the same object (instance). Thus, in the following example, only one thread can execute inside either of of the two synchronized methods. **One thread in total per instance.**	

source: [Java Synchronized Blocks by Jakob Jenkov](http://tutorials.jenkov.com/java-concurrency/synchronized.html)