I'll create a worksheet based on the lecture content with quiz questions, coding tasks, and a summary table mapping to the tutorial.

# Lecture 2: C++ Functions and Classes Worksheet

## Part 1: Quiz Questions

### Question 1: Fill in the Blanks

Complete the following paragraph about functions in C++ using the words from the word bank below:

"A function consists of a __________, a name, and __________. When a function is called, it is pushed onto the __________. The function must include a __________ statement if it has a non-void return type. The combination of a function's return type, name, and parameter types is called its __________."

**Word Bank:** `return`, `parameters`, `call stack`, `signature`, `return type`

---

### Question 2: Fill in the Blanks

Complete the following paragraph about classes using the words from the word bank below:

"In C++, a class bundles together data and functions. The data is stored in __________ variables, while the functions are called __________. A special function called the __________ initializes the object's state. Members marked as __________ cannot be accessed from outside the class, while __________ members form the interface for using the class."

**Word Bank:** `private`, `constructor`, `member`, `public`, `methods`

---

### Question 3: Multiple Choice

What will be the output of the following code?

```cpp
#include <iostream>
using namespace std;

int modify(int x) {
    x = x * 2;
    return x + 5;
}

int main() {
    int value = 10;
    int result = modify(value);
    cout << value << " " << result;
    return 0;
}
```

A) `10 25`  
B) `20 25`  
C) `10 20`  
D) `20 30`

---

### Question 4: Multiple Choice

Given the following class hierarchy, which statement is TRUE?

```cpp
class Animal {
protected:
    string name;
public:
    Animal(string n) : name(n) {}
    void speak() { cout << "Sound"; }
};

class Dog : public Animal {
public:
    Dog(string n) : Animal(n) {}
    void speak() { cout << "Woof"; }
    void fetch() { cout << "Fetching"; }
};
```

A) A Dog object can directly access the `name` variable from outside the class  
B) A Dog object can call the `speak()` method from the Animal class  
C) An Animal object can call the `fetch()` method  
D) The Dog class can directly access the `name` variable within its own methods

---

## Part 2: Coding Tasks

### Task 1: Temperature Converter with Functions

Write a program that uses functions to convert temperatures between Celsius, Fahrenheit, and Kelvin. Your program should include:

- A function `celsiusToFahrenheit(double celsius)` that returns the temperature in Fahrenheit
- A function `celsiusToKelvin(double celsius)` that returns the temperature in Kelvin
- A function `fahrenheitToCelsius(double fahrenheit)` that returns the temperature in Celsius

In your `main()` function:
1. Ask the user to enter a temperature in Celsius
2. Display the temperature in all three scales using your functions
3. Test with the value 25°C

**Expected output format:**
```
Enter temperature in Celsius: 25
25°C = 77°F
25°C = 298.15K
```

**Formulas:**
- Fahrenheit = (Celsius × 9/5) + 32
- Kelvin = Celsius + 273.15
- Celsius = (Fahrenheit - 32) × 5/9

---

### Task 2: BankAccount Class Hierarchy

Create a class hierarchy for a simple banking system:

**Base class `BankAccount`:**
- Private member: `balance` (double)
- Protected member: `accountNumber` (int)
- Constructor that takes account number and initial balance
- Public methods:
  - `deposit(double amount)` - adds to balance
  - `getBalance()` - returns current balance
  - `displayInfo()` - prints account number and balance

**Derived class `SavingsAccount`:**
- Private member: `interestRate` (double, e.g., 0.03 for 3%)
- Constructor that takes account number, initial balance, and interest rate
- Public method:
  - `applyInterest()` - adds interest to the balance (balance × interestRate)
  - Override `displayInfo()` to also show the interest rate

In `main()`:
1. Create a `SavingsAccount` with account number 12345, initial balance 1000, and 5% interest rate
2. Deposit 500
3. Apply interest
4. Display the account information

---

## Part 3: Summary Table

### Mapping Lecture Topics to Tutorial Tasks

| Lecture Topic | Related Tutorial Section(s) | Task Number(s) |
|---------------|----------------------------|----------------|
| Functions - Declaration and Implementation | Section 18: Functions (Methods) | Task 18.1 |
| Functions - Parameters and Return Values | Section 19: Functions with Parameters | Task 19.1 |
| Functions - Testing | Section 20: Testing Functions | Task 20.1 |
| Classes - Creating and Using | Section 33: Classes and Objects | Task 33.1 |
| Classes - Enumerations | Section 34: Enumerations | Task 34.1 |
| Inheritance - Creating Derived Classes | Section 37: Inheritance | Task 37.1 |
| Inheritance - Method Overriding | Section 38: Method Overriding | Task 38.1 |
| Code Reuse - Function Overloading | Section 40: Function Overloading | Task 40.1 |
| Code Reuse - Polymorphism | Section 41: Polymorphism | Task 41.1 |

---

**End of Worksheet**

---

