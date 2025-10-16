# Week 2 - CO5625 C++ Further Programming and Algorithms

## Lecture: C++ Functions and Classes - Code Reuse Techniques

### Session Overview
**Duration:** 90 minutes
- **Lecture Content:** 65 minutes
- **Worksheet Activities:** 20 minutes
- **Break:** 5 minutes

---

## Learning Objectives

By the end of this lecture, students will be able to:

1. **Explain** the concept of code reuse and why it is important in software development (Understanding)
2. **Declare and implement** functions with appropriate parameters and return types (Application)
3. **Trace** the order of execution through multiple function calls and explain the call stack (Analysis)
4. **Design and implement** classes with appropriate attributes and behaviors (Application)
5. **Differentiate** between public and private access modifiers and apply them appropriately (Analysis)
6. **Create** derived classes using inheritance to extend functionality (Application)
7. **Apply** code reuse techniques to solve real-world programming problems (Synthesis)

---

## Section 1: Introduction and Code Reuse Concepts (10 minutes)

### 1.1 Why Code Reuse Matters

**Key Principle:** *Don't Repeat Yourself (DRY)*

Code reuse is fundamental to professional software development because:

- **Reduces redundancy:** Writing the same code multiple times is inefficient and error-prone
- **Improves maintainability:** Changes need to be made in only one place
- **Enhances reliability:** Well-tested reusable code is more trustworthy
- **Saves time:** Focus on solving new problems rather than rewriting solutions
- **Promotes consistency:** The same operation behaves identically across the codebase

### 1.2 Encapsulation Through Interfaces

An **interface** is a contract that defines how other code can interact with your code without needing to know the implementation details.

**Example Scenario:**
```cpp
// Interface: How to use the function
double radius(double circumference);

// Usage: Another programmer doesn't need to know HOW it works
double circOf2pCoin = 25.91;  // Circumference in mm
double radiusOf2pCoin = radius(circOf2pCoin);
```

**Key Concept:** The function signature tells you:
- What you need to provide (input parameters)
- What you'll get back (return type)
- How to call it (function name)

### 1.3 Mechanisms for Code Reuse in C++

We achieve code reuse primarily through:
1. **Functions** - Encapsulate algorithms and operations
2. **Classes** - Encapsulate data and related operations
3. **Inheritance** - Extend and specialize existing code (covered later)

---

## Section 2: Functions in Detail (15 minutes)

### 2.1 Function Anatomy

**Complete Function Structure:**

```cpp
return_type function_name(parameter_type1 parameter_name1, parameter_type2 parameter_name2)
{
    // Function body - local scope
    // Perform operations
    return value_of_return_type;
}
```

**Concrete Example:**
```cpp
double radius(double circumference)
{
    // Note: This is a local scope - variables here don't exist outside
    const double PI = 3.14159;
    return circumference / (2 * PI);
}
```

### 2.2 Function Declaration vs. Definition

**Declaration (Prototype):** Tells the compiler the function exists
```cpp
double radius(double circumference);  // Just the signature
```

**Definition:** Provides the actual implementation
```cpp
double radius(double circumference)
{
    return circumference / (2 * 3.14159);
}
```

**Why separate them?**
- Declarations typically go in header files (.h)
- Definitions go in implementation files (.cpp)
- Allows functions to be called before they're fully defined

### 2.3 Parameters and Return Values

**By Value Parameters:**
```cpp
void printSquare(int number)
{
    int square = number * number;  // Local copy of number
    std::cout << square << std::endl;
}
// Original 'number' passed to function is unchanged
```

**Return Values:**
```cpp
int calculateSquare(int number)
{
    return number * number;  // Returns a value to caller
}

int main()
{
    int result = calculateSquare(5);  // result = 25
    return 0;
}
```

### 2.4 Order of Execution and the Call Stack

**Critical Concept:** The call stack tracks function calls

**Example Code Analysis:**
```cpp
#include <iostream>
using std::cout;

void print1()
{
    cout << "S";
}

int print2(int x)
{
    cout << "H";
    return x-2;
}

void print3(int z)
{
    int y = print2(4);
    for(int i=0; i<y+2; ++i)
        cout << "O";
}

int main()
{
    cout << "A";
    print3(4);
    print2(3);
    print1();
    return 0;
}
```

