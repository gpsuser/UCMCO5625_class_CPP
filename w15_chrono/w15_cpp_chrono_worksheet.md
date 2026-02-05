# Time Handling in C++ with std::chrono: Worksheet

## Instructions

This worksheet is designed to reinforce your understanding of time handling in C++ using the std::chrono library. Complete all sections to the best of your ability.

**Estimated Time:** 45-60 minutes

---

## Section 1: Quiz Questions (15 minutes)

### Question 1: Fill in the Blanks

Complete the following statements using the words from the word bank below.

The std::chrono library was introduced in __________ as a modern, type-safe approach to handling time. Unlike the older <ctime> library, std::chrono provides __________ type safety through templated classes and ensures __________ operations. When pausing program execution, we use the __________ function which accepts a duration object as its parameter.

**Word Bank:** C++11, thread-safe, sleep_for, strong, duration_cast, system_clock, steady_clock

---

### Question 2: Fill in the Blanks

Complete the following statements using the words from the word bank below.

A duration in std::chrono consists of two components: a __________ which is the data type used to count time periods, and a __________ which is the time unit expressed as a ratio relative to one second. When converting from larger to smaller time units, there is no __________ loss, but converting from smaller to larger units may result in __________.

**Word Bank:** precision, representation, period, truncation, ratio, conversion, integer, clock

---

### Question 3: Fill in the Blanks

Complete the following statements using the words from the word bank below.

The __________ is a monotonic clock that never goes backward and cannot be adjusted, making it ideal for measuring code execution time. In contrast, the __________ represents wall clock time and can be adjusted by the operating system. When you subtract one time_point from another, you get a __________.

**Word Bank:** steady_clock, system_clock, duration, high_resolution_clock, time_point, epoch, nanoseconds, milliseconds

---

### Question 4: Fill in the Blanks

Complete the following statements using the words from the word bank below.

The __________ library extends std::chrono with additional clock types that measure CPU time rather than wall clock time. This is important for __________ because wall time includes time when other processes are running. The boost::chrono library provides clocks like process_user_cpu_clock which measures __________ CPU time.

**Word Bank:** boost, benchmarking, user-mode, chrono, wall, real-time, system-mode, performance

---

### Question 5: Multiple Choice

What happens when you convert 5500 milliseconds to seconds using duration_cast?

A) The result is 5.5 seconds stored as a double  
B) The result is 6 seconds due to rounding up  
C) The result is 5 seconds with truncation of the remainder  
D) The conversion fails and produces a compilation error

---

### Question 6: Multiple Choice

Which of the following clock types is MOST appropriate for measuring the performance of an algorithm?

A) system_clock, because it provides the most accurate time  
B) steady_clock, because it is monotonic and not affected by system time adjustments  
C) high_resolution_clock, because it always has the highest precision  
D) All clocks are equally suitable for performance measurement

---

### Question 7: Multiple Choice

Given the following code, what does elapsed represent?

```cpp
steady_clock::time_point start = steady_clock::now();
// some code executes here
steady_clock::time_point end = steady_clock::now();
steady_clock::duration elapsed = end - start;
```

A) A time_point representing when the code finished  
B) A duration representing how long the code took to execute  
C) The current system time in seconds  
D) The difference between the epoch and the current time

---

## Section 2: Programming Tasks

### Task 1: Countdown Timer (10 minutes)

Create a program that implements a countdown timer. The program should:

1. Ask the user to enter a number of seconds to count down from
2. Display the countdown in real-time, updating every second
3. When the countdown reaches zero, display "Time's up!"

**Code Scaffolding:**

```cpp
#include <iostream>
#include <chrono>
#include <thread>

using namespace std;
using namespace std::chrono;

int main()
{
    int countdown_seconds;
    
    cout << "Enter countdown time in seconds: ";
    cin >> countdown_seconds;
    
    cout << "\nStarting countdown from " << countdown_seconds << " seconds...\n" << endl;
    
    // TODO: Implement the countdown loop
    // Hint: Use a loop that decrements from countdown_seconds to 0
    // Hint: Use this_thread::sleep_for() to pause between counts
    // Hint: Display each number as it counts down
    
    
    
    
    
    return 0;
}
```

**Sample Input/Output:**

```
Enter countdown time in seconds: 5

Starting countdown from 5 seconds...

5
4
3
2
1
Time's up!
```

---

### Task 2: Stopwatch with Lap Times (10 minutes)

Create a stopwatch program that allows the user to record lap times. The program should:

