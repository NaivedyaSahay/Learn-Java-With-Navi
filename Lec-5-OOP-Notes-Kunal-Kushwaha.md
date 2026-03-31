# Advanced OOP in Java — In-Depth Notes
### Topic: Abstract Classes, Interfaces & Polymorphism

---

## Table of Contents
1. [Why Multiple Inheritance is Blocked in Java](#1-why-multiple-inheritance-is-blocked-in-java)
2. [Abstract Classes — Deep Dive](#2-abstract-classes--deep-dive)
3. [Interfaces — Deep Dive](#3-interfaces--deep-dive)
4. [Modern Java Interface Features (Java 8+)](#4-modern-java-interface-features-java-8)
5. [Method Overriding & Runtime Polymorphism](#5-method-overriding--runtime-polymorphism--in-depth)
6. [Nested Interfaces](#6-nested-interfaces)
7. [Abstract Class vs Interface — Complete Comparison](#7-abstract-class-vs-interface--complete-comparison)
8. [Key Takeaways & Best Practices](#8-key-takeaways--best-practices)

---

## 1. Why Multiple Inheritance is Blocked in Java

Java deliberately forbids a class from extending more than one class. The reason is the **Diamond Problem**: if class `C` extends both `A` and `B`, and both `A` and `B` define a method `display()`, the compiler cannot decide which version to use in `C`. To avoid this ambiguity, Java allows only **single class inheritance** but compensates with interfaces.

```
         A (display)
        / \
       B   C      ← both inherit display() from A
        \ /
         D         ← which display() does D use? Ambiguous!
```

> **Java's solution:** You can `extend` only one class, but `implement` many interfaces.

---

## 2. Abstract Classes — Deep Dive

### What is an Abstract Class?

An abstract class is a **partially implemented class** that acts as a template. It says *what* its children must do, without necessarily saying *how*. The keyword `abstract` must appear both on the class and on any methods that have no body.

```java
abstract class Shape {
    String color;                           // instance variable — allowed

    Shape(String color) {                   // constructor — allowed
        this.color = color;
    }

    abstract double area();                 // abstract method — no body, MUST override
    abstract double perimeter();

    void describe() {                       // concrete method — has a body
        System.out.println("Color: " + color);
    }

    static void info() {                    // static method — allowed
        System.out.println("I am a Shape");
    }
}
```

### Rules — Memorise These

| Rule | Explanation |
|---|---|
| Cannot instantiate | `new Shape()` → compile error |
| If one method is abstract, class must be abstract | Even one `abstract` method forces `abstract` on the class |
| Child must override all abstract methods | Unless the child is also declared `abstract` |
| Can have constructors | Called via `super()` in child — but can't be used to make objects of the abstract class itself |
| Can have static methods | Not part of polymorphism, but valid |
| Can have instance variables | Unlike interfaces |
| Single inheritance only | `class A extends B, C` → compile error |

### `super` Keyword with Abstract Classes

When a child class calls `super()`, it invokes the abstract parent's constructor. This is how shared initialization (like setting a `color` field) happens.

```java
class Circle extends Shape {
    double radius;

    Circle(String color, double radius) {
        super(color);               // calls Shape's constructor
        this.radius = radius;
    }

    @Override
    double area() { return Math.PI * radius * radius; }

    @Override
    double perimeter() { return 2 * Math.PI * radius; }
}
```

### When to Use Abstract Classes

- When multiple child classes **share common code or state** (fields, concrete methods)
- When you want to enforce that children implement certain behaviors
- When there is a strong **"is-a"** relationship (a `Circle` IS-A `Shape`)

---

## 3. Interfaces — Deep Dive

### What is an Interface?

An interface is a **pure contract** — it defines *what* a class must do, with no shared state. Before Java 8, interfaces could only have abstract methods. After Java 8, they can also have `default` and `static` methods.

```java
interface Drawable {
    int MAX_SIZE = 1000;       // implicitly: public static final
    void draw();               // implicitly: public abstract
    void resize(int factor);   // implicitly: public abstract
}
```

### Key Characteristics

| Feature | Detail |
|---|---|
| Methods | Implicitly `public abstract` (unless default/static) |
| Variables | Implicitly `public static final` — constants only, not instance variables |
| Keyword used | `implements` (not `extends`) |
| Constructors | Not allowed — no object creation |
| Multiple | A class can `implement` as many interfaces as needed |
| Interface extending interface | Uses `extends`, not `implements` |

### Multiple Interfaces — The Solution to the Diamond Problem

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

interface Runnable {
    void run();
}

class Duck implements Flyable, Swimmable, Runnable {
    public void fly()  { System.out.println("Duck flies"); }
    public void swim() { System.out.println("Duck swims"); }
    public void run()  { System.out.println("Duck runs");  }
}
```

The class `Duck` is now bound by three contracts. The compiler ensures all methods are implemented — no ambiguity because interfaces don't carry method bodies (except `default` methods, handled separately).

### Interface Extending Another Interface

```java
interface Animal {
    void eat();
}

interface Pet extends Animal {    // interface CAN extend another interface
    void play();
}

class Dog implements Pet {
    public void eat()  { System.out.println("Dog eats"); }
    public void play() { System.out.println("Dog plays"); }
    // must implement BOTH eat() and play()
}
```

---

## 4. Modern Java Interface Features (Java 8+)

### Default Methods

**Problem they solve:** Before Java 8, adding a new method to an interface broke all existing implementing classes — they'd all need to be updated. Default methods let you add a method with a body directly in the interface, so existing classes don't break.

```java
interface Vehicle {
    void start();

    default void fuelStatus() {           // default method — has a body
        System.out.println("Fuel status: OK");
    }
}

class Car implements Vehicle {
    public void start() { System.out.println("Car started"); }
    // fuelStatus() is inherited automatically — no need to override
}

class Truck implements Vehicle {
    public void start() { System.out.println("Truck started"); }

    @Override
    public void fuelStatus() {            // can override if needed
        System.out.println("Checking diesel level...");
    }
}
```

**Conflict rule:** If two interfaces both provide a `default` method with the same name, the implementing class **must** override it to resolve the conflict — otherwise the compiler throws an error.

```java
interface A { default void greet() { System.out.println("Hello from A"); } }
interface B { default void greet() { System.out.println("Hello from B"); } }

class C implements A, B {
    @Override
    public void greet() { A.super.greet(); }  // must explicitly pick one
}
```

### Static Interface Methods

Static methods in interfaces belong to the interface itself, not to any implementing class. They **cannot be overridden**, and must be called using the interface name.

```java
interface MathOps {
    static int square(int n) { return n * n; }
    static int cube(int n)   { return n * n * n; }
}

// Calling:
int s = MathOps.square(5);  // correct
// int s = someObject.square(5); // ERROR — can't call via object
```

### Annotations

Annotations are **metadata tags** placed on code to give instructions to the compiler or frameworks. They don't change logic directly.

| Annotation | Purpose |
|---|---|
| `@Override` | Tells compiler this method overrides a parent method — catches typos |
| `@FunctionalInterface` | Marks an interface with exactly one abstract method (for lambdas) |
| `@Deprecated` | Marks a method as outdated; compiler warns on use |
| `@SuppressWarnings` | Silences specific compiler warnings |

```java
class Animal {
    void sound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override                        // compile error if no matching parent method
    void sound() { System.out.println("Bark"); }
}
```

---

## 5. Method Overriding & Runtime Polymorphism — In Depth

### What is Overriding?

When a child class provides its own implementation of a method already defined in the parent, that's overriding. The method signature (name + parameters) must match exactly.

```java
abstract class Animal {
    abstract void sound();      // parent declares contract
    void breathe() { System.out.println("Breathing..."); }
}

class Dog extends Animal {
    @Override
    void sound() { System.out.println("Bark"); }
}

class Cat extends Animal {
    @Override
    void sound() { System.out.println("Meow"); }
}
```

### Runtime Polymorphism (Dynamic Method Dispatch)

The actual method that runs is determined **at runtime**, not compile time. The reference type can be the parent, but the object can be a child — Java picks the child's version automatically.

```java
Animal a;

a = new Dog();
a.sound();       // prints "Bark"  — decided at runtime

a = new Cat();
a.sound();       // prints "Meow" — decided at runtime
```

This is the core of polymorphism. The variable `a` is declared as `Animal`, but the JVM looks at what `a` actually points to and calls that object's version of `sound()`.

### Overriding Rules

| Rule | Detail |
|---|---|
| Method name and params must match exactly | Otherwise it's overloading, not overriding |
| Return type must be same or covariant | Covariant = a subtype of original return type |
| Access modifier can be same or broader | Can go from `protected` to `public`, never narrower |
| Cannot override `static` methods | Can only hide them (different concept) |
| Cannot override `final` methods | `final` locks the method from being changed |
| `@Override` annotation recommended | Compiler catches errors if signature doesn't match |

### Static Methods — Cannot Be Overridden

```java
class Parent {
    static void show() { System.out.println("Parent"); }
}

class Child extends Parent {
    static void show() { System.out.println("Child"); }  // hiding, NOT overriding
}

Parent p = new Child();
p.show();    // prints "Parent" — resolved at COMPILE time (no polymorphism)
```

Static methods are resolved by the **reference type** at compile time, not by the actual object at runtime. That's why they don't participate in polymorphism.

---

## 6. Nested Interfaces

A nested interface is an interface declared **inside a class or another interface**. It is used to logically group related functionality and improve encapsulation.

```java
class PaymentGateway {

    interface PaymentMethod {        // nested inside PaymentGateway
        void processPayment(double amount);
        void refund(double amount);
    }

    class CreditCard implements PaymentMethod {
        public void processPayment(double a) { System.out.println("Charged " + a); }
        public void refund(double a)         { System.out.println("Refunded " + a); }
    }
}

// Usage:
PaymentGateway.PaymentMethod method = new PaymentGateway().new CreditCard();
method.processPayment(500.0);
```

> A nested interface inside another interface is implicitly `public static`, so it can be referenced from outside without creating an instance of the outer interface.

---

## 7. Abstract Class vs Interface — Complete Comparison

| Feature | Abstract Class | Interface |
|---|---|---|
| Keyword | `abstract class` | `interface` |
| Implemented with | `extends` | `implements` |
| Multiple inheritance | One only | Multiple allowed |
| Constructors | Yes | No |
| Instance variables | Yes | No (only `public static final` constants) |
| Abstract methods | Yes | Yes (default) |
| Concrete methods | Yes | Only `default` and `static` |
| Access modifiers | Any | All methods implicitly `public` |
| Can have `static` methods | Yes | Yes (Java 8+) |
| Object creation | Cannot instantiate | Cannot instantiate |
| Best for | Shared code + partial implementation | Pure behavior contract |

---

## 8. Key Takeaways & Best Practices

**Design rule of thumb:** Ask yourself — do the classes share *state* (instance variables)? Use an abstract class. Do they just share *behavior contracts* with no shared data? Use an interface.

**Loose coupling with interfaces:** By programming to an interface rather than a class, you can swap implementations without changing dependent code. This is a foundational principle of good software design.

```java
// Tight coupling (bad) — specific class
Dog d = new Dog();

// Loose coupling (good) — interface reference
Animal a = new Dog();   // can change to new Cat() without touching the rest of the code
```

### Summary of the Inheritance Landscape in Java

- A class can `extend` exactly **one** class (abstract or concrete)
- A class can `implement` **unlimited** interfaces
- An interface can `extend` **multiple** interfaces
- Abstract classes and interfaces **cannot be instantiated** — they only exist to be extended or implemented

### Quick-Reference Cheat Sheet

```
Abstract Class                      Interface
──────────────────────────────────  ──────────────────────────────────
abstract class Foo { ... }          interface Bar { ... }
class Child extends Foo { ... }     class Child implements Bar, Baz { ... }
Can have fields & constructors      Only constants (static final)
Single inheritance                  Multiple inheritance
Use when: shared state/behavior     Use when: pure contract / multiple types
```

---

*Keywords: Inheritance · Abstract Class · Interface · Method Overriding · Runtime Polymorphism · Java · Multiple Inheritance · Static Method · OOP · Dynamic Method Dispatch*
