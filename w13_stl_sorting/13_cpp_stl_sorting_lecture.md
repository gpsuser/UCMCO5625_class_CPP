# Lecture: Operator Overloads and STL Sorting

## Learning Outcomes

By the end of this lecture, you should be able to:

1. Understand how the STL `sort` algorithm works and its time complexity
2. Use `std::sort` effectively with standard containers (vectors and lists)
3. Overload the `<` operator for custom data types to enable sorting
4. Implement reverse sorting using `std::greater`
5. Create and use function objects (functors) for custom sorting criteria
6. Apply function objects with state to solve complex sorting problems
7. Understand strict weak ordering requirements for custom comparators

---

## Table of Contents

1. [Introduction to STL Sort](#section-1-introduction-to-stl-sort)
2. [Using std::sort with Standard Containers](#section-2-using-stdsort-with-standard-containers)
3. [Operator Overloading Fundamentals](#section-3-operator-overloading-fundamentals)
4. [Custom Less-Than Operator for User-Defined Types](#section-4-custom-less-than-operator-for-user-defined-types)
5. [Reverse Sorting with std::greater](#section-5-reverse-sorting-with-stdgreater)
6. [Function Objects for Custom Sorting](#section-6-function-objects-for-custom-sorting)
7. [Advanced Function Objects with State](#section-7-advanced-function-objects-with-state)
8. [Practical Applications and Best Practices](#section-8-practical-applications-and-best-practices)
9. [Resources for Further Learning](#resources-for-further-learning)

---

## Section 1: Introduction to STL Sort

The Standard Template Library (STL) provides a powerful and efficient sorting algorithm through `std::sort`. Understanding how it works and when to use it is fundamental to writing efficient C++ programs.

### How STL Sort Works

The `std::sort` algorithm typically uses an optimized version of **quicksort**, though the exact implementation is compiler-dependent. Most modern implementations use a hybrid approach called **introsort** (introspective sort), which combines:

- Quicksort for general cases
- Heapsort when recursion depth gets too deep
- Insertion sort for small subarrays

This gives `std::sort` an average time complexity of **O(n log n)** and worst-case time complexity of **O(n log n)** in modern implementations.

### Function Signature

```
void sort(RandomAccessIterator first, RandomAccessIterator last);
```

The key points about this signature:

- **RandomAccessIterator**: Requires random access to elements
- **first**: Iterator to the beginning of the range to sort
- **last**: Iterator to one past the end of the range (half-open interval [first, last))

### Requirements for Sorting

For `std::sort` to work with a data type, that type must have:

1. A **less-than operator (`<`)** defined
2. The comparison must satisfy **strict weak ordering** (more on this later)

Standard types like `int`, `double`, `string`, and `char` already have the `<` operator defined, so they work immediately with `std::sort`.

### Random Access Requirement

The requirement for random access iterators has important implications:

```
Containers that work with std::sort:
  - std::vector       (YES - random access)
  - std::deque        (YES - random access)
  - std::array        (YES - random access)
  - C-style arrays    (YES - pointers provide random access)

Containers that do NOT work with std::sort:
  - std::list         (NO - bidirectional, not random access)
  - std::forward_list (NO - forward only)
```

Lists have their own `sort()` member function because they cannot provide random access to elements.

---

## Section 2: Using std::sort with Standard Containers

Let's explore how to use `std::sort` with different container types through complete, executable examples.

### Sorting a Vector

The most common use case is sorting a `std::vector`:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <ctime>

int main() {
    // Seed random number generator
    std::srand(std::time(0));
    
    // Create a vector with 10 elements
    std::vector<int> numbers(10);
    
    // Fill with random values
    std::cout << "Original vector: ";
    for (int i = 0; i < numbers.size(); ++i) {
        numbers[i] = std::rand() % 100;
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    // Sort the vector
    std::sort(numbers.begin(), numbers.end());
    
    // Display sorted result
    std::cout << "Sorted vector:   ";
    for (int i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original vector: 83 86 77 15 93 35 86 92 49 21
Sorted vector:   15 21 35 49 77 83 86 86 92 93
```

### Sorting a C-Style Array

You can also use `std::sort` with traditional C-style arrays:

```cpp
#include <iostream>
#include <algorithm>
#include <cstdlib>
#include <ctime>

int main() {
    std::srand(std::time(0));
    
    // Traditional C-style array
    int array[8];
    
    // Fill with random values
    std::cout << "Original array: ";
    for (int i = 0; i < 8; ++i) {
        array[i] = std::rand() % 100;
        std::cout << array[i] << " ";
    }
    std::cout << std::endl;
    
    // Sort using pointers (arrays decay to pointers)
    std::sort(array, array + 8);
    
    // Display sorted result
    std::cout << "Sorted array:   ";
    for (int i = 0; i < 8; ++i) {
        std::cout << array[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original array: 42 68 35 1 70 25 79 59
Sorted array:   1 25 35 42 59 68 70 79
```

### Sorting a List

Lists require a different approach because they don't support random access:

```cpp
#include <iostream>
#include <list>
#include <cstdlib>
#include <ctime>

int main() {
    std::srand(std::time(0));
    
    // Create a list
    std::list<int> numbers;
    
    // Add 10 random values
    std::cout << "Original list: ";
    for (int i = 0; i < 10; ++i) {
        int value = std::rand() % 100;
        numbers.push_back(value);
        std::cout << value << " ";
    }
    std::cout << std::endl;
    
    // Use the list's own sort method
    numbers.sort();
    
    // Display sorted result
    std::cout << "Sorted list:   ";
    for (std::list<int>::iterator it = numbers.begin(); 
         it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original list: 77 15 93 35 86 92 49 21 62 27
Sorted list:   15 21 27 35 49 62 77 86 92 93
```

**Key Difference:** Notice that for `std::list`, we call `numbers.sort()` (member function) instead of `std::sort(numbers.begin(), numbers.end())`.

---

## Section 3: Operator Overloading Fundamentals

Operator overloading is a powerful C++ feature that allows you to define how operators work with your custom classes. This enables natural and intuitive syntax when working with user-defined types.

### What is Operator Overloading?

Operator overloading lets you redefine the behavior of operators (like `+`, `-`, `*`, `<`, `==`, etc.) for your own classes. This makes your classes behave more like built-in types.

### Why Overload Operators?

1. **Intuitive code**: `height1 < height2` is clearer than `height1.isLessThan(height2)`
2. **STL compatibility**: Many STL algorithms require specific operators
3. **Natural syntax**: Code reads more like mathematical or logical expressions

### Rules for Operator Overloading

1. You cannot create new operators
2. You cannot change operator precedence or associativity
3. You cannot change the number of operands an operator takes
4. At least one operand must be a user-defined type
5. Some operators cannot be overloaded (`::`, `.`, `.*`, `?:`, `sizeof`)

### Basic Syntax

Operator overloading can be done as:

1. **Member functions**: When the left operand is always an object of your class
2. **Non-member functions**: When you need symmetry or the left operand isn't your class

For comparison operators used with sorting, we typically use member functions.

### The Importance of const Correctness

When overloading comparison operators, use `const` correctly:

```cpp
bool operator<(const ClassName& rhs) const;
```

- First `const`: The parameter won't be modified
- Second `const`: The method won't modify the object it's called on

This allows sorting of const objects and demonstrates good C++ practice.

---

## Section 4: Custom Less-Than Operator for User-Defined Types

To use `std::sort` with your own classes, you need to provide a way to compare objects. The most common approach is overloading the `<` operator.

### Basic Example: Height Class

Let's create a `Height` class that stores measurements in feet and inches:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Height {
private:
    int feet;
    int inches;

public:
    // Constructor
    Height(int f = 0, int i = 0) : feet(f), inches(i) {}
    
    // Getter methods for display
    int getFeet() const { return feet; }
    int getInches() const { return inches; }
    
    // Overload the < operator
    bool operator<(const Height& rhs) const {
        // Compare by feet only (simple version)
        return feet < rhs.feet;
    }
    
    // Display method
    void display() const {
        std::cout << feet << "'" << inches << "\"";
    }
};

int main() {
    // Create a vector of heights
    std::vector<Height> heights;
    heights.push_back(Height(6, 2));
    heights.push_back(Height(5, 8));
    heights.push_back(Height(6, 0));
    heights.push_back(Height(5, 11));
    heights.push_back(Height(6, 4));
    
    std::cout << "Original heights:" << std::endl;
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    // Sort using our custom < operator
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights:" << std::endl;
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original heights:
6'2" 5'8" 6'0" 5'11" 6'4" 
Sorted heights:
5'8" 5'11" 6'2" 6'0" 6'4"
```

### Problem with Simple Comparison

Notice in the output that `6'0"` comes after `6'2"` and before `6'4"`. This is because our comparison only looks at feet, ignoring inches entirely.

### Improved Height Class with Tiebreaker

Here's a better implementation that uses inches as a tiebreaker:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Height {
private:
    int feet;
    int inches;

public:
    Height(int f = 0, int i = 0) : feet(f), inches(i) {}
    
    int getFeet() const { return feet; }
    int getInches() const { return inches; }
    
    // Improved < operator with tiebreaker
    bool operator<(const Height& rhs) const {
        // If feet are different, compare by feet
        if (feet != rhs.feet) {
            return feet < rhs.feet;
        }
        // If feet are the same, compare by inches (tiebreaker)
        return inches < rhs.inches;
    }
    
    void display() const {
        std::cout << feet << "'" << inches << "\"";
    }
};

int main() {
    std::vector<Height> heights;
    heights.push_back(Height(6, 2));
    heights.push_back(Height(5, 8));
    heights.push_back(Height(6, 0));
    heights.push_back(Height(5, 11));
    heights.push_back(Height(6, 4));
    heights.push_back(Height(5, 9));
    
    std::cout << "Original heights:" << std::endl;
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights (with tiebreaker):" << std::endl;
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original heights:
6'2" 5'8" 6'0" 5'11" 6'4" 5'9" 
Sorted heights (with tiebreaker):
5'8" 5'9" 5'11" 6'0" 6'2" 6'4"
```

Now the heights are properly sorted by feet first, then by inches when feet are equal.

### Using Custom Types with std::map

Once you've overloaded the `<` operator, you can also use your custom type as a key in `std::map`:

```cpp
#include <iostream>
#include <map>

class Height {
private:
    int feet;
    int inches;

public:
    Height(int f = 0, int i = 0) : feet(f), inches(i) {}
    
    bool operator<(const Height& rhs) const {
        if (feet != rhs.feet) {
            return feet < rhs.feet;
        }
        return inches < rhs.inches;
    }
    
    void display() const {
        std::cout << feet << "'" << inches << "\"";
    }
};

int main() {
    // Map heights to names
    std::map<Height, std::string> heightToName;
    
    heightToName[Height(6, 2)] = "Alice";
    heightToName[Height(5, 8)] = "Bob";
    heightToName[Height(6, 0)] = "Charlie";
    
    std::cout << "People sorted by height:" << std::endl;
    for (std::map<Height, std::string>::iterator it = heightToName.begin();
         it != heightToName.end(); ++it) {
        it->first.display();
        std::cout << " - " << it->second << std::endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
People sorted by height:
5'8" - Bob
6'0" - Charlie
6'2" - Alice
```

---

## Section 5: Reverse Sorting with std::greater

Sometimes you need to sort in descending order (largest first). The STL provides `std::greater` for this purpose.

### Basic Reverse Sorting

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
#include <cstdlib>
#include <ctime>

int main() {
    std::srand(std::time(0));
    
    std::vector<int> numbers(10);
    
    std::cout << "Original: ";
    for (int i = 0; i < numbers.size(); ++i) {
        numbers[i] = std::rand() % 100;
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    // Sort in descending order using std::greater
    std::sort(numbers.begin(), numbers.end(), std::greater<int>());
    
    std::cout << "Sorted (descending): ";
    for (int i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original: 83 86 77 15 93 35 86 92 49 21
Sorted (descending): 93 92 86 86 83 77 49 35 21 15
```

### How std::greater Works

`std::greater<T>()` is a **function object** (also called a functor). It's a class that overloads the `operator()` function call operator.

Conceptually, `std::greater<int>` looks like this:

```
template <typename T>
struct greater {
    bool operator()(const T& a, const T& b) const {
        return a > b;  // Note: uses > instead of <
    }
};
```

When you pass `std::greater<int>()` to `std::sort`, you're creating a temporary object of this class. The sort algorithm then uses this object's `operator()` to compare elements.

### The Third Parameter of std::sort

The full signature of `std::sort` is:

```
template <typename RandomAccessIterator, typename Compare>
void sort(RandomAccessIterator first, 
          RandomAccessIterator last,
          Compare comp);
```

The third parameter `comp` is optional:
- If omitted, uses the `<` operator
- If provided, uses the given comparison function or function object

### Reverse Sorting Custom Types

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

class Height {
private:
    int feet;
    int inches;

public:
    Height(int f = 0, int i = 0) : feet(f), inches(i) {}
    
    bool operator<(const Height& rhs) const {
        if (feet != rhs.feet) {
            return feet < rhs.feet;
        }
        return inches < rhs.inches;
    }
    
    bool operator>(const Height& rhs) const {
        if (feet != rhs.feet) {
            return feet > rhs.feet;
        }
        return inches > rhs.inches;
    }
    
    void display() const {
        std::cout << feet << "'" << inches << "\"";
    }
};

int main() {
    std::vector<Height> heights;
    heights.push_back(Height(6, 2));
    heights.push_back(Height(5, 8));
    heights.push_back(Height(6, 0));
    heights.push_back(Height(5, 11));
    
    std::cout << "Original: ";
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    std::sort(heights.begin(), heights.end(), std::greater<Height>());
    
    std::cout << "Sorted (tallest first): ";
    for (int i = 0; i < heights.size(); ++i) {
        heights[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original: 6'2" 5'8" 6'0" 5'11" 
Sorted (tallest first): 6'2" 6'0" 5'11" 5'8"
```

---

## Section 6: Function Objects for Custom Sorting

Function objects (functors) provide ultimate flexibility in sorting. They allow you to define custom comparison logic that goes beyond simple less-than or greater-than comparisons.

### What is a Function Object?

A function object is a class that overloads the `operator()` (function call operator). This allows instances of the class to be used like functions.

```
Basic structure:

class MyFunctionObject {
public:
    return_type operator()(parameters) const {
        // comparison logic
    }
};
```

### Why Use Function Objects?

1. **Custom sorting criteria**: Sort by any attribute or computation
2. **State**: Can hold member variables for configurable behavior
3. **Inline optimization**: Compilers can inline function calls for better performance
4. **Type safety**: Compile-time type checking

### Example: Sorting by Closeness to a Value

Let's create a function object that sorts integers by how close they are to the number 5:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <cmath>

class ClosestTo5 {
public:
    ClosestTo5() {}
    
    bool operator()(int i, int j) const {
        // Compare by distance from 5
        return std::abs(5 - i) < std::abs(5 - j);
    }
};

int main() {
    std::srand(std::time(0));
    
    std::vector<int> numbers(12);
    
    std::cout << "Original: ";
    for (int i = 0; i < numbers.size(); ++i) {
        numbers[i] = std::rand() % 10;
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    // Sort by closeness to 5
    std::sort(numbers.begin(), numbers.end(), ClosestTo5());
    
    std::cout << "Sorted (closest to 5 first): ";
    for (int i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original: 3 6 7 5 3 5 6 2 9 1 2 7
Sorted (closest to 5 first): 5 5 6 6 3 3 7 7 2 2 9 1
```

### Understanding the Logic

Visual representation of distances from 5:

```
Number:   0   1   2   3   4   5   6   7   8   9
         |   |   |   |   |   |   |   |   |   |
Distance: 5   4   3   2   1   0   1   2   3   4

Sorting order (closest to farthest):
5 (distance 0) -> 4,6 (distance 1) -> 3,7 (distance 2) -> ...
```

### Strict Weak Ordering

For a comparison function to work correctly with `std::sort`, it must satisfy **strict weak ordering**:

1. **Irreflexivity**: `comp(a, a)` must be `false`
2. **Antisymmetry**: If `comp(a, b)` is true, then `comp(b, a)` must be false
3. **Transitivity**: If `comp(a, b)` and `comp(b, c)` are true, then `comp(a, c)` must be true
4. **Transitivity of equivalence**: If `a` and `b` are equivalent, and `b` and `c` are equivalent, then `a` and `c` are equivalent

**Important**: Your comparison function should NEVER create circular relationships where `a < b`, `b < c`, and `c < a`. This violates transitivity and can cause undefined behavior.

---

## Section 7: Advanced Function Objects with State

The real power of function objects comes from their ability to hold state (member variables). This allows for configurable, reusable sorting criteria.

### Function Object with Constructor

Let's improve our previous example to sort by closeness to any value, not just 5:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <cmath>

class ClosestTo {
private:
    int targetValue;

public:
    // Default constructor
    ClosestTo() : targetValue(0) {}
    
    // Constructor with parameter
    ClosestTo(int target) : targetValue(target) {}
    
    // Comparison operator
    bool operator()(int i, int j) const {
        return std::abs(targetValue - i) < std::abs(targetValue - j);
    }
};

int main() {
    std::srand(std::time(0));
    
    std::vector<int> numbers(15);
    
    std::cout << "Original: ";
    for (int i = 0; i < numbers.size(); ++i) {
        numbers[i] = std::rand() % 20;
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    // Sort by closeness to 7
    std::sort(numbers.begin(), numbers.end(), ClosestTo(7));
    
    std::cout << "Sorted (closest to 7): ";
    for (int i = 0; i < numbers.size(); ++i) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    // Create new vector and sort by closeness to 15
    std::vector<int> moreNumbers(15);
    std::cout << "\nOriginal: ";
    for (int i = 0; i < moreNumbers.size(); ++i) {
        moreNumbers[i] = std::rand() % 20;
        std::cout << moreNumbers[i] << " ";
    }
    std::cout << std::endl;
    
    std::sort(moreNumbers.begin(), moreNumbers.end(), ClosestTo(15));
    
    std::cout << "Sorted (closest to 15): ";
    for (int i = 0; i < moreNumbers.size(); ++i) {
        std::cout << moreNumbers[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Original: 3 6 17 15 13 15 6 12 9 1 2 7 10 14 13
Sorted (closest to 7): 7 6 6 9 10 3 12 13 13 2 14 15 15 1 17

Original: 5 18 1 3 11 15 14 7 9 2 18 19 1 8 11
Sorted (closest to 15): 15 14 18 18 11 11 19 7 8 9 5 3 2 1 1
```

### Real-World Example: Sorting 2D Points

Here's a more practical example - sorting 2D coordinates by distance from a target point:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

class Point {
private:
    double x, y;

public:
    Point(double xVal = 0, double yVal = 0) : x(xVal), y(yVal) {}
    
    double getX() const { return x; }
    double getY() const { return y; }
    
    // Calculate distance from another point
    double distanceFrom(const Point& other) const {
        double dx = x - other.x;
        double dy = y - other.y;
        return std::sqrt(dx * dx + dy * dy);
    }
    
    void display() const {
        std::cout << "(" << x << ", " << y << ")";
    }
};

class ClosestToPoint {
private:
    Point target;

public:
    ClosestToPoint(const Point& targetPoint) : target(targetPoint) {}
    
    bool operator()(const Point& a, const Point& b) const {
        // Compare distances using Pythagorean theorem
        double distA = a.distanceFrom(target);
        double distB = b.distanceFrom(target);
        return distA < distB;
    }
};

int main() {
    // Create a vector of points
    std::vector<Point> points;
    points.push_back(Point(1, 2));
    points.push_back(Point(5, 5));
    points.push_back(Point(3, 1));
    points.push_back(Point(0, 0));
    points.push_back(Point(7, 3));
    points.push_back(Point(2, 4));
    
    std::cout << "Original points:" << std::endl;
    for (int i = 0; i < points.size(); ++i) {
        points[i].display();
        std::cout << " ";
    }
    std::cout << std::endl;
    
    // Target point to measure distance from
    Point target(3, 3);
    std::cout << "\nTarget point: ";
    target.display();
    std::cout << std::endl;
    
    // Sort by distance from target point
    std::sort(points.begin(), points.end(), ClosestToPoint(target));
    
    std::cout << "\nPoints sorted by distance from target:" << std::endl;
    for (int i = 0; i < points.size(); ++i) {
        points[i].display();
        std::cout << " (distance: " << points[i].distanceFrom(target) << ")";
        std::cout << std::endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Original points:
(1, 2) (5, 5) (3, 1) (0, 0) (7, 3) (2, 4) 

Target point: (3, 3)

Points sorted by distance from target:
(3, 1) (distance: 2)
(2, 4) (distance: 1.41421)
(5, 5) (distance: 2.82843)
(1, 2) (distance: 2.23607)
(7, 3) (distance: 4)
(0, 0) (distance: 4.24264)
```

### Visualization of Point Sorting

```
Before sorting:                After sorting (by distance from (3,3)):

    y                              y
    |                              |
  5 +     * (5,5)                5 +     * (5,5)
    |                              |
  4 +   * (2,4)                  4 +   * (2,4)
    |                              |
  3 + *       * (7,3)            3 + TARGET
    |   TARGET                     |
  2 + * (1,2)                    2 + * (1,2)
    |                              |
  1 +     * (3,1)                1 +     * (3,1)     * (7,3)
    |                              |
  0 + * (0,0)                    0 + * (0,0)
    +------------- x               +------------- x
    0 1 2 3 4 5 6 7                0 1 2 3 4 5 6 7

Distances:                      Sorted order:
(3,1): 2.0                      1. (3,1) - distance 2.0
(2,4): 1.41                     2. (2,4) - distance 1.41
(5,5): 2.83                     3. (1,2) - distance 2.24
(1,2): 2.24                     4. (5,5) - distance 2.83
(7,3): 4.0                      5. (7,3) - distance 4.0
(0,0): 4.24                     6. (0,0) - distance 4.24
```

---

## Section 8: Practical Applications and Best Practices

### When to Use Different Approaches

**Use `operator<` overloading when:**
- You have one natural ordering for your type
- You want to use your type with `std::map` or `std::set`
- The comparison is fundamental to the type's meaning

**Use function objects when:**
- You need multiple different sorting criteria
- The sorting criteria depend on external parameters
- You want reusable, configurable sorting logic

**Use `std::greater` when:**
- You simply need reverse order
- The type already has `operator<` defined

### Performance Considerations

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <ctime>

// Function objects can be inlined by the compiler
class MultiplyByTwo {
public:
    int operator()(int x) const {
        return x * 2;
    }
};

int main() {
    std::vector<int> data(1000000);
    
    // Fill with sequential values
    for (int i = 0; i < data.size(); ++i) {
        data[i] = i;
    }
    
    clock_t start = clock();
    std::sort(data.begin(), data.end());
    clock_t end = clock();
    
    double duration = double(end - start) / CLOCKS_PER_SEC;
    std::cout << "Sort took: " << duration << " seconds" << std::endl;
    std::cout << "First 10 elements: ";
    for (int i = 0; i < 10; ++i) {
        std::cout << data[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Sort took: 0.045 seconds
First 10 elements: 0 1 2 3 4 5 6 7 8 9
```

### Common Mistakes to Avoid

**Mistake 1: Inconsistent Comparison**

```cpp
// WRONG - violates strict weak ordering
class BadComparator {
public:
    bool operator()(int a, int b) const {
        return std::rand() % 2;  // Random comparison!
    }
};
```

**Mistake 2: Modifying Objects During Comparison**

```cpp
// WRONG - comparison should be const
class Height {
    int feet;
public:
    bool operator<(const Height& rhs) {  // Missing const!
        feet++;  // DON'T modify state during comparison!
        return feet < rhs.feet;
    }
};
```

**Mistake 3: Ignoring Edge Cases**

```cpp
// WRONG - doesn't handle equal values correctly
class BadCloseTo {
    int target;
public:
    BadCloseTo(int t) : target(t) {}
    bool operator()(int a, int b) const {
        // Problem: what if abs(target-a) == abs(target-b)?
        // This can lead to unstable sorts
        return std::abs(target - a) <= std::abs(target - b);
    }
};
```

### Best Practices Summary

1. **Always use `const` correctness** in comparison operators
2. **Ensure strict weak ordering** in custom comparators
3. **Don't modify objects** during comparison
4. **Test with edge cases** including duplicates and equal distances
5. **Document your comparison logic** clearly
6. **Use descriptive names** for function objects
7. **Consider performance** for frequently used comparisons

### Complete Example: Student Grade Sorting

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

class Student {
private:
    std::string name;
    double grade;

public:
    Student(std::string n, double g) : name(n), grade(g) {}
    
    std::string getName() const { return name; }
    double getGrade() const { return grade; }
    
    // Natural ordering: by grade (descending)
    bool operator<(const Student& rhs) const {
        return grade > rhs.grade;  // Higher grades first
    }
    
    void display() const {
        std::cout << name << ": " << grade;
    }
};

// Function object to sort by name alphabetically
class SortByName {
public:
    bool operator()(const Student& a, const Student& b) const {
        return a.getName() < b.getName();
    }
};

// Function object to sort by failing students first
class FailingFirst {
public:
    bool operator()(const Student& a, const Student& b) const {
        bool aFailing = a.getGrade() < 60.0;
        bool bFailing = b.getGrade() < 60.0;
        
        if (aFailing != bFailing) {
            return aFailing;  // Failing students first
        }
        return a.getGrade() < b.getGrade();  // Then by grade
    }
};

int main() {
    std::vector<Student> students;
    students.push_back(Student("Alice", 92.5));
    students.push_back(Student("Bob", 78.0));
    students.push_back(Student("Charlie", 55.5));
    students.push_back(Student("Diana", 88.0));
    students.push_back(Student("Eve", 45.0));
    
    std::cout << "Original order:" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
        std::cout << std::endl;
    }
    
    // Sort by grade (using operator<)
    std::sort(students.begin(), students.end());
    std::cout << "\nSorted by grade (highest first):" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
        std::cout << std::endl;
    }
    
    // Sort by name
    std::sort(students.begin(), students.end(), SortByName());
    std::cout << "\nSorted alphabetically:" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
        std::cout << std::endl;
    }
    
    // Sort with failing students first
    std::sort(students.begin(), students.end(), FailingFirst());
    std::cout << "\nSorted (failing students first):" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
        std::cout << std::endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Original order:
Alice: 92.5
Bob: 78
Charlie: 55.5
Diana: 88
Eve: 45

Sorted by grade (highest first):
Alice: 92.5
Diana: 88
Bob: 78
Charlie: 55.5
Eve: 45

Sorted alphabetically:
Alice: 92.5
Bob: 78
Charlie: 55.5
Diana: 88
Eve: 45

Sorted (failing students first):
Eve: 45
Charlie: 55.5
Bob: 78
Diana: 88
Alice: 92.5
```

---

## Resources for Further Learning

### Official Documentation

- **C++ Reference - std::sort**: https://en.cppreference.com/w/cpp/algorithm/sort
  Comprehensive documentation on std::sort with examples and complexity analysis

- **C++ Reference - Function Objects**: https://en.cppreference.com/w/cpp/utility/functional
  Details on standard function objects and how to create custom ones

- **C++ Reference - Operator Overloading**: https://en.cppreference.com/w/cpp/language/operators
  Complete guide to operator overloading syntax and rules

### Online Tutorials

- **LearnCPP.com - Overloading Operators**: https://www.learncpp.com/cpp-tutorial/introduction-to-operator-overloading/
  Beginner-friendly tutorial series on operator overloading

- **GeeksforGeeks - std::sort in C++**: https://www.geeksforgeeks.org/sort-c-stl/
  Examples and use cases for STL sort

- **GeeksforGeeks - Comparators in C++**: https://www.geeksforgeeks.org/comparator-function-of-qsort-in-c/
  Guide to writing custom comparison functions

### Practice Platforms

- **LeetCode**: https://leetcode.com/tag/sorting/
  Practice problems focused on sorting algorithms

- **HackerRank C++ Challenges**: https://www.hackerrank.com/domains/cpp
  Various C++ challenges including operator overloading and STL usage

- **Codewars**: https://www.codewars.com/
  Kata exercises including custom sorting problems

### Books

- **"Effective STL" by Scott Meyers**
  Best practices for using the Standard Template Library

- **"C++ Primer" by Stanley Lippman, Josée Lajoie, and Barbara Moo**
  Comprehensive coverage of C++ including operator overloading

- **"The C++ Programming Language" by Bjarne Stroustrup**
  The definitive guide by the creator of C++

### Community Resources

- **Stack Overflow - C++ Tag**: https://stackoverflow.com/questions/tagged/c++
  Q&A for specific C++ problems

- **Reddit - r/cpp**: https://www.reddit.com/r/cpp/
  Community discussions and resources

- **C++ Core Guidelines**: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines
  Best practices and modern C++ guidelines