1. Start timing automatically
2. Each time the user presses Enter, record and display a lap time
3. After 5 laps, display all lap times and the total elapsed time
4. Calculate and display the average lap time

**Code Scaffolding:**

```cpp
#include <iostream>
#include <chrono>
#include <vector>
#include <string>

using namespace std;
using namespace std::chrono;

int main()
{
    cout << "=== Stopwatch with Lap Timer ===" << endl;
    cout << "Press ENTER to record each lap time" << endl;
    cout << "Total of 5 laps will be recorded\n" << endl;
    
    // TODO: Create a vector to store lap durations
    
    
    // TODO: Record the start time
    
    
    // TODO: Create a variable to track the previous lap time
    
    
    string input;
    getline(cin, input); // Clear any initial input
    
    cout << "Starting... Press ENTER for Lap 1" << endl;
    
    // TODO: Implement loop to record 5 laps
    // Hint: For each lap, wait for user input
    // Hint: Calculate the duration since the last lap
    // Hint: Store this duration and display it
    
    
    
    
    
    
    
    
    // TODO: Display all lap times and calculate statistics
    // Hint: Show each lap time in milliseconds
    // Hint: Calculate total time and average lap time
    
    
    
    
    
    return 0;
}
```

**Sample Input/Output:**

```
=== Stopwatch with Lap Timer ===
Press ENTER to record each lap time
Total of 5 laps will be recorded

Starting... Press ENTER for Lap 1
[User presses Enter]
Lap 1: 1234 ms

Press ENTER for Lap 2
[User presses Enter]
Lap 2: 1456 ms

Press ENTER for Lap 3
[User presses Enter]
Lap 3: 1123 ms

Press ENTER for Lap 4
[User presses Enter]
Lap 4: 1567 ms

Press ENTER for Lap 5
[User presses Enter]
Lap 5: 1334 ms

=== Lap Summary ===
Lap 1: 1234 ms
Lap 2: 1456 ms
Lap 3: 1123 ms
Lap 4: 1567 ms
Lap 5: 1334 ms
-----------------
Total time: 6714 ms
Average lap: 1342 ms
```

---

### Challenge Task: Speed Typing Test with Accuracy (15 minutes)

Create a typing speed test program that measures both speed and accuracy. The program should:

1. Display a sentence for the user to type
2. Measure how long it takes the user to type the sentence
3. Compare the user's input with the target sentence
4. Calculate typing speed in words per minute (WPM)
5. Calculate accuracy as a percentage
6. Provide feedback based on performance

**Accuracy Calculation:** Count how many characters match the target sentence (in the correct position)

**WPM Calculation:** (Number of characters typed / 5) / (time in minutes)

**Code Scaffolding:**

```cpp
#include <iostream>
#include <chrono>
#include <string>

using namespace std;
using namespace std::chrono;

int main()
{
    string target = "The quick brown fox jumps over the lazy dog";
    
    cout << "=== Speed Typing Test ===" << endl;
    cout << "\nType the following sentence exactly as shown:" << endl;
    cout << "\"" << target << "\"" << endl;
    cout << "\nPress ENTER when ready to start...\n";
    
    string ready;
    getline(cin, ready);
    
    cout << "START TYPING NOW!\n";
    
    // TODO: Record start time
    
    
    string user_input;
    getline(cin, user_input);
    
    // TODO: Record end time
    
    
    // TODO: Calculate elapsed time
    
    
    // TODO: Calculate accuracy
    // Hint: Compare each character position
    // Hint: Count matches and calculate percentage
    
    
    
    
    
    
    // TODO: Calculate typing speed (WPM)
    // Hint: WPM = (characters typed / 5) / (minutes elapsed)
    
    
    
    
    // TODO: Display results
    // Hint: Show time taken, accuracy, and WPM
    // Hint: Provide performance feedback
    
    
    
    
    
    return 0;
}
```

**Sample Input/Output:**

```
=== Speed Typing Test ===

Type the following sentence exactly as shown:
"The quick brown fox jumps over the lazy dog"

Press ENTER when ready to start...
[User presses Enter]

START TYPING NOW!
The quick brown fox jumps over the lazy dog
[User presses Enter]

=== Results ===
Time taken: 8.45 seconds
Accuracy: 100%
Speed: 63 WPM

Performance: Excellent typing!
```

---

## Reflection Questions

1. Why is `steady_clock` preferred over `system_clock` for measuring code performance?

2. What are the advantages of using `duration_cast` compared to manual time unit conversions?

3. In what situations would you need to use the Boost.Chrono library instead of std::chrono?

---

**End of Worksheet**
