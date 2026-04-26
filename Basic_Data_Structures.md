
# Basic Data Structures in C++

## 1. Overview

Data structures are ways to store and organize data in computers, including relationships between data elements and operations on these elements. The C++ Standard Library provides rich implementations of data structures. Mastering these basic data structures is key to writing efficient programs.

## 2. Array

### 2.1 Basic Concept

An array is an ordered collection of elements of the same type, accessed by index.

### 2.2 Declaration and Initialization

```cpp
// Static array
int arr[5];                    // Uninitialized
int arr[5] = {1, 2, 3, 4, 5};  // Initialized
int arr[] = {1, 2, 3};         // Auto-detect size

// Dynamic array
int* arr = new int[5];
delete[] arr;
```

### 2.3 Accessing Elements

```cpp
int arr[5] = {10, 20, 30, 40, 50};
cout << arr[0];  // Output: 10
cout << arr[2];  // Output: 30
```

### 2.4 Two-Dimensional Array

```cpp
int matrix[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};
```

## 3. String

### 3.1 C-style String

```cpp
char str[10] = "Hello";
char* str = "World";  // Read-only string
```

### 3.2 C++-style String

```cpp
#include <string>

std::string s1 = "Hello";
std::string s2("World");
std::string s3 = s1 + " " + s2;  // String concatenation
```

### 3.3 Common Operations

```cpp
std::string s = "Hello, World!";

s.size();           // Return length
s.length();         // Return length (same as size)
s.empty();          // Check if empty
s.clear();          // Clear string
s.substr(0, 5);     // Substring "Hello"
s.find("World");    // Find substring position
s.replace(0, 5, "Hi");  // Replace
```

## 4. Vector

### 4.1 Basic Concept

Vector is a dynamic array that supports automatic resizing.

### 4.2 Declaration and Initialization

```cpp
#include <vector>

std::vector<int> v1;                  // Empty vector
std::vector<int> v2(5);               // 5 elements, initialized to 0
std::vector<int> v3(5, 10);           // 5 elements, initialized to 10
std::vector<int> v4 = {1, 2, 3, 4, 5}; // List initialization
```

### 4.3 Common Operations

```cpp
std::vector<int> v = {1, 2, 3};

v.push_back(4);     // Add element to end
v.pop_back();       // Remove last element
v.insert(v.begin() + 1, 10);  // Insert 10 at position 1
v.erase(v.begin() + 2);       // Remove element at position 2
v.size();           // Return number of elements
v.empty();          // Check if empty
v.clear();          // Clear vector
```

### 4.4 Accessing Elements

```cpp
std::vector<int> v = {10, 20, 30};

v[0];              // Random access, no bounds check
v.at(0);           // Random access, with bounds check
v.front();         // First element
v.back();          // Last element
```

## 5. Linked List

### 5.1 Singly Linked List

```cpp
#include <forward_list>

std::forward_list<int> fl = {1, 2, 3, 4};

fl.push_front(0);   // Add to front
fl.pop_front();     // Remove from front
fl.insert_after(fl.begin(), 5);  // Insert after position
```

### 5.2 Doubly Linked List

```cpp
#include <list>

std::list<int> l = {1, 2, 3};

l.push_back(4);     // Add to back
l.push_front(0);    // Add to front
l.pop_back();       // Remove from back
l.pop_front();      // Remove from front
l.insert(l.begin(), 10);  // Insert at position
```

## 6. Stack

### 6.1 Basic Concept

Stack is a Last-In-First-Out (LIFO) data structure.

### 6.2 Common Operations

```cpp
#include <stack>

std::stack<int> s;

s.push(10);         // Push to stack
s.push(20);
s.push(30);

s.top();            // Return top element (30)
s.pop();            // Pop from stack (remove 30)
s.size();           // Return number of elements (2)
s.empty();          // Check if empty
```

## 7. Queue

### 7.1 Basic Concept

