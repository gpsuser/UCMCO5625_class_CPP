# Lecture 6: C++ Worksheet

## Part 1: Quiz Questions

### Question 1

Complete the following statement about array memory layout:

Arrays store elements in __________ memory locations, which means all elements are placed __________ to each other in memory. This layout allows for __________ time access to any element, making the access complexity __________. The memory address of any element can be calculated using the formula: base_address + (index × __________).

**Word Bank:** O(1), element_size, next, constant, contiguous, random, scattered, O(n)

---

### Question 2

Fill in the blanks about linked list node structure:

A linked list node contains two main components: the __________ field which holds the actual value, and a __________ field which stores the address of the __________ node in the sequence. The first node is accessed through a __________ pointer, and the last node's pointer field points to __________ to indicate the end of the list.

**Word Bank:** NULL, data, next, previous, head, tail, pointer, current

---

### Question 3

Complete the statement about insertion operations:

In a linked list, inserting a new node when you have a pointer to the __________ node is an __________ operation. However, inserting an element in the middle of an array requires __________ all subsequent elements, making it an __________ operation where n is the number of elements that need to be moved.

**Word Bank:** O(n), shifting, previous, O(1), next, copying, O(log n), current

---

### Question 4

Fill in the blanks about STL containers:

The `std::vector` container provides a dynamic __________ implementation with automatic __________ management. Elements can be accessed using the __________ operator for random access, or the `at()` method which provides __________ checking. The `std::list` container implements a __________ linked list, which requires __________ for traversal rather than index-based access.

**Word Bank:** array, bounds, singly, memory, subscript, iterators, doubly, pointers, resize

---

### Question 5

**Multiple Choice:** What is the time complexity of accessing an element at a specific index in a linked list?

A) O(1) - constant time  
B) O(log n) - logarithmic time  
C) O(n) - linear time  
D) O(n²) - quadratic time

---

### Question 6

**Multiple Choice:** Which of the following statements about memory overhead is TRUE?

A) Arrays have higher memory overhead than linked lists because they need contiguous space  
B) Linked lists have higher memory overhead because each node stores additional pointer(s)  
C) Both structures have identical memory overhead  
D) Memory overhead depends only on the data type being stored, not the structure

---

### Question 7

**Multiple Choice:** When would you prefer using a `std::list` over a `std::vector`?

A) When you need frequent random access to elements by index  
B) When you need to maximize cache performance  
C) When you frequently insert and delete elements in the middle of the collection  
D) When memory usage needs to be minimized

---

## Part 2: Programming Tasks

### Task 1: Array Rotation

Write a program that rotates an array to the right by a specified number of positions. For example, rotating `[1, 2, 3, 4, 5]` by 2 positions should result in `[4, 5, 1, 2, 3]`.

**Requirements:**
- Use a static array or `std::vector`
- Implement the rotation in-place (without creating a new array)
- Handle rotation values larger than the array size

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>
using namespace std;

void rotateArray(vector<int>& arr, int positions) {
    // TODO: Implement rotation logic
    
}

void printArray(const vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8};
    
    cout << "Original array: ";
    printArray(numbers);
    
    int rotateBy = 3;
    rotateArray(numbers, rotateBy);
    
    cout << "After rotating by " << rotateBy << " positions: ";
    printArray(numbers);
    
    return 0;
}
```

**Sample Input/Output:**

```
Original array: 1 2 3 4 5 6 7 8 
After rotating by 3 positions: 6 7 8 1 2 3 4 5
```

---

### Task 2: Linked List Reversal

Write a program that reverses a singly linked list. The reversal should be done by changing the pointers, not by swapping data values.

**Requirements:**
- Implement the reversal by manipulating pointers
- Do not use extra space for another list
- Handle empty lists and single-node lists

**Code Scaffolding:**

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
    while (current != nullptr) {
        cout << current->data;
        if (current->next != nullptr) {
            cout << " -> ";
        }
        current = current->next;
    }
    cout << " -> NULL" << endl;
}

Node* reverseList(Node* head) {
    // TODO: Implement reversal logic
    
}

int main() {
    // Create list: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->next = new Node(3);
    head->next->next->next = new Node(4);
    head->next->next->next->next = new Node(5);
    
    cout << "Original list: ";
    printList(head);
    
    head = reverseList(head);
    
    cout << "Reversed list: ";
    printList(head);
    
    // Cleanup
    Node* current = head;
    while (current != nullptr) {
        Node* temp = current;
        current = current->next;
        delete temp;
    }
    
    return 0;
}
```

**Sample Input/Output:**

```
Original list: 1 -> 2 -> 3 -> 4 -> 5 -> NULL
Reversed list: 5 -> 4 -> 3 -> 2 -> 1 -> NULL
```

---

**End of Worksheet**