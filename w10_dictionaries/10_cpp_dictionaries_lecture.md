# CO5625: Dictionary Types in C++

## Table of Contents

1. [Learning Outcomes](#learning-outcomes)
2. [Section 1: Introduction to Dictionary Data Structures](#section-1-introduction-to-dictionary-data-structures)
3. [Section 2: Binary Search Trees](#section-2-binary-search-trees)
4. [Section 3: Hash Tables](#section-3-hash-tables)
5. [Section 4: STL Dictionary Implementations](#section-4-stl-dictionary-implementations)
6. [Section 5: Practical Applications and Examples](#section-5-practical-applications-and-examples)
7. [Section 6: Choosing Between map and unordered_map](#section-6-choosing-between-map-and-unordered_map)
8. [Resources for Further Learning](#resources-for-further-learning)

---

## Learning Outcomes

By the end of this lecture, students should be able to:

1. **Explain** the limitations of array-based indexing and the need for dictionary data structures
2. **Describe** the fundamental differences between Binary Search Trees and Hash Tables as dictionary implementations
3. **Implement** basic programs using C++ STL `map` (BST-based) and `unordered_map` (hash table-based) containers
4. **Analyze** the time complexity characteristics of BST and hash table operations
5. **Evaluate** and select the appropriate dictionary structure based on specific use case requirements
6. **Create** practical applications using dictionary types, such as telephone directories or word frequency counters

---

## Section 1: Introduction to Dictionary Data Structures

### The Problem with Traditional Arrays

Arrays and vectors in C++ provide excellent performance for accessing elements by numeric index. They allow us to store and retrieve data using integers from 0 to (n-1), where n is the number of elements.

```
Array Indexing:
┌───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │  ← indices
├───┼───┼───┼───┼───┼───┼───┤
│ A │ B │ C │ D │ E │ F │ G │  ← data
└───┴───┴───┴───┴───┴───┴───┘
```

However, this indexing scheme has significant limitations when we want to look up data using:
- Names or strings (e.g., looking up a person's phone number by their name)
- Non-continuous numeric ranges (e.g., employee IDs that aren't sequential)
- Any other non-numeric keys

### What is a Dictionary?

A **dictionary** (also called an associative array or map) is a data structure that stores key-value pairs, allowing you to look up values using arbitrary keys rather than numeric indices.

```
Dictionary Concept:
┌──────────────┬─────────────┐
│     Key      │    Value    │
├──────────────┼─────────────┤
│ "Alice"      │ "555-1234"  │
│ "Bob"        │ "555-5678"  │
│ "Charlie"    │ "555-9012"  │
└──────────────┴─────────────┘
```

The ideal syntax would look like this:

```cpp
telephoneNumber["Mary Johnson"] = "01244 654356";
string number = telephoneNumber["Mary Johnson"];
```

### Two Implementation Approaches

There are two fundamental ways to implement dictionary structures:

1. **Binary Search Tree (BST)** - Organizes data in a sorted tree structure
2. **Hash Table** - Uses a hash function to map keys to array positions

Each approach has different performance characteristics and use cases.

---

## Section 2: Binary Search Trees

### What is a Binary Search Tree?

A Binary Search Tree is a hierarchical data structure where each node contains a key-value pair and has at most two children. The tree maintains a sorted order: for any node, all keys in the left subtree are less than the node's key, and all keys in the right subtree are greater.

```
Basic BST Structure:
                    ┌───┐
                    │ 8 │  ← root
                    └─┬─┘
           ┌──────────┴──────────┐
         ┌─┴─┐                 ┌─┴──┐
         │ 3 │                 │ 10 │
         └─┬─┘                 └─┬──┘
      ┌────┴────┐           ┌────┴────┐
    ┌─┴─┐     ┌─┴─┐       ┌─┴──┐    ┌─┴──┐
    │ 1 │     │ 6 │       │ 14 │    │    │
    └───┘     └─┬─┘       └────┘    └────┘
            ┌───┴───┐
          ┌─┴─┐   ┌─┴─┐
          │ 4 │   │ 7 │
          └───┘   └───┘
```

### BST Node Structure

Each node in a BST is similar to a linked list node, but with two "next" pointers instead of one:

```
BST Node:
┌─────────────────┐
│  key: value     │
│  ┌───┐   ┌───┐  │
│  │ * │   │ * │  │  * = pointer
│  └─┬─┘   └─┬─┘  │
└────┼───────┼────┘
     │       │
  left child  right child
```

### Searching in a BST

To find a key k in a BST:

1. Start at the root node
2. Compare k with the current node's key
3. If k equals the current key, we found it
4. If k is less than the current key, move to the left child
5. If k is greater than the current key, move to the right child
6. Repeat until found or reach a null pointer (key not in tree)

**Complete BST Search Example:**

```cpp
#include <iostream>
#include <memory>
#include <string>

using namespace std;

// Node structure for BST
template <typename KeyType, typename ValueType>
struct BSTNode {
    KeyType key;
    ValueType value;
    shared_ptr<BSTNode> left;
    shared_ptr<BSTNode> right;
    
    BSTNode(KeyType k, ValueType v) : key(k), value(v), left(nullptr), right(nullptr) {}
};

// Simple BST class
template <typename KeyType, typename ValueType>
class SimpleBST {
private:
    shared_ptr<BSTNode<KeyType, ValueType>> root;
    
    // Helper function to search recursively
    shared_ptr<BSTNode<KeyType, ValueType>> searchHelper(
        shared_ptr<BSTNode<KeyType, ValueType>> node, KeyType key) {
        
        if (node == nullptr) {
            return nullptr;  // Key not found
        }
        
        if (key == node->key) {
            return node;  // Found it
        } else if (key < node->key) {
            return searchHelper(node->left, key);  // Search left
        } else {
            return searchHelper(node->right, key);  // Search right
        }
    }
    
public:
    SimpleBST() : root(nullptr) {}
    
    // Simple insert (without balancing)
    void insert(KeyType key, ValueType value) {
        if (root == nullptr) {
            root = make_shared<BSTNode<KeyType, ValueType>>(key, value);
            return;
        }
        
        auto current = root;
        while (true) {
            if (key < current->key) {
                if (current->left == nullptr) {
                    current->left = make_shared<BSTNode<KeyType, ValueType>>(key, value);
                    break;
                }
                current = current->left;
            } else if (key > current->key) {
                if (current->right == nullptr) {
                    current->right = make_shared<BSTNode<KeyType, ValueType>>(key, value);
                    break;
                }
                current = current->right;
            } else {
                // Key already exists, update value
                current->value = value;
                break;
            }
        }
    }
    
    // Search for a key
    bool search(KeyType key, ValueType& result) {
        auto node = searchHelper(root, key);
        if (node != nullptr) {
            result = node->value;
            return true;
        }
        return false;
    }
};

int main() {
    SimpleBST<string, string> bst;
    
    // Build a BST with names and animals
    cout << "Building BST with name-animal pairs..." << endl;
    bst.insert("DAWN", "sunrise");
    bst.insert("DAVE", "person");
    bst.insert("MIKE", "name");
    bst.insert("BETH", "name");
    bst.insert("DAVID", "king");
    bst.insert("GINA", "name");
    bst.insert("PAT", "name");
    bst.insert("CINDI", "name");
    bst.insert("SUE", "name");
    
    // Search for keys
    string result;
    cout << "\nSearching for 'DAVE': ";
    if (bst.search("DAVE", result)) {
        cout << "Found - Value: " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    cout << "Searching for 'GINA': ";
    if (bst.search("GINA", result)) {
        cout << "Found - Value: " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    cout << "Searching for 'JOHN': ";
    if (bst.search("JOHN", result)) {
        cout << "Found - Value: " << result << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
Building BST with name-animal pairs...

Searching for 'DAVE': Found - Value: person
Searching for 'GINA': Found - Value: name
Searching for 'JOHN': Not found
```

### BST with Different Key Types

BSTs can use any data type as a key, as long as that type supports comparison operations (less than, greater than, equality). This makes them highly versatile for creating dictionary structures.

### BST Performance and Depth

The efficiency of BST operations depends critically on the **depth** (or height) of the tree, not the number of items stored.

```
Balanced BST (Best Case):
          ┌─────┐
          │ the │
          └──┬──┘
     ┌───────┴───────┐
   ┌─┴──┐         ┌──┴─┐
   │ it │         │ was│
   └─┬──┘         └──┬─┘
  ┌──┴──┐         ┌──┴──┐
┌─┴─┐ ┌─┴─┐     ┌─┴─┐ ┌─┴──┐
│best│of │     │times│worst│
└───┘ └───┘     └────┘ └────┘

Depth = 3, Nodes = 7
Time complexity: O(log n)


Unbalanced BST (Worst Case):
┌──────────┐
│ alligator│
└────┬─────┘
     │
  ┌──┴──┐
  │ bat │
  └──┬──┘
     │
  ┌──┴───┐
  │cobra │
  └──┬───┘
     │
   ┌─┴─┐
   │dog│
   └─┬─┘
     │
   ┌─┴──┐
   │fox │
   └─┬──┘
      │
   ┌──┴───┐
   │horse │
   └──┬───┘
      
Depth = 6+, Nodes = 7
Time complexity: O(n)
```

**Key Points about BST Depth:**

- **Best case:** When perfectly balanced, depth grows as O(log n) where n is the number of nodes
- **Worst case:** When completely unbalanced (essentially a linked list), depth equals n
- **Self-balancing BSTs:** Modern implementations automatically rebalance after insertions and deletions to maintain O(log n) depth
- **Operations complexity:** Search, insert, and delete all take O(log n) time in a balanced BST

### BST as a Compromise

BSTs represent a middle ground between arrays and linked lists:

- **Arrays:** O(1) access, but O(n) insertion/deletion
- **Linked lists:** O(1) insertion/deletion at known position, but O(n) search
- **BSTs:** O(log n) for search, insertion, AND deletion (when balanced)

This makes BSTs highly efficient for applications requiring frequent insertions, deletions, and lookups.

---

## Section 3: Hash Tables

### What is a Hash Table?

A hash table is fundamentally an array of linked lists. It uses a **hash function** to convert keys into array indices, providing very fast average-case access.

```
Hash Table Structure:

Keys          Hash Function       Table (Array)      Linked Lists (Chains)
              (converts key       (fixed size)
               to index)

"Jack       ───────────►  0  ─────►  
Williams"                         

"Sam        ───────────►  1  ─────► [Sam Taylor│486-25-13]
Taylor"                           

                          2  ─────► [Jack Williams│779-64-52] → [Andrew Wilson│576-31-87]

"Sandra     ───────────►  3  ─────► [Sandra Miller│254-63-56]
Miller"                           

"Mattew     ───────────►  4  ─────► [Mattew Davis│183-74-90]
Davis"                            

"Andrew     ───────────►  2  (collision! added to chain at index 2)
Wilson"
```

### Hash Table Operations

**Insertion:**
1. Apply hash function to the key to get an index k
2. Insert the key-value pair into the linked list at array position k
3. Time complexity: O(1) average case

**Lookup:**
1. Apply hash function to the key to get index k
2. Search through the linked list at array position k for the key
3. Time complexity: O(1) average case, O(n) worst case

**Collision Handling:**

When two different keys hash to the same index, we have a collision. The linked list at that index stores all key-value pairs that hash to the same value. This is called **chaining**.

### Hash Function Characteristics

A good hash function should:
- Distribute keys uniformly across the array (minimize collisions)
- Be deterministic (same key always produces same hash value)
- Be fast to compute
- Minimize clustering of similar keys

```
Good Hash Distribution:          Bad Hash Distribution:
┌──┐                             ┌──┐
│  │                             │  │
├──┤                             ├──┤
│*─┼──►[A]                       │  │
├──┤                             ├──┤
│*─┼──►[B]                       │  │
├──┤                             ├──┤
│*─┼──►[C]                       │**┼──►[A]→[B]→[C]→[D]→[E]→[F]
├──┤                             ├──┤
│*─┼──►[D]                       │  │
├──┤                             ├──┤
│*─┼──►[E]                       │  │
├──┤                             ├──┤
│*─┼──►[F]                       │  │
└──┘                             └──┘
Good spread = fast lookups       Clustering = slow lookups
```

### Hash Table Performance

**Efficiency characteristics:**

- **Insertion:** Always O(1) - hash the key and add to linked list
- **Access/Removal:** 
  - Average case: O(1) when keys are well-distributed
  - Worst case: O(n) when all keys hash to the same value (all in one linked list)
- **Space complexity:** O(n + m) where n is the number of items and m is the array size

The worst case occurs when the hash function causes all keys to collide, effectively turning the hash table into a single linked list.

**Complete Hash Table Example:**

```cpp
#include <iostream>
#include <list>
#include <vector>
#include <string>
#include <functional>

using namespace std;

// Simple hash table implementation
template <typename KeyType, typename ValueType>
class SimpleHashTable {
private:
    struct KeyValuePair {
        KeyType key;
        ValueType value;
        
        KeyValuePair(KeyType k, ValueType v) : key(k), value(v) {}
    };
    
    vector<list<KeyValuePair>> table;
    size_t tableSize;
    
    // Hash function - converts key to index
    size_t hash(const KeyType& key) const {
        hash<KeyType> hasher;
        return hasher(key) % tableSize;
    }
    
public:
    SimpleHashTable(size_t size = 10) : tableSize(size) {
        table.resize(tableSize);
    }
    
    // Insert or update a key-value pair
    void insert(const KeyType& key, const ValueType& value) {
        size_t index = hash(key);
        
        // Check if key already exists
        for (auto& pair : table[index]) {
            if (pair.key == key) {
                pair.value = value;  // Update existing
                return;
            }
        }
        
        // Key doesn't exist, add new pair
        table[index].push_back(KeyValuePair(key, value));
    }
    
    // Search for a key
    bool search(const KeyType& key, ValueType& result) const {
        size_t index = hash(key);
        
        for (const auto& pair : table[index]) {
            if (pair.key == key) {
                result = pair.value;
                return true;
            }
        }
        
        return false;
    }
    
    // Remove a key
    bool remove(const KeyType& key) {
        size_t index = hash(key);
        
        auto& chain = table[index];
        for (auto it = chain.begin(); it != chain.end(); ++it) {
            if (it->key == key) {
                chain.erase(it);
                return true;
            }
        }
        
        return false;
    }
    
    // Display hash table contents (for demonstration)
    void display() const {
        for (size_t i = 0; i < tableSize; ++i) {
            cout << "Bucket " << i << ": ";
            if (table[i].empty()) {
                cout << "(empty)";
            } else {
                for (const auto& pair : table[i]) {
                    cout << "[" << pair.key << ":" << pair.value << "] ";
                }
            }
            cout << endl;
        }
    }
};

int main() {
    SimpleHashTable<string, string> phoneBook(5);
    
    cout << "Building hash table phone book..." << endl;
    phoneBook.insert("Sam Taylor", "486-25-13");
    phoneBook.insert("Jack Williams", "779-64-52");
    phoneBook.insert("Andrew Wilson", "576-31-87");
    phoneBook.insert("Sandra Miller", "254-63-56");
    phoneBook.insert("Mattew Davis", "183-74-90");
    
    cout << "\nHash table contents:" << endl;
    phoneBook.display();
    
    // Search for entries
    string number;
    cout << "\nSearching for 'Sam Taylor': ";
    if (phoneBook.search("Sam Taylor", number)) {
        cout << "Found - Phone: " << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    cout << "Searching for 'Sandra Miller': ";
    if (phoneBook.search("Sandra Miller", number)) {
        cout << "Found - Phone: " << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    cout << "Searching for 'John Doe': ";
    if (phoneBook.search("John Doe", number)) {
        cout << "Found - Phone: " << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Remove an entry
    cout << "\nRemoving 'Jack Williams'..." << endl;
    phoneBook.remove("Jack Williams");
    
    cout << "\nHash table after removal:" << endl;
    phoneBook.display();
    
    return 0;
}
```

**Sample Output:**
```
Building hash table phone book...

Hash table contents:
Bucket 0: [Mattew Davis:183-74-90] 
Bucket 1: [Sandra Miller:254-63-56] 
Bucket 2: [Andrew Wilson:576-31-87] 
Bucket 3: [Sam Taylor:486-25-13] 
Bucket 4: [Jack Williams:779-64-52] 

Searching for 'Sam Taylor': Found - Phone: 486-25-13
Searching for 'Sandra Miller': Found - Phone: 254-63-56
Searching for 'John Doe': Not found

Removing 'Jack Williams'...

Hash table after removal:
Bucket 0: [Mattew Davis:183-74-90] 
Bucket 1: [Sandra Miller:254-63-56] 
Bucket 2: [Andrew Wilson:576-31-87] 
Bucket 3: [Sam Taylor:486-25-13] 
Bucket 4: (empty)
```

---

## Section 4: STL Dictionary Implementations

### C++ Standard Library Dictionaries

The C++ Standard Template Library (STL) provides two dictionary implementations:

1. **`map`** - Implemented as a self-balancing Binary Search Tree (typically Red-Black Tree)
2. **`unordered_map`** - Implemented as a Hash Table

This explicit distinction is superior to many other programming languages where the implementation details are hidden.

### Using std::map (Binary Search Tree)

The `map` container stores elements in sorted order based on the key.

**Complete map Example:**

```cpp
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main() {
    // Create a map with string keys and string values
    map<string, string> animalMap;
    
    cout << "=== std::map Example (BST Implementation) ===" << endl;
    cout << "\nInserting key-value pairs..." << endl;
    
    // Insert elements
    animalMap["fred"] = "horse";
    animalMap["jemima"] = "dog";
    animalMap["arthur"] = "sheep";
    animalMap["zebra"] = "striped";
    animalMap["alice"] = "person";
    
    // Access elements
    cout << "\nAccessing 'jemima': " << animalMap["jemima"] << endl;
    cout << "Accessing 'fred': " << animalMap["fred"] << endl;
    
    // Display all elements (note: sorted by key)
    cout << "\nAll entries (notice alphabetical order):" << endl;
    for (const auto& pair : animalMap) {
        cout << "  " << pair.first << " -> " << pair.second << endl;
    }
    
    // Check if key exists
    string searchKey = "arthur";
    cout << "\nSearching for '" << searchKey << "': ";
    if (animalMap.find(searchKey) != animalMap.end()) {
        cout << "Found - Value: " << animalMap[searchKey] << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Size and empty check
    cout << "\nMap size: " << animalMap.size() << endl;
    cout << "Map is empty: " << (animalMap.empty() ? "yes" : "no") << endl;
    
    // Erase an element
    cout << "\nErasing 'zebra'..." << endl;
    animalMap.erase("zebra");
    
    cout << "Remaining entries:" << endl;
    for (const auto& pair : animalMap) {
        cout << "  " << pair.first << " -> " << pair.second << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
=== std::map Example (BST Implementation) ===

Inserting key-value pairs...

Accessing 'jemima': dog
Accessing 'fred': horse

All entries (notice alphabetical order):
  alice -> person
  arthur -> sheep
  fred -> horse
  jemima -> dog
  zebra -> striped

Searching for 'arthur': Found - Value: sheep

Map size: 5
Map is empty: no

Erasing 'zebra'...
Remaining entries:
  alice -> person
  arthur -> sheep
  fred -> horse
  jemima -> dog
```

### Using std::unordered_map (Hash Table)

The `unordered_map` container does not maintain any particular order of elements.

**Complete unordered_map Example:**

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

using namespace std;

int main() {
    // Create an unordered_map with string keys and string values
    unordered_map<string, string> animalMap;
    
    cout << "=== std::unordered_map Example (Hash Table Implementation) ===" << endl;
    cout << "\nInserting key-value pairs..." << endl;
    
    // Insert elements
    animalMap["fred"] = "horse";
    animalMap["jemima"] = "dog";
    animalMap["arthur"] = "sheep";
    animalMap["zebra"] = "striped";
    animalMap["alice"] = "person";
    
    // Access elements
    cout << "\nAccessing 'jemima': " << animalMap["jemima"] << endl;
    cout << "Accessing 'fred': " << animalMap["fred"] << endl;
    
    // Display all elements (note: no guaranteed order)
    cout << "\nAll entries (no particular order):" << endl;
    for (const auto& pair : animalMap) {
        cout << "  " << pair.first << " -> " << pair.second << endl;
    }
    
    // Check if key exists
    string searchKey = "arthur";
    cout << "\nSearching for '" << searchKey << "': ";
    if (animalMap.find(searchKey) != animalMap.end()) {
        cout << "Found - Value: " << animalMap[searchKey] << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Size and empty check
    cout << "\nMap size: " << animalMap.size() << endl;
    cout << "Map is empty: " << (animalMap.empty() ? "yes" : "no") << endl;
    
    // Bucket information (hash table specific)
    cout << "\nHash table statistics:" << endl;
    cout << "  Number of buckets: " << animalMap.bucket_count() << endl;
    cout << "  Load factor: " << animalMap.load_factor() << endl;
    
    // Erase an element
    cout << "\nErasing 'zebra'..." << endl;
    animalMap.erase("zebra");
    
    cout << "Remaining entries:" << endl;
    for (const auto& pair : animalMap) {
        cout << "  " << pair.first << " -> " << pair.second << endl;
    }
    
    return 0;
}
```

**Sample Output:**
```
=== std::unordered_map Example (Hash Table Implementation) ===

Inserting key-value pairs...

Accessing 'jemima': dog
Accessing 'fred': horse

All entries (no particular order):
  alice -> person
  zebra -> striped
  arthur -> sheep
  jemima -> dog
  fred -> horse

Searching for 'arthur': Found - Value: sheep

Map size: 5
Map is empty: no

Hash table statistics:
  Number of buckets: 11
  Load factor: 0.454545

Erasing 'zebra'...
Remaining entries:
  alice -> person
  arthur -> sheep
  jemima -> dog
  fred -> horse
```

### Common Operations for Both map and unordered_map

Both containers share similar interfaces:

```cpp
// Declaration
map<KeyType, ValueType> myMap;
unordered_map<KeyType, ValueType> myUnorderedMap;

// Insertion
myMap[key] = value;
myMap.insert({key, value});

// Access
ValueType val = myMap[key];
ValueType val = myMap.at(key);  // Throws exception if key doesn't exist

// Search
if (myMap.find(key) != myMap.end()) { /* key exists */ }
if (myMap.count(key) > 0) { /* key exists */ }

// Deletion
myMap.erase(key);

// Size
size_t size = myMap.size();

// Iteration
for (const auto& pair : myMap) {
    cout << pair.first << " -> " << pair.second << endl;
}
```

---

## Section 5: Practical Applications and Examples

### Example 1: Telephone Directory

A classic use case for dictionaries is implementing a telephone directory where we look up phone numbers by name.

**Complete Telephone Directory Program:**

```cpp
#include <iostream>
#include <map>
#include <string>
#include <limits>

using namespace std;

class TelephoneDirectory {
private:
    map<string, string> directory;
    
public:
    void addEntry(const string& name, const string& number) {
        directory[name] = number;
        cout << "Added: " << name << " -> " << number << endl;
    }
    
    bool lookupNumber(const string& name, string& number) const {
        auto it = directory.find(name);
        if (it != directory.end()) {
            number = it->second;
            return true;
        }
        return false;
    }
    
    void displayAll() const {
        if (directory.empty()) {
            cout << "Directory is empty." << endl;
            return;
        }
        
        cout << "\n=== Complete Directory ===" << endl;
        for (const auto& entry : directory) {
            cout << entry.first << ": " << entry.second << endl;
        }
    }
    
    bool removeEntry(const string& name) {
        return directory.erase(name) > 0;
    }
    
    size_t size() const {
        return directory.size();
    }
};

int main() {
    TelephoneDirectory phoneBook;
    
    cout << "=== Telephone Directory System ===" << endl;
    cout << "\nPopulating directory..." << endl;
    
    phoneBook.addEntry("Alice Johnson", "555-0123");
    phoneBook.addEntry("Bob Smith", "555-0456");
    phoneBook.addEntry("Carol Williams", "555-0789");
    phoneBook.addEntry("David Brown", "555-0321");
    phoneBook.addEntry("Eve Davis", "555-0654");
    
    phoneBook.displayAll();
    
    // Lookup examples
    cout << "\n=== Lookup Examples ===" << endl;
    string number;
    
    string searchName = "Bob Smith";
    cout << "\nLooking up " << searchName << ": ";
    if (phoneBook.lookupNumber(searchName, number)) {
        cout << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    searchName = "Eve Davis";
    cout << "Looking up " << searchName << ": ";
    if (phoneBook.lookupNumber(searchName, number)) {
        cout << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    searchName = "Frank Miller";
    cout << "Looking up " << searchName << ": ";
    if (phoneBook.lookupNumber(searchName, number)) {
        cout << number << endl;
    } else {
        cout << "Not found" << endl;
    }
    
    // Interactive section
    cout << "\n=== Interactive Mode ===" << endl;
    cout << "Commands: (a)dd, (l)ookup, (d)isplay, (r)emove, (q)uit" << endl;
    
    char command;
    bool running = true;
    
    while (running) {
        cout << "\nEnter command: ";
        cin >> command;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');
        
        switch (command) {
            case 'a':
            case 'A': {
                string name, phoneNum;
                cout << "Enter name: ";
                getline(cin, name);
                cout << "Enter phone number: ";
                getline(cin, phoneNum);
                phoneBook.addEntry(name, phoneNum);
                break;
            }
            case 'l':
            case 'L': {
                string name;
                cout << "Enter name to lookup: ";
                getline(cin, name);
                string phoneNum;
                if (phoneBook.lookupNumber(name, phoneNum)) {
                    cout << "Phone number: " << phoneNum << endl;
                } else {
                    cout << "Name not found in directory." << endl;
                }
                break;
            }
            case 'd':
            case 'D':
                phoneBook.displayAll();
                break;
            case 'r':
            case 'R': {
                string name;
                cout << "Enter name to remove: ";
                getline(cin, name);
                if (phoneBook.removeEntry(name)) {
                    cout << "Removed " << name << " from directory." << endl;
                } else {
                    cout << "Name not found in directory." << endl;
                }
                break;
            }
            case 'q':
            case 'Q':
                running = false;
                cout << "Exiting..." << endl;
                break;
            default:
                cout << "Invalid command. Use a, l, d, r, or q." << endl;
        }
    }
    
    cout << "\nFinal directory size: " << phoneBook.size() << " entries" << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Telephone Directory System ===

Populating directory...
Added: Alice Johnson -> 555-0123
Added: Bob Smith -> 555-0456
Added: Carol Williams -> 555-0789
Added: David Brown -> 555-0321
Added: Eve Davis -> 555-0654

=== Complete Directory ===
Alice Johnson: 555-0123
Bob Smith: 555-0456
Carol Williams: 555-0789
David Brown: 555-0321
Eve Davis: 555-0654

=== Lookup Examples ===

Looking up Bob Smith: 555-0456
Looking up Eve Davis: 555-0654
Looking up Frank Miller: Not found

=== Interactive Mode ===
Commands: (a)dd, (l)ookup, (d)isplay, (r)emove, (q)uit

Enter command: l
Enter name to lookup: Alice Johnson
Phone number: 555-0123

Enter command: q
Exiting...

Final directory size: 5 entries
```

### Example 2: Word Frequency Counter

Another common application is counting the frequency of words in text.

**Complete Word Frequency Counter:**

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
#include <sstream>
#include <vector>
#include <algorithm>

using namespace std;

class WordFrequencyCounter {
private:
    unordered_map<string, int> wordCounts;
    
    // Convert string to lowercase
    string toLower(string str) const {
        transform(str.begin(), str.end(), str.begin(), ::tolower);
        return str;
    }
    
public:
    void addText(const string& text) {
        stringstream ss(text);
        string word;
        
        while (ss >> word) {
            // Remove punctuation and convert to lowercase
            word.erase(remove_if(word.begin(), word.end(), ::ispunct), word.end());
            if (!word.empty()) {
                word = toLower(word);
                wordCounts[word]++;
            }
        }
    }
    
    int getCount(const string& word) const {
        string lowerWord = toLower(word);
        auto it = wordCounts.find(lowerWord);
        if (it != wordCounts.end()) {
            return it->second;
        }
        return 0;
    }
    
    void displayAll() const {
        cout << "\n=== Word Frequencies ===" << endl;
        for (const auto& pair : wordCounts) {
            cout << pair.first << ": " << pair.second << endl;
        }
    }
    
    void displayTopN(int n) const {
        // Create vector of pairs and sort by frequency
        vector<pair<string, int>> sortedWords(wordCounts.begin(), wordCounts.end());
        
        sort(sortedWords.begin(), sortedWords.end(),
             [](const pair<string, int>& a, const pair<string, int>& b) {
                 return a.second > b.second;  // Sort by frequency (descending)
             });
        
        cout << "\n=== Top " << n << " Most Frequent Words ===" << endl;
        int count = 0;
        for (const auto& pair : sortedWords) {
            if (count >= n) break;
            cout << count + 1 << ". " << pair.first << ": " << pair.second << endl;
            count++;
        }
    }
    
    size_t uniqueWords() const {
        return wordCounts.size();
    }
    
    int totalWords() const {
        int total = 0;
        for (const auto& pair : wordCounts) {
            total += pair.second;
        }
        return total;
    }
};

int main() {
    WordFrequencyCounter counter;
    
    cout << "=== Word Frequency Counter ===" << endl;
    
    // Sample texts
    string text1 = "The quick brown fox jumps over the lazy dog.";
    string text2 = "The dog was lazy, but the fox was quick.";
    string text3 = "Quick movements make the brown fox jump high.";
    
    cout << "\nAnalyzing texts..." << endl;
    cout << "Text 1: " << text1 << endl;
    cout << "Text 2: " << text2 << endl;
    cout << "Text 3: " << text3 << endl;
    
    counter.addText(text1);
    counter.addText(text2);
    counter.addText(text3);
    
    counter.displayAll();
    
    // Statistics
    cout << "\n=== Statistics ===" << endl;
    cout << "Total unique words: " << counter.uniqueWords() << endl;
    cout << "Total word count: " << counter.totalWords() << endl;
    
    // Lookup specific words
    cout << "\n=== Specific Word Queries ===" << endl;
    vector<string> queryWords = {"the", "fox", "quick", "lazy", "elephant"};
    
    for (const string& word : queryWords) {
        int count = counter.getCount(word);
        cout << "'" << word << "' appears " << count << " time(s)" << endl;
    }
    
    // Display top 5 most frequent words
    counter.displayTopN(5);
    
    return 0;
}
```

**Sample Output:**
```
=== Word Frequency Counter ===

Analyzing texts...
Text 1: The quick brown fox jumps over the lazy dog.
Text 2: The dog was lazy, but the fox was quick.
Text 3: Quick movements make the brown fox jump high.

=== Word Frequencies ===
dog: 2
lazy: 2
over: 1
jumps: 1
brown: 2
was: 2
the: 4
fox: 3
but: 1
quick: 3
movements: 1
make: 1
jump: 1
high: 1

=== Statistics ===
Total unique words: 14
Total word count: 24

=== Specific Word Queries ===
'the' appears 4 time(s)
'fox' appears 3 time(s)
'quick' appears 3 time(s)
'lazy' appears 2 time(s)
'elephant' appears 0 time(s)

=== Top 5 Most Frequent Words ===
1. the: 4
2. fox: 3
3. quick: 3
4. dog: 2
5. lazy: 2
```

### Example 3: Student Grade Management

**Complete Grade Management System:**

```cpp
#include <iostream>
#include <map>
#include <string>
#include <vector>
#include <iomanip>

using namespace std;

struct StudentGrade {
    string studentName;
    vector<double> grades;
    
    double getAverage() const {
        if (grades.empty()) return 0.0;
        double sum = 0.0;
        for (double grade : grades) {
            sum += grade;
        }
        return sum / grades.size();
    }
};

class GradeBook {
private:
    map<string, StudentGrade> students;
    
public:
    void addStudent(const string& studentId, const string& name) {
        StudentGrade sg;
        sg.studentName = name;
        students[studentId] = sg;
        cout << "Added student: " << name << " (ID: " << studentId << ")" << endl;
    }
    
    void addGrade(const string& studentId, double grade) {
        auto it = students.find(studentId);
        if (it != students.end()) {
            it->second.grades.push_back(grade);
            cout << "Added grade " << grade << " for " << it->second.studentName << endl;
        } else {
            cout << "Student ID " << studentId << " not found." << endl;
        }
    }
    
    void displayStudent(const string& studentId) const {
        auto it = students.find(studentId);
        if (it != students.end()) {
            const StudentGrade& sg = it->second;
            cout << "\nStudent: " << sg.studentName << " (ID: " << studentId << ")" << endl;
            cout << "Grades: ";
            for (size_t i = 0; i < sg.grades.size(); ++i) {
                cout << sg.grades[i];
                if (i < sg.grades.size() - 1) cout << ", ";
            }
            cout << endl;
            cout << fixed << setprecision(2);
            cout << "Average: " << sg.getAverage() << endl;
        } else {
            cout << "Student ID " << studentId << " not found." << endl;
        }
    }
    
    void displayAllStudents() const {
        if (students.empty()) {
            cout << "No students in gradebook." << endl;
            return;
        }
        
        cout << "\n=== All Students ===" << endl;
        cout << fixed << setprecision(2);
        for (const auto& pair : students) {
            cout << "ID: " << pair.first << " | Name: " << pair.second.studentName 
                 << " | Average: " << pair.second.getAverage() << endl;
        }
    }
    
    void displayClassStatistics() const {
        if (students.empty()) {
            cout << "No students in gradebook." << endl;
            return;
        }
        
        double classTotal = 0.0;
        int studentCount = 0;
        double highest = -1.0;
        double lowest = 101.0;
        string highestStudent, lowestStudent;
        
        for (const auto& pair : students) {
            double avg = pair.second.getAverage();
            if (avg > 0) {  // Only count students with grades
                classTotal += avg;
                studentCount++;
                
                if (avg > highest) {
                    highest = avg;
                    highestStudent = pair.second.studentName;
                }
                if (avg < lowest) {
                    lowest = avg;
                    lowestStudent = pair.second.studentName;
                }
            }
        }
        
        cout << "\n=== Class Statistics ===" << endl;
        cout << fixed << setprecision(2);
        if (studentCount > 0) {
            cout << "Class Average: " << (classTotal / studentCount) << endl;
            cout << "Highest Average: " << highest << " (" << highestStudent << ")" << endl;
            cout << "Lowest Average: " << lowest << " (" << lowestStudent << ")" << endl;
        }
        cout << "Total Students: " << students.size() << endl;
    }
};

int main() {
    GradeBook gradebook;
    
    cout << "=== Student Grade Management System ===" << endl;
    
    // Add students
    cout << "\nAdding students..." << endl;
    gradebook.addStudent("S001", "Alice Johnson");
    gradebook.addStudent("S002", "Bob Smith");
    gradebook.addStudent("S003", "Carol Williams");
    gradebook.addStudent("S004", "David Brown");
    
    // Add grades
    cout << "\nAdding grades..." << endl;
    gradebook.addGrade("S001", 85.5);
    gradebook.addGrade("S001", 92.0);
    gradebook.addGrade("S001", 88.5);
    
    gradebook.addGrade("S002", 78.0);
    gradebook.addGrade("S002", 82.5);
    gradebook.addGrade("S002", 85.0);
    
    gradebook.addGrade("S003", 95.0);
    gradebook.addGrade("S003", 97.5);
    gradebook.addGrade("S003", 93.0);
    
    gradebook.addGrade("S004", 72.0);
    gradebook.addGrade("S004", 75.5);
    gradebook.addGrade("S004", 78.0);
    
    // Display individual students
    cout << "\n--- Individual Student Records ---" << endl;
    gradebook.displayStudent("S001");
    gradebook.displayStudent("S003");
    
    // Display all students
    gradebook.displayAllStudents();
    
    // Display class statistics
    gradebook.displayClassStatistics();
    
    return 0;
}
```

**Sample Output:**
```
=== Student Grade Management System ===

Adding students...
Added student: Alice Johnson (ID: S001)
Added student: Bob Smith (ID: S002)
Added student: Carol Williams (ID: S003)
Added student: David Brown (ID: S004)

Adding grades...
Added grade 85.5 for Alice Johnson
Added grade 92 for Alice Johnson
Added grade 88.5 for Alice Johnson
Added grade 78 for Bob Smith
Added grade 82.5 for Bob Smith
Added grade 85 for Bob Smith
Added grade 95 for Carol Williams
Added grade 97.5 for Carol Williams
Added grade 93 for Carol Williams
Added grade 72 for David Brown
Added grade 75.5 for David Brown
Added grade 78 for David Brown

--- Individual Student Records ---

Student: Alice Johnson (ID: S001)
Grades: 85.5, 92, 88.5
Average: 88.67

Student: Carol Williams (ID: S003)
Grades: 95, 97.5, 93
Average: 95.17

=== All Students ===
ID: S001 | Name: Alice Johnson | Average: 88.67
ID: S002 | Name: Bob Smith | Average: 81.83
ID: S003 | Name: Carol Williams | Average: 95.17
ID: S004 | Name: David Brown | Average: 75.17

=== Class Statistics ===
Class Average: 85.21
Highest Average: 95.17 (Carol Williams)
Lowest Average: 75.17 (David Brown)
Total Students: 4
```

---

## Section 6: Choosing Between map and unordered_map

### Summary Comparison

| Feature | map (BST) | unordered_map (Hash Table) |
|---------|-----------|---------------------------|
| **Implementation** | Self-balancing BST (Red-Black Tree) | Hash Table with chaining |
| **Ordering** | Sorted by key | No particular order |
| **Search Time** | O(log n) guaranteed | O(1) average, O(n) worst |
| **Insert Time** | O(log n) guaranteed | O(1) average, O(n) worst |
| **Delete Time** | O(log n) guaranteed | O(1) average, O(n) worst |
| **Memory** | Lower overhead | Higher overhead (buckets) |
| **Iterator Validity** | Not affected by insertions (except deleted elements) | May be invalidated by insertions |
| **Key Requirements** | Must be comparable (<, >, ==) | Must be hashable and comparable (==) |

### When to Use map

Choose `map` when you need:
- Elements in sorted order
- Guaranteed logarithmic time complexity
- Range queries (finding all keys between two values)
- Predictable performance regardless of input
- Lower memory overhead

### When to Use unordered_map

Choose `unordered_map` when you need:
- Fastest average-case performance
- No requirement for sorted data
- Simple key lookups
- Better performance for large datasets (with good hash function)

### Performance Demonstration

```cpp
#include <iostream>
#include <map>
#include <unordered_map>
#include <chrono>
#include <random>

using namespace std;
using namespace std::chrono;

int main() {
    const int NUM_OPERATIONS = 100000;
    
    map<int, int> orderedMap;
    unordered_map<int, int> unorderedMap;
    
    // Random number generator
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dis(1, 1000000);
    
    cout << "=== Performance Comparison ===" << endl;
    cout << "Number of operations: " << NUM_OPERATIONS << endl;
    
    // Test map insertion
    auto start = high_resolution_clock::now();
    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        orderedMap[dis(gen)] = i;
    }
    auto end = high_resolution_clock::now();
    auto mapInsertTime = duration_cast<milliseconds>(end - start).count();
    
    // Test unordered_map insertion
    start = high_resolution_clock::now();
    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        unorderedMap[dis(gen)] = i;
    }
    end = high_resolution_clock::now();
    auto unorderedMapInsertTime = duration_cast<milliseconds>(end - start).count();
    
    cout << "\n--- Insertion Performance ---" << endl;
    cout << "map: " << mapInsertTime << " ms" << endl;
    cout << "unordered_map: " << unorderedMapInsertTime << " ms" << endl;
    
    // Test map lookup
    start = high_resolution_clock::now();
    int found = 0;
    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        if (orderedMap.find(dis(gen)) != orderedMap.end()) {
            found++;
        }
    }
    end = high_resolution_clock::now();
    auto mapLookupTime = duration_cast<milliseconds>(end - start).count();
    
    // Test unordered_map lookup
    start = high_resolution_clock::now();
    found = 0;
    for (int i = 0; i < NUM_OPERATIONS; ++i) {
        if (unorderedMap.find(dis(gen)) != unorderedMap.end()) {
            found++;
        }
    }
    end = high_resolution_clock::now();
    auto unorderedMapLookupTime = duration_cast<milliseconds>(end - start).count();
    
    cout << "\n--- Lookup Performance ---" << endl;
    cout << "map: " << mapLookupTime << " ms" << endl;
    cout << "unordered_map: " << unorderedMapLookupTime << " ms" << endl;
    
    cout << "\n--- Size Information ---" << endl;
    cout << "map size: " << orderedMap.size() << endl;
    cout << "unordered_map size: " << unorderedMap.size() << endl;
    
    return 0;
}
```

**Sample Output:**
```
=== Performance Comparison ===
Number of operations: 100000

--- Insertion Performance ---
map: 245 ms
unordered_map: 156 ms

--- Lookup Performance ---
map: 198 ms
unordered_map: 124 ms

--- Size Information ---
map size: 63289
unordered_map size: 63312
```

---

## Resources for Further Learning

### Official Documentation
- **C++ map reference**: http://www.cplusplus.com/reference/map/
- **C++ unordered_map reference**: http://www.cplusplus.com/reference/unordered_map/
- **C++ Standard Library documentation**: https://en.cppreference.com/

### Online Learning Resources
- **Data Structure Visualizations**: http://www.cs.usfca.edu/~galles/visualization/Algorithms.html
  - Interactive demonstrations of BST, Hash Tables, and many other data structures
- **GeeksforGeeks - Map in C++ STL**: https://www.geeksforgeeks.org/map-associative-containers-the-c-standard-template-library-stl/
- **GeeksforGeeks - Unordered Map in C++**: https://www.geeksforgeeks.org/unordered_map-in-cpp-stl/

### Textbooks and In-Depth Reading
- **Data Structures and Algorithm Analysis in C++** by Mark Allen Weiss
  - Chapters on Trees and Hashing
- **Introduction to Algorithms** by Cormen, Leiserson, Rivest, and Stein
  - Chapters 12 (Binary Search Trees) and 11 (Hash Tables)
- **The C++ Standard Library** by Nicolai Josuttis
  - Comprehensive coverage of STL containers including map and unordered_map

### Practice Platforms
- **LeetCode**: https://leetcode.com/
  - Search for problems tagged with "Hash Table" or "Binary Search Tree"
- **HackerRank**: https://www.hackerrank.com/
  - C++ practice problems involving maps and dictionaries
- **Codeforces**: https://codeforces.com/
  - Competitive programming problems using STL containers

### Algorithm Complexity Resources
- **Big-O Cheat Sheet**: https://www.bigocheatsheet.com/
  - Quick reference for time and space complexity of common data structures
- **C++ STL Complexity**: https://www.cplusplus.com/reference/stl/
  - Detailed complexity analysis for all STL operations

### Community Resources
- **Stack Overflow**: https://stackoverflow.com/questions/tagged/c%2b%2b+stl
  - Community-driven Q&A for specific implementation questions
- **C++ Subreddit**: https://www.reddit.com/r/cpp/
  - Discussions about modern C++ practices and STL usage
- **CPP Reference Forum**: https://en.cppreference.com/w/
  - Comprehensive technical reference with examples

---