# CO5625: Memory Management in C++

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Memory Management](#section-1-introduction-to-memory-management)
3. [Section 2: Dynamic Memory Allocation](#section-2-dynamic-memory-allocation)
4. [Section 3: The Stack and the Heap](#section-3-the-stack-and-the-heap)
5. [Section 4: Memory Deallocation](#section-4-memory-deallocation)
6. [Section 5: Memory Management in Classes](#section-5-memory-management-in-classes)
7. [Section 6: Implementing a Dynamic Array Class](#section-6-implementing-a-dynamic-array-class)
8. [Section 7: Dynamic Array Resizing](#section-7-dynamic-array-resizing)
9. [Section 8: Common Pitfalls and Best Practices](#section-8-common-pitfalls-and-best-practices)
10. [Resources](#resources)

---

## Learning Outcomes

By the end of this lecture, you should be able to:

- Allocate and deallocate dynamic memory using the `new` and `delete` operators
- Understand the differences between stack and heap memory allocation
- Recognize when to use static versus dynamic arrays
- Implement proper memory management within C++ classes
- Create constructors and destructors for resource management
- Apply Scope-Based Resource Management (SBRM) principles
- Design and implement a resizable dynamic array class
- Identify and avoid common memory management errors

---

## Section 1: Introduction to Memory Management

Memory management is a fundamental concept in C++ that gives programmers fine-grained control over how their programs use computer memory. Unlike languages with automatic garbage collection, C++ requires explicit management of dynamically allocated memory.

### Why Memory Management Matters

In C++, we have two primary ways to store data:

1. **Automatic (Stack) Storage**: Variables declared normally within functions
2. **Dynamic (Heap) Storage**: Memory explicitly requested at runtime

The stack is limited in size and scope, while the heap allows for flexible memory usage.

In pasrticular the stack is used for:

- Local variables
- Function parameters
- Return addresses

The heap is used for:

- Dynamically allocated memory (using `new` and `delete`)

Understanding when and how to use each type is crucial for writing efficient, reliable C++ programs.

### The Problem with Static Arrays

Static arrays have a fixed size determined at compile time - in terms of both memory allocation and lifetime. This can lead to inefficiencies and limitations.

Consider this limitation:

```cpp
#include <iostream>
using namespace std;

int main() {
    int size;
    cout << "How many numbers do you want to store? ";
    cin >> size;
    
    // This will NOT compile!
    // int numbers[size];  // Array size must be known at compile time
    
    return 0;
}
```

Static arrays must have their size determined at compile time. This inflexibility is where dynamic memory allocation becomes essential. This memory is allocated on the heap, which we will explore next.

---

## Section 2: Dynamic Memory Allocation

Dynamic memory allocation allows us to request memory at runtime, giving our programs the flexibility to adapt to varying data sizes.

### The `new` Operator

The `new` operator allocates memory on the heap and returns a pointer to the beginning of that memory block.

#### Basic Syntax

```cpp
type* pointerName = new type[size];
```

### Complete Example: Dynamic Array Allocation

```cpp
#include <iostream>
using namespace std;

int main() {
    int size;
    cout << "How many integers do you want to store? ";
    cin >> size;
    
    // Allocate dynamic array
    int* data = new int[size];
    
    // Use the array just like a normal array
    cout << "Enter " << size << " numbers:\n";
    for (int i = 0; i < size; i++) {
        cout << "Number " << (i + 1) << ": ";
        cin >> data[i];
    }
    
    // Display the data
    cout << "\nYou entered: ";
    for (int i = 0; i < size; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    // Clean up (we'll discuss this more in Section 4)
    delete[] data;
    
    return 0;
}
```

**Sample Output:**
```
How many integers do you want to store? 5
Enter 5 numbers:
Number 1: 10
Number 2: 20
Number 3: 30
Number 4: 40
Number 5: 50

You entered: 10 20 30 40 50
```

### Key Points About Dynamic Allocation

- **Pointer Declaration**: We declare a pointer (`int* data`) to hold the address of our allocated memory
- **Type Specification**: The pointer type (`int*`) indicates what kind of data will be stored
- **Size Specification**: We use square brackets `[size]` to specify how many elements to allocate
- **Array-Like Access**: We can use array indexing (`data[i]`) to access elements

### Allocating Single Objects

You can also allocate single objects dynamically:

You can think of a single objects as a dynamic array of size 1.

```cpp
#include <iostream>
using namespace std;

int main() {
    // Allocate a single integer
    int* singleNum = new int;
    *singleNum = 42;
    
    cout << "The value is: " << *singleNum << endl;
    cout << "Stored at address: " << singleNum << endl;
    
    delete singleNum;  // Note: no [] for single objects
    
    return 0;
}
```

**Sample Output:**
```
The value is: 42
Stored at address: 0x7ffd8b2c1a40
```

---

## Section 3: The Stack and the Heap

Understanding where your data lives in memory is crucial for effective C++ programming.


In particular, C++ programs use two main memory areas:

1. **The Stack**: For automatic storage (local variables, function parameters) 
2. **The Heap**: For dynamic storage (memory allocated with `new`)

### Memory Layout Visualization

```
        High Memory Addresses
        ┌──────────────────────┐
        │                      │
        │       Stack          │  ← Automatic storage
        │   (grows downward)   │     Local variables
        │                      │     Function parameters
        ├──────────────────────┤
        │                      │
        │         ↓            │
        │                      │
        │      (unused)        │
        │                      │
        │         ↑            │
        │                      │
        ├──────────────────────┤
        │                      │
        │        Heap          │  ← Dynamic storage
        │   (grows upward)     │     new/delete
        │                      │
        └──────────────────────┘
        Low Memory Addresses
```

### The Stack

The stack is a region of memory that stores temporary variables created by functions. It operates in a last-in, first-out (LIFO) manner.

**Characteristics:**

- Small, fixed-size memory region
- Automatically managed (variables created and destroyed)
- Fast access
- Limited size (typically 1-8 MB)

**What Lives on the Stack:**

As mentioned earlier, the stack is used for:

- Local variables
- Function parameters
- Return addresses

```cpp
#include <iostream>
using namespace std;

void demonstrateStack() {
    int x = 10;           // Lives on stack
    double y = 3.14;      // Lives on stack
    int arr[5];           // Lives on stack
    
    cout << "All these variables are on the stack" << endl;
    cout << "They'll be automatically destroyed when this function ends" << endl;
} // x, y, and arr are automatically destroyed here

int main() {
    demonstrateStack();
    // x, y, and arr no longer exist
    return 0;
}
```

### The Heap

**Characteristics:**

- Large memory region (limited by available RAM)
- Manually managed (programmer responsibility)
- Slower access than stack
- Flexible size

**What Lives on the Heap:**

- Dynamically allocated memory (`new` operator)
- Large data structures
- Data that needs to persist beyond function scope

```cpp
#include <iostream>
using namespace std;

int* createArray() {
    int* arr = new int[100];  // Lives on heap
    
    for (int i = 0; i < 100; i++) {
        arr[i] = i;
    }
    
    return arr;  // Pointer returned, data persists
} // arr pointer destroyed, but heap memory remains!

int main() {
    int* numbers = createArray();
    
    cout << "First element: " << numbers[0] << endl;
    cout << "Last element: " << numbers[99] << endl;
    
    delete[] numbers;  // Must manually free heap memory
    
    return 0;
}
```

**Sample Output:**
```
First element: 0
Last element: 99
```

### Static vs Dynamic Arrays

Due to syntax similarities, we use specific terminology:

- **Static Arrays**: Arrays with compile-time size, stored on stack
- **Dynamic Arrays**: Arrays allocated with `new`, stored on heap

```cpp
#include <iostream>
using namespace std;

int main() {
    // STATIC ARRAY (Stack)
    int staticArr[5] = {1, 2, 3, 4, 5};
    
    // DYNAMIC ARRAY (Heap)
    int* dynamicArr = new int[5];
    for (int i = 0; i < 5; i++) {
        dynamicArr[i] = i + 1;
    }
    
    cout << "Both can be accessed similarly:" << endl;
    cout << "Static: " << staticArr[2] << endl;
    cout << "Dynamic: " << dynamicArr[2] << endl;
    
    delete[] dynamicArr;
    
    return 0;
}
```

### Stack Overflow

When you allocate too much data on the stack, you get a **stack overflow error**:

This often happens with large arrays or deep recursion.

Here's an example that may cause a stack overflow:

```cpp
#include <iostream>
using namespace std;

int main() {
    // This might cause a stack overflow!
    // int huge[1000000];
    
    // Solution: use heap instead
    int* huge = new int[1000000];
    cout << "Successfully allocated 1 million integers on heap" << endl;
    
    delete[] huge;
    
    return 0;
}
```

**Sample Output:**
```
Successfully allocated 1 million integers on heap
```

The stack overflow line is commented out to prevent crashes.

---

## Section 4: Memory Deallocation

Every allocation must have a corresponding deallocation. Failing to deallocate memory causes **memory leaks**.

### The `delete` Operator

The `delete` operator returns dynamically allocated memory to the system.

#### Syntax Rules

```cpp
delete pointer;       // For single objects
delete[] pointer;     // For arrays
```

### Complete Example: Proper Deallocation

```cpp
#include <iostream>
using namespace std;

int main() {
    // Allocate array
    double* myNumbers = new double[75];
    
    // Use the array
    for (int i = 0; i < 75; i++) {
        myNumbers[i] = i * 1.5;
    }
    
    cout << "Element 10: " << myNumbers[10] << endl;
    cout << "Element 50: " << myNumbers[50] << endl;
    
    // Deallocate - CRITICAL!
    delete[] myNumbers;
    
    cout << "Memory successfully freed" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Element 10: 15
Element 50: 75
Memory successfully freed
```

### Why We Need `delete[]`

The `[]` syntax tells the compiler to deallocate the **entire array**, not just the first element:

```cpp
#include <iostream>
using namespace std;

int main() {
    int* arr = new int[10];
    
    // Fill array
    for (int i = 0; i < 10; i++) {
        arr[i] = i;
    }
    
    // WRONG: delete arr;     // Only deletes first element!
    // RIGHT:
    delete[] arr;              // Deletes entire array
    
    // Single object allocation
    int* single = new int(42);
    delete single;             // No [] for single objects
    
    return 0;
}
```

### Stack vs Heap Deallocation

Stack memory is automatically managed, while heap memory requires manual management. Here's a comparison:

```cpp
#include <iostream>
using namespace std;

int main() {
    // STACK: Automatic deallocation
    {
        int stackVar = 10;
        int stackArr[5] = {1, 2, 3, 4, 5};
        cout << "Stack variables created" << endl;
    } // stackVar and stackArr automatically destroyed here
    
    cout << "Stack variables gone" << endl;
    
    // HEAP: Manual deallocation required
    {
        int* heapVar = new int(10);
        int* heapArr = new int[5];
        cout << "Heap variables created" << endl;
        
        delete heapVar;
        delete[] heapArr;
        cout << "Heap variables manually deleted" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Stack variables created
Stack variables gone
Heap variables created
Heap variables manually deleted
```

### Memory Leak Example

```cpp
#include <iostream>
using namespace std;

void memoryLeak() {
    int* data = new int[1000];
    // ... use data ...
    
    // FORGOT TO DELETE!
    // Memory is now leaked
}

void properMemory() {
    int* data = new int[1000];
    // ... use data ...
    
    delete[] data;  // Properly freed
}

int main() {
    for (int i = 0; i < 100; i++) {
        memoryLeak();  // Leaks memory each iteration!
    }
    
    for (int i = 0; i < 100; i++) {
        properMemory();  // No leaks
    }
    
    return 0;
}
```

---

## Section 5: Memory Management in Classes

Object-oriented programming provides elegant solutions for memory management through constructors and destructors.

### Scope-Based Resource Management (SBRM)

SBRM is a technique where:
- **Constructor** allocates resources (memory, files, etc.)
- **Destructor** releases resources
- Resources are tied to object lifetime

### Destructor Syntax

A destructor is a special member function:
- Same name as the class, preceded by `~`
- No parameters
- No return type
- Called automatically when object goes out of scope

```cpp
class MyClass {
public:
    MyClass() {
        // Constructor
    }
    
    ~MyClass() {
        // Destructor
    }
};
```

### Virtual Destructors

Destructors are often declared `virtual` to ensure proper cleanup in inheritance hierarchies:

```cpp
class Base {
public:
    virtual ~Base() {
        // Virtual destructor
    }
};

class Derived : public Base {
public:
    ~Derived() override {
        // Will be called even through Base pointer
    }
};
```

### Why Virtual Matters

Virtual destructors ensure that when deleting derived class objects through base class pointers, the derived class destructor is called first, preventing resource leaks.

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    Base() { cout << "Base constructed" << endl; }
    virtual ~Base() { cout << "Base destructed" << endl; }
};

class Derived : public Base {
private:
    int* data;
public:
    Derived() : Base() {
        data = new int[100];
        cout << "Derived constructed" << endl;
    }
    
    ~Derived() override {
        delete[] data;
        cout << "Derived destructed" << endl;
    }
};

int main() {
    cout << "Creating Derived through Base pointer:" << endl;
    Base* ptr = new Derived();
    
    cout << "\nDeleting through Base pointer:" << endl;
    delete ptr;  // Calls both destructors due to virtual
    
    return 0;
}
```

**Sample Output:**
```
Creating Derived through Base pointer:
Base constructed
Derived constructed

Deleting through Base pointer:
Derived destructed
Base destructed
```

---

## Section 6: Implementing a Dynamic Array Class

Let's build a complete dynamic array class that manages its own memory. This will illustrate constructors, destructors, and operator overloading - and from a memory management perpective we will see that it encapsulates dynamic memory handling.  

### Complete dynArray Class

```cpp
#include <iostream>
using namespace std;

class dynArray {
private:
    int* data;
    int sz;

public:
    // Default constructor
    dynArray() : data(nullptr), sz(0) {
        cout << "Default constructor: empty array created" << endl;
    }
    
    // Parameterized constructor
    dynArray(int size) : sz(size) {
        data = new int[sz];
        for (int i = 0; i < sz; i++) {
            data[i] = 0;  // Initialize to zero
        }
        cout << "Constructor: array of size " << sz << " created" << endl;
    }
    
    // Destructor
    virtual ~dynArray() {
        if (data) {
            delete[] data;
            cout << "Destructor: array memory freed" << endl;
        }
    }
    
    // Operator overload for array access
    int& operator[](int idx) {
        return data[idx];
    }
    
    // Size getter
    int size() const {
        return sz;
    }
    
    // Display method
    void display() const {
        cout << "[ ";
        for (int i = 0; i < sz; i++) {
            cout << data[i] << " ";
        }
        cout << "]" << endl;
    }
};

int main() {
    cout << "=== Creating array of size 10 ===" << endl;
    dynArray arr(10);
    
    // Set values using operator[]
    cout << "\n=== Setting values ===" << endl;
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = i * 2;
    }
    
    // Access and display
    cout << "\n=== Accessing elements ===" << endl;
    cout << "Element at index 3: " << arr[3] << endl;
    cout << "Element at index 7: " << arr[7] << endl;
    
    cout << "\n=== Full array ===" << endl;
    arr.display();
    
    cout << "\n=== End of main (destructor will be called) ===" << endl;
    return 0;
}
```

**Sample Output:**
```
=== Creating array of size 10 ===
Constructor: array of size 10 created

=== Setting values ===

=== Accessing elements ===
Element at index 3: 6
Element at index 7: 14

=== Full array ===
[ 0 2 4 6 8 10 12 14 16 18 ]

=== End of main (destructor will be called) ===
Destructor: array memory freed
```

### Understanding the Components

These are some key parts of the `dynArray` class implementation that are important for memory management:

#### Constructor Initialization List

This initializes member variables before the constructor body runs. These variables must be initialized before use. From a memory management perspective, this ensures that `sz` is set before we allocate memory based on it.

```cpp
dynArray(int size) : sz(size) {
    // sz is initialized before the constructor body runs
}
```

#### Null Pointer Check in Destructor

This is a safety check to avoid deleting a null pointer. From a memory management perspective, it ensures we only attempt to free memory that was actually allocated.

```cpp
if (data) {
    delete[] data;
}
```

This prevents attempting to delete a null pointer (though it's technically safe in C++, it's good practice).

#### Operator Overloading

This allows array-like access to the dynamic array elements. From a memory management perspective, it encapsulates access to the underlying dynamically allocated memory.

```cpp
int& operator[](int idx) {
    return data[idx];
}
```

This returns a **reference** so we can both read and write:

- Reading: `int x = arr[5];`
- Writing: `arr[5] = 10;`

### Automatic Memory Management

The destructor ensures that when a `dynArray` object goes out of scope, its allocated memory is automatically freed, preventing memory leaks. From a memory management perspective, this encapsulates the cleanup logic within the class itself.

```cpp
#include <iostream>
using namespace std;

class dynArray {
private:
    int* data;
    int sz;

public:
    dynArray(int size) : sz(size) {
        data = new int[sz];
        cout << "Allocated " << sz << " integers" << endl;
    }
    
    virtual ~dynArray() {
        delete[] data;
        cout << "Freed memory" << endl;
    }
    
    int& operator[](int idx) { return data[idx]; }
    int size() const { return sz; }
};

void processData() {
    dynArray temp(100);
    temp[0] = 42;
    cout << "Processing..." << endl;
    // Destructor automatically called when function ends
}

int main() {
    cout << "=== Calling processData ===" << endl;
    processData();
    cout << "=== Back in main ===" << endl;
    cout << "Memory was automatically cleaned up!" << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Calling processData ===
Allocated 100 integers
Processing...
Freed memory
=== Back in main ===
Memory was automatically cleaned up!
```

---

## Section 7: Dynamic Array Resizing

One of the key advantages of dynamic arrays is the ability to resize them at runtime.

### Resizing Strategy

To resize a dynamic array:

1. Allocate a new block with the desired size
2. Copy existing data to the new block
3. Delete the old block
4. Update the pointer to point to the new block

### Adding resize() Method

```cpp
#include <iostream>
using namespace std;

class dynArray {
private:
    int* data;
    int sz;

public:
    dynArray() : data(nullptr), sz(0) {}
    
    dynArray(int size) : sz(size) {
        data = new int[sz];
        for (int i = 0; i < sz; i++) {
            data[i] = 0;
        }
    }
    
    virtual ~dynArray() {
        if (data) delete[] data;
    }
    
    void resize(int newSize) {
        if (newSize > sz) {
            // Allocate new block
            int* newData = new int[newSize];
            
            // Copy existing data
            for (int i = 0; i < sz; i++) {
                newData[i] = data[i];
            }
            
            // Initialize new elements to zero
            for (int i = sz; i < newSize; i++) {
                newData[i] = 0;
            }
            
            // Delete old block
            delete[] data;
            
            // Update pointer and size
            data = newData;
        }
        sz = newSize;
    }
    
    int& operator[](int idx) { return data[idx]; }
    int size() const { return sz; }
    
    void display() const {
        cout << "Size " << sz << ": [ ";
        for (int i = 0; i < sz; i++) {
            cout << data[i] << " ";
        }
        cout << "]" << endl;
    }
};

int main() {
    cout << "=== Creating initial array ===" << endl;
    dynArray arr(5);
    
    // Fill with data
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = i + 1;
    }
    
    cout << "Original: ";
    arr.display();
    
    cout << "\n=== Resizing to 10 ===" << endl;
    arr.resize(10);
    
    cout << "After resize: ";
    arr.display();
    
    // Fill new elements
    for (int i = 5; i < arr.size(); i++) {
        arr[i] = (i + 1) * 10;
    }
    
    cout << "With new data: ";
    arr.display();
    
    return 0;
}
```

**Sample Output:**
```
=== Creating initial array ===
Original: Size 5: [ 1 2 3 4 5 ]

=== Resizing to 10 ===
After resize: Size 10: [ 1 2 3 4 5 0 0 0 0 0 ]
With new data: Size 10: [ 1 2 3 4 5 60 70 80 90 100 ]
```

### Memory Visualization

We can see the memory changes during resizing:

```
Before resize(10):
  data → [1][2][3][4][5]  (sz = 5)

During resize:
  data → [1][2][3][4][5]  (old block)
  newData → [1][2][3][4][5][0][0][0][0][0]  (new block)

After resize:
  data → [1][2][3][4][5][0][0][0][0][0]  (sz = 10)
  (old block deleted)
```

Notice how the old memory is properly freed to avoid leaks.

### Growth Example

Here is an example demonstrating dynamic growth of the array as elements are added:

```cpp
#include <iostream>
using namespace std;

class dynArray {
private:
    int* data;
    int sz;

public:
    dynArray(int size) : sz(size) {
        data = new int[sz];
        for (int i = 0; i < sz; i++) data[i] = 0;
    }
    
    ~dynArray() { delete[] data; }
    
    void resize(int newSize) {
        if (newSize > sz) {
            int* newData = new int[newSize];
            for (int i = 0; i < sz; i++) newData[i] = data[i];
            for (int i = sz; i < newSize; i++) newData[i] = 0;
            delete[] data;
            data = newData;
        }
        sz = newSize;
    }
    
    int& operator[](int idx) { return data[idx]; }
    int size() const { return sz; }
    
    void display() const {
        for (int i = 0; i < sz; i++) cout << data[i] << " ";
        cout << endl;
    }
};

int main() {
    dynArray arr(2);
    int value = 10;
    
    cout << "Growing array dynamically:" << endl;
    for (int i = 0; i < 8; i++) {
        if (i >= arr.size()) {
            cout << "Resizing from " << arr.size();
            arr.resize(arr.size() * 2);
            cout << " to " << arr.size() << endl;
        }
        arr[i] = value;
        value += 10;
        
        cout << "After adding element " << i << ": ";
        arr.display();
    }
    
    return 0;
}
```

**Sample Output:**
```
Growing array dynamically:
After adding element 0: 10 0 
After adding element 1: 10 20 
Resizing from 2 to 4
After adding element 2: 10 20 30 0 
After adding element 3: 10 20 30 40 
Resizing from 4 to 8
After adding element 4: 10 20 30 40 50 0 0 0 
After adding element 5: 10 20 30 40 50 60 0 0 
After adding element 6: 10 20 30 40 50 60 70 0 
After adding element 7: 10 20 30 40 50 60 70 80 
```

Notice how the array resizes as needed, doubling its size each time it runs out of space.

---

## Section 8: Common Pitfalls and Best Practices

### Common Mistakes

#### 1. Memory Leaks

Memory leaks occur when allocated memory is not freed.

```cpp
#include <iostream>
using namespace std;

void memoryLeakExample() {
    int* data = new int[100];
    // ... use data ...
    // FORGOT delete[] data;
}

void correctExample() {
    int* data = new int[100];
    // ... use data ...
    delete[] data;  // Properly freed
}

int main() {
    cout << "Memory leak example (don't do this!)" << endl;
    memoryLeakExample();
    
    cout << "Correct example" << endl;
    correctExample();
    
    return 0;
}
```

#### 2. Double Delete

Double deleting the same memory causes undefined behavior.

```cpp
#include <iostream>
using namespace std;

int main() {
    int* data = new int[10];
    
    delete[] data;
    // delete[] data;  // WRONG! Double delete causes crash
    
    // Safe practice: set to nullptr after delete
    data = nullptr;
    delete[] data;  // Safe - deleting nullptr does nothing
    
    return 0;
}
```

#### 3. Dangling Pointers

Dangling pointers point to memory that has already been freed.

```cpp
#include <iostream>
using namespace std;

int main() {
    int* ptr = new int(42);
    cout << "Value: " << *ptr << endl;
    
    delete ptr;
    // ptr is now a dangling pointer
    
    // WRONG: cout << *ptr << endl;  // Undefined behavior!
    
    // Safe practice
    ptr = nullptr;
    if (ptr != nullptr) {
        cout << *ptr << endl;  // This won't execute
    }
    
    return 0;
}
```

Note: Always set pointers to `nullptr` after deletion to avoid dangling pointers.

#### 4. Mixing delete and delete[]

Mixing `delete` and `delete[]` leads to undefined behavior.

```cpp
#include <iostream>
using namespace std;

int main() {
    int* single = new int(42);
    int* array = new int[10];
    
    delete single;    // Correct for single object
    delete[] array;   // Correct for array
    
    // WRONG combinations:
    // delete array;    // Should be delete[]
    // delete[] single; // Should be delete
    
    return 0;
}
```

Here we see the importance of using the correct form of delete based on how memory was allocated.

### Best Practices

#### 1. Always Pair new with delete

This ensures that every allocation has a corresponding deallocation.

```cpp
#include <iostream>
using namespace std;

class SafeArray {
private:
    int* data;
    int sz;
    
public:
    SafeArray(int size) : sz(size) {
        data = new int[sz];
    }
    
    ~SafeArray() {
        delete[] data;  // Always pairs with new[]
    }
    
    int& operator[](int idx) { return data[idx]; }
};

int main() {
    SafeArray arr(10);
    // No manual delete needed - destructor handles it
    return 0;
}
```

#### 2. Initialize Pointers

Make it a habit to initialize pointers to `nullptr`.

```cpp
#include <iostream>
using namespace std;

int main() {
    // Good practice: initialize to nullptr
    int* ptr = nullptr;
    
    if (/* some condition */ true) {
        ptr = new int[100];
    }
    
    // Safe to check
    if (ptr != nullptr) {
        delete[] ptr;
        ptr = nullptr;
    }
    
    return 0;
}
```

#### 3. Use SBRM (Resource Acquisition Is Initialization)

SBRM ties resource management to object lifetime, ensuring automatic cleanup.

```cpp
#include <iostream>
using namespace std;

class ResourceManager {
private:
    int* resource;
    
public:
    ResourceManager(int size) {
        resource = new int[size];
        cout << "Resource acquired" << endl;
    }
    
    ~ResourceManager() {
        delete[] resource;
        cout << "Resource released" << endl;
    }
    
    // Prevent copying (advanced topic - mentioned for completeness)
    ResourceManager(const ResourceManager&) = delete;
    ResourceManager& operator=(const ResourceManager&) = delete;
};

int main() {
    {
        ResourceManager rm(100);
        // Use resource
    }  // Automatically cleaned up here
    
    cout << "Resource automatically freed" << endl;
    return 0;
}
```

**Sample Output:**
```
Resource acquired
Resource released
Resource automatically freed
```

### Quick Reference Guide

```
┌─────────────────────────────────────────────────────────┐
│                 Memory Management Rules                  │
├─────────────────────────────────────────────────────────┤
│ Allocation         │ Deallocation                       │
├────────────────────┼────────────────────────────────────┤
│ new Type           │ delete ptr                         │
│ new Type[size]     │ delete[] ptr                       │
│ Constructor        │ Destructor                         │
├────────────────────┴────────────────────────────────────┤
│ Best Practices:                                         │
│ ✓ Always pair new with delete                           │
│ ✓ Use delete[] for arrays                               │
│ ✓ Set pointers to nullptr after delete                  │
│ ✓ Check for nullptr before dereferencing                │
│ ✓ Use SBRM in classes                                   │
│ ✓ Make destructors virtual in base classes              │
└─────────────────────────────────────────────────────────┘
```

---

## Resources

### Official Documentation

- **C++ Reference**: https://en.cppreference.com/
  - Dynamic memory management: https://en.cppreference.com/w/cpp/memory
  - new operator: https://en.cppreference.com/w/cpp/language/new
  - delete operator: https://en.cppreference.com/w/cpp/language/delete

### Textbooks and Tutorials

- **LearnCpp.com**: https://www.learncpp.com/
  - Chapter on dynamic memory allocation
  - RAII and smart pointers section

- **C++ Programming Language** by Bjarne Stroustrup
  - Chapters on memory management and resource management

- **Effective C++** by Scott Meyers
  - Items on resource management and destructors

### Online Practice Platforms

- **HackerRank C++**: https://www.hackerrank.com/domains/cpp
  - Memory management challenges

- **LeetCode**: https://leetcode.com/
  - Algorithm problems requiring dynamic memory

- **Exercism C++ Track**: https://exercism.org/tracks/cpp
  - Guided exercises with mentoring

### Community Resources

- **Stack Overflow**: https://stackoverflow.com/questions/tagged/c++
  - Search for memory management questions
  
- **C++ Super-FAQ**: https://isocpp.org/faq
  - Section on memory management

- **r/cpp**: https://www.reddit.com/r/cpp/
  - Active C++ community discussions

### Tools for Memory Debugging

- **Valgrind**: Memory leak detection tool for Linux
  - Website: https://valgrind.org/
  
- **AddressSanitizer**: Built into GCC and Clang compilers
  - Documentation: https://github.com/google/sanitizers

- **Visual Studio Memory Diagnostics**: Built-in tools for Windows developers

### Additional Topics for Further Study

- Smart pointers (std::unique_ptr, std::shared_ptr)
- Move semantics and rvalue references
- Custom memory allocators
- Memory pools and object pools
- Cache-friendly data structures

---

**End of Lecture**