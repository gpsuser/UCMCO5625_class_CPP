# Lecture 11: C++ Heaps Worksheet

## Quiz Section (10-15 minutes)

### Question 1: Heap Fundamentals

Complete the following statement by filling in the blanks using words from the word bank below.

A **________** is a specialized tree-based data structure that maintains a **________** ordering of its elements. Unlike fully sorted structures, heaps guarantee that the **________** or **________** element is always at the root. The heap property states that for a minimum heap, each parent node must be **________** than or equal to both of its children.

**Word Bank:** partial, complete, maximum, minimum, heap, smaller, larger, balanced

---

### Question 2: Insertion Process

Complete the following description of the heap insertion algorithm by filling in the blanks.

When inserting a new element into a heap, we first place it at the next **________** position in the tree. We then perform a **________** operation, where we repeatedly compare the new element with its **________** and swap if necessary. For a minimum heap, we swap when the new element is **________** than its parent. This process continues until we reach the **________** or the heap property is satisfied.

**Word Bank:** smaller, parent, root, percolate up, available, larger, bottom, percolate down

---

### Question 3: Time Complexity

Complete the following statements about heap operation complexities by filling in the blanks.

The time complexity of inserting an element into a heap is **________** because the height of a complete binary tree with n elements is **________**. Extracting the minimum element also takes **________** time. Accessing the minimum element takes **________** time because it is always at the root. Building a heap from n elements using the heapify algorithm takes **________** time.

**Word Bank:** O(log n), O(n), O(1), O(n²), logarithmic, O(n log n)

---

### Question 4: Array Implementation

Complete the following statements about array-based heap implementation by filling in the blanks.

In a 0-based array implementation of a heap, for a node at index **i**, the parent is located at index **________**, the left child is at index **________**, and the right child is at index **________**. This arithmetic relationship works because a heap is a **________** binary tree, which means all levels are fully filled except possibly the **________** level.

**Word Bank:** (i-1)/2, 2*i+1, 2*i+2, i/2, complete, last, balanced, first

---

### Question 5: Multiple Choice - Heap Property

Which of the following arrays represents a valid **maximum heap**?

a) `[10, 8, 9, 6, 5, 7, 4]`
b) `[10, 6, 8, 5, 9, 7, 4]`
c) `[4, 6, 5, 8, 7, 9, 10]`
d) `[10, 9, 8, 7, 6, 5, 4]`

---

### Question 6: Multiple Choice - Complete Binary Tree

A complete binary tree with 15 nodes has what height?

a) 2
b) 3
c) 4
d) 5

Consider the following visualization:
```
       Level 0: 1 node
       Level 1: 2 nodes
       Level 2: 4 nodes
       Level 3: 8 nodes
```

---

### Question 7: Multiple Choice - Heap Sort

Which statement about heap sort is **FALSE**?

a) Heap sort has a worst-case time complexity of O(n log n)
b) Heap sort can be implemented in-place with O(1) extra space
c) Heap sort is a stable sorting algorithm
d) Heap sort uses either a min heap or max heap to sort elements

---

## Task Section

### Task 1: Validate Min Heap (5-10 minutes)

Write a function `isValidMinHeap()` that checks whether a given vector represents a valid minimum heap. The function should return `true` if the array satisfies the min heap property, and `false` otherwise.

**Requirements:**
- Check that every parent is less than or equal to both its children
- Handle arrays of any size, including empty arrays
- Use the array index relationships for heaps

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>

bool isValidMinHeap(const std::vector<int>& arr) {
    // TODO: Implement this function
    // Hint: Start from index 0 and check each node that has children
    // Hint: Left child of index i is at 2*i+1, right child at 2*i+2
    
}

