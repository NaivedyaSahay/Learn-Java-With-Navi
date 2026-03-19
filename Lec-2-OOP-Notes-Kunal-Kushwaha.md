# Mastering Java OOPs: Core & Advanced Concepts

---

## 1. Packages in Java

Packages are structural folders used to organize classes and prevent naming collisions.

- **The Core Concept:** You cannot have two classes named `Greeting.java` in the same folder. Packages allow identical class names to exist safely in different directories (e.g., `com.project.utils` vs. `com.project.core`).
- **Importing:** To use a class from another package, you must explicitly bring it in using the `import` statement.

---

## 2. The `static` Keyword (The "Class vs. Object" Rule)

When a variable or method is declared `static`, it belongs to the **Class blueprint itself**, not to any individual object.

### Static Variables
- Shared across all instances. If one object changes a static variable, that change is reflected globally for all objects.
- **Best Practice:** Always access them using the Class name (e.g., `Human.population`), not an object reference.

### Static Methods (Like `main`)
- Can be executed **without creating an object** of the class.
- **The Golden Rule:** A static method cannot directly access non-static data. Non-static data requires an object to exist in memory, but a static method can run before any objects are ever created.
- **The `this` Keyword:** You cannot use `this` inside a static method, because `this` points to a specific object, and static methods are object-independent.

```java
class Human {
    String name;
    static long population; // Shared property

    public Human(String name) {
        this.name = name;
        Human.population += 1; // Access via Class name
    }
}

// Usage
Human h1 = new Human("Kunal");
Human h2 = new Human("Rahul");
System.out.println(Human.population); // Output: 2
```

---

## 3. Static Initialization Blocks (The Memory Timeline)

A static block is a chunk of code that runs **exactly once**, the very millisecond the class is loaded into memory — before any objects are created.

- **Why use it?** When initializing a static variable requires complex logic (like a math calculation or fetching initial configuration data).
- **Interview Application:** In a project like a Simon-Game, you would use a static block to load global configurations (like the all-time highest score across all players) into memory just once, while the regular constructor handles setup for individual, localized game sessions.

```java
class StaticDemo {
    static int a = 4;
    static int b;

    // This block runs only once when the class is loaded
    static {
        System.out.println("I am in a static block");
        b = a * 5;
    }
}
```

---

## 4. Inner Classes: Static vs. Non-Static (The "Hidden Pointer" Secret)

A top-level, outer class can **never** be static. But when you nest a class inside another, `static` determines how memory is managed.

### Non-Static Inner Class (Dependent on the Object)
- **The "Why":** The Java compiler secretly injects a **hidden pointer** (reference) into the inner class that connects it directly to the outer class's object in memory.
- **The Result:** It must be tied to a specific instance of the outer class. Because of that hidden pointer, it can easily access all the private/non-static variables of the outer object.
- **Real-world analogy:** In a full-stack platform like Wanderlust, a `Room` class nested inside a `Hotel` class. A room cannot exist without a specific physical hotel, and it needs access to that hotel's data (like its address).

### Static Inner Class (Independent Namespace)
- **The "Why":** Adding `static` tells the compiler **not** to generate that hidden pointer. It is completely independent in memory and just uses the outer class as an organizational folder.
- **The Result:** You can instantiate it without creating the outer object first. However, because it lacks the hidden pointer, it has **zero access** to the outer class's non-static instance variables.
- **Real-world analogy:** A `BookingPolicy` class nested inside `Hotel`. The policy applies to the general concept of the hotel chain, not one specific building, so it doesn't need access to instance-specific data.

---

## 5. Internal Working of `System.out.println` & `toString()`

- **`System.out.println` breakdown:**
  - `System` → a standard class
  - `out` → a `static` reference variable (of type `PrintStream`) inside the `System` class
  - `println()` → a method belonging to `PrintStream`

- **The `toString()` Method:** Whenever you try to print an object reference directly, Java automatically calls its `toString()` method. By default, this prints a memory hashcode. To display readable data, you must **override** `toString()` in your custom class.

---

## 6. The Singleton Class Pattern

A design pattern used to restrict a class so that **only one single object** can ever be created from it.

### Implementation Steps

1. Make the constructor `private` so external classes cannot use the `new` keyword.
2. Create a `private static` variable of the class type to store the single instance.
3. Expose a `public static` method (e.g., `getInstance()`) that checks if the instance is `null`. If it is, it creates the object. If it isn't, it simply returns the existing object.

> **The Result:** No matter how many reference variables call `getInstance()`, they will all point to the exact same object in memory.

```java
public class Singleton {

    // 1. Private static variable to hold the single instance
    private static Singleton instance;

    // 2. Private constructor prevents instantiation from other classes
    private Singleton() {
    }

    // 3. Public static method to get the single instance
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // Created only once
        }
        return instance;
    }
}
```

---

