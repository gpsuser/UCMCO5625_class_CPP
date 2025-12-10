# CO5625: Simple Algorithms in C++

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Algorithms](#section-1-introduction-to-algorithms)
3. [Section 2: Reversing an Array](#section-2-reversing-an-array)
4. [Section 3: Finding Maximum and Minimum Values](#section-3-finding-maximum-and-minimum-values)
5. [Section 4: Searching Algorithms](#section-4-searching-algorithms)
6. [Section 5: Binary Search and Logarithmic Time](#section-5-binary-search-and-logarithmic-time)
7. [Section 6: The STL Algorithm Library](#section-6-the-stl-algorithm-library)
8. [Resources for Further Learning](#resources-for-further-learning)
9. [Summary](#summary)

---

## Learning Outcomes

By the end of this lecture, students will be able to:

1. **Define** what an algorithm is and explain its role in problem-solving
2. **Implement** at least three different approaches to reversing an array in C++
3. **Design** algorithms to find maximum and minimum values in a collection
4. **Compare** linear search and binary search, explaining when each is appropriate
5. **Analyze** the time complexity of binary search and explain logarithmic time O(log n)
6. **Apply** standard library algorithms from `<algorithm>` to solve common problems

---

## Section 1: Introduction to Algorithms

### What is an Algorithm?

An **algorithm** is a precisely defined, step-by-step process used to solve a problem. Algorithms are fundamental to computer science and programming - every program you write is essentially implementing one or more algorithms.

Key characteristics of algorithms:
- **Precise**: Each step must be clearly defined
- **Finite**: The algorithm must eventually terminate
- **Effective**: Each step must be achievable and contribute to solving the problem
- **Input/Output**: Takes input data and produces output results

### Why Study Algorithms?

Understanding algorithms allows you to:
- Solve problems more efficiently
- Write better, more maintainable code
- Understand performance implications of your code
- Leverage existing solutions in standard libraries

### Common Algorithm Categories

Today we'll explore several fundamental algorithms that operate on collections of data:

1. **Reorganization algorithms** - Changing the order of elements (e.g., reversing)
2. **Search algorithms** - Finding specific elements in a collection
3. **Selection algorithms** - Finding elements with particular properties (e.g., maximum/minimum)

These algorithms form the building blocks for more complex operations you'll encounter throughout your programming career.

---

## Section 2: Reversing an Array

### The Problem

Given an array of elements, produce a version where the elements appear in reverse order.

```
Original: [1, 2, 3, 4, 5]
Reversed: [5, 4, 3, 2, 1]
```

Let's explore three different algorithmic approaches to this problem.

### Approach 1: Copy to a Second Array

The simplest approach uses a second array and copies elements from back to front.

**Algorithm:**
1. Create a new array of the same size
2. Start at the back of the original array
3. Copy each element to the front of the new array
4. Move backward through the original, forward through the new

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 5;
    int original[SIZE] = {1, 2, 3, 4, 5};
    int reversed[SIZE];
    
    cout << "Original array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << original[i] << " ";
    }
    cout << endl;
    
    // Reverse by copying to second array
    for(int i = 0; i < SIZE; i++) {
        reversed[i] = original[SIZE - 1 - i];
    }
    
    cout << "Reversed array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << reversed[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original array: 1 2 3 4 5 
Reversed array: 5 4 3 2 1 
```

**Analysis:**
- **Advantage**: Simple to understand and implement
- **Disadvantage**: Requires extra memory for the second array

### Approach 2: Using a Stack

A stack's Last-In-First-Out (LIFO) property makes it naturally suited for reversing.

**Algorithm:**
1. Push all elements onto a stack
2. Pop elements one by one back into the original array
3. The last element pushed is the first popped

```
Visual representation:
Array: [1, 2, 3, 4, 5]

Push all:
        |   |
        | 5 |  <- Top
        | 4 |
        | 3 |
        | 2 |
        | 1 |
        +---+
        Stack

Pop all back:
Array: [5, 4, 3, 2, 1]
```

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    const int SIZE = 5;
    int arr[SIZE] = {1, 2, 3, 4, 5};
    stack<int> s;
    
    cout << "Original array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    // Push all elements onto stack
    for(int i = 0; i < SIZE; i++) {
        s.push(arr[i]);
    }
    
    // Pop elements back into array
    for(int i = 0; i < SIZE; i++) {
        arr[i] = s.top();
        s.pop();
    }
    
    cout << "Reversed array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original array: 1 2 3 4 5 
Reversed array: 5 4 3 2 1 
```

**Analysis:**
- **Advantage**: Demonstrates practical use of stack data structure
- **Disadvantage**: Still requires extra memory for the stack

### Approach 3: Two-Pointer Swap (In-Place)

The most efficient approach uses two pointers moving toward each other, swapping elements.

**Algorithm:**
1. Create two pointers: left (start) and right (end)
2. Swap the elements at these positions
3. Move left pointer forward, right pointer backward
4. Terminate when left >= right

```
Visual step-by-step:
Initial:     [1, 2, 3, 4, 5]
              L           R

After swap:  [5, 2, 3, 4, 1]
                 L     R

After swap:  [5, 4, 3, 2, 1]
                    LR

Done (left >= right)
```

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 5;
    int arr[SIZE] = {1, 2, 3, 4, 5};
    
    cout << "Original array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    // Two-pointer approach
    int left = 0;
    int right = SIZE - 1;
    
    while(left < right) {
        // Swap elements
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        
        // Move pointers inward
        left++;
        right--;
    }
    
    cout << "Reversed array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original array: 1 2 3 4 5 
Reversed array: 5 4 3 2 1 
```

**Analysis:**
- **Advantage**: No extra memory needed (in-place), most efficient
- **Advantage**: Works with arrays of any size (odd or even)

---

## Section 3: Finding Maximum and Minimum Values

### The Problem

Given a collection of values, find the largest (maximum) and smallest (minimum) elements.

### Algorithm Design

A straightforward approach:
1. Assume the first element is the maximum/minimum
2. Compare each subsequent element with the current maximum/minimum
3. Update if a larger/smaller element is found

This requires only one pass through the data.

### Finding the Maximum

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 8;
    int data[SIZE] = {45, 23, 67, 12, 89, 34, 56, 78};
    
    cout << "Array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    // Find maximum
    int maximum = data[0];  // Assume first element is max
    
    for(int i = 1; i < SIZE; i++) {
        if(data[i] > maximum) {
            maximum = data[i];
        }
    }
    
    cout << "Maximum value: " << maximum << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array: 45 23 67 12 89 34 56 78 
Maximum value: 89
```

### Finding the Minimum

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 8;
    int data[SIZE] = {45, 23, 67, 12, 89, 34, 56, 78};
    
    cout << "Array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    // Find minimum
    int minimum = data[0];  // Assume first element is min
    
    for(int i = 1; i < SIZE; i++) {
        if(data[i] < minimum) {
            minimum = data[i];
        }
    }
    
    cout << "Minimum value: " << minimum << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array: 45 23 67 12 89 34 56 78 
Minimum value: 12
```

### Finding Both Maximum and Minimum

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 8;
    int data[SIZE] = {45, 23, 67, 12, 89, 34, 56, 78};
    
    cout << "Array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    // Find both maximum and minimum in one pass
    int maximum = data[0];
    int minimum = data[0];
    
    for(int i = 1; i < SIZE; i++) {
        if(data[i] > maximum) {
            maximum = data[i];
        }
        if(data[i] < minimum) {
            minimum = data[i];
        }
    }
    
    cout << "Maximum value: " << maximum << endl;
    cout << "Minimum value: " << minimum << endl;
    cout << "Range: " << (maximum - minimum) << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array: 45 23 67 12 89 34 56 78 
Maximum value: 89
Minimum value: 12
Range: 77
```

### Time Complexity

All three algorithms have **O(n)** time complexity - they must examine each element once. This is optimal for unsorted data, as we cannot determine the maximum or minimum without checking all values.

---

## Section 4: Searching Algorithms

Searching is one of the most fundamental operations in computer science. The approach we use depends critically on whether the data is ordered.

### Linear Search (Unordered Data)

When data is **unordered**, we have no choice but to check each element sequentially until we find the target or reach the end.

**Algorithm:**
1. Start at the beginning of the collection
2. Compare each element with the target value
3. If found, return the position
4. If we reach the end without finding it, return -1 (not found)

```cpp
#include <iostream>
using namespace std;

int linearSearch(int arr[], int size, int target) {
    for(int i = 0; i < size; i++) {
        if(arr[i] == target) {
            return i;  // Found at index i
        }
    }
    return -1;  // Not found
}

int main() {
    int data[] = {45, 23, 67, 12, 89, 34, 56, 78};
    int size = 8;
    int target = 34;
    
    cout << "Searching for " << target << " in: ";
    for(int i = 0; i < size; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    int result = linearSearch(data, size, target);
    
    if(result != -1) {
        cout << "Found " << target << " at index " << result << endl;
    } else {
        cout << target << " not found in array" << endl;
    }
    
    // Try searching for something not in the array
    target = 99;
    result = linearSearch(data, size, target);
    
    if(result != -1) {
        cout << "Found " << target << " at index " << result << endl;
    } else {
        cout << target << " not found in array" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Searching for 34 in: 45 23 67 12 89 34 56 78 
Found 34 at index 5
99 not found in array
```

**Time Complexity:** O(n) - worst case must check all elements

### Binary Search (Ordered Data)

When data is **sorted**, we can do much better! Binary search eliminates half of the remaining elements with each comparison.

**The Key Insight:**
Think about looking up a name in a telephone directory:
- **Unordered directory**: You'd have to check every entry
- **Ordered directory**: You can open to the middle, see if your target comes before or after that point, and eliminate half the book with one check

---

## Section 5: Binary Search and Logarithmic Time

### Binary Search Algorithm

Binary search works by repeatedly dividing the search space in half.

**Algorithm:**
1. Look at the middle element of the current range
2. If it equals the target, we're done
3. If the target is less than the middle, search the left half
4. If the target is greater than the middle, search the right half
5. Repeat until found or range is empty

```
Visual example - searching for 46:

Array: [1, 2, 3, 5, 9, 13, 46, 78]
        L              M           R

Middle = 5 (index 3)
46 > 5, so search right half

Array: [1, 2, 3, 5, | 9, 13, 46, 78]
                       L    M      R

Middle = 13 (index 5)
46 > 13, so search right half

Array: [1, 2, 3, 5, 9, 13, | 46, 78]
                              L M  R

Middle = 46 (index 6)
46 == 46, FOUND!
```

### Implementation

```cpp
#include <iostream>
using namespace std;

int binarySearch(int arr[], int size, int target) {
    int left = 0;
    int right = size - 1;
    
    while(left <= right) {
        int middle = left + (right - left) / 2;  // Avoids overflow
        
        if(arr[middle] == target) {
            return middle;  // Found
        }
        else if(arr[middle] < target) {
            left = middle + 1;  // Search right half
        }
        else {
            right = middle - 1;  // Search left half
        }
    }
    
    return -1;  // Not found
}

int main() {
    int data[] = {1, 2, 3, 5, 9, 13, 46, 78};  // Must be sorted!
    int size = 8;
    
    cout << "Sorted array: ";
    for(int i = 0; i < size; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    int target = 46;
    int result = binarySearch(data, size, target);
    
    if(result != -1) {
        cout << "Found " << target << " at index " << result << endl;
    } else {
        cout << target << " not found" << endl;
    }
    
    // Try searching for values not in array
    target = 50;
    result = binarySearch(data, size, target);
    
    if(result != -1) {
        cout << "Found " << target << " at index " << result << endl;
    } else {
        cout << target << " not found" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Sorted array: 1 2 3 5 9 13 46 78 
Found 46 at index 6
50 not found
```

### Understanding Logarithmic Time: O(log n)

Binary search is much faster than linear search, but how much faster?

**The Question:** How many times can we divide n by 2 before reaching 1?

This is equivalent to asking: 2^? = n, which we write as log₂(n)

**Example with n = 8:**
```
Start: 8 elements
After 1 division: 4 elements
After 2 divisions: 2 elements
After 3 divisions: 1 element

3 divisions needed, and log₂(8) = 3
```

**Example with n = 1000:**
```
Linear search worst case: 1000 comparisons
Binary search worst case: log₂(1000) ≈ 10 comparisons

Binary search is 100× faster!
```

**Growth Comparison:**

```
n        O(n)      O(log n)
10       10        ~3
100      100       ~7
1,000    1,000     ~10
10,000   10,000    ~13
100,000  100,000   ~17
```

As you can see, logarithmic growth is extremely slow - this makes binary search incredibly efficient for large datasets!

### Time Complexity Notation

In Big O notation, we drop the base of the logarithm and write **O(log n)** because logarithms in all bases grow similarly with respect to n.

**Important Note:** Binary search only works on **sorted** data! If your data isn't sorted, you must either:
1. Sort it first (which takes time), or
2. Use linear search

---

## Section 6: The STL Algorithm Library

C++ provides a rich set of pre-implemented algorithms in the `<algorithm>` header. These are well-tested, optimized, and should be your first choice when available.

### Finding Maximum and Minimum Elements

The STL provides `std::max_element` and `std::min_element` which return **iterators** to the maximum or minimum elements.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> data = {1, 2, 3, 5, 9, 13, 46, 78};
    
    cout << "Vector: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    // Find maximum element
    auto maxIt = max_element(data.begin(), data.end());
    cout << "Maximum value: " << *maxIt << endl;
    cout << "Maximum at index: " << (maxIt - data.begin()) << endl;
    
    // Find minimum element
    auto minIt = min_element(data.begin(), data.end());
    cout << "Minimum value: " << *minIt << endl;
    cout << "Minimum at index: " << (minIt - data.begin()) << endl;
    
    return 0;
}
```

**Sample Output:**
```
Vector: 1 2 3 5 9 13 46 78 
Maximum value: 78
Maximum at index: 7
Minimum value: 1
Minimum at index: 0
```

**Key Point:** These functions return iterators, so you must **dereference** them with `*` to get the actual value.

### Reversing a Container

The `std::reverse` algorithm reverses elements in place.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> data = {1, 2, 3, 5, 9, 13, 46, 78};
    
    cout << "Original: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    // Reverse the vector
    reverse(data.begin(), data.end());
    
    cout << "Reversed: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original: 1 2 3 5 9 13 46 78 
Reversed: 78 46 13 9 5 3 2 1 
```

### Linear Search with std::find

For **unsorted** data, use `std::find` which performs linear search.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> data = {45, 23, 67, 12, 89, 34, 56, 78};
    
    cout << "Unsorted vector: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    int target = 12;
    auto it = find(data.begin(), data.end(), target);
    
    if(it != data.end()) {
        cout << "Found " << target << " at index " 
             << (it - data.begin()) << endl;
    } else {
        cout << target << " not found" << endl;
    }
    
    // Try searching for something not present
    target = 99;
    it = find(data.begin(), data.end(), target);
    
    if(it != data.end()) {
        cout << "Found " << target << endl;
    } else {
        cout << target << " not found" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Unsorted vector: 45 23 67 12 89 34 56 78 
Found 12 at index 3
99 not found
```

**Key Point:** `std::find` returns an iterator to the found element, or `end()` if not found. Always check against `end()` before dereferencing!

### Binary Search with std::binary_search

For **sorted** data, use `std::binary_search` which returns a boolean.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> data = {1, 2, 3, 5, 9, 13, 46, 78};  // Must be sorted!
    
    cout << "Sorted vector: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    int target = 13;
    if(binary_search(data.begin(), data.end(), target)) {
        cout << "data contains " << target << endl;
    } else {
        cout << "data does not contain " << target << endl;
    }
    
    // Try searching for something not present
    target = 50;
    if(binary_search(data.begin(), data.end(), target)) {
        cout << "data contains " << target << endl;
    } else {
        cout << "data does not contain " << target << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Sorted vector: 1 2 3 5 9 13 46 78 
data contains 13
data does not contain 50
```

### Comprehensive Example

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> data = {45, 23, 67, 12, 89, 34, 56, 78};
    
    cout << "Original data: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    // Find min and max
    cout << "\n--- Finding extremes ---" << endl;
    cout << "Minimum: " << *min_element(data.begin(), data.end()) << endl;
    cout << "Maximum: " << *max_element(data.begin(), data.end()) << endl;
    
    // Search in unsorted data
    cout << "\n--- Linear search (unsorted) ---" << endl;
    if(find(data.begin(), data.end(), 67) != data.end()) {
        cout << "Found 67" << endl;
    }
    
    // Sort the data
    cout << "\n--- Sorting data ---" << endl;
    sort(data.begin(), data.end());
    cout << "Sorted: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    // Binary search in sorted data
    cout << "\n--- Binary search (sorted) ---" << endl;
    if(binary_search(data.begin(), data.end(), 67)) {
        cout << "Found 67 using binary search" << endl;
    }
    
    // Reverse the sorted data
    cout << "\n--- Reversing ---" << endl;
    reverse(data.begin(), data.end());
    cout << "Reversed: ";
    for(int value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original data: 45 23 67 12 89 34 56 78 

--- Finding extremes ---
Minimum: 12
Maximum: 89

--- Linear search (unsorted) ---
Found 67

--- Sorting data ---
Sorted: 12 23 34 45 56 67 78 89 

--- Binary search (sorted) ---
Found 67 using binary search

--- Reversing ---
Reversed: 89 78 67 56 45 34 23 12 
```

### Why Use STL Algorithms?

1. **Tested and optimized** - Written by experts, extensively tested
2. **Expressive** - Code intent is immediately clear
3. **Generic** - Work with any container type
4. **Maintained** - Updated as language and compilers improve
5. **Safe** - Handle edge cases correctly

---

## Resources for Further Learning

### Official Documentation
- **C++ Reference - Algorithm Library**: https://en.cppreference.com/w/cpp/header/algorithm
  Comprehensive reference for all STL algorithms with examples
- **C++ Reference - Iterator Library**: https://en.cppreference.com/w/cpp/iterator
  Understanding iterators is crucial for using STL algorithms effectively

### Online Tutorials
- **LearnCpp.com - STL Algorithms**: https://www.learncpp.com/
  Clear explanations with practical examples
- **GeeksforGeeks - Algorithms**: https://www.geeksforgeeks.org/algorithms/
  Extensive algorithm tutorials with complexity analysis
- **Programiz - C++ Algorithm**: https://www.programiz.com/cpp-programming/algorithms
  Beginner-friendly tutorials on common algorithms

### Practice Platforms
- **LeetCode**: https://leetcode.com/
  Practice searching and sorting problems
- **HackerRank - Algorithms Track**: https://www.hackerrank.com/domains/algorithms
  Structured problems progressing in difficulty
- **Codeforces**: https://codeforces.com/
  Competitive programming with algorithm focus

### Textbooks
- **"Introduction to Algorithms" by Cormen, Leiserson, Rivest, and Stein (CLRS)**
  The definitive algorithms textbook
- **"Algorithm Design" by Kleinberg and Tardos**
  Excellent for understanding algorithm design principles
- **"C++ Primer" by Lippman, Lajoie, and Moo**
  Chapter on STL containers and algorithms

### Community Resources
- **Stack Overflow - C++ Tag**: https://stackoverflow.com/questions/tagged/c++
  Community Q&A for specific implementation questions
- **Reddit - r/cpp**: https://www.reddit.com/r/cpp/
  Discussions on C++ best practices and algorithms

---

## Summary

In this lecture, we explored fundamental algorithms for common programming tasks:

**Key Concepts:**

1. **Algorithms** are precise, step-by-step processes for solving problems
2. **Multiple approaches** often exist for the same problem - consider trade-offs
3. **Time complexity** measures algorithm efficiency as data grows
4. **Data organization** dramatically affects algorithm performance

**Algorithms Covered:**

- **Reversing arrays**: Three approaches (copy, stack, two-pointer)
- **Finding extremes**: Linear scan to find max/min values
- **Linear search**: O(n) - for unsorted data
- **Binary search**: O(log n) - for sorted data only

**STL Algorithms:**

- `std::max_element` / `std::min_element` - Finding extremes
- `std::reverse` - Reversing containers
- `std::find` - Linear search
- `std::binary_search` - Binary search

**Remember:**
- Always use STL algorithms when available - they're tested, optimized, and expressive
- Understand time complexity to make informed decisions
- Binary search requires sorted data
- Iterators are the bridge between algorithms and containers

The algorithms we've studied today form the foundation for more complex operations. As you continue in your studies, you'll see these patterns repeated and combined in increasingly sophisticated ways.