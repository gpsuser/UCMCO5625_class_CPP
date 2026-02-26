# Organizing and Documenting Larger C++ Projects

## Learning Outcomes

By the end of this lecture, you should be able to:

* Organize C++ projects into multiple files using header and implementation files
* Distinguish between when to use .h/.cpp separation and .hpp files for templates
* Create and use custom header files with proper include guards
* Understand the difference between quoted and angled bracket includes
* Implement operator overloading in separate header files
* Document C++ code using Javadoc-style comments for Doxygen
* Generate HTML documentation from source code using Doxygen
* Configure and customize Doxygen settings for project documentation
* Create complete documented projects with classes, templates, and namespaces
* Define and use custom namespaces to avoid naming conflicts

---

## Table of Contents

1. [Introduction to Multi-File Projects](#section-1-introduction-to-multi-file-projects)
2. [Header Files and Implementation Files](#section-2-header-files-and-implementation-files)
3. [Working with Templates](#section-3-working-with-templates)
4. [Code Documentation with Doxygen](#section-4-code-documentation-with-doxygen)
5. [Namespaces](#section-5-namespaces)
6. [Complete Doxygen Project Example](#section-6-complete-doxygen-project-example)
7. [Resources](#resources)

---

## Section 1: Introduction to Multi-File Projects

As C++ projects grow in size and complexity, organizing code into multiple files becomes essential for maintainability, reusability, and team collaboration. Rather than placing all code in a single source file, we can structure our projects into logical units.

### Why Use Multiple Files?

* **Separation of Concerns**: Each class or group of related functions can have its own file
* **Compilation Efficiency**: Only modified files need to be recompiled
* **Code Reusability**: Header files can be included in multiple projects
* **Team Collaboration**: Different team members can work on different files
* **Readability**: Smaller files are easier to understand and maintain

### File Organization Strategy

The general organization strategy for C++ projects is:

* **Each class** gets its own header file (.h) and possibly its own implementation file (.cpp)
* **Related standalone functions** are grouped together (e.g., math functions, graphics functions)
* **Template code** goes entirely in .hpp files (no separate implementation)

### Why Templates Are Different

Templates require special handling because their compilation depends on template parameters. The compiler needs to see the full implementation when instantiating a template, so we cannot compile template code in advance. Therefore, template declarations and implementations must be in the same file (conventionally .hpp).

```
Project Structure Example:
├── main.cpp
├── Height.h           (class declaration)
├── Height.cpp         (class implementation)
├── MathUtils.h        (utility functions declaration)
├── MathUtils.cpp      (utility functions implementation)
└── Container.hpp      (template class - all code)
```

[Back to Table of Contents](#table-of-contents)

---

## Section 2: Header Files and Implementation Files

Header files (.h) and implementation files (.cpp) work together to separate interface from implementation. This separation is a fundamental principle in C++ programming.

### The Header File (.h)

The header file contains:
* Class declarations
* Method declarations (signatures)
* Inline functions
* Constants and type definitions
* Include guards or `#pragma once`

### The Implementation File (.cpp)

The implementation file contains:
* Method implementations
* Definitions of non-inline functions
* Includes the corresponding header file

### Example: Height Class

Let's examine a complete example of a Height class that demonstrates proper file organization.

**Height.h:**

```cpp
#pragma once
#include <iostream>

class Height
{
    int ft, in;
public:
    Height() {}
    Height(int feet, int inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height& rhs) const;
    bool operator > (const Height& rhs) const;
    Height operator + (const Height& rhs) const;
    
    friend std::ostream& operator << (std::ostream& o, const Height& h);
};
```

**Height.cpp:**

```cpp
#include "Height.h"

bool Height::operator < (const Height& rhs) const
{
    int totalInchesThis = ft * 12 + in;
    int totalInchesRhs = rhs.ft * 12 + rhs.in;
    return totalInchesThis < totalInchesRhs;
}

bool Height::operator > (const Height& rhs) const
{
    int totalInchesThis = ft * 12 + in;
    int totalInchesRhs = rhs.ft * 12 + rhs.in;
    return totalInchesThis > totalInchesRhs;
}

Height Height::operator + (const Height& rhs) const
{
    int totalInches = (ft * 12 + in) + (rhs.ft * 12 + rhs.in);
    return Height(totalInches / 12, totalInches % 12);
}

std::ostream& operator << (std::ostream& o, const Height& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Understanding #pragma once

The `#pragma once` directive is an include guard that prevents multiple inclusion of the same header file. Without it, including the same header twice would cause compilation errors due to duplicate definitions.

Traditional include guards look like this:
```cpp
#ifndef HEIGHT_H
#define HEIGHT_H
// header content
#endif
```

However, `#pragma once` is simpler and widely supported by modern compilers.

### When to Implement in Header vs CPP

**Implement in the header file (.h):**
* Very short functions (single line)
* Inline functions
* Template code (actually in .hpp)

**Implement in the implementation file (.cpp):**
* Multi-line methods
* Functions that may change frequently
* Any function where compilation time matters

This separation allows the .cpp file to be compiled once to an object (.o) file and only recompiled when the implementation changes.

### Using Your Header File

Here's a complete program that uses the Height class:

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.h"

int main()
{
    std::vector<Height> heights;
    heights.push_back(Height(6, 2));
    heights.push_back(Height(4, 9));
    heights.push_back(Height(5, 11));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights:" << std::endl;
    for (auto i : heights)
        std::cout << i << std::endl;
    
    Height total(0, 0);
    for (auto i : heights)
        total = total + i;
    
    std::cout << "Total height: " << total << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted heights:
4'9"
5'11"
6'2"
Total height: 16'10"
```

### Include Conventions: Quotes vs Angle Brackets

Notice the different include styles:

```cpp
#include <iostream>      // Standard library - angle brackets
#include "Height.h"      // Project file - quotes
```

**Angle brackets `< >`:** The compiler searches in system include directories (where standard library headers are located)

**Quotes `" "`:** The compiler first searches in the current project directory, then falls back to system directories

This convention helps distinguish between:
* Standard library and third-party library headers (angle brackets)
* Your project's headers (quotes)

[Back to Table of Contents](#table-of-contents)

---

## Section 3: Working with Templates

Templates require special consideration because their compilation is parameter-dependent. This section explores how to organize template code effectively.

### Why Templates Need Different Organization

Consider what happens when you use a template:

```cpp
std::vector<int> numbers;        // Compiler generates vector for int
std::vector<std::string> words;  // Compiler generates vector for string
```

The compiler must generate different code for each template parameter combination. This means:
* The implementation must be visible at compile time
* We cannot pre-compile templates into .o files
* Declaration and implementation must be in the same file

### The .hpp Convention

By convention, template code goes in .hpp files. While this is not required by the C++ standard, it's a widely recognized practice that signals "this file contains templates."

### Template Height Class Example

Let's convert our Height class to a template that allows fractional inches:

**Height.hpp:**

```cpp
#pragma once
#include <iostream>

// Template class declaration
template <typename T>
class Height
{
    int ft;
    T in;
public:
    Height() : ft(0), in(0) {}
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height<T>& rhs) const;
    bool operator > (const Height<T>& rhs) const;
    Height<T> operator + (const Height<T>& rhs) const;
    
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Template implementation (in same file)
template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<T>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Using the Template

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.hpp"

int main()
{
    std::vector<Height<double>> heights;
    heights.push_back(Height<double>(6, 2.5));
    heights.push_back(Height<double>(4, 9.75));
    heights.push_back(Height<double>(5, 11.8));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Sorted heights:" << std::endl;
    for (auto i : heights)
        std::cout << i << std::endl;
    
    Height<double> total(0, 0.0);
    for (auto i : heights)
        total = total + i;
    
    std::cout << "Total height: " << total << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Sorted heights:
4'9.75"
5'11.8"
6'2.5"
Total height: 17'0.05"
```

### Template Organization Best Practices

Even though everything goes in one file, we still maintain separation between:

1. **Declaration section** (top of file):
   * Class template declaration
   * Method signatures
   * Documentation comments

2. **Implementation section** (bottom of file):
   * Method implementations
   * Template specializations

This organization preserves readability and makes the header file serve as documentation.

[Back to Table of Contents](#table-of-contents)

---

## Section 4: Code Documentation with Doxygen

As we create reusable header files, documentation becomes crucial. Other programmers (including your future self) need clear instructions on how to use your code.

### Why Document?

When you've invested time in creating reusable code following the DRY (Don't Repeat Yourself) principle, documentation ensures that effort pays off. Well-documented headers can be as accessible as the Standard Library.

### Javadoc-Style Comments

Javadoc is a documentation standard that works with both Java and C++. Special comment blocks are processed by tools like Doxygen to generate HTML documentation.

### Doxygen Comment Syntax

**Special comment blocks:**
```cpp
/**
 * This is a Doxygen comment block
 */
```

**Common Doxygen tags:**
* `@brief` - Short description
* `@param` - Parameter description
* `@return` - Return value description
* `@tparam` - Template parameter description
* `@throw` - Exception that may be thrown
* `@note` - Additional notes
* `@warning` - Warnings about usage

### Documented Height Template

Here's the Height class with complete Doxygen documentation:

**Height.hpp (documented):**

```cpp
#pragma once
#include <iostream>

/**
 * @brief A template class representing a height in feet and inches
 * 
 * This class stores a height using separate values for feet (integer)
 * and inches (template type). It provides comparison operators and
 * addition functionality.
 * 
 * @tparam T The type for storing inches (e.g., int, double, float)
 */
template <typename T>
class Height
{
    int ft;  ///< Feet component of the height
    T in;    ///< Inches component of the height

public:
    /**
     * @brief Default constructor
     * 
     * Initializes height to 0 feet, 0 inches
     */
    Height() : ft(0), in(0) {}
    
    /**
     * @brief Construct a height from feet and inches
     * 
     * @param feet The number of feet
     * @param inches The number of inches
     */
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    /**
     * @brief Less-than comparison operator
     * 
     * Compares two heights by converting both to total inches
     * 
     * @param rhs The height to compare against
     * @return true if this height is less than rhs
     */
    bool operator < (const Height<T>& rhs) const;
    
    /**
     * @brief Greater-than comparison operator
     * 
     * @param rhs The height to compare against
     * @return true if this height is greater than rhs
     */
    bool operator > (const Height<T>& rhs) const;
    
    /**
     * @brief Addition operator
     * 
     * Adds two heights together, properly handling inch overflow
     * 
     * @param rhs The height to add
     * @return A new Height object containing the sum
     */
    Height<T> operator + (const Height<T>& rhs) const;
    
    /**
     * @brief Stream output operator
     * 
     * Outputs height in the format: feet'inches"
     * 
     * @tparam U Template parameter for Height
     * @param o The output stream
     * @param h The height to output
     * @return Reference to the output stream
     */
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Implementation section

template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<U>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}
```

### Generating Documentation with Doxygen

1. **Install Doxygen** from https://www.doxygen.nl/index.html

2. **Create a Doxygen configuration file**:
   ```bash
   doxygen -g
   ```

3. **Edit the Doxyfile** to set:
   * `PROJECT_NAME = "Height Class Documentation"`
   * `INPUT = .` (or your source directory)
   * `RECURSIVE = YES`
   * `EXTRACT_ALL = YES`

4. **Generate documentation**:
   ```bash
   doxygen Doxyfile
   ```

5. **View the HTML output** in `html/index.html`

The generated documentation will include:
* Class hierarchy
* Member descriptions
* Parameter and return value information
* Links between related classes
* Search functionality

### Documentation Best Practices

* **Document the interface, not the implementation** - Focus on what the code does, not how
* **Keep documentation up-to-date** - Outdated documentation is worse than no documentation
* **Be concise but complete** - Include all necessary information without being verbose
* **Use consistent formatting** - Follow a standard style throughout your project
* **Document edge cases and limitations** - Help users avoid common pitfalls

[Back to Table of Contents](#table-of-contents)

---

## Section 5: Namespaces

As you create more reusable code, the risk of naming conflicts increases. Namespaces provide a way to organize code and prevent these conflicts.

### The Problem: Name Collisions

Imagine two libraries both define a class called `Height`:

```cpp
// Library A's Height
class Height { /* ... */ };

// Library B's Height
class Height { /* ... */ };
```

Which `Height` does the compiler use? This causes a compilation error.

### The Solution: Namespaces

Namespaces create distinct scopes for identifiers:

```cpp
namespace LibraryA {
    class Height { /* ... */ };
}

namespace LibraryB {
    class Height { /* ... */ };
}
```

Now we can use both:
```cpp
LibraryA::Height h1;
LibraryB::Height h2;
```

### Creating a Namespace

Wrap your code in a namespace declaration:

```cpp
namespace MyNamespace {
    // All your code goes here
    class MyClass { /* ... */ };
    void myFunction() { /* ... */ }
}
```

### Height Class with Namespace

Here's the complete Height class in the CO5625 namespace:

**Height.hpp:**

```cpp
#pragma once
#include <iostream>

namespace CO5625 {

/**
 * @brief A template class representing a height in feet and inches
 * @tparam T The type for storing inches
 */
template <typename T>
class Height
{
    int ft;
    T in;

public:
    Height() : ft(0), in(0) {}
    Height(int feet, T inches) : ft(feet), in(inches) {}
    
    bool operator < (const Height<T>& rhs) const;
    bool operator > (const Height<T>& rhs) const;
    Height<T> operator + (const Height<T>& rhs) const;
    
    template <typename U>
    friend std::ostream& operator << (std::ostream& o, const Height<U>& h);
};

// Implementation
template <typename T>
bool Height<T>::operator < (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis < totalInchesRhs;
}

template <typename T>
bool Height<T>::operator > (const Height<T>& rhs) const
{
    double totalInchesThis = ft * 12.0 + static_cast<double>(in);
    double totalInchesRhs = rhs.ft * 12.0 + static_cast<double>(rhs.in);
    return totalInchesThis > totalInchesRhs;
}

template <typename T>
Height<T> Height<T>::operator + (const Height<T>& rhs) const
{
    double totalInches = (ft * 12.0 + static_cast<double>(in)) + 
                         (rhs.ft * 12.0 + static_cast<double>(rhs.in));
    int newFeet = static_cast<int>(totalInches / 12.0);
    T newInches = static_cast<T>(totalInches - newFeet * 12.0);
    return Height<T>(newFeet, newInches);
}

template <typename T>
std::ostream& operator << (std::ostream& o, const Height<T>& h)
{
    o << h.ft << "'" << h.in << "\"";
    return o;
}

} // namespace CO5625
```

### Using Code from a Namespace

**Option 1: Fully qualified names**
```cpp
CO5625::Height<double> h(6, 2.5);
```

**Option 2: Using declarations**
```cpp
using CO5625::Height;
Height<double> h(6, 2.5);
```

**Option 3: Using directives**
```cpp
using namespace CO5625;
Height<double> h(6, 2.5);
```

### Complete Example with Namespace

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
#include "Height.hpp"

using namespace CO5625;

int main()
{
    std::vector<Height<double>> heights;
    heights.push_back(Height<double>(6, 2.5));
    heights.push_back(Height<double>(4, 9.75));
    heights.push_back(Height<double>(5, 11.8));
    
    std::sort(heights.begin(), heights.end());
    
    std::cout << "Heights from CO5625 namespace:" << std::endl;
    for (const auto& h : heights)
        std::cout << h << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
Heights from CO5625 namespace:
4'9.75"
5'11.8"
6'2.5"
```

### Namespace Best Practices

* **Use namespaces for all reusable code** - Protect against future conflicts
* **Choose descriptive namespace names** - Avoid generic names like "Utilities"
* **Avoid `using namespace` in headers** - This pollutes the global namespace for all includers
* **Nest namespaces for organization** - e.g., `MyCompany::Graphics::2D`
* **Use inline namespaces for versioning** - Allow backward compatibility

### Nested Namespaces

```cpp
namespace CO5625 {
    namespace DataStructures {
        class LinkedList { /* ... */ };
    }
    
    namespace Algorithms {
        void quickSort() { /* ... */ };
    }
}

// C++17 syntax
namespace CO5625::DataStructures {
    class LinkedList { /* ... */ };
}
```

[Back to Table of Contents](#table-of-contents)

---

## Section 6: Complete Doxygen Project Example

This section demonstrates a complete workflow for creating a documented C++ project using Doxygen. We'll build a small project with a class, a template, and a namespace, then generate professional HTML documentation.

### Project Overview

We'll create a simple geometry library with:
* A `Rectangle` class (regular class)
* A `Container` template (template class)
* Everything organized in the `Geometry` namespace

### Project Structure

```
GeometryLib/
├── Rectangle.h
├── Rectangle.cpp
├── Container.hpp
├── main.cpp
└── Doxyfile (generated)
```

### File 1: Rectangle.h

```cpp
#pragma once
#include <iostream>

namespace Geometry {

/**
 * @brief A class representing a rectangle with width and height
 * 
 * This class provides basic rectangle operations including area
 * calculation, perimeter calculation, and comparison operators.
 * All dimensions are stored as double values.
 * 
 * @author Geometry Library Team
 * @version 1.0
 */
class Rectangle
{
private:
    double width;   ///< Width of the rectangle in units
    double height;  ///< Height of the rectangle in units

public:
    /**
     * @brief Default constructor
     * 
     * Creates a rectangle with width and height of 0.0
     */
    Rectangle();
    
    /**
     * @brief Construct a rectangle with specified dimensions
     * 
     * @param w The width of the rectangle (must be positive)
     * @param h The height of the rectangle (must be positive)
     * @throw std::invalid_argument if width or height is negative
     */
    Rectangle(double w, double h);
    
    /**
     * @brief Calculate the area of the rectangle
     * 
     * @return The area (width * height)
     */
    double area() const;
    
    /**
     * @brief Calculate the perimeter of the rectangle
     * 
     * @return The perimeter (2 * width + 2 * height)
     */
    double perimeter() const;
    
    /**
     * @brief Get the width of the rectangle
     * 
     * @return The width value
     */
    double getWidth() const;
    
    /**
     * @brief Get the height of the rectangle
     * 
     * @return The height value
     */
    double getHeight() const;
    
    /**
     * @brief Set new dimensions for the rectangle
     * 
     * @param w The new width (must be positive)
     * @param h The new height (must be positive)
     * @throw std::invalid_argument if width or height is negative
     */
    void setDimensions(double w, double h);
    
    /**
     * @brief Less-than comparison operator
     * 
     * Compares rectangles based on their area
     * 
     * @param other The rectangle to compare against
     * @return true if this rectangle's area is less than other's area
     */
    bool operator<(const Rectangle& other) const;
    
    /**
     * @brief Stream insertion operator for output
     * 
     * Outputs rectangle in the format: [width x height]
     * 
     * @param os The output stream
     * @param rect The rectangle to output
     * @return Reference to the output stream
     */
    friend std::ostream& operator<<(std::ostream& os, const Rectangle& rect);
};

} // namespace Geometry
```

### File 2: Rectangle.cpp

```cpp
#include "Rectangle.h"
#include <stdexcept>

namespace Geometry {

Rectangle::Rectangle() : width(0.0), height(0.0) {}

Rectangle::Rectangle(double w, double h) : width(w), height(h)
{
    if (w < 0 || h < 0)
    {
        throw std::invalid_argument("Width and height must be non-negative");
    }
}

double Rectangle::area() const
{
    return width * height;
}

double Rectangle::perimeter() const
{
    return 2 * width + 2 * height;
}

double Rectangle::getWidth() const
{
    return width;
}

double Rectangle::getHeight() const
{
    return height;
}

void Rectangle::setDimensions(double w, double h)
{
    if (w < 0 || h < 0)
    {
        throw std::invalid_argument("Width and height must be non-negative");
    }
    width = w;
    height = h;
}

bool Rectangle::operator<(const Rectangle& other) const
{
    return this->area() < other.area();
}

std::ostream& operator<<(std::ostream& os, const Rectangle& rect)
{
    os << "[" << rect.width << " x " << rect.height << "]";
    return os;
}

} // namespace Geometry
```

### File 3: Container.hpp

```cpp
#pragma once
#include <vector>
#include <algorithm>
#include <iostream>

namespace Geometry {

/**
 * @brief A generic container template class for storing and managing objects
 * 
 * This template class provides a wrapper around std::vector with additional
 * functionality for common operations like finding minimum/maximum elements,
 * sorting, and displaying contents.
 * 
 * @tparam T The type of elements to store (must support operator< and operator<<)
 * 
 * @note Type T must be comparable (operator<) for sorting and min/max operations
 * @note Type T must support stream output (operator<<) for display operations
 */
template <typename T>
class Container
{
private:
    std::vector<T> items;  ///< Internal storage for items

public:
    /**
     * @brief Default constructor
     * 
     * Creates an empty container
     */
    Container() {}
    
    /**
     * @brief Add an item to the container
     * 
     * @param item The item to add
     */
    void add(const T& item)
    {
        items.push_back(item);
    }
    
    /**
     * @brief Get the number of items in the container
     * 
     * @return The number of items
     */
    size_t size() const
    {
        return items.size();
    }
    
    /**
     * @brief Check if the container is empty
     * 
     * @return true if container has no items, false otherwise
     */
    bool empty() const
    {
        return items.empty();
    }
    
    /**
     * @brief Sort all items in ascending order
     * 
     * Uses the operator< of type T for comparison
     */
    void sort()
    {
        std::sort(items.begin(), items.end());
    }
    
    /**
     * @brief Find the minimum element
     * 
     * @return The smallest element in the container
     * @throw std::runtime_error if container is empty
     */
    T findMin() const
    {
        if (items.empty())
        {
            throw std::runtime_error("Cannot find minimum of empty container");
        }
        return *std::min_element(items.begin(), items.end());
    }
    
    /**
     * @brief Find the maximum element
     * 
     * @return The largest element in the container
     * @throw std::runtime_error if container is empty
     */
    T findMax() const
    {
        if (items.empty())
        {
            throw std::runtime_error("Cannot find maximum of empty container");
        }
        return *std::max_element(items.begin(), items.end());
    }
    
    /**
     * @brief Display all items in the container
     * 
     * Outputs each item on a separate line using operator<<
     */
    void display() const
    {
        for (const auto& item : items)
        {
            std::cout << item << std::endl;
        }
    }
    
    /**
     * @brief Access an item by index
     * 
     * @param index The index of the item to access
     * @return Reference to the item at the specified index
     * @throw std::out_of_range if index is invalid
     */
    T& operator[](size_t index)
    {
        return items.at(index);
    }
    
    /**
     * @brief Access an item by index (const version)
     * 
     * @param index The index of the item to access
     * @return Const reference to the item at the specified index
     * @throw std::out_of_range if index is invalid
     */
    const T& operator[](size_t index) const
    {
        return items.at(index);
    }
};

} // namespace Geometry
```

### File 4: main.cpp

```cpp
#include <iostream>
#include "Rectangle.h"
#include "Container.hpp"

using namespace Geometry;

/**
 * @file main.cpp
 * @brief Demonstration program for the Geometry library
 * 
 * This program demonstrates the use of the Rectangle class
 * and Container template with various operations.
 */

int main()
{
    std::cout << "=== Rectangle Examples ===" << std::endl;
    
    // Create rectangles
    Rectangle r1(5.0, 3.0);
    Rectangle r2(4.0, 4.0);
    Rectangle r3(6.0, 2.0);
    
    // Display rectangles
    std::cout << "Rectangle 1: " << r1 << std::endl;
    std::cout << "  Area: " << r1.area() << std::endl;
    std::cout << "  Perimeter: " << r1.perimeter() << std::endl;
    
    std::cout << "Rectangle 2: " << r2 << std::endl;
    std::cout << "  Area: " << r2.area() << std::endl;
    
    std::cout << "Rectangle 3: " << r3 << std::endl;
    std::cout << "  Area: " << r3.area() << std::endl;
    
    // Comparison
    if (r1 < r2)
        std::cout << "\nRectangle 1 has smaller area than Rectangle 2" << std::endl;
    
    std::cout << "\n=== Container Examples ===" << std::endl;
    
    // Create container of rectangles
    Container<Rectangle> rectContainer;
    rectContainer.add(r1);
    rectContainer.add(r2);
    rectContainer.add(r3);
    
    std::cout << "Container has " << rectContainer.size() << " rectangles" << std::endl;
    
    std::cout << "\nBefore sorting:" << std::endl;
    rectContainer.display();
    
    rectContainer.sort();
    
    std::cout << "\nAfter sorting (by area):" << std::endl;
    rectContainer.display();
    
    std::cout << "\nMinimum rectangle: " << rectContainer.findMin() << std::endl;
    std::cout << "Maximum rectangle: " << rectContainer.findMax() << std::endl;
    
    // Container with integers
    std::cout << "\n=== Integer Container ===" << std::endl;
    Container<int> intContainer;
    intContainer.add(42);
    intContainer.add(17);
    intContainer.add(99);
    intContainer.add(8);
    
    std::cout << "Before sorting:" << std::endl;
    intContainer.display();
    
    intContainer.sort();
    
    std::cout << "\nAfter sorting:" << std::endl;
    intContainer.display();
    
    std::cout << "\nMin: " << intContainer.findMin() << std::endl;
    std::cout << "Max: " << intContainer.findMax() << std::endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Rectangle Examples ===
Rectangle 1: [5 x 3]
  Area: 15
  Perimeter: 16
Rectangle 2: [4 x 4]
  Area: 16
Rectangle 3: [6 x 2]
  Area: 12

Rectangle 1 has smaller area than Rectangle 2

=== Container Examples ===
Container has 3 rectangles

Before sorting:
[5 x 3]
[4 x 4]
[6 x 2]

After sorting (by area):
[6 x 2]
[5 x 3]
[4 x 4]

Minimum rectangle: [6 x 2]
Maximum rectangle: [4 x 4]

=== Integer Container ===
Before sorting:
42
17
99
8

After sorting:
8
17
42
99

Min: 8
Max: 99
```

### Generating Documentation with Doxygen

Now let's generate the HTML documentation for this project.

**Step 1: Generate the Doxyfile configuration**

In the project directory, run:
```bash
doxygen -g
```

This creates a `Doxyfile` with default settings.

**Step 2: Edit the Doxyfile**

Open the Doxyfile and modify these key settings:

```bash
PROJECT_NAME           = "Geometry Library"
PROJECT_BRIEF          = "A simple C++ library for geometric shapes"
PROJECT_NUMBER         = 1.0
OUTPUT_DIRECTORY       = ./docs
INPUT                  = .
RECURSIVE              = YES
EXTRACT_ALL            = YES
EXTRACT_PRIVATE        = YES
EXTRACT_STATIC         = YES
GENERATE_LATEX         = NO
HAVE_DOT               = NO
```

**Step 3: Generate the documentation**

Run Doxygen:
```bash
doxygen Doxyfile
```

This will create a `docs/` directory with HTML documentation.

**Step 4: View the documentation**

Open the generated documentation:
```bash
# On Linux/Mac
open docs/html/index.html

# On Windows
start docs/html/index.html

# Or simply navigate to the file in your browser
```

### What the Generated Documentation Looks Like

The generated HTML documentation will include several sections:

**Main Page (index.html):**
```
=====================================================
        Geometry Library Documentation
           Version 1.0
=====================================================

A simple C++ library for geometric shapes

Classes | Namespaces | Files

Quick Links:
- Class List
- Namespace List
- File List
```

**Namespace Page (namespace_geometry.html):**
```
Geometry Namespace Reference

Classes:
  Rectangle - A class representing a rectangle with 
              width and height
  Container<T> - A generic container template class 
                 for storing and managing objects

Functions:
  std::ostream& operator<< - Stream insertion operator
```

**Rectangle Class Page (class_geometry_rectangle.html):**
```
Geometry::Rectangle Class Reference

A class representing a rectangle with width and height

#include <Rectangle.h>

Public Member Functions:
  Rectangle()
    Default constructor
    
  Rectangle(double w, double h)
    Construct a rectangle with specified dimensions
    Parameters:
      w - The width of the rectangle (must be positive)
      h - The height of the rectangle (must be positive)
    Throws:
      std::invalid_argument if width or height is negative
      
  double area() const
    Calculate the area of the rectangle
    Returns:
      The area (width * height)
      
  double perimeter() const
    Calculate the perimeter of the rectangle
    Returns:
      The perimeter (2 * width + 2 * height)
      
  [... more methods listed ...]

Private Attributes:
  double width
    Width of the rectangle in units
    
  double height
    Height of the rectangle in units

Detailed Description:
This class provides basic rectangle operations including 
area calculation, perimeter calculation, and comparison 
operators. All dimensions are stored as double values.

Author: Geometry Library Team
Version: 1.0
```

**Container Template Page (class_geometry_container.html):**
```
Geometry::Container<T> Template Class Reference

A generic container template class for storing and 
managing objects

#include <Container.hpp>

Template Parameters:
  T - The type of elements to store (must support 
      operator< and operator<<)

Public Member Functions:
  Container()
    Default constructor
    
  void add(const T& item)
    Add an item to the container
    Parameters:
      item - The item to add
      
  size_t size() const
    Get the number of items in the container
    Returns:
      The number of items
      
  void sort()
    Sort all items in ascending order
    
  T findMin() const
    Find the minimum element
    Returns:
      The smallest element in the container
    Throws:
      std::runtime_error if container is empty
      
  [... more methods listed ...]

Notes:
- Type T must be comparable (operator<) for sorting 
  and min/max operations
- Type T must support stream output (operator<<) for 
  display operations
```

**File Documentation (Rectangle_8h.html):**
```
Rectangle.h File Reference

#include <iostream>

Classes:
  class Geometry::Rectangle
    A class representing a rectangle with width and height

Namespaces:
  namespace Geometry
```

### Key Features in Generated Documentation

The Doxygen output provides:

1. **Class Hierarchy** - Shows inheritance relationships (if any)
2. **Detailed Member Documentation** - Each method with parameters and return values
3. **Cross-References** - Clickable links between related classes
4. **Search Functionality** - Search bar to find classes, methods, etc.
5. **Source Code Links** - Links to view actual source code
6. **Collaboration Diagrams** - Visual representation of class relationships (if Graphviz is installed)
7. **Namespace Organization** - All classes organized by namespace
8. **Parameter Documentation** - Each parameter explained with @param tags
9. **Exception Documentation** - Lists all exceptions that may be thrown
10. **Template Documentation** - Template parameters clearly documented

### Doxygen Best Practices Summary

* Use `@brief` for short descriptions (shown in lists)
* Use `@param` for each parameter
* Use `@return` to describe return values
* Use `@throw` or `@exception` for exceptions
* Use `@tparam` for template parameters
* Use `@note` and `@warning` for additional information
* Document member variables with `///<` for inline comments
* Keep descriptions concise but complete
* Update documentation when code changes

[Back to Table of Contents](#table-of-contents)

---

## Resources

### Official Documentation
* **C++ Reference**: https://en.cppreference.com/
  * Header files: https://en.cppreference.com/w/cpp/header
* **ISO C++ Standards**: https://isocpp.org/
* **Doxygen Manual**: https://www.doxygen.nl/manual/index.html
* **Doxygen Download**: https://www.doxygen.nl/download.html

### Project Organization
* **Google C++ Style Guide**: https://google.github.io/styleguide/cppguide.html
* **C++ Core Guidelines**: https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines
* **Organizing Code Files**: https://isocpp.org/wiki/faq/coding-standards#organize-files

### Documentation Tools and Resources
* **Doxygen Tags Reference**: https://www.doxygen.nl/manual/commands.html
* **Javadoc Style Guide**: https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html
* **Graphviz** (for Doxygen diagrams): https://graphviz.org/
* **Doxygen Awesome CSS**: https://jothepro.github.io/doxygen-awesome-css/ (styling for documentation)

### Build Systems for Multi-File Projects
* **CMake**: https://cmake.org/ - Cross-platform build system
* **Make Tutorial**: https://makefiletutorial.com/
* **CMake Tutorial**: https://cmake.org/cmake/help/latest/guide/tutorial/index.html

### Practice and Learning
* **LeetCode**: https://leetcode.com/problemset/ - Algorithm practice
* **HackerRank C++**: https://www.hackerrank.com/domains/cpp
* **Exercism C++ Track**: https://exercism.org/tracks/cpp
* **Project Euler**: https://projecteuler.net/ - Mathematical programming challenges

### Community
* **Stack Overflow**: https://stackoverflow.com/questions/tagged/c++
* **Reddit r/cpp**: https://www.reddit.com/r/cpp/
* **C++ Slack**: https://cpplang.slack.com/
* **ISO C++ Forums**: https://isocpp.org/forums

### Further Reading
* **The C++ Programming Language** by Bjarne Stroustrup
* **Effective C++** by Scott Meyers
* **Modern C++ Design** by Andrei Alexandrescu
* **Large-Scale C++ Software Design** by John Lakos
* **API Design for C++** by Martin Reddy

---

[Back to Table of Contents](#table-of-contents)