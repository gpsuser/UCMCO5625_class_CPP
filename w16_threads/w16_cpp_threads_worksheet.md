# C++ Worksheet

## Quiz Section

### Question 1

Complete the following sentences using the words from the word bank below. Each word should be used exactly once.

**Word Bank:** `parallelism`, `concurrency`, `interleaving`, `simultaneously`, `cores`, `single`

a) __________ refers to multiple threads making progress on tasks, potentially by __________ execution on a __________ core.

b) __________ refers to multiple threads executing __________ on multiple processor __________.

---

### Question 2

Fill in the blanks using the words from the word bank below. Each word should be used exactly once.

**Word Bank:** `lock`, `unlock`, `destructor`, `constructor`, `exception`, `scope`

The RAII (Resource Acquisition Is Initialization) principle ensures that a mutex is acquired in the __________ and released in the __________. This guarantees that the mutex is properly unlocked even if an __________ occurs or when the lock guard goes out of __________.

---

### Question 3

Complete the following statements about mutex operations using the word bank below. Each word should be used exactly once.

**Word Bank:** `blocks`, `acquire`, `release`, `locked`, `try_lock`, `returns`

The `lock()` operation attempts to __________ the mutex and __________ if the mutex is already __________. The `unlock()` operation will __________ the mutex. The `__________()` operation attempts to acquire the mutex but immediately __________ if it cannot.

---

### Question 4

Fill in the blanks using the words from the word bank below. Each word should be used exactly once.

**Word Bank:** `notify_one`, `notify_all`, `wait`, `mutex`, `relock`, `condition`

When a thread calls `__________()` on a condition variable, it releases the __________ and sleeps until notified. When the thread wakes up, it will __________ the mutex before checking the __________. The `__________()` function wakes all waiting threads, while `__________()` wakes only one.

---

### Question 5

Which of the following statements about thread joinability is **correct**?

a) A thread becomes non-joinable after calling `detach()` but remains joinable after calling `join()`

b) A thread is joinable if it has been constructed with a callable entity and has not yet been joined or detached

c) You can call `join()` multiple times on the same thread to wait for it repeatedly

d) A non-joinable thread will automatically be joined when it goes out of scope

---

### Question 6

What is the primary advantage of using `boost::unique_lock` over `boost::lock_guard`?

a) `unique_lock` has less overhead and is more efficient

b) `unique_lock` allows manual locking and unlocking, while `lock_guard` does not

c) `unique_lock` prevents deadlocks, while `lock_guard` does not

d) `unique_lock` works with condition variables, while `lock_guard` cannot be used at all

---

### Question 7

Consider the following scenario with two threads and two mutexes:

```
Thread 1:           Thread 2:
mutex1.lock()       mutex2.lock()
mutex2.lock()       mutex1.lock()
// work             // work
mutex2.unlock()     mutex1.unlock()
mutex1.unlock()     mutex2.unlock()
```

What problem does this code demonstrate?

a) Data race

b) Deadlock

c) Race condition

d) Spurious wakeup

---

## Task Section

### Task 1: Thread-Safe Counter

Create a thread-safe counter class that can be safely incremented by multiple threads. The counter should use a mutex to protect shared data.

**Requirements:**
- Implement a `Counter` class with an `increment()` method
- Use proper mutex protection
- Create 5 threads that each increment the counter 1000 times
- Print the final count (should be 5000)

**Code Scaffolding:**

```cpp
#include <iostream>
#include <boost/thread.hpp>

class Counter
{
    int count;
    boost::mutex mtx;
    
public:
    Counter() : count(0) {}
    
    void increment()
    {
        // TODO: Implement thread-safe increment
    }
    
    int getCount()
    {
        // TODO: Implement thread-safe getter
    }
};

void incrementCounter(Counter& counter, int times)
{
    // TODO: Increment the counter 'times' number of times
}

int main()
{
    // TODO: Create Counter object
    // TODO: Create 5 threads that each call incrementCounter 1000 times
    // TODO: Join all threads
    // TODO: Print final count
    
    return 0;
}
```

**Sample Output:**
```
Final count: 5000
```

---

### Task 2: Simple Barrier Synchronization

Implement a simple barrier that makes multiple threads wait until all threads have reached a synchronization point before continuing.

**Requirements:**
- Create a `Barrier` class that waits for a specified number of threads
- Use a mutex and condition variable for synchronization
- Create 4 worker threads that perform work, reach the barrier, then continue
- Each thread should print messages before and after the barrier

**Code Scaffolding:**

