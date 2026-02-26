# C++ Worksheet

## Quiz Section

### Question 1

Complete the following statement by filling in the blanks using words from the word bank below.

The `______ once` directive is an include guard that prevents `______ inclusion` of the same header file. Without it, including the same header twice would cause `______ errors` due to duplicate `______`. The traditional alternative uses `______ HEIGHT_H` and `______ HEIGHT_H` directives, but the modern approach is simpler and widely `______` by compilers.

**Word Bank:** #define, supported, #pragma, multiple, compilation, #ifndef, definitions

---

### Question 2

Complete the following statement by filling in the blanks using words from the word bank below.

Templates require special handling because their compilation is `______-dependent`. The compiler must generate different `______ for each template parameter combination`. This means the implementation must be `______ at compile time`, so we cannot pre-compile templates into `______ files`. Therefore, template declarations and implementations must be in the `______ file`, conventionally using the `______ extension`.

**Word Bank:** .o, visible, same, .hpp, code, parameter

---

### Question 3

Complete the following statement by filling in the blanks using words from the word bank below.

Doxygen uses special `______ blocks` to generate HTML documentation. The `______ tag provides a short description`, while `______ describes each parameter` and `______ explains the return value`. Template parameters use the `______ tag`. The generated documentation includes class `______`, detailed member descriptions, and `______ functionality` to find classes and methods.

**Word Bank:** @param, search, @brief, @tparam, hierarchy, comment, @return

---

### Question 4

Complete the following statement by filling in the blanks using words from the word bank below.

Namespaces create distinct `______ for identifiers` to prevent naming `______`. Code within a namespace can be accessed using `______ qualified names` like `MyNamespace::MyClass`, or by using `______ declarations`. It's important to avoid `using ______` in header files as this pollutes the `______ namespace` for all includers.

**Word Bank:** global, fully, conflicts, using, scopes, namespace

---

### Question 5

When organizing C++ projects, what is the correct convention for include directives?

A) Use angle brackets `< >` for all includes to ensure consistency  
B) Use quotes `" "` for all includes to search the project directory first  
C) Use angle brackets `< >` for standard library headers and quotes `" "` for project files  
D) Use quotes `" "` for standard library headers and angle brackets `< >` for project files

---

### Question 6

Which of the following statements about separating code into .h and .cpp files is correct?

A) All class methods should always be implemented in the .cpp file for best practice  
B) Template classes should have their declaration in .h and implementation in .cpp  
C) Very short inline functions can be implemented in the header, while multi-line methods go in the .cpp file  
D) The .h file should contain implementations and the .cpp file should contain declarations

---

### Question 7

What is the best practice regarding namespaces in reusable C++ code?

A) Avoid using namespaces as they add unnecessary complexity  
B) Use generic namespace names like "Utilities" for flexibility  
C) Always use `using namespace` directives at the top of header files  
D) Use descriptive namespace names and avoid `using namespace` in headers

---

## Task Section

### Task 1: Multi-File Temperature Class

Create a Temperature class that converts between Celsius and Fahrenheit. Organize it using proper header/implementation file separation.

**Requirements:**
- Create Temperature.h with class declaration
- Create Temperature.cpp with method implementations
- Store temperature internally in Celsius (double)
- Provide methods: getCelsius(), getFahrenheit(), setCelsius(double), setFahrenheit(double)
- Overload operator<< to output in format: "25.0C (77.0F)"
- Include proper include guards

**Scaffolding - Temperature.h:**
```cpp
#pragma once
#include <iostream>

class Temperature
{
private:
    double celsius;

public:
    Temperature();
    Temperature(double c);
    
    // Add method declarations here
    
    friend std::ostream& operator<<(std::ostream& os, const Temperature& t);
};
```

**Scaffolding - Temperature.cpp:**
```cpp
#include "Temperature.h"

Temperature::Temperature() : celsius(0.0) {}

Temperature::Temperature(double c) : celsius(c) {}

// Implement methods here
// Conversion formulas:
// F = C * 9/5 + 32
// C = (F - 32) * 5/9
```

