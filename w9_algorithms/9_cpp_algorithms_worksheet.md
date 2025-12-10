# Lecture 9: C++ Algorithms Worksheet

## Quiz Section

**Time guideline: 10-15 minutes**

### Question 1: Fill in the Blanks

Complete the following sentences using the words from the word bank below.

An __________ is a precisely defined, step-by-step process used to solve a problem. Algorithms must be __________, meaning each step must be clearly defined. They must also be __________, ensuring they eventually terminate. Additionally, algorithms must be __________, with each step being achievable and contributing to solving the problem.

**Word Bank:** `effective`, `finite`, `precise`, `algorithm`

---

### Question 2: Fill in the Blanks

The two-pointer approach to reversing an array uses two pointers: one starting at the __________ of the array and one at the __________. At each step, the elements at these positions are __________, then the left pointer moves __________ and the right pointer moves __________. The algorithm terminates when left is greater than or equal to __________.

**Word Bank:** `backward`, `swapped`, `forward`, `right`, `beginning`, `end`

---

### Question 3: Fill in the Blanks

Binary search works by repeatedly dividing the search space in __________. At each step, we examine the __________ element of the current range. If the target is less than this element, we search the __________ half; otherwise, we search the __________ half. This approach only works on __________ data.

**Word Bank:** `sorted`, `middle`, `left`, `half`, `right`

---

### Question 4: Fill in the Blanks

The STL function `std::max_element()` returns an __________ to the maximum element in a container. To access the actual value, you must __________ the iterator using the __________ operator. If you want to find the index position instead, you can subtract __________ from the iterator.

**Word Bank:** `iterator`, `begin()`, `dereference`, `*`

---

### Question 5: Multiple Choice

What is the time complexity of binary search on a sorted array of n elements?

A) O(n)  
B) O(n²)  
C) O(log n)  
D) O(1)

---

### Question 6: Multiple Choice

Which of the following approaches to reversing an array does NOT require extra memory?

A) Copying elements to a second array  
B) Using a stack to temporarily hold elements  
C) Using two pointers and swapping in-place  
D) All of the above require extra memory

---

### Question 7: Multiple Choice

Consider an array of 1,000,000 elements. Approximately how many comparisons would binary search need in the worst case to find a target element?

A) 10  
B) 20  
C) 1,000  
D) 1,000,000

---

## Task Section

**Time guideline: 5-10 minutes per task**

### Task 1: Finding the Second Largest Element

Write a program that finds the second largest element in an array. Your algorithm should work in a single pass through the array (O(n) time complexity).

**Requirements:**
- Handle arrays with at least 2 elements
- Your algorithm should correctly handle cases where the maximum value appears multiple times
- Do NOT sort the array

**Code Scaffolding:**

```cpp
#include <iostream>
#include <climits>  // For INT_MIN
using namespace std;

int main() {
    const int SIZE = 8;
    int data[SIZE] = {45, 89, 67, 89, 23, 56, 78, 34};
    
    // Your code here
    // Initialize two variables: largest and secondLargest
    
    
    // Loop through the array starting from index 0
    
    
    
    cout << "Array: ";
    for(int i = 0; i < SIZE; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    cout << "Second largest element: " << secondLargest << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array: 45 89 67 89 23 56 78 34 
Second largest element: 78
```

**Test Cases:**
- `{10, 20, 30, 40, 50}` → Second largest: 40
- `{100, 100, 90, 80, 70}` → Second largest: 90
- `{5, 5, 5, 4, 3}` → Second largest: 4

---

### Task 2: Count Occurrences Using Binary Search

Write a program that counts how many times a target value appears in a **sorted** array. You should use a modified approach inspired by binary search concepts.

**Hint:** Once you find the target using binary search, you need to check elements on both sides to count all occurrences.

**Requirements:**
- The array is already sorted
- Handle the case when the target is not in the array (return 0)
- Be efficient - don't just scan the entire array!

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

int countOccurrences(int arr[], int size, int target) {
    // First, use binary search to find ANY occurrence of target
    int left = 0;
    int right = size - 1;
    int position = -1;
    
    // Binary search code here
    
    
    // If not found, return 0
    if(position == -1) {
        return 0;
    }
    
    // Count occurrences by expanding left and right from position
    int count = 1;
    
    // Check left side
    
    
    // Check right side
    
    
    return count;
}

int main() {
    int data[] = {1, 2, 3, 5, 5, 5, 5, 9, 13, 46};
    int size = 10;
    int target = 5;
    
    cout << "Sorted array: ";
    for(int i = 0; i < size; i++) {
        cout << data[i] << " ";
    }
    cout << endl;
    
    int count = countOccurrences(data, size, target);
    cout << "Number " << target << " appears " << count << " times" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted array: 1 2 3 5 5 5 5 9 13 46 
Number 5 appears 4 times
```

**Test Cases:**
- Searching for 1 in `{1, 2, 3, 5, 5, 5, 5, 9, 13, 46}` → 1 occurrence
- Searching for 7 in `{1, 2, 3, 5, 5, 5, 5, 9, 13, 46}` → 0 occurrences
- Searching for 5 in `{5, 5, 5, 5, 5}` → 5 occurrences

---

**End of Worksheet**