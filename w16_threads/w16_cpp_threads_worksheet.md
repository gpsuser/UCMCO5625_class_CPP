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
