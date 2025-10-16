# C++ Programming Tutorial

## Introduction

Welcome to this comprehensive C++ programming tutorial! This course covers fundamental programming concepts using C++, mirroring topics typically taught in introductory Java courses but adapted for C++'s syntax and conventions.

Each section includes:
- Clear explanations of concepts
- Worked examples with code
- A practical task for you to complete

---

## 1. Integrated Development Environments (IDEs)

### What is an IDE?

An Integrated Development Environment (IDE) is software that provides comprehensive facilities for software development. Popular C++ IDEs include:
- **Visual Studio** (Windows - Professional and Community editions)
- **Code::Blocks** (Cross-platform, free)
- **CLion** (Cross-platform, JetBrains)
- **Visual Studio Code** with C++ extensions
- **Dev-C++** (Windows, lightweight)

### Creating a New C++ Project

**In Visual Studio:**
1. Open Visual Studio
2. Click "Create a new project"
3. Select "Console App" (C++)
4. Choose a location and name
5. Click "Create"

**In Code::Blocks:**
1. Open Code::Blocks
2. File > New > Project
3. Select "Console Application"
4. Choose C++ and follow the wizard

### Your First Program

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, World!" << endl;
    return 0;
}
```

**Explanation:**
- `#include <iostream>`: Includes input/output library
- `using namespace std;`: Allows us to use `cout` without `std::`
- `int main()`: Entry point of the program
- `cout`: Output stream for printing
- `endl`: End line (newline character)
- `return 0;`: Indicates successful program completion

### Task 1.1
Create a new C++ file called `first_program.cpp` and write a program that prints your name, your course, and the current year on three separate lines.

---

## 2. String Variables

### What are Variables?

Variables are containers that store data values. In C++, you must declare the type of each variable.

### Working with Strings

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Creating string variables
    string name = "Alice";
    string greeting = "Hello";
    
    // Printing variables
    cout << name << endl;
    cout << greeting << endl;
    
    // Strings must use double quotes
    string message = "Welcome to C++";
    
    return 0;
}
```

**Important:** In C++, you must include `<string>` to use the string type.

### Task 2.1
Create variables to store your first name, last name, and favorite programming language. Print each variable on a separate line.

---

## 3. String Concatenation

### Combining Strings

In C++, you can combine strings using the `+` operator or other methods.

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string firstName = "John";
    string lastName = "Smith";
    
    // Using + operator
    string fullName = firstName + " " + lastName;
    cout << fullName << endl;  // Output: John Smith
    
    // Combining strings and text
    string greeting = "Hello, " + firstName + " " + lastName + "!";
    cout << greeting << endl;
    
    // Using append() method
    string message = "My name is ";
    message.append(firstName);
    message.append(" ");
    message.append(lastName);
    cout << message << endl;
    
    return 0;
}
```

### Task 3.1
Create variables for a street name, city, and postal code. Concatenate them to form a complete address and print it in a nicely formatted way.

---

## 4. Appending Strings

### Building Strings Incrementally

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Using += to append to strings
    string sentence = "C++";
    sentence += " is";
    sentence += " awesome";
    cout << sentence << endl;  // Output: C++ is awesome
    
    // Building a longer message
    string story = "Once upon a time";
    story += ", there was a programmer";
    story += " who loved C++.";
    cout << story << endl;
    
    return 0;
}
```

### Task 4.1
Start with an empty string and use the `+=` operator to build a sentence word by word. Your sentence should be at least 8 words long and describe what you're learning.

---

## 5. Getting User Input

### The cin Object

The `cin` object allows you to get data from the user.

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Getting user input
    string name;
    cout << "What is your name? ";
    cin >> name;
    cout << "Hello, " << name << "!" << endl;
    
    // Getting multiple inputs
    int age;
    cout << "How old are you? ";
    cin >> age;
    cout << "You are " << age << " years old." << endl;
    
    return 0;
}
```

**Note:** `cin >>` stops at whitespace. For strings with spaces, use `getline()`:

```cpp
string fullName;
cout << "Enter your full name: ";
cin.ignore(); // Clear the input buffer
getline(cin, fullName);
cout << "Hello, " << fullName << "!" << endl;
```

### Task 5.1
Write a program that asks the user for their favorite food and favorite color, then prints a message using both pieces of information.

---

## 6. Data Types

### Understanding C++ Data Types

C++ has several built-in data types:

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Integer (whole numbers)
    int age = 25;
    int count = -10;
    
    // Float (decimal numbers - single precision)
    float price = 19.99f;
    
    // Double (decimal numbers - double precision)
    double temperature = -5.5;
    
    // Character (single character)
    char grade = 'A';
    char initial = 'J';
    
    // String (text)
    string name = "C++";
    
    // Boolean (true or false)
    bool isStudent = true;
    bool hasLicense = false;
    
    // Displaying values
    cout << "Age: " << age << endl;
    cout << "Price: " << price << endl;
    cout << "Temperature: " << temperature << endl;
    cout << "Grade: " << grade << endl;
    cout << "Name: " << name << endl;
    cout << "Is student: " << isStudent << endl;
    
    return 0;
}
```

### Type Sizes

```cpp
cout << "Size of int: " << sizeof(int) << " bytes" << endl;
cout << "Size of double: " << sizeof(double) << " bytes" << endl;
cout << "Size of char: " << sizeof(char) << " bytes" << endl;
```

### Task 6.1
Write a program that asks the user for their birth year (as an integer) and calculates their age in 2025. Print the result with an appropriate message.

---

## 7. Basic Calculations

### Arithmetic Operations

```cpp
#include <iostream>
using namespace std;

int main() {
    // Basic operators
    int a = 10;
    int b = 3;
    
    cout << "Addition: " << (a + b) << endl;           // 13
    cout << "Subtraction: " << (a - b) << endl;        // 7
    cout << "Multiplication: " << (a * b) << endl;     // 30
    cout << "Division: " << (a / b) << endl;           // 3 (integer division)
    cout << "Division (float): " << (a / 3.0) << endl; // 3.33333
    cout << "Modulus (remainder): " << (a % b) << endl;// 1
    
    // Order of operations (PEMDAS)
    int result = 10 + 5 * 2;
    cout << "Result: " << result << endl;  // 20 (not 30)
    
    result = (10 + 5) * 2;
    cout << "Result: " << result << endl;  // 30
    
    return 0;
}
```

**Important:** Integer division truncates the decimal part. For decimal results, use `double` or `float`.

### Task 7.1
Write a program that asks the user for the length and width of a rectangle, then calculates and prints both the perimeter and area.

---

## 8. Assignment Operators & Operator Precedence

### Compound Assignment Operators

```cpp
#include <iostream>
using namespace std;

int main() {
    int x = 10;
    
    // These are shortcuts
    x += 5;   // Same as: x = x + 5
    cout << x << endl;  // 15
    
    x -= 3;   // Same as: x = x - 3
    cout << x << endl;  // 12
    
    x *= 2;   // Same as: x = x * 2
    cout << x << endl;  // 24
    
    x /= 4;   // Same as: x = x / 4
    cout << x << endl;  // 6
    
    x %= 4;   // Same as: x = x % 4
    cout << x << endl;  // 2
    
    // Increment and decrement
    x++;      // Same as: x = x + 1
    cout << x << endl;  // 3
    
    x--;      // Same as: x = x - 1
    cout << x << endl;  // 2
    
    return 0;
}
```

### Operator Precedence

C++ follows this order (highest to lowest):
1. Parentheses `()`
2. Increment/Decrement `++`, `--`
3. Multiplication, Division, Modulus `*`, `/`, `%`
4. Addition, Subtraction `+`, `-`

```cpp
int result = 2 + 3 * 4;
cout << result << endl;  // 14 (not 20)
```

### Task 8.1
Write a program that asks the user for a number, then performs the following operations in order: add 10, multiply by 2, subtract 5, and divide by 3. Print the result at each step.

---

## 9. If Statements

### Making Decisions in Code

```cpp
#include <iostream>
using namespace std;

