# Lecture 10: C++ Worksheet

## Part 1: Quiz Section (10-15 minutes)

### Fill-in-the-Blank Questions

**Question 1:** A __________ is a data structure that stores key-value pairs, allowing you to look up values using arbitrary keys rather than numeric indices. In a Binary Search Tree, for any node, all keys in the left subtree are __________ than the node's key, and all keys in the right subtree are __________ than the node's key.

**Word Bank:** greater, dictionary, less, array, sorted, pointer

---

**Question 2:** In a hash table, when two different keys produce the same hash value, we have a __________. This is resolved using __________, where each array position contains a linked list to store all key-value pairs that hash to the same value. A good hash function should distribute keys __________ across the array.

**Word Bank:** uniformly, collision, clustering, chaining, sequentially, bucket

---

**Question 3:** The C++ STL provides two dictionary implementations: __________ which is implemented as a self-balancing Binary Search Tree, and __________ which is implemented as a hash table. The __________ container maintains elements in sorted order by key, while __________ does not maintain any particular order.

**Word Bank:** unordered_map, vector, map, list, map, unordered_map

---

**Question 4:** In a balanced Binary Search Tree, search operations take __________ time, where n is the number of nodes. Hash tables provide __________ average-case time for lookups, but in the worst case when all keys collide, the time complexity degrades to __________.

**Word Bank:** O(n), O(1), O(log n), O(n²), O(log n), O(n log n)

---

### Multiple Choice Questions

**Question 5:** Which of the following scenarios would make `std::map` a better choice than `std::unordered_map`?

A) You need the fastest possible average-case lookup performance  
B) You need to iterate through elements in sorted order by key  
C) You want to minimize memory overhead  
D) Your keys are simple integers

---

**Question 6:** What is the primary advantage of a hash table over a Binary Search Tree?

A) Hash tables always use less memory  
B) Hash tables maintain sorted order  
C) Hash tables provide O(1) average-case access time  
D) Hash tables have better worst-case performance

---

**Question 7:** In the following BST, if you search for the value 14, how many node comparisons will be required?

```
        8
       / \
      3   10
     / \    \
    1   6   14
       / \
      4   7
```

A) 2 comparisons  
B) 3 comparisons  
C) 4 comparisons  
D) 5 comparisons

---

## Part 2: Programming Tasks

### Task 1: Product Inventory System (5-10 minutes)

Create a simple inventory management system using `std::map` that stores product names as keys and their quantities as values. Your program should support adding stock, selling items, and displaying the inventory.

**Requirements:**
- Use `std::map<string, int>` to store products and quantities
- Implement functions to add stock, sell items (decrease quantity), and display inventory
- Handle cases where products don't exist or insufficient stock

**Code Scaffolding:**

```cpp
#include <iostream>
#include <map>
#include <string>

using namespace std;

class Inventory {
private:
    map<string, int> products;
    
public:
    // Add stock to a product (create if doesn't exist)
    void addStock(const string& productName, int quantity) {
        // TODO: Implement this function
    }
    
    // Sell items (decrease quantity)
    bool sellItem(const string& productName, int quantity) {
        // TODO: Implement this function
        // Return true if sale successful, false if insufficient stock or product doesn't exist
    }
    
    // Display all products and quantities
    void displayInventory() const {
        // TODO: Implement this function
    }
};

int main() {
    Inventory inventory;
    
    // Test your implementation
    inventory.addStock("Laptop", 10);
    inventory.addStock("Mouse", 50);
    inventory.addStock("Keyboard", 30);
    
    inventory.displayInventory();
    
    inventory.sellItem("Mouse", 5);
    inventory.sellItem("Laptop", 2);
    
    inventory.displayInventory();
    
    return 0;
}
```

**Sample Output:**
```
=== Current Inventory ===
Keyboard: 30
Laptop: 10
Mouse: 50

Sold 5 Mouse(s)
Sold 2 Laptop(s)

=== Current Inventory ===
Keyboard: 30
Laptop: 8
Mouse: 45
```

---

### Task 2: Character Frequency Analyzer (5-10 minutes)

Write a program using `std::unordered_map` that analyzes a string and counts the frequency of each character (ignoring spaces). Display the results showing each character and how many times it appears.

