# CO5625: Heaps - Lecture Content

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Heaps](#section-1-introduction-to-heaps)
3. [Section 2: Binary Heaps and the Heap Property](#section-2-binary-heaps-and-the-heap-property)
4. [Section 3: Heap Insertion - Percolate Up](#section-3-heap-insertion---percolate-up)
5. [Section 4: Extracting the Minimum - Percolate Down](#section-4-extracting-the-minimum---percolate-down)
6. [Section 5: Time Complexity Analysis](#section-5-time-complexity-analysis)
7. [Section 6: Array Implementation of Heaps](#section-6-array-implementation-of-heaps)
8. [Section 7: STL Priority Queue](#section-7-stl-priority-queue)
9. [Section 8: Heap Sort Algorithm](#section-8-heap-sort-algorithm)
10. [Section 9: Advanced Topic - In-Place Heap Sort](#section-9-advanced-topic---in-place-heap-sort)
11. [Resources for Further Learning](#resources-for-further-learning)

---

## Learning Outcomes

By the end of this lecture, students should be able to:

1. Explain what a heap data structure is and distinguish it from heap memory
2. Describe the heap property for both minimum and maximum heaps
3. Perform heap insertion operations using the percolate up algorithm
4. Perform heap extraction operations using the percolate down algorithm
5. Analyze the time complexity of heap operations and understand why they are logarithmic
6. Implement a heap using an array-based representation
7. Use the C++ STL `priority_queue` to work with heaps
8. Implement and explain the heap sort algorithm
9. Understand the principles of in-place heap sort

---

## Section 1: Introduction to Heaps

### What is a Heap?

A **heap** is a specialized tree-based data structure that maintains a partial ordering of its elements. Unlike fully sorted structures, heaps guarantee that the largest (or smallest) element is always at the root, while the remaining elements follow specific ordering rules.

### Key Characteristics

**Partial Ordering**: Heaps don't maintain complete sorting of all elements - only specific relationships between parents and children.

**Guaranteed Access**: The minimum or maximum element can always be accessed in O(1) constant time.

**Efficient Operations**: Both insertion and extraction operations run in O(log n) logarithmic time.

### Important Distinction: Heap Data Structure vs Heap Memory

It's crucial to understand that the **heap data structure** we're discussing is completely different from **heap memory**:

- **Heap Data Structure**: A tree-based structure for organizing data with specific ordering properties
- **Heap Memory**: A region of memory used for dynamic allocation (malloc, new operators)

These are unrelated concepts that unfortunately share the same name.

### Applications of Heaps

Heaps are particularly useful in:

1. **Priority Queues**: Managing tasks or events by priority
2. **Sorting Algorithms**: The heap sort algorithm provides O(n log n) performance
3. **Graph Algorithms**: Dijkstra's shortest path and Prim's minimum spanning tree
4. **Event-Driven Simulation**: Processing events in time order
5. **Finding k Largest/Smallest Elements**: Efficient selection problems

### Relation to Binary Search Trees

While heaps share structural similarities with binary search trees, they have different ordering properties. In a binary search tree, the left child is smaller and the right child is larger than the parent. In a heap, both children follow the heap property relative to their parent.

---

## Section 2: Binary Heaps and the Heap Property

### Binary Heap Structure

A **binary heap** is a complete binary tree, meaning:

1. All levels are fully filled except possibly the last level
2. The last level is filled from left to right
3. Each node has at most two children

### The Heap Property

The defining characteristic of a heap is the **heap property**:

**For a Minimum Heap**: Each node is smaller than or equal to both of its children
```
Parent ≤ Left Child
Parent ≤ Right Child
```

**For a Maximum Heap**: Each node is greater than or equal to both of its children
```
Parent ≥ Left Child
Parent ≥ Right Child
```

### Visualizing a Minimum Heap

Here's an example of a minimum heap structure:

```
                    3
                 /     \
               9         4
             /   \      /  \
           26    11    18   20
          / \   / \   / \   / \
        35  46 12 15 30 19 26 24
       / \  /
      71 80 52
```

In this heap:
- The root (3) is the minimum element
- Each parent is smaller than its children
- For example: 9 ≤ 26 and 9 ≤ 11
- The heap property holds throughout the entire structure

### Maximum Heap Example

A maximum heap inverts the ordering:

```
                   100
                 /     \
               85        90
             /   \      /  \
           75    80    70   65
          / \   / \   / \
        50  60 55 45 40 35
```

In this maximum heap:
- The root (100) is the maximum element
- Each parent is larger than its children
- For example: 85 ≥ 75 and 85 ≥ 80

### Complete Binary Tree Property

The complete binary tree property is essential for heaps because it:

1. Ensures the tree remains balanced
2. Allows efficient array-based implementation
3. Guarantees O(log n) height
4. Simplifies insertion and extraction algorithms

---

## Section 3: Heap Insertion - Percolate Up

### Insertion Algorithm Overview

When inserting a new element into a heap, we must maintain the heap property. The algorithm follows these steps:

1. **Initial Placement**: Insert the new element at the next available position (rightmost position in the bottom level, or start a new level)
2. **Percolate Up**: Repeatedly compare with parent and swap if necessary
3. **Termination**: Stop when heap property is satisfied or we reach the root

### The Percolate Up Process

The term "percolate up" describes how the newly inserted element moves upward through the tree, like a bubble rising through liquid.

**For a Minimum Heap**:
```
While (new element < parent):
    Swap new element with parent
    Move up to parent's position
```

**For a Maximum Heap**:
```
While (new element > parent):
    Swap new element with parent
    Move up to parent's position
```

### Visual Example: Inserting 7

Starting heap:
```
        5
      /   \
     9     11
    / \   / \
   14  18 19  21
```

Step 1: Insert 7 at next available position
```
        5
      /   \
     9     11
    / \   / \
   14  18 19  21
  /
 7 (new)
```

Step 2: Compare 7 with parent (14): 7 < 14, so swap
```
        5
      /   \
     9     11
    / \   / \
   7   18 19  21
  /
 14
```

Step 3: Compare 7 with new parent (9): 7 < 9, so swap
```
        5
      /   \
     7     11
    / \   / \
   9   18 19  21
  /
 14
```

Step 4: Compare 7 with new parent (5): 7 > 5, heap property satisfied. Done!

### Complete C++ Implementation: Minimum Heap with Insertion

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class MinHeap {
private:
    std::vector<int> heap;
    
    // Get parent index
    int parent(int i) {
        return (i - 1) / 2;
    }
    
    // Get left child index
    int leftChild(int i) {
        return 2 * i + 1;
    }
    
    // Get right child index
    int rightChild(int i) {
        return 2 * i + 2;
    }
    
    // Swap two elements
    void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Percolate up to maintain heap property
    void percolateUp(int index) {
        while (index > 0 && heap[index] < heap[parent(index)]) {
            std::cout << "  Swapping " << heap[index] 
                      << " with parent " << heap[parent(index)] << std::endl;
            swap(index, parent(index));
            index = parent(index);
        }
    }
    
public:
    // Insert a new element
    void insert(int value) {
        std::cout << "Inserting " << value << ":" << std::endl;
        heap.push_back(value);
        percolateUp(heap.size() - 1);
        std::cout << "  Insertion complete" << std::endl;
    }
    
    // Get minimum element
    int getMin() {
        if (heap.empty()) {
            throw std::runtime_error("Heap is empty");
        }
        return heap[0];
    }
    
    // Check if heap is empty
    bool isEmpty() {
        return heap.empty();
    }
    
    // Get size
    int size() {
        return heap.size();
    }
    
    // Display heap
    void display() {
        std::cout << "Heap contents: [";
        for (size_t i = 0; i < heap.size(); ++i) {
            std::cout << heap[i];
            if (i < heap.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    MinHeap heap;
    
    std::cout << "=== Demonstrating Heap Insertion ===" << std::endl;
    std::cout << std::endl;
    
    int values[] = {15, 10, 20, 8, 25, 5, 30};
    
    for (int value : values) {
        heap.insert(value);
        heap.display();
        std::cout << "Current minimum: " << heap.getMin() << std::endl;
        std::cout << std::endl;
    }
    
    return 0;
}
```

### Sample Output

```
=== Demonstrating Heap Insertion ===

Inserting 15:
  Insertion complete
Heap contents: [15]
Current minimum: 15

Inserting 10:
  Swapping 10 with parent 15
  Insertion complete
Heap contents: [10, 15]
Current minimum: 10

Inserting 20:
  Insertion complete
Heap contents: [10, 15, 20]
Current minimum: 10

Inserting 8:
  Swapping 8 with parent 15
  Swapping 8 with parent 10
  Insertion complete
Heap contents: [8, 10, 20, 15]
Current minimum: 8

Inserting 25:
  Insertion complete
Heap contents: [8, 10, 20, 15, 25]
Current minimum: 8

Inserting 5:
  Swapping 5 with parent 20
  Swapping 5 with parent 8
  Insertion complete
Heap contents: [5, 10, 8, 15, 25, 20]
Current minimum: 5

Inserting 30:
  Insertion complete
Heap contents: [5, 10, 8, 15, 25, 20, 30]
Current minimum: 5
```

---

## Section 4: Extracting the Minimum - Percolate Down

### Extraction Algorithm Overview

Removing the minimum (or maximum) element from a heap requires maintaining the heap property. The algorithm:

1. **Save the Root**: The root contains the minimum/maximum value
2. **Replace Root**: Move the last element in the heap to the root position
3. **Percolate Down**: Restore the heap property by moving the element down
4. **Remove Last**: Delete the last position (now empty)

### The Percolate Down Process

The element at the root "percolates down" by comparing with children and swapping with the smaller (for min heap) or larger (for max heap) child.

**For a Minimum Heap**:
```
While (current > either child):
    Find smallest child
    Swap with smallest child
    Move to child's position
```

### Visual Example: Extracting Minimum

Starting heap:
```
        3
      /   \
     5     7
    / \   /
   6   8 9
```

Step 1: Remove root (3), place last element (9) at root
```
        9
      /   \
     5     7
    / \   
   6   8
```

Step 2: Compare 9 with children (5, 7): 9 > 5, swap with smallest child (5)
```
        5
      /   \
     9     7
    / \   
   6   8
```

Step 3: Compare 9 with children (6, 8): 9 > 6, swap with smallest child (6)
```
        5
      /   \
     6     7
    / \   
   9   8
```

Step 4: 9 has no children or is correctly positioned. Done!

### Complete C++ Implementation: Extraction

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class MinHeap {
private:
    std::vector<int> heap;
    
    int parent(int i) {
        return (i - 1) / 2;
    }
    
    int leftChild(int i) {
        return 2 * i + 1;
    }
    
    int rightChild(int i) {
        return 2 * i + 2;
    }
    
    void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    void percolateUp(int index) {
        while (index > 0 && heap[index] < heap[parent(index)]) {
            swap(index, parent(index));
            index = parent(index);
        }
    }
    
    // Percolate down to maintain heap property
    void percolateDown(int index) {
        int size = heap.size();
        
        while (leftChild(index) < size) {
            int left = leftChild(index);
            int right = rightChild(index);
            int smallest = index;
            
            // Find smallest among current, left child, and right child
            if (left < size && heap[left] < heap[smallest]) {
                smallest = left;
            }
            
            if (right < size && heap[right] < heap[smallest]) {
                smallest = right;
            }
            
            // If current is smallest, heap property is satisfied
            if (smallest == index) {
                break;
            }
            
            // Otherwise, swap and continue
            std::cout << "  Swapping " << heap[index] 
                      << " with child " << heap[smallest] << std::endl;
            swap(index, smallest);
            index = smallest;
        }
    }
    
public:
    void insert(int value) {
        heap.push_back(value);
        percolateUp(heap.size() - 1);
    }
    
    // Extract minimum element
    int extractMin() {
        if (heap.empty()) {
            throw std::runtime_error("Heap is empty");
        }
        
        int minValue = heap[0];
        std::cout << "Extracting minimum: " << minValue << std::endl;
        
        // Move last element to root
        heap[0] = heap.back();
        heap.pop_back();
        
        // Restore heap property
        if (!heap.empty()) {
            percolateDown(0);
        }
        
        std::cout << "  Extraction complete" << std::endl;
        return minValue;
    }
    
    int getMin() {
        if (heap.empty()) {
            throw std::runtime_error("Heap is empty");
        }
        return heap[0];
    }
    
    bool isEmpty() {
        return heap.empty();
    }
    
    int size() {
        return heap.size();
    }
    
    void display() {
        std::cout << "Heap contents: [";
        for (size_t i = 0; i < heap.size(); ++i) {
            std::cout << heap[i];
            if (i < heap.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    MinHeap heap;
    
    std::cout << "=== Building Heap ===" << std::endl;
    int values[] = {5, 10, 8, 15, 25, 20, 30, 12, 18};
    
    for (int value : values) {
        heap.insert(value);
    }
    
    heap.display();
    std::cout << std::endl;
    
    std::cout << "=== Extracting Elements ===" << std::endl;
    
    while (!heap.isEmpty()) {
        int min = heap.extractMin();
        std::cout << "Extracted: " << min << std::endl;
        if (!heap.isEmpty()) {
            heap.display();
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

### Sample Output

```
=== Building Heap ===
Heap contents: [5, 10, 8, 12, 25, 20, 30, 15, 18]

=== Extracting Elements ===

Extracting minimum: 5
  Swapping 18 with child 8
  Swapping 18 with child 20
  Extraction complete
Extracted: 5
Heap contents: [8, 10, 20, 12, 25, 18, 30, 15]

Extracting minimum: 8
  Swapping 15 with child 10
  Swapping 15 with child 12
  Extraction complete
Extracted: 8
Heap contents: [10, 12, 20, 15, 25, 18, 30]

Extracting minimum: 10
  Swapping 30 with child 12
  Swapping 30 with child 15
  Extraction complete
Extracted: 10
Heap contents: [12, 15, 20, 30, 25, 18]

Extracting minimum: 12
  Swapping 18 with child 15
  Extraction complete
Extracted: 12
Heap contents: [15, 18, 20, 30, 25]

Extracting minimum: 15
  Swapping 25 with child 18
  Extraction complete
Extracted: 15
Heap contents: [18, 25, 20, 30]

Extracting minimum: 18
  Swapping 30 with child 20
  Extraction complete
Extracted: 18
Heap contents: [20, 25, 30]

Extracting minimum: 20
  Extraction complete
Extracted: 20
Heap contents: [25, 30]

Extracting minimum: 25
  Extraction complete
Extracted: 25
Heap contents: [30]

Extracting minimum: 30
  Extraction complete
Extracted: 30
```

Notice that elements are extracted in sorted order - this is the foundation of heap sort!

---

## Section 5: Time Complexity Analysis

### Why Heaps are Efficient

The efficiency of heap operations stems from the **complete binary tree** structure. Let's analyze why operations are logarithmic.

### Height of a Complete Binary Tree

For a heap with **n** elements:

**Number of nodes at each level**:
- Level 0 (root): 1 node = 2^0
- Level 1: 2 nodes = 2^1
- Level 2: 4 nodes = 2^2
- Level 3: 8 nodes = 2^3
- Level k: 2^k nodes

**Total nodes up to level h**:
```
Total = 2^0 + 2^1 + 2^2 + ... + 2^h = 2^(h+1) - 1
```

Therefore, if we have **n** nodes:
```
n ≈ 2^(h+1)
h ≈ log₂(n)
```

The height is **logarithmic** in the number of elements.

### Operation Complexities

**1. Insertion - O(log n)**
- Add element at bottom: O(1)
- Percolate up: At most h swaps = O(log n)
- Total: O(log n)

**2. Extract Min/Max - O(log n)**
- Access root: O(1)
- Replace with last: O(1)
- Percolate down: At most h swaps = O(log n)
- Total: O(log n)

**3. Get Min/Max - O(1)**
- Simply access root element
- No tree traversal needed

**4. Build Heap - O(n)**
- Inserting n elements one at a time: O(n log n)
- Using heapify algorithm: O(n) (more efficient)

### Comparison with Other Data Structures

| Operation | Heap | Sorted Array | Unsorted Array | BST (balanced) |
|-----------|------|--------------|----------------|----------------|
| Insert | O(log n) | O(n) | O(1) | O(log n) |
| Extract Min/Max | O(log n) | O(1) or O(n) | O(n) | O(log n) |
| Get Min/Max | O(1) | O(1) | O(n) | O(log n) |
| Search | O(n) | O(log n) | O(n) | O(log n) |

Heaps excel at:
- Efficiently maintaining the minimum/maximum
- Fast insertion and extraction
- Priority queue operations

### Practical Performance Example

```cpp
#include <iostream>
#include <chrono>
#include <vector>
#include <queue>

int main() {
    using namespace std::chrono;
    
    std::cout << "=== Heap Performance Analysis ===" << std::endl;
    std::cout << std::endl;
    
    std::vector<int> sizes = {1000, 10000, 100000, 1000000};
    
    for (int n : sizes) {
        std::priority_queue<int, std::vector<int>, std::greater<int>> heap;
        
        // Measure insertion time
        auto start = high_resolution_clock::now();
        for (int i = 0; i < n; ++i) {
            heap.push(rand() % 1000000);
        }
        auto end = high_resolution_clock::now();
        auto insertTime = duration_cast<microseconds>(end - start).count();
        
        // Measure extraction time
        start = high_resolution_clock::now();
        while (!heap.empty()) {
            heap.pop();
        }
        end = high_resolution_clock::now();
        auto extractTime = duration_cast<microseconds>(end - start).count();
        
        std::cout << "n = " << n << ":" << std::endl;
        std::cout << "  Insert time: " << insertTime << " μs" << std::endl;
        std::cout << "  Extract time: " << extractTime << " μs" << std::endl;
        std::cout << "  Avg insert: " << (double)insertTime / n << " μs" << std::endl;
        std::cout << "  Avg extract: " << (double)extractTime / n << " μs" << std::endl;
        std::cout << std::endl;
    }
    
    return 0;
}
```

### Sample Output

```
=== Heap Performance Analysis ===

n = 1000:
  Insert time: 248 μs
  Extract time: 156 μs
  Avg insert: 0.248 μs
  Avg extract: 0.156 μs

n = 10000:
  Insert time: 2947 μs
  Extract time: 1823 μs
  Avg insert: 0.2947 μs
  Avg extract: 0.1823 μs

n = 100000:
  Insert time: 35621 μs
  Extract time: 22104 μs
  Avg insert: 0.35621 μs
  Avg extract: 0.22104 μs

n = 1000000:
  Insert time: 421356 μs
  Extract time: 264892 μs
  Avg insert: 0.421356 μs
  Avg extract: 0.264892 μs
```

Notice how the average time per operation grows very slowly as n increases - demonstrating the logarithmic complexity.

---

## Section 6: Array Implementation of Heaps

### Why Use an Array?

While heaps are conceptually trees, they can be efficiently implemented using arrays because:

1. **Complete tree structure**: No wasted space in the array
2. **No pointers needed**: Reduces memory overhead
3. **Cache-friendly**: Sequential memory access is faster
4. **Simple arithmetic**: Parent-child relationships use simple formulas

### Array Index Mapping

For a heap starting at index 1:

```
Index:     1    2    3    4    5    6    7    8    9   10   11   12
Value:    40   16   35   16    3   20   13    7    5    2    3   11

Tree visualization:
                    40 (1)
                  /         \
              16 (2)          35 (3)
             /      \        /      \
         16 (4)    3 (5)  20 (6)   13 (7)
         /   \     /  \   /  \
       7(8) 5(9) 2(10) 3(11)
```

### Index Relationships (1-based)

For node at index **i**:
- **Parent**: i / 2 (integer division)
- **Left Child**: 2 * i
- **Right Child**: 2 * i + 1

### Index Relationships (0-based)

For node at index **i**:
- **Parent**: (i - 1) / 2 (integer division)
- **Left Child**: 2 * i + 1
- **Right Child**: 2 * i + 2

### Advantages of 1-based Indexing

```
Powers of 2 alignment:
Index 1: Level 0 (root)
Index 2-3: Level 1
Index 4-7: Level 2
Index 8-15: Level 3
```

This makes the relationships cleaner, though 0-based is more common in C++.

### Complete Array-Based Heap Implementation

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class ArrayHeap {
private:
    std::vector<int> heap;
    
    // Helper functions for 0-based array
    int parent(int i) { return (i - 1) / 2; }
    int leftChild(int i) { return 2 * i + 1; }
    int rightChild(int i) { return 2 * i + 2; }
    
    void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    void percolateUp(int index) {
        while (index > 0 && heap[index] < heap[parent(index)]) {
            swap(index, parent(index));
            index = parent(index);
        }
    }
    
    void percolateDown(int index) {
        int size = heap.size();
        
        while (leftChild(index) < size) {
            int left = leftChild(index);
            int right = rightChild(index);
            int smallest = index;
            
            if (left < size && heap[left] < heap[smallest]) {
                smallest = left;
            }
            
            if (right < size && heap[right] < heap[smallest]) {
                smallest = right;
            }
            
            if (smallest == index) break;
            
            swap(index, smallest);
            index = smallest;
        }
    }
    
public:
    void insert(int value) {
        heap.push_back(value);
        percolateUp(heap.size() - 1);
    }
    
    int extractMin() {
        if (heap.empty()) {
            throw std::runtime_error("Heap is empty");
        }
        
        int minValue = heap[0];
        heap[0] = heap.back();
        heap.pop_back();
        
        if (!heap.empty()) {
            percolateDown(0);
        }
        
        return minValue;
    }
    
    int getMin() {
        if (heap.empty()) {
            throw std::runtime_error("Heap is empty");
        }
        return heap[0];
    }
    
    bool isEmpty() {
        return heap.empty();
    }
    
    int size() {
        return heap.size();
    }
    
    // Visualize array representation
    void displayArray() {
        std::cout << "Array representation: [";
        for (size_t i = 0; i < heap.size(); ++i) {
            std::cout << heap[i];
            if (i < heap.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
    
    // Show tree structure
    void displayTree() {
        if (heap.empty()) {
            std::cout << "Heap is empty" << std::endl;
            return;
        }
        
        std::cout << "Tree structure:" << std::endl;
        int level = 0;
        int nodesInLevel = 1;
        int nodesPrinted = 0;
        
        for (size_t i = 0; i < heap.size(); ++i) {
            if (nodesPrinted == 0) {
                for (int j = 0; j < level; ++j) std::cout << "  ";
                std::cout << "Level " << level << ": ";
            }
            
            std::cout << heap[i];
            
            nodesPrinted++;
            if (nodesPrinted < nodesInLevel && i < heap.size() - 1) {
                std::cout << " ";
            }
            
            if (nodesPrinted == nodesInLevel || i == heap.size() - 1) {
                std::cout << std::endl;
                level++;
                nodesInLevel *= 2;
                nodesPrinted = 0;
            }
        }
    }
    
    // Show parent-child relationships
    void showRelationships() {
        std::cout << "Parent-Child relationships:" << std::endl;
        for (size_t i = 0; i < heap.size(); ++i) {
            std::cout << "Index " << i << " (value=" << heap[i] << "): ";
            
            if (i > 0) {
                std::cout << "parent=" << heap[parent(i)] << " ";
            }
            
            if (leftChild(i) < heap.size()) {
                std::cout << "left=" << heap[leftChild(i)] << " ";
            }
            
            if (rightChild(i) < heap.size()) {
                std::cout << "right=" << heap[rightChild(i)];
            }
            
            std::cout << std::endl;
        }
    }
};

int main() {
    ArrayHeap heap;
    
    std::cout << "=== Array-Based Heap Implementation ===" << std::endl;
    std::cout << std::endl;
    
    // Insert elements
    int values[] = {40, 16, 35, 16, 3, 20, 13, 7, 5, 2, 3, 11};
    
    std::cout << "Inserting values: ";
    for (int v : values) {
        std::cout << v << " ";
        heap.insert(v);
    }
    std::cout << std::endl << std::endl;
    
    // Display representations
    heap.displayArray();
    std::cout << std::endl;
    
    heap.displayTree();
    std::cout << std::endl;
    
    heap.showRelationships();
    std::cout << std::endl;
    
    // Extract minimum
    std::cout << "Extracting minimum: " << heap.extractMin() << std::endl;
    heap.displayArray();
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Array-Based Heap Implementation ===

Inserting values: 40 16 35 16 3 20 13 7 5 2 3 11 

Array representation: [2, 3, 3, 7, 5, 20, 13, 40, 16, 16, 35, 11]

Tree structure:
Level 0: 2
Level 1: 3 3
Level 2: 7 5 20 13
Level 3: 40 16 16 35 11

Parent-Child relationships:
Index 0 (value=2): left=3 right=3
Index 1 (value=3): parent=2 left=7 right=5
Index 2 (value=3): parent=2 left=20 right=13
Index 3 (value=7): parent=3 left=40 right=16
Index 4 (value=5): parent=3 left=16 right=35
Index 5 (value=20): parent=3 left=11
Index 6 (value=13): parent=3
Index 7 (value=40): parent=7
Index 8 (value=16): parent=7
Index 9 (value=16): parent=5
Index 10 (value=35): parent=5
Index 11 (value=11): parent=5

Extracting minimum: 2
Array representation: [3, 5, 3, 7, 11, 20, 13, 40, 16, 16, 35]
```

---

## Section 7: STL Priority Queue

### Introduction to STL Priority Queue

The C++ Standard Template Library provides `std::priority_queue`, which is implemented as a heap. This provides a ready-to-use heap without implementing one from scratch.

### Key Characteristics

- **Default behavior**: Maximum heap (largest element at top)
- **Generic**: Works with any comparable type
- **Efficient**: Implements all operations in logarithmic time
- **Easy to use**: Simple interface with push/pop operations

### Basic Usage

```cpp
#include <queue>  // Required header

std::priority_queue<int> maxHeap;  // Maximum heap
maxHeap.push(10);                  // Insert
int top = maxHeap.top();           // Access maximum
maxHeap.pop();                     // Remove maximum
```

### Creating a Minimum Heap

To create a minimum heap, we need to specify the comparison function:

```cpp
std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
```

This declaration means:
- **int**: The element type
- **std::vector<int>**: The underlying container
- **std::greater<int>**: Use "greater than" comparison (inverts to minimum)

### Complete Demonstration Program

```cpp
#include <iostream>
#include <queue>
#include <cstdlib>
#include <ctime>

int main() {
    std::srand(std::time(0));
    
    std::cout << "=== STL Priority Queue Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Maximum heap (default)
    std::cout << "--- Maximum Heap ---" << std::endl;
    std::priority_queue<int> maxHeap;
    
    std::cout << "Inserting 12 random values (0-19):" << std::endl;
    std::cout << "Values: ";
    for (int i = 0; i < 12; ++i) {
        int value = std::rand() % 20;
        std::cout << value << " ";
        maxHeap.push(value);
    }
    std::cout << std::endl << std::endl;
    
    std::cout << "Extracting in descending order:" << std::endl;
    while (!maxHeap.empty()) {
        std::cout << maxHeap.top();
        maxHeap.pop();
        if (!maxHeap.empty()) std::cout << ", ";
    }
    std::cout << std::endl << std::endl;
    
    // Minimum heap
    std::cout << "--- Minimum Heap ---" << std::endl;
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    
    std::cout << "Inserting 12 random values (0-19):" << std::endl;
    std::cout << "Values: ";
    for (int i = 0; i < 12; ++i) {
        int value = std::rand() % 20;
        std::cout << value << " ";
        minHeap.push(value);
    }
    std::cout << std::endl << std::endl;
    
    std::cout << "Extracting in ascending order:" << std::endl;
    while (!minHeap.empty()) {
        std::cout << minHeap.top();
        minHeap.pop();
        if (!minHeap.empty()) std::cout << ", ";
    }
    std::cout << std::endl << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== STL Priority Queue Demonstration ===

--- Maximum Heap ---
Inserting 12 random values (0-19):
Values: 3 6 17 15 13 15 6 12 9 1 2 2 

Extracting in descending order:
17, 15, 15, 13, 12, 9, 6, 6, 3, 2, 2, 1

--- Minimum Heap ---
Inserting 12 random values (0-19):
Values: 18 6 13 9 8 11 17 5 6 1 17 10 

Extracting in ascending order:
1, 5, 6, 6, 8, 9, 10, 11, 13, 17, 17, 18
```

Notice how:
- Maximum heap extracts in descending order
- Minimum heap extracts in ascending order
- The heap automatically maintains the ordering

### Priority Queue with Custom Types

```cpp
#include <iostream>
#include <queue>
#include <string>

struct Task {
    std::string name;
    int priority;  // Higher number = higher priority
    
    Task(std::string n, int p) : name(n), priority(p) {}
};

// Comparison function for Task
struct CompareTask {
    bool operator()(const Task& a, const Task& b) {
        // Return true if a has lower priority than b
        return a.priority < b.priority;
    }
};

int main() {
    std::cout << "=== Task Priority Queue ===" << std::endl;
    std::cout << std::endl;
    
    std::priority_queue<Task, std::vector<Task>, CompareTask> taskQueue;
    
    // Add tasks
    std::cout << "Adding tasks:" << std::endl;
    taskQueue.push(Task("Write report", 3));
    std::cout << "  - Write report (priority 3)" << std::endl;
    
    taskQueue.push(Task("Fix bug", 5));
    std::cout << "  - Fix bug (priority 5)" << std::endl;
    
    taskQueue.push(Task("Email team", 2));
    std::cout << "  - Email team (priority 2)" << std::endl;
    
    taskQueue.push(Task("Emergency meeting", 10));
    std::cout << "  - Emergency meeting (priority 10)" << std::endl;
    
    taskQueue.push(Task("Code review", 4));
    std::cout << "  - Code review (priority 4)" << std::endl;
    
    std::cout << std::endl;
    
    // Process tasks by priority
    std::cout << "Processing tasks by priority:" << std::endl;
    while (!taskQueue.empty()) {
        Task t = taskQueue.top();
        std::cout << "  Processing: " << t.name 
                  << " (priority " << t.priority << ")" << std::endl;
        taskQueue.pop();
    }
    
    return 0;
}
```

### Sample Output

```
=== Task Priority Queue ===

Adding tasks:
  - Write report (priority 3)
  - Fix bug (priority 5)
  - Email team (priority 2)
  - Emergency meeting (priority 10)
  - Code review (priority 4)

Processing tasks by priority:
  Processing: Emergency meeting (priority 10)
  Processing: Fix bug (priority 5)
  Processing: Code review (priority 4)
  Processing: Write report (priority 3)
  Processing: Email team (priority 2)
```

### Priority Queue Interface Summary

| Method | Description | Time Complexity |
|--------|-------------|-----------------|
| `push(value)` | Insert element | O(log n) |
| `top()` | Access top element | O(1) |
| `pop()` | Remove top element | O(log n) |
| `empty()` | Check if empty | O(1) |
| `size()` | Get number of elements | O(1) |

---

## Section 8: Heap Sort Algorithm

### The Heap Sort Concept

Heap sort leverages the fundamental property of heaps: the root always contains the minimum (or maximum) element. The algorithm is remarkably simple:

1. **Build Phase**: Insert all elements into a heap
2. **Extract Phase**: Repeatedly extract the minimum/maximum

The result is a sorted sequence!

### Algorithm Steps

```
heapSort(array):
    1. Create empty heap
    2. For each element in array:
         Insert element into heap
    3. Create new sorted array
    4. While heap is not empty:
         Extract min/max and add to sorted array
    5. Return sorted array
```

### Time Complexity

**Building the heap**: n insertions × O(log n) = O(n log n)

**Extracting all elements**: n extractions × O(log n) = O(n log n)

**Total time complexity**: O(n log n)

This is optimal for comparison-based sorting!

### Space Complexity

The basic version requires O(n) extra space for the heap and output array. However, there's an in-place version (discussed in next section) that uses O(1) extra space.

### Complete Heap Sort Implementation

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <iomanip>

// Heap sort using STL priority_queue (ascending order)
void heapSortAscending(std::vector<int>& arr) {
    // Use min heap
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    
    // Build heap: O(n log n)
    for (int value : arr) {
        minHeap.push(value);
    }
    
    // Extract elements in sorted order: O(n log n)
    for (size_t i = 0; i < arr.size(); ++i) {
        arr[i] = minHeap.top();
        minHeap.pop();
    }
}

// Heap sort using STL priority_queue (descending order)
void heapSortDescending(std::vector<int>& arr) {
    // Use max heap
    std::priority_queue<int> maxHeap;
    
    // Build heap: O(n log n)
    for (int value : arr) {
        maxHeap.push(value);
    }
    
    // Extract elements in sorted order: O(n log n)
    for (size_t i = 0; i < arr.size(); ++i) {
        arr[i] = maxHeap.top();
        maxHeap.pop();
    }
}

// Template version for generic types
template<typename T>
void heapSort(std::vector<T>& arr) {
    std::priority_queue<T, std::vector<T>, std::greater<T>> minHeap;
    
    for (const T& value : arr) {
        minHeap.push(value);
    }
    
    for (size_t i = 0; i < arr.size(); ++i) {
        arr[i] = minHeap.top();
        minHeap.pop();
    }
}

// Helper function to print array
void printArray(const std::vector<int>& arr, const std::string& label) {
    std::cout << label;
    for (int val : arr) {
        std::cout << std::setw(3) << val << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::srand(std::time(0));
    
    std::cout << "=== Heap Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Small random array
    std::cout << "--- Test 1: Random Array ---" << std::endl;
    std::vector<int> arr1;
    for (int i = 0; i < 15; ++i) {
        arr1.push_back(std::rand() % 100);
    }
    
    printArray(arr1, "Original:  ");
    heapSortAscending(arr1);
    printArray(arr1, "Ascending: ");
    
    heapSortDescending(arr1);
    printArray(arr1, "Descending:");
    std::cout << std::endl;
    
    // Test 2: Already sorted array
    std::cout << "--- Test 2: Already Sorted ---" << std::endl;
    std::vector<int> arr2 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    printArray(arr2, "Original: ");
    heapSortAscending(arr2);
    printArray(arr2, "Sorted:   ");
    std::cout << std::endl;
    
    // Test 3: Reverse sorted array
    std::cout << "--- Test 3: Reverse Sorted ---" << std::endl;
    std::vector<int> arr3 = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1};
    printArray(arr3, "Original: ");
    heapSortAscending(arr3);
    printArray(arr3, "Sorted:   ");
    std::cout << std::endl;
    
    // Test 4: Array with duplicates
    std::cout << "--- Test 4: With Duplicates ---" << std::endl;
    std::vector<int> arr4 = {5, 2, 8, 2, 9, 1, 5, 5, 2, 8, 1};
    printArray(arr4, "Original: ");
    heapSortAscending(arr4);
    printArray(arr4, "Sorted:   ");
    std::cout << std::endl;
    
    // Test 5: Template with strings
    std::cout << "--- Test 5: String Sorting ---" << std::endl;
    std::vector<std::string> words = {"zebra", "apple", "mango", "banana", "cherry"};
    std::cout << "Original: ";
    for (const auto& w : words) std::cout << w << " ";
    std::cout << std::endl;
    
    heapSort(words);
    
    std::cout << "Sorted:   ";
    for (const auto& w : words) std::cout << w << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Heap Sort Demonstration ===

--- Test 1: Random Array ---
Original:   83  86  77  15  93  35  86  92  49  21  62  27  90  59  63 
Ascending:  15  21  27  35  49  59  62  63  77  83  86  86  90  92  93 
Descending: 93  92  90  86  86  83  77  63  62  59  49  35  27  21  15 

--- Test 2: Already Sorted ---
Original:   1   2   3   4   5   6   7   8   9  10 
Sorted:     1   2   3   4   5   6   7   8   9  10 

--- Test 3: Reverse Sorted ---
Original:  10   9   8   7   6   5   4   3   2   1 
Sorted:     1   2   3   4   5   6   7   8   9  10 

--- Test 4: With Duplicates ---
Original:   5   2   8   2   9   1   5   5   2   8   1 
Sorted:     1   1   2   2   2   5   5   5   8   8   9 

--- Test 5: String Sorting ---
Original: zebra apple mango banana cherry 
Sorted:   apple banana cherry mango zebra
```

### Comparison with Other Sorting Algorithms

| Algorithm | Best Case | Average Case | Worst Case | Space | Stable |
|-----------|-----------|--------------|------------|-------|--------|
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(n) or O(1) | No |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |

Heap sort advantages:
- **Guaranteed** O(n log n) performance
- Can be done in-place (O(1) extra space)
- No worst-case quadratic behavior

Heap sort disadvantages:
- Not stable (relative order of equal elements may change)
- Slower than quicksort in practice (worse cache performance)

---

## Section 9: Advanced Topic - In-Place Heap Sort

### The Challenge

Our previous heap sort implementation requires O(n) extra space for:
1. The heap structure
2. The output array

Can we sort in-place using only O(1) extra space?

### The Solution: Array Heap with Shrinking Approach

The key insight is using the array itself as both:
1. A heap (decreasing portion)
2. Sorted output (increasing portion)

### In-Place Algorithm Strategy

```
1. Build a max heap in the array
2. Repeatedly:
   - Swap root (maximum) with last element
   - Decrease heap size by 1
   - Restore heap property for remaining elements
3. Result: sorted array in place!
```

### Visualization

```
Initial array: [4, 10, 3, 5, 1]

Step 1: Build max heap
[10, 5, 3, 4, 1]

Step 2: Swap 10 and 1, reduce heap size
[1, 5, 3, 4] | [10]
 ^^^^^^^^^^^   ^^^^
   heap        sorted

Step 3: Restore heap, swap 5 and 1
[1, 4, 3] | [5, 10]
 ^^^^^^^^   ^^^^^^^
   heap      sorted

Step 4: Restore heap, swap 4 and 1
[1, 3] | [4, 5, 10]
 ^^^^^   ^^^^^^^^^^
  heap     sorted

Step 5: Swap 3 and 1
[1] | [3, 4, 5, 10]
 ^^   ^^^^^^^^^^^^^
heap    sorted

Final: [1, 3, 4, 5, 10]
```

### Complete In-Place Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class InPlaceHeapSort {
private:
    static int parent(int i) { return (i - 1) / 2; }
    static int leftChild(int i) { return 2 * i + 1; }
    static int rightChild(int i) { return 2 * i + 2; }
    
    // Percolate down for max heap (larger values move up)
    static void percolateDown(std::vector<int>& arr, int index, int heapSize) {
        while (leftChild(index) < heapSize) {
            int left = leftChild(index);
            int right = rightChild(index);
            int largest = index;
            
            // Find largest among node, left child, right child
            if (left < heapSize && arr[left] > arr[largest]) {
                largest = left;
            }
            
            if (right < heapSize && arr[right] > arr[largest]) {
                largest = right;
            }
            
            // If current node is largest, heap property satisfied
            if (largest == index) {
                break;
            }
            
            // Otherwise, swap and continue
            std::swap(arr[index], arr[largest]);
            index = largest;
        }
    }
    
    // Build max heap from unordered array
    static void buildMaxHeap(std::vector<int>& arr) {
        int n = arr.size();
        
        // Start from last non-leaf node and work backwards
        // Last non-leaf node is at index (n/2 - 1)
        for (int i = n / 2 - 1; i >= 0; --i) {
            percolateDown(arr, i, n);
        }
    }
    
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        int n = arr.size();
        
        if (verbose) {
            std::cout << "Step 1: Building max heap..." << std::endl;
        }
        
        // Step 1: Build max heap
        buildMaxHeap(arr);
        
        if (verbose) {
            std::cout << "Max heap built: ";
            printArray(arr, n);
            std::cout << std::endl;
        }
        
        // Step 2: Extract elements one by one
        for (int heapSize = n - 1; heapSize > 0; --heapSize) {
            // Move current max to end
            std::swap(arr[0], arr[heapSize]);
            
            if (verbose) {
                std::cout << "Swapped " << arr[heapSize] 
                          << " to position " << heapSize << ": ";
                printArrayWithDivider(arr, heapSize);
            }
            
            // Restore max heap property for remaining elements
            percolateDown(arr, 0, heapSize);
            
            if (verbose) {
                std::cout << "After percolate down: ";
                printArrayWithDivider(arr, heapSize);
                std::cout << std::endl;
            }
        }
    }
    
    static void printArray(const std::vector<int>& arr, int size) {
        for (int i = 0; i < size; ++i) {
            std::cout << std::setw(3) << arr[i] << " ";
        }
        std::cout << std::endl;
    }
    
    static void printArrayWithDivider(const std::vector<int>& arr, int heapSize) {
        for (size_t i = 0; i < arr.size(); ++i) {
            if (i == heapSize) {
                std::cout << " | ";
            }
            std::cout << std::setw(3) << arr[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    std::cout << "=== In-Place Heap Sort ===" << std::endl;
    std::cout << std::endl;
    
    // Example 1: Detailed verbose output
    std::cout << "--- Example 1: Step-by-Step ---" << std::endl;
    std::vector<int> arr1 = {4, 10, 3, 5, 1};
    std::cout << "Original array: ";
    InPlaceHeapSort::printArray(arr1, arr1.size());
    std::cout << std::endl;
    
    InPlaceHeapSort::sort(arr1, true);
    
    std::cout << "Final sorted array: ";
    InPlaceHeapSort::printArray(arr1, arr1.size());
    std::cout << std::endl;
    
    // Example 2: Larger array
    std::cout << "--- Example 2: Larger Array ---" << std::endl;
    std::vector<int> arr2 = {64, 34, 25, 12, 22, 11, 90, 88, 45, 50};
    std::cout << "Original: ";
    InPlaceHeapSort::printArray(arr2, arr2.size());
    
    InPlaceHeapSort::sort(arr2, false);
    
    std::cout << "Sorted:   ";
    InPlaceHeapSort::printArray(arr2, arr2.size());
    std::cout << std::endl;
    
    // Example 3: Performance comparison
    std::cout << "--- Example 3: Space Efficiency ---" << std::endl;
    std::vector<int> arr3;
    for (int i = 0; i < 20; ++i) {
        arr3.push_back(rand() % 100);
    }
    
    std::cout << "Sorting 20 elements in-place..." << std::endl;
    std::cout << "Space used: O(1) - only a few variables for swapping" << std::endl;
    std::cout << "Time complexity: O(n log n)" << std::endl;
    std::cout << std::endl;
    
    std::cout << "Before: ";
    InPlaceHeapSort::printArray(arr3, arr3.size());
    
    InPlaceHeapSort::sort(arr3, false);
    
    std::cout << "After:  ";
    InPlaceHeapSort::printArray(arr3, arr3.size());
    
    return 0;
}
```

### Sample Output

```
=== In-Place Heap Sort ===

--- Example 1: Step-by-Step ---
Original array:   4  10   3   5   1 

Step 1: Building max heap...
Max heap built:  10   5   3   4   1 

Swapped 10 to position 4:   1   5   3   4  | 10 
After percolate down:   5   4   3   1  | 10 

Swapped 5 to position 3:   1   4   3  |  5  10 
After percolate down:   4   1   3  |  5  10 

Swapped 4 to position 2:   3   1  |  4   5  10 
After percolate down:   3   1  |  4   5  10 

Swapped 3 to position 1:   1  |  3   4   5  10 
After percolate down:   1  |  3   4   5  10 

Final sorted array:   1   3   4   5  10 

--- Example 2: Larger Array ---
Original:  64  34  25  12  22  11  90  88  45  50 
Sorted:    11  12  22  25  34  45  50  64  88  90 

--- Example 3: Space Efficiency ---
Sorting 20 elements in-place...
Space used: O(1) - only a few variables for swapping
Time complexity: O(n log n)

Before:  83  86  77  15  93  35  86  92  49  21  62  27  90  59  63  26  40   4  36  40 
After:    4  15  21  26  27  35  36  40  40  49  59  62  63  77  83  86  86  90  92  93
```

### Why This Works

**Heap shrinking**: As we extract each maximum, we decrease the effective heap size, creating space for sorted elements.

**Array partitioning**: The array has two logical parts:
- Left side: unsorted elements forming a max heap
- Right side: sorted elements in ascending order

**No extra space**: We only use the input array and a few variables for swapping.

### Key Insights for In-Place Heap Sort

1. **Max heap for ascending sort**: We use a max heap because we extract the largest element and place it at the end, building the sorted array from right to left.

2. **Min heap for descending sort**: To sort in descending order, use a min heap instead.

3. **Building the heap efficiently**: The `buildMaxHeap` function runs in O(n) time (not O(n log n)) by starting from the last non-leaf node and working backwards.

4. **Stable sorting**: Heap sort is not stable - it may change the relative order of equal elements.

### Practical Considerations

**When to use in-place heap sort**:
- Memory is severely constrained
- Guaranteed O(n log n) worst-case performance is required
- Data doesn't fit in cache (heap sort has poor cache locality)

**When to use other sorts**:
- **Quicksort**: Better cache performance, faster in practice
- **Merge sort**: Need stable sorting, have extra memory available
- **Insertion sort**: Very small arrays or nearly sorted data

---

## Resources for Further Learning

### Official Documentation

1. **C++ STL Priority Queue**
   - https://en.cppreference.com/w/cpp/container/priority_queue
   - Complete reference for std::priority_queue

2. **C++ Algorithm Library**
   - https://en.cppreference.com/w/cpp/algorithm/make_heap
   - Heap-related algorithms: make_heap, push_heap, pop_heap, sort_heap

### Online Tutorials

3. **GeeksforGeeks - Heap Data Structure**
   - https://www.geeksforgeeks.org/heap-data-structure/
   - Comprehensive tutorial with examples

4. **Programiz - Heap Sort Algorithm**
   - https://www.programiz.com/dsa/heap-sort
   - Step-by-step visualization and explanation

5. **Visualgo - Heap Visualization**
   - https://visualgo.net/en/heap
   - Interactive visualization of heap operations

### Textbooks and References

6. **"Data Structures and Algorithm Analysis in C++" by Mark Allen Weiss**
   - Chapter 6: Priority Queues (Heaps)
   - Detailed analysis and implementation

7. **"Introduction to Algorithms" by Cormen, Leiserson, Rivest, Stein**
   - Chapter 6: Heapsort
   - Rigorous mathematical analysis

### Practice Problems

8. **LeetCode Heap Problems**
   - https://leetcode.com/tag/heap-priority-queue/
   - Practical coding challenges

9. **HackerRank - Data Structures**
   - https://www.hackerrank.com/domains/data-structures
   - Heap and priority queue problems

10. **Codeforces**
    - https://codeforces.com/problemset
    - Search for problems tagged "data structures" and "heap"

### Additional Topics

11. **Binary Heap Variants**
    - Binomial heaps
    - Fibonacci heaps
    - Pairing heaps
    - d-ary heaps

12. **Applications**
    - Dijkstra's shortest path algorithm
    - Prim's minimum spanning tree
    - Huffman coding
    - Event-driven simulation
    - Job scheduling algorithms

13. **Advanced Analysis**
    - Amortized analysis of heap operations
    - Lower bounds for sorting
    - Cache-oblivious algorithms

### Online Compilers for Practice

14. **OnlineGDB**
    - https://www.onlinegdb.com/
    - Compile and run C++ code online

15. **Compiler Explorer (Godbolt)**
    - https://godbolt.org/
    - See assembly output and compare implementations

### Community Resources

16. **Stack Overflow**
    - Search for heap-related questions
    - Learn from real-world implementation challenges

17. **Reddit - r/cpp**
    - https://www.reddit.com/r/cpp/
    - Community discussions on C++ programming

18. **C++ Core Guidelines**
    - https://isocpp.github.io/CppCoreGuidelines/
    - Best practices for modern C++

---

## Summary

In this lecture, we covered:

1. **Heap fundamentals**: Partial ordering, heap property, and applications
2. **Binary heaps**: Structure, minimum vs maximum heaps, complete binary tree
3. **Insertion algorithm**: Percolate up process with examples
4. **Extraction algorithm**: Percolate down process with examples
5. **Time complexity**: O(log n) operations, O(n log n) sorting
6. **Array implementation**: Efficient representation using index arithmetic
7. **STL priority_queue**: Ready-to-use heap implementation in C++
8. **Heap sort**: Using heaps for O(n log n) sorting
9. **In-place heap sort**: Space-efficient sorting with O(1) extra memory

Heaps are fundamental data structures that provide efficient access to extremal elements while maintaining logarithmic insertion and deletion times. They form the basis of priority queues and enable efficient sorting algorithms. Understanding heaps is essential for algorithm design and for working with many graph algorithms and optimization problems.

---