**Scaffolding - main.cpp:**
```cpp
#include <iostream>
#include "Temperature.h"

int main()
{
    Temperature t1(25.0);
    std::cout << "Temperature 1: " << t1 << std::endl;
    
    Temperature t2;
    t2.setFahrenheit(98.6);
    std::cout << "Temperature 2: " << t2 << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Temperature 1: 25.0C (77.0F)
Temperature 2: 37.0C (98.6F)
```

---

### Task 2: Documented Stack Template

Create a template-based Stack class with full Doxygen documentation. The stack should store elements in a vector and provide standard stack operations.

**Requirements:**
- Create Stack.hpp with template class
- Include Doxygen comments for class and all methods
- Implement: push(), pop(), top(), isEmpty(), size()
- Use std::vector for internal storage
- Include proper documentation tags: @brief, @tparam, @param, @return, @throw

**Scaffolding - Stack.hpp:**
```cpp
#pragma once
#include <vector>
#include <stdexcept>

/**
 * @brief A template-based stack data structure
 * 
 * @tparam T The type of elements stored in the stack
 */
template <typename T>
class Stack
{
private:
    std::vector<T> data;

public:
    /**
     * @brief Add documentation here
     */
    void push(const T& item)
    {
        // Implement
    }
    
    /**
     * @brief Add documentation here
     */
    void pop()
    {
        // Implement - throw exception if empty
    }
    
    // Add remaining methods with documentation
};
```

**Scaffolding - main.cpp:**
```cpp
#include <iostream>
#include <string>
#include "Stack.hpp"

int main()
{
    Stack<int> intStack;
    intStack.push(10);
    intStack.push(20);
    intStack.push(30);
    
    std::cout << "Top element: " << intStack.top() << std::endl;
    std::cout << "Stack size: " << intStack.size() << std::endl;
    
    intStack.pop();
    std::cout << "After pop, top: " << intStack.top() << std::endl;
    
    Stack<std::string> strStack;
    strStack.push("Hello");
    strStack.push("World");
    std::cout << "String stack top: " << strStack.top() << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Top element: 30
Stack size: 3
After pop, top: 20
String stack top: World
```

---

### Challenge Task: Complete Library System

Create a small library system with multiple files, namespaces, and Doxygen documentation. The system should manage books with proper organization.

**Requirements:**
- Create Book class (Book.h and Book.cpp) with: title, author, ISBN, available status
- Create Library template (Library.hpp) that can store any type of item
- Put everything in a "LibrarySystem" namespace
- Include full Doxygen documentation
- Book should have: getters, setters, checkout(), returnBook(), operator<<
- Library should have: addItem(), removeItem(), findItem(), listAll()
- Create a main.cpp that demonstrates the functionality

**Scaffolding - Book.h:**
```cpp
#pragma once
#include <string>
#include <iostream>

namespace LibrarySystem {

/**
 * @brief Represents a book in the library system
 */
class Book
{
private:
    std::string title;
    std::string author;
    std::string isbn;
    bool available;

public:
    /**
     * @brief Construct a new Book object
     * @param t Book title
     * @param a Book author  
     * @param i ISBN number
     */
    Book(const std::string& t, const std::string& a, const std::string& i);
    
    // Add method declarations with documentation
    
    friend std::ostream& operator<<(std::ostream& os, const Book& book);
};

} // namespace LibrarySystem
```

**Scaffolding - Library.hpp:**
```cpp
#pragma once
#include <vector>
#include <algorithm>
#include <iostream>

namespace LibrarySystem {

/**
 * @brief A template class for managing library items
 * @tparam T The type of items to store
 */
template <typename T>
class Library
{
private:
    std::vector<T> items;
    std::string libraryName;

public:
    /**
     * @brief Construct a new Library object
     * @param name The name of the library
     */
    Library(const std::string& name) : libraryName(name) {}
    
    // Implement methods with documentation
};

} // namespace LibrarySystem
```

