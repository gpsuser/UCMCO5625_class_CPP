# Lecture 8: C++ Stacks and Queues Worksheet

## Quiz Section

### Question 1: Fill in the Blanks

Complete the following paragraph by filling in the blanks with the appropriate terms.

A __________ follows the LIFO principle, meaning the __________ element added is the first one to be removed. In contrast, a __________ follows the FIFO principle, where the __________ element added is removed first. When using a C++ stack, the __________ method adds elements and the __________ method returns the top element without removing it.

**Word Bank:** `oldest`, `queue`, `most recent`, `stack`, `push`, `top`

---

### Question 2: Fill in the Blanks

Complete the following code snippet by filling in the blanks.

```cpp
std::stack<int> s;
s.__________(10);
s.__________(20);
s.__________(30);
std::cout << s.__________() << "\n";  // Outputs 30
s.__________();
std::cout << s.__________() << "\n";  // Outputs 20
```

**Word Bank:** `pop`, `push`, `top`, `front`, `back`, `empty`

---

### Question 3: Fill in the Blanks

Complete the following paragraph about checking for empty containers.

Before accessing elements from a stack or queue, it's important to check if the container is __________ using the __________ method, which returns __________ if there are no elements. Attempting to call __________ on an empty stack or __________ on an empty queue results in __________ behavior.

**Word Bank:** `undefined`, `empty`, `top`, `true`, `empty()`, `front`

---

### Question 4: Fill in the Blanks - Stack vs Queue Behavior

Given the following operations, complete the output sequence.

```cpp
std::stack<int> st;
std::queue<int> qu;

for (int i = 1; i <= 4; ++i) {
    st.push(i);
    qu.push(i);
}
```

The stack will output elements in order: __________, __________, __________, __________
The queue will output elements in order: __________, __________, __________, __________

**Word Bank:** `1`, `2`, `3`, `4`

---

### Question 5: Multiple Choice

What is the time complexity of the `push()` operation for both `std::stack` and `std::queue`?

A) O(n)  
B) O(log n)  
C) O(1)  
D) O(n²)

---

### Question 6: Multiple Choice

Which of the following applications is BEST suited for a queue data structure?

A) Implementing an undo feature in a text editor  
B) Checking for balanced parentheses in an expression  
C) Managing print jobs in a printer  
D) Evaluating postfix expressions  

---

### Question 7: Multiple Choice

Consider the following code:

```cpp
std::stack<int> s;
s.push(5);
s.push(10);
s.push(15);
s.pop();
s.push(20);
int x = s.top();
```

What is the value of `x`?

A) 5  
B) 10  
C) 15  
D) 20  

---

## Programming Tasks

### Task 1: Palindrome Checker Using a Stack

Write a program that uses a stack to check if a given string is a palindrome (reads the same forwards and backwards, ignoring spaces and case). The program should push characters onto a stack and then compare them with the original string.

**Requirements:**
- Ignore spaces in the input
- Ignore case differences (treat 'A' and 'a' as the same)
- Use a stack to help with the comparison

**Code Scaffolding:**

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <cctype>

int main() {
    std::string input;
    std::cout << "Enter a string: ";
    std::getline(std::cin, input);
    
    std::stack<char> charStack;
    
    // TODO: Push characters onto stack (ignore spaces, convert to lowercase)
    
    
    // TODO: Compare with original string by popping from stack
    
    
    // TODO: Output result
    
    return 0;
}
```

**Sample Input/Output:**

```
Enter a string: racecar
Output: "racecar" is a palindrome

Enter a string: A man a plan a canal Panama
Output: "A man a plan a canal Panama" is a palindrome

Enter a string: hello
Output: "hello" is not a palindrome
```

---

### Task 2: Queue-Based Number Processor

Write a program that uses a queue to process a series of numbers. The program should:
1. Read integers from the user until they enter -1
2. Add all positive even numbers to a queue
3. Process the queue by removing and displaying each number
4. Calculate and display the sum of all processed numbers

**Requirements:**
- Only store even positive numbers in the queue
- Display each number as it's removed from the queue
- Calculate the total sum

**Code Scaffolding:**

```cpp
#include <iostream>
#include <queue>

int main() {
    std::queue<int> evenQueue;
    int number;
    
    std::cout << "Enter integers (enter -1 to stop):\n";
    
    // TODO: Read numbers and add even positive numbers to queue
    
    
    std::cout << "\nProcessing even numbers from queue:\n";
    
    // TODO: Process queue and calculate sum
    
    
    // TODO: Display sum
    
    return 0;
}
```

**Sample Input/Output:**

```
Enter integers (enter -1 to stop):
12
7
8
15
4
20
-1

Processing even numbers from queue:
12
8
4
20
Sum of processed numbers: 44
```

---