```cpp
#include <iostream>
#include <boost/thread.hpp>

class Barrier
{
    int count;
    int threshold;
    boost::mutex mtx;
    boost::condition_variable cv;
    
public:
    Barrier(int num_threads) : count(0), threshold(num_threads) {}
    
    void wait()
    {
        // TODO: Implement barrier wait logic
        // Increment count, wait if not all threads arrived
        // Notify all threads when threshold is reached
    }
};

void worker(int id, Barrier& barrier)
{
    std::cout << "Thread " << id << " doing initial work\n";
    boost::this_thread::sleep_for(boost::chrono::milliseconds(id * 100));
    
    std::cout << "Thread " << id << " reached barrier\n";
    // TODO: Wait at barrier
    
    std::cout << "Thread " << id << " continuing after barrier\n";
}

int main()
{
    // TODO: Create Barrier for 4 threads
    // TODO: Create 4 worker threads
    // TODO: Join all threads
    
    return 0;
}
```

**Sample Output:**
```
Thread 1 doing initial work
Thread 2 doing initial work
Thread 3 doing initial work
Thread 4 doing initial work
Thread 1 reached barrier
Thread 2 reached barrier
Thread 3 reached barrier
Thread 4 reached barrier
Thread 1 continuing after barrier
Thread 2 continuing after barrier
Thread 3 continuing after barrier
Thread 4 continuing after barrier
```

---

### Challenge Task: Multi-Producer Multi-Consumer Queue

Implement a thread-safe queue with multiple producer and consumer threads. Producers add items to the queue, and consumers remove and process them.

**Requirements:**
- Create a `ThreadSafeQueue` class with `push()` and `pop()` methods
- Use mutex and condition variable for synchronization
- Handle queue empty condition for consumers
- Implement proper shutdown mechanism
- Create 2 producer threads and 3 consumer threads
- Producers should each add 5 items
- Consumers should process items and terminate when queue is empty and producers are done

**Code Scaffolding:**

```cpp
#include <iostream>
#include <queue>
#include <boost/thread.hpp>

class ThreadSafeQueue
{
    std::queue<int> data;
    boost::mutex mtx;
    boost::condition_variable cv;
    bool finished;
    
public:
    ThreadSafeQueue() : finished(false) {}
    
    void push(int value)
    {
        // TODO: Implement thread-safe push
        // Lock mutex, push value, notify consumers
    }
    
    bool pop(int& value)
    {
        // TODO: Implement thread-safe pop
        // Wait for data or finished signal
        // Return false if finished and queue empty
        // Return true and set value if item retrieved
    }
    
    void setFinished()
    {
        // TODO: Signal that no more items will be added
    }
};

void producer(ThreadSafeQueue& queue, int id, int count)
{
    // TODO: Produce 'count' items
    // Each item should be: id * 100 + item_number
    // Sleep briefly between productions
    // Print what was produced
}

void consumer(ThreadSafeQueue& queue, int id)
{
    // TODO: Consume items until queue is finished
    // Print what was consumed
    // Sleep briefly while processing
}

int main()
{
    // TODO: Create ThreadSafeQueue
    // TODO: Create 2 producer threads (each producing 5 items)
    // TODO: Create 3 consumer threads
    // TODO: Join producer threads
    // TODO: Signal queue is finished
    // TODO: Join consumer threads
    
    return 0;
}
```

**Sample Output:**
```
Producer 1 produced: 100
Producer 2 produced: 200
Consumer 1 consumed: 100
Producer 1 produced: 101
Consumer 2 consumed: 200
Producer 2 produced: 201
Consumer 3 consumed: 101
Producer 1 produced: 102
...
Consumer 1 finished
Consumer 2 finished
Consumer 3 finished
```
---

## Standard Thread Library Section

This section focuses on the C++ Standard Library threading facilities (`std::thread`, `std::mutex`, `std::condition_variable`, etc.) introduced in C++11.

---

### Worked Example 1: Basic Thread Creation and Joining

This example demonstrates creating threads, passing parameters, and properly joining them.

```cpp
#include <iostream>
#include <thread>
#include <vector>

void printMessage(int id, const std::string& message)
{
    std::cout << "Thread " << id << ": " << message << std::endl;
}

void countToN(int id, int n)
{
    for(int i = 1; i <= n; ++i)
    {
        std::cout << "Thread " << id << " - Count: " << i << std::endl;
    }
}

int main()
{
    // Create a thread with function and parameters
    std::thread t1(printMessage, 1, "Hello from thread 1!");
    
    // Create another thread
    std::thread t2(countToN, 2, 3);
    
    // Create thread with lambda
    std::thread t3([](int id) {
        std::cout << "Thread " << id << " - Lambda thread!" << std::endl;
    }, 3);
    
    // Join all threads (wait for completion)
    t1.join();
    t2.join();
    t3.join();
    
    std::cout << "All threads completed!" << std::endl;
    
    return 0;
}
```

