# CO5073 Week 4a: Templates in C++

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Generic Programming](#section-1-introduction-to-generic-programming)
3. [Section 2: Template Functions](#section-2-template-functions)
4. [Section 3: Template Classes](#section-3-template-classes)
5. [Section 4: Template Specialization](#section-4-template-specialization)
6. [Section 5: Template Compilation and Best Practices](#section-5-template-compilation-and-best-practices)
7. [Section 6: Advanced Topics - Template Metaprogramming](#section-6-advanced-topics---template-metaprogramming)
8. [Resources](#resources)

---

## Learning Outcomes

By the end of this lecture, students should be able to:

1. Explain the concept of generic programming and why templates are necessary in C++
2. Write and use template functions to create type-independent functions
3. Design and implement template classes for generic data structures
4. Understand the difference between function overloading and template functions
5. Apply template syntax correctly with proper declaration and instantiation
6. Recognize and avoid common template-related compilation errors
7. Understand template specialization and its use cases
8. Appreciate the power of compile-time computation using templates

---

## Section 1: Introduction to Generic Programming

### What is Generic Programming?

Generic programming is a programming paradigm that focuses on writing code that works with multiple data types without sacrificing type safety or performance. Instead of writing separate implementations for each data type, we write a single generic implementation that can be adapted to work with any compatible type.

### The Problem Templates Solve

Consider the data structures we've studied:
- Arrays of integers
- Arrays of doubles
- Linked lists of strings
- Linked lists of custom objects

Each time we want to store a different data type, do we need to rewrite the entire data structure implementation? Without templates, the answer would be yes. This leads to:
- Code duplication
- Maintenance nightmares
- Increased chance of bugs
- Wasted development time

### Real-World Analogy

Think of a template as a blueprint or mold. A cookie cutter is a template - you can use the same cookie cutter shape with different types of dough (chocolate, vanilla, gingerbread) to produce cookies of the same shape but different "types."

### Type Safety vs. Flexibility

Templates provide both type safety and flexibility. Unlike void pointers (which can point to any type but lose type information), templates maintain strong typing while allowing code reuse.

```
┌─────────────────────────────────────────────────────────┐
│  Without Templates: Separate Implementation Per Type    │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  class IntArray { int* data; ... }                       │
│  class DoubleArray { double* data; ... }                 │
│  class StringArray { string* data; ... }                 │
│                                                           │
│  Problem: Code duplication, maintenance burden           │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  With Templates: Single Generic Implementation          │
├─────────────────────────────────────────────────────────┤
│                                                           │
│  template <class T>                                      │
│  class Array { T* data; ... }                            │
│                                                           │
│  Usage: Array<int>, Array<double>, Array<string>         │
│                                                           │
│  Benefit: Write once, use with any type                  │
└─────────────────────────────────────────────────────────┘
```

---

## Section 2: Template Functions

### Understanding Function Overloading First

Before templates, C++ programmers used function overloading to create type-specific versions of functions:

```cpp
#include <iostream>
#include <cmath>

// Three overloaded versions of the same function
float circleArea(float radius) {
    return radius * radius * 3.14159f;
}

double circleArea(double radius) {
    return radius * radius * 3.14159265358979;
}

long double circleArea(long double radius) {
    return radius * radius * 3.14159265358979323846L;
}

int main() {
    float f = 2.5f;
    double d = 2.5;
    long double ld = 2.5L;
    
    std::cout << "Float result: " << circleArea(f) << std::endl;
    std::cout << "Double result: " << circleArea(d) << std::endl;
    std::cout << "Long double result: " << circleArea(ld) << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Float result: 19.6349
Double result: 19.635
Long double result: 19.635
```

### The Problem with Overloading

While overloading works, notice that we've written essentially the same code three times. The logic is identical - only the type changes. This violates the DRY (Don't Repeat Yourself) principle.

### Template Function Syntax

A template function solves this by allowing us to write the function once and use it with multiple types:

```cpp
#include <iostream>

// Template function declaration and definition
template <class T>
T circleArea(T radius) {
    return radius * radius * 3.14159265358979323846L;
}

int main() {
    float f = 2.5f;
    double d = 2.5;
    long double ld = 2.5L;
    
    // Explicit template instantiation
    std::cout << "Float result: " << circleArea<float>(f) << std::endl;
    std::cout << "Double result: " << circleArea<double>(d) << std::endl;
    std::cout << "Long double result: " << circleArea<long double>(ld) << std::endl;
    
    // Implicit template instantiation (compiler deduces type)
    std::cout << "\nUsing type deduction:" << std::endl;
    std::cout << "Float result: " << circleArea(f) << std::endl;
    std::cout << "Double result: " << circleArea(d) << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Float result: 19.635
Double result: 19.635
Long double result: 19.635

Using type deduction:
Float result: 19.635
Double result: 19.635
```

### Breaking Down Template Syntax

```cpp
template <class T>
T circleArea(T radius)
```

- `template` - keyword indicating this is a template
- `<class T>` - template parameter declaration (T is a placeholder for any type)
- Note: `class` can be replaced with `typename` - they mean the same thing in this context
- `T circleArea(T radius)` - return type T, parameter type T

### Multiple Template Parameters

Templates can have multiple type parameters:

```cpp
#include <iostream>

template <class T1, class T2>
void printPair(T1 first, T2 second) {
    std::cout << "First: " << first << ", Second: " << second << std::endl;
}

int main() {
    printPair<int, double>(42, 3.14);
    printPair<std::string, int>("Age", 25);
    
    // With type deduction
    printPair(100, 2.5);  // T1=int, T2=double
    printPair("Hello", 'X');  // T1=const char*, T2=char
    
    return 0;
}
```

**Sample Output:**
```
First: 42, Second: 3.14
First: Age, Second: 25
First: 100, Second: 2.5
First: Hello, Second: X
```

### Rectangle Area Example

Here's a template function that calculates rectangle area with two parameters:

```cpp
#include <iostream>

template <class T>
T rectangleArea(T height, T width) {
    return height * width;
}

int main() {
    // Test with different types
    int intHeight = 5, intWidth = 10;
    float floatHeight = 5.5f, floatWidth = 10.2f;
    double doubleHeight = 5.75, doubleWidth = 10.25;
    
    std::cout << "Integer rectangle: " << rectangleArea(intHeight, intWidth) << std::endl;
    std::cout << "Float rectangle: " << rectangleArea(floatHeight, floatWidth) << std::endl;
    std::cout << "Double rectangle: " << rectangleArea(doubleHeight, doubleWidth) << std::endl;
    
    // What happens with strings?
    // This won't compile because strings don't support multiplication
    // std::string str1 = "Hello";
    // std::string str2 = "World";
    // rectangleArea(str1, str2);  // ERROR!
    
    return 0;
}
```

**Sample Output:**
```
Integer rectangle: 50
Float rectangle: 56.1
Double rectangle: 58.9375
```

### Template Constraints

Templates don't perform magic - they only work if the operations used in the template function are valid for the type being used. The rectangle example fails with strings because `std::string` doesn't support the multiplication operator.

```
┌────────────────────────────────────────────────────┐
│  Template Function Compilation Process             │
├────────────────────────────────────────────────────┤
│                                                     │
│  1. Write template function                        │
│  2. Call template with specific type               │
│  3. Compiler generates actual function             │
│  4. Compiler checks if operations are valid        │
│  5. If valid → code compiles                       │
│     If invalid → compilation error                 │
│                                                     │
└────────────────────────────────────────────────────┘
```

---

## Section 3: Template Classes

### Motivation for Template Classes

Just as template functions eliminate redundant function code, template classes eliminate redundant class code. Consider a dynamic array class - without templates, we'd need separate classes for `IntArray`, `DoubleArray`, `StringArray`, etc.

### Original Non-Template Dynamic Array

Let's start with a non-template version to see what we're improving:

```cpp
#include <iostream>

class dynArray {
public:
    dynArray(int size) : sz(size) {
        data = new int[sz];
        for(int i = 0; i < sz; i++) {
            data[i] = 0;
        }
    }
    
    virtual ~dynArray() {
        if(data) delete [] data;
    }
    
    int& operator[](int idx) {
        return data[idx];
    }
    
    int size() {
        return sz;
    }
    
private:
    int* data;
    int sz;
};

int main() {
    dynArray arr(10);
    arr[3] = 42;
    arr[7] = 100;
    
    std::cout << "Element 3: " << arr[3] << std::endl;
    std::cout << "Element 7: " << arr[7] << std::endl;
    std::cout << "Array size: " << arr.size() << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Element 3: 42
Element 7: 100
Array size: 10
```

This works fine for integers, but what if we need to store doubles? Or strings? Or custom objects?

### Template Class Syntax

Here's the template version that works with any type:

```cpp
#include <iostream>
#include <string>

template <class T>
class dynArray {
public:
    dynArray(int size) : sz(size) {
        data = new T[sz];
        // Initialize array elements to default value
        for(int i = 0; i < sz; i++) {
            data[i] = T();  // Default constructor for type T
        }
    }
    
    virtual ~dynArray() {
        if(data) delete [] data;
    }
    
    T& operator[](int idx) {
        return data[idx];
    }
    
    int size() {
        return sz;
    }
    
private:
    T* data;  // Now stores any type T
    int sz;
};

int main() {
    // Array of doubles
    dynArray<double> doubleArr(5);
    doubleArr[0] = 3.14;
    doubleArr[2] = 2.71;
    
    std::cout << "Double array:" << std::endl;
    for(int i = 0; i < doubleArr.size(); i++) {
        std::cout << "  [" << i << "] = " << doubleArr[i] << std::endl;
    }
    
    // Array of strings
    dynArray<std::string> stringArr(3);
    stringArr[0] = "Hello";
    stringArr[1] = "Template";
    stringArr[2] = "World";
    
    std::cout << "\nString array:" << std::endl;
    for(int i = 0; i < stringArr.size(); i++) {
        std::cout << "  [" << i << "] = " << stringArr[i] << std::endl;
    }
    
    // Array of integers
    dynArray<int> intArr(4);
    intArr[0] = 10;
    intArr[3] = 99;
    
    std::cout << "\nInteger array:" << std::endl;
    for(int i = 0; i < intArr.size(); i++) {
        std::cout << "  [" << i << "] = " << intArr[i] << std::endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Double array:
  [0] = 3.14
  [1] = 0
  [2] = 2.71
  [3] = 0
  [4] = 0

String array:
  [0] = Hello
  [1] = Template
  [2] = World

Integer array:
  [0] = 10
  [1] = 0
  [2] = 0
  [3] = 99
```

### Template Class Declaration Breakdown

```cpp
template <class T>
class dynArray {
    // ...
    T* data;        // Member variable of type T*
    T& operator[]   // Returns reference to T
    // ...
};
```

Every occurrence of `int` from the original class has been replaced with `T`. The template parameter `T` becomes a placeholder that will be replaced with an actual type when we instantiate the class.

### Using Template Classes

To create an instance of a template class, you must specify the type:

```cpp
dynArray<double> arr(10);  // T is replaced with double
```

The syntax `<double>` tells the compiler to generate a version of `dynArray` where every `T` is replaced with `double`.

### Template Rectangle Class

Here's a template class for rectangles that can store dimensions in any numeric type:

```cpp
#include <iostream>

template <class T>
class Rectangle {
public:
    Rectangle(T h, T w) : height(h), width(w) {}
    
    T area() const {
        return height * width;
    }
    
    T perimeter() const {
        return 2 * (height + width);
    }
    
    void setDimensions(T h, T w) {
        height = h;
        width = w;
    }
    
    T getHeight() const { return height; }
    T getWidth() const { return width; }
    
private:
    T height;
    T width;
};

int main() {
    // Integer rectangle
    Rectangle<int> intRect(5, 10);
    std::cout << "Integer Rectangle:" << std::endl;
    std::cout << "  Dimensions: " << intRect.getHeight() << " x " << intRect.getWidth() << std::endl;
    std::cout << "  Area: " << intRect.area() << std::endl;
    std::cout << "  Perimeter: " << intRect.perimeter() << std::endl;
    
    // Double rectangle
    Rectangle<double> doubleRect(5.5, 10.25);
    std::cout << "\nDouble Rectangle:" << std::endl;
    std::cout << "  Dimensions: " << doubleRect.getHeight() << " x " << doubleRect.getWidth() << std::endl;
    std::cout << "  Area: " << doubleRect.area() << std::endl;
    std::cout << "  Perimeter: " << doubleRect.perimeter() << std::endl;
    
    // Float rectangle
    Rectangle<float> floatRect(3.2f, 7.8f);
    std::cout << "\nFloat Rectangle:" << std::endl;
    std::cout << "  Dimensions: " << floatRect.getHeight() << " x " << floatRect.getWidth() << std::endl;
    std::cout << "  Area: " << floatRect.area() << std::endl;
    std::cout << "  Perimeter: " << floatRect.perimeter() << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Integer Rectangle:
  Dimensions: 5 x 10
  Area: 50
  Perimeter: 30

Double Rectangle:
  Dimensions: 5.5 x 10.25
  Area: 56.375
  Perimeter: 31.5

Float Rectangle:
  Dimensions: 3.2 x 7.8
  Area: 24.96
  Perimeter: 22
```

### Templates with Multiple Type Parameters

Just like template functions, template classes can have multiple type parameters:

```cpp
#include <iostream>
#include <string>

template <class K, class V>
class KeyValuePair {
public:
    KeyValuePair(K k, V v) : key(k), value(v) {}
    
    K getKey() const { return key; }
    V getValue() const { return value; }
    
    void setValue(V v) { value = v; }
    
    void display() const {
        std::cout << "Key: " << key << ", Value: " << value << std::endl;
    }
    
private:
    K key;
    V value;
};

int main() {
    // String key, int value
    KeyValuePair<std::string, int> age("Alice", 25);
    age.display();
    
    // Int key, double value
    KeyValuePair<int, double> measurement(42, 3.14159);
    measurement.display();
    
    // String key, string value
    KeyValuePair<std::string, std::string> config("hostname", "localhost");
    config.display();
    
    return 0;
}
```

**Sample Output:**
```
Key: Alice, Value: 25
Key: 42, Value: 3.14159
Key: hostname, Value: localhost
```

---

## Section 4: Template Specialization

### What is Template Specialization?

Sometimes we need a template to behave differently for specific types. Template specialization allows us to provide custom implementations for particular types while keeping the generic implementation for all others.

### Full Template Specialization

```cpp
#include <iostream>
#include <string>
#include <cmath>

// Generic template
template <class T>
class Calculator {
public:
    T add(T a, T b) {
        return a + b;
    }
    
    T multiply(T a, T b) {
        return a * b;
    }
};

// Specialized template for strings
template <>
class Calculator<std::string> {
public:
    std::string add(std::string a, std::string b) {
        return a + " and " + b;  // String concatenation with " and "
    }
    
    std::string multiply(std::string a, std::string b) {
        return "Cannot multiply strings: " + a + " * " + b;
    }
};

int main() {
    // Generic version for integers
    Calculator<int> intCalc;
    std::cout << "Integer: 5 + 3 = " << intCalc.add(5, 3) << std::endl;
    std::cout << "Integer: 5 * 3 = " << intCalc.multiply(5, 3) << std::endl;
    
    // Generic version for doubles
    Calculator<double> doubleCalc;
    std::cout << "\nDouble: 2.5 + 3.7 = " << doubleCalc.add(2.5, 3.7) << std::endl;
    std::cout << "Double: 2.5 * 3.7 = " << doubleCalc.multiply(2.5, 3.7) << std::endl;
    
    // Specialized version for strings
    Calculator<std::string> stringCalc;
    std::cout << "\nString: 'Hello' + 'World' = " << stringCalc.add("Hello", "World") << std::endl;
    std::cout << "String: 'Hello' * 'World' = " << stringCalc.multiply("Hello", "World") << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Integer: 5 + 3 = 8
Integer: 5 * 3 = 15

Double: 2.5 + 3.7 = 6.2
Double: 2.5 * 3.7 = 9.25

String: 'Hello' + 'World' = Hello and World
String: 'Hello' * 'World' = Cannot multiply strings: Hello * World
```

### Partial Template Specialization

For templates with multiple parameters, we can specialize some but not all:

```cpp
#include <iostream>

// General template
template <class T, class U>
class Pair {
public:
    Pair(T first, U second) : a(first), b(second) {}
    
    void display() {
        std::cout << "Generic pair: (" << a << ", " << b << ")" << std::endl;
    }
    
private:
    T a;
    U b;
};

// Partial specialization: when both types are the same
template <class T>
class Pair<T, T> {
public:
    Pair(T first, T second) : a(first), b(second) {}
    
    void display() {
        std::cout << "Same-type pair: (" << a << ", " << b << ")" << std::endl;
    }
    
    bool areEqual() {
        return a == b;
    }
    
private:
    T a;
    T b;
};

int main() {
    Pair<int, double> p1(42, 3.14);
    p1.display();
    
    Pair<int, int> p2(10, 20);
    p2.display();
    std::cout << "Are they equal? " << (p2.areEqual() ? "Yes" : "No") << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Generic pair: (42, 3.14)
Same-type pair: (10, 20)
Are they equal? No
```

---

## Section 5: Template Compilation and Best Practices

### Template Compilation Model

Templates are not compiled like regular code. Instead, they follow a two-phase compilation model:

```
┌──────────────────────────────────────────────────────┐
│  Phase 1: Template Definition                        │
│  - Syntax checking only                              │
│  - No actual code generated yet                      │
│  - Template is just a "recipe"                       │
└──────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────┐
│  Phase 2: Template Instantiation                     │
│  - Occurs when template is used with specific type   │
│  - Compiler generates actual code                    │
│  - Type checking happens here                        │
│  - Compilation errors may occur here                 │
└──────────────────────────────────────────────────────┘
```

### Header-Only Implementation

Because of how templates are compiled, template definitions typically must be in header files, not separate .cpp files. This is different from regular classes:

```cpp
// Rectangle.h - Template class (complete implementation in header)
#ifndef RECTANGLE_H
#define RECTANGLE_H

#include <iostream>

template <class T>
class Rectangle {
public:
    Rectangle(T h, T w) : height(h), width(w) {}
    
    T area() const {
        return height * width;
    }
    
    void display() const {
        std::cout << "Rectangle: " << height << " x " << width 
                  << ", Area: " << area() << std::endl;
    }
    
private:
    T height;
    T width;
};

#endif
```

```cpp
// main.cpp - Using the template class
#include "Rectangle.h"

int main() {
    Rectangle<int> r1(5, 10);
    r1.display();
    
    Rectangle<double> r2(5.5, 10.5);
    r2.display();
    
    return 0;
}
```

### Common Template Errors

Understanding common template errors helps with debugging:

```cpp
#include <iostream>
#include <string>

template <class T>
T divide(T a, T b) {
    return a / b;
}

int main() {
    // This works fine
    std::cout << divide(10.0, 2.0) << std::endl;
    
    // This will cause a compilation error - strings don't support division
    // Uncomment to see the error:
    // std::string s1 = "Hello";
    // std::string s2 = "World";
    // std::cout << divide(s1, s2) << std::endl;
    
    // Error message will indicate that operator/ is not defined for std::string
    
    return 0;
}
```

**Sample Output:**
```
5
```

### Best Practices for Using Templates

1. **Name template parameters clearly**: Use meaningful names like `ValueType` or descriptive single letters
2. **Document type requirements**: Specify what operations a type must support
3. **Use const where appropriate**: Make template functions const-correct
4. **Consider performance**: Templates generate code at compile time, which can increase binary size
5. **Test with multiple types**: Ensure your template works with various types before considering it complete

```cpp
#include <iostream>

// Well-documented template function
// Requirements: T must support operator* and conversion to double
template <class T>
double circleArea(const T& radius) {
    const double PI = 3.14159265358979323846;
    return static_cast<double>(radius * radius * PI);
}

int main() {
    int r1 = 5;
    float r2 = 3.5f;
    double r3 = 7.25;
    
    std::cout << "Circle with radius " << r1 << " has area " << circleArea(r1) << std::endl;
    std::cout << "Circle with radius " << r2 << " has area " << circleArea(r2) << std::endl;
    std::cout << "Circle with radius " << r3 << " has area " << circleArea(r3) << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Circle with radius 5 has area 78.5398
Circle with radius 3.5 has area 38.4845
Circle with radius 7.25 has area 165.13
```

---

## Section 6: Advanced Topics - Template Metaprogramming

### Compile-Time Computation

One of the most powerful (and mind-bending) features of C++ templates is that they can perform computations at compile time. This means calculations are done during compilation, not during program execution.

### Factorial Example Using Templates

```cpp
#include <iostream>

// Primary template - recursive case
template <int N>
class FACTORIAL {
public:
    static const int value = N * FACTORIAL<N-1>::value;
};

// Template specialization - base case
template <>
class FACTORIAL<0> {
public:
    static const int value = 1;
};

int main() {
    // These values are computed at compile time!
    std::cout << "Factorial of 5: " << FACTORIAL<5>::value << std::endl;
    std::cout << "Factorial of 10: " << FACTORIAL<10>::value << std::endl;
    std::cout << "Factorial of 0: " << FACTORIAL<0>::value << std::endl;
    std::cout << "Factorial of 7: " << FACTORIAL<7>::value << std::endl;
    
    // The compiler has already computed these values
    // They're just constants in the final program
    
    return 0;
}
```

**Sample Output:**
```
Factorial of 5: 120
Factorial of 10: 3628800
Factorial of 0: 1
Factorial of 7: 5040
```

### How This Works

Let's trace through `FACTORIAL<3>`:

```
FACTORIAL<3>::value
= 3 * FACTORIAL<2>::value
= 3 * (2 * FACTORIAL<1>::value)
= 3 * (2 * (1 * FACTORIAL<0>::value))
= 3 * (2 * (1 * 1))
= 3 * 2 * 1
= 6
```

The compiler performs this entire calculation at compile time. The final program contains just the result (6), not the calculation code.

### Fibonacci Using Template Metaprogramming

```cpp
#include <iostream>

// Primary template - recursive case
template <int N>
class FIBONACCI {
public:
    static const int value = FIBONACCI<N-1>::value + FIBONACCI<N-2>::value;
};

// Base cases
template <>
class FIBONACCI<0> {
public:
    static const int value = 0;
};

template <>
class FIBONACCI<1> {
public:
    static const int value = 1;
};

int main() {
    std::cout << "Fibonacci sequence (compile-time computed):" << std::endl;
    std::cout << "F(0) = " << FIBONACCI<0>::value << std::endl;
    std::cout << "F(1) = " << FIBONACCI<1>::value << std::endl;
    std::cout << "F(2) = " << FIBONACCI<2>::value << std::endl;
    std::cout << "F(5) = " << FIBONACCI<5>::value << std::endl;
    std::cout << "F(10) = " << FIBONACCI<10>::value << std::endl;
    std::cout << "F(15) = " << FIBONACCI<15>::value << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Fibonacci sequence (compile-time computed):
F(0) = 0
F(1) = 1
F(2) = 1
F(5) = 5
F(10) = 55
F(15) = 610
```

### Why Use Template Metaprogramming?

Advantages:
- Zero runtime cost - computations done at compile time
- Type safety enforced at compile time
- Can catch errors before the program runs
- Enables powerful compile-time optimizations

Disadvantages:
- Increased compilation time
- Complex and harder to read
- Error messages can be cryptic
- Not suitable for all problems

### Practical Example: Compile-Time Power Calculation

```cpp
#include <iostream>

// Compute base^exponent at compile time
template <int base, int exponent>
class POWER {
public:
    static const int value = base * POWER<base, exponent-1>::value;
};

// Base case: anything to the power 0 is 1
template <int base>
class POWER<base, 0> {
public:
    static const int value = 1;
};

int main() {
    std::cout << "Compile-time power calculations:" << std::endl;
    std::cout << "2^8 = " << POWER<2, 8>::value << std::endl;
    std::cout << "3^4 = " << POWER<3, 4>::value << std::endl;
    std::cout << "5^3 = " << POWER<5, 3>::value << std::endl;
    std::cout << "10^2 = " << POWER<10, 2>::value << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Compile-time power calculations:
2^8 = 256
3^4 = 81
5^3 = 125
10^2 = 100
```

### Modern C++ Alternatives

Modern C++ (C++11 and later) provides `constexpr` as a more readable alternative to template metaprogramming for many use cases:

```cpp
#include <iostream>

// Modern approach using constexpr
constexpr int factorial(int n) {
    return (n <= 1) ? 1 : n * factorial(n - 1);
}

int main() {
    // Computed at compile time
    constexpr int f5 = factorial(5);
    constexpr int f10 = factorial(10);
    
    std::cout << "Factorial of 5: " << f5 << std::endl;
    std::cout << "Factorial of 10: " << f10 << std::endl;
    
    // Can also be used at runtime
    int n;
    std::cout << "Enter a number: ";
    std::cin >> n;
    std::cout << "Factorial of " << n << ": " << factorial(n) << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Factorial of 5: 120
Factorial of 10: 3628800
Enter a number: 6
Factorial of 6: 720
```

---

## Resources

### Official Documentation

- **C++ Reference - Templates**: https://en.cppreference.com/w/cpp/language/templates
  Comprehensive reference for template syntax and semantics
  
- **C++ Standard Library Documentation**: https://en.cppreference.com/w/cpp/header
  Examples of standard library templates (vector, array, etc.)

### Textbooks and Reading

- **"C++ Primer" by Stanley Lippman, Josée Lajoie, and Barbara Moo**
  Chapter on templates provides excellent in-depth coverage
  
- **"Effective C++" by Scott Meyers**
  Items on templates discuss best practices and common pitfalls
  
- **"C++ Templates: The Complete Guide" by David Vandevoorde and Nicolai M. Josuttis**
  Comprehensive deep-dive into template programming

### Online Tutorials

- **LearnCpp.com - Templates**: https://www.learncpp.com/cpp-tutorial/template-classes/
  Beginner-friendly tutorial series on templates
  
- **GeeksforGeeks C++ Templates**: https://www.geeksforgeeks.org/templates-cpp/
  Multiple articles covering different aspects of templates
  
- **Microsoft C++ Documentation - Templates**: https://docs.microsoft.com/en-us/cpp/cpp/templates-cpp
  Clear explanations with Visual Studio integration

### Practice and Challenges

- **Exercism C++ Track**: https://exercism.org/tracks/cpp
  Practice problems that often involve template usage
  
- **HackerRank C++ Domain**: https://www.hackerrank.com/domains/cpp
  Includes template-specific challenges
  
- **LeetCode**: https://leetcode.com/
  Algorithm problems that benefit from generic programming

### Interactive Learning

- **Compiler Explorer (Godbolt)**: https://godbolt.org/
  See how templates are compiled and instantiated
  
- **C++ Insights**: https://cppinsights.io/
  Visualize how the compiler transforms template code

### Community Resources

- **Stack Overflow - C++ Templates**: https://stackoverflow.com/questions/tagged/c%2b%2b-templates
  Q&A for specific template-related problems
  
- **Reddit r/cpp**: https://www.reddit.com/r/cpp/
  Discussions about modern C++ template techniques
  
- **C++ Core Guidelines - Templates**: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#t-templates-and-generic-programming
  Best practices from C++ experts

### Advanced Topics

- **Template Metaprogramming**: https://en.wikibooks.org/wiki/C%2B%2B_Programming/Templates/Template_Meta-Programming
  Introduction to compile-time computation
  
- **SFINAE and Type Traits**: https://en.cppreference.com/w/cpp/language/sfinae
  Advanced template techniques for type selection
  
- **Concepts (C++20)**: https://en.cppreference.com/w/cpp/language/constraints
  Modern approach to constraining templates

---