int main() {
    // Basic if statement
    int age = 18;
    
    if (age >= 18) {
        cout << "You are an adult" << endl;
    }
    
    // If with comparison operators
    int temperature = 25;
    
    if (temperature > 30) {
        cout << "It's hot!" << endl;
    }
    
    // Multiple independent conditions
    int score = 85;
    
    if (score >= 90) {
        cout << "Grade: A" << endl;
    }
    
    if (score >= 80) {
        cout << "Grade: B or higher" << endl;
    }
    
    if (score >= 70) {
        cout << "Grade: C or higher" << endl;
    }
    
    return 0;
}
```

**Comparison Operators:**
- `==` Equal to
- `!=` Not equal to
- `>` Greater than
- `<` Less than
- `>=` Greater than or equal to
- `<=` Less than or equal to

### Task 9.1
Write a program that asks the user for a number. If the number is positive, print "Positive". If the number is greater than 100, print "Large number".

---

## 10. If-Else Statements

### Two-Way Decisions

```cpp
#include <iostream>
using namespace std;

int main() {
    int age;
    cout << "Enter your age: ";
    cin >> age;
    
    if (age >= 18) {
        cout << "You are an adult" << endl;
    } else {
        cout << "You are a minor" << endl;
    }
    
    // Else if for multiple conditions
    int grade;
    cout << "Enter your grade (0-100): ";
    cin >> grade;
    
    if (grade >= 90) {
        cout << "Excellent! Grade: A" << endl;
    } else if (grade >= 80) {
        cout << "Great! Grade: B" << endl;
    } else if (grade >= 70) {
        cout << "Good! Grade: C" << endl;
    } else if (grade >= 60) {
        cout << "Pass. Grade: D" << endl;
    } else {
        cout << "Fail. Grade: F" << endl;
    }
    
    return 0;
}
```

### Task 10.1
Write a program that asks the user for a temperature in Celsius. If it's below 0, print "Freezing". If it's 0-15, print "Cold". If it's 16-25, print "Mild". If it's above 25, print "Warm".

---

## 11. Switch Statements

### Multi-Way Decisions

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    int day;
    cout << "Enter a day number (1-7): ";
    cin >> day;
    
    switch (day) {
        case 1:
            cout << "Monday" << endl;
            break;
        case 2:
            cout << "Tuesday" << endl;
            break;
        case 3:
            cout << "Wednesday" << endl;
            break;
        case 4:
            cout << "Thursday" << endl;
            break;
        case 5:
            cout << "Friday" << endl;
            break;
        case 6:
            cout << "Saturday" << endl;
            break;
        case 7:
            cout << "Sunday" << endl;
            break;
        default:
            cout << "Invalid day" << endl;
    }
    
    // Multiple cases with same result
    switch (day) {
        case 1:
        case 2:
        case 3:
        case 4:
        case 5:
            cout << "Weekday" << endl;
            break;
        case 6:
        case 7:
            cout << "Weekend" << endl;
            break;
        default:
            cout << "Invalid" << endl;
    }
    
    return 0;
}
```

**Important:** Don't forget the `break` statement, or execution will "fall through" to the next case!

### Task 11.1
Create a simple calculator using switch statements. Ask the user for two numbers and an operation (+, -, *, /). Perform the operation and print the result.

---

## 12. While Loops

### Repeating Code with Conditions

```cpp
#include <iostream>
using namespace std;

int main() {
    // Basic while loop
    int count = 1;
    while (count <= 5) {
        cout << "Count: " << count << endl;
        count++;
    }
    
    // User input validation
    string password;
    while (password != "cpp123") {
        cout << "Enter password: ";
        cin >> password;
        if (password != "cpp123") {
            cout << "Incorrect! Try again." << endl;
        }
    }
    cout << "Access granted!" << endl;
    
    // Infinite loop with break
    while (true) {
        string userInput;
        cout << "Type 'quit' to exit: ";
        cin >> userInput;
        if (userInput == "quit") {
            break;
        }
        cout << "You typed: " << userInput << endl;
    }
    
    return 0;
}
```

### Task 12.1
Write a program that asks the user to guess a secret number (you choose the number). Keep asking until they guess correctly. After each wrong guess, tell them if their guess was too high or too low.

---

## 13. Do-While Loops

### Loops That Run At Least Once

```cpp
#include <iostream>
using namespace std;

int main() {
    // Do-while always runs at least once
    int number;
    do {
        cout << "Enter a positive number: ";
        cin >> number;
        if (number <= 0) {
            cout << "That's not positive! Try again." << endl;
        }
    } while (number <= 0);
    
    cout << "You entered: " << number << endl;
    
    // Menu example
    int choice;
    do {
        cout << "\n=== Menu ===" << endl;
        cout << "1. Option A" << endl;
        cout << "2. Option B" << endl;
        cout << "3. Exit" << endl;
        cout << "Your choice: ";
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "You selected Option A" << endl;
                break;
            case 2:
                cout << "You selected Option B" << endl;
                break;
            case 3:
                cout << "Goodbye!" << endl;
                break;
            default:
                cout << "Invalid choice" << endl;
        }
    } while (choice != 3);
    
    return 0;
}
```

### Task 13.1
Create a menu-driven program that displays a menu of 3 options, gets the user's choice, and performs an action. The program should keep running until the user chooses to exit.

---

## 14. Logical Operators

### Combining Conditions

```cpp
#include <iostream>
using namespace std;

int main() {
    // AND operator (&&) - both must be true
    int age = 25;
    bool hasLicense = true;
    
    if (age >= 18 && hasLicense) {
        cout << "You can drive" << endl;
    }
    
    // OR operator (||) - at least one must be true
    bool isWeekend = true;
    bool isHoliday = false;
    
    if (isWeekend || isHoliday) {
        cout << "No work today!" << endl;
    }
    
    // NOT operator (!) - inverts the boolean
    bool isRaining = false;
    
    if (!isRaining) {
        cout << "It's a nice day" << endl;
    }
    
    // Complex conditions
    int temperature = 22;
    bool isSunny = true;
    
    if (temperature > 20 && isSunny && !isRaining) {
        cout << "Perfect day for a picnic!" << endl;
    }
    
    // Short-circuit evaluation
    int x = 10;
    if (x > 0 && x < 100 && x % 2 == 0) {
        cout << "x is a positive even number less than 100" << endl;
    }
    
    return 0;
}
```

### Task 14.1
Write a program that asks for a user's age and whether they are a student. If they are under 18 OR they are a student, they get a discount. Print an appropriate message based on whether they qualify for the discount.

---

## 15. The String Class

### String Methods and Operations

```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int main() {
    string text = "Hello, C++ Programming!";
    
    // Length
    cout << "Length: " << text.length() << endl;
    cout << "Length: " << text.size() << endl;  // Same as length()
    
    // Accessing characters
    cout << "First char: " << text[0] << endl;
    cout << "Last char: " << text[text.length() - 1] << endl;
    
    // Substring
    string sub = text.substr(7, 3);  // Start at index 7, length 3
    cout << "Substring: " << sub << endl;  // "C++"
    
    // Finding
    size_t pos = text.find("C++");
    if (pos != string::npos) {
        cout << "Found 'C++' at position: " << pos << endl;
    }
    
    // Replacing
    string text2 = text;
    text2.replace(7, 3, "Java");
    cout << text2 << endl;
    
    // Case conversion (requires algorithm)
    string lower = text;
    transform(lower.begin(), lower.end(), lower.begin(), ::tolower);
    cout << "Lowercase: " << lower << endl;
    
    string upper = text;
    transform(upper.begin(), upper.end(), upper.begin(), ::toupper);
    cout << "Uppercase: " << upper << endl;
    
    // Comparing strings
    string str1 = "apple";
    string str2 = "banana";
    if (str1 < str2) {
        cout << str1 << " comes before " << str2 << endl;
    }
    
    // Check if empty
    string empty = "";
    if (empty.empty()) {
        cout << "String is empty" << endl;
    }
    
    return 0;
}
```

### Task 15.1
Write a program that asks the user for a sentence. Count and print: the number of characters, whether the sentence contains the word "cpp" (case-insensitive), and the position of the first space character.

---

## 16. For Loops

### Iterating a Specific Number of Times

