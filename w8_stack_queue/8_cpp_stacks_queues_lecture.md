# Stacks and Queues in C++

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Linear Data Structures](#section-1-introduction-to-linear-data-structures)
3. [Section 2: Understanding Stacks (LIFO)](#section-2-understanding-stacks-lifo)
4. [Section 3: Working with STL Stack](#section-3-working-with-stl-stack)
5. [Section 4: Understanding Queues (FIFO)](#section-4-understanding-queues-fifo)
6. [Section 5: Working with STL Queue](#section-5-working-with-stl-queue)
7. [Section 6: Checking for Empty Containers](#section-6-checking-for-empty-containers)
8. [Section 7: Stack and Queue Operations with Other Containers](#section-7-stack-and-queue-operations-with-other-containers)
9. [Section 8: Practical Applications and Use Cases](#section-8-practical-applications-and-use-cases)
10. [Resources](#resources)

---

## Learning Outcomes

By the end of this lecture, you should be able to:

* Explain the fundamental differences between stacks and queues
* Understand and apply the LIFO (Last In First Out) principle for stacks
* Understand and apply the FIFO (First In First Out) principle for queues
* Use the C++ STL `stack` container and its primary methods (`push`, `pop`, `top`)
* Use the C++ STL `queue` container and its primary methods (`push`, `pop`, `front`)
* Implement programs that utilize stacks and queues to solve practical problems
* Recognize when to use stacks versus queues in algorithm design
* Use the `empty()` method to safely iterate through stack and queue contents
* Apply stack and queue operations to standard containers like vectors and lists

---

## Section 1: Introduction to Linear Data Structures

### Overview

Stacks and queues are fundamental abstract data types that impose specific restrictions on how elements can be accessed. Unlike arrays, vectors, or lists where you have random access to any element at any position, stacks and queues only allow you to interact with **one specific element** at any given time.

### Key Characteristics

**Limited Access Pattern:**
* You cannot access arbitrary elements in the middle of the structure
* You can only interact with the element at a designated position
* This restriction actually makes certain algorithms simpler and more efficient

**The Distinction:**
* **Stacks**: Access the **last** item that was inserted (most recently added)
* **Queues**: Access the **first** item that was inserted (oldest item)

### LIFO vs FIFO

These access patterns give rise to two important acronyms:

**LIFO - Last In, First Out (Stacks)**
* The most recently added element is the first one to be removed
* Think of it like a stack of plates: you add plates to the top and remove plates from the top

**FIFO - First In, First Out (Queues)**
* The oldest element is the first one to be removed
* Think of it like a line at a coffee shop: the first person in line is the first person served

### Visual Representation

```
STACK (LIFO):                    QUEUE (FIFO):

    pop/push here ↓                  ← pop from front
    +-----+                       +-----+-----+-----+-----+
    |  3  |  ← top                |  1  |  2  |  3  |  4  |
    +-----+                       +-----+-----+-----+-----+
    |  2  |                       push to back →
    +-----+
    |  1  |
    +-----+

Push 3, then 2, then 1           Push 1, then 2, then 3, then 4
Pop gives: 1, then 2, then 3     Pop gives: 1, then 2, then 3, then 4
```

### Why These Structures Matter

Stacks and queues appear frequently in computer science:
* **Stacks**: Function call management, expression evaluation, undo mechanisms, backtracking algorithms
* **Queues**: Task scheduling, breadth-first search, printer job management, message buffers

---

## Section 2: Understanding Stacks (LIFO)

### The Stack Concept

A stack is best understood through a physical analogy: imagine a tube of tennis balls. When you want to retrieve a ball, you can only access the one at the top of the tube. This is naturally the last ball you inserted.

```
Tennis Ball Tube:          Stack Operations:

    ←  Can only              push(D) ↓
       access top         +-----+
                          |  D  |  ← top() returns this
    +-------+             +-----+
    |   •   | ← Last in   |  C  |
    |   •   |             +-----+
    |   •   |             |  B  |
    |   •   | ← First in  +-----+
    +-------+             |  A  |
                          +-----+
                          pop() removes from top ↑
```

### Real-World Stack Applications

**Function Call Stack:**
* When a program calls a function, the execution context is pushed onto the call stack
* When the function completes, it's popped off the stack
* This is why the last function called must complete before the previous function can continue
* Recursion naturally creates stack-like behavior!

**Expression Evaluation:**
* Compilers use stacks to evaluate mathematical expressions
* Example: Converting infix notation (3 + 4) to postfix notation (3 4 +)

**Undo Functionality:**
* Text editors maintain a stack of previous states
* Each edit pushes a new state onto the stack
* Undo pops the most recent state

### Basic Stack Operations

The three fundamental operations on a stack are:

1. **push(item)**: Add an item to the top of the stack
2. **pop()**: Remove the item from the top of the stack
3. **top()**: View the item at the top without removing it

### Stack Behavior Example

Let's trace through a sequence of operations:

```
Operation      Stack State        Notes
---------      -----------        -----
(empty)        []                 Initial state
push(5)        [5]                5 is at top
push(10)       [5, 10]            10 is now at top
push(15)       [5, 10, 15]        15 is at top
top()          [5, 10, 15]        Returns 15, stack unchanged
pop()          [5, 10]            15 removed
top()          [5, 10]            Returns 10
push(20)       [5, 10, 20]        20 is now at top
pop()          [5, 10]            20 removed
pop()          [5]                10 removed
```

---

## Section 3: Working with STL Stack

### Including the Stack Library

The C++ Standard Template Library (STL) provides a ready-to-use stack implementation. To use it, you need to include the appropriate header:

```cpp
#include <stack>
```

The STL stack is a template class, meaning you can create stacks of any data type.

### Creating and Using Stacks

Here's a complete program demonstrating basic stack operations:

```cpp
#include <iostream>
#include <stack>

int main() {
    // Create a stack of doubles
    std::stack<double> s;
    
    // Push values onto the stack
    s.push(12.34);
    std::cout << "Pushed 12.34 onto stack\n";
    
    s.push(11.23);
    std::cout << "Pushed 11.23 onto stack\n";
    
    // View the top element
    std::cout << "Top element: " << s.top() << "\n";
    
    // Remove the top element
    s.pop();
    std::cout << "Popped top element\n";
    
    // View the new top element
    std::cout << "New top element: " << s.top() << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Pushed 12.34 onto stack
Pushed 11.23 onto stack
Top element: 11.23
Popped top element
New top element: 12.34
```

### Important Stack Methods

The STL stack provides several key methods:

* `push(value)`: Adds an element to the top of the stack
* `pop()`: Removes the top element (does NOT return the value)
* `top()`: Returns a reference to the top element without removing it
* `empty()`: Returns true if the stack is empty
* `size()`: Returns the number of elements in the stack

**Critical Note**: The `pop()` method does NOT return the removed value. You must use `top()` before calling `pop()` if you need the value.

### Comprehensive Stack Example

Here's a program that demonstrates various stack operations:

```cpp
#include <iostream>
#include <stack>
#include <string>

int main() {
    std::stack<std::string> books;
    
    // Adding books to the stack
    books.push("The C++ Programming Language");
    books.push("Effective C++");
    books.push("C++ Primer");
    
    std::cout << "Stack size: " << books.size() << "\n";
    std::cout << "Top book: " << books.top() << "\n\n";
    
    // Removing and displaying books
    std::cout << "Removing books from stack:\n";
    while (!books.empty()) {
        std::cout << "- " << books.top() << "\n";
        books.pop();
    }
    
    std::cout << "\nStack size after removing all: " << books.size() << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Stack size: 3
Top book: C++ Primer

Removing books from stack:
- C++ Primer
- Effective C++
- The C++ Programming Language

Stack size after removing all: 0
```

### Tracing Stack Operations

Let's work through a more complex example to understand stack behavior:

```cpp
#include <iostream>
#include <stack>

int main() {
    std::stack<int> st;
    
    // Push multiples of 2 from 0 to 18
    for (int i = 0; i < 10; ++i) {
        st.push(i * 2);
    }
    // Stack now contains: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
    // Top is 18
    
    st.pop();  // Remove 18
    // Stack now: [0, 2, 4, 6, 8, 10, 12, 14, 16]
    
    std::cout << st.top() << "\n";  // Outputs 16
    std::cout << st.top() << "\n";  // Outputs 16 again (top doesn't remove)
    
    st.pop();  // Remove 16
    // Stack now: [0, 2, 4, 6, 8, 10, 12, 14]
    
    st.push(11);  // Add 11
    // Stack now: [0, 2, 4, 6, 8, 10, 12, 14, 11]
    
    std::cout << st.top() << "\n";  // Outputs 11
    
    st.pop();  // Remove 11
    // Stack now: [0, 2, 4, 6, 8, 10, 12, 14]
    
    std::cout << st.top() << "\n";  // Outputs 14
    
    return 0;
}
```

**Sample Output:**
```
16
16
11
14
```

---

## Section 4: Understanding Queues (FIFO)

### The Queue Concept

A queue is conceptually simpler than a stack because it mirrors everyday experience: standing in a line. The first person who joins the queue is the first person to be served. In computing terms, the first element added is the first element removed.

```
Queue at Coffee Shop:           Queue Data Structure:

First → [Person1][Person2][Person3][Person4] ← Last
        ↓ served                         ↑ joins
        
        front() ↓                   ← push() to back
        +-----+-----+-----+-----+
        |  A  |  B  |  C  |  D  |
        +-----+-----+-----+-----+
        ↑ pop() from front
```

### Real-World Queue Applications

**Task Scheduling:**
* Operating systems use queues to manage processes waiting for CPU time
* First process to request CPU gets it first (in simple scheduling)

**Print Queue:**
* Documents sent to a printer are queued
* They print in the order they were submitted

**Message Buffers:**
* Network packets are often queued for processing
* Messages in communication systems follow FIFO order

**Breadth-First Search:**
* Graph traversal algorithms use queues to explore nodes level by level

### Basic Queue Operations

The three fundamental operations on a queue are:

1. **push(item)**: Add an item to the back (rear) of the queue
2. **pop()**: Remove the item from the front of the queue
3. **front()**: View the item at the front without removing it

### Queue Behavior Example

Let's trace through a sequence of operations:

```
Operation      Queue State           Notes
---------      -----------           -----
(empty)        []                    Initial state
push(5)        [5]                   5 is at front
push(10)       [5, 10]               5 still at front, 10 at back
push(15)       [5, 10, 15]           5 at front, 15 at back
front()        [5, 10, 15]           Returns 5, queue unchanged
pop()          [10, 15]              5 removed from front
front()        [10, 15]              Returns 10
push(20)       [10, 15, 20]          20 added to back
pop()          [15, 20]              10 removed from front
```

Notice how elements are always removed in the order they were added, maintaining the FIFO principle.

---

## Section 5: Working with STL Queue

### Including the Queue Library

Similar to stack, the STL provides a queue implementation:

```cpp
#include <queue>
```

### Creating and Using Queues

Here's a complete program demonstrating basic queue operations:

```cpp
#include <iostream>
#include <queue>

int main() {
    // Create a queue of doubles
    std::queue<double> q;
    
    // Add values to the queue
    q.push(12.34);
    std::cout << "Added 12.34 to queue\n";
    
    q.push(11.23);
    std::cout << "Added 11.23 to queue\n";
    
    // View the front element
    std::cout << "Front element: " << q.front() << "\n";
    
    // Remove the front element
    q.pop();
    std::cout << "Removed front element\n";
    
    // View the new front element
    std::cout << "New front element: " << q.front() << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Added 12.34 to queue
Added 11.23 to queue
Front element: 12.34
Removed front element
New front element: 11.23
```

### Important Queue Methods

The STL queue provides these key methods:

* `push(value)`: Adds an element to the back of the queue
* `pop()`: Removes the front element (does NOT return the value)
* `front()`: Returns a reference to the front element
* `back()`: Returns a reference to the back element
* `empty()`: Returns true if the queue is empty
* `size()`: Returns the number of elements in the queue

### Comprehensive Queue Example

Here's a program simulating a customer service queue:

```cpp
#include <iostream>
#include <queue>
#include <string>

int main() {
    std::queue<std::string> customerQueue;
    
    // Customers joining the queue
    customerQueue.push("Alice");
    customerQueue.push("Bob");
    customerQueue.push("Charlie");
    customerQueue.push("Diana");
    
    std::cout << "Customers in queue: " << customerQueue.size() << "\n";
    std::cout << "Next customer to serve: " << customerQueue.front() << "\n";
    std::cout << "Last customer in queue: " << customerQueue.back() << "\n\n";
    
    // Serving customers
    std::cout << "Serving customers:\n";
    while (!customerQueue.empty()) {
        std::cout << "Now serving: " << customerQueue.front() << "\n";
        customerQueue.pop();
    }
    
    std::cout << "\nAll customers served!\n";
    std::cout << "Customers remaining: " << customerQueue.size() << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Customers in queue: 4
Next customer to serve: Alice
Last customer in queue: Diana

Serving customers:
Now serving: Alice
Now serving: Bob
Now serving: Charlie
Now serving: Diana

All customers served!
Customers remaining: 0
```

### Tracing Queue Operations

Let's work through a complex example to understand queue behavior:

```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> q;
    
    // Push multiples of 2 from 0 to 18
    for (int i = 0; i < 10; ++i) {
        q.push(i * 2);
    }
    // Queue now contains: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
    // Front is 0, back is 18
    
    q.pop();  // Remove 0
    // Queue now: [2, 4, 6, 8, 10, 12, 14, 16, 18]
    
    std::cout << q.front() << "\n";  // Outputs 2
    std::cout << q.front() << "\n";  // Outputs 2 again
    
    q.pop();  // Remove 2
    // Queue now: [4, 6, 8, 10, 12, 14, 16, 18]
    
    q.push(11);  // Add 11 to back
    // Queue now: [4, 6, 8, 10, 12, 14, 16, 18, 11]
    
    std::cout << q.front() << "\n";  // Outputs 4
    
    q.pop();  // Remove 4
    // Queue now: [6, 8, 10, 12, 14, 16, 18, 11]
    
    std::cout << q.front() << "\n";  // Outputs 6
    
    return 0;
}
```

**Sample Output:**
```
2
2
4
6
```

---

## Section 6: Checking for Empty Containers

### The empty() Method

Both stacks and queues provide an `empty()` method that returns a boolean value:
* Returns `true` if the container has no elements
* Returns `false` if the container has at least one element

This method is crucial for safely processing all elements in a stack or queue without causing errors.

### Why empty() is Important

**Avoiding Errors:**
* Calling `top()` on an empty stack causes undefined behavior
* Calling `front()` on an empty queue causes undefined behavior
* Always check if the container is empty before accessing elements

### Processing All Stack Elements

Here's a pattern for safely processing all elements in a stack:

```cpp
#include <iostream>
#include <stack>

int main() {
    std::stack<int> st;
    
    // Add some values
    for (int i = 1; i <= 5; ++i) {
        st.push(i * 10);
    }
    
    std::cout << "Stack contents (top to bottom):\n";
    
    // Process all elements
    while (!st.empty()) {
        std::cout << st.top() << " ";
        st.pop();
    }
    
    std::cout << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Stack contents (top to bottom):
50 40 30 20 10 
```

### Processing All Queue Elements

The same pattern works for queues:

```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> q;
    
    // Add some values
    for (int i = 1; i <= 5; ++i) {
        q.push(i * 10);
    }
    
    std::cout << "Queue contents (front to back):\n";
    
    // Process all elements
    while (!q.empty()) {
        std::cout << q.front() << " ";
        q.pop();
    }
    
    std::cout << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Queue contents (front to back):
10 20 30 40 50 
```

### Comparing Output Order

Notice the difference in output between stack and queue when given the same input sequence:
* **Stack** (LIFO): Elements come out in reverse order (50, 40, 30, 20, 10)
* **Queue** (FIFO): Elements come out in original order (10, 20, 30, 40, 50)

### Complete Example with Size and Empty Checks

```cpp
#include <iostream>
#include <stack>
#include <string>

int main() {
    std::stack<std::string> tasks;
    
    tasks.push("Write code");
    tasks.push("Test code");
    tasks.push("Debug code");
    tasks.push("Deploy code");
    
    std::cout << "Total tasks: " << tasks.size() << "\n";
    std::cout << "Is stack empty? " << (tasks.empty() ? "Yes" : "No") << "\n\n";
    
    std::cout << "Processing tasks:\n";
    int count = 1;
    while (!tasks.empty()) {
        std::cout << count << ". " << tasks.top() << "\n";
        tasks.pop();
        count++;
    }
    
    std::cout << "\nTotal tasks: " << tasks.size() << "\n";
    std::cout << "Is stack empty? " << (tasks.empty() ? "Yes" : "No") << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Total tasks: 4
Is stack empty? No

Processing tasks:
1. Deploy code
2. Debug code
3. Test code
4. Write code

Total tasks: 0
Is stack empty? Yes
```

---

## Section 7: Stack and Queue Operations with Other Containers

### Stack-Like Behavior in Vectors

Vectors in C++ support operations that allow them to be used like stacks. This can be useful when you need both random access and stack operations:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v;
    
    // Initial values
    v.push_back(10);  // Like stack push
    v.push_back(20);
    v.push_back(30);
    
    std::cout << "Vector top element: " << v.back() << "\n";  // Like stack top
    
    v.pop_back();  // Like stack pop
    
    std::cout << "After pop, top element: " << v.back() << "\n";
    
    // Vector also allows random access (unlike a true stack)
    std::cout << "First element (not possible with stack): " << v[0] << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Vector top element: 30
After pop, top element: 20
First element (not possible with stack): 10
```

### Stack and Queue Operations with Lists

Lists provide both stack and queue operations, making them versatile:

```cpp
#include <iostream>
#include <list>

int main() {
    std::list<int> myList;
    
    std::cout << "=== Using list as a STACK ===\n";
    // Stack operations (using back)
    myList.push_back(1);   // Push onto stack
    myList.push_back(2);
    myList.push_back(3);
    
    std::cout << "Top element: " << myList.back() << "\n";
    myList.pop_back();  // Pop from stack
    std::cout << "After pop, top: " << myList.back() << "\n\n";
    
    std::cout << "=== Using list as a QUEUE ===\n";
    // Queue operations (push to back, pop from front)
    myList.clear();  // Start fresh
    myList.push_back(10);  // Enqueue
    myList.push_back(20);
    myList.push_back(30);
    
    std::cout << "Front element: " << myList.front() << "\n";
    myList.pop_front();  // Dequeue
    std::cout << "After dequeue, front: " << myList.front() << "\n";
    
    return 0;
}
```

**Sample Output:**
```
=== Using list as a STACK ===
Top element: 3
After pop, top: 2

=== Using list as a QUEUE ===
Front element: 10
After dequeue, front: 20
```

### Method Comparison Table

Here's a comparison of equivalent operations across different containers:

| Operation | Stack | Queue | Vector | List (Stack) | List (Queue) |
|-----------|-------|-------|--------|--------------|--------------|
| Add element | `push()` | `push()` | `push_back()` | `push_back()` | `push_back()` |
| Remove element | `pop()` | `pop()` | `pop_back()` | `pop_back()` | `pop_front()` |
| Access element | `top()` | `front()` | `back()` | `back()` | `front()` |
| Check if empty | `empty()` | `empty()` | `empty()` | `empty()` | `empty()` |
| Get size | `size()` | `size()` | `size()` | `size()` | `size()` |

### When to Use Each Container

**Use std::stack when:**
* You only need LIFO access
* You want to clearly express stack semantics in your code
* You don't need random access to elements

**Use std::queue when:**
* You only need FIFO access
* You want to clearly express queue semantics
* You're implementing scheduling or buffering algorithms

**Use std::vector with stack operations when:**
* You need occasional random access in addition to stack operations
* You want contiguous memory storage
* You need to iterate through elements without removing them

**Use std::list when:**
* You need both stack and queue operations on the same container
* You need efficient insertion/deletion at both ends
* You want to implement a deque-like structure

### Complete Demonstration Program

```cpp
#include <iostream>
#include <stack>
#include <queue>
#include <vector>
#include <list>

int main() {
    // Same data in different containers
    std::stack<int> s;
    std::queue<int> q;
    std::vector<int> v;
    std::list<int> l;
    
    // Add values 1-5 to each
    for (int i = 1; i <= 5; ++i) {
        s.push(i);
        q.push(i);
        v.push_back(i);
        l.push_back(i);
    }
    
    std::cout << "Stack processes (LIFO): ";
    while (!s.empty()) {
        std::cout << s.top() << " ";
        s.pop();
    }
    std::cout << "\n";
    
    std::cout << "Queue processes (FIFO): ";
    while (!q.empty()) {
        std::cout << q.front() << " ";
        q.pop();
    }
    std::cout << "\n";
    
    std::cout << "Vector as stack: ";
    while (!v.empty()) {
        std::cout << v.back() << " ";
        v.pop_back();
    }
    std::cout << "\n";
    
    std::cout << "List as queue: ";
    while (!l.empty()) {
        std::cout << l.front() << " ";
        l.pop_front();
    }
    std::cout << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Stack processes (LIFO): 5 4 3 2 1 
Queue processes (FIFO): 1 2 3 4 5 
Vector as stack: 5 4 3 2 1 
List as queue: 1 2 3 4 5 
```

---

## Section 8: Practical Applications and Use Cases

### Application 1: Reversing Input

Stacks are perfect for reversing sequences because of their LIFO nature:

```cpp
#include <iostream>
#include <stack>
#include <string>

int main() {
    std::stack<char> charStack;
    std::string input = "Hello";
    
    // Push each character onto stack
    std::cout << "Original string: " << input << "\n";
    for (char c : input) {
        charStack.push(c);
    }
    
    // Pop to get reversed string
    std::cout << "Reversed string: ";
    while (!charStack.empty()) {
        std::cout << charStack.top();
        charStack.pop();
    }
    std::cout << "\n";
    
    return 0;
}
```

**Sample Output:**
```
Original string: Hello
Reversed string: olleH
```

### Application 2: Balanced Parentheses Checker

Stacks are essential for checking balanced brackets in expressions:

```cpp
#include <iostream>
#include <stack>
#include <string>

bool isBalanced(const std::string& expr) {
    std::stack<char> s;
    
    for (char c : expr) {
        if (c == '(' || c == '[' || c == '{') {
            s.push(c);
        } else if (c == ')' || c == ']' || c == '}') {
            if (s.empty()) {
                return false;
            }
            char top = s.top();
            s.pop();
            
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {
                return false;
            }
        }
    }
    
    return s.empty();
}

int main() {
    std::string test1 = "{[()]}";
    std::string test2 = "{[(])}";
    std::string test3 = "((()))";
    std::string test4 = "((())";
    
    std::cout << test1 << " is " << (isBalanced(test1) ? "balanced" : "not balanced") << "\n";
    std::cout << test2 << " is " << (isBalanced(test2) ? "balanced" : "not balanced") << "\n";
    std::cout << test3 << " is " << (isBalanced(test3) ? "balanced" : "not balanced") << "\n";
    std::cout << test4 << " is " << (isBalanced(test4) ? "balanced" : "not balanced") << "\n";
    
    return 0;
}
```

**Sample Output:**
```
{[()]} is balanced
{[(])} is not balanced
((())) is balanced
((()) is not balanced
```

### Application 3: Task Scheduling with Queues

Queues naturally model task scheduling systems:

```cpp
#include <iostream>
#include <queue>
#include <string>

struct Task {
    std::string name;
    int priority;
    
    Task(std::string n, int p) : name(n), priority(p) {}
};

int main() {
    std::queue<Task> taskQueue;
    
    // Add tasks to queue
    taskQueue.push(Task("Compile code", 1));
    taskQueue.push(Task("Run tests", 2));
    taskQueue.push(Task("Generate docs", 3));
    taskQueue.push(Task("Deploy", 4));
    
    std::cout << "Processing tasks in FIFO order:\n";
    int taskNum = 1;
    
    while (!taskQueue.empty()) {
        Task current = taskQueue.front();
        std::cout << "Task " << taskNum << ": " << current.name 
                  << " (Priority: " << current.priority << ")\n";
        taskQueue.pop();
        taskNum++;
    }
    
    return 0;
}
```

**Sample Output:**
```
Processing tasks in FIFO order:
Task 1: Compile code (Priority: 1)
Task 2: Run tests (Priority: 2)
Task 3: Generate docs (Priority: 3)
Task 4: Deploy (Priority: 4)
```

### Application 4: Undo Mechanism

Stacks are ideal for implementing undo functionality:

```cpp
#include <iostream>
#include <stack>
#include <string>

class TextEditor {
private:
    std::string text;
    std::stack<std::string> history;
    
public:
    void write(const std::string& newText) {
        history.push(text);  // Save current state
        text += newText;
    }
    
    void undo() {
        if (!history.empty()) {
            text = history.top();
            history.pop();
        } else {
            std::cout << "Nothing to undo!\n";
        }
    }
    
    void display() {
        std::cout << "Current text: \"" << text << "\"\n";
    }
};

int main() {
    TextEditor editor;
    
    editor.write("Hello");
    editor.display();
    
    editor.write(" World");
    editor.display();
    
    editor.write("!");
    editor.display();
    
    std::cout << "\nUndoing last change...\n";
    editor.undo();
    editor.display();
    
    std::cout << "\nUndoing again...\n";
    editor.undo();
    editor.display();
    
    return 0;
}
```

**Sample Output:**
```
Current text: "Hello"
Current text: "Hello World"
Current text: "Hello World!"

Undoing last change...
Current text: "Hello World"

Undoing again...
Current text: "Hello"
```

### Application 5: Level-Order Tree Traversal (Conceptual)

Queues are fundamental to breadth-first algorithms. Here's a simplified demonstration:

```cpp
#include <iostream>
#include <queue>
#include <vector>

void breadthFirstPrint(const std::vector<int>& items) {
    std::queue<int> q;
    
    // Simulate adding nodes level by level
    for (int item : items) {
        q.push(item);
    }
    
    std::cout << "Processing in breadth-first (level) order:\n";
    while (!q.empty()) {
        std::cout << q.front() << " ";
        q.pop();
    }
    std::cout << "\n";
}

int main() {
    // Simulating tree levels: [root] [level1] [level2]
    std::vector<int> treeByLevel = {1, 2, 3, 4, 5, 6, 7};
    breadthFirstPrint(treeByLevel);
    
    return 0;
}
```

**Sample Output:**
```
Processing in breadth-first (level) order:
1 2 3 4 5 6 7 
```

### When to Choose Stack vs Queue

**Choose Stack when you need:**
* Most recent item first (LIFO)
* Function call management
* Expression evaluation
* Backtracking algorithms
* Undo/redo functionality
* Depth-first traversal

**Choose Queue when you need:**
* Oldest item first (FIFO)
* Task scheduling
* Message buffering
* Breadth-first traversal
* Fair resource allocation
* Print job management

---

## Resources

### Official Documentation
* **C++ Reference - Stack**: https://en.cppreference.com/w/cpp/container/stack
* **C++ Reference - Queue**: https://en.cppreference.com/w/cpp/container/queue
* **C++ Standard Library Documentation**: https://en.cppreference.com/w/cpp/container

### Textbooks and Reading Materials
* Stroustrup, B. "The C++ Programming Language" (4th Edition) - Chapters on STL containers
* Lippman, S., Lajoie, J., and Moo, B. "C++ Primer" (5th Edition) - Sequential container chapter
* Sedgewick, R. "Algorithms in C++" - Sections on fundamental data structures

### Online Practice Platforms
* **LeetCode**: https://leetcode.com/ - Search for "stack" and "queue" problems
* **HackerRank**: https://www.hackerrank.com/domains/data-structures - Stack and queue challenges
* **Codewars**: https://www.codewars.com/ - Filter by data structures kata

### Additional Learning Resources
* **GeeksforGeeks Stack Articles**: https://www.geeksforgeeks.org/stack-data-structure/
* **GeeksforGeeks Queue Articles**: https://www.geeksforgeeks.org/queue-data-structure/
* **Stack Overflow**: https://stackoverflow.com/ - Search for specific implementation questions

### Coding Challenge Sites
* **Project Euler**: https://projecteuler.net/ - Mathematical problems that may use stacks/queues
* **Codeforces**: https://codeforces.com/ - Competitive programming problems
* **AtCoder**: https://atcoder.jp/ - Algorithm and data structure problems

### Community Forums
* **Reddit r/cpp**: https://www.reddit.com/r/cpp/ - C++ community discussions
* **Reddit r/cpp_questions**: https://www.reddit.com/r/cpp_questions/ - Beginner-friendly help
* **C++ Forum**: https://www.cplusplus.com/forum/ - General C++ programming help

---