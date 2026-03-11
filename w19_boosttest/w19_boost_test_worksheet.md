# Worksheet: Unit Testing with Boost.Test

---

## Quiz

### Part A: Fill in the Blanks

For each question, choose the correct word or phrase from the scrambled word bank and fill in the blanks.

---

**Question 1**

Complete the following paragraph about unit testing in C++.

Unit testing is the practice of testing individual __________ of a program in __________
to verify that they behave as expected. Unlike languages such as Python and Java, C++ does
not include a testing framework in its __________ library. As a result, C++ developers rely
on third-party frameworks such as Boost.Test, Google Test, and __________. Running the test
suite after every change allows developers to detect __________ — breakage introduced by
a new edit that was previously working.

**Word bank (scrambled):** `Catch2` / `isolation` / `regressions` / `standard` / `components`

---

**Question 2**

Complete the following paragraph about the three distribution variants of Boost.Test.

Boost.Test can be used in three forms. The __________ variant compiles all Boost.Test logic
directly into your project from headers alone and requires no separate __________
step. The __________ library variant compiles Boost.Test once and links it as a `.lib` file,
which reduces compile time. The __________ library variant is similar, but the compiled
library can be used across multiple projects at __________. This course uses the first
variant because it is the most __________.

**Word bank (scrambled):** `runtime` / `build` / `static` / `header-only` / `shared` / `portable`

---

**Question 3**

Complete the following paragraph about the `BOOST_CHECK` and `BOOST_REQUIRE` macros.

Both `BOOST_CHECK` and `BOOST_REQUIRE` record a failure when an assertion does not hold.
The difference is in what happens __________. `BOOST_CHECK` records the failure but
__________ executing the rest of the test case. `BOOST_REQUIRE` records the failure and
immediately __________ the current test case. `BOOST_REQUIRE` should be used when
subsequent assertions would be __________ if the first one fails — for example, when
checking a pointer is not __________ before dereferencing it.

**Word bank (scrambled):** `aborts` / `afterwards` / `null` / `continues` / `meaningless`

---

**Question 4**

Complete the following paragraph about `BOOST_DATA_TEST_CASE` and random testing.

`BOOST_DATA_TEST_CASE` accepts a __________ and runs the test body once for each element.
When pairing related values from two datasets — such as a term and its expected payment — the
`__________` operator is used to zip the datasets together. The result is a dataset of
__________, where element `i` from one dataset is paired with element `i` from the other.
To generate a controlled number of random test cases, `bdata::__________` produces a
sequence from 0 to n-1 and also provides an __________ variable for identifying individual
runs in error messages.

**Word bank (scrambled):** `^` / `tuples` / `index` / `dataset` / `xrange`

---

### Part B: Multiple Choice

Circle or mark the letter of the best answer.

---

**Question 5**

When setting up Boost.Test in Visual Studio, why is a separate build configuration (such as
"Debug Test") needed?

```
A) Because Boost.Test requires a different compiler version than the main project.

B) Because Boost.Test generates its own main() function, which conflicts with the
   production main() if both are compiled together.

C) Because the NuGet package for Boost cannot be installed in the Debug configuration.

D) Because Boost.Test only works when all source files are excluded from the build.
```

---

**Question 6**

Consider the following test code:

```cpp
BOOST_DATA_TEST_CASE(
    payment_test,
    bdata::make({10, 15, 20}) ^ bdata::make({1200, 900, 750}),
    term, expected
) {
    Loan L(term, 100000, 3.0);
    BOOST_TEST(std::round(L.getPayment()) == expected);
}
```

How many individual test cases does this produce, and what values does `term` take across
those runs?

```
A) 1 test case; term takes the value {10, 15, 20} as a single set.

B) 3 test cases; term takes the values 10, 15, and 20 in successive runs.

C) 6 test cases; term takes each value paired with every value in the second dataset.

D) 3 test cases; term always takes the first value (10) because zip stops at the shortest
   dataset.
```

---

**Question 7**

The diagram below shows how four datasets are combined for a random test. Which statement
correctly describes the role of `bdata::xrange(500)` in this expression?

```
+-----------------------------+----------+
|  bdata::random(1, 30)       | term     |
+-----------------------------+----------+
|  bdata::random(50000,300000)| amount   |   } zipped with ^
+-----------------------------+----------+
|  uniform_real(1.0, 10.0)    | rate     |
+-----------------------------+----------+
|  bdata::xrange(500)         | index    |
+-----------------------------+----------+
              |
              v
       500 test cases generated
```

```
A) It limits the random ranges so that no value outside 0-499 is produced for term,
   amount, or rate.

B) It controls the total number of test cases generated and provides an index variable
   that can be used to identify individual runs in error output.

C) It replaces the other three datasets and generates 500 identical test cases.

D) It seeds the random number generator so that the same 500 values are produced on
   every run.
```

---

## Tasks

### Task 1: Writing a Basic Test Case for a Temperature Converter

