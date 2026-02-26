# C++ Code Organization Worksheet

## Quiz Section

### Question 1

Complete the following statement by filling in the blanks with the appropriate terms from the word bank below.

"The __________ directive prevents multiple inclusion of the same header file. When including standard library headers, we use __________ brackets, but when including project headers, we use __________ marks. This helps the compiler distinguish between system libraries and __________."

**Word Bank:** angle, project files, pragma once, quotation

---

### Question 2

Fill in the blanks using words from the scrambled word bank.

"Template code must have both declaration and implementation in the __________ file because the compiler needs to see the full __________ when instantiating a template. By convention, template code goes in __________ files rather than separate .h and .cpp files. The reason templates cannot be __________ in advance is that their compilation depends on template __________."

**Word Bank:** same, compiled, parameters, implementation, .hpp

---

### Question 3

Complete the following paragraph about namespaces using the word bank provided.

"Namespaces help prevent __________ conflicts when integrating multiple libraries. To use code from a namespace, you can either use fully __________ names with the scope resolution operator, or you can add a using __________ to bring specific identifiers into scope. However, you should avoid using namespace directives in __________ files because this pollutes the global namespace for all __________."

**Word Bank:** qualified, naming, header, declaration, includers

---

### Question 4

Fill in the blanks about thread synchronization using the word bank.

"When multiple threads access __________ data, we need synchronization to prevent race __________. The std::__________ class provides mutual exclusion, and std::lock_guard implements the __________ pattern to automatically lock and unlock. The critical section should be kept as __________ as possible to minimize contention."

**Word Bank:** shared, RAII, mutex, conditions, small

---

### Question 5

Which of the following is the PRIMARY reason for separating class declarations and implementations into .h and .cpp files?

A) To make the code look more professional  
B) To allow compilation of implementation once into object files, recompiling only when implementation changes  
C) To satisfy C++ compiler requirements  
D) To ensure the code runs faster  

---

### Question 6

Which Doxygen tag would you use to document what a function returns?

A) @result  
B) @output  
C) @return  
D) @gives  

---

### Question 7

What happens if you create a std::thread object but forget to call either join() or detach() before it goes out of scope?

A) The thread continues running in the background  
B) The program calls std::terminate() and aborts  
C) The thread is automatically joined  
D) A warning is issued but the program continues normally  

---

## Programming Tasks

### Task 1: Header and Implementation File Separation

Create a `Temperature` class that stores a temperature value and can convert between Celsius and Fahrenheit. Split the class into separate header and implementation files.

**Requirements:**
- Create `Temperature.h` with class declaration
- Create `Temperature.cpp` with method implementations
- Include proper header guards
- Implement the following methods:
  - Constructor that takes Celsius value
  - `toFahrenheit()` - returns temperature in Fahrenheit
  - `toCelsius()` - returns temperature in Celsius
  - `operator<<` for output in format: "XX.X C (XX.X F)"

**Scaffolding for Temperature.h:**

```cpp
#pragma once
#include <iostream>

class Temperature
{
    double celsius;
public:
    // TODO: Add constructor
    
    // TODO: Add toFahrenheit method
    
    // TODO: Add toCelsius method
    
    // TODO: Add friend operator<< declaration
};
```

**Scaffolding for Temperature.cpp:**

```cpp
#include "Temperature.h"

// TODO: Implement constructor

// TODO: Implement toFahrenheit
// Formula: F = C * 9/5 + 32

// TODO: Implement toCelsius

// TODO: Implement operator<<
```

**Sample main.cpp:**

```cpp
#include <iostream>
#include "Temperature.h"

int main()
{
    Temperature t1(0);
    Temperature t2(100);
    Temperature t3(37.5);
    
    std::cout << t1 << std::endl;
    std::cout << t2 << std::endl;
    std::cout << t3 << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
0.0 C (32.0 F)
100.0 C (212.0 F)
37.5 C (99.5 F)
```

---

### Task 2: Template Class with Documentation

Create a documented template class `Pair` that stores two values of potentially different types. The class should be in a single .hpp file with Doxygen comments.

