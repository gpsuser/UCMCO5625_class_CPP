# Week 7: C++ Templates Worksheet

## Section 1: Quiz Questions

### Question 1

Fill in the blanks using the words provided below:

Templates in C++ allow us to write __________ code that works with multiple data types. The keyword __________ is used to define a template, followed by angle brackets containing the template __________. When we create an instance of a template class with a specific type, this is called template __________. Templates maintain __________ while providing code reuse, unlike void pointers which lose type information.

**Word Bank:** `instantiation`, `generic`, `template`, `safety`, `parameters`

---

### Question 2

Fill in the blanks using the words provided below:

Template functions can use __________ template instantiation where the compiler automatically determines the type, or __________ template instantiation where we specify the type using angle brackets. Template code follows a two-phase __________ model: first the template definition is checked for syntax, then actual code is generated during __________. Because of this compilation model, template definitions must typically be placed in __________ files rather than separate .cpp files.

**Word Bank:** `explicit`, `compilation`, `header`, `implicit`, `instantiation`

---

### Question 3

Fill in the blanks using the words provided below:

Template __________ allows us to provide custom implementations for specific types. A __________ template specialization provides a completely different implementation for one particular type, while __________ template specialization can be used when a template has multiple type parameters. Templates only work if the __________ used in the template are valid for the type being used. For example, a template using multiplication won't work with __________ types that don't support the multiplication operator.

**Word Bank:** `operations`, `full`, `partial`, `specialization`, `string`

---

### Question 4

Fill in the blanks using the words provided below:

Template __________ is a technique that performs computations at compile time rather than runtime. This is achieved through __________ template definitions combined with template __________. The advantages include zero __________ cost and type safety enforced at compile time. Modern C++ provides __________ as a more readable alternative for many compile-time computation scenarios.

**Word Bank:** `constexpr`, `metaprogramming`, `runtime`, `specialization`, `recursive`

---

### Question 5

Which of the following statements about C++ templates is **correct**?

A) Templates are compiled like regular functions, generating code immediately when the template is defined

B) Template functions must always explicitly specify the type in angle brackets when called

C) Templates generate code at compile time when instantiated with a specific type, which can increase binary size

D) Template classes can only have a single type parameter

---

### Question 6

Consider the following template function:

```cpp
template <class T>
T calculate(T a, T b) {
    return a * a + b * b;
}
```

Which statement is **true** about this function?

A) It will work with any type T, including strings and custom classes

B) It will only work with types that support the multiplication and addition operators

C) It must be explicitly instantiated in a separate .cpp file

D) The template parameter must use `typename` instead of `class`

---

### Question 7

What is the primary difference between template specialization and function overloading?

A) Template specialization occurs at compile time and provides different implementations for specific types of a generic template; function overloading creates entirely separate functions with different signatures

B) Template specialization is only for classes while function overloading is only for functions

C) Template specialization requires the `virtual` keyword while function overloading does not

D) There is no practical difference between the two techniques

---

## Section 2: Coding Tasks

### Task 1: Template Function for Minimum of Three Values

Write a template function called `minimum3` that takes three parameters of the same type and returns the smallest value among them.

**Requirements:**
- The function should work with any type that supports the less-than (`<`) operator
- Test your function with integers, doubles, and characters

**Code Scaffolding:**

```cpp
#include <iostream>
#include <string>
using namespace std;

// TODO: Write your template function here
template <class T>
T minimum3(T a, T b, T c) {
    // Your code here
}

int main() {
    // Test with integers
    cout << "Minimum of 5, 2, 8: " << minimum3(5, 2, 8) << endl;
    
    // Test with doubles
    cout << "Minimum of 3.7, 2.1, 5.9: " << minimum3(3.7, 2.1, 5.9) << endl;
    
    // Test with characters
    cout << "Minimum of 'z', 'a', 'm': " << minimum3('z', 'a', 'm') << endl;
    
    return 0;
}
```

**Expected Output:**
```
Minimum of 5, 2, 8: 2
Minimum of 3.7, 2.1, 5.9: 2.1
Minimum of 'z', 'a', 'm': a
```

---

### Task 2: Template Class for a Simple Pair Container

Create a template class called `Pair` that stores two values of potentially different types. The class should provide methods to get and set both values, and a method to swap them.

**Requirements:**
- The class should have two template parameters (one for each value)
- Implement `getFirst()`, `getSecond()`, `setFirst()`, and `setSecond()` methods
- Implement a `swap()` method that exchanges the two values (this only works when both types are the same)
- Implement a `display()` method that prints both values

**Code Scaffolding:**

```cpp
#include <iostream>
#include <string>
using namespace std;

// TODO: Write your template class here
template <class T1, class T2>
class Pair {
private:
    T1 first;
    T2 second;
    
public:
    // Constructor
    Pair(T1 f, T2 s) : first(f), second(s) {}
    
    // TODO: Implement getter methods
    
    // TODO: Implement setter methods
    
    // TODO: Implement display method
    
};

// TODO: Add a specialized swap method for when T1 and T2 are the same
// (This is optional but demonstrates template specialization)

int main() {
    // Test with different types
    Pair<string, int> person("Alice", 25);
    person.display();
    
    // Test with same types
    Pair<int, int> coordinates(10, 20);
    coordinates.display();
    cout << "After swap: ";
    // coordinates.swap();  // If you implement swap
    coordinates.display();
    
    // Test setters
    person.setFirst("Bob");
    person.setSecond(30);
    person.display();
    
    return 0;
}
```

**Expected Output:**
```
(Alice, 25)
(10, 20)
After swap: (20, 10)
(Bob, 30)
```

---

## Summary Table

| Lecture Topic | Quiz Questions | Coding Tasks |
|--------------|----------------|--------------|
| Generic Programming & Template Motivation | Q1 | - |
| Template Function Syntax | Q2, Q6 | Task 1 |
| Template Class Syntax | Q3 | Task 2 |
| Template Specialization | Q3, Q7 | Task 2 (optional) |
| Template Compilation | Q2, Q5 | - |
| Template Metaprogramming | Q4 | - |