```cpp
#include <iostream>
using namespace std;

int main() {
    // Basic for loop
    for (int i = 0; i < 5; i++) {
        cout << "Iteration " << i << endl;
    }
    // Output: Iteration 0, 1, 2, 3, 4
    
    // Starting from 1
    for (int i = 1; i <= 5; i++) {
        cout << i << endl;
    }
    // Output: 1, 2, 3, 4, 5
    
    // Stepping by 2
    for (int i = 0; i < 10; i += 2) {
        cout << i << " ";
    }
    cout << endl;
    // Output: 0 2 4 6 8
    
    // Counting backwards
    for (int i = 10; i > 0; i--) {
        cout << i << " ";
    }
    cout << endl;
    // Output: 10 9 8 7 6 5 4 3 2 1
    
    // Iterating over a string
    string name = "C++";
    for (int i = 0; i < name.length(); i++) {
        cout << name[i] << endl;
    }
    
    return 0;
}
```

### Task 16.1
Write a program that prints the multiplication table for a number entered by the user (from 1 to 12).

---

## 17. Further For Loops

### Nested Loops and Advanced Patterns

```cpp
#include <iostream>
using namespace std;

int main() {
    // Nested loops
    for (int i = 1; i <= 3; i++) {
        for (int j = 1; j <= 3; j++) {
            cout << "i=" << i << ", j=" << j << endl;
        }
    }
    
    // Creating patterns
    for (int i = 1; i <= 5; i++) {
        for (int j = 0; j < i; j++) {
            cout << "*";
        }
        cout << endl;
    }
    // Output:
    // *
    // **
    // ***
    // ****
    // *****
    
    // Breaking out of loops
    for (int i = 0; i < 10; i++) {
        if (i == 5) {
            break;  // Exit loop when i equals 5
        }
        cout << i << " ";
    }
    cout << endl;
    
    // Continue statement
    for (int i = 0; i < 10; i++) {
        if (i % 2 == 0) {
            continue;  // Skip even numbers
        }
        cout << i << " ";
    }
    cout << endl;  // Output: 1 3 5 7 9
    
    return 0;
}
```

### Task 17.1
Write a program that prints a multiplication grid from 1 to 10. Each row should show one times table.

---

## 18. Functions (Methods)

### Creating Reusable Code

```cpp
#include <iostream>
using namespace std;

// Simple function
void greet() {
    cout << "Hello!" << endl;
}

// Function with return value
int addNumbers() {
    return 5 + 3;
}

// More complex function
double calculateCircleArea() {
    double radius = 5.0;
    double pi = 3.14159;
    double area = pi * radius * radius;
    return area;
}

int main() {
    greet();  // Call the function
    
    int result = addNumbers();
    cout << "Result: " << result << endl;  // 8
    
    double area = calculateCircleArea();
    cout << "Circle area: " << area << endl;
    
    return 0;
}
```

**Important:** In C++, functions must be declared before they are used, or you must use function prototypes:

```cpp
#include <iostream>
using namespace std;

// Function prototype (declaration)
void greet();
int addNumbers();

int main() {
    greet();
    cout << addNumbers() << endl;
    return 0;
}

// Function definitions
void greet() {
    cout << "Hello!" << endl;
}

int addNumbers() {
    return 5 + 3;
}
```

### Task 18.1
Write a function called `isEven()` that returns `true` if a hardcoded number is even, and `false` otherwise. Test your function by calling it from main and printing the result.

---

## 19. Functions with Parameters

### Passing Data to Functions

```cpp
#include <iostream>
#include <string>
using namespace std;

// Function with one parameter
void greet(string name) {
    cout << "Hello, " << name << "!" << endl;
}

// Function with multiple parameters
int add(int a, int b) {
    return a + b;
}

// Function with default parameters
int power(int base, int exponent = 2) {
    int result = 1;
    for (int i = 0; i < exponent; i++) {
        result *= base;
    }
    return result;
}

// More complex example
char calculateGrade(int score, int bonus = 0) {
    int total = score + bonus;
    if (total >= 90) return 'A';
    else if (total >= 80) return 'B';
    else if (total >= 70) return 'C';
    else if (total >= 60) return 'D';
    else return 'F';
}

int main() {
    greet("Alice");
    greet("Bob");
    
    cout << add(5, 3) << endl;  // 8
    
    cout << power(5) << endl;      // 25 (uses default exponent=2)
    cout << power(5, 3) << endl;   // 125 (overrides default)
    
    cout << calculateGrade(85) << endl;      // B
    cout << calculateGrade(85, 10) << endl;  // A
    
    return 0;
}
```

### Pass by Value vs Pass by Reference

```cpp
// Pass by value (copy)
void modifyValue(int x) {
    x = 100;  // Only modifies the copy
}

// Pass by reference (original)
void modifyReference(int& x) {
    x = 100;  // Modifies the original
}

int main() {
    int num = 50;
    modifyValue(num);
    cout << num << endl;  // 50 (unchanged)
    
    modifyReference(num);
    cout << num << endl;  // 100 (changed)
    
    return 0;
}
```

### Task 19.1
Write a function called `calculateRectangleArea(length, width)` that returns the area of a rectangle. Write another function `calculateRectanglePerimeter(length, width)` that returns the perimeter. Test both functions with different values.

---

## 20. Testing Functions

### Using Assertions

```cpp
#include <iostream>
#include <cassert>
using namespace std;

int add(int a, int b) {
    return a + b;
}

int main() {
    // Simple assertion testing
    assert(add(2, 3) == 5);
    assert(add(-1, 1) == 0);
    assert(add(0, 0) == 0);
    cout << "All add() tests passed!" << endl;
    
    // Testing with output
    int result = add(10, 20);
    if (result == 30) {
        cout << "Test passed: 10 + 20 = 30" << endl;
    } else {
        cout << "Test failed!" << endl;
    }
    
    return 0;
}
```

**Note:** Assertions will cause the program to terminate if they fail. They are useful during development but should be used carefully in production code.

### Task 20.1
Write a function `isPalindrome(word)` that returns `true` if a word is a palindrome (reads the same forwards and backwards) and `false` otherwise. Write at least 3 assertions to test your function.

---

## 21. Arrays

### Working with Fixed-Size Collections

```cpp
#include <iostream>
using namespace std;

int main() {
    // Declaring and initializing arrays
    int numbers[5] = {10, 20, 30, 40, 50};
    
    // Accessing elements
    cout << "First element: " << numbers[0] << endl;  // 10
    cout << "Last element: " << numbers[4] << endl;   // 50
    
    // Modifying elements
    numbers[1] = 25;
    cout << "Modified second element: " << numbers[1] << endl;  // 25
    
    // Array size
    int size = sizeof(numbers) / sizeof(numbers[0]);
    cout << "Array size: " << size << endl;  // 5
    
    // String array
    string fruits[3] = {"apple", "banana", "cherry"};
    cout << "First fruit: " << fruits[0] << endl;
    
    // Declaring without initialization
    int scores[10];
    for (int i = 0; i < 10; i++) {
        scores[i] = i * 10;
    }
    
    // Character array (C-style string)
    char name[20] = "C++ Programming";
    cout << name << endl;
    
    return 0;
}
```

**Important:** C++ arrays have fixed size and don't have built-in bounds checking. Accessing outside the array bounds causes undefined behavior!

### Task 21.1
Create an array of 5 student names. Write code to: print the first and last student, modify the second student's name, and print all students using a loop.

---

## 22. Array Iteration

### Looping Through Arrays

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Basic iteration
    string fruits[3] = {"apple", "banana", "cherry"};
    int size = 3;
    
    for (int i = 0; i < size; i++) {
        cout << fruits[i] << endl;
    }
    
    // Iterating and modifying
    int numbers[5] = {1, 2, 3, 4, 5};
    for (int i = 0; i < 5; i++) {
        numbers[i] = numbers[i] * 2;
    }
    
    // Print modified array
    for (int i = 0; i < 5; i++) {
        cout << numbers[i] << " ";
    }
    cout << endl;  // Output: 2 4 6 8 10
    
    // Finding maximum
    int values[10] = {34, 12, 78, 45, 23, 89, 56, 67, 90, 11};
    int max = values[0];
    for (int i = 1; i < 10; i++) {
        if (values[i] > max) {
            max = values[i];
        }
    }
    cout << "Maximum value: " << max << endl;
    
    // Calculating sum
    int sum = 0;
    for (int i = 0; i < 10; i++) {
        sum += values[i];
    }
    cout << "Sum: " << sum << endl;
    cout << "Average: " << (sum / 10.0) << endl;
    
    return 0;
}
```

### Task 22.1
Create an array of 10 numbers. Write code to: print all even numbers, calculate and print the sum of all numbers, and find and print the smallest number.

---

## 23. Range-Based For Loops

### Modern C++ Iteration (C++11 and later)

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Range-based for loop (like for-each)
    int numbers[] = {1, 2, 3, 4, 5};
    
    for (int num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // With strings
    string names[] = {"Alice", "Bob", "Charlie"};
    for (string name : names) {
        cout << "Hello, " << name << "!" << endl;
    }
    
    // Using auto keyword
    int values[] = {10, 20, 30, 40, 50};
    for (auto value : values) {
        cout << value << " ";
    }
    cout << endl;
    
    // Modifying with reference
    int scores[] = {85, 90, 78, 92, 88};
    for (int& score : scores) {
        score += 5;  // Add 5 bonus points
    }
    
    // Print modified scores
    for (int score : scores) {
        cout << score << " ";
    }
    cout << endl;
    
    return 0;
}
```

