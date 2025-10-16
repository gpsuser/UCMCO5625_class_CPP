# Week 1 - CO5625 C++ Further Programming and Algorithms

## C++ Overview: Part 1 - Fundamentals and Control Structures
### 90-Minute Lecture Session

### Learning Objectives
By the end of this lecture, students will be able to:
1. **Construct** a basic C++ program with proper structure, including headers, namespaces, and the main function
2. **Declare and manipulate** variables using appropriate primitive data types for given scenarios
3. **Apply** type casting techniques to convert between different data types safely
4. **Implement** scope rules to manage variable lifetime and accessibility effectively
5. **Design and implement** control structures (selection and iteration) to solve programming problems
6. **Create and manipulate** arrays for storing and processing collections of data

---

## Section 1: Structure of a Simple C++ Program (10 minutes)

### 1.1 Program Anatomy
C++ programs follow a specific structure that differs significantly from languages like Java. Unlike Java, C++ doesn't require everything to be encapsulated in classes - functions can exist independently.

#### Basic Program Template:
```cpp
#include <iostream>     // Preprocessor directive for input/output
#include <string>       // Additional headers as needed
using namespace std;    // Namespace declaration (optional but common)

int main()             // Entry point of the program
{
    // Program logic goes here
    return 0;          // Return status to operating system
}
```

### 1.2 Input/Output Operations
C++ uses stream-based I/O with special operators:
- **cout** (console output): Uses `<<` operator to send data to the console
- **cin** (console input): Uses `>>` operator to receive data from the user
- **endl**: Inserts a newline and flushes the output buffer

#### Example: Interactive Program
```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Please enter your age: " << endl;
    int age;
    cin >> age;
    cout << "In 10 years, you will be " << age + 10 << " years old." << endl;
    return 0;
}
```

### 1.3 Chaining Operations
The stream operators can be chained for multiple operations:
```cpp
int x, y, z;
cin >> x >> y >> z;  // Read three values in sequence
cout << "Sum: " << x + y + z << " Average: " << (x + y + z) / 3.0 << endl;
```

---

## Section 2: Variables and Primitive Data Types (12 minutes)

### 2.1 Variable Declaration and Initialization
Variables in C++ must be declared with a specific type before use. Each variable consists of:
- **Name**: Identifier following C++ naming conventions
- **Type**: Determines how memory is interpreted
- **Value**: The actual data stored

#### Declaration Syntax:
```cpp
type variable_name;                    // Declaration
type variable_name = initial_value;    // Declaration with initialization
```

### 2.2 Core Primitive Types

#### Essential Data Types:
- **int**: Integer values (typically 4 bytes, range: -2³¹ to 2³¹-1)
- **double**: Floating-point numbers (8 bytes, ~15-17 decimal digits precision)
- **char**: Single character (1 byte, ASCII/Unicode)
- **bool**: Boolean values (true/false)

#### Examples:
```cpp
int studentCount = 30;
double averageGrade = 85.7;
char letterGrade = 'A';
bool isPassing = true;
```

### 2.3 Extended Integer Types
C++ provides various integer sizes for different requirements:

```cpp
short smallNumber = 100;           // At least 16 bits
int normalNumber = 50000;          // At least 16 bits (typically 32)
long largeNumber = 1000000L;       // At least 32 bits
long long hugeNumber = 1000000000LL; // At least 64 bits
```

### 2.4 Signed vs Unsigned
Integer types can be signed (default) or unsigned:

```cpp
unsigned int positiveOnly = 42;    // Range: 0 to 2³²-1
signed int canBeNegative = -42;    // Range: -2³¹ to 2³¹-1

// Practical application: Array indices
unsigned int arrayIndex = 0;       // Indices are never negative
```

### 2.5 Character Types
Different character types for various encoding needs:

```cpp
char asciiChar = 'A';              // Basic ASCII character
char16_t utf16Char = u'世';        // UTF-16 character
char32_t utf32Char = U'🚀';        // UTF-32 character (emoji support)
wchar_t wideChar = L'界';          // Wide character (platform-dependent)
```