**Key Points:**
- Use `std::thread` constructor with function and arguments
- Lambda functions can be used directly
- Always `join()` or `detach()` threads before they go out of scope
- Destroying a joinable thread calls `std::terminate()`

---

### Worked Example 2: Thread-Safe Operations with std::mutex

This example shows how to protect shared data using `std::mutex` and `std::lock_guard`.

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

std::mutex cout_mutex;  // Protect console output
int shared_counter = 0;
std::mutex counter_mutex;

void safeIncrement(int thread_id, int iterations)
{
    for(int i = 0; i < iterations; ++i)
    {
        // Lock the counter mutex
        {
            std::lock_guard<std::mutex> lock(counter_mutex);
            ++shared_counter;
        }
        
        // Lock cout mutex for clean output
        {
            std::lock_guard<std::mutex> lock(cout_mutex);
            std::cout << "Thread " << thread_id 
                      << " - Counter: " << shared_counter << std::endl;
        }
    }
}

int main()
{
    std::vector<std::thread> threads;
    
    // Create 3 threads, each incrementing 5 times
    for(int i = 0; i < 3; ++i)
    {
        threads.push_back(std::thread(safeIncrement, i, 5));
    }
    
    // Join all threads
    for(auto& t : threads)
    {
        t.join();
    }
    
    std::cout << "Final counter value: " << shared_counter << std::endl;
    
    return 0;
}
```

**Key Points:**
- `std::lock_guard` provides RAII-based mutex management
- Scope blocks `{}` can be used to control lock lifetime
- Separate mutexes for separate resources (counter vs cout)
- Vector can store threads for easier management

---

### Worked Example 3: Condition Variables for Thread Coordination

This example demonstrates using `std::condition_variable` for thread synchronization.

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

std::queue<int> data_queue;
std::mutex queue_mutex;
std::condition_variable data_cond;
bool finished = false;

void producer(int id, int items)
{
    for(int i = 0; i < items; ++i)
    {
        int value = id * 10 + i;
        
        {
            std::lock_guard<std::mutex> lock(queue_mutex);
            data_queue.push(value);
            std::cout << "Producer " << id << " added: " << value << std::endl;
        }
        
        data_cond.notify_one();  // Wake up one consumer
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

void consumer(int id)
{
    while(true)
    {
        std::unique_lock<std::mutex> lock(queue_mutex);
        
        // Wait until data is available or production is finished
        data_cond.wait(lock, []() {
            return !data_queue.empty() || finished;
        });
        
        if(data_queue.empty() && finished)
        {
            std::cout << "Consumer " << id << " exiting" << std::endl;
            break;
        }
        
        int value = data_queue.front();
        data_queue.pop();
        lock.unlock();
        
        std::cout << "Consumer " << id << " processed: " << value << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(150));
    }
}

int main()
{
    std::thread p1(producer, 1, 3);
    std::thread p2(producer, 2, 3);
    
    std::thread c1(consumer, 1);
    std::thread c2(consumer, 2);
    
    p1.join();
    p2.join();
    
    {
        std::lock_guard<std::mutex> lock(queue_mutex);
        finished = true;
    }
    data_cond.notify_all();  // Wake all consumers
    
    c1.join();
    c2.join();
    
    std::cout << "All threads completed!" << std::endl;
    
    return 0;
}
```

**Key Points:**
- `std::unique_lock` required for condition variables (allows manual unlock)
- Predicate lambda prevents spurious wakeups
- `notify_one()` wakes one waiting thread, `notify_all()` wakes all
- Finished flag signals consumers to exit

---

### Task 1: Thread-Safe Accumulator

Implement a thread-safe accumulator that multiple threads can add values to simultaneously. Calculate the sum of numbers from 1 to 1000 using 4 threads.

**Requirements:**
- Create an `Accumulator` class with an `add()` method
- Use `std::mutex` and `std::lock_guard` for thread safety
- Create 4 threads, each adding a portion of numbers 1-1000
- Thread 1: adds 1-250, Thread 2: adds 251-500, Thread 3: adds 501-750, Thread 4: adds 751-1000
- Print the final sum (should be 500,500)

**Code Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

