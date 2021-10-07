---
title: "03 State Predicates"

---
# State Predicate (Invariants)

When the framework is running tests, it needs to know whether a test passed or failed. It does so by verifying that certain conditions are met. `StatePredicate` are those conditions

```java
class StatePredicate extends java.util.Function.Predicate<AbstractState> {
  String name;
  Function<AbstractState, Pair<Boolean, String>> predicate;
  
  public boolean test(AbstractState state);
  public String detail(AbstractState state);
    
  public StatePredicate negate();
  public StatePredicate and(StatePredicate other);
  public StatePredicate or(StatePredicate other);
    
}
```



### java.util.Function.Predicate

```java
interface Predicate<T> {
  boolean test(T t);
  //default methods
  default Predicate<T> and(Predicate<? super T> other);
  default Predicate<T> negate();
  default Predicate<T> or(Predicate<? super T> other);
}
```



* Predicate is a standard java interface with just one abstract method signature. `bool test(T)` . It takes a generic `T` as input and produces bool as output.
* For any testing framework this can be a useful interface. The framework's "conditions" should implement this interface. They take state of the system as input to `test()`. Then `test()` executes some logic and produces the result indicating whether it was a pass or a fail.



### Adding Names and Messages

StatePredicate class adds two more things

1. It stores a human readable `name` for the "condition"
2. Along with returning whether condition was met or not, it also returns a small message indicating success or why there was a failure.

```
                                                                      
+--------------+                                       +-------------+
|              |            +----------------+         |             |
|AbstractState |----------> | StatePredicate |-------> |<bool_result, 
|(State of the |            |  predicate()   |         |string_msg>  |
|    System)   |            +----------------+         |             |
+--------------+                                       +-------------+
```