**Execution Trace:**
1. `main()` executes → prints "A"
2. `print3(4)` is called
   - Stack: `[main] [print3]`
3. Inside `print3`, `print2(4)` is called
   - Stack: `[main] [print3] [print2]`
   - `print2` prints "H", returns 2
   - Stack: `[main] [print3]`
4. Loop executes with `y=2`, so `i` goes from 0 to 3 → prints "OOOO"
5. `print3` returns
   - Stack: `[main]`
6. `print2(3)` is called → prints "H", returns 1
7. `print1()` is called → prints "S"
8. Program ends

**Output:** `AHOOOOHS`
**Maximum Stack Depth:** 3 functions (main, print3, print2)

**Key Learning Point:** Functions are called in a Last-In-First-Out (LIFO) manner - the most recently called function must complete before the previous one resumes.

---

## Section 3: Classes - Object-Oriented Programming (25 minutes)

### 3.1 What Are Classes?

**Multiple Perspectives:**

1. **Blueprint Perspective:** A class is a blueprint for creating objects
2. **Encapsulation Perspective:** A class bundles related data and functions together
3. **State Machine Perspective:** A class is a collection of functions sharing common state

**Fundamental Structure:**
```cpp
class ClassName
{
private:
    // Member variables (attributes) - internal state
    
public:
    // Member functions (methods/behaviors)
};  // CRITICAL: Don't forget the semicolon!
```

### 3.2 Building a Circle Class

**Step 1: Class Declaration**

```cpp
class Circle
{
private:
    double radius;  // The internal state
    
public:
    Circle() : radius(0) {}  // Constructor with initializer list
    
    void setRadius(double rad);
    double getRadius();
    double getArea();
    double getCircumference();
    double getDiameter();
    void scale(double factor);
};
```

**Key Points:**
- **Private members:** Cannot be accessed outside the class (data hiding)
- **Public members:** Form the interface for using the class
- **Constructor:** Special function that initializes the object

### 3.3 Constructors in Detail

**Default Constructor:**
```cpp
Circle() : radius(0) {}
```

**Breaking down the syntax:**
- `Circle()` - Constructor has the same name as the class, no return type
- `: radius(0)` - Initializer list (preferred way to initialize members)
- `{}` - Empty body (initialization is done in the list)

**Alternative (less preferred):**
```cpp
Circle()
{
    radius = 0;  // Assignment in body
}
```

**Why initializer lists are better:**
- More efficient (direct initialization vs. default construction + assignment)
- Required for const members and references
- Required for base class initialization (shown later)

### 3.4 Implementing Member Functions

**Two Approaches:**

**Approach 1: Inline Definition (inside class)**
```cpp
class Circle
{
private:
    double radius;
    
public:
    Circle() : radius(0) {}
    
    double getArea()
    {
        return radius * radius * 3.14159;  // Direct access to radius
    }
};
```

**Approach 2: Separate Implementation (outside class)**
```cpp
// In header file (Circle.h)
class Circle
{
private:
    double radius;
    
public:
    Circle() : radius(0) {}
    double getArea();  // Declaration only
};

// In implementation file (Circle.cpp)
double Circle::getArea()  // Scope resolution operator ::
{
    return radius * radius * 3.14159;
}
```

**The `::` operator:** Tells the compiler "this function belongs to the Circle class"

### 3.5 Complete Circle Class Implementation

```cpp
class Circle
{
private:
    double radius;
    
public:
    // Constructors
    Circle() : radius(0) {}
    
    Circle(double r) : radius(r) {}  // Parameterized constructor
    
    // Getters
    double getRadius() { return radius; }
    
    double getArea() { return radius * radius * 3.14159; }
    
    double getCircumference() { return 2 * 3.14159 * radius; }
    
    double getDiameter() { return 2 * radius; }
    
    // Setters
    void setRadius(double rad) 
    { 
        if (rad >= 0)  // Validation!
            radius = rad; 
    }
    
    // Modifiers
    void scale(double factor) 
    { 
        radius *= factor; 
    }
};
```