class Accumulator
{
    int sum;
    std::mutex mtx;
    
public:
    Accumulator() : sum(0) {}
    
    void add(int value)
    {
        // TODO: Implement thread-safe add
    }
    
    int getSum()
    {
        // TODO: Implement thread-safe getter
    }
};

void addRange(Accumulator& acc, int start, int end)
{
    // TODO: Add all values from start to end (inclusive)
}

int main()
{
    // TODO: Create Accumulator object
    // TODO: Create 4 threads for ranges: 1-250, 251-500, 501-750, 751-1000
    // TODO: Join all threads
    // TODO: Print final sum
    
    return 0;
}
```

**Expected Output:**
```
Final sum: 500500
```

---

### Task 2: Reader-Writer Counter

Implement a counter that allows multiple readers to access the count simultaneously, but writers must have exclusive access. Use `std::shared_mutex` (C++17) or implement with regular mutexes.

**Requirements:**
- Create a `RWCounter` class with `increment()` (write) and `read()` methods
- Allow multiple readers simultaneously
- Writers need exclusive access
- Create 2 writer threads (each increments 100 times)
- Create 5 reader threads (each reads 50 times)
- Readers should print the count they observe
- Use `std::this_thread::sleep_for()` to simulate work

**Code Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <shared_mutex>
#include <vector>
#include <chrono>

class RWCounter
{
    int count;
    std::shared_mutex mtx;  // C++17
    
public:
    RWCounter() : count(0) {}
    
    void increment()
    {
        // TODO: Implement exclusive write access
    }
    
    int read()
    {
        // TODO: Implement shared read access
    }
};

void writer(RWCounter& counter, int id, int times)
{
    // TODO: Increment counter 'times' times
    // Sleep briefly between increments
    // Print occasional status
}

void reader(RWCounter& counter, int id, int times)
{
    // TODO: Read counter 'times' times
    // Print the value read
    // Sleep briefly between reads
}

int main()
{
    // TODO: Create RWCounter object
    // TODO: Create 2 writer threads (each incrementing 100 times)
    // TODO: Create 5 reader threads (each reading 50 times)
    // TODO: Join all threads
    // TODO: Print final count
    
    return 0;
}
```

**Expected Output:**
```
Reader 1 read: 5
Writer 1 incremented to: 10
Reader 2 read: 10
Reader 3 read: 15
...
Final count: 200
```

---

### Challenge Task: Simple Thread Pool

Implement a basic thread pool that can execute tasks from a queue. This is a practical pattern used in many applications.

**Requirements:**
- Create a `ThreadPool` class that manages worker threads
- Workers should fetch and execute tasks from a queue
- Use `std::function<void()>` to store tasks
- Implement `enqueue()` to add tasks to the pool
- Implement proper shutdown mechanism
- Create a pool with 3 worker threads
- Submit 10 tasks that print a message and simulate work
- Wait for all tasks to complete before shutting down

**Code Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <functional>
#include <vector>
#include <chrono>

class ThreadPool
{
    std::vector<std::thread> workers;
    std::queue<std::function<void()>> tasks;
    
    std::mutex queue_mutex;
    std::condition_variable condition;
    bool stop;
    
public:
    ThreadPool(size_t num_threads) : stop(false)
    {
        // TODO: Create worker threads
        // Each worker should loop, waiting for tasks
        // When task available, execute it
        // Exit when stop is true and queue is empty
    }
    
    void enqueue(std::function<void()> task)
    {
        // TODO: Add task to queue
        // Notify one worker
    }
    
    ~ThreadPool()
    {
        // TODO: Set stop flag
        // TODO: Notify all workers
        // TODO: Join all worker threads
    }
    
private:
    void workerThread()
    {
        // TODO: Implement worker loop
        // Wait for tasks or stop signal
        // Execute tasks from queue
    }
};

int main()
{
    // TODO: Create ThreadPool with 3 threads
    
    // TODO: Enqueue 10 tasks
    // Each task should:
    // - Print "Task X starting"
    // - Sleep for a short time
    // - Print "Task X completed"
    
    // TODO: Sleep to allow tasks to complete
    // ThreadPool destructor will be called and handle cleanup
    
    std::cout << "All tasks submitted!" << std::endl;
    
    // Wait a bit before destruction
    std::this_thread::sleep_for(std::chrono::seconds(3));
    
    return 0;
}
```

**Expected Output:**
```
Task 1 starting
Task 2 starting
Task 3 starting
Task 1 completed
Task 4 starting
Task 2 completed
Task 5 starting
...
Task 10 completed
All tasks submitted!
```