**Requirements:**
- Create `Pair.hpp` with template class
- Use proper Doxygen documentation tags
- Implement methods to get first and second values
- Implement `swap()` method to exchange the values
- Overload `operator<<` to output in format: "(first, second)"
- Place everything in a namespace called `DataStructures`

**Scaffolding for Pair.hpp:**

```cpp
#pragma once
#include <iostream>

namespace DataStructures {

/**
 * @brief TODO: Add brief description
 * 
 * TODO: Add detailed description
 * 
 * @tparam T1 TODO: Document first type parameter
 * @tparam T2 TODO: Document second type parameter
 */
template <typename T1, typename T2>
class Pair
{
    T1 first;
    T2 second;

public:
    /**
     * @brief TODO: Document constructor
     * @param TODO: Add parameter documentation
     */
    Pair(T1 f, T2 s) : first(f), second(s) {}
    
    // TODO: Add getFirst() method with documentation
    
    // TODO: Add getSecond() method with documentation
    
    // TODO: Add swap() method with documentation
    
    // TODO: Add operator<< friend function with documentation
};

// TODO: Implement methods here

} // namespace DataStructures
```

**Sample main.cpp:**

```cpp
#include <iostream>
#include <string>
#include "Pair.hpp"

using namespace DataStructures;

int main()
{
    Pair<int, std::string> p1(42, "Answer");
    std::cout << "Before swap: " << p1 << std::endl;
    
    p1.swap();
    std::cout << "After swap: " << p1 << std::endl;
    
    Pair<double, char> p2(3.14, 'A');
    std::cout << p2 << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Before swap: (42, Answer)
After swap: (Answer, 42)
(3.14, A)
```

---

### Task 3: Challenge - Fraction Class with Multiple Files and Namespace

Create a complete `Fraction` class that represents mathematical fractions with proper file organization, namespace, and operator overloading.

**Requirements:**
- Create `Fraction.h` and `Fraction.cpp`
- Use a namespace called `MathUtils`
- Implement proper header guards
- Support construction from numerator and denominator
- Implement `operator+`, `operator*`, and `operator<` 
- Implement simplification (reduce to lowest terms)
- Overload `operator<<` for output in format: "num/denom"

**Scaffolding for Fraction.h:**

```cpp
#pragma once
#include <iostream>

namespace MathUtils {

class Fraction
{
    int numerator;
    int denominator;
    
    // Helper function to find GCD
    int gcd(int a, int b) const;
    
    // Helper function to simplify the fraction
    void simplify();
    
public:
    Fraction(int num = 0, int denom = 1);
    
    // TODO: Declare operator+
    // TODO: Declare operator*
    // TODO: Declare operator<
    
    friend std::ostream& operator<<(std::ostream& os, const Fraction& f);
};

} // namespace MathUtils
```

**Scaffolding for Fraction.cpp:**

```cpp
#include "Fraction.h"
#include <stdexcept>

namespace MathUtils {

// TODO: Implement constructor
// Remember to check for zero denominator and call simplify()

// TODO: Implement gcd using Euclidean algorithm
int Fraction::gcd(int a, int b) const
{
    // Hint: gcd(a, 0) = a
    // Hint: gcd(a, b) = gcd(b, a % b)
}

// TODO: Implement simplify
// Divide both numerator and denominator by their GCD

// TODO: Implement operator+
// Formula: a/b + c/d = (ad + bc)/(bd), then simplify

// TODO: Implement operator*
// Formula: a/b * c/d = (ac)/(bd), then simplify

// TODO: Implement operator<
// Compare: a/b < c/d if ad < bc

// TODO: Implement operator<<

} // namespace MathUtils
```

**Sample main.cpp:**

```cpp
#include <iostream>
#include "Fraction.h"

using namespace MathUtils;

int main()
{
    Fraction f1(1, 2);
    Fraction f2(1, 3);
    Fraction f3(2, 4);  // Should simplify to 1/2
    
    std::cout << f1 << " + " << f2 << " = " << (f1 + f2) << std::endl;
    std::cout << f1 << " * " << f2 << " = " << (f1 * f2) << std::endl;
    std::cout << f1 << " < " << f2 << " ? " << (f1 < f2 ? "true" : "false") << std::endl;
    std::cout << f1 << " < " << f3 << " ? " << (f1 < f3 ? "true" : "false") << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
1/2 + 1/3 = 5/6
1/2 * 1/3 = 1/6
1/2 < 1/3 ? false
1/2 < 1/2 ? false
```

