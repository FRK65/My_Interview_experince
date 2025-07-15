# My Understanding on Abstratc Class 

- An abstract class in Java is a class that cannot be instantiated.
- An abstract class provides a partial implementation and forces subclasses to complete it

 **🔹 What You Got Right:**

- ✅ Abstract classes can’t be instantiated directly.
- ✅ They are used as **base classes** for inheritance.
- ✅ They can have **abstract methods** (methods with no body).
- ✅ Child classes must provide **implementations** for all abstract methods.
- ✅ The `abstract` keyword is required to declare an abstract class or method. MUST
- ✅ Abstract class can have both abstract and concrete methods.
- ✅ Cannot be instantiated.
- ✅ Must be extended by a child class.
- ✅ Abstract methods must be implemented in child classes (unless the child is also abstract).
- ✅ If child class is not providing implementation then this should also declare as Abstract.
- ✅ Now, Abstract Class A -> Abstract Class B -> class C will extend B only and Then Class C provides implementations for all remaining abstract methods.
- ✅ Use `abstract` keyword **before** return type. // public abstract int getLegs(); 



⚠️ **Corrections and Improvements**

🔴 1. **Not all methods in an abstract class must be abstract**

✅  An abstract class **can have both abstract and non-abstract methods**.

Example:

```java
public abstract class Animal {
    public abstract void sound(); // abstract method

    public void eat() {           // regular method
        System.out.println("Animal is eating...");
    }
}
```

🔴 2. **correct method declaration syntax**

```java
public abstract void sound();
public abstract void sleep();
public abstract int getLegs(); 
```

🔹 The `abstract` keyword comes **before the return type and access modifiers**, not after.

🔴 3. **Access modifiers missing**

By default, Java methods are **package-private** (less visible). 
In real-world projects, abstract methods are usually declared as `public` to ensure they can be accessed and overridden from other classes.

```java
public abstract class Animal {
    public abstract void sound();
    public abstract void sleep();
}
```


**✅ Improved & Corrected Version of Your Code:**

```java
public abstract class Animal {

    public abstract void sound();
    public abstract void sleep();

    public void eat() {
        System.out.println("Animal is eating...");
    }

    public abstract int getLegs(); // 👈 Added abstract method
}

class Cat extends Animal {
    public void sound() {
        System.out.println("Meow Meow...");
    }

    public void sleep() {
        System.out.println("Cat is sleeping..");
    }

    public int getLegs() {
        return 4;
    }
}

class Dog extends Animal {
    public void sound() {
        System.out.println("Barking Barking...");
    }

    public void sleep() {
        System.out.println("Dog is sleeping..");
    }

    public int getLegs() {
        return 4;
    }
}

class Main {
    public static void main(String[] args) {
        Animal c = new Cat();
        c.sound();
        c.sleep();
        c.eat();
        System.out.println("Cat has " + c.getLegs() + " legs");

        Animal d = new Dog();
        d.sound();
        d.sleep();
        d.eat();
        System.out.println("Dog has " + d.getLegs() + " legs");
    }
}

```

---

**✅ What You Can Add to Make Your Understanding Complete**

🔹 Abstract Class vs Interface (Quick Compare)

| Feature                | Abstract Class        | Interface                |
| ---------------------- | --------------------- | ------------------------ |
| Can have method bodies | ✅ Yes                 | ✅ Since Java 8 (default) |
| Multiple inheritance   | ❌ No (one class only) | ✅ Yes (multiple allowed) |
| Constructors allowed   | ✅ Yes                 | ❌ No                     |
| Fields                 | ✅ Can have fields     | ✅ Only constants         |

---

# My Doubts on abstract class 

- ✅ **1. Is using `abstract` keyword a must with class while declaring an abstract class?**
- ✅ **2. Is using `abstract` keyword a must with method while declaring abstract method?**
- ✅ **3. Why do we have concrete methods in an abstract class if it's "abstract"?**
- ✅ **4. Can we create an abstract class instance without a child class?**
- ✅ **5. If we create a constructor in an abstract class, what is the use of it? Should it be public or private?**
- ✅ **6. Can we have variables in an abstract class? What types? Access modifiers?**
- ✅ **7. Add an `int` return type method to the corrected code**
- ✅ **8. Can a class extend two abstract classes?**

---

**Summary Table**

