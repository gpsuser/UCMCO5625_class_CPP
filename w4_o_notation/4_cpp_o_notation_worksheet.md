# Lecture 4: C++ Worksheet

## Part 1: Quiz Section

### Question 1: Fill in the Blanks

Complete the following passage about Big O notation by filling in the blanks with the appropriate words from the word bank below.

---

When analyzing algorithms, we use Big O notation to describe the __________ of an algorithm as the input size grows. Big O focuses on the __________ case scenario and ignores constant factors. For example, if an algorithm takes 34 + 12n + 23n² operations, we keep only the __________ term and express it as O(__________). This approach is called __________ analysis because we care about behavior as n approaches infinity.

**Word Bank (in scrambled order):**
- asymptotic
- worst
- dominant
- n²
- efficiency

---

### Question 2: Fill in the Blanks

Complete the following passage about analyzing loop complexity by filling in the blanks with the appropriate words from the word bank below.

---

A single loop that runs from 0 to n has a time complexity of O(__________). When we have two __________ loops, the complexity becomes O(n²) because we __________ their complexities together. However, if we have two loops running one after another (sequentially), we __________ their complexities, which still results in O(n) after dropping __________. A loop that divides the problem size by 2 each iteration has O(__________) complexity.

**Word Bank (in scrambled order):**
- log n
- constants
- n
- add
- nested
- multiply

---

### Question 3: Multiple Choice

What is the time complexity of the following code?

```cpp
for(int i = 0; i < n; i++) {
    for(int j = i; j < n; j++) {
        cout << i * j << " ";
    }
}
```

A) O(n)  
B) O(n²)  
C) O(log n)  
D) O(n log n)

---

### Question 4: Multiple Choice

Consider two algorithms:
- Algorithm X takes 1000 + 5n operations
- Algorithm Y takes 10 + n² operations

For which value of n will Algorithm Y start to perform worse than Algorithm X?

A) n = 10  
B) n = 32  
C) n = 50  
D) n = 100

---

## Part 2: Programming Tasks

### Task 1: Frequency Counter

Write a function `countOccurrences` that takes an array of integers and its size, and prints how many times each unique number appears in the array. Your function should be as efficient as possible.

**Requirements:**
- Function signature: `void countOccurrences(int arr[], int n)`
- Print each unique number and its frequency
- Analyze and state the time complexity of your solution

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

void countOccurrences(int arr[], int n) {
    // Your code here
    // Hint: Consider using a simple nested loop approach first
    
}

int main() {
    int arr[] = {5, 2, 8, 2, 5, 1, 8, 5, 9, 2};
    int n = 10;
    
    cout << "Array: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl << endl;
    
    countOccurrences(arr, n);
    
    return 0;
}
```

**Sample Input:**
```
Array: 5 2 8 2 5 1 8 5 9 2
```

**Sample Output:**
```
Array: 5 2 8 2 5 1 8 5 9 2

Number Frequency:
5 appears 3 times
2 appears 3 times
8 appears 2 times
1 appears 1 times
9 appears 1 times
```

---

### Task 2: Power of Two Checker with Visualization

Write a function `isPowerOfTwo` that determines if a number is a power of 2 by repeatedly dividing by 2. The function should also visualize the division steps and count the number of iterations.

**Requirements:**
- Function signature: `bool isPowerOfTwo(int n)`
- Print each division step
- Count and display the number of iterations
- Return true if n is a power of 2, false otherwise
- Analyze the time complexity

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

bool isPowerOfTwo(int n) {
    // Handle edge cases
    if (n <= 0) {
        return false;
    }
    
    cout << "Checking if " << n << " is a power of 2..." << endl;
    int iterations = 0;
    
    // Your code here
    // Hint: Keep dividing by 2 until you reach 1 or an odd number
    
    
    return false; // Replace with your logic
}

int main() {
    int testValues[] = {16, 18, 64, 100, 128};
    
    for(int value : testValues) {
        bool result = isPowerOfTwo(value);
        cout << "Result: " << (result ? "YES" : "NO") << endl;
        cout << string(40, '-') << endl << endl;
    }
    
    return 0;
}
```

**Sample Input (built into code):**
```
Test values: 16, 18, 64, 100, 128
```

**Sample Output (for n=16):**
```
Checking if 16 is a power of 2...
Step 1: 16 ÷ 2 = 8
Step 2: 8 ÷ 2 = 4
Step 3: 4 ÷ 2 = 2
Step 4: 2 ÷ 2 = 1
Iterations: 4
Result: YES
----------------------------------------
```

**Sample Output (for n=18):**
```
Checking if 18 is a power of 2...
Step 1: 18 ÷ 2 = 9
9 is odd and not equal to 1
Iterations: 1
Result: NO
----------------------------------------
```

---

## Part 3: Summary Table

### Lecture Topics and Related Tutorial Tasks

| Lecture Topic | Tutorial Section | Related Tasks |
|---------------|------------------|---------------|
| Loop Analysis and O(n) Complexity | Section 16: For Loops | Task 16.1 (Multiplication table) |
| Nested Loops and O(n²) Complexity | Section 17: Further For Loops | Task 17.1 (Multiplication grid) |
| Sequential Operations | Section 8: Assignment Operators | Task 8.1 (Sequential operations) |
| Array Iteration | Section 22: Array Iteration | Task 22.1 (Finding min/max, sum) |
| Searching Arrays | Section 26: Searching Arrays | Task 26.1 (Linear search with comparison count) |
| Logarithmic Complexity Patterns | Section 12: While Loops | Task 12.1 (Number guessing - binary search concept) |
| Function Analysis | Section 19: Functions with Parameters | Task 19.1 (Rectangle calculations) |
| Multiple Loops Analysis | Section 21: Arrays | Task 21.1 (Array operations) |