---

## Section 3: Type Casting (8 minutes)

### 3.1 C-Style Casting
Traditional casting inherited from C:

```cpp
double pi = 3.14159;
int wholePart = (int)pi;           // Truncates to 3
char letter = 'A';
int asciiValue = (int)letter;      // Gets ASCII value 65
```

### 3.2 Modern C++ Static Cast
Preferred method in modern C++ for compile-time type checking:

```cpp
double temperature = 98.6;
int roundedTemp = static_cast<int>(temperature);

// More complex example
int totalScore = 456;
int numTests = 5;
double average = static_cast<double>(totalScore) / numTests;  // 91.2
```

### 3.3 Implicit vs Explicit Casting
```cpp
// Implicit casting (automatic)
int x = 10;
double y = x;  // int automatically promoted to double

// Explicit casting (required to avoid data loss)
double precise = 3.7;
int truncated = static_cast<int>(precise);  // Must be explicit

// Warning example
int bigNumber = 70000;
short smallContainer = static_cast<short>(bigNumber);  // Potential overflow!
```

---

## Section 4: Scope and Variable Lifetime (10 minutes)

### 4.1 Block Scope
Variables exist only within their enclosing braces:

```cpp
int main() {
    int outerVar = 10;
    
    {  // New scope block
        int innerVar = 20;
        outerVar = 30;  // Can access outer scope
    }  // innerVar destroyed here
    
    // innerVar is not accessible here
    cout << outerVar;  // Outputs: 30
}
```

### 4.2 Function Scope
Parameters and local variables are scoped to functions:

```cpp
void calculateArea(double radius) {
    double pi = 3.14159;  // Local to this function
    double area = pi * radius * radius;
    cout << "Area: " << area << endl;
}  // radius, pi, and area destroyed here

int main() {
    calculateArea(5.0);
    // pi is not accessible here
}
```

### 4.3 Loop Scope
Variables declared in loop headers have loop scope:

```cpp
for (int i = 0; i < 10; i++) {
    int squared = i * i;  // Local to loop body
    cout << squared << " ";
}
// Neither i nor squared accessible here

// Different loops can reuse variable names
for (int i = 0; i < 5; i++) {  // New 'i' variable
    cout << i << endl;
}
```

### 4.4 Why Global Variables Are Problematic
Global variables should be avoided because they:
- Create hidden dependencies between functions
- Make code harder to test and debug
- Can be modified from anywhere, leading to unexpected behavior
- Increase coupling and reduce modularity

```cpp
// BAD PRACTICE - Don't do this!
int globalCounter = 0;  // Accessible everywhere

void incrementCounter() {
    globalCounter++;  // Hidden side effect
}

// GOOD PRACTICE - Pass as parameter
void incrementCounter(int& counter) {
    counter++;  // Explicit dependency
}
```

---

## Section 5: Control Structures - Selection (10 minutes)

### 5.1 If-Else Statements
Basic conditional execution:

```cpp
double grade = 75.5;

if (grade >= 90) {
    cout << "Grade: A" << endl;
} else if (grade >= 80) {
    cout << "Grade: B" << endl;
} else if (grade >= 70) {
    cout << "Grade: C" << endl;
} else if (grade >= 60) {
    cout << "Grade: D" << endl;
} else {
    cout << "Grade: F" << endl;
}
```

### 5.2 Nested Conditions
Complex decision trees:

```cpp
int age = 25;
bool hasLicense = true;
bool hasInsurance = true;

if (age >= 18) {
    if (hasLicense) {
        if (hasInsurance) {
            cout << "You can drive!" << endl;
        } else {
            cout << "You need insurance first." << endl;
        }
    } else {
        cout << "You need a license first." << endl;
    }
} else {
    cout << "You're too young to drive." << endl;
}
```

### 5.3 Switch Statements
Efficient multi-way branching for discrete values:

