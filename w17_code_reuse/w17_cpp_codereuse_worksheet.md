# C++ Worksheet: Code Organization and Documentation

## Quiz Section

### Question 1

Complete the following statement by filling in the blanks using words from the word bank below:

"The __________ directive prevents multiple inclusion of the same header file. When using traditional include guards, you would use __________, __________, and __________ preprocessor directives to wrap the header content."

**Word Bank:** `#endif`, `#define`, `#pragma once`, `#ifndef`, `#include`, `namespace`

---

### Question 2

Fill in the blanks using words from the word bank:

"Template code must be placed entirely in __________ files because the compiler needs to see the full __________ when instantiating a template. This is different from regular classes where the __________ goes in .h files and the __________ goes in .cpp files."

**Word Bank:** `.hpp`, `implementation`, `declaration`, `namespace`, `documentation`, `template`

---

### Question 3

Complete the following statement using words from the word bank:

"In Doxygen comments, the __________ tag provides a short description, the __________ tag describes function parameters, and the __________ tag documents template type parameters. The __________ tag describes what exceptions might be thrown."

**Word Bank:** `@param`, `@brief`, `@throw`, `@return`, `@tparam`, `@class`

---

### Question 4

Fill in the blanks using words from the word bank:

"To use code from a namespace, you can use __________ names like `std::vector`, a __________ declaration like `using std::vector`, or a __________ directive like `using namespace std`. However, you should avoid using __________ directives in header files."

**Word Bank:** `qualified`, `using`, `namespace`, `fully`, `inline`, `friend`

---

### Question 5

When including header files in C++, which statement is correct?

A) Angle brackets `< >` are used for project files, quotes `" "` are used for system libraries  
B) Angle brackets `< >` are used for system libraries, quotes `" "` are used for project files  
C) Both angle brackets and quotes can be used interchangeably  
D) Quotes search only in the current directory and never in system directories

---

### Question 6

Which of the following is the PRIMARY reason templates cannot be separated into .h and .cpp files?

A) Templates are too complex for the linker to handle  
B) The compiler must see the full implementation when instantiating templates with specific types  
C) .cpp files cannot contain template code  
D) Templates always use inline functions

---

### Question 7

What is the best practice for using namespaces in reusable header files?

A) Always use `using namespace std;` at the top of header files  
B) Never use namespaces in header files  
C) Avoid `using namespace` directives in headers to prevent polluting the global namespace  
D) Use `using namespace` for all custom namespaces but not for std

---

## Task Section

### Task 1: Temperature Class with File Separation

Create a `Temperature` class that stores temperature in Celsius and provides conversion to Fahrenheit. Organize the code using proper header/implementation file separation.

**Requirements:**
- Create `Temperature.h` with the class declaration
- Create `Temperature.cpp` with the implementation
- Include proper include guards
- Implement constructors, getter methods, and a conversion method
- Test in main.cpp

**Code Scaffolding:**

**Temperature.h:**
```cpp
#pragma once
#include <iostream>

class Temperature
{
private:
    double celsius;
    
public:
    // Default constructor (0 degrees C)
    Temperature();
    
    // Constructor with initial temperature
    Temperature(double c);
    
    // Get celsius value
    double getCelsius() const;
    
    // Get fahrenheit value
    double getFahrenheit() const;
    
    // Set celsius value
    void setCelsius(double c);
    
    // Overload << operator for output
    friend std::ostream& operator<<(std::ostream& os, const Temperature& t);
};
```

**Temperature.cpp:**
```cpp
#include "Temperature.h"

// TODO: Implement default constructor

// TODO: Implement parameterized constructor

// TODO: Implement getCelsius()

// TODO: Implement getFahrenheit() 
// Formula: F = C * 9/5 + 32

// TODO: Implement setCelsius()

// TODO: Implement operator<< to output in format "X°C (Y°F)"
```

**main.cpp:**
```cpp
#include <iostream>
#include "Temperature.h"

int main()
{
    // TODO: Create temperature objects
    // TODO: Test all methods
    // TODO: Display temperatures
    
    return 0;
}
```

**Sample Output:**
```
0°C (32°F)
25°C (77°F)
100°C (212°F)
-40°C (-40°F)
```

---

### Task 2: Generic Maximum Template with Documentation

Create a documented template function that finds the maximum of three values using Doxygen-style comments.

**Requirements:**
- Create a template function in a .hpp file
- Add complete Doxygen documentation
- The function should work with any comparable type
- Test with at least two different types

