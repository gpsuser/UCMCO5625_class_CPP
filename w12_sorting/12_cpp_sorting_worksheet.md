# Lecture 12: C++ Worksheet

## Quiz Section

**Time Allocation: 10-15 minutes**

---

### Question 1

Complete the following statement by filling in the blanks with words from the word bank below:

"__________ Sort has a time complexity of O(n²) in all cases, but it minimizes the number of __________ operations. This makes it useful when __________ operations are particularly expensive, even though it performs many __________ operations."

**Word Bank:** `swaps`, `comparisons`, `Selection`, `write`, `Insertion`, `memory`

---

### Question 2

Complete the following statement about sorting stability:

"A __________ sorting algorithm maintains the __________ order of elements with equal keys. For example, __________ Sort and __________ Sort are stable, while __________ Sort and Quicksort are not."

**Word Bank:** `stable`, `Merge`, `relative`, `Selection`, `Insertion`, `absolute`, `Bubble`

---

### Question 3

Fill in the blanks describing the Merge Sort process:

"Merge Sort uses a __________ strategy. It recursively __________ the array into halves until each subarray contains __________ element(s). Then it __________ the sorted subarrays back together. The time complexity is O(n log n) because there are __________ levels and each level processes __________ elements."

**Word Bank:** `divides`, `log n`, `merges`, `one`, `divide-and-conquer`, `n`, `two`, `combines`

---

### Question 4

Complete this statement about Quicksort:

"In Quicksort, the __________ operation rearranges elements so that all elements __________ than the pivot come before it, and all elements __________ than the pivot come after it. The pivot selection strategy affects performance - choosing the __________ element on already sorted data leads to __________ case performance of O(n²)."

**Word Bank:** `partition`, `greater`, `smaller`, `worst`, `median`, `last`, `first`, `best`

---

### Question 5

Which of the following statements about Insertion Sort is TRUE?

A) Insertion Sort always performs better than Merge Sort regardless of input  
B) Insertion Sort has O(n) best-case time complexity on already sorted data  
C) Insertion Sort requires O(n) additional space for the sorting process  
D) Insertion Sort is not stable and may reorder equal elements  

---

### Question 6

What is the primary advantage of Radix Sort over comparison-based sorting algorithms?

A) It uses less memory than any other sorting algorithm  
B) It can achieve O(n) time complexity for fixed-width integers  
C) It works on any data type including floating-point numbers  
D) It has better worst-case performance than Quicksort on all inputs  

---

### Question 7

Consider sorting an array of 1000 elements that is "nearly sorted" (95% of elements are already in correct positions). Which algorithm would likely perform BEST?

A) Selection Sort - because it always performs consistently  
B) Merge Sort - because it guarantees O(n log n) time  
C) Insertion Sort - because it's adaptive to nearly sorted data  
D) Radix Sort - because it's the fastest for all integer data  

---

## Task Section

**Time Allocation: 30-40 minutes total**

---

### Task 1: Counting Inversions (5-10 minutes)

An **inversion** in an array is a pair of indices (i, j) where i < j but arr[i] > arr[j]. The number of inversions indicates how far the array is from being sorted. For example:
- Array [1, 2, 3] has 0 inversions (already sorted)
- Array [3, 2, 1] has 3 inversions: (0,1), (0,2), (1,2)
- Array [2, 4, 1, 3] has 3 inversions: (0,2), (1,2), (1,3)

Implement a function that counts the number of inversions in an array.

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>

class InversionCounter {
public:
    static int countInversions(const std::vector<int>& arr) {
        int count = 0;
        
        // TODO: Implement your solution here
        // Hint: Use nested loops to compare each pair of elements
        
        return count;
    }
};