Queue is a First-In-First-Out (FIFO) data structure.

### 7.2 Common Operations

```cpp
#include <queue>

std::queue<int> q;

q.push(10);         // Enqueue
q.push(20);
q.push(30);

q.front();          // Return front element (10)
q.back();           // Return back element (30)
q.pop();            // Dequeue (remove 10)
q.size();           // Return number of elements (2)
q.empty();          // Check if empty
```

### 7.3 Priority Queue

```cpp
#include <queue>

std::priority_queue<int> pq;

pq.push(3);
pq.push(1);
pq.push(4);
pq.push(2);

pq.top();           // Return largest element (4)
pq.pop();           // Remove largest element
```

## 8. Map

### 8.1 Basic Concept

Map is an ordered collection of key-value pairs with unique keys.

### 8.2 Common Operations

```cpp
#include <map>

std::map<std::string, int> m;

m["Alice"] = 25;    // Add key-value pair
m["Bob"] = 30;
m.insert({"Charlie", 28});  // Another insert method

m["Alice"];         // Access value (25)
m.find("Bob");      // Find key
m.erase("Charlie"); // Remove key-value pair
m.size();           // Return number of elements
m.empty();          // Check if empty
```

### 8.3 Unordered Map

```cpp
#include <unordered_map>

std::unordered_map<std::string, int> um;

um["apple"] = 5;
um["banana"] = 3;
```

## 9. Set

### 9.1 Basic Concept

Set is an ordered collection of unique elements.

### 9.2 Common Operations

```cpp
#include <set>

std::set<int> s;

s.insert(1);        // Insert element
s.insert(2);
s.insert(2);        // Duplicate ignored

s.find(1);          // Find element
s.count(2);         // Count (0 or 1)
s.erase(1);         // Remove element
s.size();           // Return number of elements
```

### 9.3 Unordered Set

```cpp
#include <unordered_set>

std::unordered_set<int> us;

us.insert(10);
us.insert(20);
us.contains(10);    // Check if contains
```

## 10. Iterator

### 10.1 Basic Concept

Iterator is an object used to traverse containers.

### 10.2 Usage Example

```cpp
#include <vector>
#include <iostream>

std::vector<int> v = {1, 2, 3, 4, 5};

// Using iterator
for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
    std::cout << *it << " ";
}

// Range-based for loop (C++11+)
for (int num : v) {
    std::cout << num << " ";
}
```

## 11. Practical Examples

### Example 1: Parentheses Matching with Stack

```cpp
#include <iostream>
#include <stack>
#include <string>

bool isValid(const std::string& s) {
    std::stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            char top = st.top();
            if ((c == ')' && top == '(') ||
                (c == '}' && top == '{') ||
                (c == ']' && top == '[')) {
                st.pop();
            } else {
                return false;
            }
        }
    }
    return st.empty();
}
```

### Example 2: Word Frequency with Map

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::string s = "hello world hello cpp";
    std::map<std::string, int> freq;
    
    std::string word;
    for (char c : s) {
        if (c == ' ') {
            freq[word]++;
            word.clear();
        } else {
            word += c;
        }
    }
    freq[word]++;  // Last word
    
    for (const auto& pair : freq) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    return 0;
}
```

## 12. Summary

| Data Structure | Header | Characteristics | Common Operations |
|----------------|--------|-----------------|-------------------|
| Array | - | Fixed size, fast access | [] |
| String | `<string>` | Variable length | size, substr, find |
| Vector | `<vector>` | Dynamic array | push_back, pop_back |
| Linked List | `<list>` | Doubly linked list | push_front, push_back |
| Stack | `<stack>` | LIFO | push, pop, top |
| Queue | `<queue>` | FIFO | push, pop, front |
| Map | `<map>` | Ordered key-value | insert, find, erase |
| Set | `<set>` | Ordered unique | insert, find, erase |

Choosing the right data structure can significantly improve program efficiency and readability.