**Code Scaffolding:**

**MaxFinder.hpp:**
```cpp
#pragma once

/**
 * TODO: Add @brief tag describing the function
 * 
 * TODO: Add full description
 * 
 * TODO: Add @tparam tag for template parameter T
 * TODO: Add @param tags for a, b, c parameters
 * TODO: Add @return tag
 * TODO: Add @note about type requirements
 */
template <typename T>
T findMaxOfThree(const T& a, const T& b, const T& c)
{
    // TODO: Implement logic to find maximum of three values
    // Hint: Use nested comparisons or find max of (a, max(b, c))
}
```

**main.cpp:**
```cpp
#include <iostream>
#include <string>
#include "MaxFinder.hpp"

int main()
{
    // TODO: Test with integers
    
    // TODO: Test with doubles
    
    // TODO: Test with strings (alphabetical comparison)
    
    return 0;
}
```

**Sample Output:**
```
Maximum of 5, 12, 8: 12
Maximum of 3.14, 2.71, 9.8: 9.8
Maximum of apple, zebra, mango: zebra
```

---

### Task 3: Complete Statistics Library (Challenge)

Create a small statistics library with proper organization: namespace, regular class, template class, and full Doxygen documentation.

**Requirements:**
- Create a `Stats` namespace
- Implement a `Dataset` class (regular class with .h/.cpp separation) that stores a name and description
- Implement a `Calculator` template class (in .hpp) that performs statistical calculations on containers of numeric data
- Add complete Doxygen documentation for all classes
- Test both classes together in main.cpp

**Code Scaffolding:**

**Dataset.h:**
```cpp
#pragma once
#include <string>

namespace Stats {

/**
 * TODO: Add class documentation
 */
class Dataset
{
private:
    std::string name;
    std::string description;
    
public:
    // TODO: Add documentation
    Dataset();
    
    // TODO: Add documentation
    Dataset(const std::string& n, const std::string& desc);
    
    // TODO: Add documentation
    std::string getName() const;
    
    // TODO: Add documentation
    std::string getDescription() const;
    
    // TODO: Add documentation
    void display() const;
};

} // namespace Stats
```

**Dataset.cpp:**
```cpp
#include "Dataset.h"
#include <iostream>

namespace Stats {

// TODO: Implement all Dataset methods

} // namespace Stats
```

**Calculator.hpp:**
```cpp
#pragma once
#include <vector>
#include <numeric>
#include <algorithm>
#include <stdexcept>

namespace Stats {

/**
 * TODO: Add template class documentation
 * TODO: Document template parameter
 */
template <typename T>
class Calculator
{
private:
    std::vector<T> data;
    
public:
    /**
     * TODO: Add method documentation
     */
    Calculator() {}
    
    /**
     * TODO: Add method documentation
     */
    void addValue(T value)
    {
        data.push_back(value);
    }
    
    /**
     * TODO: Add method documentation
     * Calculates mean = sum / count
     */
    double mean() const
    {
        // TODO: Implement
        // Throw exception if data is empty
    }
    
    /**
     * TODO: Add method documentation
     * Returns minimum value in dataset
     */
    T min() const
    {
        // TODO: Implement using std::min_element
        // Throw exception if data is empty
    }
    
    /**
     * TODO: Add method documentation
     * Returns maximum value in dataset
     */
    T max() const
    {
        // TODO: Implement using std::max_element
        // Throw exception if data is empty
    }
    
    /**
     * TODO: Add method documentation
     */
    size_t count() const
    {
        return data.size();
    }
};

} // namespace Stats
```

**main.cpp:**
```cpp
#include <iostream>
#include "Dataset.h"
#include "Calculator.hpp"

using namespace Stats;

int main()
{
    // TODO: Create a Dataset object with name and description
    
    // TODO: Create a Calculator<int> object
    
    // TODO: Add several values to calculator
    
    // TODO: Display dataset info
    
    // TODO: Display statistics (count, min, max, mean)
    
    // TODO: Test with Calculator<double> for decimal values
    
    return 0;
}
```

**Sample Output:**
```
Dataset: Temperature Readings
Description: Daily temperatures for January

Statistics:
Count: 5
Minimum: 15
Maximum: 28
Mean: 21.2

Dataset: Rainfall Measurements
Description: Monthly rainfall in mm

Statistics:
Count: 4
Minimum: 23.5
Maximum: 87.3
Mean: 51.575
```