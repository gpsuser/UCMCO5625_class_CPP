# Unit Testing with Boost.Test

## Learning Outcomes

By the end of this lecture, students should be able to:

- Explain why unit testing is important in C++ development and why third-party libraries are needed for it
- Set up the Boost.Test header-only library in a Visual Studio project and create a working test configuration
- Write basic test cases using `BOOST_AUTO_TEST_CASE` with `BOOST_CHECK_EQUAL` and `BOOST_TEST` assertions
- Organise test cases using suites and fixtures to group related tests around a shared object instance
- Write parameterised (data-driven) tests using `BOOST_DATA_TEST_CASE` and the zip operator
- Apply random data generation to stress-test a class with many automatically generated inputs
- Apply floating-point tolerances to handle approximate results in test assertions

---

## Table of Contents

1. [Section 1: Why Unit Testing Matters in C++](#section-1-why-unit-testing-matters-in-c)
2. [Section 2: Introducing Boost.Test](#section-2-introducing-boosttest)
3. [Section 3: Setting Up Boost.Test in Visual Studio](#section-3-setting-up-boosttest-in-visual-studio)
4. [Section 4: Writing Your First Test Cases](#section-4-writing-your-first-test-cases)
5. [Section 5: Parameterised (Data-Driven) Tests](#section-5-parameterised-data-driven-tests)
6. [Section 6: Random Testing](#section-6-random-testing)
7. [Section 7: Fixtures and Test Suites](#section-7-fixtures-and-test-suites)
8. [Section 8: Handling Floating-Point Comparisons and Tolerances](#section-8-handling-floating-point-comparisons-and-tolerances)
9. [Resources](#resources)

---

## Section 1: Why Unit Testing Matters in C++

Unit testing is the practice of testing individual components of a program in isolation to verify that they behave as expected. Rather than waiting until a full program is assembled and then running it to see what happens, unit testing allows developers to verify each function or class incrementally.

### The problem with manual testing

When software grows in complexity, manually testing every function after every change becomes impractical. A small edit in one area can unexpectedly break behaviour elsewhere. Without a formal test suite, these regressions can go unnoticed until they cause visible failures in production.

Unit tests act as a safety net: run the suite after every change, and any newly introduced breakage is flagged immediately.

### Why C++ lacks built-in testing support

Languages such as Python and Java ship with testing frameworks as part of their standard libraries. C++ does not. This is largely a consequence of the language's philosophy of providing a minimal core and deferring higher-level concerns to libraries. As a result, C++ developers rely on third-party testing frameworks such as:

- Boost.Test
- Google Test (gtest)
- Catch2
- doctest

Each has its own trade-offs. This lecture focuses on Boost.Test because it is mature, well-documented, and representative of how many professional C++ testing frameworks operate. Importantly, it has a strong track record of influencing C++ standardisation — many features originally found in Boost have eventually made their way into the standard library.

---

[Back to Table of Contents](#table-of-contents)

---

## Section 2: Introducing Boost.Test

### What is Boost?

Boost is a large collection of peer-reviewed C++ libraries that extend the capabilities of the language. Libraries in the Boost collection are considered high-quality and are frequently used as proving grounds for features that may later enter the C++ standard.

The Boost.Test library provides:

- A macro-based system for declaring test cases and assertions
- Tools for organising tests into suites
- Support for data-driven and random testing
- Detailed output reporting on which tests passed or failed

Full documentation is available at:
https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/index.html

### Three distribution variants

Boost.Test can be used in three forms:

- **Header-only**: all logic is compiled into your project from headers alone. No pre-compilation is needed. Portable and simple to install, but adds to compile time.
- **Static library**: the Boost.Test code is compiled once and linked as a `.lib` file. Faster to compile your own project against.
- **Shared library**: similar to static, but the library can be shared across multiple projects at runtime.

For this course we use the **header-only** variant. It is the most portable option and requires no separate build step — just include the right header and you are ready to go.

### Initialising Boost.Test

To use the header-only variant, add the following at the top of your test file (before any other Boost.Test includes):

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
```

`BOOST_TEST_MAIN` tells Boost.Test to generate its own `main()` function. This means your test file should not define a `main()` of its own — Boost handles program entry automatically.

### Macros as the primary interface

Rather than writing conventional function calls, Boost.Test uses C preprocessor macros for its main operations. This may look unusual at first, but the rationale is pragmatic: macros allow the framework to capture source file names and line numbers for error reporting, and they allow test case declarations to look like named blocks without requiring the user to manually register each test.

The macros fall into three categories:

- **Declaring test cases** — `BOOST_AUTO_TEST_CASE`, `BOOST_DATA_TEST_CASE`
- **Organising tests into suites** — `BOOST_FIXTURE_TEST_SUITE`, `BOOST_AUTO_TEST_SUITE_END`
- **Assertions** — `BOOST_CHECK_EQUAL`, `BOOST_TEST`, `BOOST_REQUIRE`, and others

The design rationale behind this macro-driven approach is discussed here:
https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/boost_test/intro/design_rationale.html

---

[Back to Table of Contents](#table-of-contents)

---

## Section 3: Setting Up Boost.Test in Visual Studio

### Installing Boost via NuGet

In Visual Studio, the easiest way to obtain Boost is through the NuGet package manager:

1. Open your project in Visual Studio.
2. Go to **Tools -> NuGet Package Manager -> Manage NuGet Packages for Solution**.
3. Search for `boost` and install the main Boost package.
4. The download is approximately 2 GB, so allow time for this step.

Once installed, the Boost headers will be available on the include path for your project.

### Why a separate test configuration is needed

A typical project has a `main()` function in `Source.cpp` (or similar). Boost.Test generates its own `main()`. If both are compiled together, the compiler will produce a "multiple definition of main" error.

The solution is to create a separate **build configuration** for testing. In this configuration, the production `main()` file is excluded from the build, and the test file is included. In the normal Debug configuration, the reverse applies.

### Creating a test build configuration step by step

```
1. In Visual Studio, go to:
   Build -> Configuration Manager

2. Under the "Active solution configuration" dropdown, click <New...>

3. Name the new configuration "Debug Test"
   Copy settings from: "Debug"
   Click OK

4. Exclude the production main file (e.g. Source.cpp) from "Debug Test":
   - Switch to "Debug Test" configuration
   - Right-click Source.cpp in Solution Explorer
   - Select Properties
   - Set "Excluded From Build" to Yes

5. Exclude the test file (e.g. tests.cpp) from "Debug":
   - Switch to "Debug" configuration
   - Right-click tests.cpp in Solution Explorer
   - Select Properties
   - Set "Excluded From Build" to Yes
```

This ensures each configuration compiles cleanly without conflicting `main()` definitions.

---

[Back to Table of Contents](#table-of-contents)

---

## Section 4: Writing Your First Test Cases

### A case study: the Mortgage class

To make testing concrete, we will use a `Mortgage` class throughout this lecture. The class models a repayment mortgage and exposes the following interface:

- `Mortgage(term, amount, rate)` — constructor taking the term in years, the loan amount, and the annual interest rate as a percentage
- `getPayment()` — returns the monthly repayment amount
- `getTotalInterest()` — returns the total interest paid over the full term
- `owedAtYear(y)` — returns the outstanding balance at the end of year `y`

The standard formula for monthly mortgage repayments is:

```
         P * r * (1 + r)^n
M = --------------------------
         (1 + r)^n - 1

Where:
  P = principal (amount borrowed)
  r = monthly interest rate (annual rate / 12 / 100)
  n = number of monthly payments (term in years * 12)
  M = monthly payment
```

Below is a minimal but complete implementation that will be used throughout the examples:

```cpp
#include <iostream>
#include <cmath>

class Mortgage {
private:
    int term;
    double amount;
    double rate;

public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }

    double getTotalInterest() const {
        return getPayment() * term * 12 - amount;
    }

    double owedAtYear(int y) const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        int paid = y * 12;
        double balance = amount * std::pow(1 + r, paid)
                       - getPayment() * (std::pow(1 + r, paid) - 1) / r;
        return balance < 0 ? 0 : balance;
    }
};

int main() {
    Mortgage m(25, 150000, 3.5);
    std::cout << "Monthly payment: " << m.getPayment() << "\n";
    std::cout << "Owed at year 25: " << m.owedAtYear(25) << "\n";
    std::cout << "Total interest:  " << m.getTotalInterest() << "\n";
    return 0;
}
```

Sample output:

```
Monthly payment: 750.65
Owed at year 25: 3.45e-05
Total interest:  75195.7
```

### A simple test case

We can verify that a 25-year £150,000 mortgage at 3.5% produces a monthly payment of £751 and clears completely at the end of the term. An online mortgage calculator confirms these values.

The macro `BOOST_AUTO_TEST_CASE` declares a named test. Inside it, we use assertion macros to check individual conditions:

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <cmath>

// Mortgage class definition would normally be in a header;
// included inline here for a self-contained example.
class Mortgage {
private:
    int term; double amount; double rate;
public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }

    double owedAtYear(int y) const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        int paid = y * 12;
        double balance = amount * std::pow(1 + r, paid)
                       - getPayment() * (std::pow(1 + r, paid) - 1) / r;
        return balance < 0 ? 0 : balance;
    }
};

BOOST_AUTO_TEST_CASE(basic_mortgage_check) {
    Mortgage m(25, 150000, 3.5);
    BOOST_CHECK_EQUAL(std::round(m.getPayment()), 751);
    BOOST_CHECK_EQUAL(std::round(m.owedAtYear(25)), 0);
}
```

Sample output (run in "Debug Test" configuration):

```
Running 1 test case...

*** No errors detected
```

### BOOST_CHECK_EQUAL vs BOOST_TEST

There are two common ways to express equality assertions. Both are valid; the choice is largely stylistic:

**Using `BOOST_CHECK_EQUAL`:**

```cpp
BOOST_CHECK_EQUAL(std::round(m.getPayment()), 751);
```

**Using `BOOST_TEST`:**

```cpp
BOOST_TEST(std::round(m.getPayment()) == 751);
```

The `BOOST_TEST` macro is more flexible and can handle a wider range of expressions, including relational operators like `<` and `>=`. `BOOST_CHECK_EQUAL` is slightly more explicit about equality and produces clearer output when two values do not match (it prints both the expected and actual values side by side).

A key distinction to know is between `BOOST_CHECK` and `BOOST_REQUIRE`:

- `BOOST_CHECK` — records a failure but continues executing the rest of the test case.
- `BOOST_REQUIRE` — records a failure and immediately aborts the current test case. Use this when subsequent assertions would be meaningless if the first one fails (for example, when checking a pointer for null before dereferencing it).

---

[Back to Table of Contents](#table-of-contents)

---

## Section 5: Parameterised (Data-Driven) Tests

### The motivation

Suppose you want to test that the mortgage calculator produces correct monthly payments for a range of different term lengths. Writing a separate `BOOST_AUTO_TEST_CASE` for each scenario is repetitive and hard to maintain. Data-driven testing solves this by separating the test logic from the test data.

### BOOST_DATA_TEST_CASE

The `BOOST_DATA_TEST_CASE` macro accepts a dataset and executes the test body once for each element in that dataset.

To use it, include the data testing header:

```cpp
#include <boost/test/data/test_case.hpp>
```

and bring the data namespace into scope:

```cpp
namespace bdata = boost::unit_test::data;
```

### The zip operator

When testing with multiple related values (for example, a term and its expected payment), you need to pair up the datasets. Boost.Test uses the `^` operator as a **zip** operator. It takes two (or more) datasets and produces a dataset of tuples, where element `i` of the result pairs element `i` from the left dataset with element `i` from the right.

```
bdata::make({17, 18, 19, 20})   ^   bdata::make({993, 949, 909, 874})
      |                                       |
   terms                               expected payments
      |                                       |
      +------- zipped: (17,993), (18,949), (19,909), (20,874)
```

The last two arguments to `BOOST_DATA_TEST_CASE` name the variables that each element is bound to inside the test body.

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <boost/test/data/test_case.hpp>
#include <cmath>

namespace bdata = boost::unit_test::data;

class Mortgage {
private:
    int term; double amount; double rate;
public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }
};

BOOST_DATA_TEST_CASE(
    parameterised_payment,
    bdata::make({17, 18, 19, 20}) ^ bdata::make({993, 949, 909, 874}),
    y, p
) {
    Mortgage m(y, 165000, 2.49);
    BOOST_TEST(std::round(m.getPayment()) == p);
}
```

Sample output:

```
Running 4 test cases...

*** No errors detected
```

Each of the four input pairs runs independently. If one fails, the others still execute, and the output identifies exactly which parameter combination caused the failure.

---

[Back to Table of Contents](#table-of-contents)

---

## Section 6: Random Testing

### What is random (fuzz) testing?

Parameterised tests let you verify specific known input/output pairs. But there is another valuable form of testing: providing a large volume of randomly generated inputs to verify a **property** that should hold in all cases, without needing to know the exact expected output for each input.

For example: regardless of the term, amount, or rate of a mortgage, the balance owed at the end of the term should always be zero (or negligibly close to it). This is a property we can check with thousands of randomly generated mortgages without ever needing to know the monthly payment for any specific combination.

### Generating random data with Boost.Test

Boost.Test provides `bdata::random()` to generate random values. For integers it accepts a range; for floating-point values you pass a standard distribution object from `<random>`.

The `bdata::xrange(n)` produces a sequence `0, 1, 2, ..., n-1` and is used to control how many test cases are generated. It also provides a useful index variable that identifies each run in the output.

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <boost/test/data/test_case.hpp>
#include <cmath>
#include <random>

namespace bdata = boost::unit_test::data;

class Mortgage {
private:
    int term; double amount; double rate;
public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }

    double owedAtYear(int y) const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        int paid = y * 12;
        double balance = amount * std::pow(1 + r, paid)
                       - getPayment() * (std::pow(1 + r, paid) - 1) / r;
        return balance < 0 ? 0 : balance;
    }
};

BOOST_DATA_TEST_CASE(
    random_owed_at_end_of_term,
    bdata::random(1, 25)
    ^ bdata::random(100000, 200000)
    ^ bdata::random(bdata::distribution =
          std::uniform_real_distribution<double>(2, 8))
    ^ bdata::xrange(1000),
    term, amount, rate, index
) {
    Mortgage m(term, amount, rate);
    BOOST_TEST(std::round(m.owedAtYear(term)) == 0);
}
```

Sample output (truncated):

```
Running 1000 test cases...

*** No errors detected
```

### Breaking down the dataset expression

The four datasets zipped together are:

- `bdata::random(1, 25)` — a random integer between 1 and 25 (mortgage term in years)
- `bdata::random(100000, 200000)` — a random integer between 100,000 and 200,000 (loan amount in pounds)
- `bdata::random(bdata::distribution = std::uniform_real_distribution<double>(2, 8))` — a random double between 2.0 and 8.0 (annual interest rate as a percentage). Note the use of `std::uniform_real_distribution` here, which produces uniformly distributed floating-point values — the same distribution type used in the pseudorandom number generation lecture.
- `bdata::xrange(1000)` — produces a sequence from 0 to 999, controlling how many iterations are run and providing the `index` variable for referencing individual runs in error messages.

Visually:

```
+--------------------------+---------+
|  bdata::random(1,25)     | term    |
+--------------------------+---------+
|  bdata::random(100k,200k)| amount  |    } zipped with ^
+--------------------------+---------+
|  uniform_real(2.0, 8.0)  | rate    |
+--------------------------+---------+
|  bdata::xrange(1000)     | index   |
+--------------------------+---------+
          |
          v
   1000 test cases, each with a unique (term, amount, rate, index) tuple
```

---

[Back to Table of Contents](#table-of-contents)

---

## Section 7: Fixtures and Test Suites

### The problem fixtures solve

The inverse of testing one property across many instances is testing many properties on a single instance. Consider that we might want to check the monthly payment, the balance at year 10, and the total interest all for the same mortgage configuration. We could create the `Mortgage` object inside each test case, but that is repetitive and makes it easy for test cases to accidentally use slightly different configurations.

A **fixture** solves this by defining the shared object once. Each test case in the suite receives a fresh copy of the fixture, ensuring tests are independent of each other.

### Defining a fixture

A fixture is a `struct` (or class) that holds the object(s) under test and initialises them in its constructor:

```
+-----------------------+
|  struct example       |
|  {                    |
|    Mortgage m;        |       <-- shared object
|    example() :        |
|      m(18,165000,2.49)|       <-- initialised once per test
|    {}                 |
|  };                   |
+-----------------------+
```

### Creating a suite with a fixture

Use `BOOST_FIXTURE_TEST_SUITE` to open a suite that is bound to the fixture, and `BOOST_AUTO_TEST_SUITE_END` to close it. Inside the suite, individual test cases refer to the fixture's members directly by name:

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <cmath>

class Mortgage {
private:
    int term; double amount; double rate;
public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }

    double getTotalInterest() const {
        return getPayment() * term * 12 - amount;
    }

    double owedAtYear(int y) const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        int paid = y * 12;
        double balance = amount * std::pow(1 + r, paid)
                       - getPayment() * (std::pow(1 + r, paid) - 1) / r;
        return balance < 0 ? 0 : balance;
    }
};

struct example {
    Mortgage m;
    example() : m(18, 165000, 2.49) {}
};

BOOST_FIXTURE_TEST_SUITE(mortgage_suite, example)

BOOST_AUTO_TEST_CASE(payment) {
    BOOST_CHECK_EQUAL(std::round(m.getPayment()), 949);
}

BOOST_AUTO_TEST_CASE(owed_at_ten_years) {
    BOOST_CHECK_EQUAL(std::round(m.owedAtYear(10)), 82491);
}

BOOST_AUTO_TEST_CASE(total_interest) {
    BOOST_CHECK_EQUAL(std::round(m.getTotalInterest()), 39921);
}

BOOST_AUTO_TEST_SUITE_END()
```

Sample output:

```
Running 3 test cases...

tests.cpp(42): error: in "mortgage_suite/total_interest": 
  check std::round(m.getTotalInterest()) == 39921 has failed 
  [40006 != 39921]

*** 1 failure is detected in the test module "master_test_suite"
```

This brings us naturally to the next section — some of these failures are expected because the formula used is an approximation of real-world bank calculations.

---

[Back to Table of Contents](#table-of-contents)

---

## Section 8: Handling Floating-Point Comparisons and Tolerances

### Why tests can fail for the right reasons

The failure in the fixture example above is not necessarily a bug. Mortgage calculations involve a degree of approximation: banks may use slightly different compounding conventions, rounding rules, or fee structures. Our formula is mathematically sound but produces results that may differ slightly from tabulated values.

Similarly, floating-point arithmetic in C++ is inherently imprecise. Repeated multiplication and division accumulates small rounding errors that can cause an exact equality check to fail even when the result is correct to many decimal places.

### Applying a tolerance with BOOST_TEST

The `BOOST_TEST` macro supports a tolerance modifier using the `%` operator. The value specifies the acceptable relative error as a percentage:

```cpp
BOOST_TEST(computed_value == expected_value, boost::test_tools::tolerance(1.0));
```

This passes if `computed_value` is within 1% of `expected_value`.

Full documentation on floating-point comparison tools is at:
https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/boost_test/testing_tools/extended_comparison/floating_point.html

### Example: fixture with tolerance

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <cmath>

class Mortgage {
private:
    int term; double amount; double rate;
public:
    Mortgage(int term, double amount, double rate)
        : term(term), amount(amount), rate(rate) {}

    double getPayment() const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        return amount * r * std::pow(1 + r, n) / (std::pow(1 + r, n) - 1);
    }

    double getTotalInterest() const {
        return getPayment() * term * 12 - amount;
    }

    double owedAtYear(int y) const {
        double r = rate / 100.0 / 12.0;
        int n = term * 12;
        int paid = y * 12;
        double balance = amount * std::pow(1 + r, paid)
                       - getPayment() * (std::pow(1 + r, paid) - 1) / r;
        return balance < 0 ? 0 : balance;
    }
};

struct example {
    Mortgage m;
    example() : m(18, 165000, 2.49) {}
};

BOOST_FIXTURE_TEST_SUITE(mortgage_suite_tolerant, example)

BOOST_AUTO_TEST_CASE(payment_approx) {
    // Pass if within 1% of expected value
    BOOST_TEST(m.getPayment() == 949.0, boost::test_tools::tolerance(1.0));
}

BOOST_AUTO_TEST_CASE(owed_at_ten_years_approx) {
    BOOST_TEST(m.owedAtYear(10) == 82491.0, boost::test_tools::tolerance(1.0));
}

BOOST_AUTO_TEST_CASE(total_interest_approx) {
    BOOST_TEST(m.getTotalInterest() == 39921.0, boost::test_tools::tolerance(1.0));
}

BOOST_AUTO_TEST_SUITE_END()
```

Sample output:

```
Running 3 test cases...

*** No errors detected
```

### When to use tolerance — and when not to

Tolerance is appropriate when:

- Testing against values from an external reference (such as an online calculator) where minor rounding differences are expected
- Working with algorithms that accumulate floating-point error over many steps
- Comparing results from different implementations of the same formula

Tolerance should be used with care when:

- Exact integer arithmetic is expected (use `BOOST_CHECK_EQUAL` for these cases)
- The tolerance is large enough to mask genuine bugs

A common approach is to use exact integer checks where possible (by rounding intermediate results, as in the early examples) and reserve tolerance-based assertions for cases where rounding is genuinely inappropriate.

---

[Back to Table of Contents](#table-of-contents)

---

## Resources

### Official Documentation

- Boost.Test main documentation: https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/index.html
- Boost.Test design rationale: https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/boost_test/intro/design_rationale.html
- Floating-point comparison tools: https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/boost_test/testing_tools/extended_comparison/floating_point.html
- Boost.Test data-driven testing: https://www.boost.org/doc/libs/1_80_0/libs/test/doc/html/boost_test/tests_organization/test_cases/test_case_generation.html

### C++ Reference

- `std::uniform_real_distribution` (used in random testing): https://en.cppreference.com/w/cpp/numeric/random/uniform_real_distribution
- `std::round`: https://en.cppreference.com/w/cpp/numeric/math/round

### Further Reading and Practice

- LearnCpp — general C++ reference and tutorials: https://www.learncpp.com
- cppreference — authoritative C++ standard library reference: https://en.cppreference.com
- HackerRank C++ practice: https://www.hackerrank.com/domains/cpp
- LeetCode C++ problems: https://leetcode.com/problemset/all/
- Google Test (an alternative unit testing framework, useful for comparison): https://google.github.io/googletest/

---

[Back to Table of Contents](#table-of-contents)
