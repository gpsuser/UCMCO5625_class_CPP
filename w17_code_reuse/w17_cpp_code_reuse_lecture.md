# Organizing and Documenting Larger C++ Projects

## Learning Outcomes

By the end of this lecture, you should be able to:

* Organize C++ projects into multiple files using header and implementation files
* Distinguish between when to use .h/.cpp separation and .hpp files for templates
* Create and use custom header files with proper include guards
* Understand the difference between quoted and angled bracket includes
* Implement operator overloading in separate header files
* Document C++ code using Javadoc-style comments for Doxygen
* Generate HTML documentation from source code using Doxygen
* Define and use custom namespaces to avoid naming conflicts
* Create multi-threaded applications using the C++ Standard Thread Library
* Implement thread synchronization and data sharing between threads
* Apply threading to solve practical programming problems

---

## Table of Contents

1. [Introduction to Multi-File Projects](#section-1-introduction-to-multi-file-projects)
2. [Header Files and Implementation Files](#section-2-header-files-and-implementation-files)
3. [Working with Templates](#section-3-working-with-templates)
4. [Code Documentation with Doxygen](#section-4-code-documentation-with-doxygen)
5. [Namespaces](#section-5-namespaces)
6. [Standard Thread Library](#section-6-standard-thread-library)
7. [Resources](#resources)

---

## Section 1: Introduction to Multi-File Projects

As C++ projects grow in size and complexity, organizing code into multiple files becomes essential for maintainability, reusability, and team collaboration. Rather than placing all code in a single source file, we can structure our projects into logical units.

### Why Use Multiple Files?

* **Separation of Concerns**: Each class or group of related functions can have its own file
* **Compilation Efficiency**: Only modified files need to be recompiled
* **Code Reusability**: Header files can be included in multiple projects
* **Team Collaboration**: Different team members can work on different files
* **Readability**: Smaller files are easier to understand and maintain

### File Organization Strategy

The general organization strategy for C++ projects is:

* **Each class** gets its own header file (.h) and possibly its own implementation file (.cpp)
* **Related standalone functions** are grouped together (e.g., math functions, graphics functions)
* **Template code** goes entirely in .hpp files (no separate implementation)

### Why Templates Are Different

Templates require special handling because their compilation depends on template parameters. The compiler needs to see the full implementation when instantiating a template, so we cannot compile template code in advance. Therefore, template declarations and implementations must be in the same file (conventionally .hpp).

```
Project Structure Example:
├── main.cpp
├── Height.h           (class declaration)
├── Height.cpp         (class implementation)
├── MathUtils.h        (utility functions declaration)
├── MathUtils.cpp      (utility functions implementation)
└── Container.hpp      (template class - all code)
```

[Back to Table of Contents](#table-of-contents)

---

## Section 2: Header Files and Implementation Files

Header files (.h) and implementation files (.cpp) work together to separate interface from implementation. This separation is a fundamental principle in C++ programming.

### The Header File (.h)

The header file contains:
* Class declarations
* Method declarations (signatures)
* Inline functions
* Constants and type definitions
* Include guards or `#pragma once`

### The Implementation File (.cpp)

The implementation file contains:
* Method implementations
* Definitions of non-inline functions
* Includes the corresponding header file

### Example: Height Class

Let's examine a complete example of a Height class that demonstrates proper file organization.

**Height.h:**

```cpp
#pragma once
#include <iostream>

class Height
{
    int ft, in;
public:
    Height() {}
    Height(int feet, int inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height& rhs) const;
    bool operator > (const Height& rhs) const;
    Height operator + (const Height& rhs) const;
    
    friend std::ostream& operator << (std::ostream& o, const Height& h);
};
```

**Height.cpp:**

```cpp
#include "Height.h"

bool Height::operator < (const Height& rhs) const
{
    int totalInchesThis = ft * 12 + in;
    int totalInchesRhs = rhs.ft * 12 + rhs.in;
    return totalInchesThis < totalInchesRhs;
}

bool Height::operator > (const Height& rhs) const
{
    int totalInchesThis = ft * 12 + in;
    int totalInchesRhs = rhs.ft * 12 + rhs.in;
    return totalInchesThis > totalInchesRhs;
}

Height Height::operator + (const Height& rhs) const
{
    int totalInches = (ft * 12 + in) + (rhs.ft * 12 + rhs.in);
    return Height(totalInches / 12, totalInches % 12);
}

std::ostream& operator << (std::ostream& o, const Height& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Understanding #pragma once

The `#pragma once` directive is an include guard that prevents multiple inclusion of the same header file. Without it, including the same header twice would cause compilation errors due to duplicate definitions.

Traditional include guards look like this:
```cpp
#ifndef HEIGHT_H
#define HEIGHT_H
// header content
#endif
```

However, `#pragma once` is simpler and widely supported by modern compilers.

### When to Implement in Header vs CPP

**Implement in the header file (.h):**
* Very short functions (single line)
* Inline functions
* Template code (actually in .hpp)

**Implement in the implementation file (.cpp):**
* Multi-line methods
* Functions that may change frequently
* Any function where compilation time matters

This separation allows the .cpp file to be compiled once to an object (.o) file and only recompiled when the implementation changes.

### Using Your Header File

Here's a complete program that uses the Height class:

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.h"

int main()
{
    std::vector<Height> heights;
    heights.push_back(Height(6, 2));
    heights.push_back(Height(4, 9));
    heights.push_back(Height(5, 11));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights:" << std::endl;
    for (auto i : heights)
        std::cout << i << std::endl;
    
    Height total(0, 0);
    for (auto i : heights)
        total = total + i;
    
    std::cout << "Total height: " << total << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted heights:
4'9"
5'11"
6'2"
Total height: 16'10"
```

### Include Conventions: Quotes vs Angle Brackets

Notice the different include styles:

```cpp
#include <iostream>      // Standard library - angle brackets
#include "Height.h"      // Project file - quotes
```

**Angle brackets `< >`:** The compiler searches in system include directories (where standard library headers are located)

**Quotes `" "`:** The compiler first searches in the current project directory, then falls back to system directories

This convention helps distinguish between:
* Standard library and third-party library headers (angle brackets)
* Your project's headers (quotes)

[Back to Table of Contents](#table-of-contents)

---

## Section 3: Working with Templates

Templates require special consideration because their compilation is parameter-dependent. This section explores how to organize template code effectively.

### Why Templates Need Different Organization

Consider what happens when you use a template:

```cpp
std::vector<int> numbers;        // Compiler generates vector for int
std::vector<std::string> words;  // Compiler generates vector for string
```

The compiler must generate different code for each template parameter combination. This means:
* The implementation must be visible at compile time
* We cannot pre-compile templates into .o files
* Declaration and implementation must be in the same file

### The .hpp Convention

By convention, template code goes in .hpp files. While this is not required by the C++ standard, it's a widely recognized practice that signals "this file contains templates."

### Template Height Class Example

Let's convert our Height class to a template that allows fractional inches:

**Height.hpp:**

```cpp
#pragma once
#include <iostream>

// Template class declaration
template <typename T>
class Height
{
    int ft;
    T in;
public:
    Height() : ft(0), in(0) {}
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height<T>& rhs) const;
    bool operator > (const Height<T>& rhs) const;
    Height<T> operator + (const Height<T>& rhs) const;
    
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Template implementation (in same file)
template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<T>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Using the Template

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.hpp"

int main()
{
    std::vector<Height<double>> heights;
    heights.push_back(Height<double>(6, 2.5));
    heights.push_back(Height<double>(4, 9.75));
    heights.push_back(Height<double>(5, 11.8));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights:" << std::endl;
    for (auto i : heights)
        std::cout << i << std::endl;
    
    Height<double> total(0, 0.0);
    for (auto i : heights)
        total = total + i;
    
    std::cout << "Total height: " << total << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted heights:
4'9.75"
5'11.8"
6'2.5"
Total height: 17'0.05"
```

### Template Organization Best Practices

Even though everything goes in one file, we still maintain separation between:

1. **Declaration section** (top of file):
   * Class template declaration
   * Method signatures
   * Documentation comments

2. **Implementation section** (bottom of file):
   * Method implementations
   * Template specializations

This organization preserves readability and makes the header file serve as documentation.

[Back to Table of Contents](#table-of-contents)

---

## Section 4: Code Documentation with Doxygen

As we create reusable header files, documentation becomes crucial. Other programmers (including your future self) need clear instructions on how to use your code.

### Why Document?

When you've invested time in creating reusable code following the DRY (Don't Repeat Yourself) principle, documentation ensures that effort pays off. Well-documented headers can be as accessible as the Standard Library.

### Javadoc-Style Comments

Javadoc is a documentation standard that works with both Java and C++. Special comment blocks are processed by tools like Doxygen to generate HTML documentation.

### Doxygen Comment Syntax

**Special comment blocks:**
```cpp
/**
 * This is a Doxygen comment block
 */
```

**Common Doxygen tags:**
* `@brief` - Short description
* `@param` - Parameter description
* `@return` - Return value description
* `@tparam` - Template parameter description
* `@throw` - Exception that may be thrown
* `@note` - Additional notes
* `@warning` - Warnings about usage

### Documented Height Template

Here's the Height class with complete Doxygen documentation:

**Height.hpp (documented):**

```cpp
#pragma once
#include <iostream>

/**
 * @brief A template class representing a height in feet and inches
 * 
 * This class stores a height using separate values for feet (integer)
 * and inches (template type). It provides comparison operators and
 * addition functionality.
 * 
 * @tparam T The type for storing inches (e.g., int, double, float)
 */
template <typename T>
class Height
{
    int ft;  ///< Feet component of the height
    T in;    ///< Inches component of the height

public:
    /**
     * @brief Default constructor
     * 
     * Initializes height to 0 feet, 0 inches
     */
    Height() : ft(0), in(0) {}
    
    /**
     * @brief Construct a height from feet and inches
     * 
     * @param feet The number of feet
     * @param inches The number of inches
     */
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    /**
     * @brief Less-than comparison operator
     * 
     * Compares two heights by converting both to total inches
     * 
     * @param rhs The height to compare against
     * @return true if this height is less than rhs
     */
    bool operator < (const Height<T>& rhs) const;
    
    /**
     * @brief Greater-than comparison operator
     * 
     * @param rhs The height to compare against
     * @return true if this height is greater than rhs
     */
    bool operator > (const Height<T>& rhs) const;
    
    /**
     * @brief Addition operator
     * 
     * Adds two heights together, properly handling inch overflow
     * 
     * @param rhs The height to add
     * @return A new Height object containing the sum
     */
    Height<T> operator + (const Height<T>& rhs) const;
    
    /**
     * @brief Stream output operator
     * 
     * Outputs height in the format: feet'inches"
     * 
     * @tparam U Template parameter for Height
     * @param o The output stream
     * @param h The height to output
     * @return Reference to the output stream
     */
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Implementation section

template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<U>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Generating Documentation with Doxygen

1. **Install Doxygen** from https://www.doxygen.nl/index.html

2. **Create a Doxygen configuration file**:
   ```bash
   doxygen -g
   ```

3. **Edit the Doxyfile** to set:
   * `PROJECT_NAME = "Height Class Documentation"`
   * `INPUT = .` (or your source directory)
   * `RECURSIVE = YES`
   * `EXTRACT_ALL = YES`

4. **Generate documentation**:
   ```bash
   doxygen Doxyfile
   ```

5. **View the HTML output** in `html/index.html`

The generated documentation will include:
* Class hierarchy
* Member descriptions
* Parameter and return value information
* Links between related classes
* Search functionality

### Documentation Best Practices

* **Document the interface, not the implementation** - Focus on what the code does, not how
* **Keep documentation up-to-date** - Outdated documentation is worse than no documentation
* **Be concise but complete** - Include all necessary information without being verbose
* **Use consistent formatting** - Follow a standard style throughout your project
* **Document edge cases and limitations** - Help users avoid common pitfalls

[Back to Table of Contents](#table-of-contents)

---

## Section 5: Namespaces

As you create more reusable code, the risk of naming conflicts increases. Namespaces provide a way to organize code and prevent these conflicts.

### The Problem: Name Collisions

Imagine two libraries both define a class called `Height`:

```cpp
// Library A's Height
class Height { /* ... */ };

// Library B's Height
class Height { /* ... */ };
```

Which `Height` does the compiler use? This causes a compilation error.

### The Solution: Namespaces

Namespaces create distinct scopes for identifiers:

```cpp
namespace LibraryA {
    class Height { /* ... */ };
}

namespace LibraryB {
    class Height { /* ... */ };
}
```

Now we can use both:
```cpp
LibraryA::Height h1;
LibraryB::Height h2;
```

### Creating a Namespace

Wrap your code in a namespace declaration:

```cpp
namespace MyNamespace {
    // All your code goes here
    class MyClass { /* ... */ };
    void myFunction() { /* ... */ }
}
```

### Height Class with Namespace

Here's the complete Height class in the CO5625 namespace:

**Height.hpp:**

```cpp
#pragma once
#include <iostream>

namespace CO5625 {

/**
 * @brief A template class representing a height in feet and inches
 * @tparam T The type for storing inches
 */
template <typename T>
class Height
{
    int ft;
    T in;

public:
    Height() : ft(0), in(0) {}
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height<T>& rhs) const;
    bool operator > (const Height<T>& rhs) const;
    Height<T> operator + (const Height<T>& rhs) const;
    
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Implementation
template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<T>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}

} // namespace CO5625
```

### Using Code from a Namespace

**Option 1: Fully qualified names**
```cpp
CO5625::Height<double> h(6, 2.5);
```

**Option 2: Using declarations**
```cpp
using CO5625::Height;
Height<double> h(6, 2.5);
```

**Option 3: Using directives**
```cpp
using namespace CO5625;
Height<double> h(6, 2.5);
```

### Complete Example with Namespace

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.hpp"

using namespace CO5625;

int main()
{
    std::vector<Height<double>> heights;
    heights.push_back(Height<double>(6, 2.5));
    heights.push_back(Height<double>(4, 9.75));
    heights.push_back(Height<double>(5, 11.8));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Heights from CO5625 namespace:" << std::endl;
    for (const auto& h : heights)
        std::cout << h << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Heights from CO5625 namespace:
4'9.75"
5'11.8"
6'2.5"
```

### Namespace Best Practices

* **Use namespaces for all reusable code** - Protect against future conflicts
* **Choose descriptive namespace names** - Avoid generic names like "Utilities"
* **Avoid `using namespace` in headers** - This pollutes the global namespace for all includers
* **Nest namespaces for organization** - e.g., `MyCompany::Graphics::2D`
* **Use inline namespaces for versioning** - Allow backward compatibility

### Nested Namespaces

```cpp
namespace CO5625 {
    namespace DataStructures {
        class LinkedList { /* ... */ };
    }
    
    namespace Algorithms {
        void quickSort() { /* ... */ };
    }
}

// C++17 syntax
namespace CO5625::DataStructures {
    class LinkedList { /* ... */ };
}
```

[Back to Table of Contents](#table-of-contents)

---

## Section 6: Standard Thread Library

Modern C++ provides robust threading support through the Standard Thread Library (std::thread). This allows programs to execute multiple tasks concurrently, improving performance on multi-core processors.

### Introduction to std::thread

The `<thread>` header provides the std::thread class for creating and managing threads. Unlike older threading libraries (like Boost.Thread or POSIX threads), std::thread is part of the C++ standard and works across platforms.

### Basic Thread Concepts

A **thread** is an independent sequence of execution within a program. Multiple threads can run concurrently:

```
Single-threaded:           Multi-threaded:
    Task A                     Task A
    Task B                     Task B  (running simultaneously)
    Task C                     Task C
```

**Key advantages:**
* Utilize multiple CPU cores
* Keep UI responsive while processing
* Handle multiple clients simultaneously
* Improve throughput for parallel tasks

**Key challenges:**
* Race conditions (concurrent access to shared data)
* Deadlocks (threads waiting for each other)
* Synchronization overhead

### Thread Lifecycle

```
Created -> Running -> Joined/Detached -> Terminated

1. Create thread with function/lambda
2. Thread executes concurrently
3. Main thread can:
   - join() - wait for thread to complete
   - detach() - let thread run independently
4. Thread terminates when function completes
```

### Worked Example 1: Basic Thread Creation

This example demonstrates creating and joining a simple thread:

```cpp
#include <iostream>
#include <thread>
#include <chrono>

// Function to be executed by thread
void printNumbers(int count)
{
    for (int i = 1; i <= count; ++i)
    {
        std::cout << "Thread: " << i << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

int main()
{
    std::cout << "Main thread starting..." << std::endl;
    
    // Create and start a thread
    std::thread workerThread(printNumbers, 5);
    
    // Main thread continues executing
    for (int i = 1; i <= 5; ++i)
    {
        std::cout << "Main: " << i << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(150));
    }
    
    // Wait for worker thread to complete
    workerThread.join();
    
    std::cout << "Main thread finished." << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Main thread starting...
Thread: 1
Main: 1
Thread: 2
Thread: 3
Main: 2
Thread: 4
Main: 3
Thread: 5
Main: 4
Main: 5
Main thread finished.
```

**Key points:**
* `std::thread` constructor takes a function and its arguments
* Both threads execute concurrently
* `join()` blocks until the thread completes
* Always join or detach threads before they go out of scope

### Worked Example 2: Thread Synchronization with Mutex

When threads share data, we need synchronization to prevent race conditions:

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

std::mutex coutMutex;  // Protects std::cout
int sharedCounter = 0;
std::mutex counterMutex;  // Protects sharedCounter

void incrementCounter(int threadId, int iterations)
{
    for (int i = 0; i < iterations; ++i)
    {
        // Lock mutex before modifying shared data
        {
            std::lock_guard<std::mutex> lock(counterMutex);
            ++sharedCounter;
        }
        
        // Lock mutex before printing
        {
            std::lock_guard<std::mutex> lock(coutMutex);
            std::cout << "Thread " << threadId 
                      << " incremented counter to " 
                      << sharedCounter << std::endl;
        }
        
        std::this_thread::sleep_for(std::chrono::milliseconds(10));
    }
}

int main()
{
    const int numThreads = 3;
    const int iterations = 5;
    
    std::vector<std::thread> threads;
    
    std::cout << "Starting " << numThreads << " threads..." << std::endl;
    
    // Create threads
    for (int i = 0; i < numThreads; ++i)
    {
        threads.push_back(std::thread(incrementCounter, i + 1, iterations));
    }
    
    // Wait for all threads to complete
    for (auto& t : threads)
    {
        t.join();
    }
    
    std::cout << "\nFinal counter value: " << sharedCounter << std::endl;
    std::cout << "Expected value: " << (numThreads * iterations) << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Starting 3 threads...
Thread 1 incremented counter to 1
Thread 2 incremented counter to 2
Thread 3 incremented counter to 3
Thread 1 incremented counter to 4
Thread 2 incremented counter to 5
...
Thread 3 incremented counter to 15

Final counter value: 15
Expected value: 15
```

**Key concepts:**
* `std::mutex` provides mutual exclusion
* `std::lock_guard` automatically locks/unlocks (RAII pattern)
* Lock scope should be as small as possible
* Without mutex, race conditions cause incorrect results

### Race Condition Visualization

```
Without mutex (race condition):
Thread 1: Read counter (0) -> Increment (1) -> Write (1)
Thread 2:      Read counter (0) -> Increment (1) -> Write (1)
Result: Counter = 1 (should be 2!)

With mutex (synchronized):
Thread 1: Lock -> Read (0) -> Increment -> Write (1) -> Unlock
Thread 2:                    Wait -> Lock -> Read (1) -> Increment -> Write (2) -> Unlock
Result: Counter = 2 (correct!)
```

### Worked Example 3: Producer-Consumer Pattern

A common threading pattern where one thread produces data and another consumes it:

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <chrono>

std::queue<int> dataQueue;
std::mutex queueMutex;
std::condition_variable dataCondition;
bool finished = false;

void producer(int itemCount)
{
    for (int i = 1; i <= itemCount; ++i)
    {
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
        
        {
            std::lock_guard<std::mutex> lock(queueMutex);
            dataQueue.push(i);
            std::cout << "Produced: " << i << std::endl;
        }
        
        // Notify consumer that data is available
        dataCondition.notify_one();
    }
    
    // Signal completion
    {
        std::lock_guard<std::mutex> lock(queueMutex);
        finished = true;
    }
    dataCondition.notify_one();
}

void consumer()
{
    while (true)
    {
        std::unique_lock<std::mutex> lock(queueMutex);
        
        // Wait for data or completion signal
        dataCondition.wait(lock, []{ return !dataQueue.empty() || finished; });
        
        // Process all available data
        while (!dataQueue.empty())
        {
            int value = dataQueue.front();
            dataQueue.pop();
            lock.unlock();
            
            std::cout << "Consumed: " << value << std::endl;
            std::this_thread::sleep_for(std::chrono::milliseconds(150));
            
            lock.lock();
        }
        
        // Exit if finished and queue is empty
        if (finished && dataQueue.empty())
            break;
    }
}

int main()
{
    std::cout << "Starting Producer-Consumer example..." << std::endl;
    
    std::thread producerThread(producer, 10);
    std::thread consumerThread(consumer);
    
    producerThread.join();
    consumerThread.join();
    
    std::cout << "All items processed." << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Starting Producer-Consumer example...
Produced: 1
Consumed: 1
Produced: 2
Produced: 3
Consumed: 2
Produced: 4
Consumed: 3
Produced: 5
Consumed: 4
...
Consumed: 10
All items processed.
```

**Key concepts:**
* `std::condition_variable` allows threads to wait for events
* Producer signals consumer when data is ready
* Consumer waits efficiently (no busy-waiting)
* Flag indicates when production is complete

### Threading Tasks

Now try these exercises to practice multi-threaded programming:

**Task 1: Parallel Array Sum**

Create a program that splits an array into sections and uses multiple threads to calculate partial sums, then combines them for the total sum.

Requirements:
* Create an array of 1000 integers
* Use 4 threads to sum different sections
* Combine results to get total sum
* Compare performance with single-threaded version

**Task 2: Thread-Safe Logger**

Implement a simple logging system where multiple threads can write log messages without corruption.

Requirements:
* Create a Logger class with a log() method
* Use mutex to protect file/console output
* Create 5 threads that each write 10 log messages
* Each message should include thread ID and timestamp
* Ensure all messages are written correctly

**Task 3: Parallel File Search**

Create a program that searches for a string in multiple files using parallel threads.

Requirements:
* Create 5 text files with random content
* Search for a specific word across all files
* Use one thread per file
* Collect and display results showing which files contain the word
* Report total time taken

### Thread Synchronization Primitives Summary

| Primitive | Purpose | Use When |
|-----------|---------|----------|
| `std::mutex` | Mutual exclusion | Protecting shared data |
| `std::lock_guard` | Scoped locking | Simple lock/unlock pattern |
| `std::unique_lock` | Flexible locking | Need to manually unlock or use with condition variables |
| `std::condition_variable` | Thread notification | Waiting for events or conditions |
| `std::atomic` | Lock-free operations | Simple counters or flags |

### Threading Best Practices

1. **Minimize shared state** - Less sharing means fewer synchronization issues
2. **Keep critical sections small** - Hold locks for minimal time
3. **Avoid deadlocks** - Always acquire multiple locks in the same order
4. **Use RAII for locks** - `lock_guard` and `unique_lock` prevent forgetting to unlock
5. **Consider std::atomic** - For simple shared variables, atomics are faster than mutexes
6. **Test with thread sanitizers** - Tools can detect race conditions and deadlocks
7. **Document thread safety** - Clearly indicate which functions are thread-safe

### Common Threading Pitfalls

**Pitfall 1: Forgetting to join or detach**
```cpp
// BAD - thread object destroyed before thread completes
{
    std::thread t(someFunction);
} // t destroyed - std::terminate() called!

// GOOD
{
    std::thread t(someFunction);
    t.join(); // or t.detach();
}
```

**Pitfall 2: Returning reference to local data**
```cpp
// BAD - data destroyed before thread uses it
void createThread()
{
    int localData = 42;
    std::thread t([&](){ std::cout << localData; });
    t.detach(); // localData destroyed when function returns!
}

// GOOD - pass by value
void createThread()
{
    int localData = 42;
    std::thread t([localData](){ std::cout << localData; });
    t.detach(); // safe - localData copied
}
```

**Pitfall 3: Data races**
```cpp
// BAD - no synchronization
int counter = 0;
std::thread t1([&](){ ++counter; });
std::thread t2([&](){ ++counter; });

// GOOD - use atomic or mutex
std::atomic<int> counter(0);
std::thread t1([&](){ ++counter; });
std::thread t2([&](){ ++counter; });
```

[Back to Table of Contents](#table-of-contents)

---

## Resources

### Official Documentation
* **C++ Reference**: https://en.cppreference.com/
  * Header files: https://en.cppreference.com/w/cpp/header
  * Threading: https://en.cppreference.com/w/cpp/thread
* **ISO C++ Standards**: https://isocpp.org/
* **Doxygen Manual**: https://www.doxygen.nl/manual/index.html

### Project Organization
* **Google C++ Style Guide**: https://google.github.io/styleguide/cppguide.html
* **C++ Core Guidelines**: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines

### Threading Resources
* **C++ Concurrency in Action** by Anthony Williams - Comprehensive threading guide
* **Thread Support Library**: https://en.cppreference.com/w/cpp/thread
* **Mutex Reference**: https://en.cppreference.com/w/cpp/thread/mutex
* **Condition Variables**: https://en.cppreference.com/w/cpp/thread/condition_variable

### Tools
* **Doxygen**: https://www.doxygen.nl/index.html - Documentation generator
* **CMake**: https://cmake.org/ - Build system for multi-file projects
* **Valgrind**: https://valgrind.org/ - Memory and threading analysis (Linux)
* **Thread Sanitizer**: Part of Clang/GCC - Detects data races

### Practice and Learning
* **LeetCode**: https://leetcode.com/problemset/ - Algorithm practice
* **HackerRank C++**: https://www.hackerrank.com/domains/cpp
* **Exercism C++ Track**: https://exercism.org/tracks/cpp
* **Project Euler**: https://projecteuler.net/ - Mathematical programming challenges

### Community
* **Stack Overflow**: https://stackoverflow.com/questions/tagged/c++
* **Reddit r/cpp**: https://www.reddit.com/r/cpp/
* **C++ Slack**: https://cpplang.slack.com/
* **ISO C++ Forums**: https://isocpp.org/forums

### Further Reading
* **The C++ Programming Language** by Bjarne Stroustrup
* **Effective C++** by Scott Meyers
* **Modern C++ Design** by Andrei Alexandrescu
* **C++ Concurrency in Action** by Anthony Williams

---

[Back to Table of Contents](#table-of-contents)