**Note:** Range-based for loops are read-only by default. Use `int&` to modify elements.

### Task 23.1
Create two arrays: one with 5 product names and one with 5 prices. Use traditional for loops with an index to print each product and its price in a formatted way.

---

## 24. Two-Dimensional Arrays

### Arrays of Arrays

```cpp
#include <iostream>
using namespace std;

int main() {
    // Creating a 2D array (matrix)
    int grid[3][3] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };
    
    // Accessing elements
    cout << "Element at [0][0]: " << grid[0][0] << endl;  // 1
    cout << "Element at [1][2]: " << grid[1][2] << endl;  // 6
    cout << "Element at [2][1]: " << grid[2][1] << endl;  // 8
    
    // Modifying elements
    grid[0][0] = 10;
    cout << "Modified [0][0]: " << grid[0][0] << endl;  // 10
    
    // Iterating through 2D array
    cout << "Grid:" << endl;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << grid[i][j] << " ";
        }
        cout << endl;
    }
    
    // Initializing with user input
    int matrix[2][3];
    cout << "Enter 6 numbers:" << endl;
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            cout << "Element [" << i << "][" << j << "]: ";
            cin >> matrix[i][j];
        }
    }
    
    // Display the matrix
    cout << "Your matrix:" << endl;
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

### Task 24.1
Create a 3x3 tic-tac-toe board (represented as a 2D char array with 'X', 'O', or ' '). Write code to print the board in a nice format, then place an "X" at position [1][1] and print the board again.

---

## 25. Vectors (Dynamic Arrays)

### C++'s Alternative to ArrayList

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main() {
    // Creating vectors
    vector<int> numbers;  // Empty vector
    vector<string> names = {"Alice", "Bob", "Charlie"};
    
    // Adding elements
    numbers.push_back(10);
    numbers.push_back(20);
    numbers.push_back(30);
    
    // Accessing elements
    cout << "First element: " << numbers[0] << endl;
    cout << "Using at(): " << numbers.at(1) << endl;
    
    // Size
    cout << "Size: " << numbers.size() << endl;
    
    // Inserting at position
    numbers.insert(numbers.begin() + 1, 15);  // Insert 15 at index 1
    
    // Removing elements
    numbers.pop_back();  // Remove last element
    numbers.erase(numbers.begin() + 1);  // Remove element at index 1
    
    // Checking if element exists
    bool found = false;
    for (int num : numbers) {
        if (num == 20) {
            found = true;
            break;
        }
    }
    if (found) {
        cout << "20 is in the vector" << endl;
    }
    
    // Iterating
    for (int num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // Clearing
    numbers.clear();
    cout << "After clear, size: " << numbers.size() << endl;
    
    // Check if empty
    if (numbers.empty()) {
        cout << "Vector is empty" << endl;
    }
    
    return 0;
}
```

**Advantages of vectors over arrays:**
- Dynamic size
- Bounds checking with `at()`
- Many built-in methods
- Can easily grow and shrink

### Task 25.1
Create an empty vector. Write a program that repeatedly asks the user to enter a number (or 'done' to finish). Store each number in the vector, then print the vector, its size, and the average of all numbers.

---

## 26. Searching Arrays

### Finding Elements

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Linear search
int linearSearch(int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) {
            return i;  // Return index if found
        }
    }
    return -1;  // Return -1 if not found
}

// Binary search (requires sorted array)
int binarySearch(int arr[], int size, int target) {
    int left = 0;
    int right = size - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}