**Requirements:**
- Use `std::unordered_map<char, int>` to store character frequencies
- Ignore spaces in the count
- Display characters and their frequencies

**Code Scaffolding:**

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

using namespace std;

class CharacterAnalyzer {
private:
    unordered_map<char, int> charFrequency;
    
public:
    // Analyze a string and count character frequencies
    void analyzeString(const string& text) {
        // TODO: Implement this function
        // Hint: Iterate through each character in the string
        // Ignore spaces
    }
    
    // Display character frequencies
    void displayFrequencies() const {
        // TODO: Implement this function
    }
    
    // Get frequency of a specific character
    int getFrequency(char c) const {
        // TODO: Implement this function
    }
};

int main() {
    CharacterAnalyzer analyzer;
    
    string text = "hello world programming";
    cout << "Analyzing: " << text << endl;
    
    analyzer.analyzeString(text);
    analyzer.displayFrequencies();
    
    cout << "\nFrequency of 'o': " << analyzer.getFrequency('o') << endl;
    cout << "Frequency of 'l': " << analyzer.getFrequency('l') << endl;
    
    return 0;
}
```

**Sample Output:**
```
Analyzing: hello world programming
Character Frequencies:
a: 1
d: 1
e: 1
g: 2
h: 1
i: 1
l: 3
m: 2
n: 1
o: 3
p: 1
r: 3
w: 1

Frequency of 'o': 3
Frequency of 'l': 3
```

---

### Challenge Task: Contact Manager with Multiple Lookups (10-15 minutes)

Create a contact management system that allows lookup by both name and phone number. This requires using two separate maps to enable efficient bidirectional lookup.

**Requirements:**
- Use `std::map<string, string>` for name-to-phone lookup
- Use `std::map<string, string>` for phone-to-name lookup
- Keep both maps synchronized when adding or removing contacts
- Implement search by name and search by phone number

**Code Scaffolding:**

```cpp
#include <iostream>
#include <map>
#include <string>

using namespace std;

class ContactManager {
private:
    map<string, string> nameToPhone;  // name -> phone
    map<string, string> phoneToName;  // phone -> name
    
public:
    // Add a new contact (maintain both maps)
    void addContact(const string& name, const string& phone) {
        // TODO: Implement this function
        // Add to both maps
        // Handle case where name or phone already exists
    }
    
    // Find phone number by name
    bool findPhoneByName(const string& name, string& phone) const {
        // TODO: Implement this function
    }
    
    // Find name by phone number
    bool findNameByPhone(const string& phone, string& name) const {
        // TODO: Implement this function
    }
    
    // Remove a contact by name (remove from both maps)
    bool removeContact(const string& name) {
        // TODO: Implement this function
        // Remove from both maps
        // Return true if contact existed and was removed
    }
    
    // Display all contacts
    void displayAllContacts() const {
        // TODO: Implement this function
    }
};

int main() {
    ContactManager contacts;
    
    contacts.addContact("Alice Smith", "555-0123");
    contacts.addContact("Bob Johnson", "555-0456");
    contacts.addContact("Carol White", "555-0789");
    
    contacts.displayAllContacts();
    
    string result;
    cout << "\n--- Search by Name ---" << endl;
    if (contacts.findPhoneByName("Bob Johnson", result)) {
        cout << "Bob Johnson's phone: " << result << endl;
    }
    
    cout << "\n--- Search by Phone ---" << endl;
    if (contacts.findNameByPhone("555-0789", result)) {
        cout << "555-0789 belongs to: " << result << endl;
    }
    
    cout << "\n--- After Removing Alice Smith ---" << endl;
    contacts.removeContact("Alice Smith");
    contacts.displayAllContacts();
    
    return 0;
}
```

**Sample Output:**
```
=== All Contacts ===
Alice Smith: 555-0123
Bob Johnson: 555-0456
Carol White: 555-0789

--- Search by Name ---
Bob Johnson's phone: 555-0456

--- Search by Phone ---
555-0789 belongs to: Carol White

--- After Removing Alice Smith ---
=== All Contacts ===
Bob Johnson: 555-0456
Carol White: 555-0789
```

---

