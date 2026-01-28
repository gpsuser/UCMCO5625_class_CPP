# C++ Random Number Generation Worksheet

## Quiz Section

### Question 1: Fill in the Blanks

Complete the following statement about pseudorandom number generators:

"Pseudorandom number generators are __________ algorithms that produce sequences of numbers that appear random. Given the same __________, a PRNG will always produce the __________ sequence. This property is useful for __________ and testing."

**Word Bank (scrambled):** identical, debugging, seed, deterministic

---

### Question 2: Fill in the Blanks

Complete the following statement about the traditional `rand()` function:

"The `rand()` function has several limitations including lack of __________ safety, non-reproducibility across different __________, and dependence on the __________ and operating system. Additionally, seeding with `time(0)` is problematic because it only changes every __________."

**Word Bank (scrambled):** second, thread, compiler, platforms

---

### Question 3: Fill in the Blanks

Complete the following statement about the modern C++ random library:

"The `<random>` library separates the __________ (which generates uniform random bits) from the __________ (which shapes the output). This allows using the same generator with different __________, or different generators with the same distribution. The __________ is commonly recommended for general-purpose use."

**Word Bank (scrambled):** distributions, generator, distribution, mt19937

---

### Question 4: Fill in the Blanks

Complete the following statement about seeding random number generators:

"The __________ class provides access to system-implemented high-quality randomness and is preferred over using __________. When you need __________ sequences for testing, you can seed a generator with a specific __________ value."

**Word Bank (scrambled):** numeric, time(0), std::random_device, reproducible

---

### Question 5: Multiple Choice

What is the period of the Mersenne Twister algorithm (mt19937)?

A) 2^32 - 1  
B) 2^64 - 1  
C) 2^19937 - 1  
D) 2^31 - 1

---

### Question 6: Multiple Choice

Which of the following is NOT an advantage of the modern `<random>` library over `rand()`?

A) Independent generators with no shared global state  
B) Ability to choose specific PRNG algorithms  
C) Faster execution speed in all cases  
D) Built-in support for various probability distributions

---

### Question 7: Multiple Choice

Consider the following code:

```cpp
std::mt19937 gen1(42);
std::mt19937 gen2(42);
std::cout << gen1() << " " << gen2();
```

What will be the relationship between the two output values?

A) They will always be different  
B) They will always be identical  
C) They will be identical approximately 50% of the time  
D) The behavior is undefined

---

## Programming Tasks

### Task 1: Random Password Generator

Write a program that generates a random password of a specified length. The password should include:
- Lowercase letters (a-z)
- Uppercase letters (A-Z)
- Digits (0-9)

Use the modern `<random>` library with proper seeding.

**Code Scaffolding:**

```cpp
#include <iostream>
#include <random>
#include <string>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    
    int length;
    std::cout << "Enter password length: ";
    std::cin >> length;
    
    // Define the character set
    std::string chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    
    // TODO: Create a distribution to select random characters
    
    // TODO: Generate the password
    
    // TODO: Output the password
    
    return 0;
}
```

**Sample Input/Output:**

```
Enter password length: 12
Generated password: aB7xK2mPqR9t

Enter password length: 8
Generated password: Xy4pLm8Q
```

---

### Task 2: Biased Coin Flip Simulator

Write a program that simulates flipping a biased coin. The coin has a 70% chance of landing on heads and a 30% chance of landing on tails. The program should:
- Flip the coin a user-specified number of times
- Count the number of heads and tails
- Display the results

Use `std::discrete_distribution` to model the biased coin.

**Code Scaffolding:**

```cpp
#include <iostream>
#include <random>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // TODO: Create a discrete distribution for biased coin (70% heads, 30% tails)
    
    int flips;
    std::cout << "Enter number of coin flips: ";
    std::cin >> flips;
    
    int heads = 0;
    int tails = 0;
    
    // TODO: Flip the coin and count results
    
    // TODO: Display the results
    
    return 0;
}
```

**Sample Input/Output:**

```
Enter number of coin flips: 100
Results:
Heads: 68
Tails: 32

Enter number of coin flips: 1000
Results:
Heads: 702
Tails: 298
```

---

### Challenge Task: Card Deck Shuffler

Write a program that creates a standard 52-card deck and shuffles it using the Fisher-Yates shuffle algorithm with the modern `<random>` library. Display the first 10 cards after shuffling.

Each card should be represented as a string combining rank and suit (e.g., "Ace of Spades", "7 of Hearts").

**Requirements:**
- Create a deck with all 52 cards
- Implement shuffle using `std::uniform_int_distribution`
- Display the first 10 cards after shuffling
- Run the shuffle twice to show different results

**Code Scaffolding:**

```cpp
#include <iostream>
#include <random>
#include <vector>
#include <string>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    
    // Define ranks and suits
    std::vector<std::string> ranks = {"Ace", "2", "3", "4", "5", "6", "7", 
                                       "8", "9", "10", "Jack", "Queen", "King"};
    std::vector<std::string> suits = {"Hearts", "Diamonds", "Clubs", "Spades"};
    
    // TODO: Create a deck of 52 cards
    
    // TODO: Implement Fisher-Yates shuffle
    // Hint: For each position i from n-1 down to 1:
    //       - Generate random j between 0 and i (inclusive)
    //       - Swap elements at positions i and j
    
    // TODO: Display first 10 cards
    
    return 0;
}
```

**Sample Output:**

```
First shuffle - First 10 cards:
1. 7 of Diamonds
2. King of Spades
3. 3 of Hearts
4. Jack of Clubs
5. Ace of Diamonds
6. 9 of Spades
7. 4 of Hearts
8. Queen of Diamonds
9. 2 of Clubs
10. 8 of Hearts

Second shuffle - First 10 cards:
1. 5 of Spades
2. Queen of Hearts
3. 10 of Diamonds
4. 3 of Clubs
5. King of Hearts
6. 6 of Spades
7. Ace of Clubs
8. 9 of Diamonds
9. Jack of Hearts
10. 4 of Spades
```

---
