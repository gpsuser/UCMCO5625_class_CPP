# Exception Handling in C++

---

## Learning Outcomes

By the end of this lecture, students should be able to:

- Explain what exceptions are and why exception handling is necessary in robust software
- Use `try` and `catch` blocks to handle exceptions gracefully
- Handle multiple exception types within a single `try` block
- Throw exceptions manually using the `throw` keyword
- Pass descriptive messages to `std::exception` when throwing
- Create custom exception classes that inherit from `std::exception`
- Override the `what()` method to provide meaningful error messages
- Explain the purpose and significance of the `noexcept` keyword
- Apply exception handling techniques to real-world scenarios such as container underflow

---

## Table of Contents

- [Section 1: What Are Exceptions?](#section-1-what-are-exceptions)
- [Section 2: The try...catch Block](#section-2-the-trycatch-block)
- [Section 3: Handling Multiple Exception Types](#section-3-handling-multiple-exception-types)
- [Section 4: Throwing Exceptions](#section-4-throwing-exceptions)
- [Section 5: Catching Thrown Exceptions](#section-5-catching-thrown-exceptions)
- [Section 6: Adding Detail to Exceptions](#section-6-adding-detail-to-exceptions)
- [Section 7: Creating Custom Exception Classes](#section-7-creating-custom-exception-classes)
- [Section 8: The noexcept Keyword](#section-8-the-noexcept-keyword)
- [Section 9: Exception Handling in Practice](#section-9-exception-handling-in-practice)
- [Resources](#resources)

---

## Section 1: What Are Exceptions?

> An introduction to what exceptions are, why they occur, and the philosophy behind handling them.

### What Is an Exception?

An exception is an event that disrupts the normal flow of a program at runtime. Unlike logic errors or syntax errors (which you catch during development), exceptions often arise from circumstances that are difficult or impossible to predict when writing code.

Consider the difference between:

- A **compile-time error**: caught by the compiler before the program runs.
- A **runtime error**: occurs while the program is executing, often due to unpredictable conditions.

Exceptions fall into the second category. They represent situations where the program cannot safely continue using its normal control flow.

### Common Causes of Exceptions

| Scenario                          | Example                                              |
|-----------------------------------|------------------------------------------------------|
| Memory exhaustion                 | Allocating a large array when no memory is available |
| File system issues                | Opening a file that does not exist                   |
| Network failures                  | Attempting a connection to an unreachable server     |
| Invalid operations on data        | Popping from an empty stack                          |
| Arithmetic issues                 | Division by zero (in some contexts)                  |

### Why Handle Exceptions?

Without exception handling, a program that encounters an unexpected condition will either:

1. Produce incorrect results silently (dangerous).
2. Crash abruptly without any helpful information (unhelpful).

Exception handling gives us a structured way to detect problems, report meaningful errors, and recover gracefully or exit cleanly.

```
Normal execution flow:

  [Start] --> [Operation A] --> [Operation B] --> [Operation C] --> [End]

With an exception at Operation B:

  [Start] --> [Operation A] --> [Operation B throws!]
                                        |
                                        v
                                 [Exception Handler]
                                        |
                                        v
                                 [Graceful Exit / Recovery]
```

[Back to Table of Contents](#table-of-contents)

---

## Section 2: The try...catch Block

> How to structure exception handling code using the `try` and `catch` keywords.

### Basic Syntax

The fundamental mechanism for exception handling in C++ is the `try...catch` block:

```cpp
try
{
    // Code that might throw an exception
}
catch(std::exception& e)
{
    std::cerr << e.what() << std::endl;
}
```

- The `try` block encloses code that may throw an exception.
- The `catch` block defines what happens when an exception of the specified type is thrown.
- `std::exception` is the base class for all standard library exceptions.
- `e.what()` returns a C-style string describing the exception.
- We use `std::cerr` rather than `std::cout` for error output - it is unbuffered and writes to the standard error stream.

### The Exception Hierarchy

The C++ standard library defines a hierarchy of exception classes, all deriving from `std::exception`:

```
std::exception
    |
    +-- std::logic_error
    |       +-- std::invalid_argument
    |       +-- std::out_of_range
    |       +-- std::domain_error
    |
    +-- std::runtime_error
    |       +-- std::overflow_error
    |       +-- std::underflow_error
    |       +-- std::range_error
    |
    +-- std::bad_alloc
    +-- std::bad_cast
```

Catching `std::exception&` (by reference) will catch any exception in this hierarchy.

### Complete Example: Basic Exception Handling

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>

int main()
{
    try
    {
        std::vector<int> v = {1, 2, 3};
        // .at() performs bounds checking and throws std::out_of_range
        std::cout << v.at(10) << std::endl;
    }
    catch(std::exception& e)
    {
        std::cerr << "Exception caught: " << e.what() << std::endl;
    }

    std::cout << "Program continues after exception." << std::endl;
    return 0;
}
```

**Sample Output:**
```
Exception caught: vector::_M_range_check: __n (which is 10) >= this->size() (which is 3)
Program continues after exception.
```

Notice that the program does not crash - it handles the exception and continues running normally.

[Back to Table of Contents](#table-of-contents)

---

## Section 3: Handling Multiple Exception Types

> How to catch different types of exceptions from the same `try` block using multiple `catch` clauses.

### Motivation

A single block of code might involve several operations, each capable of throwing a different type of exception. For example, deserialising data from a file and then sending it over a network could fail at either stage - but for entirely different reasons.

### Multiple catch Clauses

You can chain multiple `catch` blocks after a single `try` block. C++ will evaluate them in order and execute the first one that matches the thrown exception type.

```
try
{
    [code that may throw]
}
catch(SpecificExceptionType& ex)   <-- checked first
{
    [handle specific case]
}
catch(std::exception& ex)          <-- checked second (more general)
{
    [handle general case]
}
```

**Important:** Always order `catch` clauses from most specific to most general. If `std::exception` is listed first, it will catch everything and the more specific handlers below it will never execute.

### Complete Example: Multiple Exception Types

```cpp
#include <iostream>
#include <stdexcept>
#include <string>

void processInput(const std::string& input)
{
    if (input.empty())
        throw std::invalid_argument("Input string cannot be empty.");

    if (input.size() > 5)
        throw std::overflow_error("Input string is too long.");

    std::cout << "Processing: " << input << std::endl;
}

int main()
{
    std::string testCases[] = {"Hello", "", "TooLongString", "OK"};

    for (const auto& test : testCases)
    {
        try
        {
            processInput(test);
        }
        catch(std::invalid_argument& e)
        {
            std::cerr << "[Invalid Argument] " << e.what() << std::endl;
        }
        catch(std::overflow_error& e)
        {
            std::cerr << "[Overflow Error] " << e.what() << std::endl;
        }
        catch(std::exception& e)
        {
            std::cerr << "[General Exception] " << e.what() << std::endl;
        }
    }

    return 0;
}
```

**Sample Output:**
```
Processing: Hello
[Invalid Argument] Input string cannot be empty.
[Overflow Error] Input string is too long.
Processing: OK
```

### The Catch-All Handler

There is also a special catch clause that catches any exception regardless of type:

```cpp
catch(...)
{
    std::cerr << "An unknown exception occurred." << std::endl;
}
```

This should always appear last if used, as a final safety net. It is useful when integrating with third-party libraries that might throw non-standard exception types.

[Back to Table of Contents](#table-of-contents)

---

## Section 4: Throwing Exceptions

> How to throw exceptions manually from within your own code using the `throw` keyword.

### Why Throw Your Own Exceptions?

So far, exceptions have been thrown automatically by the standard library (e.g., `std::vector::at()`). However, there are many situations where you need to detect an invalid condition in your own code and signal it to the caller.

Consider a class representing a packet of mints:

```cpp
#include <iostream>

class PacketOfMints
{
    unsigned int nMints;
public:
    PacketOfMints(unsigned int size) : nMints(size) {}
    unsigned int count() { return nMints; }
    void eat() { nMints--; }
};

int main()
{
    PacketOfMints p(10);
    for(int i = 0; i < 11; ++i)
        p.eat();

    std::cout << p.count() << std::endl;
    std::cin.ignore();
    return 0;
}
```

**Sample Output:**
```
4294967295
```

### The Problem: Integer Overflow

The `nMints` variable is declared as `unsigned int`. When you subtract 1 from 0 using unsigned arithmetic, the result wraps around to the largest possible value for that type (4,294,967,295 on most systems). The program produces a completely wrong result without any warning.

```
unsigned int underflow:

  0  -  1  =  4294967295

  [0x00000000] - 1 = [0xFFFFFFFF]

This is called integer wraparound or unsigned underflow.
```

This scenario is directly analogous to calling `pop()` on an empty stack or `dequeue()` on an empty queue - a common real-world exception scenario in data structure implementations.

### Using throw to Signal an Error

We can fix this by checking for the invalid condition and throwing an exception:

```cpp
#include <iostream>
#include <exception>

class PacketOfMints
{
    unsigned int nMints;
public:
    PacketOfMints(unsigned int size) : nMints(size) {}
    unsigned int count() { return nMints; }
    void eat();
};

void PacketOfMints::eat()
{
    if (nMints == 0)
        throw std::exception();  // throw an exception object
    else
        nMints--;
}

int main()
{
    PacketOfMints p(10);
    for(int i = 0; i < 11; ++i)
        p.eat();

    std::cout << p.count() << std::endl;
    std::cin.ignore();
    return 0;
}
```

**Sample Output:**
```
terminate called after throwing an instance of 'std::exception'
  what():  std::exception
Aborted
```

The program exits abruptly because we threw an exception but did not catch it. This is still better than silently producing a wrong answer - but we need to handle it properly.

[Back to Table of Contents](#table-of-contents)

---

## Section 5: Catching Thrown Exceptions

> Wrapping code that throws in a `try` block to handle the exception properly and allow the program to exit cleanly.

### Handling the Thrown Exception

When a `throw` statement is executed, the runtime unwinds the call stack looking for a matching `catch` block. If none is found, `std::terminate()` is called and the program ends abruptly.

To handle the exception from our mints example, we wrap the loop in a `try` block:

```cpp
#include <iostream>
#include <exception>

class PacketOfMints
{
    unsigned int nMints;
public:
    PacketOfMints(unsigned int size) : nMints(size) {}
    unsigned int count() { return nMints; }
    void eat();
};

void PacketOfMints::eat()
{
    if (nMints == 0)
        throw std::exception();
    else
        nMints--;
}

int main()
{
    PacketOfMints p(10);

    try
    {
        for (int i = 0; i < 11; ++i)
            p.eat();
    }
    catch (std::exception& ex)
    {
        std::cerr << ex.what() << std::endl;
    }

    std::cout << "Mints remaining: " << p.count() << std::endl;
    std::cin.ignore();
    return 0;
}
```

**Sample Output:**
```
std::exception
Mints remaining: 0
```

### What Happened?

```
Execution trace:

  i=0 : eat() -> nMints = 9
  i=1 : eat() -> nMints = 8
  ...
  i=9 : eat() -> nMints = 0
  i=10: eat() -> nMints == 0, throw std::exception()
                              |
                              v
                   catch(std::exception& ex)
                   prints: "std::exception"
                              |
                              v
                   Execution resumes AFTER the try/catch block
                   p.count() == 0 (correct!)
```

The program now exits cleanly with the correct mint count (0). The exception prevented the destructive wrap-around and gave us a chance to respond.

### Stack Unwinding

When an exception is thrown, C++ performs **stack unwinding** - destructors for all local objects in the current scope (and any enclosing scopes up to the matching `catch`) are called automatically. This means RAII (Resource Acquisition Is Initialisation) objects such as `std::unique_ptr` and `std::fstream` will still be properly cleaned up even when exceptions occur.

[Back to Table of Contents](#table-of-contents)

---

## Section 6: Adding Detail to Exceptions

> How to make thrown exceptions more informative by passing messages or creating descriptive objects.

### The Problem With Bare std::exception

The output from our earlier example was:
```
std::exception
```
or on some platforms:
```
Unknown exception
```

This is not very helpful. When debugging a real application you want to know:
- What went wrong.
- Where it went wrong.
- Any relevant data that can help diagnose the issue.

### Method 1: Passing a String to the Exception Constructor

Some exception types accept a string in their constructor, which `what()` will then return:

```cpp
#include <iostream>
#include <exception>
#include <stdexcept>

class PacketOfMints
{
    unsigned int nMints;
public:
    PacketOfMints(unsigned int size) : nMints(size) {}
    unsigned int count() { return nMints; }
    void eat();
};

void PacketOfMints::eat()
{
    if (nMints == 0)
        throw std::runtime_error("Cannot eat: packet is already empty.");
    else
        nMints--;
}

int main()
{
    PacketOfMints p(3);

    try
    {
        for (int i = 0; i < 5; ++i)
            p.eat();
    }
    catch (std::exception& ex)
    {
        std::cerr << "Exception: " << ex.what() << std::endl;
    }

    std::cout << "Mints remaining: " << p.count() << std::endl;
    return 0;
}
```

**Sample Output:**
```
Exception: Cannot eat: packet is already empty.
Mints remaining: 0
```

Note: `std::runtime_error` (from `<stdexcept>`) accepts a string argument in its constructor and is the appropriate base for exceptions representing conditions only detectable at runtime.

### Method 2: Creating a Custom Exception Class

For more complex scenarios, you can create your own exception class that carries additional data. This is covered in detail in the next section.

[Back to Table of Contents](#table-of-contents)

---

## Section 7: Creating Custom Exception Classes

> How to design and implement your own exception types by inheriting from `std::exception`.

### Why Custom Exceptions?

Custom exceptions allow you to:

- Provide structured, domain-specific error information.
- Include additional data relevant to the error (not just a message string).
- Allow callers to catch your specific exception type separately from general exceptions.
- Create a clear and readable exception hierarchy for your application or library.

### Rules for Custom Exceptions

1. Always inherit from `std::exception` (directly or indirectly) so they can be caught by `catch(std::exception&)`.
2. Override the `what()` method to return a meaningful C-style string.
3. Mark `what()` as `virtual`, `const`, and `noexcept` to match the base class signature.

### The negativeMintException Class

```cpp
#include <iostream>
#include <exception>

class negativeMintException : public std::exception
{
    int packetSize;
public:
    negativeMintException(int size) : packetSize(size) {}

    virtual const char* what() const noexcept
    {
        return "Negative mint exception: attempted to eat from an empty packet.";
    }

    int size() { return packetSize; }
};

class PacketOfMints
{
    unsigned int nMints;
public:
    PacketOfMints(unsigned int size) : nMints(size) {}
    unsigned int count() { return nMints; }
    void eat();
};

void PacketOfMints::eat()
{
    if (nMints == 0)
        throw negativeMintException(static_cast<int>(nMints));
    else
        nMints--;
}

int main()
{
    PacketOfMints p(2);

    try
    {
        for (int i = 0; i < 5; ++i)
            p.eat();
    }
    catch (negativeMintException& ex)
    {
        std::cerr << ex.what() << std::endl;
        std::cerr << "Packet size at time of exception: " << ex.size() << std::endl;
    }
    catch (std::exception& ex)
    {
        std::cerr << "General exception: " << ex.what() << std::endl;
    }

    std::cout << "Mints remaining: " << p.count() << std::endl;
    return 0;
}
```

**Sample Output:**
```
Negative mint exception: attempted to eat from an empty packet.
Packet size at time of exception: 0
Mints remaining: 0
```

### Anatomy of the Custom Exception

```
class negativeMintException : public std::exception
          |                          |
          |                          +-- Inherits from std::exception
          |                              so it can be caught generically
          |
          +-- Our custom exception name

    int packetSize;                  <-- Extra data the caller can query

    negativeMintException(int size)  <-- Constructor stores the data
        : packetSize(size) {}

    virtual const char* what()       <-- virtual: subclasses can override it
        const                        <-- const: does not modify the object
        noexcept                     <-- noexcept: cannot itself throw
    {
        return "...";                <-- Returns a string literal
    }

    int size()                       <-- Extra method: exposes stored data
    { return packetSize; }
```

### Catching Custom vs General Exceptions

Because `negativeMintException` inherits from `std::exception`, it can be caught by either:

```cpp
catch (negativeMintException& ex)  // Specific - access extra data
catch (std::exception& ex)         // General - just the message
```

If both are present, list the specific one first. The general handler acts as a fallback.

[Back to Table of Contents](#table-of-contents)

---

## Section 8: The noexcept Keyword

> Understanding `noexcept` - what it means, why it matters, and its historical context.

### What Does noexcept Mean?

When you mark a function with `noexcept`, you are telling the compiler (and other developers) that this function will never throw an exception:

```cpp
virtual const char* what() const noexcept
{
    return "negative mint exception";
}
```

If a `noexcept` function does somehow throw an exception, `std::terminate()` is called immediately - the exception is not allowed to propagate.

### Why Use noexcept on what()?

The `what()` method is called inside a `catch` block - that is, it is called while the program is already in the middle of handling an exception. If `what()` were to throw another exception at this point, the behaviour would be undefined or the program would terminate. Using `noexcept` prevents this and provides a strong safety guarantee.

```
Exception handling flow:

  throw negativeMintException()
          |
          v
  catch (negativeMintException& ex)
          |
          v
  ex.what()   <-- This must NOT throw. noexcept guarantees it.
          |
          v
  Display message, continue execution
```

### noexcept as a Specifier and an Operator

`noexcept` has two roles in C++:

1. **Specifier** (what we have been using): tells the compiler a function does not throw.
   ```cpp
   void myFunction() noexcept { /* ... */ }
   ```

2. **Operator**: evaluates at compile time whether an expression is declared noexcept.
   ```cpp
   bool safe = noexcept(myFunction()); // true
   ```

### Historical Context: throw()

Before `noexcept` was introduced in C++11, the same intent was expressed with the older `throw()` syntax (an empty exception specification):

```cpp
virtual const char* what() const throw()
{
    return "my exception";
}
```

This syntax appeared in older codebases and libraries - for example, the Boost archive exception header used it historically. The `throw()` specifier was deprecated in C++11 and formally removed in C++17. Any modern code should use `noexcept` instead.

### Complete Demonstration: noexcept in Action

```cpp
#include <iostream>
#include <exception>

class SafeException : public std::exception
{
public:
    virtual const char* what() const noexcept override
    {
        return "This is a safe, noexcept exception message.";
    }
};

void riskyOperation(bool fail)
{
    if (fail)
        throw SafeException();
    std::cout << "Operation succeeded." << std::endl;
}

int main()
{
    // Check at compile time whether what() is noexcept
    SafeException ex;
    std::cout << "what() is noexcept: "
              << std::boolalpha << noexcept(ex.what()) << std::endl;

    try
    {
        riskyOperation(false);
        riskyOperation(true);
    }
    catch (std::exception& e)
    {
        std::cerr << "Caught: " << e.what() << std::endl;
    }

    return 0;
}
```

**Sample Output:**
```
what() is noexcept: true
Operation succeeded.
Caught: This is a safe, noexcept exception message.
```

[Back to Table of Contents](#table-of-contents)

---

## Section 9: Exception Handling in Practice

> Bringing all the concepts together with a realistic, complete example demonstrating best practices.

### A Realistic Stack with Exception Handling

The mints example was deliberately simple, but it directly mirrors the kind of exception handling needed when implementing data structures. Here is a minimal integer stack that uses a custom exception hierarchy:

```cpp
#include <iostream>
#include <exception>
#include <vector>
#include <string>

// -------------------------------------------------------
// Custom exception base class for Stack errors
// -------------------------------------------------------
class StackException : public std::exception
{
    std::string message;
public:
    explicit StackException(const std::string& msg) : message(msg) {}

    virtual const char* what() const noexcept override
    {
        return message.c_str();
    }
};

// -------------------------------------------------------
// Derived: underflow (pop from empty stack)
// -------------------------------------------------------
class StackUnderflowException : public StackException
{
public:
    StackUnderflowException()
        : StackException("Stack underflow: cannot pop from an empty stack.") {}
};

// -------------------------------------------------------
// Derived: overflow (push to full stack)
// -------------------------------------------------------
class StackOverflowException : public StackException
{
    int capacity;
public:
    explicit StackOverflowException(int cap)
        : StackException("Stack overflow: maximum capacity reached."),
          capacity(cap) {}

    int getCapacity() const { return capacity; }
};

// -------------------------------------------------------
// Simple fixed-capacity stack using a vector
// -------------------------------------------------------
class IntStack
{
    std::vector<int> data;
    int maxSize;
public:
    explicit IntStack(int capacity) : maxSize(capacity) {}

    void push(int value)
    {
        if (static_cast<int>(data.size()) >= maxSize)
            throw StackOverflowException(maxSize);
        data.push_back(value);
    }

    int pop()
    {
        if (data.empty())
            throw StackUnderflowException();
        int val = data.back();
        data.pop_back();
        return val;
    }

    int peek() const
    {
        if (data.empty())
            throw StackUnderflowException();
        return data.back();
    }

    bool empty() const { return data.empty(); }
    int size() const { return static_cast<int>(data.size()); }
};

// -------------------------------------------------------
// Main: demonstrate the stack with exception handling
// -------------------------------------------------------
int main()
{
    IntStack stack(3);

    // Push within capacity
    try
    {
        stack.push(10);
        stack.push(20);
        stack.push(30);
        std::cout << "Pushed 10, 20, 30 onto stack." << std::endl;

        // This will overflow the stack
        stack.push(40);
    }
    catch (StackOverflowException& ex)
    {
        std::cerr << "[Overflow] " << ex.what()
                  << " Capacity: " << ex.getCapacity() << std::endl;
    }

    // Pop all elements and then attempt one too many
    try
    {
        while (true)
        {
            std::cout << "Popped: " << stack.pop() << std::endl;
        }
    }
    catch (StackUnderflowException& ex)
    {
        std::cerr << "[Underflow] " << ex.what() << std::endl;
    }
    catch (StackException& ex)
    {
        // Catches any other StackException
        std::cerr << "[Stack Error] " << ex.what() << std::endl;
    }

    std::cout << "Stack is empty: "
              << std::boolalpha << stack.empty() << std::endl;

    return 0;
}
```

**Sample Output:**
```
Pushed 10, 20, 30 onto stack.
[Overflow] Stack overflow: maximum capacity reached. Capacity: 3
Popped: 30
Popped: 20
Popped: 10
[Underflow] Stack cannot pop from an empty stack.
Stack is empty: true
```

### Exception Handling Best Practices

- **Catch by reference (`&`)**: avoids slicing (losing derived-class information when catching by value) and avoids unnecessary copies.
- **Throw by value**: throw exception objects by value, not by pointer. The runtime makes a copy of the thrown object.
- **Order catch clauses**: most specific first, most general last.
- **Do not use exceptions for normal control flow**: exceptions carry overhead and should represent genuinely exceptional conditions.
- **Always ensure cleanup**: use RAII patterns (`std::unique_ptr`, `std::fstream`) so resources are released even when exceptions occur.
- **noexcept on destructors and `what()`**: destructors are implicitly `noexcept` in C++11 and later; always mark `what()` as `noexcept`.

```
Exception design summary:

  +--------------------------+
  |   std::exception         |  <-- Base: catch all standard exceptions
  +--------------------------+
             ^
             |
  +--------------------------+
  |   StackException         |  <-- Your library base: catch all stack errors
  +--------------------------+
             ^
        +---------+
        |         |
  +----------+ +----------+
  |Underflow | |Overflow  |  <-- Specific types: catch distinct conditions
  +----------+ +----------+
```

[Back to Table of Contents](#table-of-contents)

---

## Resources

### Official Documentation

- **cppreference.com - Exception handling**
  https://en.cppreference.com/w/cpp/language/exceptions

- **cppreference.com - std::exception**
  https://en.cppreference.com/w/cpp/error/exception

- **cppreference.com - noexcept specifier**
  https://en.cppreference.com/w/cpp/language/noexcept_spec

- **cppreference.com - std::runtime_error and std::logic_error**
  https://en.cppreference.com/w/cpp/error/runtime_error

- **Boost archive_exception (historical reference for throw() syntax)**
  https://www.boost.org/doc/libs/1_34_0/boost/archive/archive_exception.hpp

### Textbooks

- *The C++ Programming Language* (4th Edition) - Bjarne Stroustrup (Chapter 13: Exception Handling)
- *Effective C++* - Scott Meyers (Item 29: Strive for exception-safe code)
- *A Tour of C++* - Bjarne Stroustrup (Chapter 4: Error Handling)

### Online Tutorials and Practice

- **LearnCpp.com - Exception handling**
  https://www.learncpp.com/cpp-tutorial/basic-exception-handling/

- **LearnCpp.com - Custom exceptions**
  https://www.learncpp.com/cpp-tutorial/exceptions-classes-and-inheritance/

- **HackerRank - C++ domain (practice problems)**
  https://www.hackerrank.com/domains/cpp

- **LeetCode - Data structure problems (apply exception handling to stacks and queues)**
  https://leetcode.com/problemset/all/

- **Compiler Explorer (Godbolt) - Test and examine compiled C++ code**
  https://godbolt.org/

[Back to Table of Contents](#table-of-contents)