int main() {
    // Test case 1: Valid min heap
    std::vector<int> heap1 = {1, 3, 2, 7, 5, 8, 9};
    std::cout << "Test 1: " << (isValidMinHeap(heap1) ? "PASS" : "FAIL") << std::endl;
    
    // Test case 2: Invalid min heap
    std::vector<int> heap2 = {1, 5, 2, 7, 3, 8, 9};
    std::cout << "Test 2: " << (isValidMinHeap(heap2) ? "FAIL" : "PASS") << std::endl;
    
    // Test case 3: Empty heap
    std::vector<int> heap3 = {};
    std::cout << "Test 3: " << (isValidMinHeap(heap3) ? "PASS" : "FAIL") << std::endl;
    
    // Test case 4: Single element
    std::vector<int> heap4 = {42};
    std::cout << "Test 4: " << (isValidMinHeap(heap4) ? "PASS" : "FAIL") << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Test 1: PASS
Test 2: PASS
Test 3: PASS
Test 4: PASS
```

---

### Task 2: Find Kth Smallest Element (5-10 minutes)

Write a function `findKthSmallest()` that finds the kth smallest element in an unsorted vector using a max heap. Use the STL `priority_queue` to implement this efficiently.

**Strategy:** Maintain a max heap of size k. For each element:
- If heap size < k, insert the element
- If element < heap top, remove top and insert element
- The top of the heap after processing all elements is the kth smallest

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <queue>

int findKthSmallest(const std::vector<int>& arr, int k) {
    // TODO: Implement using a max heap (priority_queue)
    // Hint: std::priority_queue<int> creates a max heap
    // Hint: Maintain heap of size k, top element is kth smallest
    
}

int main() {
    std::vector<int> data = {7, 10, 4, 3, 20, 15, 8, 2};
    
    std::cout << "Array: ";
    for (int val : data) std::cout << val << " ";
    std::cout << std::endl << std::endl;
    
    for (int k = 1; k <= 5; ++k) {
        std::cout << k << "th smallest: " << findKthSmallest(data, k) << std::endl;
    }
    
    return 0;
}
```

**Expected Output:**
```
Array: 7 10 4 3 20 15 8 2 

1th smallest: 2
2th smallest: 3
3th smallest: 4
4th smallest: 7
5th smallest: 8
```

---

### Challenge Task: Merge K Sorted Arrays (10-15 minutes)

Write a function `mergeKSortedArrays()` that merges k sorted arrays into a single sorted array using a min heap. This is an efficient solution that processes elements in sorted order.

**Strategy:**
- Create a min heap containing the first element from each array along with array/index information
- Repeatedly extract the minimum and insert the next element from that array
- Continue until all elements are processed

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <queue>

// Structure to hold element info in the heap
struct HeapNode {
    int value;          // The actual value
    int arrayIndex;     // Which array it came from
    int elementIndex;   // Position in that array
    
    // Constructor
    HeapNode(int v, int a, int e) : value(v), arrayIndex(a), elementIndex(e) {}
};

// Comparison function for min heap
struct CompareNode {
    bool operator()(const HeapNode& a, const HeapNode& b) {
        return a.value > b.value;  // Min heap: return true if a > b
    }
};

std::vector<int> mergeKSortedArrays(const std::vector<std::vector<int>>& arrays) {
    std::vector<int> result;
    
    // TODO: Create a min heap using priority_queue
    // std::priority_queue<HeapNode, std::vector<HeapNode>, CompareNode> minHeap;
    
    // TODO: Insert first element from each array into heap
    
    // TODO: While heap is not empty:
    //   1. Extract minimum
    //   2. Add to result
    //   3. If that array has more elements, insert next element
    
    return result;
}

int main() {
    // Test with 3 sorted arrays
    std::vector<std::vector<int>> arrays = {
        {1, 4, 7, 10},
        {2, 5, 8, 11},
        {3, 6, 9, 12}
    };
    
    std::cout << "Input arrays:" << std::endl;
    for (size_t i = 0; i < arrays.size(); ++i) {
        std::cout << "Array " << i << ": ";
        for (int val : arrays[i]) {
            std::cout << val << " ";
        }
        std::cout << std::endl;
    }
    std::cout << std::endl;
    
    std::vector<int> merged = mergeKSortedArrays(arrays);
    
    std::cout << "Merged array: ";
    for (int val : merged) {
        std::cout << val << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Input arrays:
Array 0: 1 4 7 10 
Array 1: 2 5 8 11 
Array 2: 3 6 9 12 

Merged array: 1 2 3 4 5 6 7 8 9 10 11 12
```

**Bonus Challenge:** Analyze the time and space complexity of your solution. How does it compare to a naive approach that repeatedly finds the minimum across all arrays?

---
