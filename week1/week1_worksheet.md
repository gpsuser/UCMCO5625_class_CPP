# Lecture 1: C++ Worksheet

## Quiz Section

### Question 1: Fill in the Blanks

Complete the following statement about C++ program structure:

"Every C++ program must have a _______ function that serves as the program's entry point. This function typically returns an _______ value to indicate successful completion. To use input/output operations, we include the _______ header file and can use the _______ namespace to simplify our code."

**Word Bank (scrambled):** `std`, `integer`, `iostream`, `main`

---

### Question 2: Fill in the Blanks

Complete the following statement about type casting in C++:

"The modern C++ approach to type conversion uses _______ which provides compile-time type checking. When dividing two integers, the result is truncated unless we explicitly cast at least one operand to a _______ type. The traditional C-style cast uses _______ around the type name, but the C++ style is _______ for better type safety."

**Word Bank (scrambled):** `preferred`, `parentheses`, `static_cast`, `floating-point`

---

### Question 3: Multiple Choice

What is the output of the following code?

```cpp
int x = 7;
int y = 3;
double result = x / y;
cout << result << endl;
```

A) 2.33333  
B) 2.0  
C) 2  
D) The code will not compile

---

### Question 4: Multiple Choice

Which of the following statements about variable scope in C++ is **FALSE**?

A) Variables declared inside a loop are only accessible within that loop  
B) Global variables should be avoided because they create hidden dependencies  
C) Variables declared in a function can be accessed from any other function  
D) Block scope means variables exist only within their enclosing braces

---

## Task Section

### Task 1: Temperature Converter with Validation

Write a program that converts temperatures between Celsius and Fahrenheit. The program should:

1. Display a menu asking the user to choose:
   - 1 for Celsius to Fahrenheit
   - 2 for Fahrenheit to Celsius
   - 0 to exit

2. Get the user's choice and validate that it's 0, 1, or 2

3. If the choice is 1 or 2, ask for the temperature value

4. Perform the conversion:
   - Celsius to Fahrenheit: F = (C × 9/5) + 32
   - Fahrenheit to Celsius: C = (F - 32) × 5/9

5. Display the result with 2 decimal places

6. The program should continue running until the user enters 0

**Hint:** Use `static_cast` for proper floating-point division and remember to use a loop to keep the program running.

---

### Task 2: Array Statistics Calculator

Write a program that:

1. Declares an array of 8 integers

2. Asks the user to enter 8 numbers to fill the array

3. Calculates and displays:
   - The sum of all numbers
   - The average (as a decimal number)
   - The maximum value
   - The minimum value
   - How many numbers are above the average

4. Uses appropriate loops to process the array

5. Uses `static_cast` when calculating the average to ensure decimal precision

**Example output:**
```
Enter 8 numbers:
10 25 30 15 40 35 20 45

Sum: 220
Average: 27.50
Maximum: 45
Minimum: 10
Numbers above average: 4
```

---

## Summary Table: Lecture Topics to Tutorial Tasks

| Lecture Topic | Tutorial Section | Task Reference |
|--------------|------------------|----------------|
| **Program Structure** | Section 1: Integrated Development Environments | Task 1.1: Create first program with name, course, year |
| **Variables and Data Types** | Section 2: String Variables | Task 2.1: Create and print name, language variables |
| | Section 6: Data Types | Task 6.1: Calculate age from birth year |
| **Type Casting** | Section 31: Type Conversion (Casting) | Task 31.1: Convert string to decimal, display as integer and rounded |
| **Scope** | Section 18: Functions (Methods) | Task 18.1: Create isEven() function |
| **Selection Structures (If/Else)** | Section 9: If Statements | Task 9.1: Check if number is positive or large |
| | Section 10: If-Else Statements | Task 10.1: Temperature classification program |
| **Selection Structures (Switch)** | Section 11: Switch Statements | Task 11.1: Simple calculator with switch |
| **Iteration (While Loops)** | Section 12: While Loops | Task 12.1: Number guessing game |
| | Section 13: Do-While Loops | Task 13.1: Menu-driven program |
| **Iteration (For Loops)** | Section 16: For Loops | Task 16.1: Print multiplication table |
| | Section 17: Further For Loops | Task 17.1: Print multiplication grid 1-10 |
| **Arrays** | Section 21: Arrays | Task 21.1: Create array of 5 student names |
| | Section 22: Array Iteration | Task 22.1: Process array of 10 numbers (evens, sum, minimum) |