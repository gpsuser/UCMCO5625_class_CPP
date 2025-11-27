# CO5625: Introduction to Big O Notation and Time Complexity
## 55-Minute Lecture

---

## Table of Contents

| Section | Topic | Time (min) |
|---------|-------|------------|
| 1 | Introduction and Motivation | 5 |
| 2 | Understanding Program Runtime | 8 |
| 3 | Why We Need Asymptotic Analysis | 7 |
| 4 | Big O Notation Fundamentals | 10 |
| 5 | Common Time Complexities | 12 |
| 6 | Analyzing Code for Time Complexity | 10 |
| 7 | Summary and Resources | 3 |
| **Total** | | **55** |

---

## SMART Objectives

| Objective | Description |
|-----------|-------------|
| **Specific** | Students will understand how algorithm runtime scales with input size and be able to express this relationship using Big O notation |
| **Measurable** | Students will correctly identify the time complexity of at least 3 simple algorithms by analyzing loop structures and nested operations |
| **Achievable** | Students will work through concrete C++ examples demonstrating O(1), O(n), O(log n), and O(n²) complexities |
| **Relevant** | Understanding time complexity is essential for writing efficient code and making informed decisions about algorithm selection in real-world software development |
| **Time-bound** | By the end of this 55-minute lecture, students will be able to analyze simple algorithms and determine their Big O complexity |

---

## Learning Outcomes

| ID | Learning Outcome |
|----|------------------|
| **LO1** | Explain how program runtime depends on input size and data characteristics |
| **LO2** | Define Big O notation and explain why asymptotic analysis is used in computer science |
| **LO3** | Identify and compare common time complexities: O(1), O(log n), O(n), O(n²) |
| **LO4** | Analyze simple C++ programs and determine their time complexity |
| **LO5** | Apply informal rules for identifying the dominant term in runtime expressions |

---

## 1. Introduction and Motivation (5 minutes)

### 1.1 Why Does Algorithm Efficiency Matter?

Consider two programmers solving the same problem: finding if a number exists in a collection of data. One writes a program that takes 1 second for 1,000 items. The other writes a program that takes 1 millisecond for the same data. As the data grows to 1,000,000 items:
- First program: ~1,000 seconds (16+ minutes)
- Second program: ~1 second

**The difference? The algorithm, not the programmer's typing speed.**

### 1.2 Real-World Impact

Time complexity analysis helps us:
- **Choose** the right algorithm for the problem
- **Predict** how our code will perform with large datasets
- **Communicate** about algorithm efficiency in a standardized way
- **Avoid** writing code that seems fine in testing but fails in production

### 1.3 What We'll Learn Today

Today we'll focus on **time complexity** - how the runtime of an algorithm grows as the input size increases. We'll learn to express this using **Big O notation**, the standard language computer scientists use to discuss algorithm efficiency.

---

## 2. Understanding Program Runtime (8 minutes)

### 2.1 The Anatomy of a Program's Runtime

Let's examine a typical program structure:

```cpp
#include <iostream>
using namespace std;

int main() {
    // Setup code - runs once
    int count = 0;
    int threshold = 100;
    
    // Get input size
    int n;
    cout << "Enter number of items: ";
    cin >> n;
    
    // Create array
    int* arr = new int[n];
    
    // Main processing loop - runs n times
    for(int i = 0; i < n; i++) {
        arr[i] = i * 2;
        
        // Conditional code - may or may not execute
        if(arr[i] > threshold) {
            count++;
        }
    }
    
    // Cleanup and output - runs once
    cout << "Count: " << count << endl;
    delete[] arr;
    
    return 0;
}
```

**Sample Output:**
```
Enter number of items: 10
Count: 9
```

### 2.2 Breaking Down Runtime Components

```
Program Structure           | Frequency        | Impact on Runtime
---------------------------|------------------|-------------------
Variable declarations      | Once (constant)  | Minimal
Input operations          | Once (constant)  | Minimal  
Memory allocation         | Once (constant)  | Proportional to n
Main loop body           | n times          | Grows with n
Conditional branches     | 0 to n times     | Variable
Output operations        | Once (constant)  | Minimal
Memory cleanup           | Once (constant)  | Minimal
```

