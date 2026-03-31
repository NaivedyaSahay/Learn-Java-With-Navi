# Multiple Inheritance in Java

## 🚨 The Root Problem
Java does **not support Multiple Inheritance** directly through classes.

### ❗ The Diamond Problem
If:
- Class `C` extends both `A` and `B`
- And both `A` and `B` have a method `fun()`

👉 The compiler gets confused:  
**Which `fun()` should be called?**

### ✅ The Fix
Java uses **Interfaces** to safely achieve multiple inheritance.

---

# 1. Abstract Classes

Think of an **Abstract Class** as a *template*.  
It tells the child class **what to do**, but not **how to do it**.

## 🔑 Key Rules & Characteristics

- **Declaration:** Use `abstract` keyword  
- **Abstract Methods:** No body  
- **Implementation:** Child class must override all abstract methods  
- **Object Creation:** ❌ Cannot create objects  
- **Constructors:** ✅ Allowed  
- **Static Methods:** ✅ Allowed  
- **Abstract Static Methods:** ❌ Not allowed  
- **Final Keyword:** ❌ Cannot be `final`

---

## 💻 Code Example: Abstract Class

```java
abstract class Parent {
    int age;

    public Parent(int age) {
        this.age = age;
    }

    abstract void career(String name);
    abstract void partner(String name);

    void normalMethod() {
        System.out.println("This is a normal method");
    }
}

class Son extends Parent {
    public Son(int age) {
        super(age);
    }

    @Override
    void career(String name) {
        System.out.println("I am going to be a " + name);
    }

    @Override
    void partner(String name) {
        System.out.println("I love " + name);
    }
}
```

---

# 2. Interfaces

Interfaces are **pure contracts**.

## 🔑 Key Rules & Characteristics

- **Keyword:** `interface`
- **Implementation:** `implements`
- **Variables:** `public static final`
- **Methods:** `public abstract`
- **Object Creation:** ❌ Not allowed
- **Multiple Inheritance:** ✅ Supported

---

## 💻 Code Example: Interfaces

```java
interface Engine {
    int PRICE = 78000;
    void start();
    void stop();
}

interface Brake {
    void brake();
}

class Car implements Engine, Brake {

    @Override
    public void start() {
        System.out.println("I start like a normal car engine.");
    }

    @Override
    public void stop() {
        System.out.println("I stop like a normal car engine.");
    }

    @Override
    public void brake() {
        System.out.println("I brake like a normal car.");
    }
}
```

---

# 3. Advanced Interface Concepts (Java 8+)

## 🔹 Default Methods

```java
interface A {
    void greet();

    default void fun() {
        System.out.println("I am a default method in Interface A");
    }
}
```

---

## 🔹 Static Methods

```java
InterfaceName.methodName();
```

---

# 🎯 Interview Cheat Sheet

| Feature | Abstract Class | Interface |
|--------|---------------|----------|
| Inheritance | Single | Multiple |
| Methods | Abstract + Concrete | Abstract (default/static allowed) |
| Variables | Any type | public static final |
| Access Modifiers | public, protected, private | public only |
| Constructors | Allowed | Not allowed |

---

# 🧠 When to Use?

### Use Abstract Class:
- Shared base

### Use Interface:
- Shared behavior
