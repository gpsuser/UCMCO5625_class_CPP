# Extra notes on Sorting

Below is a minimal implementations of Functors in C++.

Recall that a `functor` is a class or struct that overloads the `operator()`, allowing instances of the class to be called like functions. Functors are often used in the C++ Standard Template Library (STL) for custom sorting, comparisons, and other operations.

Essentially a `functor` is just an object with operator() — used to inject behavior.

Recall a `struct` is a minimal class object where members are public by default.

## Functor - minimal example

```
// minimalfunctor.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include <iostream>
#include <algorithm>
#include <vector>
#include <string>

struct Person {
    std::string name;
    int age;
};

struct CompareByAge {
    bool operator()(const Person& a, const Person& b) const {
        return a.age < b.age;
    }
};

int main() {
    std::vector<Person> people = {
        {"Alice", 30},
        {"Bob", 25},
        {"Carol", 40}
    };

    std::sort(people.begin(), people.end(), CompareByAge{});

    for (const auto& p : people) {
        std::cout << p.name << " (" << p.age << ")\n";
    }

    return 0;
}

```