# Java OOP Notes

## 🏗️ 1. Classes and Objects

- **Class:** A blueprint or template. It is a logical construct and does not occupy memory (e.g., the idea of a `Student`).
- **Object:** A physical reality or instance of a class. It occupies space in the Heap memory (e.g., a specific student named `Naivedya`).
- **Dot Operator (`.`):** Used to link the object's reference variable to its specific instance variables or methods (e.g., `s1.name`).

### 💡 Example

```java
// Class = blueprint (no memory yet)
class Student {
    String name;
    int age;

    void greet() {
        System.out.println("Hi, I am " + name);
    }
}

// Object = actual instance (memory allocated in Heap)
Student s1 = new Student();   // s1 is stored in Stack, the object in Heap
s1.name = "Naivedya";            // dot operator links s1 to its 'name' field
s1.age  = 20;
s1.greet();                   // Output: Hi, I am Naivedya
```

---

## 🧠 2. Memory Management

- **Stack Memory:** Stores reference variables (the pointers to the objects).
- **Heap Memory:** Stores the actual objects and their data.
- **Dynamic Memory Allocation:** Happens at Runtime using the `new` keyword.
- **Garbage Collection:** Java automatically deletes objects in the Heap that have no references in the Stack. 🗑️

### 💡 Example

```java
Student s1 = new Student();   // Stack: s1 → Heap: { name=null, age=0 }
Student s2 = new Student();   // Stack: s2 → Heap: { name=null, age=0 }

s1 = null;   // s1 no longer points to its object
             // That Heap object has NO reference → eligible for Garbage Collection 🗑️
             // s2's object is still alive
```

```
Stack          Heap
-----          ----
s1 ──────────► Student { "Naivedya", 20 }   ← unreachable after s1 = null
s2 ──────────► Student { "Rohan", 21 }
```

---

## ⚡ 3. Constructors and `this`

- **Constructor:** A special method called when an object is created to initialize its properties. It has no return type and matches the class name.
- **Default Constructor:** Provided by Java automatically if you don't write any constructors.
- **`this` Keyword:** Refers to the current object instance. Used to distinguish between instance variables and parameters.
- **Constructor Overloading:** Defining multiple constructors with different parameters.
- **Constructor Chaining:** Using `this()` to call one constructor from another within the same class (must be the first line).

### 💡 Example — Basic Constructor + `this`

```java
class Student {
    String name;
    int age;

    // Constructor — same name as class, no return type
    Student(String name, int age) {
        this.name = name;   // 'this.name' = instance variable, 'name' = parameter
        this.age  = age;
    }
}

Student s1 = new Student("Naivedya", 20);
System.out.println(s1.name);  // Output: Naivedya
```

### 💡 Example — Constructor Overloading

```java
class Student {
    String name;
    int age;

    Student(String name) {           // Constructor 1 — only name
        this.name = name;
        this.age  = 18;              // default age
    }

    Student(String name, int age) {  // Constructor 2 — name + age
        this.name = name;
        this.age  = age;
    }
}

Student s1 = new Student("Naivedya");        // uses Constructor 1 → age defaults to 18
Student s2 = new Student("Rohan", 22);    // uses Constructor 2
```

### 💡 Example — Constructor Chaining with `this()`

```java
class Student {
    String name;
    int age;
    String college;

    Student(String name) {
        this(name, 18);           // chains to 2-param constructor (must be first line)
    }

    Student(String name, int age) {
        this(name, age, "MIT");   // chains to 3-param constructor (must be first line)
    }

    Student(String name, int age, String college) {
        this.name    = name;
        this.age     = age;
        this.college = college;
    }
}

Student s1 = new Student("Naivedya");
System.out.println(s1.college);   // Output: MIT
System.out.println(s1.age);       // Output: 18
```

---

## 🔒 4. Key Modifiers

- **`final` (Primitive):** Prevents the value from being changed (makes it a constant).
- **`final` (Object Reference):** Prevents the reference variable from pointing to a different object, but you can still change the data inside the object.

### 💡 Example — `final` on a Primitive

```java
final int MAX_MARKS = 100;
MAX_MARKS = 90;    // ❌ Compile Error — cannot reassign a final variable
```

### 💡 Example — `final` on an Object Reference

```java
final Student s1 = new Student("Naivedya", 20);

s1.name = "Rohan";              // ✅ Allowed — modifying data inside the object is fine
s1 = new Student("Rohan", 22);  // ❌ Compile Error — s1 cannot point to a new object
```

### 💡 Example — `new` keyword

```java
Student s1;          // just a reference in Stack, points to nothing (null)
s1 = new Student();  // 'new' allocates memory in Heap at runtime and returns a reference
```

---

## 📋 Quick Reference

| Keyword | Usage |
|---------|-------|
| `new` | Allocates memory in the Heap at runtime. |
| `this` | Points to the current instance. |
| `final` | Prevents reassignment. |
