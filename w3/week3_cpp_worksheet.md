# Lecture 3: C++ Pointers and References Worksheet

## Part 1: Quiz Questions

### Question 1: Fill in the Blanks

Complete the following sentences using the words from the word bank below:

When you declare a variable in C++, the compiler allocates a block of __________ to store its value. Each byte in memory has a unique __________. To find where a variable is stored, you can use the __________ operator, which is written as `&`. A __________ is a special variable that stores the memory address of another variable. To access the value stored at a pointer's address, you use the __________ operator, which is also written as `*`.

**Word Bank:** `dereference`, `address`, `memory`, `pointer`, `address-of`

---

### Question 2: Fill in the Blanks

Complete the following code snippet using the words from the word bank below:

```cpp
int value = 42;
int* ptr = __________;        // Store address of value
cout << __________ << endl;    // Print the address stored in ptr
cout << __________ << endl;    // Print the value 42 via pointer
__________ = 100;              // Modify value through pointer
```

**Word Bank:** `*ptr`, `&value`, `ptr`, `*ptr`

---

### Question 3: Multiple Choice

What will be the output of the following code?

```cpp
int x = 10;
int* p = &x;
*p = 20;
cout << x << endl;
```

A) 10  
B) 20  
C) The memory address of x  
D) Compilation error  

---

### Question 4: Multiple Choice

Which of the following function declarations will allow the function to modify the original variable passed to it?

A) `void modify(int value)`  
B) `void modify(int& value)`  
C) `void modify(int* value)`  
D) Both B and C  

---

## Part 2: Programming Tasks

### Task 1: Calculate Statistics with Multiple Return Values

Write a function called `calculateStats` that takes an array of integers and its size, and uses reference parameters to return three values: the minimum, maximum, and average of the numbers in the array.

**Requirements:**
- Function should use pass-by-reference for the three output parameters
- Calculate and store the minimum value
- Calculate and store the maximum value
- Calculate and store the average as a double

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

// TODO: Implement this function
void calculateStats(int arr[], int size, int& min, int& max, double& avg) {
    // Your code here
}

int main() {
    int numbers[] = {45, 23, 67, 12, 89, 34, 56};
    int size = 7;
    int minimum, maximum;
    double average;
    
    calculateStats(numbers, size, minimum, maximum, average);
    
    cout << "Minimum: " << minimum << endl;
    cout << "Maximum: " << maximum << endl;
    cout << "Average: " << average << endl;
    
    return 0;
}
```

**Expected Output:**
```
Minimum: 12
Maximum: 89
Average: 46.5714
```

---

### Task 2: Pointer-Based Array Reversal

Write a function called `reverseArray` that takes a pointer to an integer array and its size, and reverses the array in-place (modifies the original array). Use pointer arithmetic to access array elements.

**Requirements:**
- Function should take an `int*` pointer and size
- Reverse the array in-place (don't create a new array)
- Use pointer arithmetic (e.g., `*(ptr + i)`) instead of array indexing at least once

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

// TODO: Implement this function
void reverseArray(int* arr, int size) {
    // Your code here
}

void printArray(int* arr, int size) {
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

int main() {
    int numbers[] = {10, 20, 30, 40, 50};
    int size = 5;
    
    cout << "Original array: ";
    printArray(numbers, size);
    
    reverseArray(numbers, size);
    
    cout << "Reversed array: ";
    printArray(numbers, size);
    
    return 0;
}
```

**Expected Output:**
```
Original array: 10 20 30 40 50 
Reversed array: 50 40 30 20 10 
```

---

## Part 3: Summary Table

### Mapping Lecture Topics to Tutorial Tasks

| Lecture Topic | Relevant Tutorial Section(s) | Task Number(s) |
|---------------|------------------------------|----------------|
| Memory Addresses and Address-of Operator (&) | Section 6: Data Types | Task 6.1 |
| Pointers: Declaration and Initialization | Section 18: Functions (Methods) | Task 18.1 |
| Pass by Value vs Pass by Reference | Section 19: Functions with Parameters | Task 19.1 |
| Practical Applications: Multiple Return Values | Section 20: Testing Functions | Task 20.1 |
| Working with Arrays via Pointers | Section 21: Arrays | Task 21.1 |
| Array Iteration with Pointers | Section 22: Array Iteration | Task 22.1 |

---

**Note:** This worksheet is designed to reinforce the concepts covered in Lecture 3 on C++ Pointers and References. Complete all sections to ensure you understand memory management, pointer operations, and different parameter-passing mechanisms in C++.