```cpp
char operation;
double num1 = 10, num2 = 5, result;

cout << "Enter operation (+, -, *, /): ";
cin >> operation;

switch(operation) {
    case '+':
        result = num1 + num2;
        cout << "Result: " << result << endl;
        break;
        
    case '-':
        result = num1 - num2;
        cout << "Result: " << result << endl;
        break;
        
    case '*':
        result = num1 * num2;
        cout << "Result: " << result << endl;
        break;
        
    case '/':
        if (num2 != 0) {
            result = num1 / num2;
            cout << "Result: " << result << endl;
        } else {
            cout << "Error: Division by zero!" << endl;
        }
        break;
        
    default:
        cout << "Invalid operation!" << endl;
}
```

### 5.4 Fall-through Behavior
Using intentional fall-through:

```cpp
int month = 4;
int days;

switch(month) {
    case 1: case 3: case 5: case 7: 
    case 8: case 10: case 12:
        days = 31;
        break;
        
    case 4: case 6: case 9: case 11:
        days = 30;
        break;
        
    case 2:
        days = 28;  // Ignoring leap years for simplicity
        break;
        
    default:
        days = 0;
        cout << "Invalid month!" << endl;
}
```

---

## Section 6: Control Structures - Iteration (10 minutes)

### 6.1 While Loops
Execute while condition is true:

```cpp
int countdown = 10;
while (countdown > 0) {
    cout << countdown << "..." << endl;
    countdown--;
}
cout << "Liftoff!" << endl;

// Input validation example
int userInput;
cout << "Enter a positive number: ";
cin >> userInput;

while (userInput <= 0) {
    cout << "Invalid! Please enter a positive number: ";
    cin >> userInput;
}
```

### 6.2 Do-While Loops
Execute at least once, then check condition:

```cpp
char choice;
do {
    cout << "\n=== MENU ===" << endl;
    cout << "1. New Game" << endl;
    cout << "2. Load Game" << endl;
    cout << "3. Settings" << endl;
    cout << "Q. Quit" << endl;
    cout << "Enter choice: ";
    cin >> choice;
    
    // Process choice here...
    
} while (choice != 'Q' && choice != 'q');
```

### 6.3 For Loops
Compact syntax for counter-based iteration:

```cpp
// Standard counting loop
for (int i = 0; i < 10; i++) {
    cout << i << " ";
}
cout << endl;

// Counting backwards
for (int i = 10; i > 0; i--) {
    cout << i << " ";
}
cout << endl;

// Stepping by different increments
for (int i = 0; i <= 100; i += 10) {
    cout << i << "% ";
}
cout << endl;
```

### 6.4 Nested Loops
Loops within loops for multi-dimensional processing:

```cpp
// Multiplication table
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        cout << i * j << "\t";
    }
    cout << endl;
}

// Pattern printing
int rows = 5;
for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= i; j++) {
        cout << "* ";
    }
    cout << endl;
}
```

---

## Section 7: Arrays (15 minutes)

### 7.1 Array Declaration and Initialization
Arrays store multiple values of the same type:

```cpp
// Declaration
int scores[5];                    // Uninitialized array of 5 integers

// Declaration with initialization
int temperatures[7] = {68, 72, 75, 71, 69, 73, 74};

// Partial initialization (rest filled with 0)
int values[10] = {1, 2, 3};      // Rest are 0

// Size deduction
int primes[] = {2, 3, 5, 7, 11}; // Size = 5
```

### 7.2 Array Indexing
Arrays use zero-based indexing:

```cpp
int numbers[5] = {10, 20, 30, 40, 50};

// Accessing elements
cout << "First element: " << numbers[0] << endl;   // 10
cout << "Last element: " << numbers[4] << endl;    // 50

// Modifying elements
numbers[2] = 35;  // Change third element
numbers[0] = numbers[1] + numbers[2];  // 20 + 35 = 55
```

### 7.3 Processing Arrays with Loops
Systematic array manipulation:

```cpp
const int SIZE = 10;
double readings[SIZE];

// Initialize array with calculated values
for (int i = 0; i < SIZE; i++) {
    readings[i] = i * 1.5;
}

// Calculate sum and average
double sum = 0;
for (int i = 0; i < SIZE; i++) {
    sum += readings[i];
}
double average = sum / SIZE;

// Find maximum value
double maxValue = readings[0];
for (int i = 1; i < SIZE; i++) {
    if (readings[i] > maxValue) {
        maxValue = readings[i];
    }
}
```

### 7.4 Common Array Patterns

#### Linear Search:
```cpp
int target = 25;
int data[8] = {10, 15, 20, 25, 30, 35, 40, 45};
bool found = false;
int position = -1;

for (int i = 0; i < 8; i++) {
    if (data[i] == target) {
        found = true;
        position = i;
        break;
    }
}

if (found) {
    cout << "Found at index: " << position << endl;
} else {
    cout << "Not found" << endl;
}
```

#### Array Reversal:
```cpp
int arr[6] = {1, 2, 3, 4, 5, 6};
int temp;

for (int i = 0; i < 3; i++) {
    temp = arr[i];
    arr[i] = arr[5 - i];
    arr[5 - i] = temp;
}
```

### 7.5 Array Bounds and Safety
Understanding array limitations:

```cpp
int data[5];

// DANGEROUS - Buffer overflow
// data[10] = 100;  // Undefined behavior!

// Safe practice: Use constants for size
const int ARRAY_SIZE = 5;
int safeArray[ARRAY_SIZE];

for (int i = 0; i < ARRAY_SIZE; i++) {
    safeArray[i] = i * 2;
}
```

### 7.6 Multidimensional Arrays (Brief Introduction)
Arrays can have multiple dimensions:

```cpp
// 2D array (3 rows, 4 columns)
int matrix[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// Accessing elements
cout << matrix[1][2];  // Outputs: 7

// Processing with nested loops
for (int row = 0; row < 3; row++) {
    for (int col = 0; col < 4; col++) {
        cout << matrix[row][col] << " ";
    }
    cout << endl;
}
```

---

## Time Allocation and Learning Outcomes Summary

| Section | Topic | Duration | Learning Outcomes Addressed |
|---------|-------|----------|---------------------------|
| 1 | Program Structure | 10 min | LO1: Construct basic C++ programs |
| 2 | Variables & Data Types | 12 min | LO2: Declare and manipulate variables |
| 3 | Type Casting | 8 min | LO3: Apply type casting techniques |
| 4 | Scope | 10 min | LO4: Implement scope rules |
| 5 | Selection Structures | 10 min | LO5: Design control structures |
| 6 | Iteration Structures | 10 min | LO5: Design control structures |
| 7 | Arrays | 15 min | LO6: Create and manipulate arrays |
| **Total Lecture Time** | | **75 min** | |
| **Worksheet Activities** | | **20 min** | |
| **Break** | | **5 min** | |
| **Total Session** | | **90 min** | |

---

## Resources for Further Study

### Essential References
1. **Stroustrup, B. (2013).** *The C++ Programming Language* (4th Edition). Addison-Wesley.
2. **Gregoire, M. (2021).** *Professional C++* (5th Edition). Wrox.

### Online Documentation
- [cppreference.com](https://en.cppreference.com/) - Comprehensive C++ reference
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/) - Best practices
- [learncpp.com](https://www.learncpp.com/) - Structured tutorials

### Practice Platforms
- [HackerRank C++ Domain](https://www.hackerrank.com/domains/cpp) - Progressive challenges
- [Codewars](https://www.codewars.com/) - Kata-based learning
- [LeetCode](https://leetcode.com/) - Algorithm practice

### Video Resources
- [The Cherno C++ Series](https://www.youtube.com/playlist?list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb) - Comprehensive video tutorials
- [CppCon YouTube Channel](https://www.youtube.com/user/CppCon) - Conference talks

### Recommended Practice Topics
1. Write programs using different data types and observe overflow behavior
2. Practice with nested control structures to solve complex logic problems
3. Implement common array algorithms (sorting, searching, filtering)
4. Experiment with scope by creating functions and observing variable accessibility
5. Convert between data types and understand precision loss scenarios