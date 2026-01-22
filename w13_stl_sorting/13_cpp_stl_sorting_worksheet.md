# Lecture 13: C++ Worksheet

## Section 1: Quiz Questions

**Time Allocation: 10-15 minutes**

### Question 1

Complete the following statement about STL sort:

The `std::sort` algorithm has an average and worst-case time complexity of __________ in modern implementations. It requires __________ iterators to work, which means it cannot be used directly with __________. Instead, these containers must use their own __________ method.

**Word Bank (scrambled):** member, O(n log n), lists, random access

---

### Question 2

Fill in the blanks about operator overloading:

When overloading the less-than operator for use with `std::sort`, the function signature should be `bool operator<(const ClassName& rhs) __________`. The first `const` keyword ensures the __________ is not modified, while the second `const` keyword ensures the __________ the method is called on is not modified. This is important for __________ correctness.

**Word Bank (scrambled):** object, const, parameter, const

---

### Question 3

Complete this statement about function objects:

A function object (or functor) is a class that overloads the __________ operator. Function objects can hold __________ variables, making them more flexible than regular functions. The STL provides `std::greater<T>()` which is a function object that compares elements using the __________ operator instead of the __________ operator.

**Word Bank (scrambled):** greater-than, member, less-than, function call

---

### Question 4

Fill in the blanks about strict weak ordering:

For a comparison function to work correctly with `std::sort`, it must satisfy __________ ordering. This means the comparison must be irreflexive (comp(a,a) must be __________), antisymmetric, and __________. Violating these rules can cause __________ behavior.

**Word Bank (scrambled):** transitive, strict weak, undefined, false

---

### Question 5 (Multiple Choice)

What happens if you try to use `std::sort` directly on a `std::list` container?

A) It works but is slower than the list's member sort function  
B) It causes a compilation error because lists don't have random access iterators  
C) It works but produces incorrect results  
D) It automatically converts the list to a vector first

---

### Question 6 (Multiple Choice)

Given this code:
```cpp
class Product {
    string name;
    double price;
public:
    Product(string n, double p) : name(n), price(p) {}
    bool operator<(const Product& rhs) const {
        return price < rhs.price;
    }
};
```

If you sort a vector of Products, how will they be ordered?

A) By name alphabetically  
B) By price from lowest to highest  
C) By price from highest to lowest  
D) The code will not compile

---

### Question 7 (Multiple Choice)

Which of the following statements about function objects is FALSE?

A) Function objects can maintain state through member variables  
B) Function objects are typically more efficient than function pointers due to inlining  
C) Function objects must always be used instead of overloading operator<  
D) Function objects allow you to pass parameters to configure sorting behavior

---

## Section 2: Programming Tasks

**Time Allocation: 5-10 minutes per task**

### Task 1: Book Sorting System

Create a `Book` class that stores a title (string), author (string), and publication year (int). Implement the less-than operator to sort books by publication year (oldest first). If two books have the same year, use the title as a tiebreaker (alphabetically).

**Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

class Book {
private:
    std::string title;
    std::string author;
    int year;

public:
    Book(std::string t, std::string a, int y) 
        : title(t), author(a), year(y) {}
    
    // TODO: Implement operator< for sorting
    
    void display() const {
        std::cout << year << ": " << title 
                  << " by " << author << std::endl;
    }
};

int main() {
    std::vector<Book> library;
    library.push_back(Book("1984", "Orwell", 1949));
    library.push_back(Book("Brave New World", "Huxley", 1932));
    library.push_back(Book("Animal Farm", "Orwell", 1945));
    library.push_back(Book("Fahrenheit 451", "Bradbury", 1953));
    library.push_back(Book("The Hobbit", "Tolkien", 1937));
    
    std::cout << "Original order:" << std::endl;
    for (int i = 0; i < library.size(); ++i) {
        library[i].display();
    }
    
    // TODO: Sort the library
    
    std::cout << "\nSorted by year:" << std::endl;
    for (int i = 0; i < library.size(); ++i) {
        library[i].display();
    }
    
    return 0;
}
```

**Expected Output:**
```
Original order:
1949: 1984 by Orwell
1932: Brave New World by Huxley
1945: Animal Farm by Orwell
1953: Fahrenheit 451 by Bradbury
1937: The Hobbit by Tolkien