int main() {
    std::vector<int> test1 = {1, 2, 3, 4};
    std::vector<int> test2 = {4, 3, 2, 1};
    std::vector<int> test3 = {2, 4, 1, 3, 5};
    
    std::cout << "Inversions in [1,2,3,4]: " 
              << InversionCounter::countInversions(test1) << std::endl;
    std::cout << "Inversions in [4,3,2,1]: " 
              << InversionCounter::countInversions(test2) << std::endl;
    std::cout << "Inversions in [2,4,1,3,5]: " 
              << InversionCounter::countInversions(test3) << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Inversions in [1,2,3,4]: 0
Inversions in [4,3,2,1]: 6
Inversions in [2,4,1,3,5]: 3
```

---

### Task 2: Selection Sort with Position Tracking (5-10 minutes)

Implement a modified Selection Sort that prints the position where each element ends up after being selected. This helps visualize how Selection Sort moves elements to their final positions.

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class TrackedSelectionSort {
public:
    static void sort(std::vector<int>& arr) {
        int n = arr.size();
        
        std::cout << "Initial array: ";
        printArray(arr);
        std::cout << std::endl;
        
        for (int i = 0; i < n - 1; ++i) {
            // TODO: Find minimum element in unsorted portion
            
            // TODO: Swap if needed
            
            // TODO: Print which element was placed and where
            // Format: "Placed X at position Y"
            
            std::cout << "After pass " << (i + 1) << ": ";
            printArray(arr);
        }
    }
    
private:
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    std::vector<int> arr = {64, 25, 12, 22, 11};
    TrackedSelectionSort::sort(arr);
    return 0;
}
```

**Expected Output:**
```
Initial array: [64, 25, 12, 22, 11]

Placed 11 at position 0
After pass 1: [11, 25, 12, 22, 64]
Placed 12 at position 1
After pass 2: [11, 12, 25, 22, 64]
Placed 22 at position 2
After pass 3: [11, 12, 22, 25, 64]
Placed 25 at position 3
After pass 4: [11, 12, 22, 25, 64]
```

---

### Task 3: CHALLENGE - Hybrid Sort Implementation (10-15 minutes)

Modern sorting algorithms often use a **hybrid approach**: they use Insertion Sort for small subarrays and Quicksort for larger ones. This is because Insertion Sort has less overhead and performs well on small arrays.

Implement a hybrid sorting algorithm that:
- Uses Insertion Sort when the subarray size is ≤ 10 elements
- Uses Quicksort for larger subarrays

**Code Scaffolding:**

```cpp
#include <iostream>
#include <vector>

class HybridSort {
public:
    static void sort(std::vector<int>& arr) {
        hybridSortRecursive(arr, 0, arr.size() - 1);
    }
    
private:
    static const int INSERTION_THRESHOLD = 10;
    
    static void hybridSortRecursive(std::vector<int>& arr, int low, int high) {
        if (low < high) {
            // TODO: Check if subarray is small enough for insertion sort
            if (high - low + 1 <= INSERTION_THRESHOLD) {
                // TODO: Use insertion sort for small subarrays
                insertionSort(arr, low, high);
            } else {
                // TODO: Use quicksort partitioning for larger subarrays
                int pivotIndex = partition(arr, low, high);
                hybridSortRecursive(arr, low, pivotIndex - 1);
                hybridSortRecursive(arr, pivotIndex + 1, high);
            }
        }
    }
    
    static void insertionSort(std::vector<int>& arr, int low, int high) {
        // TODO: Implement insertion sort for the range [low, high]
        // Hint: Adapt standard insertion sort to work on a subarray
        
    }
    
    static int partition(std::vector<int>& arr, int low, int high) {
        // TODO: Implement standard quicksort partition
        // Hint: Use last element as pivot
        
        return 0; // Replace with actual pivot index
    }
};

int main() {
    // Test with small array (should use insertion sort)
    std::vector<int> small = {5, 2, 8, 1, 9};
    std::cout << "Small array before: ";
    for (int x : small) std::cout << x << " ";
    std::cout << std::endl;
    
    HybridSort::sort(small);
    
    std::cout << "Small array after:  ";
    for (int x : small) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    // Test with larger array (should use hybrid approach)
    std::vector<int> large = {64, 34, 25, 12, 22, 11, 90, 88, 45, 50, 
                              23, 67, 33, 19, 77, 55, 28, 91, 14, 8};
    std::cout << "Large array before: ";
    for (int x : large) std::cout << x << " ";
    std::cout << std::endl;
    
    HybridSort::sort(large);
    
    std::cout << "Large array after:  ";
    for (int x : large) std::cout << x << " ";
    std::cout << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Small array before: 5 2 8 1 9 
Small array after:  1 2 5 8 9 

Large array before: 64 34 25 12 22 11 90 88 45 50 23 67 33 19 77 55 28 91 14 8 
Large array after:  8 11 12 14 19 22 23 25 28 33 34 45 50 55 64 67 77 88 90 91
```

---
