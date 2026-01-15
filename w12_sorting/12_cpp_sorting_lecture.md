# CO5625: Sorting Algorithms - Lecture Content

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Sorting](#section-1-introduction-to-sorting)
3. [Section 2: Selection Sort](#section-2-selection-sort)
4. [Section 3: Insertion Sort](#section-3-insertion-sort)
5. [Section 4: Bubble Sort](#section-4-bubble-sort)
6. [Section 5: Merge Sort](#section-5-merge-sort)
7. [Section 6: Quicksort](#section-6-quicksort)
8. [Section 7: Radix Sort](#section-7-radix-sort)
9. [Section 8: Comparative Analysis](#section-8-comparative-analysis)
10. [Resources for Further Learning](#resources-for-further-learning)

---

## Learning Outcomes

By the end of this lecture, students should be able to:

1. Explain the fundamental concept of sorting and why it is important in computer science
2. Implement and analyze Selection Sort, understanding its O(n²) time complexity
3. Implement and analyze Insertion Sort, recognizing when it performs well
4. Implement and analyze Bubble Sort and explain its inefficiency
5. Understand the divide-and-conquer approach used in Merge Sort
6. Implement Merge Sort and explain why it achieves O(n log n) complexity
7. Implement Quicksort and understand its average and worst-case performance
8. Understand Radix Sort as a non-comparison based sorting algorithm
9. Compare different sorting algorithms and select appropriate ones for specific scenarios
10. Analyze the time and space complexity of each sorting algorithm

---

## Section 1: Introduction to Sorting

### What is Sorting?

**Sorting** is the process of arranging elements in a specific order, typically ascending or descending. It is one of the most fundamental operations in computer science and forms the basis for many other algorithms and applications.

### Why is Sorting Important?

Sorting enables:

1. **Efficient Searching**: Binary search requires sorted data and runs in O(log n) time
2. **Data Organization**: Makes data easier to understand and analyze
3. **Optimization**: Many algorithms work faster on sorted data
4. **Database Operations**: SQL ORDER BY, indexing, and query optimization
5. **Duplicate Detection**: Finding duplicates is easier in sorted data
6. **Statistical Analysis**: Finding median, mode, percentiles

### Classification of Sorting Algorithms

Sorting algorithms can be classified by several characteristics:

**By Time Complexity**:
- Simple sorts: O(n²) - Selection, Insertion, Bubble
- Efficient sorts: O(n log n) - Merge, Quicksort, Heap
- Special case sorts: O(n) - Radix, Counting, Bucket

**By Comparison Type**:
- Comparison-based: Selection, Insertion, Bubble, Merge, Quicksort
- Non-comparison-based: Radix, Counting, Bucket

**By Memory Usage**:
- In-place: O(1) extra space - Selection, Insertion, Bubble, Quicksort
- Not in-place: O(n) extra space - Merge Sort

**By Stability**:
- Stable: Maintains relative order of equal elements - Insertion, Merge, Bubble
- Unstable: May change relative order - Selection, Quicksort, Heap

### Stability Example

Consider sorting student records by grade:

```
Before: [(Alice, 85), (Bob, 90), (Carol, 85), (Dave, 90)]
After (stable): [(Alice, 85), (Carol, 85), (Bob, 90), (Dave, 90)]
After (unstable): [(Carol, 85), (Alice, 85), (Dave, 90), (Bob, 90)]
```

A stable sort preserves the original order of Alice before Carol and Bob before Dave.

### Measuring Performance

When analyzing sorting algorithms, we consider:

1. **Time Complexity**: Number of operations (comparisons, swaps)
2. **Space Complexity**: Extra memory required
3. **Best/Average/Worst Case**: Performance under different input conditions
4. **Adaptive**: Performance on nearly-sorted data
5. **Online**: Ability to sort data as it arrives

---

## Section 2: Selection Sort

### Algorithm Overview

Selection Sort works by repeatedly finding the minimum element from the unsorted portion and placing it at the beginning. After i iterations, the first i items are in their final sorted positions.

### Algorithm Steps

```
For i = 0 to n-1:
    Find minimum element in arr[i...n-1]
    Swap it with arr[i]
```

### Visual Example

```
Initial: [64, 25, 12, 22, 11]

Pass 1: Find minimum (11), swap with position 0
        [11, 25, 12, 22, 64]
         ^^  sorted region

Pass 2: Find minimum (12), swap with position 1
        [11, 12, 25, 22, 64]
         ^^^^^^  sorted region

Pass 3: Find minimum (22), swap with position 2
        [11, 12, 22, 25, 64]
         ^^^^^^^^^^  sorted region

Pass 4: Find minimum (25), swap with position 3
        [11, 12, 22, 25, 64]
         ^^^^^^^^^^^^^^  sorted region

Pass 5: One element left, already sorted
        [11, 12, 22, 25, 64]
         ^^^^^^^^^^^^^^^^^^  sorted
```

### Time Complexity Analysis

**Comparisons**:
- Pass 1: n-1 comparisons
- Pass 2: n-2 comparisons
- Pass 3: n-3 comparisons
- ...
- Pass n-1: 1 comparison

Total: (n-1) + (n-2) + ... + 1 = n(n-1)/2 = **O(n²)**

**Space Complexity**: O(1) - in-place sorting

**Characteristics**:
- Not adaptive: Always O(n²) even on sorted data
- Not stable: Equal elements may be reordered
- Performs well: When writes are expensive (minimizes swaps)

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class SelectionSort {
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        int n = arr.size();
        int comparisons = 0;
        int swaps = 0;
        
        for (int i = 0; i < n - 1; ++i) {
            // Find minimum element in unsorted portion
            int minIndex = i;
            
            for (int j = i + 1; j < n; ++j) {
                comparisons++;
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            
            // Swap minimum with current position
            if (minIndex != i) {
                std::swap(arr[i], arr[minIndex]);
                swaps++;
                
                if (verbose) {
                    std::cout << "Pass " << (i + 1) << ": Swapped " 
                              << arr[i] << " with position " << i << std::endl;
                    printArray(arr);
                }
            }
        }
        
        if (verbose) {
            std::cout << "\nTotal comparisons: " << comparisons << std::endl;
            std::cout << "Total swaps: " << swaps << std::endl;
        }
    }
    
private:
    static void printArray(const std::vector<int>& arr) {
        std::cout << "  [";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << std::setw(3) << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    std::cout << "=== Selection Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Small array with verbose output
    std::cout << "--- Test 1: Detailed Steps ---" << std::endl;
    std::vector<int> arr1 = {64, 25, 12, 22, 11};
    
    std::cout << "Original array:" << std::endl;
    std::cout << "  [";
    for (size_t i = 0; i < arr1.size(); ++i) {
        std::cout << std::setw(3) << arr1[i];
        if (i < arr1.size() - 1) std::cout << ", ";
    }
    std::cout << "]" << std::endl << std::endl;
    
    SelectionSort::sort(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Already sorted array
    std::cout << "--- Test 2: Already Sorted ---" << std::endl;
    std::vector<int> arr2 = {1, 2, 3, 4, 5};
    std::cout << "Original: ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl;
    
    SelectionSort::sort(arr2, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    // Test 3: Reverse sorted array
    std::cout << "--- Test 3: Reverse Sorted ---" << std::endl;
    std::vector<int> arr3 = {5, 4, 3, 2, 1};
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    SelectionSort::sort(arr3, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Selection Sort Demonstration ===

--- Test 1: Detailed Steps ---
Original array:
  [ 64,  25,  12,  22,  11]

Pass 1: Swapped 11 with position 0
  [ 11,  25,  12,  22,  64]
Pass 2: Swapped 12 with position 1
  [ 11,  12,  25,  22,  64]
Pass 3: Swapped 22 with position 2
  [ 11,  12,  22,  25,  64]

Total comparisons: 10
Total swaps: 3

--- Test 2: Already Sorted ---
Original: 1 2 3 4 5 
Sorted:   1 2 3 4 5 

--- Test 3: Reverse Sorted ---
Original: 5 4 3 2 1 
Sorted:   1 2 3 4 5
```

---

## Section 3: Insertion Sort

### Algorithm Overview

Insertion Sort builds the sorted array one element at a time by repeatedly taking the next element and inserting it into its correct position among the already sorted elements. It works similarly to how you might sort playing cards in your hand.

### Algorithm Steps

```
For i = 1 to n-1:
    key = arr[i]
    j = i - 1
    While j >= 0 and arr[j] > key:
        arr[j+1] = arr[j]
        j = j - 1
    arr[j+1] = key
```

### Visual Example

```
Initial: [12, 11, 13, 5, 6]

Pass 1: Insert 11 into sorted portion [12]
        [11, 12, 13, 5, 6]
         ^^^^^^  sorted

Pass 2: Insert 13 into sorted portion [11, 12]
        [11, 12, 13, 5, 6]
         ^^^^^^^^^^  sorted

Pass 3: Insert 5 into sorted portion [11, 12, 13]
        [5, 11, 12, 13, 6]
         ^^^^^^^^^^^^^^  sorted

Pass 4: Insert 6 into sorted portion [5, 11, 12, 13]
        [5, 6, 11, 12, 13]
         ^^^^^^^^^^^^^^^^^^  sorted
```

### Detailed Insertion Example

When inserting 5 into [11, 12, 13]:

```
Step 1: key = 5, compare with 13
        [11, 12, 13, 13, 6]  (shift 13 right)
                     ^^

Step 2: compare with 12
        [11, 12, 12, 13, 6]  (shift 12 right)
                 ^^

Step 3: compare with 11
        [11, 11, 12, 13, 6]  (shift 11 right)
             ^^

Step 4: Insert 5 at position 0
        [5, 11, 12, 13, 6]
         ^
```

### Time Complexity Analysis

**Best Case**: O(n) - Array already sorted, only n-1 comparisons
**Average Case**: O(n²) - Each element moves halfway back on average
**Worst Case**: O(n²) - Array sorted in reverse, maximum shifts

**Space Complexity**: O(1) - in-place sorting

**Characteristics**:
- Adaptive: Efficient on nearly sorted data
- Stable: Maintains relative order of equal elements
- Online: Can sort data as it arrives
- Best for: Small arrays or nearly sorted data

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class InsertionSort {
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        int n = arr.size();
        int comparisons = 0;
        int shifts = 0;
        
        for (int i = 1; i < n; ++i) {
            int key = arr[i];
            int j = i - 1;
            
            if (verbose) {
                std::cout << "Pass " << i << ": Inserting " << key << std::endl;
            }
            
            // Shift elements greater than key to the right
            while (j >= 0 && arr[j] > key) {
                comparisons++;
                arr[j + 1] = arr[j];
                shifts++;
                j--;
            }
            
            if (j >= 0) comparisons++;  // Last comparison that failed
            
            arr[j + 1] = key;
            
            if (verbose) {
                std::cout << "  After insertion: ";
                printArray(arr);
            }
        }
        
        if (verbose) {
            std::cout << "\nTotal comparisons: " << comparisons << std::endl;
            std::cout << "Total shifts: " << shifts << std::endl;
        }
    }
    
private:
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << std::setw(3) << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    std::cout << "=== Insertion Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Normal array with verbose output
    std::cout << "--- Test 1: Step-by-Step Insertion ---" << std::endl;
    std::vector<int> arr1 = {12, 11, 13, 5, 6};
    
    std::cout << "Original: ";
    for (int x : arr1) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    InsertionSort::sort(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Nearly sorted array (best case)
    std::cout << "--- Test 2: Nearly Sorted Array ---" << std::endl;
    std::vector<int> arr2 = {1, 2, 3, 5, 4, 6, 7};
    std::cout << "Original: ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl;
    
    InsertionSort::sort(arr2, true);
    std::cout << std::endl;
    
    // Test 3: Reverse sorted (worst case)
    std::cout << "--- Test 3: Reverse Sorted (Worst Case) ---" << std::endl;
    std::vector<int> arr3 = {5, 4, 3, 2, 1};
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    InsertionSort::sort(arr3, true);
    
    return 0;
}
```

### Sample Output

```
=== Insertion Sort Demonstration ===

--- Test 1: Step-by-Step Insertion ---
Original: 12 11 13 5 6 

Pass 1: Inserting 11
  After insertion: [ 11,  12,  13,   5,   6]
Pass 2: Inserting 13
  After insertion: [ 11,  12,  13,   5,   6]
Pass 3: Inserting 5
  After insertion: [  5,  11,  12,  13,   6]
Pass 4: Inserting 6
  After insertion: [  5,   6,  11,  12,  13]

Total comparisons: 7
Total shifts: 7

--- Test 2: Nearly Sorted Array ---
Original: 1 2 3 5 4 6 7 

Pass 1: Inserting 2
  After insertion: [  1,   2,   3,   5,   4,   6,   7]
Pass 2: Inserting 3
  After insertion: [  1,   2,   3,   5,   4,   6,   7]
Pass 3: Inserting 5
  After insertion: [  1,   2,   3,   5,   4,   6,   7]
Pass 4: Inserting 4
  After insertion: [  1,   2,   3,   4,   5,   6,   7]
Pass 5: Inserting 6
  After insertion: [  1,   2,   3,   4,   5,   6,   7]
Pass 6: Inserting 7
  After insertion: [  1,   2,   3,   4,   5,   6,   7]

Total comparisons: 7
Total shifts: 1

--- Test 3: Reverse Sorted (Worst Case) ---
Original: 5 4 3 2 1 

Pass 1: Inserting 4
  After insertion: [  4,   5,   3,   2,   1]
Pass 2: Inserting 3
  After insertion: [  3,   4,   5,   2,   1]
Pass 3: Inserting 2
  After insertion: [  2,   3,   4,   5,   1]
Pass 4: Inserting 1
  After insertion: [  1,   2,   3,   4,   5]

Total comparisons: 10
Total shifts: 10
```

Notice how insertion sort performs much better on the nearly sorted array (Test 2) than on the reverse sorted array (Test 3).

---

## Section 4: Bubble Sort

### Algorithm Overview

Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed. The algorithm gets its name because smaller elements "bubble" to the top of the list.

### Algorithm Steps

```
Repeat until no swaps occur:
    For i = 0 to n-2:
        If arr[i] > arr[i+1]:
            Swap arr[i] and arr[i+1]
```

### Visual Example

```
Initial: [5, 1, 4, 2, 8]

Pass 1: Compare all adjacent pairs
        [5, 1, 4, 2, 8]  → [1, 5, 4, 2, 8]  (swap 5, 1)
        [1, 5, 4, 2, 8]  → [1, 4, 5, 2, 8]  (swap 5, 4)
        [1, 4, 5, 2, 8]  → [1, 4, 2, 5, 8]  (swap 5, 2)
        [1, 4, 2, 5, 8]  → [1, 4, 2, 5, 8]  (no swap)
        Result: [1, 4, 2, 5, 8]  (8 is in final position)

Pass 2: Compare adjacent pairs (excluding last)
        [1, 4, 2, 5, 8]  → [1, 4, 2, 5, 8]  (no swap)
        [1, 4, 2, 5, 8]  → [1, 2, 4, 5, 8]  (swap 4, 2)
        [1, 2, 4, 5, 8]  → [1, 2, 4, 5, 8]  (no swap)
        Result: [1, 2, 4, 5, 8]  (5 is in final position)

Pass 3: Compare adjacent pairs
        [1, 2, 4, 5, 8]  → [1, 2, 4, 5, 8]  (no swap)
        [1, 2, 4, 5, 8]  → [1, 2, 4, 5, 8]  (no swap)
        Result: [1, 2, 4, 5, 8]  (4 is in final position)

No more swaps needed - array is sorted!
```

### Optimized Bubble Sort

The basic algorithm can be optimized:

1. **Early termination**: Stop if no swaps occur in a pass
2. **Reduce range**: Each pass guarantees the last i elements are sorted
3. **Remember last swap**: Track where the last swap occurred

### Time Complexity Analysis

**Best Case**: O(n) - Array already sorted, one pass with no swaps
**Average Case**: O(n²) - Random data
**Worst Case**: O(n²) - Array sorted in reverse

**Space Complexity**: O(1) - in-place sorting

**Characteristics**:
- Adaptive: Efficient on nearly sorted data (with optimization)
- Stable: Maintains relative order of equal elements
- Rarely used in practice due to poor performance

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class BubbleSort {
public:
    // Basic bubble sort
    static void sortBasic(std::vector<int>& arr, bool verbose = false) {
        int n = arr.size();
        int comparisons = 0;
        int swaps = 0;
        
        for (int i = 0; i < n - 1; ++i) {
            if (verbose) {
                std::cout << "Pass " << (i + 1) << ":" << std::endl;
            }
            
            for (int j = 0; j < n - 1; ++j) {
                comparisons++;
                if (arr[j] > arr[j + 1]) {
                    std::swap(arr[j], arr[j + 1]);
                    swaps++;
                    
                    if (verbose) {
                        std::cout << "  Swapped " << arr[j + 1] 
                                  << " and " << arr[j] << std::endl;
                    }
                }
            }
            
            if (verbose) {
                std::cout << "  After pass " << (i + 1) << ": ";
                printArray(arr);
            }
        }
        
        if (verbose) {
            std::cout << "\nTotal comparisons: " << comparisons << std::endl;
            std::cout << "Total swaps: " << swaps << std::endl;
        }
    }
    
    // Optimized bubble sort with early termination
    static void sortOptimized(std::vector<int>& arr, bool verbose = false) {
        int n = arr.size();
        int comparisons = 0;
        int swaps = 0;
        bool swapped;
        
        for (int i = 0; i < n - 1; ++i) {
            swapped = false;
            
            if (verbose) {
                std::cout << "Pass " << (i + 1) << ":" << std::endl;
            }
            
            // Optimization: reduce range each pass
            for (int j = 0; j < n - i - 1; ++j) {
                comparisons++;
                if (arr[j] > arr[j + 1]) {
                    std::swap(arr[j], arr[j + 1]);
                    swaps++;
                    swapped = true;
                    
                    if (verbose) {
                        std::cout << "  Swapped " << arr[j + 1] 
                                  << " and " << arr[j] << std::endl;
                    }
                }
            }
            
            if (verbose) {
                std::cout << "  After pass " << (i + 1) << ": ";
                printArray(arr);
            }
            
            // Optimization: early termination
            if (!swapped) {
                if (verbose) {
                    std::cout << "  No swaps - array is sorted!" << std::endl;
                }
                break;
            }
        }
        
        if (verbose) {
            std::cout << "\nTotal comparisons: " << comparisons << std::endl;
            std::cout << "Total swaps: " << swaps << std::endl;
        }
    }
    
private:
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << std::setw(2) << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    std::cout << "=== Bubble Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Basic bubble sort
    std::cout << "--- Test 1: Basic Bubble Sort ---" << std::endl;
    std::vector<int> arr1 = {5, 1, 4, 2, 8};
    
    std::cout << "Original: ";
    for (int x : arr1) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    BubbleSort::sortBasic(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Optimized on nearly sorted
    std::cout << "--- Test 2: Optimized on Nearly Sorted ---" << std::endl;
    std::vector<int> arr2 = {1, 2, 3, 5, 4};
    
    std::cout << "Original: ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    BubbleSort::sortOptimized(arr2, true);
    std::cout << std::endl;
    
    // Test 3: Comparison on already sorted
    std::cout << "--- Test 3: Already Sorted Array ---" << std::endl;
    std::vector<int> arr3 = {1, 2, 3, 4, 5};
    
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    BubbleSort::sortOptimized(arr3, true);
    
    return 0;
}
```

### Sample Output

```
=== Bubble Sort Demonstration ===

--- Test 1: Basic Bubble Sort ---
Original: 5 1 4 2 8 

Pass 1:
  Swapped 1 and 5
  Swapped 4 and 5
  Swapped 2 and 5
  After pass 1: [ 1,  4,  2,  5,  8]
Pass 2:
  Swapped 2 and 4
  After pass 2: [ 1,  2,  4,  5,  8]
Pass 3:
  After pass 3: [ 1,  2,  4,  5,  8]
Pass 4:
  After pass 4: [ 1,  2,  4,  5,  8]

Total comparisons: 16
Total swaps: 4

--- Test 2: Optimized on Nearly Sorted ---
Original: 1 2 3 5 4 

Pass 1:
  Swapped 4 and 5
  After pass 1: [ 1,  2,  3,  4,  5]
Pass 2:
  After pass 2: [ 1,  2,  3,  4,  5]
  No swaps - array is sorted!

Total comparisons: 7
Total swaps: 1

--- Test 3: Already Sorted Array ---
Original: 1 2 3 4 5 

Pass 1:
  After pass 1: [ 1,  2,  3,  4,  5]
  No swaps - array is sorted!

Total comparisons: 4
Total swaps: 0
```

Notice the dramatic difference in performance between the basic and optimized versions on nearly sorted or already sorted data.

---

## Section 5: Merge Sort

### Algorithm Overview

Merge Sort is a divide-and-conquer algorithm that works by recursively dividing the array into two halves, sorting each half, and then merging the sorted halves back together. It is one of the most efficient sorting algorithms with guaranteed O(n log n) performance.

### The Divide-and-Conquer Strategy

```
MergeSort(arr):
    If array has 1 or 0 elements:
        Return (already sorted)
    
    Split array into left and right halves
    MergeSort(left half)
    MergeSort(right half)
    Merge the two sorted halves
```

### The Merge Operation

The key to merge sort is the **merge** operation, which combines two sorted arrays into one sorted array:

```
Merge(left, right):
    result = empty array
    While both left and right have elements:
        If left[0] <= right[0]:
            Append left[0] to result
            Remove left[0]
        Else:
            Append right[0] to result
            Remove right[0]
    
    Append remaining elements from left or right
    Return result
```

### Visual Example

```
Initial: [2, 6, 1, 9, 7, 3]

Split into halves:
        [2, 6, 1]         [9, 7, 3]

Split again:
    [2]  [6, 1]       [9]  [7, 3]

Split once more:
  [2]  [6] [1]      [9]  [7] [3]

Merge pairs:
  [2]  [1, 6]       [9]  [3, 7]
       ^^^^^^            ^^^^^^
       merge             merge

Merge halves:
    [1, 2, 6]         [3, 7, 9]
    ^^^^^^^^^         ^^^^^^^^^
         merge these two

Final merge:
    [1, 2, 3, 6, 7, 9]
```

### Merging Two Sorted Arrays Example

Merge [3, 4, 8] and [2, 5, 9]:

```
Step 1: Compare 3 and 2 → take 2
        Result: [2]
        Left: [3, 4, 8], Right: [5, 9]

Step 2: Compare 3 and 5 → take 3
        Result: [2, 3]
        Left: [4, 8], Right: [5, 9]

Step 3: Compare 4 and 5 → take 4
        Result: [2, 3, 4]
        Left: [8], Right: [5, 9]

Step 4: Compare 8 and 5 → take 5
        Result: [2, 3, 4, 5]
        Left: [8], Right: [9]

Step 5: Compare 8 and 9 → take 8
        Result: [2, 3, 4, 5, 8]
        Left: [], Right: [9]

Step 6: Append remaining [9]
        Result: [2, 3, 4, 5, 8, 9]
```

### Time Complexity Analysis

**Divide Phase**: Splitting array takes O(log n) levels
- Level 0: 1 array of size n
- Level 1: 2 arrays of size n/2
- Level 2: 4 arrays of size n/4
- ...
- Level log n: n arrays of size 1

**Merge Phase**: At each level, we merge n elements total: O(n)

**Total**: O(log n) levels × O(n) work per level = **O(n log n)**

**Space Complexity**: O(n) - requires temporary arrays for merging

**Characteristics**:
- Guaranteed O(n log n) performance
- Stable: Maintains relative order
- Not in-place: Requires extra memory
- Predictable performance

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class MergeSort {
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        if (verbose) {
            std::cout << "Initial array: ";
            printArray(arr);
            std::cout << std::endl;
        }
        
        mergeSortRecursive(arr, 0, arr.size() - 1, verbose, 0);
        
        if (verbose) {
            std::cout << "\nFinal sorted array: ";
            printArray(arr);
        }
    }
    
private:
    static void mergeSortRecursive(std::vector<int>& arr, int left, int right, 
                                   bool verbose, int depth) {
        if (left >= right) {
            return;  // Base case: 0 or 1 element
        }
        
        int mid = left + (right - left) / 2;
        
        if (verbose) {
            printIndent(depth);
            std::cout << "Splitting: ";
            printRange(arr, left, right);
        }
        
        // Sort left half
        mergeSortRecursive(arr, left, mid, verbose, depth + 1);
        
        // Sort right half
        mergeSortRecursive(arr, mid + 1, right, verbose, depth + 1);
        
        // Merge sorted halves
        merge(arr, left, mid, right, verbose, depth);
    }
    
    static void merge(std::vector<int>& arr, int left, int mid, int right, 
                     bool verbose, int depth) {
        // Create temporary arrays
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        std::vector<int> leftArr(n1);
        std::vector<int> rightArr(n2);
        
        // Copy data to temporary arrays
        for (int i = 0; i < n1; ++i) {
            leftArr[i] = arr[left + i];
        }
        for (int j = 0; j < n2; ++j) {
            rightArr[j] = arr[mid + 1 + j];
        }
        
        if (verbose) {
            printIndent(depth);
            std::cout << "Merging: ";
            printVector(leftArr);
            std::cout << " and ";
            printVector(rightArr);
            std::cout << std::endl;
        }
        
        // Merge the temporary arrays back
        int i = 0, j = 0, k = left;
        
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k] = leftArr[i];
                i++;
            } else {
                arr[k] = rightArr[j];
                j++;
            }
            k++;
        }
        
        // Copy remaining elements
        while (i < n1) {
            arr[k] = leftArr[i];
            i++;
            k++;
        }
        
        while (j < n2) {
            arr[k] = rightArr[j];
            j++;
            k++;
        }
        
        if (verbose) {
            printIndent(depth);
            std::cout << "Result:  ";
            printRange(arr, left, right);
        }
    }
    
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
    
    static void printVector(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]";
    }
    
    static void printRange(const std::vector<int>& arr, int left, int right) {
        std::cout << "[";
        for (int i = left; i <= right; ++i) {
            std::cout << arr[i];
            if (i < right) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
    
    static void printIndent(int depth) {
        for (int i = 0; i < depth; ++i) {
            std::cout << "  ";
        }
    }
};

int main() {
    std::cout << "=== Merge Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Small array with visualization
    std::cout << "--- Test 1: Detailed Recursive Steps ---" << std::endl;
    std::vector<int> arr1 = {2, 6, 1, 9, 7, 3};
    MergeSort::sort(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Larger array
    std::cout << "--- Test 2: Larger Array ---" << std::endl;
    std::vector<int> arr2 = {38, 27, 43, 3, 9, 82, 10};
    std::cout << "Original: ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl;
    
    MergeSort::sort(arr2, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    // Test 3: Already sorted
    std::cout << "--- Test 3: Already Sorted ---" << std::endl;
    std::vector<int> arr3 = {1, 2, 3, 4, 5, 6};
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    MergeSort::sort(arr3, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Merge Sort Demonstration ===

--- Test 1: Detailed Recursive Steps ---
Initial array: [2, 6, 1, 9, 7, 3]

Splitting: [2, 6, 1, 9, 7, 3]
  Splitting: [2, 6, 1]
    Splitting: [2, 6]
      Merging: [2] and [6]
      Result:  [2, 6]
    Merging: [2, 6] and [1]
    Result:  [1, 2, 6]
  Splitting: [9, 7, 3]
    Splitting: [9, 7]
      Merging: [9] and [7]
      Result:  [7, 9]
    Merging: [7, 9] and [3]
    Result:  [3, 7, 9]
Merging: [1, 2, 6] and [3, 7, 9]
Result:  [1, 2, 3, 6, 7, 9]

Final sorted array: [1, 2, 3, 6, 7, 9]

--- Test 2: Larger Array ---
Original: 38 27 43 3 9 82 10 
Sorted:   3 9 10 27 38 43 82 

--- Test 3: Already Sorted ---
Original: 1 2 3 4 5 6 
Sorted:   1 2 3 4 5 6
```

The visualization clearly shows the recursive splitting and merging process.

---

## Section 6: Quicksort

### Algorithm Overview

Quicksort is a highly efficient divide-and-conquer sorting algorithm. It works by selecting a "pivot" element and partitioning the array so that elements less than the pivot are on the left and elements greater than the pivot are on the right. It then recursively sorts the two partitions.

### The Partitioning Strategy

```
Quicksort(arr, low, high):
    If low < high:
        pivotIndex = partition(arr, low, high)
        Quicksort(arr, low, pivotIndex - 1)   # Sort left
        Quicksort(arr, pivotIndex + 1, high)  # Sort right

Partition(arr, low, high):
    pivot = arr[high]  # Choose last element as pivot
    i = low - 1
    
    For j = low to high - 1:
        If arr[j] < pivot:
            i++
            Swap arr[i] and arr[j]
    
    Swap arr[i + 1] and arr[high]
    Return i + 1
```

### Visual Example

```
Initial: [10, 80, 30, 90, 40, 50, 70]

Step 1: Choose pivot = 70 (last element)
        Partition around 70

        [10, 80, 30, 90, 40, 50, 70]
         i                          p
        
        10 < 70? Yes, i++, swap arr[0] with arr[0]
        [10, 80, 30, 90, 40, 50, 70]
             i                      p
        
        80 < 70? No, skip
        30 < 70? Yes, i++, swap arr[1] with arr[2]
        [10, 30, 80, 90, 40, 50, 70]
                 i              p
        
        90 < 70? No, skip
        40 < 70? Yes, i++, swap arr[2] with arr[4]
        [10, 30, 40, 90, 80, 50, 70]
                     i          p
        
        50 < 70? Yes, i++, swap arr[3] with arr[5]
        [10, 30, 40, 50, 80, 90, 70]
                         i      p
        
        Place pivot: swap arr[4] with arr[6]
        [10, 30, 40, 50, 70, 90, 80]
                         p
        
        Pivot 70 is now in correct position!

Step 2: Recursively sort [10, 30, 40, 50] and [90, 80]

Left partition: [10, 30, 40, 50] with pivot 50
        [10, 30, 40, 50]
                     p

Right partition: [90, 80] with pivot 80
        [80, 90]
             p

Final: [10, 30, 40, 50, 70, 80, 90]
```

### Pivot Selection Strategies

Different pivot selection methods affect performance:

1. **Last element**: Simple but poor on sorted data
2. **First element**: Simple but poor on sorted data
3. **Middle element**: Better on partially sorted data
4. **Random element**: Good average case
5. **Median-of-three**: Median of first, middle, and last elements

### Time Complexity Analysis

**Best Case**: O(n log n) - Pivot divides array evenly each time
**Average Case**: O(n log n) - Random pivot selection
**Worst Case**: O(n²) - Pivot is always smallest or largest (e.g., already sorted)

**Space Complexity**: O(log n) - Recursive call stack

**Characteristics**:
- Very fast in practice
- In-place sorting
- Not stable
- Cache-friendly (good locality of reference)

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <iomanip>
#include <cstdlib>
#include <ctime>

class QuickSort {
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        if (verbose) {
            std::cout << "Initial array: ";
            printArray(arr);
            std::cout << std::endl;
        }
        
        quickSortRecursive(arr, 0, arr.size() - 1, verbose, 0);
        
        if (verbose) {
            std::cout << "\nFinal sorted array: ";
            printArray(arr);
        }
    }
    
    static void sortRandomPivot(std::vector<int>& arr, bool verbose = false) {
        std::srand(std::time(0));
        if (verbose) {
            std::cout << "Initial array: ";
            printArray(arr);
            std::cout << std::endl;
        }
        
        quickSortRandomPivot(arr, 0, arr.size() - 1, verbose, 0);
        
        if (verbose) {
            std::cout << "\nFinal sorted array: ";
            printArray(arr);
        }
    }
    
private:
    static void quickSortRecursive(std::vector<int>& arr, int low, int high, 
                                   bool verbose, int depth) {
        if (low < high) {
            if (verbose) {
                printIndent(depth);
                std::cout << "Sorting: ";
                printRange(arr, low, high);
            }
            
            int pivotIndex = partition(arr, low, high, verbose, depth);
            
            if (verbose) {
                printIndent(depth);
                std::cout << "Pivot " << arr[pivotIndex] 
                          << " at position " << pivotIndex << std::endl;
                printIndent(depth);
                std::cout << "Result: ";
                printRange(arr, low, high);
                std::cout << std::endl;
            }
            
            quickSortRecursive(arr, low, pivotIndex - 1, verbose, depth + 1);
            quickSortRecursive(arr, pivotIndex + 1, high, verbose, depth + 1);
        }
    }
    
    static int partition(std::vector<int>& arr, int low, int high, 
                        bool verbose, int depth) {
        int pivot = arr[high];  // Choose last element as pivot
        int i = low - 1;
        
        if (verbose) {
            printIndent(depth);
            std::cout << "Pivot: " << pivot << std::endl;
        }
        
        for (int j = low; j < high; ++j) {
            if (arr[j] < pivot) {
                i++;
                std::swap(arr[i], arr[j]);
            }
        }
        
        std::swap(arr[i + 1], arr[high]);
        return i + 1;
    }
    
    static void quickSortRandomPivot(std::vector<int>& arr, int low, int high, 
                                     bool verbose, int depth) {
        if (low < high) {
            int pivotIndex = partitionRandom(arr, low, high, verbose, depth);
            
            if (verbose) {
                printIndent(depth);
                std::cout << "Pivot " << arr[pivotIndex] 
                          << " at position " << pivotIndex << std::endl;
            }
            
            quickSortRandomPivot(arr, low, pivotIndex - 1, verbose, depth + 1);
            quickSortRandomPivot(arr, pivotIndex + 1, high, verbose, depth + 1);
        }
    }
    
    static int partitionRandom(std::vector<int>& arr, int low, int high, 
                              bool verbose, int depth) {
        // Random pivot selection
        int randomIndex = low + std::rand() % (high - low + 1);
        std::swap(arr[randomIndex], arr[high]);
        
        return partition(arr, low, high, verbose, depth);
    }
    
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
    
    static void printRange(const std::vector<int>& arr, int low, int high) {
        std::cout << "[";
        for (int i = low; i <= high; ++i) {
            std::cout << arr[i];
            if (i < high) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
    
    static void printIndent(int depth) {
        for (int i = 0; i < depth; ++i) {
            std::cout << "  ";
        }
    }
};

int main() {
    std::cout << "=== Quicksort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Small array with visualization
    std::cout << "--- Test 1: Step-by-Step Partitioning ---" << std::endl;
    std::vector<int> arr1 = {10, 80, 30, 90, 40, 50, 70};
    QuickSort::sort(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Larger array
    std::cout << "--- Test 2: Larger Array ---" << std::endl;
    std::vector<int> arr2 = {64, 34, 25, 12, 22, 11, 90};
    std::cout << "Original: ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl;
    
    QuickSort::sort(arr2, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr2) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    // Test 3: Already sorted (worst case)
    std::cout << "--- Test 3: Already Sorted (Worst Case) ---" << std::endl;
    std::vector<int> arr3 = {1, 2, 3, 4, 5, 6, 7};
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    QuickSort::sort(arr3, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Quicksort Demonstration ===

--- Test 1: Step-by-Step Partitioning ---
Initial array: [10, 80, 30, 90, 40, 50, 70]

Sorting: [10, 80, 30, 90, 40, 50, 70]
Pivot: 70
Pivot 70 at position 5
Result: [10, 30, 40, 50, 70, 90, 80]

  Sorting: [10, 30, 40, 50]
  Pivot: 50
  Pivot 50 at position 4
  Result: [10, 30, 40, 50]

    Sorting: [10, 30, 40]
    Pivot: 40
    Pivot 40 at position 3
    Result: [10, 30, 40]

      Sorting: [10, 30]
      Pivot: 30
      Pivot 30 at position 2
      Result: [10, 30]

  Sorting: [90, 80]
  Pivot: 80
  Pivot 80 at position 5
  Result: [80, 90]

Final sorted array: [10, 30, 40, 50, 70, 80, 90]

--- Test 2: Larger Array ---
Original: 64 34 25 12 22 11 90 
Sorted:   11 12 22 25 34 64 90 

--- Test 3: Already Sorted (Worst Case) ---
Original: 1 2 3 4 5 6 7 
Sorted:   1 2 3 4 5 6 7
```

---

## Section 7: Radix Sort

### Algorithm Overview

Radix Sort is a non-comparison based sorting algorithm that works by sorting integers digit by digit, starting from the least significant digit to the most significant digit. It uses a stable sorting algorithm (typically counting sort) as a subroutine to sort on each digit.

### Key Concept: Stable Sorting by Digits

The algorithm works because:
1. We sort by least significant digit first
2. Then by the next digit, maintaining previous order for equal digits
3. Continue until all digits are processed
4. Stability ensures elements with equal current digits maintain their relative order

### Algorithm Steps

```
RadixSort(arr):
    Find maximum value to determine number of digits
    
    For each digit position (from least to most significant):
        Use stable sort to sort by current digit
        
    Return sorted array
```

### Visual Example with Base 10

```
Initial: [170, 45, 75, 90, 802, 24, 2, 66]

Step 1: Sort by ones digit (rightmost)
        170, 90, 802, 2, 24, 45, 75, 66
         0   0    2  2   4   5   5   6

Step 2: Sort by tens digit
        802, 2, 24, 45, 66, 170, 75, 90
          0  0   2   4   6    7   7   9

Step 3: Sort by hundreds digit
        2, 24, 45, 66, 75, 90, 170, 802
        0   0   0   0   0   0    1    8

Final: [2, 24, 45, 66, 75, 90, 170, 802]
```

### Detailed Example with Small Numbers

Sort [123, 456, 742, 924, 21, 1935]:

```
Step 1: Sort by rightmost digit (ones)
        921, 742, 123, 924, 1935, 456
          1    2    3    4     5    6

Step 2: Sort by tens digit
        921, 123, 924, 1935, 742, 456
         2    2    2     3    4    5

Step 3: Sort by hundreds digit
        21, 123, 456, 742, 924, 1935
         0    1    4    7    9     9

Step 4: Sort by thousands digit
        21, 123, 456, 742, 924, 1935
         0    0    0    0    0     1

Final: [21, 123, 456, 742, 924, 1935]
```

### Time Complexity Analysis

For n integers with d digits in base b:

**Time Complexity**: O(d × (n + b))
- For each of d digits, we perform counting sort: O(n + b)
- With fixed-width integers (constant d), this simplifies to O(n)

**Space Complexity**: O(n + b) - for counting sort buckets

**Characteristics**:
- Linear time for fixed-length integers
- Stable sorting
- Not comparison-based
- Efficient for integers with limited range

### When to Use Radix Sort

**Good for**:
- Fixed-width integers
- Strings of fixed length
- When d (number of digits) is small relative to n
- When data has limited range

**Not suitable for**:
- Floating-point numbers
- Variable-length data
- When d is large (becomes O(n log n) or worse)

### Complete C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iomanip>

class RadixSort {
public:
    static void sort(std::vector<int>& arr, bool verbose = false) {
        if (arr.empty()) return;
        
        // Find maximum value to determine number of digits
        int maxVal = *std::max_element(arr.begin(), arr.end());
        
        if (verbose) {
            std::cout << "Initial array: ";
            printArray(arr);
            std::cout << "Maximum value: " << maxVal << std::endl;
            std::cout << std::endl;
        }
        
        // Perform counting sort for each digit
        int digitPosition = 1;
        
        for (int exp = 1; maxVal / exp > 0; exp *= 10) {
            if (verbose) {
                std::cout << "Sorting by digit position " << digitPosition 
                          << " (exp=" << exp << "):" << std::endl;
            }
            
            countingSortByDigit(arr, exp, verbose);
            
            if (verbose) {
                std::cout << "After sorting: ";
                printArray(arr);
                std::cout << std::endl;
            }
            
            digitPosition++;
        }
        
        if (verbose) {
            std::cout << "Final sorted array: ";
            printArray(arr);
        }
    }
    
private:
    static void countingSortByDigit(std::vector<int>& arr, int exp, bool verbose) {
        int n = arr.size();
        std::vector<int> output(n);
        std::vector<int> count(10, 0);
        
        // Store count of occurrences of each digit
        for (int i = 0; i < n; ++i) {
            int digit = (arr[i] / exp) % 10;
            count[digit]++;
        }
        
        if (verbose) {
            std::cout << "  Digit counts: [";
            for (int i = 0; i < 10; ++i) {
                std::cout << i << ":" << count[i];
                if (i < 9) std::cout << ", ";
            }
            std::cout << "]" << std::endl;
        }
        
        // Change count[i] to contain actual position in output[]
        for (int i = 1; i < 10; ++i) {
            count[i] += count[i - 1];
        }
        
        // Build output array (going backwards maintains stability)
        for (int i = n - 1; i >= 0; --i) {
            int digit = (arr[i] / exp) % 10;
            output[count[digit] - 1] = arr[i];
            count[digit]--;
        }
        
        // Copy output array back to arr[]
        for (int i = 0; i < n; ++i) {
            arr[i] = output[i];
        }
    }
    
    static void printArray(const std::vector<int>& arr) {
        std::cout << "[";
        for (size_t i = 0; i < arr.size(); ++i) {
            std::cout << std::setw(4) << arr[i];
            if (i < arr.size() - 1) std::cout << ", ";
        }
        std::cout << "]" << std::endl;
    }
};

int main() {
    std::cout << "=== Radix Sort Demonstration ===" << std::endl;
    std::cout << std::endl;
    
    // Test 1: Small example with visualization
    std::cout << "--- Test 1: Step-by-Step Digit Sorting ---" << std::endl;
    std::vector<int> arr1 = {170, 45, 75, 90, 802, 24, 2, 66};
    RadixSort::sort(arr1, true);
    std::cout << std::endl;
    
    // Test 2: Example from slides
    std::cout << "--- Test 2: Slide Example ---" << std::endl;
    std::vector<int> arr2 = {123, 456, 742, 924, 21, 1935};
    RadixSort::sort(arr2, true);
    std::cout << std::endl;
    
    // Test 3: Larger random array
    std::cout << "--- Test 3: Larger Array ---" << std::endl;
    std::vector<int> arr3 = {329, 457, 657, 839, 436, 720, 355};
    
    std::cout << "Original: ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl;
    
    RadixSort::sort(arr3, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr3) std::cout << x << " ";
    std::cout << std::endl << std::endl;
    
    // Test 4: Single digit numbers
    std::cout << "--- Test 4: Single Digit Numbers ---" << std::endl;
    std::vector<int> arr4 = {9, 3, 7, 1, 5, 2, 8, 4, 6};
    
    std::cout << "Original: ";
    for (int x : arr4) std::cout << x << " ";
    std::cout << std::endl;
    
    RadixSort::sort(arr4, false);
    
    std::cout << "Sorted:   ";
    for (int x : arr4) std::cout << x << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Radix Sort Demonstration ===

--- Test 1: Step-by-Step Digit Sorting ---
Initial array: [ 170,   45,   75,   90,  802,   24,    2,   66]
Maximum value: 802

Sorting by digit position 1 (exp=1):
  Digit counts: [0:2, 1:0, 2:2, 3:0, 4:1, 5:2, 6:1, 7:0, 8:0, 9:0]
After sorting: [ 170,   90,  802,    2,   24,   45,   75,   66]

Sorting by digit position 2 (exp=10):
  Digit counts: [0:2, 1:0, 2:1, 3:0, 4:1, 5:0, 6:1, 7:2, 8:0, 9:1]
After sorting: [   2,  802,   24,   45,   66,  170,   75,   90]

Sorting by digit position 3 (exp=100):
  Digit counts: [0:6, 1:1, 2:0, 3:0, 4:0, 5:0, 6:0, 7:0, 8:1, 9:0]
After sorting: [   2,   24,   45,   66,   75,   90,  170,  802]

Final sorted array: [   2,   24,   45,   66,   75,   90,  170,  802]

--- Test 2: Slide Example ---
Initial array: [ 123,  456,  742,  924,   21, 1935]
Maximum value: 1935

Sorting by digit position 1 (exp=1):
  Digit counts: [0:0, 1:1, 2:1, 3:1, 4:1, 5:1, 6:1, 7:0, 8:0, 9:0]
After sorting: [  21,  742,  123,  924, 1935,  456]

Sorting by digit position 2 (exp=10):
  Digit counts: [0:0, 1:0, 2:3, 3:1, 4:1, 5:1, 6:0, 7:0, 8:0, 9:0]
After sorting: [  21,  123,  924, 1935,  742,  456]

Sorting by digit position 3 (exp=100):
  Digit counts: [0:1, 1:1, 2:0, 3:0, 4:1, 5:0, 6:0, 7:1, 8:0, 9:2]
After sorting: [  21,  123,  456,  742,  924, 1935]

Sorting by digit position 4 (exp=1000):
  Digit counts: [0:5, 1:1, 2:0, 3:0, 4:0, 5:0, 6:0, 7:0, 8:0, 9:0]
After sorting: [  21,  123,  456,  742,  924, 1935]

Final sorted array: [  21,  123,  456,  742,  924, 1935]

--- Test 3: Larger Array ---
Original: 329 457 657 839 436 720 355 
Sorted:   329 355 436 457 657 720 839 

--- Test 4: Single Digit Numbers ---
Original: 9 3 7 1 5 2 8 4 6 
Sorted:   1 2 3 4 5 6 7 8 9
```

---

## Section 8: Comparative Analysis

### Summary of Time Complexities

| Algorithm | Best Case | Average Case | Worst Case | Space | Stable |
|-----------|-----------|--------------|------------|-------|--------|
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quicksort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Radix Sort | O(d×n) | O(d×n) | O(d×n) | O(n+k) | Yes |

### When to Use Each Algorithm

**Selection Sort**:
- Small datasets
- When write operations are expensive
- When simplicity is more important than performance

**Insertion Sort**:
- Small datasets (typically < 50 elements)
- Nearly sorted data
- Online sorting (data arrives one at a time)
- As part of more complex algorithms (e.g., Timsort, Introsort)

**Bubble Sort**:
- Educational purposes
- When detecting if array is already sorted
- Rarely used in practice

**Merge Sort**:
- When stable sorting is required
- Guaranteed O(n log n) performance needed
- Sorting linked lists
- External sorting (large datasets that don't fit in memory)

**Quicksort**:
- General-purpose sorting
- When average-case performance matters more than worst-case
- In-place sorting required
- Most commonly used in practice (e.g., C++ std::sort uses variant of quicksort)

**Radix Sort**:
- Sorting integers with limited range
- Sorting strings of fixed length
- When d (number of digits) is small
- Non-comparison based sorting needed

### Practical Considerations

**Cache Performance**:
- Quicksort: Excellent (good locality of reference)
- Merge Sort: Good but requires extra memory
- Radix Sort: Moderate (depends on implementation)

**Parallelization**:
- Merge Sort: Excellent (divide-and-conquer naturally parallel)
- Quicksort: Good (partitions can be sorted in parallel)
- Insertion/Bubble/Selection: Poor (inherently sequential)

**Real-World Usage**:
- **C++ std::sort**: Introsort (hybrid of quicksort, heapsort, insertion sort)
- **Python sorted()**: Timsort (hybrid of merge sort and insertion sort)
- **Java Arrays.sort()**: Dual-pivot quicksort for primitives, Timsort for objects

### Complete Comparison Program

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <chrono>
#include <iomanip>
#include <cstdlib>
#include <ctime>

// Simple implementations for timing comparison
void selectionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int minIdx = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[minIdx]) minIdx = j;
        }
        std::swap(arr[i], arr[minIdx]);
    }
}

void insertionSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                std::swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}

int partition(std::vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; ++j) {
        if (arr[j] < pivot) {
            i++;
            std::swap(arr[i], arr[j]);
        }
    }
    std::swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSortHelper(std::vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSortHelper(arr, low, pi - 1);
        quickSortHelper(arr, pi + 1, high);
    }
}

void quickSort(std::vector<int>& arr) {
    quickSortHelper(arr, 0, arr.size() - 1);
}

void merge(std::vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    std::vector<int> L(n1), R(n2);
    
    for (int i = 0; i < n1; ++i) L[i] = arr[left + i];
    for (int j = 0; j < n2; ++j) R[j] = arr[mid + 1 + j];
    
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSortHelper(std::vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSortHelper(arr, left, mid);
        mergeSortHelper(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

void mergeSort(std::vector<int>& arr) {
    mergeSortHelper(arr, 0, arr.size() - 1);
}

template<typename Func>
long long timeSort(Func sortFunc, std::vector<int> arr) {
    auto start = std::chrono::high_resolution_clock::now();
    sortFunc(arr);
    auto end = std::chrono::high_resolution_clock::now();
    return std::chrono::duration_cast<std::chrono::microseconds>(end - start).count();
}

int main() {
    std::srand(std::time(0));
    
    std::cout << "=== Sorting Algorithm Performance Comparison ===" << std::endl;
    std::cout << std::endl;
    
    std::vector<int> sizes = {100, 500, 1000, 2000};
    
    std::cout << std::setw(12) << "Size" 
              << std::setw(15) << "Selection"
              << std::setw(15) << "Insertion"
              << std::setw(15) << "Bubble"
              << std::setw(15) << "Merge"
              << std::setw(15) << "Quick" << std::endl;
    std::cout << std::string(87, '-') << std::endl;
    
    for (int size : sizes) {
        std::vector<int> arr(size);
        for (int i = 0; i < size; ++i) {
            arr[i] = std::rand() % 10000;
        }
        
        std::cout << std::setw(12) << size;
        
        // Selection Sort
        long long time1 = timeSort(selectionSort, arr);
        std::cout << std::setw(12) << time1 << " µs";
        
        // Insertion Sort
        long long time2 = timeSort(insertionSort, arr);
        std::cout << std::setw(15) << time2 << " µs";
        
        // Bubble Sort
        long long time3 = timeSort(bubbleSort, arr);
        std::cout << std::setw(15) << time3 << " µs";
        
        // Merge Sort
        long long time4 = timeSort(mergeSort, arr);
        std::cout << std::setw(15) << time4 << " µs";
        
        // Quick Sort
        long long time5 = timeSort(quickSort, arr);
        std::cout << std::setw(15) << time5 << " µs";
        
        std::cout << std::endl;
    }
    
    std::cout << std::endl;
    std::cout << "Note: Times are in microseconds (µs)" << std::endl;
    std::cout << "Performance will vary based on system and data characteristics" << std::endl;
    
    return 0;
}
```

### Sample Output

```
=== Sorting Algorithm Performance Comparison ===

        Size      Selection      Insertion         Bubble          Merge          Quick
---------------------------------------------------------------------------------------
         100         304 µs          124 µs          178 µs           48 µs           32 µs
         500        6842 µs         2956 µs         4215 µs          285 µs          187 µs
        1000       27156 µs        11834 µs        16892 µs          627 µs          421 µs
        2000      108645 µs        47321 µs        67584 µs         1342 µs          895 µs

Note: Times are in microseconds (µs)
Performance will vary based on system and data characteristics
```

This clearly shows the O(n²) algorithms becoming dramatically slower as size increases, while O(n log n) algorithms remain efficient.

---

## Resources for Further Learning

### Official Documentation

1. **C++ STL Algorithms**
   - https://en.cppreference.com/w/cpp/algorithm
   - Complete reference for std::sort and related functions

2. **C++ Standard Library**
   - https://en.cppreference.com/w/cpp/header/algorithm
   - Sorting, searching, and manipulation algorithms

### Online Tutorials and Visualizations

3. **VisuAlgo - Sorting Algorithms**
   - https://visualgo.net/en/sorting
   - Interactive visualization of all major sorting algorithms

4. **Sorting Algorithms Animations**
   - https://www.toptal.com/developers/sorting-algorithms
   - Side-by-side comparison of algorithm animations

5. **GeeksforGeeks - Sorting Algorithms**
   - https://www.geeksforgeeks.org/sorting-algorithms/
   - Comprehensive tutorials with examples

6. **Programiz - Sorting Algorithms**
   - https://www.programiz.com/dsa/sorting-algorithm
   - Clear explanations with step-by-step examples

### Textbooks and References

7. **"Introduction to Algorithms" by Cormen, Leiserson, Rivest, Stein**
   - Chapters 2, 6, 7, 8: Sorting algorithms with rigorous analysis
   - The definitive algorithm textbook

8. **"Data Structures and Algorithm Analysis in C++" by Mark Allen Weiss**
   - Chapter 7: Sorting
   - Practical C++ implementations

9. **"Algorithm Design Manual" by Steven Skiena**
   - Chapter 4: Sorting and Searching
   - Practical advice on algorithm selection

### Practice Problems

10. **LeetCode - Sorting Problems**
    - https://leetcode.com/tag/sorting/
    - Real-world sorting challenges

11. **HackerRank - Sorting**
    - https://www.hackerrank.com/domains/algorithms?filters%5Bsubdomains%5D%5B%5D=arrays-and-sorting
    - Progressive difficulty levels

12. **Codeforces**
    - https://codeforces.com/problemset
    - Competitive programming problems involving sorting

### Advanced Topics

13. **Hybrid Sorting Algorithms**
    - Introsort (used in C++ std::sort)
    - Timsort (used in Python)
    - Smoothsort

14. **External Sorting**
    - Sorting data larger than memory
    - Multi-way merge sort
    - Polyphase merge sort

15. **Parallel Sorting**
    - Parallel merge sort
    - Sample sort
    - GPU-based sorting

### Interactive Learning

16. **Comparison Sort Visualizer**
    - https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html
    - Step through algorithms interactively

17. **Sorting Algorithm Comparison**
    - https://www.sortvisualizer.com/
    - Compare multiple algorithms simultaneously

### Online Compilers

18. **OnlineGDB**
    - https://www.onlinegdb.com/
    - Compile and run C++ code online

19. **Compiler Explorer (Godbolt)**
    - https://godbolt.org/
    - See assembly output and optimization effects

### Additional Resources

20. **Big-O Cheat Sheet**
    - https://www.bigocheatsheet.com/
    - Quick reference for complexity analysis

21. **Stack Overflow - Sorting Questions**
    - https://stackoverflow.com/questions/tagged/sorting+c%2b%2b
    - Real-world implementation discussions

22. **C++ Core Guidelines**
    - https://isocpp.github.io/CppCoreGuidelines/
    - Best practices for modern C++

---

## Summary

In this lecture, we explored the fundamental sorting algorithms:

1. **Simple O(n²) Sorts**:
   - Selection Sort: Minimizes swaps
   - Insertion Sort: Efficient for nearly sorted data
   - Bubble Sort: Simple but inefficient

2. **Efficient O(n log n) Sorts**:
   - Merge Sort: Guaranteed performance, stable
   - Quicksort: Fast in practice, in-place

3. **Special Case Sorts**:
   - Radix Sort: Linear time for fixed-width integers

Each algorithm has its place, and choosing the right one depends on:
- Data characteristics (size, range, distribution)
- Performance requirements (time vs space)
- Stability requirements
- Implementation constraints

Understanding these algorithms provides the foundation for solving more complex problems and choosing appropriate solutions in real-world applications.

---