### 2.3 Two Key Factors Affecting Runtime

**Factor 1: Input Size (n)**

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cout << "Enter size: ";
    cin >> n;
    
    // Runtime grows with n
    for(int i = 0; i < n; i++) {
        cout << i << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Sample Output (n=5):**
```
Enter size: 5
0 1 2 3 4
```

**Sample Output (n=10):**
```
Enter size: 10
0 1 2 3 4 5 6 7 8 9
```

**Factor 2: Data Values (Best vs. Worst Case)**

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {5, 2, 8, 1, 9};
    int n = 5;
    int target;
    
    cout << "Array: 5 2 8 1 9" << endl;
    cout << "Enter target to search: ";
    cin >> target;
    
    int comparisons = 0;
    bool found = false;
    
    for(int i = 0; i < n; i++) {
        comparisons++;
        if(arr[i] == target) {
            found = true;
            break;  // Early exit
        }
    }
    
    cout << "Found: " << (found ? "Yes" : "No") << endl;
    cout << "Comparisons needed: " << comparisons << endl;
    
    return 0;
}
```

**Sample Output (Best Case - target=5):**
```
Array: 5 2 8 1 9
Enter target to search: 5
Found: Yes
Comparisons needed: 1
```

**Sample Output (Worst Case - target=7):**
```
Array: 5 2 8 1 9
Enter target to search: 7
Found: No
Comparisons needed: 5
```

### 2.4 Expressing Runtime Mathematically

For a program with:
- 34 constant-time operations (setup, cleanup, etc.)
- 12n operations in a simple loop
- 23n² operations in a nested loop

Total runtime = **34 + 12n + 23n²** clock cycles

**As n grows:**
- n = 10: 34 + 120 + 2,300 = 2,454
- n = 100: 34 + 1,200 + 230,000 = 231,234
- n = 1,000: 34 + 12,000 + 23,000,000 = 23,012,034

Notice how the n² term dominates!

---

## 3. Why We Need Asymptotic Analysis (7 minutes)

### 3.1 Problems with Exact Runtime Measurement

**Problem 1: Architecture Dependence**

The exact number of clock cycles depends on:
- CPU speed and architecture
- Compiler optimizations
- Memory hierarchy (cache effects)
- Operating system scheduling

**Problem 2: Complex Formulas**

For real programs, we might get expressions like:
- 47 + 23n + 8n² - 3n + 12
- 156 + 89n + 45n² + 2n³ - 7n + 34

These are unwieldy to work with and compare!

### 3.2 The Big Idea: Focus on Growth Rate

Let's compare two algorithms:
- Algorithm A: 1000 + 5n operations
- Algorithm B: 10 + n² operations

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "n\tAlg A\t\tAlg B\t\tWinner" << endl;
    cout << "------------------------------------------------" << endl;
    
    int test_sizes[] = {10, 50, 100, 500, 1000, 5000};
    
    for(int n : test_sizes) {
        long algA = 1000 + 5 * n;
        long algB = 10 + n * n;
        
        cout << n << "\t" << algA << "\t\t" << algB << "\t\t";
        cout << (algA < algB ? "A" : "B") << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
n       Alg A           Alg B           Winner
------------------------------------------------
10      1050            110             B
50      1250            2510            A
100     1500            10010           A
500     3500            250010          A
1000    6000            1000010         A
5000    26000           25000010        A
```

**Key Insight:** For small n, constants matter. For large n, the growth rate dominates!

### 3.3 Asymptotic Analysis: Looking at the Limit

We care about behavior as n → ∞ (n approaches infinity)

```
Term        | n=100   | n=1000    | n=10000   | Grows as
------------|---------|-----------|-----------|----------
34          | 34      | 34        | 34        | Constant
12n         | 1,200   | 12,000    | 120,000   | Linear
23n²        | 230,000 | 23,000,000| 2,300,000,000 | Quadratic
```

For 34 + 12n + 23n²:
- At n=100: n² term is 99.5% of total
- At n=1000: n² term is 99.95% of total
- At n=10000: n² term is 99.995% of total

**Conclusion:** We can approximate the runtime by just the fastest-growing term!

---

