# Week 5: C++ Memory Management Worksheet

## Quiz

### Question 1: Fill in the Blanks

Complete the following paragraph about dynamic memory allocation in C++:

"To allocate memory dynamically in C++, we use the __________ operator, which returns a __________ to the allocated memory on the __________. When we are finished using this memory, we must use the __________ operator to free it. If we forget to do this, we create a __________."

**Word Bank (scrambled):** `heap`, `memory leak`, `delete`, `new`, `pointer`

---

### Question 2: Fill in the Blanks

Complete the following paragraph about the stack and heap:

"The __________ is a region of memory used for automatic storage with __________ size. It operates in a __________ manner. In contrast, the __________ is used for dynamic allocation and is limited only by available __________. Variables on the stack are __________ destroyed when they go out of scope."

**Word Bank (scrambled):** `automatically`, `limited`, `RAM`, `stack`, `LIFO`, `heap`

---

### Question 3: Fill in the Blanks

Complete the following paragraph about destructors:

"A destructor is a special member function with the same name as the class preceded by a __________. It has no __________ and no __________ type. Destructors are called __________ when an object goes out of __________. To ensure proper cleanup in inheritance hierarchies, destructors should be declared __________."

**Word Bank (scrambled):** `virtual`, `scope`, `parameters`, `return`, `tilde (~)`, `automatically`

---

### Question 4: Fill in the Blanks

Complete the following paragraph about array deallocation:

"When deallocating a dynamically allocated array, we must use __________ instead of just __________. Using the wrong form leads to __________ behavior. For a single object allocated with __________, we use the standard __________ operator without brackets."

**Word Bank (scrambled):** `delete`, `new`, `undefined`, `delete`, `delete[]`

---

### Question 5: Multiple Choice

What happens when you try to create a very large static array on the stack?

A) The program runs normally but slowly  
B) The array is automatically moved to the heap  
C) A stack overflow error occurs  
D) The compiler converts it to a dynamic array

---

### Question 6: Multiple Choice

Which of the following is TRUE about the resize operation on a dynamic array?

A) You can resize in-place by calling `realloc()` on the pointer  
B) You must allocate new memory, copy data, and delete the old block  
C) The `resize()` function is built into C++ for all pointer types  
D) Resizing is automatic when you exceed the array bounds

---

### Question 7: Multiple Choice

What is the purpose of setting a pointer to `nullptr` after calling `delete`?

A) It frees additional memory that `delete` missed  
B) It prevents the possibility of a dangling pointer being dereferenced  
C) It is required by the C++ standard or the program won't compile  
D) It automatically calls the destructor again

---

## Tasks

### Task 1: String Storage Manager

Create a class called `StringStorage` that manages dynamic memory for storing a C-style string (character array). The class should:

- Allocate memory in the constructor based on the string length
- Copy the provided string into the allocated memory
- Provide a method to display the stored string
- Properly deallocate memory in the destructor
- Track and display the memory size allocated

**Code Scaffolding:**

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class StringStorage {
private:
    char* data;
    int capacity;
    
public:
    // Constructor: allocate memory and copy the string
    StringStorage(const char* str) {
        // TODO: Calculate length (use strlen)
        // TODO: Allocate memory (length + 1 for null terminator)
        // TODO: Copy the string (use strcpy)
        // TODO: Display allocation message
    }
    
    // Destructor: free memory
    ~StringStorage() {
        // TODO: Delete allocated memory
        // TODO: Display deallocation message
    }
    
    // Display the stored string and capacity
    void display() const {
        // TODO: Print the string and capacity
    }
    
    // Get the capacity
    int getCapacity() const {
        return capacity;
    }
};

int main() {
    StringStorage str1("Hello, World!");
    str1.display();
    
    StringStorage str2("C++ Memory Management");
    str2.display();
    
    return 0;
}
```

**Expected Output:**
```
Allocated 14 bytes for string
String: Hello, World! (Capacity: 14 bytes)
Allocated 22 bytes for string
String: C++ Memory Management (Capacity: 22 bytes)
Freed 22 bytes
Freed 14 bytes
```

---

### Task 2: Dynamic Matrix Class

Create a simple `Matrix` class that stores a 2D array of integers dynamically. The class should:

- Accept rows and columns in the constructor
- Initialize all elements to zero
- Provide a method to set values at a specific position
- Provide a method to display the matrix
- Properly manage memory allocation and deallocation

**Code Scaffolding:**

```cpp
#include <iostream>
using namespace std;

class Matrix {
private:
    int** data;
    int rows;
    int cols;
    
public:
    // Constructor: allocate 2D array
    Matrix(int r, int c) : rows(r), cols(c) {
        // TODO: Allocate array of row pointers
        
        // TODO: For each row, allocate an array of columns
        
        // TODO: Initialize all elements to zero
        
        cout << "Created " << rows << "x" << cols << " matrix" << endl;
    }
    
    // Destructor: free 2D array
    ~Matrix() {
        // TODO: Delete each row
        
        // TODO: Delete the array of row pointers
        
        cout << "Matrix destroyed" << endl;
    }
    
    // Set value at position (r, c)
    void set(int r, int c, int value) {
        // TODO: Set data[r][c] = value
    }
    
    // Display the matrix
    void display() const {
        // TODO: Print matrix in grid format
    }
};

int main() {
    Matrix mat(3, 4);
    
    // Set some values
    mat.set(0, 0, 1);
    mat.set(0, 3, 2);
    mat.set(1, 1, 5);
    mat.set(2, 2, 9);
    
    cout << "\nMatrix contents:" << endl;
    mat.display();
    
    return 0;
}
```

**Expected Output:**
```
Created 3x4 matrix

Matrix contents:
1  0  0  2  
0  5  0  0  
0  0  9  0  
Matrix destroyed
```

---

**End of Worksheet**