**Important Observations:**
- All methods have access to `radius` without passing it as a parameter
- We can have multiple constructors (constructor overloading)
- Setters can include validation logic
- The class maintains its own internal state

### 3.6 Using the Circle Class

**Creating and Using Objects:**

```cpp
#include <iostream>

int main()
{
    // Create a Circle object using default constructor
    Circle c1;
    std::cout << "c1 radius: " << c1.getRadius() << std::endl;  // 0
    
    // Create a Circle with initial radius
    Circle c2(5.0);
    std::cout << "c2 area: " << c2.getArea() << std::endl;  // 78.54
    
    // Modify the circle
    c2.scale(2.0);  // Double the radius
    std::cout << "c2 new area: " << c2.getArea() << std::endl;  // 314.16
    
    // Set radius explicitly
    c1.setRadius(3.0);
    std::cout << "c1 circumference: " << c1.getCircumference() << std::endl;
    
    return 0;
}
```

**The dot operator (`.`):** Used to access members of an object

**Memory Perspective:**
- `c1` and `c2` are separate objects with their own `radius` values
- Each object maintains independent state
- Methods operate on the specific object they're called on

---

## Section 4: Inheritance - Extending Functionality (12 minutes)

### 4.1 The Inheritance Concept

**Definition:** Inheritance allows a class to acquire properties and behaviors from another class.

**Relationship:** "Is-a" relationship
- A Car **is a** Vehicle
- A Sphere **is a** (kind of extended) Circle

**Benefits:**
- Code reuse without copying
- Logical hierarchy
- Polymorphism (covered in future lectures)

### 4.2 Basic Inheritance Syntax

```cpp
class DerivedClass : public BaseClass
{
    // Additional members
};
```

**Example: Sphere inheriting from Circle**

```cpp
class Sphere : public Circle
{
public:
    // Constructor that calls base constructor
    Sphere() : Circle() {}
    
    Sphere(double r) : Circle(r) {}
    
    // New methods specific to Sphere
    double getVolume();
    double getSurfaceArea();
};
```

**Key Points:**
- `public Circle` means public inheritance (most common)
- Sphere automatically has all public members of Circle
- Sphere adds new functionality specific to 3D objects

### 4.3 Implementing Derived Class Methods

```cpp
// Sphere inherits radius from Circle
double Sphere::getVolume()
{
    double r = getRadius();  // Can call inherited methods
    return (4.0/3.0) * 3.14159 * r * r * r;
}

double Sphere::getSurfaceArea()
{
    double r = getRadius();
    return 4 * 3.14159 * r * r;
}
```

**What Sphere Has:**
- **Inherited from Circle:**
  - `radius` (private, but accessible via getters)
  - `setRadius()`, `getRadius()`, `getArea()`, `getCircumference()`, etc.
- **New to Sphere:**
  - `getVolume()`
  - `getSurfaceArea()`

### 4.4 Using Inherited Classes

```cpp
int main()
{
    Sphere s(3.0);
    
    // Can use Circle methods
    std::cout << "Radius: " << s.getRadius() << std::endl;
    std::cout << "Circle area: " << s.getArea() << std::endl;
    
    // Can use Sphere methods
    std::cout << "Volume: " << s.getVolume() << std::endl;
    std::cout << "Surface area: " << s.getSurfaceArea() << std::endl;
    
    // Can use Circle modifiers
    s.scale(2.0);
    std::cout << "New volume: " << s.getVolume() << std::endl;
    
    return 0;
}
```

### 4.5 Access Control in Inheritance

**Three access specifiers:**

| Specifier | Accessible from class itself | Accessible from derived class | Accessible from outside |
|-----------|------------------------------|-------------------------------|-------------------------|
| `public` | ✓ | ✓ | ✓ |
| `protected` | ✓ | ✓ | ✗ |
| `private` | ✓ | ✗ | ✗ |

**Example with protected:**
```cpp
class Circle
{
protected:  // Changed from private
    double radius;
    
public:
    Circle() : radius(0) {}
    Circle(double r) : radius(r) {}
    // ... methods ...
};

class Sphere : public Circle
{
public:
    double getVolume()
    {
        // Can now access radius directly!
        return (4.0/3.0) * 3.14159 * radius * radius * radius;
    }
};
```