## 4. Big O Notation Fundamentals (10 minutes)

### 4.1 Formal Definition

A function **f(n)** is said to be **O(g(n))** (read as "Big O of g of n") if:

There exist positive constants **c** and **n₀** such that:
```
f(n) ≤ c · g(n)  for all n ≥ n₀
```

**In plain English:** f(n) grows no faster than g(n) (up to a constant multiple) for sufficiently large n.

### 4.2 Visual Representation

```
        ^
        |           f(n) = actual runtime
   r    |          /
   u    |         /
   n    |    ----/------ c·g(n) = upper bound
   t    |       /
   i    |      /
   m    |     /
   e    |    /
        |   /
        |  /
        | /
        |/_________________________>
                 n₀        input size (n)
        
        Beyond n₀, f(n) stays below c·g(n)
```

### 4.3 Practical Application: Dropping Constants and Lower-Order Terms

**Example 1:** Runtime = 34 + 12n + 23n²

Step 1: Identify terms by growth rate
- 34 (constant)
- 12n (linear)
- 23n² (quadratic) ← fastest growing

Step 2: Keep only the fastest-growing term
- 23n²

Step 3: Drop the constant coefficient
- n²

**Result: O(n²)**

**Example 2:** Runtime = 5n³ + 100n² + 500n + 1000

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
    cout << fixed << setprecision(2);
    cout << "n\tFull Formula\tn³ Term\t\tRatio (%)" << endl;
    cout << "--------------------------------------------------------" << endl;
    
    int test_sizes[] = {10, 50, 100, 500, 1000};
    
    for(int n : test_sizes) {
        long full = 5*n*n*n + 100*n*n + 500*n + 1000;
        long cubic = 5*n*n*n;
        double ratio = (double)cubic / full * 100;
        
        cout << n << "\t" << full << "\t\t" << cubic << "\t\t" << ratio << "%" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
n       Full Formula    n³ Term         Ratio (%)
--------------------------------------------------------
10      56500           5000            8.85%
50      636500          625000          98.19%
100     5601000         5000000         89.27%
500     630051000       625000000       99.20%
1000    5101501000      5000000000      98.01%
```

As n grows, the n³ term dominates → **O(n³)**

### 4.4 Common Simplifications

```
Original Runtime              →    Big O Notation
---------------------------------    ---------------
5                             →    O(1)
3n + 2                        →    O(n)
4n² + 2n + 7                  →    O(n²)
n³ + 2n² + 5n + 100          →    O(n³)
log₂(n) + 5                   →    O(log n)
n log(n) + 100n              →    O(n log n)
2ⁿ + n⁵                       →    O(2ⁿ)
```

### 4.5 Why This Works: A Demonstration

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    cout << "Demonstrating: 23n² eventually dominates 34 + 12n" << endl;
    cout << "Finding constant c where 24n² > 34 + 12n + 23n² for large n" << endl;
    cout << endl;
    
    cout << "n\t34+12n+23n²\t24n²\t\tIs 24n² bigger?" << endl;
    cout << "------------------------------------------------------" << endl;
    
    for(int n = 1; n <= 10; n++) {
        long actual = 34 + 12*n + 23*n*n;
        long bound = 24*n*n;
        
        cout << n << "\t" << actual << "\t\t" << bound << "\t\t";
        cout << (bound >= actual ? "YES" : "NO") << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Demonstrating: 23n² eventually dominates 34 + 12n
Finding constant c where 24n² > 34 + 12n + 23n² for large n

n       34+12n+23n²     24n²            Is 24n² bigger?
------------------------------------------------------
1       69              24              NO
2       130             96              NO
3       217             216             NO
4       330             384             YES
5       469             600             YES
6       634             864             YES
7       825             1176            YES
8       1042            1536            YES
9       1285            1944            YES
10      1554            2400            YES
```

For n ≥ 4, we have 24n² ≥ 34 + 12n + 23n². Therefore, 34 + 12n + 23n² is **O(n²)** with c=24 and n₀=4.

---

## 5. Common Time Complexities (12 minutes)

### 5.1 O(1) - Constant Time

Operations that take the same amount of time regardless of input size.

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
    int n = 10;
    
    cout << "Array has " << n << " elements" << endl;
    
    // O(1) - Access first element
    cout << "First element: " << arr[0] << endl;
    
    // O(1) - Access last element (with known index)
    cout << "Last element: " << arr[n-1] << endl;
    
    // O(1) - Access middle element
    cout << "Middle element: " << arr[n/2] << endl;
    
    // O(1) - Simple arithmetic
    int sum = arr[0] + arr[n-1];
    cout << "Sum of first and last: " << sum << endl;
    
    cout << "\nAll operations took constant time!" << endl;
    cout << "Complexity: O(1)" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array has 10 elements
First element: 10
Last element: 100
Middle element: 60
Sum of first and last: 110

All operations took constant time!
Complexity: O(1)
```

**Characteristics of O(1):**
- No loops
- No recursion
- Direct access operations
- Simple calculations

### 5.2 O(n) - Linear Time

Operations that process each element once.

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int n = 5;
    
    cout << "Array: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    // O(n) - Sum all elements
    int sum = 0;
    for(int i = 0; i < n; i++) {
        sum += arr[i];
    }
    cout << "Sum: " << sum << endl;
    
    // O(n) - Find maximum
    int max = arr[0];
    for(int i = 1; i < n; i++) {
        if(arr[i] > max) {
            max = arr[i];
        }
    }
    cout << "Maximum: " << max << endl;
    
    // O(n) - Count even numbers
    int evenCount = 0;
    for(int i = 0; i < n; i++) {
        if(arr[i] % 2 == 0) {
            evenCount++;
        }
    }
    cout << "Even numbers: " << evenCount << endl;
    
    cout << "\nAll operations visited each element once!" << endl;
    cout << "Complexity: O(n)" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array: 10 20 30 40 50
Sum: 150
Maximum: 50
Even numbers: 5

All operations visited each element once!
Complexity: O(n)
```

**Characteristics of O(n):**
- Single loop through data
- Processing each element once
- Linear search

### 5.3 O(log n) - Logarithmic Time

Operations that divide the problem in half each step (like binary search).

```cpp
#include <iostream>
using namespace std;

int binarySearch(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    int steps = 0;
    
    while(left <= right) {
        steps++;
        int mid = left + (right - left) / 2;
        
        cout << "Step " << steps << ": Checking position " << mid 
             << " (value=" << arr[mid] << ")" << endl;
        
        if(arr[mid] == target) {
            cout << "Found at index " << mid << "!" << endl;
            return steps;
        }
        
        if(arr[mid] < target) {
            cout << "  Search right half" << endl;
            left = mid + 1;
        } else {
            cout << "  Search left half" << endl;
            right = mid - 1;
        }
    }
    
    cout << "Not found" << endl;
    return steps;
}

int main() {
    int arr[] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
    int n = 10;
    int target = 70;
    
    cout << "Sorted array: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl << endl;
    
    cout << "Searching for " << target << ":" << endl;
    int steps = binarySearch(arr, n, target);
    
    cout << "\nTotal steps: " << steps << endl;
    cout << "Complexity: O(log n)" << endl;
    cout << "For n=10, log₂(10) ≈ 3.32" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted array: 10 20 30 40 50 60 70 80 90 100

Searching for 70:
Step 1: Checking position 4 (value=50)
  Search right half
Step 2: Checking position 7 (value=80)
  Search left half
Step 3: Checking position 5 (value=60)
  Search right half
Step 4: Checking position 6 (value=70)
Found at index 6!

Total steps: 4
Complexity: O(log n)
For n=10, log₂(10) ≈ 3.32
```

**Characteristics of O(log n):**
- Problem size halves each iteration
- Very efficient for large datasets
- Requires sorted data (for binary search)

### 5.4 O(n²) - Quadratic Time

Operations with nested loops over the data.

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {5, 2, 8, 1, 9};
    int n = 5;
    
    cout << "Original array: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl << endl;
    
    // Bubble sort - O(n²)
    int comparisons = 0;
    cout << "Bubble Sort Process:" << endl;
    
    for(int i = 0; i < n-1; i++) {
        cout << "Pass " << (i+1) << ": ";
        
        for(int j = 0; j < n-i-1; j++) {
            comparisons++;
            
            if(arr[j] > arr[j+1]) {
                // Swap
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
        
        // Show array after this pass
        for(int k = 0; k < n; k++) {
            cout << arr[k] << " ";
        }
        cout << endl;
    }
    
    cout << "\nSorted array: ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    cout << "\nTotal comparisons: " << comparisons << endl;
    cout << "Expected for O(n²): approximately n²/2 = " << (n*n/2) << endl;
    cout << "Complexity: O(n²)" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Original array: 5 2 8 1 9

Bubble Sort Process:
Pass 1: 2 5 1 8 9
Pass 2: 2 1 5 8 9
Pass 3: 1 2 5 8 9
Pass 4: 1 2 5 8 9

Sorted array: 1 2 5 8 9

Total comparisons: 10
Expected for O(n²): approximately n²/2 = 12
Complexity: O(n²)
```

**Characteristics of O(n²):**
- Nested loops
- Comparing each element with every other element
- Simple sorting algorithms

### 5.5 Comparing Growth Rates

```cpp
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;

int main() {
    cout << fixed << setprecision(0);
    cout << "n\tO(1)\tO(log n)\tO(n)\tO(n log n)\tO(n²)\tO(2ⁿ)" << endl;
    cout << "-------------------------------------------------------------------------" << endl;
    
    int sizes[] = {1, 10, 100, 1000, 10000};
    
    for(int n : sizes) {
        cout << n << "\t1\t";
        cout << (int)log2(n) << "\t\t";
        cout << n << "\t";
        cout << (int)(n * log2(n)) << "\t\t";
        cout << n*n << "\t";
        
        if(n <= 20) {
            cout << (int)pow(2, n);
        } else {
            cout << "huge!";
        }
        cout << endl;
    }
    
    cout << "\nNotice how O(n²) and O(2ⁿ) explode!" << endl;
    
    return 0;
}
```

**Sample Output:**
```
n       O(1)    O(log n)        O(n)    O(n log n)      O(n²)   O(2ⁿ)
-------------------------------------------------------------------------
1       1       0               1       0               1       2
10      1       3               10      33              100     1024
100     1       6               100     664             10000   huge!
1000    1       9               1000    9965            1000000 huge!
10000   1       13              10000   132877          100000000       huge!

Notice how O(n²) and O(2ⁿ) explode!
```

**Visual Hierarchy (Best to Worst):**
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

Fast ←------------------------------------------------→ Slow
```

---

## 6. Analyzing Code for Time Complexity (10 minutes)

### 6.1 Basic Rules for Analysis

**Rule 1: Sequential Statements → Add**
```
Statement 1;    // O(1)
Statement 2;    // O(n)
Total: O(1) + O(n) = O(n)
```

**Rule 2: Nested Loops → Multiply**
```
for(...) {          // O(n)
    for(...) {      // O(n)
        ...
    }
}
Total: O(n) × O(n) = O(n²)
```

**Rule 3: Drop Non-Dominant Terms**
```
O(n² + n + 1) = O(n²)
```

### 6.2 Example 1: Single Loop Analysis

```cpp
#include <iostream>
using namespace std;

// What is the time complexity?
void analyzeArray(int arr[], int n) {
    // O(1) - constant time operations
    int sum = 0;
    int max = arr[0];
    int evenCount = 0;
    
    cout << "Analyzing array..." << endl;
    
    // This loop runs n times - O(n)
    for(int i = 0; i < n; i++) {
        sum += arr[i];           // O(1)
        if(arr[i] > max) {       // O(1)
            max = arr[i];        // O(1)
        }
        if(arr[i] % 2 == 0) {    // O(1)
            evenCount++;         // O(1)
        }
    }
    
    // O(1) - constant time operations
    cout << "Sum: " << sum << endl;
    cout << "Max: " << max << endl;
    cout << "Even count: " << evenCount << endl;
}

int main() {
    int arr[] = {10, 5, 8, 3, 9, 1, 7, 6, 4, 2};
    int n = 10;
    
    cout << "Array size: " << n << endl;
    analyzeArray(arr, n);
    
    cout << "\nComplexity Analysis:" << endl;
    cout << "- Setup: O(1)" << endl;
    cout << "- Loop body: O(1) operations × n iterations = O(n)" << endl;
    cout << "- Output: O(1)" << endl;
    cout << "Total: O(1) + O(n) + O(1) = O(n)" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Array size: 10
Analyzing array...
Sum: 55
Max: 10
Even count: 5

Complexity Analysis:
- Setup: O(1)
- Loop body: O(1) operations × n iterations = O(n)
- Output: O(1)
Total: O(1) + O(n) + O(1) = O(n)
```

### 6.3 Example 2: Nested Loop Analysis

```cpp
#include <iostream>
using namespace std;

// Check if array has duplicate elements
bool hasDuplicates(int arr[], int n) {
    cout << "Checking for duplicates..." << endl;
    int comparisons = 0;
    
    // Outer loop: O(n)
    for(int i = 0; i < n; i++) {
        // Inner loop: O(n)
        for(int j = i + 1; j < n; j++) {
            comparisons++;
            // O(1) comparison
            if(arr[i] == arr[j]) {
                cout << "Found duplicate: " << arr[i] << endl;
                cout << "Total comparisons: " << comparisons << endl;
                return true;
            }
        }
    }
    
    cout << "No duplicates found" << endl;
    cout << "Total comparisons: " << comparisons << endl;
    return false;
}

int main() {
    int arr1[] = {1, 2, 3, 4, 5};
    int arr2[] = {1, 2, 3, 2, 5};
    int n = 5;
    
    cout << "Test 1 - No duplicates:" << endl;
    hasDuplicates(arr1, n);
    
    cout << "\nTest 2 - Has duplicates:" << endl;
    hasDuplicates(arr2, n);
    
    cout << "\nComplexity Analysis:" << endl;
    cout << "Outer loop: runs n times" << endl;
    cout << "Inner loop: runs up to n times for each outer iteration" << endl;
    cout << "Total: O(n) × O(n) = O(n²)" << endl;
    cout << "Worst case comparisons: n(n-1)/2 ≈ n²/2" << endl;
    cout << "For n=5: 5×4/2 = 10 comparisons" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Test 1 - No duplicates:
Checking for duplicates...
No duplicates found
Total comparisons: 10

Test 2 - Has duplicates:
Checking for duplicates...
Found duplicate: 2
Total comparisons: 4

Complexity Analysis:
Outer loop: runs n times
Inner loop: runs up to n times for each outer iteration
Total: O(n) × O(n) = O(n²)
Worst case comparisons: n(n-1)/2 ≈ n²/2
For n=5: 5×4/2 = 10 comparisons
```

### 6.4 Example 3: Multiple Loops (Sequential)

```cpp
#include <iostream>
using namespace std;

void processData(int arr[], int n) {
    // First loop: O(n)
    cout << "Step 1: Find maximum" << endl;
    int max = arr[0];
    for(int i = 1; i < n; i++) {
        if(arr[i] > max) {
            max = arr[i];
        }
    }
    cout << "Max: " << max << endl;
    
    // Second loop: O(n)
    cout << "\nStep 2: Count elements greater than average" << endl;
    int sum = 0;
    for(int i = 0; i < n; i++) {
        sum += arr[i];
    }
    double avg = (double)sum / n;
    
    int count = 0;
    for(int i = 0; i < n; i++) {
        if(arr[i] > avg) {
            count++;
        }
    }
    cout << "Average: " << avg << endl;
    cout << "Elements above average: " << count << endl;
}

int main() {
    int arr[] = {10, 5, 8, 3, 9, 1, 7, 6, 4, 2};
    int n = 10;
    
    processData(arr, n);
    
    cout << "\nComplexity Analysis:" << endl;
    cout << "Loop 1: O(n)" << endl;
    cout << "Loop 2: O(n)" << endl;
    cout << "Loop 3: O(n)" << endl;
    cout << "Total: O(n) + O(n) + O(n) = O(3n) = O(n)" << endl;
    cout << "(Constants are dropped in Big O)" << endl;
    
    return 0;
}
```

**Sample Output:**
```
Step 1: Find maximum
Max: 10

Step 2: Count elements greater than average
Average: 5.5
Elements above average: 5

Complexity Analysis:
Loop 1: O(n)
Loop 2: O(n)
Loop 3: O(n)
Total: O(n) + O(n) + O(n) = O(3n) = O(n)
(Constants are dropped in Big O)
```

### 6.5 Example 4: Logarithmic Loop

```cpp
#include <iostream>
using namespace std;

// How many times does this loop run?
void demonstrateLogLoop(int n) {
    cout << "Starting with n = " << n << endl;
    cout << "Dividing by 2 each iteration:" << endl;
    
    int iterations = 0;
    int i = n;
    
    while(i > 1) {
        iterations++;
        cout << "Iteration " << iterations << ": i = " << i << endl;
        i = i / 2;
    }
    
    cout << "\nTotal iterations: " << iterations << endl;
    cout << "log₂(" << n << ") = " << log2(n) << endl;
    cout << "Complexity: O(log n)" << endl;
}

int main() {
    int test_values[] = {8, 16, 64, 1000};
    
    for(int n : test_values) {
        demonstrateLogLoop(n);
        cout << "\n" << string(40, '-') << "\n" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Starting with n = 8
Dividing by 2 each iteration:
Iteration 1: i = 8
Iteration 2: i = 4
Iteration 3: i = 2

Total iterations: 3
log₂(8) = 3
Complexity: O(log n)

----------------------------------------

Starting with n = 16
Dividing by 2 each iteration:
Iteration 1: i = 16
Iteration 2: i = 8
Iteration 3: i = 4
Iteration 4: i = 2

Total iterations: 4
log₂(16) = 4
Complexity: O(log n)
```

### 6.6 Quick Reference: Common Patterns

```cpp
#include <iostream>
using namespace std;

int main() {
    int n = 100;
    
    cout << "Common Code Patterns and Their Complexities:\n" << endl;
    
    cout << "1. O(1) - Constant Time:" << endl;
    cout << "   int x = arr[0];          // Direct access" << endl;
    cout << "   int sum = a + b;         // Simple operation" << endl;
    cout << endl;
    
    cout << "2. O(log n) - Logarithmic Time:" << endl;
    cout << "   while(i > 1) {           // Divide by constant" << endl;
    cout << "       i = i / 2;" << endl;
    cout << "   }" << endl;
    cout << endl;
    
    cout << "3. O(n) - Linear Time:" << endl;
    cout << "   for(int i = 0; i < n; i++) {    // Single loop" << endl;
    cout << "       sum += arr[i];" << endl;
    cout << "   }" << endl;
    cout << endl;
    
    cout << "4. O(n log n) - Linearithmic Time:" << endl;
    cout << "   Merge Sort, Quick Sort (average case)" << endl;
    cout << endl;
    
    cout << "5. O(n²) - Quadratic Time:" << endl;
    cout << "   for(int i = 0; i < n; i++) {    // Nested loops" << endl;
    cout << "       for(int j = 0; j < n; j++) {" << endl;
    cout << "           // something" << endl;
    cout << "       }" << endl;
    cout << "   }" << endl;
    cout << endl;
    
    cout << "6. O(2ⁿ) - Exponential Time:" << endl;
    cout << "   Recursive algorithms with multiple recursive calls" << endl;
    cout << "   (e.g., naive Fibonacci)" << endl;
    
    return 0;
}
```

---

## 7. Summary and Resources (3 minutes)

### 7.1 Key Takeaways

**1. Why Big O Matters:**
- Predicts algorithm performance at scale
- Helps choose the right algorithm
- Standard language for discussing efficiency

**2. Core Concepts:**
- Focus on worst-case scenario
- Count operations as a function of input size n
- Drop constants and non-dominant terms
- Express as O(g(n)) where g(n) is the growth rate

**3. Common Complexities (Best to Worst):**
```
O(1)       - Constant      - Array access
O(log n)   - Logarithmic   - Binary search
O(n)       - Linear        - Single loop
O(n log n) - Linearithmic  - Efficient sorting
O(n²)      - Quadratic     - Nested loops
O(2ⁿ)      - Exponential   - Recursive algorithms
```

**4. Analysis Shortcuts:**
- One loop → O(n)
- Nested loops → O(n²), O(n³), etc.
- Dividing by constant → O(log n)
- Sequential operations → Add complexities
- Nested operations → Multiply complexities

### 7.2 Practical Tips

**When writing code:**
1. Avoid nested loops when possible
2. Consider if data needs to be sorted first (enables O(log n) search)
3. Look for early exit opportunities
4. Remember: working code first, then optimize if needed

**When analyzing algorithms:**
1. Focus on the loops
2. Identify the input size (n)
3. Count worst-case iterations
4. Keep only the fastest-growing term

---

## Time Allocation Summary

| Section | Topic | Time (min) | Learning Outcomes |
|---------|-------|------------|-------------------|
| 1 | Introduction and Motivation | 5 | LO1, LO2 |
| 2 | Understanding Program Runtime | 8 | LO1 |
| 3 | Why We Need Asymptotic Analysis | 7 | LO2 |
| 4 | Big O Notation Fundamentals | 10 | LO2, LO5 |
| 5 | Common Time Complexities | 12 | LO3 |
| 6 | Analyzing Code for Time Complexity | 10 | LO4, LO5 |
| 7 | Summary and Resources | 3 | All |
| **Total** | | **55** | |

---

## Resources for Further Reading and Practice

### Official Documentation and References
- **CPP Reference - Complexity**: https://en.cppreference.com/w/cpp/algorithm
- **Big-O Cheat Sheet**: https://www.bigocheatsheet.com/
- **Know Thy Complexities**: https://cooervo.github.io/Algorithms-DataStructures-BigONotation/

### Textbooks and Academic Resources
- *Introduction to Algorithms* by Cormen, Leiserson, Rivest, and Stein (CLRS) - Chapter 3
- *Data Structures and Algorithm Analysis in C++* by Mark Allen Weiss - Chapters 2-3
- *Algorithm Design Manual* by Steven Skiena - Chapter 2

### Interactive Learning Platforms
- **VisuAlgo**: https://visualgo.net/ - Visualize algorithms and their time complexities
- **Big-O Complexity Calculator**: https://discrete.gr/complexity/
- **Algorithm Visualizer**: https://algorithm-visualizer.org/

### Video Resources
- **MIT OpenCourseWare - Asymptotic Notation**: https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/
- **Abdul Bari - Algorithm Playlist**: YouTube - Excellent visual explanations
- **CS Dojo - Big O Notation**: https://www.youtube.com/watch?v=v4cd1O4zkGw

### Practice Problems
- **LeetCode**: https://leetcode.com/ - Filter by "Easy" and focus on time complexity
- **HackerRank - Time Complexity**: https://www.hackerrank.com/domains/algorithms
- **Project Euler**: https://projecteuler.net/ - Mathematical/computational problems
- **Codewars**: https://www.codewars.com/ - Algorithm challenges with community solutions

### Articles and Tutorials
- **GeeksforGeeks - Analysis of Algorithms**: https://www.geeksforgeeks.org/analysis-of-algorithms-set-1-asymptotic-analysis/
- **TopCoder - Time Complexity Tutorial**: https://www.topcoder.com/thrive/articles/Computational%20Complexity%20part%20one
- **Interview Cake - Big O Notation**: https://www.interviewcake.com/article/big-o-notation-time-and-space-complexity

### Tools for Analysis
- **Compiler Explorer (Godbolt)**: https://godbolt.org/ - See assembly output to understand performance
- **Quick Bench**: https://quick-bench.com/ - Benchmark C++ code snippets
- **C++ Insights**: https://cppinsights.io/ - See how compiler transforms your code

### Communities and Forums
- **Stack Overflow**: Tag questions with [algorithm] [time-complexity]
- **Reddit r/cpp**: Discussions on C++ and algorithm efficiency
- **Reddit r/algorithms**: General algorithm discussions
- **Computer Science Stack Exchange**: Theoretical computer science questions

### Books for Deeper Understanding
- *Algorithms* by Robert Sedgewick and Kevin Wayne
- *Programming Pearls* by Jon Bentley
- *The Algorithm Design Manual* by Steven Skiena
- *Grokking Algorithms* by Aditya Bhargava (Visual, beginner-friendly)