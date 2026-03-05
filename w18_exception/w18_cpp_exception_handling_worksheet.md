# Lecture X: C++ Worksheet

---

## Section 1: Quiz

---

### Part A: Fill in the Blanks

For each question below, choose the correct word from the word bank to complete the statement. Each word is used once.

---

**Question 1**

Word bank: `noexcept` | `catch` | `throw` | `try`

When writing exception handling code, risky operations are placed inside a __________ block. If an error condition is detected, the keyword __________ is used to signal the problem. A matching __________ block then handles the error. The `what()` method on an exception class should always be marked __________ to guarantee it will not itself cause a failure during error handling.

---

**Question 2**

Word bank: `std::exception` | `what()` | `cerr` | `runtime_error`

The base class for all standard library exceptions in C++ is __________. When catching an exception by reference, calling __________ on the caught object returns a C-style string describing the problem. Error messages during exception handling are conventionally written to __________ rather than `cout` because it is unbuffered. When a problem can only be detected during execution rather than at compile time, a suitable exception type to throw is __________.

---

**Question 3**

Word bank: `override` | `public` | `virtual` | `noexcept`

When creating a custom exception class, the `what()` method must be declared __________ so that derived classes can redefine it. The keyword __________ should appear after the parameter list and `const` qualifier to indicate the function will not propagate exceptions. The __________ keyword confirms to the compiler that this method intentionally replaces the base class version. The custom exception must inherit __________ from `std::exception` so that it can be caught by a general `catch(std::exception&)` handler.

---

**Question 4**

Word bank: `specific` | `value` | `reference` | `general`

Exceptions should always be caught by __________ to avoid slicing and unnecessary copies. Conversely, exceptions should be thrown by __________, not by pointer. When multiple `catch` clauses follow a single `try` block, the most __________ exception type must appear first; placing a __________ handler such as `catch(std::exception&)` first would prevent all handlers below it from ever executing.

---

### Part B: Multiple Choice

For each question, select the best answer.

---

**Question 5**

Consider the following code:

```
unsigned int x = 0;
x--;
```

What value does `x` hold after this operation?

a) -1
b) 0
c) 4294967295
d) The program throws an exception automatically

---

**Question 6**

Which of the following statements about the `noexcept` keyword is correct?

a) A `noexcept` function silently ignores any exceptions it encounters.
b) If a `noexcept` function throws, `std::terminate()` is called immediately.
c) `noexcept` was introduced in C++17 as a replacement for `throw(int)`.
d) Marking a function `noexcept` prevents the compiler from generating it.

---

**Question 7**

A developer writes three `catch` clauses after a `try` block in the following order:

```
catch(std::exception& e)       // Handler A
catch(std::runtime_error& e)   // Handler B
catch(std::overflow_error& e)  // Handler C
```

An `std::overflow_error` is thrown. Which handler executes?

a) Handler C, because it is the most specific match.
b) Handler A, because it is listed first and `std::overflow_error` is a subtype of `std::exception`.
c) Handler B, because `std::overflow_error` derives from `std::runtime_error`.
d) All three handlers execute in sequence.

---

## Section 2: Programming Tasks

---

### Task 1: Safe Division Function

Write a function called `safeDivide` that takes two `double` parameters and returns their quotient. If the divisor is zero, the function should throw a `std::invalid_argument` exception with an appropriate message. In `main`, call the function inside a `try...catch` block, demonstrate both a successful division and an attempt to divide by zero, and print the results.

Use the scaffold below as your starting point.

```cpp
#include <iostream>
#include <stdexcept>

// TODO: Implement safeDivide
// Parameters: double numerator, double denominator
// Returns:    double
// Throws:     std::invalid_argument if denominator is zero


int main()
{
    // TODO: Call safeDivide(10.0, 2.5) inside a try block and print the result

    // TODO: Call safeDivide(7.0, 0.0) inside a try block and catch
    //       std::invalid_argument, printing ex.what()

    return 0;
}
```

**Expected Output:**

```
10 / 2.5 = 4
Division failed: Cannot divide by zero.
```

---

### Task 2: Validated Age Input

Write a function called `validateAge` that accepts an `int` parameter representing a person's age. The function should throw:

- `std::underflow_error` if the age is negative.
- `std::overflow_error` if the age is greater than 150.

If the age is valid, the function should print a confirmation message.

In `main`, test the function with at least three values: one valid age, one negative age, and one age above 150. Use separate `catch` clauses for each exception type.

```cpp
#include <iostream>
#include <stdexcept>

// TODO: Implement validateAge
// Parameter: int age
// Throws:    std::underflow_error if age < 0
//            std::overflow_error  if age > 150


int main()
{
    int testAges[] = { 25, -5, 200 };

    for (int age : testAges)
    {
        // TODO: Call validateAge inside a try block.
        //       Catch std::underflow_error and std::overflow_error separately,
        //       printing [Underflow] or [Overflow] followed by ex.what()
    }

    return 0;
}
```

**Expected Output:**

```
Age 25 is valid.
[Underflow] Age cannot be negative.
[Overflow] Age cannot exceed 150.
```

---

### Challenge Task: Custom Exception Hierarchy for a Bank Account

Design a small `BankAccount` class with a custom exception hierarchy. The requirements are:

- A base exception class `BankException` that inherits from `std::exception` and stores a message string returned by `what()`.
- Two derived exceptions:
  - `InsufficientFundsException`: thrown when a withdrawal exceeds the current balance. It should store the shortfall amount (how much more was requested than available) and expose it through a `shortfall()` method.
  - `NegativeDepositException`: thrown when a deposit amount is less than or equal to zero.
- A `BankAccount` class with:
  - A constructor that sets the initial balance.
  - A `deposit(double amount)` method.
  - A `withdraw(double amount)` method.
  - A `balance()` accessor method.

Demonstrate all three exception types in `main` using appropriate `catch` blocks.

```cpp
#include <iostream>
#include <exception>
#include <string>

// TODO: Implement BankException (base)
// TODO: Implement InsufficientFundsException : public BankException
//       - stores shortfall amount
//       - shortfall() accessor method
// TODO: Implement NegativeDepositException : public BankException

// TODO: Implement BankAccount
// - Constructor: BankAccount(double initialBalance)
// - void deposit(double amount)   -- throws NegativeDepositException if amount <= 0
// - void withdraw(double amount)  -- throws InsufficientFundsException if amount > balance
// - double balance() const

int main()
{
    BankAccount account(100.0);

    // TODO: Demonstrate a valid deposit
    // TODO: Demonstrate a valid withdrawal
    // TODO: Demonstrate a withdrawal that exceeds balance and catch
    //       InsufficientFundsException; print the shortfall using ex.shortfall()
    // TODO: Demonstrate a negative deposit and catch NegativeDepositException

    return 0;
}
```

**Expected Output:**

```
Deposited 50.00. New balance: 150.00
Withdrew 30.00. New balance: 120.00
[Insufficient Funds] Withdrawal failed: insufficient funds. Shortfall: 80.00
[Invalid Deposit] Deposit failed: amount must be greater than zero.
```

---

*End of Worksheet*
