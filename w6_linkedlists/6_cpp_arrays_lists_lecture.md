# Arrays and Linked Lists

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Data Structures](#section-1-introduction-to-data-structures)
3. [Section 2: Arrays in Detail](#section-2-arrays-in-detail)
4. [Section 3: Linked Lists Fundamentals](#section-3-linked-lists-fundamentals)
5. [Section 4: Linked List Operations](#section-4-linked-list-operations)
6. [Section 5: Variations of Linked Lists](#section-5-variations-of-linked-lists)
7. [Section 6: The Standard Template Library (STL)](#section-6-the-standard-template-library-stl)
8. [Section 7: Working with STL Containers](#section-7-working-with-stl-containers)
9. [Section 8: Summary and Comparison](#section-8-summary-and-comparison)
10. [Resources for Further Study](#resources-for-further-study)

---

## Learning Outcomes

By the end of this lecture, you should be able to:

1. Explain the structure and memory layout of arrays and linked lists
2. Analyze the time complexity of common operations on arrays and linked lists
3. Identify appropriate use cases for arrays versus linked lists based on operation requirements
4. Understand pointer manipulation in linked list operations
5. Implement basic programs using STL vector and list containers
6. Use iterators to traverse STL containers
7. Evaluate the trade-offs between different data structure choices

---

## Section 1: Introduction to Data Structures

Data structures are fundamental tools for organizing and storing data in computer programs. The choice of data structure significantly impacts program performance, memory usage, and code complexity. In this lecture, we'll explore two foundational data structures: arrays and linked lists.

These structures represent two fundamentally different approaches to storing collections of data. Arrays use contiguous memory allocation, while linked lists use distributed memory with pointer connections. Understanding both will help you make informed decisions about which structure to use in different scenarios.

---

## Section 2: Arrays in Detail

### What is an Array?

An array is a collection of elements stored in contiguous memory locations. This means all elements are placed next to each other in memory, forming one continuous block. Each element can be accessed using an index that represents its position in the array.

```
Memory Layout of an Array:
┌─────────────────────────────────────────────┐
│   Memory Address Space                      │
├─────┬─────┬─────┬─────┬─────┬─────────────┤
│     │  a  │     │     │     │             │
│     │  │  │     │     │     │             │
│     │  └──┼─────────────────┐             │
│     │     │     │     │     │             │
├─────┴─────┴─────┴─────┴─────┴─────────────┤
│         ┌─────┬─────┬─────┬─────┬─────┐   │
│         │ a[0]│ a[1]│ a[2]│ a[3]│ a[4]│   │
│         │  1  │  2  │  4  │  8  │ 16  │   │
│         └─────┴─────┴─────┴─────┴─────┘   │
└─────────────────────────────────────────────┘
```

### Strengths of Arrays

**1. Random Access (Constant Time - O(1))**

Arrays provide direct access to any element using its index. The memory address of any element can be calculated using: `base_address + (index × element_size)`. This means accessing `arr[0]` takes the same time as accessing `arr[1000]`.

**2. Easy and Fast Element Lookup**

Because of the mathematical relationship between index and memory address, finding and modifying elements is extremely fast and straightforward.

**3. Cache-Friendly**

The contiguous memory layout means that accessing sequential elements benefits from CPU cache, making array traversal very efficient.

### Weaknesses of Arrays

**1. Resizing is Expensive (O(n))**

When an array needs to grow, the entire array must be copied to a new, larger memory location. This is because arrays need contiguous memory, and there might not be enough space adjacent to the current array.

**2. Insertion and Deletion at Arbitrary Positions (O(n))**

Inserting or removing an element anywhere except the end requires shifting all subsequent elements to maintain contiguity.

```
Inserting 5 at index 1:
Before: [1, 2, 4, 8, 16]
         0  1  2  3   4

Step 1: Shift elements right
        [1, 2, 2, 4, 8, 16]
         0  1  2  3  4   5

Step 2: Insert new element
        [1, 5, 2, 4, 8, 16]
         0  1  2  3  4   5
```

### Complete Array Example Program

Here is a complete C++ program demonstrating array operations:

```cpp
#include <iostream>
using namespace std;

int main() {
    // Static array declaration
    int staticArray[5] = {1, 2, 4, 8, 16};
    
    cout << "Static Array Contents:" << endl;
    for (int i = 0; i < 5; i++) {
        cout << "staticArray[" << i << "] = " << staticArray[i] << endl;
    }
    
    // Demonstrating random access (O(1))
    cout << "\nAccessing element at index 3: " << staticArray[3] << endl;
    
    // Modifying an element (O(1))
    staticArray[2] = 100;
    cout << "After modifying index 2: " << staticArray[2] << endl;
    
    // Dynamic array allocation
    int size = 5;
    int* dynamicArray = new int[size];
    
    // Populating dynamic array
    for (int i = 0; i < size; i++) {
        dynamicArray[i] = i * i;
    }
    
    cout << "\nDynamic Array Contents:" << endl;
    for (int i = 0; i < size; i++) {
        cout << "dynamicArray[" << i << "] = " << dynamicArray[i] << endl;
    }
    
    // Simulating array "resize" (requires new allocation and copy)
    int newSize = 8;
    int* resizedArray = new int[newSize];
    
    // Copy existing elements
    for (int i = 0; i < size; i++) {
        resizedArray[i] = dynamicArray[i];
    }
    
    // Initialize new elements
    for (int i = size; i < newSize; i++) {
        resizedArray[i] = 0;
    }
    
    delete[] dynamicArray;  // Free old memory
    dynamicArray = resizedArray;
    
    cout << "\nResized Array (O(n) operation):" << endl;
    for (int i = 0; i < newSize; i++) {
        cout << "dynamicArray[" << i << "] = " << dynamicArray[i] << endl;
    }
    
    delete[] dynamicArray;  // Clean up
    
    return 0;
}
```

**Sample Output:**
```
Static Array Contents:
staticArray[0] = 1
staticArray[1] = 2
staticArray[2] = 4
staticArray[3] = 8
staticArray[4] = 16

Accessing element at index 3: 8
After modifying index 2: 100

Dynamic Array Contents:
dynamicArray[0] = 0
dynamicArray[1] = 1
dynamicArray[2] = 4
dynamicArray[3] = 9
dynamicArray[4] = 16

Resized Array (O(n) operation):
dynamicArray[0] = 0
dynamicArray[1] = 1
dynamicArray[2] = 4
dynamicArray[3] = 9
dynamicArray[4] = 16
dynamicArray[5] = 0
dynamicArray[6] = 0
dynamicArray[7] = 0
```

---

Notice how resizing the array required allocating new memory and copying existing elements, demonstrating the O(n) complexity of this operation.

## Section 3: Linked Lists Fundamentals

### What is a Linked List?

A linked list is a dynamic data structure where elements (called nodes) can be scattered anywhere in memory. Each node contains two parts: the actual data and a pointer to the next node in the sequence. This structure provides flexibility that arrays cannot offer.

```
Linked List Structure:
┌──────┐
│ head │──┐
└──────┘  │
          ▼
     ┌─────┬────┐    ┌─────┬────┐    ┌─────┬────┐    ┌─────┬────┐
     │data │next│───▶│data │next│───▶│data │next│───▶│data │next│───▶ NULL
     │  3  │  ●─┘    │ 10  │  ●─┘    │  2  │  ●─┘    │  1  │  ●─┘
     └─────┴────┘    └─────┴────┘    └─────┴────┘    └─────┴────┘
```

### Key Components

**1. Node**: The basic building block containing data and a pointer to the next node

**2. Head**: A pointer to the first node in the list

**3. NULL Pointer**: The last node points to NULL (address 0), indicating the end of the list

### Advantages of Linked Lists

**1. Dynamic Size**: Nodes can be added or removed without reallocating the entire structure

**2. Efficient Insertion/Deletion**: When you have a pointer to the location, insertion and deletion are O(1) operations

**3. No Wasted Space**: Memory is allocated only as needed

**4. No Need for Contiguous Memory**: Nodes can be anywhere in memory

### Disadvantages of Linked Lists

**1. No Random Access**: To access the nth element, you must traverse from the head through n-1 nodes (O(n) operation)

**2. Extra Memory**: Each node requires additional memory for the pointer

**3. Cache Unfriendly**: Nodes are scattered in memory, reducing cache efficiency

### Basic Linked List Implementation

Below is a simple C++ program that demonstrates the creation and traversal of a singly linked list:

```cpp
#include <iostream>
using namespace std;

// Node structure
struct Node {
    int data;
    Node* next;
    
    // Constructor for easy node creation
    Node(int value) : data(value), next(nullptr) {}
};

// Function to print the linked list
void printList(Node* head) {
    Node* current = head;
    cout << "List: ";
    while (current != nullptr) {
        cout << current->data;
        if (current->next != nullptr) {
            cout << " -> ";
        }
        current = current->next;
    }
    cout << " -> NULL" << endl;
}

// Function to get the length of the list
int getLength(Node* head) {
    int count = 0;
    Node* current = head;
    while (current != nullptr) {
        count++;
        current = current->next;
    }
    return count;
}

// Function to access element at specific index (O(n) operation)
int getElementAt(Node* head, int index) {
    Node* current = head;
    int currentIndex = 0;
    
    while (current != nullptr) {
        if (currentIndex == index) {
            return current->data;
        }
        current = current->next;
        currentIndex++;
    }
    
    return -1;  // Index out of bounds
}

int main() {
    // Creating nodes manually
    Node* head = new Node(3);
    head->next = new Node(10);
    head->next->next = new Node(2);
    head->next->next->next = new Node(1);
    
    cout << "Initial Linked List:" << endl;
    printList(head);
    
    cout << "\nLength of list: " << getLength(head) << endl;
    
    cout << "\nAccessing elements (O(n) for each access):" << endl;
    cout << "Element at index 0: " << getElementAt(head, 0) << endl;
    cout << "Element at index 2: " << getElementAt(head, 2) << endl;
    
    // Cleanup memory
    Node* current = head;
    while (current != nullptr) {
        Node* temp = current;
        current = current->next;
        delete temp;
    }
    
    return 0;
}
```

**Sample Output:**
```
Initial Linked List:
List: 3 -> 10 -> 2 -> 1 -> NULL

Length of list: 4

Accessing elements (O(n) for each access):
Element at index 0: 3
Element at index 2: 2
```

---

Notice how accessing elements requires traversal from the head, demonstrating the O(n) complexity of this operation.

## Section 4: Linked List Operations

### Insertion Operation (O(1) with pointer to previous node)

To insert a new node B between nodes A and C, we need a pointer to node A (the node before the insertion point). The algorithm is straightforward:

1. Set B's next pointer to A's next pointer (now B points to C)
2. Set A's next pointer to B (now A points to B)

```
Before Insertion:
     ┌─────┬────┐         ┌─────┬────┐
     │  A  │next│────────▶│  C  │next│───▶
     └─────┴────┘         └─────┴────┘

After Creating New Node B:
     ┌─────┬────┐
     │  B  │next│───▶ ?
     └─────┴────┘

Step 1: B->next = A->next
     ┌─────┬────┐         ┌─────┬────┐
     │  A  │next│────┐    │  C  │next│───▶
     └─────┴────┘    │    └─────┴────┘
                     │         ▲
     ┌─────┬────┐    │         │
     │  B  │next│────┘─────────┘
     └─────┴────┘

Step 2: A->next = B
     ┌─────┬────┐         ┌─────┬────┐
     │  A  │next│───┐     │  C  │next│───▶
     └─────┴────┘   │     └─────┴────┘
                    │          ▲
     ┌─────┬────┐   │          │
     │  B  │next│───┘──────────┘
     └─────┴────┘   ▲
                    │
             (A points to B)
```

This operation works even when inserting at the end of the list because the NULL pointer is simply copied to the new node.

### Deletion Operation (O(1) with pointer to previous node)

To remove node B (between A and C), we need a pointer to node A:

1. Set A's next pointer to B's next pointer (A now points to C)
2. Optionally delete B to free memory

```
Before Deletion:
     ┌─────┬────┐    ┌─────┬────┐    ┌─────┬────┐
     │  A  │next│───▶│  B  │next│───▶│  C  │next│───▶
     └─────┴────┘    └─────┴────┘    └─────┴────┘

Step 1: A->next = B->next
     ┌─────┬────┐                    ┌─────┬────┐
     │  A  │next│───────────────────▶│  C  │next│───▶
     └─────┴────┘                    └─────┴────┘
                    ┌─────┬────┐
                    │  B  │next│───▶ (orphaned)
                    └─────┴────┘

Step 2: delete B
     ┌─────┬────┐                    ┌─────┬────┐
     │  A  │next│───────────────────▶│  C  │next│───▶
     └─────┴────┘                    └─────┴────┘
```

### Complete Insertion and Deletion Example

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
    
    Node(int value) : data(value), next(nullptr) {}
};

void printList(Node* head) {
    Node* current = head;
    cout << "List: ";
    while (current != nullptr) {
        cout << current->data;
        if (current->next != nullptr) {
            cout << " -> ";
        }
        current = current->next;
    }
    cout << " -> NULL" << endl;
}

// Insert node after a given node (O(1) operation)
void insertAfter(Node* prevNode, int newData) {
    if (prevNode == nullptr) {
        cout << "Previous node cannot be NULL" << endl;
        return;
    }
    
    // Step 1: Create new node
    Node* newNode = new Node(newData);
    
    // Step 2: Make new node's next point to prevNode's next
    newNode->next = prevNode->next;
    
    // Step 3: Make prevNode's next point to new node
    prevNode->next = newNode;
    
    cout << "Inserted " << newData << " after " << prevNode->data << endl;
}

// Insert at the beginning (O(1) operation)
void insertAtBeginning(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    newNode->next = head;
    head = newNode;
    
    cout << "Inserted " << newData << " at beginning" << endl;
}

// Insert at the end (O(n) operation - must traverse to end)
void insertAtEnd(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    
    // If list is empty
    if (head == nullptr) {
        head = newNode;
        cout << "Inserted " << newData << " as first node" << endl;
        return;
    }
    
    // Traverse to the end
    Node* current = head;
    while (current->next != nullptr) {
        current = current->next;
    }
    
    current->next = newNode;
    cout << "Inserted " << newData << " at end" << endl;
}

// Delete node after a given node (O(1) operation)
void deleteAfter(Node* prevNode) {
    if (prevNode == nullptr || prevNode->next == nullptr) {
        cout << "Cannot delete: no node after the given node" << endl;
        return;
    }
    
    Node* nodeToDelete = prevNode->next;
    int deletedData = nodeToDelete->data;
    
    // Make prevNode point to the node after the one being deleted
    prevNode->next = nodeToDelete->next;
    
    delete nodeToDelete;
    
    cout << "Deleted node with value " << deletedData << endl;
}

// Delete first node (O(1) operation)
void deleteFirst(Node*& head) {
    if (head == nullptr) {
        cout << "List is empty, cannot delete" << endl;
        return;
    }
    
    Node* temp = head;
    int deletedData = temp->data;
    head = head->next;
    delete temp;
    
    cout << "Deleted first node with value " << deletedData << endl;
}

int main() {
    // Create initial list: 3 -> 10 -> 2 -> 1 -> NULL
    Node* head = new Node(3);
    head->next = new Node(10);
    head->next->next = new Node(2);
    head->next->next->next = new Node(1);
    
    cout << "Initial List:" << endl;
    printList(head);
    cout << endl;
    
    // Insertion operations
    cout << "=== Insertion Operations ===" << endl;
    
    // Insert 5 after the first node (O(1))
    insertAfter(head, 5);
    printList(head);
    cout << endl;
    
    // Insert 99 at beginning (O(1))
    insertAtBeginning(head, 99);
    printList(head);
    cout << endl;
    
    // Insert 77 at end (O(n))
    insertAtEnd(head, 77);
    printList(head);
    cout << endl;
    
    // Deletion operations
    cout << "=== Deletion Operations ===" << endl;
    
    // Delete node after head (O(1))
    deleteAfter(head);
    printList(head);
    cout << endl;
    
    // Delete first node (O(1))
    deleteFirst(head);
    printList(head);
    cout << endl;
    
    // Cleanup remaining nodes
    while (head != nullptr) {
        Node* temp = head;
        head = head->next;
        delete temp;
    }
    
    return 0;
}
```

**Sample Output:**
```
Initial List:
List: 3 -> 10 -> 2 -> 1 -> NULL

=== Insertion Operations ===
Inserted 5 after 3
List: 3 -> 5 -> 10 -> 2 -> 1 -> NULL

Inserted 99 at beginning
List: 99 -> 3 -> 5 -> 10 -> 2 -> 1 -> NULL

Inserted 77 at end
List: 99 -> 3 -> 5 -> 10 -> 2 -> 1 -> 77 -> NULL

=== Deletion Operations ===
Deleted node with value 3
List: 99 -> 5 -> 10 -> 2 -> 1 -> 77 -> NULL

Deleted first node with value 99
List: 5 -> 10 -> 2 -> 1 -> 77 -> NULL
```

### Important Note on Accessing Elements

While insertion and deletion are O(1) when you have the appropriate pointer, accessing an element at a specific position is O(n) because you must traverse from the head. This is a fundamental trade-off: linked lists excel at insertion/deletion but are slower for random access compared to arrays.

---

## Section 5: Variations of Linked Lists

### Doubly Linked Lists

A doubly linked list extends the basic linked list by adding a pointer to the previous node in addition to the next node. This provides bidirectional traversal capability.

```
Doubly Linked List Structure:
               ┌────────────┐
               │Start/Head  │
               └─────┬──────┘
                     │
                     ▼
  NULL ◀───┬─────┬────┬────┐      ┌─────┬────┬────┐      ┌─────┬────┬────┐      ┌─────┬────┬────┐
           │prev │ 10 │next│◀────▶│prev │ 8  │next│◀────▶│prev │ 4  │next│◀────▶│prev │ 2  │next│───▶ NULL
           └─────┴────┴────┘      └─────┴────┴────┘      └─────┴────┴────┘      └─────┴────┴────┘
```

**Advantages of Doubly Linked Lists:**
- Can traverse backwards efficiently
- Deletion of a node is easier because you can access the previous node directly
- Better for certain algorithms that need backward traversal

**Disadvantages:**
- Requires extra memory for the previous pointer
- More complex pointer manipulation during insertion/deletion

### Circular Linked Lists

In a circular linked list, the last node points back to the first node instead of NULL, forming a circle.

```
Circular Singly Linked List:
         ┌──────┐
         │  L   │ (reference point)
         └───┬──┘
             │
             ▼
        ┌─────┬────┐      ┌─────┬────┐      ┌─────┬────┐      ┌─────┬────┐
    ┌──▶│data │next│─────▶│data │next│─────▶│data │next│─────▶│data │next│───┐
    │   │item1│  ● │      │item2│  ● │      │item3│  ● │      │itemN│  ● │   │
    │   └─────┴────┘      └─────┴────┘      └─────┴────┘      └─────┴────┘   │
    │                                                                          │
    └──────────────────────────────────────────────────────────────────────────┘

Circular Doubly Linked List:
         ┌──────┐
         │  L   │
         └───┬──┘
             │
             ▼
   ┌─────┬────┬────┐      ┌─────┬────┬────┐      ┌─────┬────┬────┐      ┌─────┬────┬────┐
┌─▶│prev │item│next│◀────▶│prev │item│next│◀────▶│prev │item│next│◀────▶│prev │item│next│◀─┐
│  │  ●  │  1 │  ● │      │  ●  │  2 │  ● │      │  ●  │  3 │  ● │      │  ●  │  N │  ● │  │
│  └──┬──┴────┴──┬─┘      └─────┴────┴────┘      └─────┴────┴────┘      └──┬──┴────┴──┬─┘  │
│     │          │                                                           │          │    │
│     └──────────┼───────────────────────────────────────────────────────────┘          │    │
└────────────────┴────────────────────────────────────────────────────────────────────────┘
```

**Use Cases for Circular Lists:**
- Round-robin scheduling
- Implementation of circular buffers
- Games where players take turns
- Music/video playlist applications

### Implementation Example

```cpp
#include <iostream>
using namespace std;

// Doubly linked list node
struct DoublyNode {
    int data;
    DoublyNode* prev;
    DoublyNode* next;
    
    DoublyNode(int value) : data(value), prev(nullptr), next(nullptr) {}
};

// Print doubly linked list forward
void printForward(DoublyNode* head) {
    cout << "Forward: NULL <-> ";
    DoublyNode* current = head;
    while (current != nullptr) {
        cout << current->data;
        if (current->next != nullptr) {
            cout << " <-> ";
        }
        current = current->next;
    }
    cout << " <-> NULL" << endl;
}

// Print doubly linked list backward
void printBackward(DoublyNode* tail) {
    cout << "Backward: NULL <-> ";
    DoublyNode* current = tail;
    while (current != nullptr) {
        cout << current->data;
        if (current->prev != nullptr) {
            cout << " <-> ";
        }
        current = current->prev;
    }
    cout << " <-> NULL" << endl;
}

// Insert in doubly linked list after a node
void insertAfterDoubly(DoublyNode* prevNode, int newData) {
    if (prevNode == nullptr) {
        cout << "Previous node cannot be NULL" << endl;
        return;
    }
    
    DoublyNode* newNode = new DoublyNode(newData);
    
    newNode->next = prevNode->next;
    newNode->prev = prevNode;
    prevNode->next = newNode;
    
    if (newNode->next != nullptr) {
        newNode->next->prev = newNode;
    }
    
    cout << "Inserted " << newData << " after " << prevNode->data << " in doubly linked list" << endl;
}

// Circular linked list node (same as regular node)
struct CircularNode {
    int data;
    CircularNode* next;
    
    CircularNode(int value) : data(value), next(nullptr) {}
};

// Print circular linked list
void printCircular(CircularNode* head, int maxNodes = 10) {
    if (head == nullptr) {
        cout << "Empty list" << endl;
        return;
    }
    
    cout << "Circular: ";
    CircularNode* current = head;
    int count = 0;
    
    do {
        cout << current->data;
        current = current->next;
        if (current != head && count < maxNodes) {
            cout << " -> ";
        }
        count++;
    } while (current != head && count < maxNodes);
    
    cout << " -> (back to " << head->data << ")" << endl;
}

int main() {
    // === Doubly Linked List Demo ===
    cout << "=== Doubly Linked List ===" << endl;
    
    DoublyNode* doublyHead = new DoublyNode(10);
    doublyHead->next = new DoublyNode(20);
    doublyHead->next->prev = doublyHead;
    doublyHead->next->next = new DoublyNode(30);
    doublyHead->next->next->prev = doublyHead->next;
    
    // Find tail
    DoublyNode* doublyTail = doublyHead;
    while (doublyTail->next != nullptr) {
        doublyTail = doublyTail->next;
    }
    
    cout << "Initial doubly linked list:" << endl;
    printForward(doublyHead);
    printBackward(doublyTail);
    cout << endl;
    
    // Insert in doubly linked list
    insertAfterDoubly(doublyHead->next, 25);
    
    // Update tail if necessary
    doublyTail = doublyHead;
    while (doublyTail->next != nullptr) {
        doublyTail = doublyTail->next;
    }
    
    cout << "After insertion:" << endl;
    printForward(doublyHead);
    printBackward(doublyTail);
    cout << endl;
    
    // === Circular Linked List Demo ===
    cout << "=== Circular Linked List ===" << endl;
    
    CircularNode* circularHead = new CircularNode(1);
    circularHead->next = new CircularNode(2);
    circularHead->next->next = new CircularNode(3);
    circularHead->next->next->next = new CircularNode(4);
    
    // Make it circular
    circularHead->next->next->next->next = circularHead;
    
    cout << "Circular linked list:" << endl;
    printCircular(circularHead);
    cout << endl;
    
    // Cleanup doubly linked list
    DoublyNode* currentDoubly = doublyHead;
    while (currentDoubly != nullptr) {
        DoublyNode* temp = currentDoubly;
        currentDoubly = currentDoubly->next;
        delete temp;
    }
    
    // Cleanup circular list (must break the circle first)
    CircularNode* currentCircular = circularHead;
    CircularNode* firstNode = circularHead;
    circularHead->next->next->next->next = nullptr;  // Break circle
    
    while (currentCircular != nullptr) {
        CircularNode* temp = currentCircular;
        currentCircular = currentCircular->next;
        delete temp;
    }
    
    return 0;
}
```

**Sample Output:**
```
=== Doubly Linked List ===
Initial doubly linked list:
Forward: NULL <-> 10 <-> 20 <-> 30 <-> NULL
Backward: NULL <-> 30 <-> 20 <-> 10 <-> NULL

Inserted 25 after 20 in doubly linked list
After insertion:
Forward: NULL <-> 10 <-> 20 <-> 25 <-> 30 <-> NULL
Backward: NULL <-> 30 <-> 25 <-> 20 <-> 10 <-> NULL

=== Circular Linked List ===
Circular linked list:
Circular: 1 -> 2 -> 3 -> 4 -> (back to 1)
```

---

## Section 6: The Standard Template Library (STL)

### Introduction to STL

The Standard Template Library (STL) is a powerful collection of template classes and functions that provide common data structures and algorithms. 

Rather than implementing data structures from scratch, we can use well-tested, optimized STL containers.

### Key STL Container Classes

**1. `vector`** - Dynamic array implementation
- Provides random access
- Efficient at the end
- Automatic resizing

**2. `list`** - Doubly linked list implementation
- Efficient insertion/deletion anywhere
- No random access
- Bidirectional iteration

**3. Other Containers** (introduced in C++11 and later):
- `deque` - Double-ended queue
- `stack` - LIFO structure
- `queue` - FIFO structure
- `set` and `map` - Tree-based containers
- `unordered_set` and `unordered_map` - Hash table-based containers

### Why Use STL?

1. **Tested and Optimized**: STL implementations are thoroughly tested and optimized
2. **Type-Safe**: Templates provide compile-time type checking
3. **Consistent Interface**: Similar operations work across different containers
4. **Standard**: Portable across different compilers and platforms
5. **Time-Saving**: No need to reinvent the wheel

### Template Parameters

STL containers use template parameters to specify the data type they store:

```cpp
std::vector<int>        // Vector of integers
std::vector<double>     // Vector of doubles
std::vector<string>     // Vector of strings
std::list<int>          // List of integers
std::list<MyClass>      // List of custom objects
```

The pattern for includes: to use a container named `X`, include `<X>`.

---

## Section 7: Working with STL Containers

Lets explore how to use `std::vector` and `std::list` from the STL with practical examples.

### Using std::vector

The `vector` class provides a dynamic array with automatic memory management and resize capabilities.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // Creating a vector of doubles with initial size 10
    vector<double> arr(10);
    
    cout << "=== Vector Operations ===" << endl;
    cout << "Initial size: " << arr.size() << endl;
    cout << "Initial capacity: " << arr.capacity() << endl << endl;
    
    // Random access (O(1)) - just like arrays
    arr[3] = 4.7;
    arr[0] = 1.2;
    arr[5] = 9.8;
    
    cout << "After setting some values:" << endl;
    cout << "arr[0] = " << arr[0] << endl;
    cout << "arr[3] = " << arr[3] << endl;
    cout << "arr[5] = " << arr[5] << endl;
    cout << "arr[2] = " << arr[2] << " (uninitialized)" << endl << endl;
    
    // Resizing the vector
    arr.resize(24);
    cout << "After resize(24):" << endl;
    cout << "New size: " << arr.size() << endl;
    cout << "New capacity: " << arr.capacity() << endl << endl;
    
    // Setting a value in the newly allocated space
    arr[21] = 100.5;
    cout << "arr[21] = " << arr[21] << endl << endl;
    
    // Using push_back to add elements (automatic resizing)
    vector<int> numbers;
    cout << "Building vector with push_back:" << endl;
    for (int i = 1; i <= 5; i++) {
        numbers.push_back(i * 10);
        cout << "Added " << i * 10 << ", size: " << numbers.size() 
             << ", capacity: " << numbers.capacity() << endl;
    }
    cout << endl;
    
    // Iterating through vector using size()
    cout << "Vector contents using index access:" << endl;
    for (int i = 0; i < numbers.size(); ++i) {
        cout << "numbers[" << i << "] = " << numbers[i] << endl;
    }
    cout << endl;
    
    // Using at() for bounds-checked access
    cout << "Using at() method (bounds-checked):" << endl;
    try {
        cout << "numbers.at(2) = " << numbers.at(2) << endl;
        cout << "numbers.at(100) = " << numbers.at(100) << endl;  // Will throw
    } catch (const out_of_range& e) {
        cout << "Exception caught: " << e.what() << endl;
    }
    cout << endl;
    
    // Other useful vector operations
    cout << "Other operations:" << endl;
    cout << "front() = " << numbers.front() << endl;
    cout << "back() = " << numbers.back() << endl;
    cout << "empty() = " << (numbers.empty() ? "true" : "false") << endl;
    
    numbers.pop_back();
    cout << "After pop_back(), size = " << numbers.size() << endl;
    
    numbers.clear();
    cout << "After clear(), size = " << numbers.size() << endl;
    cout << "After clear(), empty() = " << (numbers.empty() ? "true" : "false") << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Vector Operations ===
Initial size: 10
Initial capacity: 10

After setting some values:
arr[0] = 1.2
arr[3] = 4.7
arr[5] = 9.8
arr[2] = 0 (uninitialized)

After resize(24):
New size: 24
New capacity: 24

arr[21] = 100.5

Building vector with push_back:
Added 10, size: 1, capacity: 1
Added 20, size: 2, capacity: 2
Added 30, size: 3, capacity: 4
Added 40, size: 4, capacity: 4
Added 50, size: 5, capacity: 8

Vector contents using index access:
numbers[0] = 10
numbers[1] = 20
numbers[2] = 30
numbers[3] = 40
numbers[4] = 50

Using at() method (bounds-checked):
numbers.at(2) = 30
Exception caught: vector::_M_range_check: __n (which is 100) >= this->size() (which is 5)

Other operations:
front() = 10
back() = 50
empty() = false
After pop_back(), size = 4
After clear(), size = 0
After clear(), empty() = true
```

### Using std::list

The `list` class provides a doubly linked list implementation with efficient insertion and deletion.

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    cout << "=== List Operations ===" << endl;
    
    // Creating an empty list of doubles
    list<double> listy;
    
    // Inserting at the end (O(1))
    listy.push_back(21.34);
    listy.push_back(13.2);
    listy.push_back(45.67);
    listy.push_back(8.91);
    
    cout << "After push_back operations:" << endl;
    cout << "Size: " << listy.size() << endl << endl;
    
    // Inserting at the beginning (O(1))
    listy.push_front(100.5);
    listy.push_front(200.7);
    
    cout << "After push_front operations:" << endl;
    cout << "Size: " << listy.size() << endl << endl;
    
    // Must use iterator to traverse a list
    cout << "List contents (using iterator):" << endl;
    for (list<double>::iterator it = listy.begin(); it != listy.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl << endl;
    
    // Using range-based for loop (C++11 and later)
    cout << "List contents (using range-based for):" << endl;
    for (double value : listy) {
        cout << value << " ";
    }
    cout << endl << endl;
    
    // Removing elements
    listy.pop_front();  // Remove first element
    listy.pop_back();   // Remove last element
    
    cout << "After pop_front() and pop_back():" << endl;
    for (double value : listy) {
        cout << value << " ";
    }
    cout << endl << endl;
    
    // Inserting in the middle using iterator
    list<double>::iterator it = listy.begin();
    ++it;  // Move to second position
    ++it;  // Move to third position
    listy.insert(it, 999.0);  // Insert before third position
    
    cout << "After inserting 999.0 at third position:" << endl;
    for (double value : listy) {
        cout << value << " ";
    }
    cout << endl << endl;
    
    // Removing specific value
    listy.remove(13.2);  // Remove all occurrences of 13.2
    
    cout << "After removing 13.2:" << endl;
    for (double value : listy) {
        cout << value << " ";
    }
    cout << endl << endl;
    
    // Other useful operations
    cout << "Other operations:" << endl;
    cout << "front() = " << listy.front() << endl;
    cout << "back() = " << listy.back() << endl;
    cout << "empty() = " << (listy.empty() ? "true" : "false") << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== List Operations ===
After push_back operations:
Size: 4

After push_front operations:
Size: 6

List contents (using iterator):
200.7 100.5 21.34 13.2 45.67 8.91 

List contents (using range-based for):
200.7 100.5 21.34 13.2 45.67 8.91 

After pop_front() and pop_back():
100.5 21.34 13.2 45.67 

After inserting 999.0 at third position:
100.5 21.34 999 13.2 45.67 

After removing 13.2:
100.5 21.34 999 45.67 

Other operations:
front() = 100.5
back() = 45.67
empty() = false
```

### Understanding STL Iterators

Iterators are objects that point to elements in a container and allow traversal. They act as a generalized pointer interface.

```cpp
#include <iostream>
#include <vector>
#include <list>
using namespace std;

int main() {
    cout << "=== Iterator Demonstration ===" << endl << endl;
    
    // Vector with iterator
    vector<int> vec = {10, 20, 30, 40, 50};
    
    cout << "Vector traversal with iterator:" << endl;
    for (vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";  // Dereference iterator to get value
    }
    cout << endl << endl;
    
    // List with iterator
    list<int> lst = {5, 10, 15, 20, 25};
    
    cout << "List traversal with iterator:" << endl;
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl << endl;
    
    // Modifying elements through iterator
    cout << "Doubling all vector elements:" << endl;
    for (vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        *it = *it * 2;
    }
    
    for (int value : vec) {
        cout << value << " ";
    }
    cout << endl << endl;
    
    // Using const_iterator for read-only access
    cout << "Using const_iterator (read-only):" << endl;
    for (list<int>::const_iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
        // *it = 100;  // This would cause a compiler error
    }
    cout << endl << endl;
    
    // Reverse iteration
    cout << "Reverse iteration through vector:" << endl;
    for (vector<int>::reverse_iterator it = vec.rbegin(); it != vec.rend(); ++it) {
        cout << *it << " ";
    }
    cout << endl << endl;
    
    // Iterator arithmetic (only for vector/random-access containers)
    cout << "Iterator arithmetic with vector:" << endl;
    vector<int>::iterator it = vec.begin();
    cout << "First element: " << *it << endl;
    
    it += 2;  // Move forward by 2
    cout << "Third element: " << *it << endl;
    
    it = vec.end() - 1;  // Point to last element
    cout << "Last element: " << *it << endl;
    cout << endl;
    
    // Note: List iterators don't support random access arithmetic
    cout << "List iterators support only increment/decrement:" << endl;
    list<int>::iterator lit = lst.begin();
    ++lit;  // OK
    ++lit;  // OK
    // lit += 2;  // Would cause compiler error
    cout << "Element after two increments: " << *lit << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Iterator Demonstration ===

Vector traversal with iterator:
10 20 30 40 50 

List traversal with iterator:
5 10 15 20 25 

Doubling all vector elements:
20 40 60 80 100 

Using const_iterator (read-only):
5 10 15 20 25 

Reverse iteration through vector:
100 80 60 40 20 

Iterator arithmetic with vector:
First element: 20
Third element: 60
Last element: 100

List iterators support only increment/decrement:
Element after two increments: 15
```

### Complete Comparison Program

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
using namespace std;

void demonstrateVectorOperations() {
    cout << "=== VECTOR (Dynamic Array) ===" << endl;
    vector<string> words;
    
    // Add elements
    words.push_back("Hello");
    words.push_back("World");
    words.push_back("from");
    words.push_back("Vector");
    
    cout << "Contents: ";
    for (size_t i = 0; i < words.size(); i++) {
        cout << words[i] << " ";
    }
    cout << endl;
    
    // Random access (O(1))
    cout << "Element at index 2 (O(1)): " << words[2] << endl;
    
    // Modifying element (O(1))
    words[1] = "Universe";
    cout << "After modification: ";
    for (size_t i = 0; i < words.size(); i++) {
        cout << words[i] << " ";
    }
    cout << endl << endl;
}

void demonstrateListOperations() {
    cout << "=== LIST (Doubly Linked List) ===" << endl;
    list<string> words;
    
    // Add elements
    words.push_back("Hello");
    words.push_back("World");
    words.push_back("from");
    words.push_back("List");
    
    cout << "Contents: ";
    for (const string& word : words) {
        cout << word << " ";
    }
    cout << endl;
    
    // Insert in middle (O(1) with iterator)
    list<string>::iterator it = words.begin();
    ++it;  // Move to second position
    words.insert(it, "Beautiful");
    
    cout << "After insertion: ";
    for (const string& word : words) {
        cout << word << " ";
    }
    cout << endl;
    
    // Remove element (O(1) with iterator)
    words.remove("World");
    
    cout << "After removal: ";
    for (const string& word : words) {
        cout << word << " ";
    }
    cout << endl << endl;
}

int main() {
    cout << "STL Container Comparison" << endl;
    cout << "========================" << endl << endl;
    
    demonstrateVectorOperations();
    demonstrateListOperations();
    
    cout << "Key Differences:" << endl;
    cout << "- Vector: Fast random access O(1), slower insert/delete O(n)" << endl;
    cout << "- List: No random access O(n), fast insert/delete O(1)" << endl;
    cout << "- Vector: Contiguous memory, cache-friendly" << endl;
    cout << "- List: Non-contiguous memory, more flexible" << endl;
    
    return 0;
}
```

**Sample Output:**
```
STL Container Comparison
========================

=== VECTOR (Dynamic Array) ===
Contents: Hello World from Vector 
Element at index 2 (O(1)): from
After modification: Hello Universe from Vector 

=== LIST (Doubly Linked List) ===
Contents: Hello World from List 
After insertion: Hello Beautiful World from List 
After removal: Hello Beautiful from List 

Key Differences:
- Vector: Fast random access O(1), slower insert/delete O(n)
- List: No random access O(n), fast insert/delete O(1)
- Vector: Contiguous memory, cache-friendly
- List: Non-contiguous memory, more flexible
```

---

## Section 8: Summary and Comparison

### Performance Comparison Table

| Operation | Array/Vector | Linked List |
|-----------|-------------|-------------|
| Access by index | O(1) | O(n) |
| Insert at beginning | O(n) | O(1) |
| Insert at end | O(1) amortized | O(1) |
| Insert in middle | O(n) | O(1) with pointer |
| Remove at beginning | O(n) | O(1) |
| Remove at end | O(1) | O(1) |
| Remove in middle | O(n) | O(1) with pointer |
| Search unsorted | O(n) | O(n) |
| Memory overhead | Low | High (pointers) |
| Cache performance | Excellent | Poor |

### When to Use Arrays/Vectors

Use arrays or vectors when:
- You need fast random access to elements
- You primarily access elements by index
- The size is relatively stable or grows only at the end
- Memory locality and cache performance are important
- You're iterating through all elements sequentially

### When to Use Linked Lists

Use linked lists when:
- You frequently insert or remove elements in the middle
- You don't need random access
- The size changes frequently in unpredictable ways
- You need to maintain order while frequently adding/removing items
- You're implementing other structures (queues, stacks) that don't require random access

### Memory Visualization Comparison

```
Array Memory Layout:
┌───────────────────────────────────┐
│  Contiguous Memory Block          │
├────┬────┬────┬────┬────┬────┬────┤
│ 10 │ 20 │ 30 │ 40 │ 50 │ 60 │ 70 │
└────┴────┴────┴────┴────┴────┴────┘
Fast sequential access, cache-friendly

Linked List Memory Layout:
┌────┐              ┌────┐         ┌────┐
│ 10 │──────┐   ┌──▶│ 30 │─┐   ┌──▶│ 50 │───▶
└────┘      │   │   └────┘ │   │   └────┘
         ┌────┐ │       ┌────┐ │
      ┌──│ 20 │─┘    ┌──│ 40 │─┘
      │  └────┘      │  └────┘
      │              │
Scattered in memory, pointer overhead
```

### Key Takeaways

1. **Arrays excel at access, lists excel at modification**: Choose based on your primary operation
2. **STL provides both**: Use `std::vector` for arrays and `std::list` for linked lists
3. **Templates enable generic programming**: Write once, use with any data type
4. **Iterators provide uniform interface**: Algorithms can work with different containers
5. **Performance matters**: Understanding Big O complexity helps make informed decisions
6. **Memory layout affects performance**: Contiguous memory is generally faster due to caching
7. **Trade-offs are inevitable**: No single data structure is best for all scenarios

### Complete Practical Example

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <chrono>
using namespace std;
using namespace std::chrono;

// Demonstrate performance difference
void comparePerformance() {
    const int SIZE = 10000;
    
    // Vector performance test
    vector<int> vec;
    auto start = high_resolution_clock::now();
    
    for (int i = 0; i < SIZE; i++) {
        vec.push_back(i);
    }
    
    auto end = high_resolution_clock::now();
    auto vectorPushBack = duration_cast<microseconds>(end - start).count();
    
    // List performance test
    list<int> lst;
    start = high_resolution_clock::now();
    
    for (int i = 0; i < SIZE; i++) {
        lst.push_back(i);
    }
    
    end = high_resolution_clock::now();
    auto listPushBack = duration_cast<microseconds>(end - start).count();
    
    // Random access test (vector only)
    start = high_resolution_clock::now();
    int sum = 0;
    for (int i = 0; i < SIZE; i++) {
        sum += vec[i];
    }
    end = high_resolution_clock::now();
    auto vectorAccess = duration_cast<microseconds>(end - start).count();
    
    // Sequential access test (list)
    start = high_resolution_clock::now();
    sum = 0;
    for (int value : lst) {
        sum += value;
    }
    end = high_resolution_clock::now();
    auto listAccess = duration_cast<microseconds>(end - start).count();
    
    cout << "Performance Comparison (" << SIZE << " elements):" << endl;
    cout << "Vector push_back: " << vectorPushBack << " microseconds" << endl;
    cout << "List push_back: " << listPushBack << " microseconds" << endl;
    cout << "Vector random access: " << vectorAccess << " microseconds" << endl;
    cout << "List sequential access: " << listAccess << " microseconds" << endl;
}

int main() {
    cout << "Arrays vs Linked Lists - Final Comparison" << endl;
    cout << "==========================================" << endl << endl;
    
    comparePerformance();
    
    cout << "\nConclusion:" << endl;
    cout << "- Both structures have their place in programming" << endl;
    cout << "- Choose based on your specific use case" << endl;
    cout << "- STL provides optimized implementations of both" << endl;
    cout << "- Understanding trade-offs is key to efficient code" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Arrays vs Linked Lists - Final Comparison
==========================================

Performance Comparison (10000 elements):
Vector push_back: 245 microseconds
List push_back: 892 microseconds
Vector random access: 12 microseconds
List sequential access: 45 microseconds

Conclusion:
- Both structures have their place in programming
- Choose based on your specific use case
- STL provides optimized implementations of both
- Understanding trade-offs is key to efficient code
```

---

## Resources for Further Study

### Official Documentation

- **C++ Reference**: [https://cppreference.com](https://cppreference.com)
  - Comprehensive documentation for STL containers
  - Detailed information on vectors, lists, and iterators
  
- **CPlusPlus.com STL Reference**: [http://www.cplusplus.com/reference/stl/](http://www.cplusplus.com/reference/stl/)
  - User-friendly STL documentation
  - Examples and tutorials for each container

### Books

- **"The C++ Standard Library: A Tutorial and Reference" by Nicolai M. Josuttis**
  - Comprehensive coverage of STL
  - In-depth explanations of containers and algorithms

- **"Effective STL" by Scott Meyers**
  - Best practices for using STL
  - 50 specific ways to improve your use of the Standard Template Library

- **"Data Structures and Algorithms in C++" by Adam Drozdek**
  - Covers both theory and implementation
  - Bridges the gap between algorithms and C++ STL

### Online Learning Platforms

- **LearnCpp.com**: [https://www.learncpp.com](https://www.learncpp.com)
  - Free comprehensive C++ tutorial
  - Excellent section on arrays and containers

- **GeeksforGeeks**: [https://www.geeksforgeeks.org/data-structures/](https://www.geeksforgeeks.org/data-structures/)
  - Articles on data structures
  - Practice problems with solutions

### Practice and Problem Solving

- **LeetCode**: [https://leetcode.com](https://leetcode.com)
  - Array and linked list problems
  - Helps understand when to use each structure

- **HackerRank**: [https://www.hackerrank.com/domains/data-structures](https://www.hackerrank.com/domains/data-structures)
  - Data structures track
  - C++ specific challenges

- **Codeforces**: [https://codeforces.com](https://codeforces.com)
  - Competitive programming problems
  - Tests understanding of data structure efficiency

### Additional Resources

- **C++ Core Guidelines**: [https://isocpp.github.io/CppCoreGuidelines/](https://isocpp.github.io/CppCoreGuidelines/)
  - Best practices for modern C++
  - Guidelines on container usage

- **Stack Overflow**: [https://stackoverflow.com/questions/tagged/c++](https://stackoverflow.com/questions/tagged/c++)
  - Community-driven Q&A
  - Real-world problems and solutions

- **GitHub C++ Projects**
  - Study open-source C++ projects
  - See how experienced developers use STL containers

---

**End of Lecture**