Sorted by year:
1932: Brave New World by Huxley
1937: The Hobbit by Tolkien
1945: Animal Farm by Orwell
1949: 1984 by Orwell
1953: Fahrenheit 451 by Bradbury
```

---

### Task 2: Temperature Sorting

Create a function object called `ClosestToRoomTemp` that sorts temperatures (integers in Celsius) by their distance from room temperature (20 degrees Celsius). Use this to sort a vector of temperature readings.

**Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>

class ClosestToRoomTemp {
public:
    // TODO: Implement the function call operator
    // Room temperature is 20 degrees Celsius
};

int main() {
    std::vector<int> temperatures;
    temperatures.push_back(15);
    temperatures.push_back(25);
    temperatures.push_back(18);
    temperatures.push_back(30);
    temperatures.push_back(20);
    temperatures.push_back(22);
    temperatures.push_back(10);
    
    std::cout << "Original temperatures: ";
    for (int i = 0; i < temperatures.size(); ++i) {
        std::cout << temperatures[i] << "C ";
    }
    std::cout << std::endl;
    
    // TODO: Sort using the function object
    
    std::cout << "Sorted by distance from room temp (20C): ";
    for (int i = 0; i < temperatures.size(); ++i) {
        std::cout << temperatures[i] << "C ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Expected Output:**
```
Original temperatures: 15C 25C 18C 30C 20C 22C 10C 
Sorted by distance from room temp (20C): 20C 18C 22C 15C 25C 10C 30C
```

---

### Challenge Task: Multi-Criteria Student Ranking

Create a `Student` class with name, grade (0-100), and attendance percentage (0-100). Create TWO function objects:

1. `RankByPerformance` - Sorts students by a combined performance score: 70% grade + 30% attendance
2. `RankByImprovement` - Takes a "previous grade" parameter in its constructor, and sorts students by how much they've improved (current grade - previous grade)

**Scaffolding:**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

class Student {
private:
    std::string name;
    double grade;
    double attendance;

public:
    Student(std::string n, double g, double a) 
        : name(n), grade(g), attendance(a) {}
    
    std::string getName() const { return name; }
    double getGrade() const { return grade; }
    double getAttendance() const { return attendance; }
    
    void display() const {
        std::cout << name << " (Grade: " << grade 
                  << ", Attendance: " << attendance << "%)" << std::endl;
    }
};

class RankByPerformance {
public:
    // TODO: Implement operator() to rank by: 70% grade + 30% attendance
};

class RankByImprovement {
private:
    // TODO: Add member variable for previous grade
public:
    // TODO: Add constructor to set previous grade
    
    // TODO: Implement operator() to rank by improvement
};

int main() {
    std::vector<Student> students;
    students.push_back(Student("Alice", 85, 95));
    students.push_back(Student("Bob", 92, 80));
    students.push_back(Student("Charlie", 78, 100));
    students.push_back(Student("Diana", 88, 90));
    
    std::cout << "Original order:" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
    }
    
    // Sort by performance
    // TODO: Implement sorting
    
    std::cout << "\nRanked by performance (70% grade + 30% attendance):" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
    }
    
    // Sort by improvement (Bob's previous grade was 75, others had 80)
    // TODO: Implement sorting with RankByImprovement(80)
    
    std::cout << "\nRanked by improvement from previous grade of 80:" << std::endl;
    for (int i = 0; i < students.size(); ++i) {
        students[i].display();
    }
    
    return 0;
}
```

**Expected Output:**
```
Original order:
Alice (Grade: 85, Attendance: 95%)
Bob (Grade: 92, Attendance: 80%)
Charlie (Grade: 78, Attendance: 100%)
Diana (Grade: 88, Attendance: 90%)

Ranked by performance (70% grade + 30% attendance):
Bob (Grade: 92, Attendance: 80%)
Diana (Grade: 88, Attendance: 90%)
Alice (Grade: 85, Attendance: 95%)
Charlie (Grade: 78, Attendance: 100%)

Ranked by improvement from previous grade of 80:
Bob (Grade: 92, Attendance: 80%)
Diana (Grade: 88, Attendance: 90%)
Alice (Grade: 85, Attendance: 95%)
Charlie (Grade: 78, Attendance: 100%)
```
