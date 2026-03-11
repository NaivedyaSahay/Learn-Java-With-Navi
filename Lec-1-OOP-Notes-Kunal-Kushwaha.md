# ☕ Java Object-Oriented Programming (OOP) - Lecture 1

Welcome to the foundational concepts of Java OOP. These notes cover the building blocks of how objects are created and managed in memory.

---

## 🏗️ 1. Classes and Objects

| Concept | Description | Example |
| :--- | :--- | :--- |
| **Class** | A **blueprint** or logical template. It occupies no memory space. | `class Student { ... }` |
| **Object** | A **physical reality** (instance). It resides in the **Heap** memory. | `Student s1 = new Student();` |
| **Dot Operator** | Used to access instance variables or methods. | `s1.name = "Kunal";` |

### 💻 Code Example: Basic Class
```java
class Student {
    int rollNo;
    String name;
    float marks;
}

// In Main method:
Student s1 = new Student(); // Creating an Object
s1.name = "Kunal";          // Using Dot Operator


🧠 2. Memory ManagementJava handles memory dynamically to keep performance high. 🚀Stack Memory: Stores reference variables (e.g., the variable s1).Heap Memory: Stores the actual objects and their values.Dynamic Allocation: Using the new keyword, memory is allocated at Runtime.Garbage Collection 🗑️: Java's automatic process of deleting objects in the Heap that no longer have any reference in the Stack.[!TIP]Check out the logic: If s1 = null;, the object in the Heap becomes eligible for Garbage Collection because its "leash" to the Stack is cut.


⚡ 3. Constructors and thisA Constructor is a special method used to initialize objects. It matches the class name exactly.Default Constructor: Provided automatically if you don't define any.this Keyword: Refers to the current object instance. Essential for distinguishing between parameters and instance variables.Constructor Overloading: Creating multiple constructors with different parameter lists.Constructor Chaining: Using this() to call one constructor from another (must be the first line).💻 Code Example: Constructor OverloadingJavaclass Student {
    int rollNo;
    String name;

    // Parameterized Constructor
    Student(int roll, String n) {
        this.rollNo = roll;
        this.name = n;
    }

    // Constructor Chaining (Calls the one above)
    Student() {
        this(0, "Default Name");
    }
}


🔒 4. Key ModifiersModifierTargetEffectfinalPrimitivePrevents value change (Constant).finalObject RefPrevents pointing to a new object; internal data can still change.🛠️ Summary TableKeywordPurposenewAllocates memory in the Heap at runtime.thisPoints to the current instance.finalPrevents reassignment.