A `TempConverter` class converts temperatures between Celsius, Fahrenheit, and Kelvin.
Its interface is:

- `TempConverter(double celsius)` — constructor taking a temperature in Celsius
- `toFahrenheit()` — returns the equivalent temperature in Fahrenheit
- `toKelvin()` — returns the equivalent temperature in Kelvin

The conversion formulas are:

```
Fahrenheit = (Celsius * 9.0 / 5.0) + 32.0
Kelvin     = Celsius + 273.15
```

**Your task:** Complete the test file below by filling in the two `BOOST_AUTO_TEST_CASE`
bodies. Use `BOOST_CHECK_EQUAL` with `std::round` to verify the results against the
expected values shown in the sample output.

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <cmath>

class TempConverter {
    double celsius;
public:
    TempConverter(double c) : celsius(c) {}

    double toFahrenheit() const {
        // TODO: implement
    }

    double toKelvin() const {
        // TODO: implement
    }
};

BOOST_AUTO_TEST_CASE(fahrenheit_check) {
    TempConverter t(100.0);
    // Check that 100 Celsius converts to 212 Fahrenheit
    // TODO: write assertion
}

BOOST_AUTO_TEST_CASE(kelvin_check) {
    TempConverter t(0.0);
    // Check that 0 Celsius converts to 273 Kelvin (rounded)
    // TODO: write assertion
}
```

**Expected output:**

```
Running 2 test cases...

*** No errors detected
```

---

### Task 2: Parameterised Test Using BOOST_DATA_TEST_CASE

A `Discount` class calculates the sale price of an item after applying a percentage
discount. Its interface is:

- `Discount(double originalPrice, double discountPercent)` — constructor
- `salePrice()` — returns the price after the discount is applied

```
sale_price = original_price * (1.0 - discount_percent / 100.0)
```

**Your task:** Complete the parameterised test below. The datasets provide original prices,
discount percentages, and expected sale prices (rounded to the nearest pound). Use the zip
operator `^` to combine them, and `BOOST_TEST` with `std::round` to assert the result.

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <boost/test/data/test_case.hpp>
#include <cmath>

namespace bdata = boost::unit_test::data;

class Discount {
    double price;
    double pct;
public:
    Discount(double p, double d) : price(p), pct(d) {}

    double salePrice() const {
        // TODO: implement
    }
};

// Original prices:   200,  150,  80,  500
// Discount percents: 10,   25,   50,  20
// Expected results:  180,  113,  40,  400

BOOST_DATA_TEST_CASE(
    discount_test,
    // TODO: build the zipped dataset with three bdata::make() calls
    origPrice, discPct, expected
) {
    Discount d(origPrice, discPct);
    // TODO: write assertion
}
```

**Expected output:**

```
Running 4 test cases...

*** No errors detected
```

---

### Challenge Task: Fixture, Suite, and Floating-Point Tolerance

A `SavingsAccount` class models a compound interest savings account. Its interface is:

- `SavingsAccount(double principal, double annualRatePercent, int years)` — constructor
- `balance()` — returns the final balance after compound interest is applied annually
- `interestEarned()` — returns the total interest earned (balance minus principal)
- `doubleTime()` — returns the number of whole years until the balance is at least double
  the principal (use a loop)

The compound interest formula is:

```
balance = principal * (1 + rate/100)^years
```

**Your task:**

1. Implement the three methods of `SavingsAccount`.
2. Define a fixture `AccountFixture` that holds a `SavingsAccount` with principal 5000,
   annual rate 4.5%, and term 10 years.
3. Write a `BOOST_FIXTURE_TEST_SUITE` containing three test cases:
   - `balance_check`: verify the balance is approximately 7763 using a 1% tolerance
   - `interest_check`: verify interest earned is approximately 2763 using a 1% tolerance
   - `double_time_check`: verify the account doubles in 16 years (exact integer check)

Use `BOOST_TEST` with `boost::test_tools::tolerance(1.0)` for the floating-point checks,
and `BOOST_CHECK_EQUAL` for the integer check.

```cpp
#define BOOST_TEST_MAIN
#include <boost/test/included/unit_test.hpp>
#include <cmath>

class SavingsAccount {
    double principal;
    double rate;
    int years;
public:
    SavingsAccount(double p, double r, int y)
        : principal(p), rate(r), years(y) {}

    double balance() const {
        // TODO: implement
    }

    double interestEarned() const {
        // TODO: implement
    }

    int doubleTime() const {
        // TODO: implement using a loop
    }
};

// TODO: define fixture struct AccountFixture

// TODO: open BOOST_FIXTURE_TEST_SUITE

// TODO: write balance_check test case

// TODO: write interest_check test case

// TODO: write double_time_check test case

// TODO: close suite with BOOST_AUTO_TEST_SUITE_END()
```

**Expected output:**

```
Running 3 test cases...

*** No errors detected
```