**Sample Output:**
```
=== City Library ===
Books in library: 3

All Books:
"1984" by George Orwell [ISBN: 978-0451524935] - Available
"To Kill a Mockingbird" by Harper Lee [ISBN: 978-0061120084] - Available
"The Great Gatsby" by F. Scott Fitzgerald [ISBN: 978-0743273565] - Available

Checking out: 1984
Book checked out successfully

After checkout:
"1984" by George Orwell [ISBN: 978-0451524935] - Checked Out

Returning book...
Book returned successfully
```

---

## Standard Thread Library Section

### Threading Task 1: Basic Thread Creation

Create a program that demonstrates basic thread creation and joining using the C++ standard library.

**Requirements:**
- Create 3 threads that each print a message with their thread ID
- Each thread should print 5 messages
- Use std::this_thread::get_id() to get thread ID
- Properly join all threads before program exits

**Scaffolding:**
```cpp
#include <iostream>
#include <thread>
#include <string>

void printMessages(const std::string& threadName)
{
    // Implement: print 5 messages with thread ID
    // Format: "[ThreadName] Message X - Thread ID: [id]"
}

int main()
{
    // Create and start 3 threads
    // Join all threads
    
    std::cout << "All threads completed" << std::endl;
    return 0;
}
```

**Sample Output:**
```
[Thread-A] Message 1 - Thread ID: 139876543210000
[Thread-B] Message 1 - Thread ID: 139876543211000
[Thread-A] Message 2 - Thread ID: 139876543210000
[Thread-C] Message 1 - Thread ID: 139876543212000
...
All threads completed
```

---

### Threading Task 2: Thread-Safe Counter

Implement a thread-safe counter using std::mutex to prevent race conditions.

**Requirements:**
- Create a Counter class with increment() and getValue() methods
- Use std::mutex to protect the counter value
- Create 5 threads that each increment the counter 1000 times
- Display the final count (should be 5000)

**Scaffolding:**
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

class Counter
{
private:
    int count;
    std::mutex mtx;

public:
    Counter() : count(0) {}
    
    void increment()
    {
        // Use std::lock_guard to protect count
    }
    
    int getValue()
    {
        // Protect access to count
    }
};

void incrementCounter(Counter& counter, int times)
{
    // Implement: increment counter 'times' times
}

int main()
{
    Counter counter;
    std::vector<std::thread> threads;
    
    // Create 5 threads, each incrementing 1000 times
    
    // Join all threads
    
    std::cout << "Final count: " << counter.getValue() << std::endl;
    return 0;
}
```

**Sample Output:**
```
Final count: 5000
```

---

### Threading Task 3: Producer-Consumer Pattern

Implement a simple producer-consumer pattern using std::thread, std::mutex, and std::condition_variable.

**Requirements:**
- Create a shared queue (std::queue<int>)
- Producer thread adds 10 numbers to the queue (0-9)
- Consumer thread removes and prints numbers from the queue
- Use condition_variable to signal when data is available
- Properly handle queue synchronization

**Scaffolding:**
```cpp
#include <iostream>
#include <thread>
#include <queue>
#include <mutex>
#include <condition_variable>

std::queue<int> dataQueue;
std::mutex mtx;
std::condition_variable cv;
bool finished = false;

void producer()
{
    // Produce numbers 0-9
    // Lock mutex, push to queue, notify consumer
    // Set finished = true when done
}

void consumer()
{
    // Wait for data using condition_variable
    // Process items from queue
    // Continue until finished and queue is empty
}

int main()
{
    std::thread prod(producer);
    std::thread cons(consumer);
    
    prod.join();
    cons.join();
    
    std::cout << "Producer-Consumer completed" << std::endl;
    return 0;
}
```

**Sample Output:**
```
Consumer received: 0
Consumer received: 1
Consumer received: 2
Consumer received: 3
Consumer received: 4
Consumer received: 5
Consumer received: 6
Consumer received: 7
Consumer received: 8
Consumer received: 9
Producer-Consumer completed
```