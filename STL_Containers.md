
# STL Containers in C++

## 1. Overview of STL

The Standard Template Library (STL) is a key part of the C++ Standard Library, providing generic template classes and functions including containers, iterators, and algorithms.

### 1.1 Container Classification

| Container Type | Characteristics | Examples |
|----------------|-----------------|----------|
| Sequence | Ordered elements | vector, list, deque |
| Associative | Ordered by keys | set, map |
| Unordered | Unordered, hash-based | unordered_set, unordered_map |
| Adapters | Wrapper containers | stack, queue, priority_queue |

## 2. Sequence Containers

### 2.1 vector

Dynamic array with random access.

```cpp
#include <vector>

std::vector<int> v;

// Add elements
v.push_back(1);
v.push_back(2);
v.push_back(3);

// Access elements
v[0];        // Random access
v.at(1);     // With bounds check
v.front();   // First element
v.back();    // Last element

// Size operations
v.size();    // Number of elements
v.empty();   // Check if empty
v.capacity();// Current capacity

// Remove elements
v.pop_back();
v.clear();
```

### 2.2 list

Doubly linked list, no random access.

```cpp
#include <list>

std::list<int> l = {1, 2, 3};

l.push_front(0);   // Add to front
l.push_back(4);    // Add to back
l.pop_front();     // Remove from front
l.pop_back();      // Remove from back
l.insert(l.begin(), 10);  // Insert
```

### 2.3 deque

Double-ended queue, fast insert/delete at both ends.

```cpp
#include <deque>

std::deque<int> d;

d.push_front(1);
d.push_back(2);
d.pop_front();
d.pop_back();
```

### 2.4 forward_list

Singly linked list, more memory efficient.

```cpp
#include <forward_list>

std::forward_list<int> fl = {1, 2, 3};
fl.push_front(0);
fl.pop_front();
```

## 3. Associative Containers

### 3.1 set

Ordered unique elements.

```cpp
#include <set>

std::set<int> s;

s.insert(3);
s.insert(1);
s.insert(2);
s.insert(2);  // Duplicate ignored

s.find(2);    // Search
s.count(3);   // Count (0 or 1)
s.erase(1);   // Remove
```

### 3.2 multiset

Ordered allows duplicates.

```cpp
#include <set>

std::multiset<int> ms;
ms.insert(1);
ms.insert(2);
ms.insert(2);  // Duplicates allowed
ms.count(2);   // Returns 2
```

### 3.3 map

Ordered key-value pairs, unique keys.

```cpp
#include <map>

std::map<std::string, int> m;

m["apple"] = 5;
m["banana"] = 3;
m.insert({"orange", 4});

m.find("apple");      // Search by key
m["apple"];           // Access value
m.erase("banana");    // Remove by key
```

### 3.4 multimap

Ordered key-value pairs, duplicate keys allowed.

```cpp
#include <map>

std::multimap<std::string, int> mm;
mm.insert({"fruit", 5});
mm.insert({"fruit", 3});  // Same key allowed
```

## 4. Unordered Containers

### 4.1 unordered_set

Hash set, unordered.

```cpp
#include <unordered_set>

std::unordered_set<int> us;

us.insert(1);
us.insert(2);
us.contains(1);  // C++20 feature
us.find(2);
```

### 4.2 unordered_map

Hash map, unordered.

```cpp
#include <unordered_map>

std::unordered_map<std::string, int> um;

um["key"] = 10;
um.find("key");
```

## 5. Container Adapters

### 5.1 stack

LIFO (Last-In-First-Out).

```cpp
#include <stack>

std::stack<int> s;

s.push(10);
s.push(20);
s.top();    // 20
s.pop();    // Remove 20
```

### 5.2 queue

FIFO (First-In-First-Out).

```cpp
#include <queue>

std::queue<int> q;

q.push(10);
q.push(20);
q.front();  // 10
q.back();   // 20
q.pop();    // Remove 10
```

### 5.3 priority_queue

Priority queue, largest element first.

```cpp
#include <queue>

std::priority_queue<int> pq;

pq.push(3);
pq.push(1);
pq.push(4);
pq.top();   // 4
pq.pop();   // Remove 4
```

## 6. Iterators

### 6.1 Iterator Types

| Type | Capability | Supported Containers |
|------|------------|---------------------|
| Random Access | Arbitrary access | vector, deque, array |
| Bidirectional | Two-way traversal | list, set, map |
| Forward | One-way traversal | forward_list |

### 6.2 Iterator Operations

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};

// Get iterators
auto it = v.begin();  // Points to first element
auto end = v.end();   // Points past last element

// Traverse
while (it != end) {
    std::cout << *it << " ";
    ++it;
}

// Range-based for (C++11)
for (int num : v) {
    std::cout << num << " ";
}
```

## 7. Common Algorithms

### 7.1 Search Algorithms

```cpp
#include <algorithm>

std::vector<int> v = {3, 1, 4, 1, 5, 9};

// Find
auto it = std::find(v.begin(), v.end(), 4);

// Count
int count = std::count(v.begin(), v.end(), 1);

// Binary search (requires sorted)
std::sort(v.begin(), v.end());
bool found = std::binary_search(v.begin(), v.end(), 5);
```

### 7.2 Sorting Algorithms

```cpp
std::vector<int> v = {3, 1, 4, 2, 5};

std::sort(v.begin(), v.end());           // Ascending
std::sort(v.begin(), v.end(), std::greater<int>());  // Descending
```

### 7.3 Transform Algorithms

```cpp
std::vector<int> v = {1, 2, 3, 4};
std::vector<int> result(4);

std::transform(v.begin(), v.end(), result.begin(), 
               [](int x) { return x * 2; });
```

## 8. Container Selection Guide

| Scenario | Recommended Container |
|----------|----------------------|
| Frequent random access | vector |
| Frequent insert/delete | list |
| Operations at both ends | deque |
| Unique element lookup | unordered_set |
| Key-value storage | unordered_map |
| LIFO operations | stack |
| FIFO operations | queue |

## 9. Practical Examples

### Example 1: Word Frequency with map

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    string text = "hello world hello cpp world";
    map<string, int> freq;
    
    string word;
    for (char c : text) {
        if (c == ' ') {
            freq[word]++;
            word.clear();
        } else {
            word += c;
        }
    }
    freq[word]++;
    
    for (const auto& pair : freq) {
        cout << pair.first << ": " << pair.second << endl;
    }
    return 0;
}
```

### Example 2: Top K Elements with priority_queue

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

vector<int> topK(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    
    for (int num : nums) {
        pq.push(num);
        if (pq.size() > k) {
            pq.pop();
        }
    }
    
    vector<int> result;
    while (!pq.empty()) {
        result.push_back(pq.top());
        pq.pop();
    }
    return result;
}
```

## 10. Summary

| Container | Characteristics | Time Complexity |
|-----------|-----------------|-----------------|
| vector | Dynamic array | O(1) access, O(1) push_back |
| list | Doubly linked list | O(1) insert/delete, O(n) access |
| deque | Double-ended queue | O(1) both ends |
| set | Ordered set | O(log n) operations |
| map | Ordered map | O(log n) operations |
| unordered_set | Hash set | O(1) average |
| unordered_map | Hash map | O(1) average |
| stack | LIFO adapter | O(1) operations |
| queue | FIFO adapter | O(1) operations |

Mastering STL containers is essential for writing efficient C++ code. Choosing the right container significantly improves program performance.
