**1. What is a Functional Interface?**

A **Functional Interface** is an interface that contains **exactly one abstract method**. It can have any number of default or static methods but only one abstract method. This single abstract method makes it suitable to be used as the target for **lambda expressions** or **method references** in Java.



### Why is it important?

Functional interfaces enable **functional programming** style in Java, introduced since Java 8. They help write concise and expressive code using **lambdas**.



### Key Points:

* Must have exactly **one abstract method**.
* Can have **default** and **static** methods.
* Annotated with `@FunctionalInterface` (optional but recommended).
* Used as the type for **lambda expressions** and **method references**.



### Common Examples:

```java
@FunctionalInterface
public interface Runnable {
    void run();  // single abstract method
}

@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);  // single abstract method
}
```



### Your Simple Custom Example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);  // only one abstract method
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression
        Calculator add = (a, b) -> a + b;
        System.out.println(add.calculate(5, 3));  // Output: 8
    }
}
```



### Summary:

* Functional interface = one abstract method
* Enables lambda expressions
* Helps functional programming in Java

---