int main() {
    int numbers[] = {10, 25, 30, 45, 50, 75};
    int size = 6;
    
    // Linear search
    int result = linearSearch(numbers, size, 30);
    if (result != -1) {
        cout << "Found at index " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Binary search
    result = binarySearch(numbers, size, 45);
    if (result != -1) {
        cout << "Found at index " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Using STL find (with vectors)
    vector<int> vec = {10, 25, 30, 45, 50, 75};
    auto it = find(vec.begin(), vec.end(), 30);
    if (it != vec.end()) {
        cout << "Found at position: " << distance(vec.begin(), it) << endl;
    }
    
    return 0;
}
```

### Task 26.1
Create an array of 10 random numbers between 1 and 100. Write a function that searches for a target number and returns how many comparisons were needed to find it (or to determine it's not there).

---

## 27. Iterators

### Working with Iterators

```cpp
#include <iostream>
#include <vector>
#include <list>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5};
    
    // Using iterators
    vector<int>::iterator it;
    for (it = numbers.begin(); it != numbers.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // Auto keyword simplifies this
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // Modifying with iterators
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        *it = *it * 2;
    }
    
    // Print modified
    for (int num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // Reverse iteration
    for (auto it = numbers.rbegin(); it != numbers.rend(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // Lists work similarly
    list<string> names = {"Alice", "Bob", "Charlie"};
    for (auto it = names.begin(); it != names.end(); ++it) {
        cout << *it << endl;
    }
    
    return 0;
}
```

### Task 27.1
Create a vector of integers from 1 to 10. Use iterators to print all even numbers in the vector.

---

## 28. File Input

### Reading from Files

```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
    // Reading entire file
    ifstream inputFile("data.txt");
    
    if (inputFile.is_open()) {
        string line;
        while (getline(inputFile, line)) {
            cout << line << endl;
        }
        inputFile.close();
    } else {
        cout << "Unable to open file" << endl;
    }
    
    // Reading numbers
    ifstream numberFile("numbers.txt");
    if (numberFile.is_open()) {
        int num;
        int sum = 0;
        int count = 0;
        
        while (numberFile >> num) {
            sum += num;
            count++;
        }
        
        numberFile.close();
        
        if (count > 0) {
            cout << "Sum: " << sum << endl;
            cout << "Average: " << (sum / (double)count) << endl;
        }
    }
    
    // Reading into vector
    ifstream file3("data.txt");
    vector<string> lines;
    string line;
    
    while (getline(file3, line)) {
        lines.push_back(line);
    }
    file3.close();
    
    cout << "Read " << lines.size() << " lines" << endl;
    
    return 0;
}
```

### Task 28.1
Create a text file called 'scores.txt' with 5 test scores (one per line). Write a program that reads the file and calculates the average score.

---

## 29. Writing Files

### Creating and Writing to Files

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

int main() {
    // Writing to a file (overwrites existing content)
    ofstream outputFile("output.txt");
    
    if (outputFile.is_open()) {
        outputFile << "Hello, World!" << endl;
        outputFile << "This is a new line." << endl;
        outputFile.close();
        cout << "File written successfully" << endl;
    } else {
        cout << "Unable to create file" << endl;
    }
    
    // Appending to a file
    ofstream appendFile("output.txt", ios::app);
    if (appendFile.is_open()) {
        appendFile << "This line is appended." << endl;
        appendFile.close();
    }
    
    // Writing a vector to a file
    vector<string> names = {"Alice", "Bob", "Charlie"};
    ofstream nameFile("names.txt");
    
    for (const string& name : names) {
        nameFile << name << endl;
    }
    nameFile.close();
    
    // Writing formatted data
    struct Student {
        string name;
        int grade;
    };
    
    vector<Student> students = {
        {"Alice", 85},
        {"Bob", 92},
        {"Charlie", 78}
    };
    
    ofstream gradeFile("grades.txt");
    for (const Student& student : students) {
        gradeFile << student.name << ": " << student.grade << endl;
    }
    gradeFile.close();
    
    return 0;
}
```

### Task 29.1
Write a program that asks the user for 5 favorite movies and saves them to a file called 'movies.txt'. Then write another program that reads and displays all the movies from the file.

---

## 30. Debugging

### Finding and Fixing Errors

```cpp
#include <iostream>
#include <cassert>
using namespace std;

// Types of errors:

// 1. Syntax Errors - caught by compiler
// if (x == 5)  // Missing opening brace

// 2. Runtime Errors - happen during execution
// int x = 10 / 0;  // Division by zero

// 3. Logical Errors - code runs but produces wrong results
double calculateAverage(int arr[], int size) {
    if (size == 0) return 0;  // Handle empty array
    
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += arr[i];
    }
    return sum / (double)size;  // Cast to double for correct result
}

// Using cout for debugging
void debugExample(int x, int y) {
    cout << "Debug: x=" << x << ", y=" << y << endl;
    int result = x + y;
    cout << "Debug: result=" << result << endl;
}

// Using assert for validation
int divide(int a, int b) {
    assert(b != 0);  // Program will terminate if b is 0
    return a / b;
}

int main() {
    // Common debugging techniques
    
    // 1. Print statements
    int value = 42;
    cout << "Value: " << value << endl;
    
    // 2. Assertions
    assert(value > 0);
    
    // 3. Check array bounds
    int arr[5] = {1, 2, 3, 4, 5};
    int index = 2;
    if (index >= 0 && index < 5) {
        cout << arr[index] << endl;
    }
    
    // 4. Initialize variables
    int sum = 0;  // Always initialize!
    for (int i = 0; i < 5; i++) {
        sum += arr[i];
    }
    
    return 0;
}
```

### Task 30.1
The following code has bugs. Find and fix them:
```cpp
int calculateTotal(int prices[], int size) {
    int total;
    for (int i = 0; i <= size; i++)
        total += prices[i]
    return total;
}
```

---

## 31. Type Conversion (Casting)

### Converting Between Types

```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main() {
    // Implicit conversion
    int intNum = 10;
    double doubleNum = intNum;  // int to double
    cout << doubleNum << endl;  // 10.0
    
    // Explicit conversion (C-style cast)
    double pi = 3.14159;
    int intPi = (int)pi;  // Truncates to 3
    cout << intPi << endl;
    
    // C++ style cast (preferred)
    double value = 3.7;
    int truncated = static_cast<int>(value);
    cout << truncated << endl;  // 3
    
    // Integer division to double
    int a = 10;
    int b = 3;
    double result = static_cast<double>(a) / b;
    cout << result << endl;  // 3.33333
    
    // String to number
    string numStr = "123";
    int num = stoi(numStr);  // String to int
    cout << num + 1 << endl;  // 124
    
    string floatStr = "3.14";
    double dbl = stod(floatStr);  // String to double
    cout << dbl * 2 << endl;  // 6.28
    
    // Number to string
    int number = 42;
    string str = to_string(number);
    cout << "String: " << str << endl;
    
    // Using stringstream for conversion
    stringstream ss;
    ss << 123;
    string converted;
    ss >> converted;
    cout << converted << endl;
    
    // Character to number
    char digit = '5';
    int digitValue = digit - '0';  // Convert '5' to 5
    cout << digitValue << endl;
    
    return 0;
}
```

### Task 31.1
Write a program that asks the user for a decimal number as a string. Convert and display it as an integer (truncated), a rounded integer, and verify its type before and after conversion.

---

## 32. Formatting Data

### Making Output Look Nice

```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main() {
    // Setting precision for floating point
    double pi = 3.14159265359;
    
    cout << fixed << setprecision(2);
    cout << "Pi to 2 decimal places: " << pi << endl;  // 3.14
    
    cout << setprecision(4);
    cout << "Pi to 4 decimal places: " << pi << endl;  // 3.1416
    
    // Field width and alignment
    cout << setw(10) << "Left" << "|" << endl;      // Right-aligned by default
    cout << left << setw(10) << "Left" << "|" << endl;   // Left-aligned
    cout << right << setw(10) << "Right" << "|" << endl; // Right-aligned
    
    // Padding with zeros
    cout << setfill('0') << setw(5) << 42 << endl;  // 00042
    cout << setfill(' ');  // Reset to spaces
    
    // Formatting in columns
    struct Product {
        string name;
        double price;
        int quantity;
    };
    
    Product products[] = {
        {"Apple", 1.50, 10},
        {"Banana", 0.75, 25},
        {"Cherry", 3.00, 5}
    };
    
    cout << left << setw(15) << "Product" 
         << right << setw(10) << "Price" 
         << setw(12) << "Quantity" << endl;
    cout << string(37, '-') << endl;
    
    for (const Product& p : products) {
        cout << left << setw(15) << p.name 
             << right << setw(9) << fixed << setprecision(2) << p.price 
             << setw(12) << p.quantity << endl;
    }
    
    // Boolean values
    bool flag = true;
    cout << "Boolean: " << flag << endl;           // 1
    cout << "Boolean: " << boolalpha << flag << endl;  // true
    
    // Hexadecimal and octal
    int number = 255;
    cout << "Decimal: " << dec << number << endl;  // 255
    cout << "Hexadecimal: " << hex << number << endl;  // ff
    cout << "Octal: " << oct << number << endl;  // 377
    
    return 0;
}
```

### Task 32.1
Create an array of 3 students with their names and grades. Print them in a nicely formatted table with headers, aligned columns, and grades shown to 1 decimal place.

---

## 33. Classes and Objects

### Object-Oriented Programming

```cpp
#include <iostream>
#include <string>
using namespace std;

// Defining a class
class Student {
public:
    // Member variables (attributes)
    string name;
    int age;
    
    // Constructor
    Student(string n, int a) {
        name = n;
        age = a;
    }
    
    // Member function (method)
    void introduce() {
        cout << "Hi, I'm " << name << " and I'm " << age << " years old" << endl;
    }
};

// Class with more functionality
class BankAccount {
private:
    string owner;
    double balance;
    
public:
    // Constructor
    BankAccount(string ownerName, double initialBalance = 0) {
        owner = ownerName;
        balance = initialBalance;
    }
    
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited $" << amount << ". New balance: $" << balance << endl;
        } else {
            cout << "Invalid amount" << endl;
        }
    }
    
    void withdraw(double amount) {
        if (amount > balance) {
            cout << "Insufficient funds" << endl;
        } else if (amount > 0) {
            balance -= amount;
            cout << "Withdrew $" << amount << ". New balance: $" << balance << endl;
        } else {
            cout << "Invalid amount" << endl;
        }
    }
    
    double getBalance() {
        return balance;
    }
    
    string getOwner() {
        return owner;
    }
};

int main() {
    // Creating objects
    Student student1("Alice", 20);
    Student student2("Bob", 22);
    
    // Accessing attributes and methods
    cout << student1.name << endl;
    student1.introduce();
    student2.introduce();
    
    // Using BankAccount class
    BankAccount account("Alice", 1000);
    account.deposit(500);
    account.withdraw(200);
    cout << "Current balance: $" << account.getBalance() << endl;
    
    return 0;
}
```

### Task 33.1
Create a `Book` class with attributes: title, author, and pages. Add a method `displayInfo()` that prints the book's information. Create 3 book objects and call displayInfo() on each.

---

## 34. Enumerations

### Using Enums

```cpp
#include <iostream>
#include <string>
using namespace std;

// Defining an enum
enum Day {
    MONDAY = 1,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};

// Enum class (C++11, strongly typed)
enum class OrderStatus {
    PENDING,
    PROCESSING,
    SHIPPED,
    DELIVERED,
    CANCELLED
};

// Using enums in a class
class Order {
private:
    int orderId;
    OrderStatus status;
    
public:
    Order(int id) : orderId(id), status(OrderStatus::PENDING) {}
    
    void ship() {
        if (status == OrderStatus::PROCESSING) {
            status = OrderStatus::SHIPPED;
            cout << "Order " << orderId << " has been shipped" << endl;
        } else {
            cout << "Cannot ship order with current status" << endl;
        }
    }
    
    void setStatus(OrderStatus newStatus) {
        status = newStatus;
    }
    
    string getStatusString() {
        switch (status) {
            case OrderStatus::PENDING: return "Pending";
            case OrderStatus::PROCESSING: return "Processing";
            case OrderStatus::SHIPPED: return "Shipped";
            case OrderStatus::DELIVERED: return "Delivered";
            case OrderStatus::CANCELLED: return "Cancelled";
            default: return "Unknown";
        }
    }
};

int main() {
    // Using traditional enum
    Day today = MONDAY;
    
    if (today == MONDAY) {
        cout << "Start of the week!" << endl;
    }
    
    // Using enum in switch
    switch (today) {
        case MONDAY:
        case TUESDAY:
        case WEDNESDAY:
        case THURSDAY:
        case FRIDAY:
            cout << "Weekday" << endl;
            break;
        case SATURDAY:
        case SUNDAY:
            cout << "Weekend" << endl;
            break;
    }
    
    // Using enum class
    Order order(12345);
    cout << "Status: " << order.getStatusString() << endl;
    order.setStatus(OrderStatus::PROCESSING);
    order.ship();
    cout << "Status: " << order.getStatusString() << endl;
    
    return 0;
}
```

### Task 34.1
Create an enum class called `Priority` with values: LOW, MEDIUM, HIGH, URGENT. Create a `Task` class that has a title and priority. Create 3 tasks with different priorities and print them.

---

## 35. Mini Project - Student Management System

### Putting It All Together

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

enum class Grade {
    A = 90,
    B = 80,
    C = 70,
    D = 60,
    F = 0
};

class Student {
private:
    string name;
    int studentId;
    vector<int> grades;
    
public:
    Student(string n, int id) : name(n), studentId(id) {}
    
    void addGrade(int grade) {
        if (grade >= 0 && grade <= 100) {
            grades.push_back(grade);
        } else {
            cout << "Invalid grade" << endl;
        }
    }
    
    double getAverage() {
        if (grades.empty()) return 0;
        
        int sum = 0;
        for (int grade : grades) {
            sum += grade;
        }
        return sum / (double)grades.size();
    }
    
    char getLetterGrade() {
        double avg = getAverage();
        if (avg >= 90) return 'A';
        else if (avg >= 80) return 'B';
        else if (avg >= 70) return 'C';
        else if (avg >= 60) return 'D';
        else return 'F';
    }
    
    void displayInfo() {
        cout << "\nStudent: " << name << endl;
        cout << "ID: " << studentId << endl;
        cout << "Grades: ";
        for (int grade : grades) {
            cout << grade << " ";
        }
        cout << "\nAverage: " << fixed << setprecision(2) << getAverage() << endl;
        cout << "Letter Grade: " << getLetterGrade() << endl;
    }
    
    string getName() { return name; }
    int getId() { return studentId; }
};

class StudentManagementSystem {
private:
    vector<Student*> students;
    
public:
    void addStudent(Student* student) {
        students.push_back(student);
    }
    
    Student* findStudent(int studentId) {
        for (Student* student : students) {
            if (student->getId() == studentId) {
                return student;
            }
        }
        return nullptr;
    }
    
    void displayAllStudents() {
        if (students.empty()) {
            cout << "No students in system" << endl;
            return;
        }
        for (Student* student : students) {
            student->displayInfo();
        }
    }
    
    ~StudentManagementSystem() {
        for (Student* student : students) {
            delete student;
        }
    }
};

int main() {
    StudentManagementSystem sms;
    
    // Create and add students
    Student* s1 = new Student("Alice", 1001);
    s1->addGrade(95);
    s1->addGrade(87);
    s1->addGrade(92);
    
    Student* s2 = new Student("Bob", 1002);
    s2->addGrade(78);
    s2->addGrade(84);
    s2->addGrade(81);
    
    sms.addStudent(s1);
    sms.addStudent(s2);
    
    // Display all students
    sms.displayAllStudents();
    
    return 0;
}
```

### Task 35.1
Extend the Student Management System by adding a `removeStudent(studentId)` method that removes a student from the system. Also add a method to find the student with the highest average grade.

---

## 36. Sorting

### Ordering Data

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
using namespace std;

// Bubble sort implementation
void bubbleSort(int arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

// Selection sort implementation
void selectionSort(int arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < size; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        // Swap
        int temp = arr[i];
        arr[i] = arr[minIdx];
        arr[minIdx] = temp;
    }
}

int main() {
    // Using STL sort (most common)
    vector<int> numbers = {64, 34, 25, 12, 22, 11, 90};
    
    sort(numbers.begin(), numbers.end());
    cout << "Sorted ascending: ";
    for (int num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // Sort in descending order
    sort(numbers.begin(), numbers.end(), greater<int>());
    cout << "Sorted descending: ";
    for (int num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // Sorting strings
    vector<string> names = {"Charlie", "Alice", "Bob", "David"};
    sort(names.begin(), names.end());
    for (const string& name : names) {
        cout << name << " ";
    }
    cout << endl;
    
    // Sorting arrays
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int size = 7;
    
    bubbleSort(arr, size);
    cout << "Bubble sorted: ";
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
    
    // Sorting with custom comparator (for objects)
    struct Student {
        string name;
        int grade;
    };
    
    vector<Student> students = {
        {"Alice", 85},
        {"Bob", 92},
        {"Charlie", 78}
    };
    
    // Sort by grade
    sort(students.begin(), students.end(), 
         [](const Student& a, const Student& b) {
             return a.grade < b.grade;
         });
    
    for (const Student& s : students) {
        cout << s.name << ": " << s.grade << endl;
    }
    
    return 0;
}
```

### Task 36.1
Create an array of 10 random numbers between 1 and 100. Implement a selection sort algorithm to sort them in ascending order. Print the array before and after sorting.

---

## 37. Inheritance

### Code Reuse Through Class Hierarchies

```cpp
#include <iostream>
#include <string>
using namespace std;

// Base class (parent class)
class Animal {
protected:
    string name;
    int age;
    
public:
    Animal(string n, int a) : name(n), age(a) {}
    
    void makeSound() {
        cout << "Some generic sound" << endl;
    }
    
    void displayInfo() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

// Derived class (child class)
class Dog : public Animal {
private:
    string breed;
    
public:
    Dog(string n, int a, string b) : Animal(n, a), breed(b) {}
    
    void makeSound() {
        cout << "Woof! Woof!" << endl;
    }
    
    void fetch() {
        cout << name << " is fetching the ball" << endl;
    }
    
    void displayInfo() {
        Animal::displayInfo();
        cout << "Breed: " << breed << endl;
    }
};

class Cat : public Animal {
private:
    string color;
    
public:
    Cat(string n, int a, string c) : Animal(n, a), color(c) {}
    
    void makeSound() {
        cout << "Meow!" << endl;
    }
    
    void scratch() {
        cout << name << " is scratching furniture" << endl;
    }
};

// More complex example
class Vehicle {
protected:
    string brand;
    string model;
    bool isRunning;
    
public:
    Vehicle(string b, string m) : brand(b), model(m), isRunning(false) {}
    
    void start() {
        isRunning = true;
        cout << brand << " " << model << " started" << endl;
    }
    
    void stop() {
        isRunning = false;
        cout << brand << " " << model << " stopped" << endl;
    }
};

class Car : public Vehicle {
private:
    int doors;
    
public:
    Car(string b, string m, int d) : Vehicle(b, m), doors(d) {}
    
    void honk() {
        cout << "Beep beep!" << endl;
    }
};

class Motorcycle : public Vehicle {
private:
    int cc;
    
public:
    Motorcycle(string b, string m, int c) : Vehicle(b, m), cc(c) {}
    
    void wheelie() {
        cout << "Doing a wheelie!" << endl;
    }
};

int main() {
    Dog dog("Buddy", 3, "Golden Retriever");
    dog.displayInfo();
    dog.makeSound();
    dog.fetch();
    
    cout << endl;
    
    Cat cat("Whiskers", 2, "Orange");
    cat.displayInfo();
    cat.makeSound();
    cat.scratch();
    
    cout << endl;
    
    Car car("Toyota", "Camry", 4);
    car.start();
    car.honk();
    car.stop();
    
    return 0;
}
```

### Task 37.1
Create a `Person` base class with name and age. Create two derived classes: `Student` (with studentId and major) and `Teacher` (with subject and yearsExperience). Add appropriate methods to each class.

---

## 38. Method Overriding

### Customizing Inherited Behavior

```cpp
#include <iostream>
#include <string>
using namespace std;

// Base class
class Shape {
protected:
    string color;
    
public:
    Shape(string c) : color(c) {}
    
    virtual double area() {
        return 0;
    }
    
    virtual double perimeter() {
        return 0;
    }
    
    virtual void display() {
        cout << "A " << color << " shape" << endl;
    }
    
    virtual ~Shape() {}  // Virtual destructor
};

// Derived classes with overridden methods
class Rectangle : public Shape {
private:
    double width;
    double height;
    
public:
    Rectangle(string c, double w, double h) 
        : Shape(c), width(w), height(h) {}
    
    double area() override {
        return width * height;
    }
    
    double perimeter() override {
        return 2 * (width + height);
    }
    
    void display() override {
        cout << "A " << color << " rectangle with area " << area() << endl;
    }
};

class Circle : public Shape {
private:
    double radius;
    const double PI = 3.14159;
    
public:
    Circle(string c, double r) : Shape(c), radius(r) {}
    
    double area() override {
        return PI * radius * radius;
    }
    
    double perimeter() override {
        return 2 * PI * radius;
    }
    
    void display() override {
        cout << "A " << color << " circle with area " << area() << endl;
    }
};

// Example with employees
class Employee {
protected:
    string name;
    
public:
    Employee(string n) : name(n) {}
    
    virtual double calculateSalary() {
        return 30000;
    }
    
    virtual void displayInfo() {
        cout << "Employee: " << name << endl;
        cout << "Salary: $" << calculateSalary() << endl;
    }
    
    virtual ~Employee() {}
};

class Manager : public Employee {
public:
    Manager(string n) : Employee(n) {}
    
    double calculateSalary() override {
        return 50000;
    }
};

class Developer : public Employee {
public:
    Developer(string n) : Employee(n) {}
    
    double calculateSalary() override {
        return 45000;
    }
};

int main() {
    Rectangle rect("blue", 5, 3);
    rect.display();
    cout << "Area: " << rect.area() << endl;
    cout << "Perimeter: " << rect.perimeter() << endl;
    
    cout << endl;
    
    Circle circle("red", 4);
    circle.display();
    cout << "Area: " << circle.area() << endl;
    cout << "Perimeter: " << circle.perimeter() << endl;
    
    cout << endl;
    
    // Polymorphism with pointers
    Employee* e1 = new Employee("John");
    Employee* e2 = new Manager("Sarah");
    Employee* e3 = new Developer("Mike");
    
    e1->displayInfo();
    e2->displayInfo();
    e3->displayInfo();
    
    delete e1;
    delete e2;
    delete e3;
    
    return 0;
}
```

### Task 38.1
Create a base class `Employee` with a method `calculateSalary()` that returns a base salary of 30000. Create two derived classes: `Manager` (salary = 50000) and `Developer` (salary = 45000). Override the calculateSalary() method in each derived class.

---

## 39. File I/O with Objects

### Saving and Loading Objects

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

class Student {
private:
    string name;
    int age;
    vector<int> grades;
    
public:
    Student() : age(0) {}
    Student(string n, int a) : name(n), age(a) {}
    
    void addGrade(int grade) {
        grades.push_back(grade);
    }
    
    // Save to file
    void saveToFile(ofstream& file) {
        file << name << endl;
        file << age << endl;
        file << grades.size() << endl;
        for (int grade : grades) {
            file << grade << " ";
        }
        file << endl;
    }
    
    // Load from file
    void loadFromFile(ifstream& file) {
        getline(file, name);
        file >> age;
        
        int numGrades;
        file >> numGrades;
        grades.clear();
        for (int i = 0; i < numGrades; i++) {
            int grade;
            file >> grade;
            grades.push_back(grade);
        }
        file.ignore();  // Ignore newline
    }
    
    void display() {
        cout << "Student: " << name << ", Age: " << age << endl;
        cout << "Grades: ";
        for (int grade : grades) {
            cout << grade << " ";
        }
        cout << endl;
    }
};

int main() {
    // Creating objects
    vector<Student> students;
    
    Student s1("Alice", 20);
    s1.addGrade(85);
    s1.addGrade(90);
    s1.addGrade(92);
    
    Student s2("Bob", 21);
    s2.addGrade(78);
    s2.addGrade(84);
    s2.addGrade(81);
    
    students.push_back(s1);
    students.push_back(s2);
    
    // Saving to file
    ofstream outFile("students.txt");
    if (outFile.is_open()) {
        outFile << students.size() << endl;
        for (Student& student : students) {
            student.saveToFile(outFile);
        }
        outFile.close();
        cout << "Students saved to file" << endl;
    }
    
    // Loading from file
    vector<Student> loadedStudents;
    ifstream inFile("students.txt");
    if (inFile.is_open()) {
        int numStudents;
        inFile >> numStudents;
        inFile.ignore();  // Ignore newline
        
        for (int i = 0; i < numStudents; i++) {
            Student student;
            student.loadFromFile(inFile);
            loadedStudents.push_back(student);
        }
        inFile.close();
        
        cout << "\nLoaded students from file:" << endl;
        for (Student& student : loadedStudents) {
            student.display();
        }
    }
    
    return 0;
}
```

### Task 39.1
Create a `Book` class with title, author, and year. Create a list of 3 books, save them to a file, then load them back and display them.

---

## 40. Function Overloading

### Multiple Functions with Different Parameters

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// Function overloading
class Calculator {
public:
    // Add two integers
    int add(int a, int b) {
        return a + b;
    }
    
    // Add three integers
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Add two doubles
    double add(double a, double b) {
        return a + b;
    }
    
    // Add strings
    string add(string a, string b) {
        return a + b;
    }
};

// Overloading with different number of parameters
class Printer {
public:
    void print(int value) {
        cout << "Integer: " << value << endl;
    }
    
    void print(double value) {
        cout << "Double: " << value << endl;
    }
    
    void print(string value) {
        cout << "String: " << value << endl;
    }
    
    void print(vector<int> values) {
        cout << "Vector: ";
        for (int v : values) {
            cout << v << " ";
        }
        cout << endl;
    }
};

// Overloading constructors
class Rectangle {
private:
    double width;
    double height;
    
public:
    // Default constructor
    Rectangle() : width(1), height(1) {}
    
    // Constructor with one parameter (square)
    Rectangle(double side) : width(side), height(side) {}
    
    // Constructor with two parameters
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() {
        return width * height;
    }
    
    void display() {
        cout << "Width: " << width << ", Height: " << height 
             << ", Area: " << area() << endl;
    }
};

int main() {
    Calculator calc;
    cout << calc.add(5, 3) << endl;         // 8
    cout << calc.add(5, 3, 2) << endl;      // 10
    cout << calc.add(5.5, 3.2) << endl;     // 8.7
    cout << calc.add("Hello", " World") << endl;  // Hello World
    
    cout << endl;
    
    Printer printer;
    printer.print(42);
    printer.print(3.14);
    printer.print("Hello");
    printer.print(vector<int>{1, 2, 3, 4, 5});
    
    cout << endl;
    
    Rectangle r1;           // Default
    Rectangle r2(5);        // Square
    Rectangle r3(4, 6);     // Rectangle
    
    r1.display();
    r2.display();
    r3.display();
    
    return 0;
}
```

### Task 40.1
Create a `Shape` class with multiple constructors: one for Circle (one parameter: radius), one for Rectangle (two parameters: width, height), and one for Triangle (three parameters: three sides). Each should calculate and display the area.

---

## 41. Polymorphism

### Using Different Types Interchangeably

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// Polymorphism through inheritance and virtual functions
class Animal {
protected:
    string name;
    
public:
    Animal(string n) : name(n) {}
    
    virtual void speak() {
        cout << name << " makes a sound" << endl;
    }
    
    virtual ~Animal() {}
};

class Dog : public Animal {
public:
    Dog(string n) : Animal(n) {}
    
    void speak() override {
        cout << name << " says: Woof!" << endl;
    }
};

class Cat : public Animal {
public:
    Cat(string n) : Animal(n) {}
    
    void speak() override {
        cout << name << " says: Meow!" << endl;
    }
};

class Cow : public Animal {
public:
    Cow(string n) : Animal(n) {}
    
    void speak() override {
        cout << name << " says: Moo!" << endl;
    }
};

// Function that works with any animal
void makeAnimalSpeak(Animal* animal) {
    animal->speak();
}

// Polymorphism with shapes
class Shape {
public:
    virtual double area() = 0;  // Pure virtual function (abstract)
    virtual double perimeter() = 0;
    virtual ~Shape() {}
};

class Rectangle : public Shape {
private:
    double width;
    double height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() override {
        return width * height;
    }
    
    double perimeter() override {
        return 2 * (width + height);
    }
};

class Circle : public Shape {
private:
    double radius;
    const double PI = 3.14159;
    
public:
    Circle(double r) : radius(r) {}
    
    double area() override {
        return PI * radius * radius;
    }
    
    double perimeter() override {
        return 2 * PI * radius;
    }
};

int main() {
    // Polymorphism with animals
    vector<Animal*> animals;
    animals.push_back(new Dog("Buddy"));
    animals.push_back(new Cat("Whiskers"));
    animals.push_back(new Cow("Bessie"));
    animals.push_back(new Dog("Max"));
    
    for (Animal* animal : animals) {
        animal->speak();
    }
    
    // Clean up
    for (Animal* animal : animals) {
        delete animal;
    }
    
    cout << endl;
    
    // Polymorphism with shapes
    vector<Shape*> shapes;
    shapes.push_back(new Rectangle(5, 3));
    shapes.push_back(new Circle(4));
    shapes.push_back(new Rectangle(10, 2));
    shapes.push_back(new Circle(7));
    
    double totalArea = 0;
    for (Shape* shape : shapes) {
        double area = shape->area();
        cout << "Area: " << area << endl;
        totalArea += area;
    }
    cout << "Total area: " << totalArea << endl;
    
    // Clean up
    for (Shape* shape : shapes) {
        delete shape;
    }
    
    return 0;
}
```

### Task 41.1
Create a base class `Vehicle` with a virtual method `move()`. Create three derived classes: `Car`, `Boat`, and `Plane`, each with their own implementation of `move()`. Create a vector of different vehicles and call `move()` on each one.

---

## 42. Basic Console Menu System

### Creating Interactive Programs

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <limits>
using namespace std;

class MenuItem {
private:
    string title;
    string description;
    
public:
    MenuItem(string t, string d) : title(t), description(d) {}
    
    string getTitle() { return title; }
    string getDescription() { return description; }
};

class Menu {
private:
    vector<MenuItem> items;
    string title;
    
public:
    Menu(string t) : title(t) {}
    
    void addItem(MenuItem item) {
        items.push_back(item);
    }
    
    void display() {
        cout << "\n=== " << title << " ===" << endl;
        for (size_t i = 0; i < items.size(); i++) {
            cout << (i + 1) << ". " << items[i].getTitle() << endl;
        }
        cout << "0. Exit" << endl;
    }
    
    int getChoice() {
        int choice;
        cout << "\nEnter your choice: ";
        
        while (!(cin >> choice) || choice < 0 || choice > items.size()) {
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid choice. Try again: ";
        }
        
        return choice;
    }
};

// Example application
class Calculator {
public:
    void run() {
        Menu menu("Calculator");
        menu.addItem(MenuItem("Add", "Add two numbers"));
        menu.addItem(MenuItem("Subtract", "Subtract two numbers"));
        menu.addItem(MenuItem("Multiply", "Multiply two numbers"));
        menu.addItem(MenuItem("Divide", "Divide two numbers"));
        
        bool running = true;
        while (running) {
            menu.display();
            int choice = menu.getChoice();
            
            switch (choice) {
                case 0:
                    running = false;
                    cout << "Goodbye!" << endl;
                    break;
                case 1:
                    performOperation('+');
                    break;
                case 2:
                    performOperation('-');
                    break;
                case 3:
                    performOperation('*');
                    break;
                case 4:
                    performOperation('/');
                    break;
            }
        }
    }
    
private:
    void performOperation(char op) {
        double a, b;
        cout << "Enter first number: ";
        cin >> a;
        cout << "Enter second number: ";
        cin >> b;
        
        double result;
        switch (op) {
            case '+': result = a + b; break;
            case '-': result = a - b; break;
            case '*': result = a * b; break;
            case '/':
                if (b != 0) {
                    result = a / b;
                } else {
                    cout << "Error: Division by zero!" << endl;
                    return;
                }
                break;
        }
        
        cout << "Result: " << result << endl;
    }
};

int main() {
    Calculator calc;
    calc.run();
    
    return 0;
}
```

### Task 42.1
Create a menu-driven contact manager where users can: 1) Add contact, 2) View all contacts, 3) Search for a contact, 4) Exit. Store contacts in a vector of objects.