| Question # | Answer Summary                                 |
| ---------- | ---------------------------------------------- |
| 1          | Yes, `abstract` keyword is required for class  |
| 2          | Yes, `abstract` keyword is required for method |
| 3          | Concrete methods allow code reuse              |
| 4          | No, can't instantiate abstract class directly  |
| 5          | Yes, constructor allowed; use public/protected |
| 6          | Yes, all variable types and access levels ok   |
| 7          | Added `public abstract int getLegs();` to corrected code        |
| 8          | No, can’t extend 2 abstract classes in Java    |

---


✅ **1. Is using `abstract` keyword a must with class while declaring an abstract class?**

> **Yes.**
> If a class contains **at least one abstract method**, it **must** be declared as `abstract`.
> Even if a class has **no abstract methods**, you can still make it `abstract` — to **prevent instantiation**.

🔸 Example:

```java
public abstract class Animal {
    // Can have all concrete methods but still be abstract
}
```

If you omit the `abstract` keyword for a class that has abstract methods, **you’ll get a compile-time error.**


✅ **2. Is using `abstract` keyword a must with method while declaring abstract method?**

> **Yes.**
> Any method **without a body** must be explicitly marked as `abstract`.

🔸 Invalid:

```java
public void sound(); // ❌ Compile-time error
```

🔸 Valid:

```java
public abstract void sound(); // ✅ Correct
```



✅ **3. Why do we have concrete methods in an abstract class if it's "abstract"?**

> An abstract class can act as a **template** or **base class**. It may include **shared functionality** (concrete methods) that **all child classes** can inherit directly.

🔹 Example:

```java
public abstract class Animal {

    public abstract void sound(); // must be implemented

    public void eat() {
        System.out.println("All animals eat food."); // shared code
    }
}
```

>  This avoids code duplication.
>  Child classes only implement what's **unique**.



✅ **4. Can we create an abstract class instance without a child class?**

> **No**, you **cannot** directly instantiate an abstract class, **even without a child class**.

However, you *can* create an **anonymous subclass** on the spot:

🔸 Example:

```java
Animal a = new Animal() {
    public void sound() {
        System.out.println("Anonymous animal sound...");
    }
};
a.sound(); // Outputs: Anonymous animal sound...
```

> Used for quick testing or short-lived custom behavior.



✅ **5. If we create a constructor in an abstract class, what is the use of it? Should it be public or private?**

🔹 Yes, you **can** have a constructor in an abstract class.

> It is **used by child classes** to initialize the **common state** (variables).

🔸 Example:

```java
public abstract class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }
}
```

🔸 **Should it be `private` or `public`?**

* `**public/protected**` ➝ ✅ Useful, so child classes can call `super(...)`.
* `**private**` ➝ ⚠️ Useless — child class **cannot access it**.

So normally it's **public or protected**.



✅ **6. Can we have variables in an abstract class? What types? Access modifiers?**

 ✔ Yes, you can have **all kinds of variables**:

| Type           | Allowed? | Example                            |
| -------------- | -------- | ---------------------------------- |
| Instance       | ✅ Yes    | `int age;`                         |
| Static         | ✅ Yes    | `static int count;`                |
| Final          | ✅ Yes    | `final String species = "Animal";` |
| Static + Final | ✅ Yes    | `static final double PI = 3.14;`   |
| Local          | ✅ Yes    | Inside method only                 |

✔ What about access modifiers?

You can use `private`, `protected`, or `public`.

* `**private**` → Accessible **only inside the abstract class**
* `**protected**` → Accessible in **child classes**
* `**public**` → Accessible from **anywhere**

🔸 Example:

```java
public abstract class Animal {
    protected int age;
    private String name;
    public static final String TYPE = "Mammal";
}
```

> Child classes **can access protected and public** fields.



✅ **7. Add an `int` return type method to the corrected code**

Sure! Here’s your updated version with a method `int getLegs()`:

```java
public abstract class Animal {
    public abstract void sound();
    public abstract void sleep();

    public void eat() {
        System.out.println("Animal is eating...");
    }

    public abstract int getLegs(); // 👈 Added abstract method
}
```

✅ **8. Can a class extend two abstract classes?**

> ❌ **No**, Java does **not support multiple inheritance** using classes.

This would cause ambiguity when two classes have methods with the same signature.

🔸 This is **not allowed**:

```java
public class Cat extends Animal, Mammal {} // ❌ Compile-time error
```

> **But you can**:

* Extend **one abstract class** **and**
* Implement **multiple interfaces**

🔸 Example:

```java
public class Cat extends Animal implements Runnable, Serializable {}
```



