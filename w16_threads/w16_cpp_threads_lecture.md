# Multithreaded Programming with Boost Threads

## Learning Outcomes

By the end of this lecture, you should be able to:

* Explain the concept of multithreaded programming and its benefits
* Create and manage threads using the Boost thread library
* Pass parameters to threads using both function parameters and function objects
* Control thread execution using boost::this_thread and timing functions
* Synchronize threads using join() and try_join_for()
* Identify and prevent data races in multithreaded programs
* Implement thread-safe code using mutexes and locks
* Apply RAII principles to mutex management using lock guards
* Coordinate thread execution using condition variables
* Design and implement multithreaded solutions to practical problems

---

## Table of Contents

1. [Introduction to Multithreading](#section-1-introduction-to-multithreading)
2. [Boost Threads Basics](#section-2-boost-threads-basics)
3. [Creating and Managing Threads](#section-3-creating-and-managing-threads)
4. [Thread Parameters and Function Objects](#section-4-thread-parameters-and-function-objects)
5. [Thread Control with boost::this_thread](#section-5-thread-control-with-boostthis_thread)
6. [Thread Synchronization with join()](#section-6-thread-synchronization-with-join)
7. [Protecting Shared Resources with Mutexes](#section-7-protecting-shared-resources-with-mutexes)
8. [RAII and Lock Guards](#section-8-raii-and-lock-guards)
9. [Condition Variables for Advanced Synchronization](#section-9-condition-variables-for-advanced-synchronization)
10. [Resources for Further Study](#resources-for-further-study)

---

## Section 1: Introduction to Multithreading

### What is Multithreading?

Multithreading is a programming technique that allows multiple threads of execution to run concurrently within a single process. Each thread represents an independent path of execution that can perform tasks in parallel with other threads.

### Key Concepts

**Thread**: A thread is the smallest unit of execution within a process. Multiple threads within the same process share the same memory space but have their own execution stack and program counter.

**Concurrency vs Parallelism**:
* **Concurrency**: Multiple threads making progress on tasks, potentially by interleaving execution on a single core
* **Parallelism**: Multiple threads executing simultaneously on multiple processor cores

### Benefits of Multithreading

1. **Improved Performance**: By utilizing multiple processor cores, multithreaded programs can execute faster than single-threaded programs for computation-intensive tasks

2. **Responsiveness**: Allows some tasks to execute independently of the main process, preventing blocking operations from freezing the entire application

3. **Resource Utilization**: Makes better use of modern multi-core processors and virtual threads

4. **Modular Design**: Separates different concerns into different threads (e.g., UI thread, worker thread, I/O thread)

### Challenges of Multithreading

While multithreading offers many benefits, it also introduces complexity:

* **Data Races**: When multiple threads access shared data simultaneously and at least one thread modifies it
* **Deadlocks**: When two or more threads are waiting for each other to release resources
* **Race Conditions**: When the outcome depends on the timing or ordering of thread execution
* **Debugging Complexity**: Multithreaded bugs can be difficult to reproduce and diagnose

### Multithreading Architecture

```
Process Memory Space
+----------------------------------+
|  Code Segment (shared)           |
|  Data Segment (shared)           |
|  Heap (shared)                   |
+----------------------------------+
|                                  |
|  Thread 1        Thread 2        |
|  +----------+    +----------+    |
|  | Stack    |    | Stack    |    |
|  | Registers|    | Registers|    |
|  | PC       |    | PC       |    |
|  +----------+    +----------+    |
|                                  |
+----------------------------------+
```

[Back to Table of Contents](#table-of-contents)

---

## Section 2: Boost Threads Basics

### What is Boost.Thread?

Boost.Thread is a comprehensive threading library that was the precursor to the C++11 standard thread library (std::thread). While the standard library has incorporated many features from Boost, the Boost version remains more feature-rich and offers additional functionality.

### Boost vs std::thread

**Similarities**:
* Both provide similar basic threading capabilities
* Both use RAII principles for resource management
* Both support similar synchronization primitives

**Boost Advantages**:
* More comprehensive feature set
* Additional timing and synchronization options (e.g., try_join_for)
* Better support for advanced use cases
* Available for C++03 and earlier compilers

**When to Use Each**:
* Use std::thread for modern C++11/14/17 projects with basic threading needs
* Use Boost.Thread when you need advanced features not yet in the standard library
* Use Boost.Thread when maintaining compatibility with older C++ standards

### Setting Up Boost.Thread

To use Boost.Thread, you need to:

1. Install the Boost libraries on your system
2. Include the appropriate header: `#include <boost/thread.hpp>`
3. Link against the Boost thread library when compiling

**Compilation Example** (using g++):
```bash
g++ -std=c++11 program.cpp -lboost_thread -lboost_system -pthread -o program
```

### Boost.Chrono

Boost.Thread works closely with Boost.Chrono, which provides timing utilities similar to std::chrono. Boost.Chrono allows you to specify time durations using different units (seconds, milliseconds, nanoseconds, etc.).

Common Boost.Chrono types:
* `boost::chrono::seconds`
* `boost::chrono::milliseconds`
* `boost::chrono::microseconds`
* `boost::chrono::nanoseconds`

[Back to Table of Contents](#table-of-contents)

---

## Section 3: Creating and Managing Threads

### Basic Thread Creation

A thread is created by constructing a `boost::thread` object and passing it a callable entity (function, function object, or lambda). The thread begins execution immediately upon construction.

**Example: Simple Thread Creation**

```cpp
#include <iostream>
#include <boost/thread.hpp>

void loadsOfAs()
{
    for(int i = 0; i < 20000000; ++i)
        std::cout << 'a';
}

int main()
{
    boost::thread myThread(loadsOfAs);  // Start new thread
    
    for(int i = 0; i < 10000000; ++i)
        std::cout << 'b';               // Do this while thread is running
    
    myThread.join();                     // Wait for thread to finish
    return 0;
}
```

**Sample Output** (may vary due to thread interleaving):
```
bbbaaabbbaaabbbaaaaabbbbaaaaaabbbbbaaa...
```

**Explanation**:
* The main thread starts executing main()
* When `myThread` is constructed, a new thread starts executing loadsOfAs()
* Both threads run concurrently, outputting 'a' and 'b' characters
* The characters are interleaved because both threads are writing to std::cout simultaneously
* myThread.join() ensures the main thread waits for myThread to complete before exiting

### Thread Lifecycle

```
Thread Creation
     |
     v
+----------+
| CREATED  |
+----------+
     |
     v
+----------+    Exception    +------------+
| RUNNING  | --------------> | TERMINATED |
+----------+                 +------------+
     |                              ^
     | Completes                    |
     +------------------------------+
```

A thread goes through several states:
1. **Created**: Thread object constructed, thread starts executing
2. **Running**: Thread is actively executing its function
3. **Terminated**: Thread function has returned or thrown an exception

### Callable Entities

You can pass any callable entity to a thread constructor:

**1. Function Pointer**:
```cpp
void myFunction() { /* ... */ }
boost::thread t(myFunction);
```

**2. Function Object (Functor)**:
```cpp
struct MyFunctor {
    void operator()() { /* ... */ }
};
MyFunctor f;
boost::thread t(f);
```

**3. Lambda Expression** (C++11 and later):
```cpp
boost::thread t([]() {
    std::cout << "Lambda thread\n";
});
```

### Thread Joinability

A thread is **joinable** if it has been constructed with a callable entity and has not yet been joined or detached.

**Important Rules**:
* You must call join() or detach() on a joinable thread before it is destroyed
* If a joinable thread object is destroyed without being joined or detached, std::terminate() is called
* After join() or detach(), the thread is no longer joinable

**Example: Checking Joinability**

```cpp
#include <iostream>
#include <boost/thread.hpp>

void simpleFunction()
{
    std::cout << "Thread executing\n";
}

int main()
{
    boost::thread t(simpleFunction);
    
    std::cout << "Thread joinable? " << t.joinable() << "\n";
    
    t.join();
    
    std::cout << "Thread joinable after join? " << t.joinable() << "\n";
    
    return 0;
}
```

**Sample Output**:
```
Thread executing
Thread joinable? 1
Thread joinable after join? 0
```

[Back to Table of Contents](#table-of-contents)

---

## Section 4: Thread Parameters and Function Objects

### Passing Parameters to Threads

There are two main approaches to passing data to threads:

1. **Pass parameters directly to the thread constructor**
2. **Use function objects with member variables**

### Approach 1: Direct Parameter Passing

You can pass arguments to thread functions by providing them after the callable entity in the thread constructor.

**Example: Passing Parameters by Value**

```cpp
#include <iostream>
#include <boost/thread.hpp>

void howManyAs(int as)
{
    for(int i = 0; i < as; ++i)
        std::cout << 'a';
}

int main()
{
    boost::thread t1(howManyAs, 100000);  // Pass 100000 as parameter
    
    for(int i = 0; i < 100000; ++i)
        std::cout << 'b';
    
    t1.join();
    return 0;
}
```

**Sample Output**:
```
bbbaaabbbaaaaabbbbaaa...
```

### Passing References with boost::ref

By default, parameters are copied when passed to threads. To pass by reference, you must use `boost::ref()` or `boost::cref()` for const references.

**Example: Passing by Reference**

```cpp
#include <iostream>
#include <vector>
#include <boost/thread.hpp>

void printVector(std::vector<char>& v)
{
    for(int i = 0; i < v.size(); ++i)
        std::cout << v[i];
}

int main()
{
    std::vector<char> letters(100000, 'b');
    
    boost::thread t2(printVector, boost::ref(letters));
    
    t2.join();
    return 0;
}
```

**Sample Output**:
```
bbbbbbbbbbbb... (100000 b's)
```

**Warning**: When passing by reference, ensure the referenced object outlives the thread. Passing references to local variables that go out of scope before the thread completes leads to undefined behavior.

### Approach 2: Function Objects

Function objects (functors) allow you to encapsulate data as member variables and behavior as operator().

**Example: Function Object with Member Variables**

```cpp
#include <iostream>
#include <boost/thread.hpp>

class FunctionObject
{
    int quantity;
    char letter;
    
public:
    FunctionObject(int n, char c) : quantity(n), letter(c) {}
    
    void operator()()
    {
        for(int i = 0; i < quantity; ++i)
            std::cout << letter;
    }
};

int main()
{
    FunctionObject f(200000, 'c');
    boost::thread t3(f);
    
    for(int i = 0; i < 100000; ++i)
        std::cout << 'd';
    
    t3.join();
    return 0;
}
```

**Sample Output**:
```
dddcccdddcccddddccccc...
```

### Complete Example: Multiple Approaches

```cpp
#include <iostream>
#include <vector>
#include <boost/thread.hpp>

// Function with parameters
void howManyAs(int as)
{
    for(int i = 0; i < as; ++i)
        std::cout << 'a';
}

// Function with reference parameter
void printVector(std::vector<char>& v)
{
    for(int i = 0; i < v.size(); ++i)
        std::cout << v[i];
}

// Function object
class FunctionObject
{
    int quantity;
    char letter;
    
public:
    FunctionObject(int n, char c) : quantity(n), letter(c) {}
    
    void operator()()
    {
        for(int i = 0; i < quantity; ++i)
            std::cout << letter;
    }
};

int main()
{
    // Approach 1: Direct parameter passing
    boost::thread t1(howManyAs, 100000);
    
    // Approach 2: Reference parameter
    std::vector<char> letters(100000, 'b');
    boost::thread t2(printVector, boost::ref(letters));
    
    // Approach 3: Function object
    FunctionObject f(200000, 'c');
    boost::thread t3(f);
    
    t1.join();
    t2.join();
    t3.join();
    
    return 0;
}
```

[Back to Table of Contents](#table-of-contents)

---

## Section 5: Thread Control with boost::this_thread

### Introduction to boost::this_thread

The `boost::this_thread` namespace provides utilities for controlling the current thread's execution. These functions can be called from within any thread to affect that thread's behavior.

### Thread Sleep

The most common use of boost::this_thread is to pause thread execution for a specified duration.

**sleep_for()**: Pauses the current thread for a specified duration

**Example: Basic Sleep**

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void ticker()
{
    for(bool tick = true; tick = !tick;)
    {
        boost::this_thread::sleep_for(boost::chrono::seconds(1));
        
        if(tick)
            std::cout << "tick" << std::endl;
        else
            std::cout << "tock" << std::endl;
    }
}

int main()
{
    boost::thread t(ticker);
    boost::this_thread::sleep_for(boost::chrono::seconds(10));
    
    return 0;
}
```

**Sample Output** (over 10 seconds):
```
tick
tock
tick
tock
tick
tock
tick
tock
tick
tock
```

### Using Different Time Units

Boost.Chrono supports multiple time units for precision timing:

**Example: Different Time Units**

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

int main()
{
    int n = 100;
    
    for(int i = n; i > 1; --i)
    {
        boost::this_thread::sleep_for(boost::chrono::milliseconds(i * 2));
        std::cout << '.';
        std::cout.flush();
    }
    
    std::cout << "\nDone!" << std::endl;
    return 0;
}
```

**Sample Output**:
```
......................
Done!
```

The dots appear with increasing speed as the sleep duration decreases.

### Yielding the Processor

**yield()**: Provides a hint to the scheduler that the current thread is willing to give up its time slice to allow other threads to run.

**Example: Using yield()**

```cpp
#include <iostream>
#include <boost/thread.hpp>

void busyWorker(int id)
{
    for(int i = 0; i < 1000000; ++i)
    {
        // Do some work
        if(i % 10000 == 0)
            boost::this_thread::yield();  // Give other threads a chance
    }
    std::cout << "Worker " << id << " finished\n";
}

int main()
{
    boost::thread t1(busyWorker, 1);
    boost::thread t2(busyWorker, 2);
    boost::thread t3(busyWorker, 3);
    
    t1.join();
    t2.join();
    t3.join();
    
    return 0;
}
```

**Sample Output**:
```
Worker 1 finished
Worker 2 finished
Worker 3 finished
```

### Getting Thread Information

**get_id()**: Returns the unique identifier of the current thread

**Example: Thread IDs**

```cpp
#include <iostream>
#include <boost/thread.hpp>

void printThreadId()
{
    std::cout << "Thread ID: " << boost::this_thread::get_id() << std::endl;
}

int main()
{
    std::cout << "Main thread ID: " << boost::this_thread::get_id() << std::endl;
    
    boost::thread t1(printThreadId);
    boost::thread t2(printThreadId);
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Sample Output**:
```
Main thread ID: 7f8a4b2c3000
Thread ID: 7f8a4b2c3700
Thread ID: 7f8a4b2c3e00
```

### Practical Example: Coordinated Timing

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void worker(int id, int delaySeconds)
{
    std::cout << "Worker " << id << " starting...\n";
    
    boost::this_thread::sleep_for(boost::chrono::seconds(delaySeconds));
    
    std::cout << "Worker " << id << " finished after " 
              << delaySeconds << " seconds\n";
}

int main()
{
    boost::thread t1(worker, 1, 2);
    boost::thread t2(worker, 2, 3);
    boost::thread t3(worker, 3, 1);
    
    std::cout << "All workers started\n";
    
    t1.join();
    t2.join();
    t3.join();
    
    std::cout << "All workers completed\n";
    
    return 0;
}
```

**Sample Output**:
```
Worker 1 starting...
Worker 2 starting...
Worker 3 starting...
All workers started
Worker 3 finished after 1 seconds
Worker 1 finished after 2 seconds
Worker 2 finished after 3 seconds
All workers completed
```

[Back to Table of Contents](#table-of-contents)

---

## Section 6: Thread Synchronization with join()

### Understanding join()

The `join()` method is a blocking operation that waits for a thread to complete its execution. The calling thread will pause until the target thread terminates.

**Thread Synchronization Diagram**:
```
Main Thread:        create t1 -----> join(t1) -----> continue
                                      |
                                      | (waits)
                                      |
Worker Thread t1:   start -----> execute -----> finish
```

### Basic join() Usage

**Example: Simple join()**

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void ticker()
{
    for(bool tick = true; tick = !tick;)
    {
        boost::this_thread::sleep_for(boost::chrono::seconds(1));
        
        if(tick)
            std::cout << "tick" << std::endl;
        else
            std::cout << "tock" << std::endl;
    }
}

int main()
{
    boost::thread t(ticker);
    
    std::cout << "Thread started\n";
    
    // Wait for thread to complete (never in this case - infinite loop)
    // t.join();  // Would wait forever
    
    // Instead, we'll wait a specific time
    boost::this_thread::sleep_for(boost::chrono::seconds(10));
    
    std::cout << "Main thread exiting\n";
    return 0;
}
```

### try_join_for() - Timed Join

One of the key advantages of Boost.Thread over std::thread is `try_join_for()`, which attempts to join a thread but gives up after a specified timeout.

**Return Value**:
* `true` if the thread completed within the timeout
* `false` if the timeout expired before the thread completed

**Example: try_join_for()**

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void ticker()
{
    for(bool tick = true; tick = !tick;)
    {
        boost::this_thread::sleep_for(boost::chrono::seconds(1));
        
        if(tick)
            std::cout << "tick" << std::endl;
        else
            std::cout << "tock" << std::endl;
    }
}

int main()
{
    boost::thread t(ticker);
    
    // Try to join for 10 seconds
    bool completed = t.try_join_for(boost::chrono::seconds(10));
    
    if(completed)
        std::cout << "Thread completed successfully\n";
    else
        std::cout << "Thread still running after timeout\n";
    
    return 0;
}
```

**Sample Output**:
```
tick
tock
tick
tock
tick
tock
tick
tock
tick
tock
Thread still running after timeout
```

### Practical Example: Worker Threads with Timeout

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void longTask(int duration)
{
    std::cout << "Long task started (" << duration << " seconds)\n";
    boost::this_thread::sleep_for(boost::chrono::seconds(duration));
    std::cout << "Long task completed\n";
}

int main()
{
    boost::thread worker(longTask, 8);
    
    std::cout << "Waiting for worker (max 5 seconds)...\n";
    
    bool finished = worker.try_join_for(boost::chrono::seconds(5));
    
    if(finished)
    {
        std::cout << "Worker completed on time\n";
    }
    else
    {
        std::cout << "Worker taking too long, continuing anyway\n";
        std::cout << "Waiting a bit longer...\n";
        
        worker.join();  // Wait indefinitely for completion
        std::cout << "Worker finally completed\n";
    }
    
    return 0;
}
```

**Sample Output**:
```
Long task started (8 seconds)
Waiting for worker (max 5 seconds)...
Worker taking too long, continuing anyway
Waiting a bit longer...
Long task completed
Worker finally completed
```

### Join Patterns

**Pattern 1: Join All Threads**
```cpp
boost::thread t1(func1);
boost::thread t2(func2);
boost::thread t3(func3);

t1.join();
t2.join();
t3.join();
```

**Pattern 2: Timeout with Fallback**
```cpp
boost::thread worker(task);

if(!worker.try_join_for(boost::chrono::seconds(5)))
{
    // Timeout handling
    std::cout << "Task taking too long\n";
    worker.join();  // Wait anyway
}
```

**Pattern 3: Thread Group** (Advanced)
```cpp
boost::thread_group workers;
workers.create_thread(task1);
workers.create_thread(task2);
workers.create_thread(task3);
workers.join_all();  // Join all threads in group
```

### Important Considerations

1. **Always join or detach**: Every thread must be either joined or detached before destruction
2. **Join from one thread only**: Multiple threads should not join the same thread
3. **Deadlock prevention**: Be careful not to create circular dependencies in joins
4. **Exception safety**: Use RAII patterns to ensure threads are joined even if exceptions occur

[Back to Table of Contents](#table-of-contents)

---

## Section 7: Protecting Shared Resources with Mutexes

### The Problem: Data Races

When multiple threads access shared data and at least one thread modifies it, a **data race** occurs. Data races lead to undefined behavior and are one of the most challenging bugs in multithreaded programming.

**Example of a Data Race**:
```
Thread 1: Read balance (100)  -->  Add 50  -->  Write balance (150)
Thread 2:     Read balance (100)  -->  Add 30  -->  Write balance (130)

Result: Balance is 130, but should be 180!
```

### Introduction to Mutexes

A **mutex** (MUTual EXclusion) is a synchronization primitive that ensures only one thread can access a protected resource at a time.

**Key Operations**:
* `lock()`: Acquire the mutex (blocks if already locked)
* `unlock()`: Release the mutex
* `try_lock()`: Attempt to acquire without blocking

**Mutex State Diagram**:
```
     +----------+
     | UNLOCKED |
     +----------+
         ^  |
  unlock |  | lock
         |  v
     +----------+
     | LOCKED   |
     +----------+
```

### Critical Section

A **critical section** is a portion of code that accesses shared resources and must not be executed by more than one thread at a time.

```cpp
mutex.lock();
// Critical section begins
// ... access shared resource ...
// Critical section ends
mutex.unlock();
```

### Example: Data Race Problem

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void printSome(int a, char b)
{
    for(int i = 0; i < a; ++i)
    {
        boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
        std::cout << b << std::flush;
    }
}

int main()
{
    boost::thread t1(printSome, 50, 'a');
    boost::thread t2(printSome, 50, 'b');
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Sample Output** (interleaved, inconsistent):
```
abbababaababbbaaabbbaaabbbaaa...
```

The output is garbled because both threads access std::cout simultaneously.

### Solution: Using Mutexes

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void printSome(int a, char b, boost::mutex& mtx)
{
    mtx.lock();  // Acquire mutex
    
    for(int i = 0; i < a; ++i)
    {
        boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
        std::cout << b << std::flush;
    }
    
    mtx.unlock();  // Release mutex
}

int main()
{
    boost::mutex mtx;
    
    boost::thread t1(printSome, 50, 'a', boost::ref(mtx));
    boost::thread t2(printSome, 50, 'b', boost::ref(mtx));
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Sample Output** (consistent, one thread at a time):
```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
```

Now one thread completely finishes before the other begins.

### Finer-Grained Locking

The previous example locks the entire loop. We can achieve better concurrency by locking only the critical section (std::cout access):

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void printSome(int a, char b, boost::mutex& mtx)
{
    for(int i = 0; i < a; ++i)
    {
        mtx.lock();  // Lock only around cout
        boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
        std::cout << b << std::flush;
        mtx.unlock();
    }
}

int main()
{
    boost::mutex mtx;
    
    boost::thread t1(printSome, 50, 'a', boost::ref(mtx));
    boost::thread t2(printSome, 50, 'b', boost::ref(mtx));
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Sample Output** (interleaved but consistent):
```
ababababababababababababababababababababababababab...
```

Now threads alternate, but each character is printed atomically.

### Mutex Types

Boost provides several mutex types:

1. **boost::mutex**: Standard mutex, non-recursive
2. **boost::recursive_mutex**: Can be locked multiple times by the same thread
3. **boost::timed_mutex**: Supports try_lock_for() and try_lock_until()
4. **boost::shared_mutex**: Allows multiple readers or single writer

### Common Pitfalls

**Deadlock Example**:
```cpp
// Thread 1
mutex1.lock();
mutex2.lock();  // Waits for Thread 2

// Thread 2
mutex2.lock();
mutex1.lock();  // Waits for Thread 1
// DEADLOCK!
```

**Solution**: Always acquire mutexes in the same order.

**Forgotten Unlock**:
```cpp
mutex.lock();
if(error)
    return;  // FORGOT TO UNLOCK!
// ...
mutex.unlock();
```

**Solution**: Use lock guards (covered in next section).

[Back to Table of Contents](#table-of-contents)

---

## Section 8: RAII and Lock Guards

### The Problem with Manual Locking

Manual lock/unlock pairs are error-prone:

1. **Forgetting to unlock**: Leads to deadlocks
2. **Early returns**: May skip unlock
3. **Exceptions**: May bypass unlock
4. **Complex control flow**: Difficult to track all unlock paths

### RAII: Resource Acquisition Is Initialization

RAII is a C++ programming technique where resource management is tied to object lifetime:

* **Acquisition**: Resource acquired in constructor
* **Release**: Resource released in destructor
* **Guarantee**: Destructor always called when object goes out of scope

### Lock Guards Implementation Concept

A simplified lock guard implementation:

```cpp
class SimpleLockGuard
{
    boost::mutex* mtx;
    
public:
    SimpleLockGuard(boost::mutex& m) : mtx(&m)
    {
        mtx->lock();  // Acquire in constructor
    }
    
    ~SimpleLockGuard()
    {
        mtx->unlock();  // Release in destructor
    }
};
```

**Usage**:
```cpp
{
    SimpleLockGuard lock(myMutex);  // Locks mutex
    // Critical section
} // Automatically unlocks when lock goes out of scope
```

### boost::unique_lock

Boost provides `boost::unique_lock` which is a more sophisticated lock guard with additional features:

**Features**:
* RAII-based automatic unlocking
* Manual lock/unlock capability
* Deferred locking
* Transfer of ownership
* Condition variable compatibility

**Example: Using unique_lock**

```cpp
#include <iostream>
#include <boost/thread.hpp>
#include <boost/chrono.hpp>

void printSome(int a, char b, boost::mutex& mtx)
{
    boost::unique_lock<boost::mutex> lock(mtx);  // Automatically locks
    
    for(int i = 0; i < a; ++i)
    {
        boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
        std::cout << b << std::flush;
    }
    
    // Automatically unlocks when lock goes out of scope
}

int main()
{
    boost::mutex mtx;
    
    boost::thread t1(printSome, 50, 'a', boost::ref(mtx));
    boost::thread t2(printSome, 50, 'b', boost::ref(mtx));
    
    t1.join();
    t2.join();
    
    return 0;
}
```

**Sample Output**:
```
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
```

### Exception Safety

Lock guards ensure mutexes are unlocked even when exceptions occur:

```cpp
#include <iostream>
#include <stdexcept>
#include <boost/thread.hpp>

void riskyOperation(boost::mutex& mtx)
{
    boost::unique_lock<boost::mutex> lock(mtx);
    
    std::cout << "Performing risky operation\n";
    
    if(true)  // Simulate error condition
        throw std::runtime_error("Operation failed");
    
    std::cout << "This line never executes\n";
    
    // Mutex is automatically unlocked when exception is thrown
}

int main()
{
    boost::mutex mtx;
    
    try
    {
        riskyOperation(mtx);
    }
    catch(const std::exception& e)
    {
        std::cout << "Caught exception: " << e.what() << "\n";
        std::cout << "Mutex was properly unlocked\n";
    }
    
    // Can still use mutex
    boost::unique_lock<boost::mutex> lock(mtx);
    std::cout << "Successfully locked mutex again\n";
    
    return 0;
}
```

**Sample Output**:
```
Performing risky operation
Caught exception: Operation failed
Mutex was properly unlocked
Successfully locked mutex again
```

### Advanced unique_lock Features

**Deferred Locking**:
```cpp
boost::unique_lock<boost::mutex> lock(mtx, boost::defer_lock);
// Mutex not locked yet
lock.lock();  // Explicitly lock when needed
```

**Manual Unlock and Relock**:
```cpp
boost::unique_lock<boost::mutex> lock(mtx);
// Critical section 1

lock.unlock();  // Temporarily release
// Non-critical section

lock.lock();    // Reacquire
// Critical section 2
```

**Checking Lock Status**:
```cpp
boost::unique_lock<boost::mutex> lock(mtx);
if(lock.owns_lock())
    std::cout << "Currently locked\n";
```

### boost::lock_guard

For simpler cases where you don't need manual control, use `boost::lock_guard`:

```cpp
boost::lock_guard<boost::mutex> lock(mtx);  // Locks immediately
// Cannot be unlocked manually
// Unlocks automatically at scope exit
```

**Comparison**:

| Feature | lock_guard | unique_lock |
|---------|-----------|-------------|
| RAII | Yes | Yes |
| Manual unlock | No | Yes |
| Deferred lock | No | Yes |
| Transfer ownership | No | Yes |
| Overhead | Minimal | Slightly higher |

### Practical Example: Bank Account

```cpp
#include <iostream>
#include <boost/thread.hpp>

class BankAccount
{
    int balance;
    boost::mutex mtx;
    
public:
    BankAccount(int initial) : balance(initial) {}
    
    void deposit(int amount)
    {
        boost::lock_guard<boost::mutex> lock(mtx);
        balance += amount;
        std::cout << "Deposited " << amount 
                  << ", balance: " << balance << "\n";
    }
    
    bool withdraw(int amount)
    {
        boost::lock_guard<boost::mutex> lock(mtx);
        
        if(balance >= amount)
        {
            balance -= amount;
            std::cout << "Withdrew " << amount 
                      << ", balance: " << balance << "\n";
            return true;
        }
        
        std::cout << "Insufficient funds\n";
        return false;
    }
    
    int getBalance()
    {
        boost::lock_guard<boost::mutex> lock(mtx);
        return balance;
    }
};

void performTransactions(BankAccount& account, int id)
{
    for(int i = 0; i < 5; ++i)
    {
        account.deposit(10);
        account.withdraw(5);
    }
}

int main()
{
    BankAccount account(100);
    
    boost::thread t1(performTransactions, boost::ref(account), 1);
    boost::thread t2(performTransactions, boost::ref(account), 2);
    
    t1.join();
    t2.join();
    
    std::cout << "Final balance: " << account.getBalance() << "\n";
    
    return 0;
}
```

**Sample Output**:
```
Deposited 10, balance: 110
Withdrew 5, balance: 105
Deposited 10, balance: 115
Withdrew 5, balance: 110
...
Final balance: 150
```

[Back to Table of Contents](#table-of-contents)

---

## Section 9: Condition Variables for Advanced Synchronization

### Beyond Mutexes

While mutexes protect shared data, they don't provide a way for threads to wait for specific conditions to become true. **Condition variables** solve this problem by allowing threads to wait until notified by another thread.

### What is a Condition Variable?

A condition variable is a synchronization primitive that enables threads to wait until a particular condition is met. It works in conjunction with a mutex to provide thread-safe condition checking and waiting.

**Key Operations**:
* `wait()`: Release mutex and wait for notification
* `notify_one()`: Wake up one waiting thread
* `notify_all()`: Wake up all waiting threads

### How Condition Variables Work

```
Thread 1 (Waiter):                  Thread 2 (Notifier):
1. Lock mutex                       1. Perform work
2. Check condition                  2. Lock mutex
3. If false, wait()                 3. Modify condition
   - Unlock mutex                   4. Unlock mutex
   - Sleep                          5. notify_one/all()
   - Wait for notification
   - Relock mutex
4. Condition is true
5. Proceed with work
6. Unlock mutex
```

### Basic Condition Variable Example

```cpp
#include <iostream>
#include <boost/thread.hpp>

struct syncManager
{
    boost::mutex mtx;
    boost::condition_variable cv;
};

void printSome(int a, char b, syncManager& sync)
{
    boost::unique_lock<boost::mutex> lock(sync.mtx);
    sync.cv.wait(lock);  // Wait for notification
    
    for(int i = 0; i < a; ++i)
    {
        boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
        std::cout << b << std::flush;
    }
}

void go(syncManager& sync)
{
    std::cout << "Press enter to start: ";
    std::cin.ignore(100, '\n');
    sync.cv.notify_all();  // Wake all waiting threads
}

int main()
{
    syncManager s;
    
    boost::thread t1(printSome, 50, 'a', boost::ref(s));
    boost::thread t2(printSome, 50, 'b', boost::ref(s));
    boost::thread t3(printSome, 50, 'c', boost::ref(s));
    boost::thread t4(printSome, 50, 'd', boost::ref(s));
    
    go(s);
    
    t1.join();
    t2.join();
    t3.join();
    t4.join();
    
    return 0;
}
```

**Sample Output**:
```
Press enter to start: [user presses enter]
abcdabcdabcdabcdabcd...
```

### Coordinated Output with Mutex and Condition Variable

To prevent output overlap, we need to add a mutex for the output:

```cpp
#include <iostream>
#include <boost/thread.hpp>

struct syncManager
{
    boost::mutex mtx;
    boost::mutex outputMtx;  // Separate mutex for output
    boost::condition_variable cv;
};

void printSome(int a, char b, syncManager& sync)
{
    // Wait for signal
    {
        boost::unique_lock<boost::mutex> lock(sync.mtx);
        sync.cv.wait(lock);
    }
    
    // Print with output protection
    {
        boost::unique_lock<boost::mutex> lock(sync.outputMtx);
        for(int i = 0; i < a; ++i)
        {
            boost::this_thread::sleep_for(boost::chrono::milliseconds(50));
            std::cout << b << std::flush;
        }
    }
}

void go(syncManager& sync)
{
    std::cout << "Press enter to start: ";
    std::cin.ignore(100, '\n');
    sync.cv.notify_all();
}

int main()
{
    syncManager s;
    
    boost::thread t1(printSome, 50, 'a', boost::ref(s));
    boost::thread t2(printSome, 50, 'b', boost::ref(s));
    boost::thread t3(printSome, 50, 'c', boost::ref(s));
    boost::thread t4(printSome, 50, 'd', boost::ref(s));
    
    go(s);
    
    t1.join();
    t2.join();
    t3.join();
    t4.join();
    
    return 0;
}
```

**Sample Output**:
```
Press enter to start: [user presses enter]
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbccccccccccccccccccccccccccccccccccccccccccccccccccddddddddddddddddddddddddddddddddddddddddddddddddd
```

### Producer-Consumer Pattern

A classic use case for condition variables:

```cpp
#include <iostream>
#include <queue>
#include <boost/thread.hpp>

struct Buffer
{
    std::queue<int> data;
    boost::mutex mtx;
    boost::condition_variable cv;
    bool done;
    
    Buffer() : done(false) {}
};

void producer(Buffer& buffer, int count)
{
    for(int i = 1; i <= count; ++i)
    {
        {
            boost::lock_guard<boost::mutex> lock(buffer.mtx);
            buffer.data.push(i);
            std::cout << "Produced: " << i << "\n";
        }
        buffer.cv.notify_one();  // Notify consumer
        boost::this_thread::sleep_for(boost::chrono::milliseconds(100));
    }
    
    {
        boost::lock_guard<boost::mutex> lock(buffer.mtx);
        buffer.done = true;
    }
    buffer.cv.notify_all();  // Final notification
}

void consumer(Buffer& buffer, int id)
{
    while(true)
    {
        boost::unique_lock<boost::mutex> lock(buffer.mtx);
        
        // Wait for data or completion
        buffer.cv.wait(lock, [&buffer]() {
            return !buffer.data.empty() || buffer.done;
        });
        
        if(buffer.data.empty() && buffer.done)
            break;
        
        if(!buffer.data.empty())
        {
            int value = buffer.data.front();
            buffer.data.pop();
            lock.unlock();
            
            std::cout << "Consumer " << id << " consumed: " << value << "\n";
            boost::this_thread::sleep_for(boost::chrono::milliseconds(150));
        }
    }
    
    std::cout << "Consumer " << id << " finished\n";
}

int main()
{
    Buffer buffer;
    
    boost::thread prod(producer, boost::ref(buffer), 10);
    boost::thread cons1(consumer, boost::ref(buffer), 1);
    boost::thread cons2(consumer, boost::ref(buffer), 2);
    
    prod.join();
    cons1.join();
    cons2.join();
    
    return 0;
}
```

**Sample Output**:
```
Produced: 1
Consumer 1 consumed: 1
Produced: 2
Consumer 2 consumed: 2
Produced: 3
Consumer 1 consumed: 3
...
Consumer 1 finished
Consumer 2 finished
```

### Condition Variable with Predicate

Using a predicate (lambda or function) prevents spurious wakeups:

```cpp
// Instead of:
cv.wait(lock);

// Use:
cv.wait(lock, []() { return condition_is_true; });
```

The predicate version is equivalent to:
```cpp
while(!condition_is_true)
    cv.wait(lock);
```

### notify_one() vs notify_all()

**notify_one()**:
* Wakes up one waiting thread
* More efficient when only one thread needs to wake
* Use when threads are interchangeable

**notify_all()**:
* Wakes up all waiting threads
* Necessary when condition might be relevant to multiple threads
* Use when threads have different roles or conditions

### Complete Example: Task Queue

```cpp
#include <iostream>
#include <queue>
#include <string>
#include <boost/thread.hpp>

class TaskQueue
{
    std::queue<std::string> tasks;
    boost::mutex mtx;
    boost::condition_variable cv;
    bool shutdown;
    
public:
    TaskQueue() : shutdown(false) {}
    
    void addTask(const std::string& task)
    {
        {
            boost::lock_guard<boost::mutex> lock(mtx);
            tasks.push(task);
            std::cout << "Added task: " << task << "\n";
        }
        cv.notify_one();
    }
    
    void worker(int id)
    {
        while(true)
        {
            std::string task;
            
            {
                boost::unique_lock<boost::mutex> lock(mtx);
                cv.wait(lock, [this]() {
                    return !tasks.empty() || shutdown;
                });
                
                if(tasks.empty() && shutdown)
                    break;
                
                task = tasks.front();
                tasks.pop();
            }
            
            std::cout << "Worker " << id << " processing: " << task << "\n";
            boost::this_thread::sleep_for(boost::chrono::seconds(1));
            std::cout << "Worker " << id << " completed: " << task << "\n";
        }
        
        std::cout << "Worker " << id << " shutting down\n";
    }
    
    void stop()
    {
        {
            boost::lock_guard<boost::mutex> lock(mtx);
            shutdown = true;
        }
        cv.notify_all();
    }
};

int main()
{
    TaskQueue queue;
    
    boost::thread w1(&TaskQueue::worker, &queue, 1);
    boost::thread w2(&TaskQueue::worker, &queue, 2);
    
    queue.addTask("Task A");
    queue.addTask("Task B");
    queue.addTask("Task C");
    queue.addTask("Task D");
    queue.addTask("Task E");
    
    boost::this_thread::sleep_for(boost::chrono::seconds(6));
    
    queue.stop();
    
    w1.join();
    w2.join();
    
    return 0;
}
```

**Sample Output**:
```
Added task: Task A
Added task: Task B
Worker 1 processing: Task A
Added task: Task C
Worker 2 processing: Task B
Added task: Task D
Added task: Task E
Worker 1 completed: Task A
Worker 1 processing: Task C
Worker 2 completed: Task B
Worker 2 processing: Task D
Worker 1 completed: Task C
Worker 1 processing: Task E
Worker 2 completed: Task D
Worker 1 completed: Task E
Worker 2 shutting down
Worker 1 shutting down
```

[Back to Table of Contents](#table-of-contents)

---

## Resources for Further Study

### Official Documentation

* **Boost Thread Library Documentation**
  - https://www.boost.org/doc/libs/release/doc/html/thread.html
  - Comprehensive reference for all Boost.Thread features

* **Boost Chrono Library Documentation**
  - https://www.boost.org/doc/libs/release/doc/html/chrono.html
  - Timing and duration utilities

* **C++ Reference - std::thread**
  - https://en.cppreference.com/w/cpp/thread/thread
  - Standard library threading (similar to Boost)

### Online Tutorials and Guides

* **Boost Thread Tutorial**
  - https://theboostcpplibraries.com/boost.thread
  - Practical examples and explanations

* **Modern C++ Concurrency**
  - https://www.modernescpp.com/index.php/category/multithreading
  - Articles on threading in modern C++

* **Concurrency in C++**
  - https://www.educative.io/courses/concurrency-in-cpp
  - Interactive learning platform

### Books

* **C++ Concurrency in Action** by Anthony Williams
  - Comprehensive guide to multithreading in C++
  - Covers both std::thread and Boost.Thread

* **The C++ Programming Language (4th Edition)** by Bjarne Stroustrup
  - Chapter on concurrency and threading
  - Authoritative C++ reference

* **Professional C++ (5th Edition)** by Marc Gregoire
  - Practical coverage of multithreading
  - Real-world examples and best practices

### Practice Platforms

* **HackerRank - C++ Challenges**
  - https://www.hackerrank.com/domains/cpp
  - Practice problems including concurrency

* **LeetCode - Concurrency Problems**
  - https://leetcode.com/problemset/concurrency/
  - Threading and synchronization challenges

* **Exercism - C++ Track**
  - https://exercism.org/tracks/cpp
  - Mentored practice with threading exercises

### Community Resources

* **Stack Overflow - boost::thread tag**
  - https://stackoverflow.com/questions/tagged/boost-thread
  - Questions and answers from the community

* **C++ Slack/Discord Communities**
  - Active discussion of threading topics
  - Real-time help from experienced developers

* **Reddit - r/cpp**
  - https://www.reddit.com/r/cpp/
  - Discussions on C++ threading and concurrency

### Advanced Topics

* **Lock-Free Programming**
  - https://www.boost.org/doc/libs/release/doc/html/atomic.html
  - Boost.Atomic for lock-free data structures

* **Thread Pools**
  - https://www.boost.org/doc/libs/release/libs/asio/doc/html/boost_asio/overview/core/threads.html
  - Boost.Asio thread pool implementation

* **Parallel Algorithms**
  - https://en.cppreference.com/w/cpp/algorithm
  - C++17 parallel execution policies

---

[Back to Table of Contents](#table-of-contents)