---

## 43. Advanced Console Interaction

### Input Validation and Formatting

```cpp
#include <iostream>
#include <iomanip>
#include <string>
#include <sstream>
#include <limits>
using namespace std;

class InputValidator {
public:
    static int getInt(string prompt, int min, int max) {
        int value;
        while (true) {
            cout << prompt;
            if (cin >> value && value >= min && value <= max) {
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                return value;
            }
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid input. Please enter a number between " 
                 << min << " and " << max << endl;
        }
    }
    
    static double getDouble(string prompt) {
        double value;
        while (true) {
            cout << prompt;
            if (cin >> value) {
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                return value;
            }
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            cout << "Invalid input. Please enter a number." << endl;
        }
    }
    
    static string getString(string prompt) {
        string value;
        cout << prompt;
        cin.ignore();
        getline(cin, value);
        return value;
    }
    
    static bool getYesNo(string prompt) {
        string response;
        while (true) {
            cout << prompt << " (yes/no): ";
            cin >> response;
            if (response == "yes" || response == "y") return true;
            if (response == "no" || response == "n") return false;
            cout << "Please enter 'yes' or 'no'" << endl;
        }
    }
};

class FormattedOutput {
public:
    static void printHeader(string text) {
        int width = 50;
        cout << string(width, '=') << endl;
        cout << "| " << left << setw(width - 4) << text << " |" << endl;
        cout << string(width, '=') << endl;
    }
    
    static void printTable(vector<vector<string>> data, vector<int> widths) {
        // Print top border
        for (int width : widths) {
            cout << "+" << string(width + 2, '-');
        }
        cout << "+" << endl;
        
        // Print rows
        for (const auto& row : data) {
            for (size_t i = 0; i < row.size(); i++) {
                cout << "| " << left << setw(widths[i]) << row[i] << " ";
            }
            cout << "|" << endl;
        }
        
        // Print bottom border
        for (int width : widths) {
            cout << "+" << string(width + 2, '-');
        }
        cout << "+" << endl;
    }
};

int main() {
    FormattedOutput::printHeader("Welcome to the System");
    
    string name = InputValidator::getString("Enter your name: ");
    int age = InputValidator::getInt("Enter your age (1-120): ", 1, 120);
    double salary = InputValidator::getDouble("Enter your salary: ");
    bool confirmed = InputValidator::getYesNo("Confirm information?");
    
    if (confirmed) {
        cout << "\nUser Information:" << endl;
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
        cout << fixed << setprecision(2);
        cout << "Salary: $" << salary << endl;
        
        // Example table
        vector<vector<string>> data = {
            {"Name", "Age", "Salary"},
            {name, to_string(age), "$" + to_string(salary)}
        };
        vector<int> widths = {20, 5, 12};
        
        cout << "\nFormatted Table:" << endl;
        FormattedOutput::printTable(data, widths);
    }
    
    return 0;
}
```

### Task 43.1
Create a simple grade management system with proper input validation that: asks for student name (string), number of grades (1-10), then each grade (0-100), and displays a formatted report with average.

---

## Conclusion

Congratulations on completing this C++ programming tutorial! You've covered:

- **Basics**: Variables, data types, operators, input/output
- **Control Flow**: If statements, loops, switch statements
- **Functions**: Creating and using functions, parameters, overloading
- **Collections**: Arrays, vectors, 2D arrays, iteration
- **File I/O**: Reading and writing files
- **OOP**: Classes, inheritance, polymorphism, virtual functions
- **Advanced**: Enums, sorting, file I/O with objects
- **Best Practices**: Input validation, formatting, debugging

## Next Steps

1. **Practice**: Complete all the tasks in this tutorial
2. **Build Projects**: Create your own programs combining these concepts
3. **Explore STL**: Learn about Standard Template Library containers and algorithms
4. **Learn Modern C++**: Study C++11, C++14, C++17, and C++20 features
5. **Read Documentation**: https://cppreference.com/
6. **Join Communities**: C++ forums, Stack Overflow, Reddit's r/cpp_questions

Keep coding and exploring!