---

## Standard Thread Library Tasks

### Threading Task 1: Parallel Sum Calculation

Write a program that calculates the sum of an array using multiple threads. Divide the array into sections and have each thread sum its section, then combine the results.

**Requirements:**
- Create an array of 1000 integers (fill with values 1-1000)
- Use 4 threads to sum different quarters of the array
- Use a mutex to safely add partial sums to a total
- Print each thread's partial sum and the final total

**Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <mutex>

std::mutex sumMutex;
int totalSum = 0;

void calculatePartialSum(const std::vector<int>& data, int start, int end, int threadId)
{
    int partialSum = 0;
    
    // TODO: Calculate sum from data[start] to data[end-1]
    
    // TODO: Lock mutex and add partialSum to totalSum
    
    // TODO: Print thread ID and partial sum
}

int main()
{
    // TODO: Create vector with numbers 1-1000
    
    // TODO: Create 4 threads, each processing 250 elements
    
    // TODO: Join all threads
    
    // TODO: Print total sum (should be 500500)
    
    return 0;
}
```

**Expected Output:**
```
Thread 1 partial sum: 31375
Thread 2 partial sum: 93875
Thread 3 partial sum: 156375
Thread 4 partial sum: 218875
Total sum: 500500
```

---

### Threading Task 2: Thread-Safe Counter

Create a Counter class that can be safely incremented from multiple threads. Demonstrate its use with multiple threads incrementing the counter concurrently.

**Requirements:**
- Create a Counter class with increment() and getValue() methods
- Use std::mutex for thread safety
- Create 5 threads, each incrementing the counter 1000 times
- Print the final counter value (should be 5000)

**Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <vector>

class Counter
{
    int count;
    std::mutex mtx;
    
public:
    Counter() : count(0) {}
    
    void increment()
    {
        // TODO: Lock mutex and increment count
    }
    
    int getValue()
    {
        // TODO: Lock mutex and return count
    }
};

void incrementCounter(Counter& counter, int iterations, int threadId)
{
    // TODO: Call counter.increment() iterations times
    
    // TODO: Print thread completion message
}

int main()
{
    // TODO: Create Counter object
    
    // TODO: Create 5 threads, each calling incrementCounter with 1000 iterations
    
    // TODO: Join all threads
    
    // TODO: Print final counter value
    
    return 0;
}
```

**Expected Output:**
```
Thread 1 completed 1000 increments
Thread 2 completed 1000 increments
Thread 3 completed 1000 increments
Thread 4 completed 1000 increments
Thread 5 completed 1000 increments
Final counter value: 5000
```

---

### Threading Task 3: Parallel File Processing

Create a program that processes multiple text files concurrently, counting the number of lines in each file. Use one thread per file.

**Requirements:**
- Create 3 text files with different numbers of lines
- Use separate threads to count lines in each file
- Store results in a thread-safe container
- Print results for each file

**Scaffolding:**

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <fstream>
#include <mutex>
#include <map>

std::mutex resultsMutex;
std::map<std::string, int> lineCounts;

void countLines(const std::string& filename)
{
    // TODO: Open file and count lines
    
    // TODO: Lock mutex and store result in lineCounts map
    
    // TODO: Print filename and line count
}

int main()
{
    // TODO: Create 3 test files with different content
    // file1.txt - 10 lines
    // file2.txt - 15 lines
    // file3.txt - 20 lines
    
    std::vector<std::string> files = {"file1.txt", "file2.txt", "file3.txt"};
    std::vector<std::thread> threads;
    
    // TODO: Create thread for each file
    
    // TODO: Join all threads
    
    // TODO: Print total lines across all files
    
    return 0;
}
```

**Expected Output:**
```
file1.txt: 10 lines
file2.txt: 15 lines
file3.txt: 20 lines
Total lines: 45
```

---

## End of Worksheet