**Best Practice:** Use `protected` when you expect classes to be inherited and derived classes might need direct access to members.

---

## Section 5: Summary and Key Takeaways (3 minutes)

### Core Concepts Covered

1. **Code Reuse Principles**
   - DRY principle
   - Encapsulation through interfaces
   - Separation of declaration and implementation

2. **Functions**
   - Structure: return type, name, parameters
   - Declaration vs. definition
   - Call stack and order of execution

3. **Classes**
   - Bundling data and methods
   - Public vs. private access
   - Constructors and initialization
   - Member function implementation
   - Object instantiation and usage

4. **Inheritance**
   - Code reuse through class hierarchies
   - Base and derived classes
   - Access control (public, protected, private)
   - Extending functionality

### Connecting to Real-World Development

These concepts are foundational to:
- **Large-scale software development:** Organizing code into maintainable units
- **API design:** Creating clear interfaces for other developers
- **Framework development:** Building extensible class hierarchies
- **Team collaboration:** Understanding and extending others' code

---

## Time Allocation and Learning Outcomes Summary

| Section | Duration | Primary Learning Outcomes Addressed |
|---------|----------|-------------------------------------|
| 1. Introduction & Code Reuse Concepts | 10 min | LO1: Explain code reuse importance |
| 2. Functions in Detail | 15 min | LO2: Declare/implement functions<br>LO3: Trace execution |
| 3. Classes - OOP | 25 min | LO4: Design/implement classes<br>LO5: Apply access modifiers |
| 4. Inheritance | 12 min | LO6: Create derived classes<br>LO7: Apply code reuse |
| 5. Summary & Q&A | 3 min | All LOs (reinforcement) |
| **Total Lecture Content** | **65 min** | |
| **Break** | **5 min** | |
| **Worksheet Activities** | **20 min** | Application of all concepts |
| **Total Session** | **90 min** | |

---

## Resources for Further Reading and Practice

### Official Documentation
- **C++ Reference:** https://en.cppreference.com/
  - Functions: https://en.cppreference.com/w/cpp/language/functions
  - Classes: https://en.cppreference.com/w/cpp/language/classes

### Recommended Textbooks
- **Stroustrup, B. (2013).** *The C++ Programming Language* (4th ed.). Addison-Wesley.
  - Chapters 2-3 (Functions and Classes)
- **Gregoire, M., Solter, N.A., Kleper, S.J. (2018).** *Professional C++* (4th ed.). Wrox.
  - Chapter 5 (Classes and Objects)

### Online Tutorials and Practice
- **LearnCpp.com:**
  - Functions: https://www.learncpp.com/cpp-tutorial/introduction-to-functions/
  - Classes: https://www.learncpp.com/cpp-tutorial/introduction-to-classes/
  - Inheritance: https://www.learncpp.com/cpp-tutorial/introduction-to-inheritance/

- **CppReference Tutorials:**
  - https://cplusplus.com/doc/tutorial/functions/
  - https://cplusplus.com/doc/tutorial/classes/

### Coding Practice Platforms
- **HackerRank C++ Domain:** https://www.hackerrank.com/domains/cpp
  - Focus on: Functions, Classes, Inheritance tracks
- **LeetCode:** https://leetcode.com/
  - Filter by "Easy" problems to practice class design
- **Exercism C++ Track:** https://exercism.org/tracks/cpp
  - Mentor-reviewed exercises on OOP concepts

### Video Resources
- **The Cherno C++ Series (YouTube):**
  - "How to Write Functions in C++"
  - "Classes in C++"
  - "Inheritance in C++"

### Key Concepts to Practice
1. Implement various mathematical function libraries (geometry, statistics, etc.)
2. Design class hierarchies for real-world entities (vehicles, animals, shapes)
3. Trace execution through complex function call chains
4. Convert procedural code to object-oriented designs

### Preparation for Next Session
- Review memory management concepts
- Practice with pointers and references
- Explore virtual functions and polymorphism in the textbook

---

**End of Lecture Content**