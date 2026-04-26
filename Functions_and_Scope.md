
# Functions and Scope in C++

## 1. Overview of Functions

A function is a block of code that performs a specific task. It can receive parameters, return values, and enables code reuse and modularity.

### 1.1 Function Structure

```cpp
return_type function_name(parameter_list) {
    // function body
    return value;
}
```

### 1.2 Function Declaration vs Definition

**Declaration** (function prototype):
```cpp
int add(int a, int b);  // Declaration
```

**Definition** (implementation):
```cpp
int add(int a, int b) {
    return a + b;
}
```

## 2. Function Parameters

### 2.1 Parameter Passing Methods

#### Pass by Value
```cpp
void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;  // Only affects local variables
}
```

#### Pass by Reference
```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;  // Affects actual parameters
}
```

#### Pass by Pointer
```cpp
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;  // Modify through pointer
}
```

### 2.2 Default Parameters

```cpp
int multiply(int a, int b = 2) {
    return a * b;
}

// Usage
multiply(5);    // 5 * 2 = 10
multiply(5, 3); // 5 * 3 = 15
```

### 2.3 Variable Arguments

```cpp
#include <cstdarg>

int sum(int count, ...) {
    va_list args;
    va_start(args, count);
    
    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);
    }
    
    va_end(args);
    return total;
}

// Usage
sum(3, 1, 2, 3);  // Returns 6
```

## 3. Function Return Values

### 3.1 Return Single Value

```cpp
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

### 3.2 Return Reference

```cpp
int& getMax(int& a, int& b) {
    return (a > b) ? a : b;
}

// Usage
int x = 10, y = 20;
getMax(x, y) = 30;  // Modify the larger value
```

### 3.3 Return Multiple Values

Using reference parameters:
```cpp
void divide(int a, int b, int& quotient, int& remainder) {
    quotient = a / b;
    remainder = a % b;
}
```

Using struct:
```cpp
struct Result {
    int quotient;
    int remainder;
};

Result divide(int a, int b) {
    Result res;
    res.quotient = a / b;
    res.remainder = a % b;
    return res;
}
```

## 4. Function Overloading

### 4.1 Concept

Multiple functions with the same name but different parameter lists in the same scope.

### 4.2 Example

```cpp
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}
```

### 4.3 Overload Resolution

Compiler selects the appropriate function based on parameter types and count.

## 5. Recursive Functions

### 5.1 Concept

A function that calls itself.

### 5.2 Example: Factorial

```cpp
int factorial(int n) {
    if (n == 0 || n == 1) {
        return 1;  // Base case
    }
    return n * factorial(n - 1);  // Recursive call
}
```

### 5.3 Example: Fibonacci Sequence

```cpp
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### 5.4 Tail Recursion Optimization

```cpp
int factorialTail(int n, int acc = 1) {
    if (n == 0) {
        return acc;
    }
    return factorialTail(n - 1, n * acc);
}
```

## 6. Inline Functions

### 6.1 Concept

Insert function body at call site during compilation to reduce function call overhead.

### 6.2 Syntax

```cpp
inline int square(int x) {
    return x * x;
}
```

### 6.3 Use Cases

- Small function body
- Frequently called
- No complex control flow

## 7. Scope

### 7.1 Scope Types

| Type | Description | Example |
|------|-------------|---------|
| Global Scope | Visible throughout the program | Global variables |
| Local Scope | Visible within function/block | Local variables |
| Class Scope | Visible within class | Member variables |
| Namespace Scope | Visible within namespace | Namespace members |

### 7.2 Variable Lifetime

```cpp
#include <iostream>
using namespace std;

int global = 10;  // Global variable, program lifetime

void func() {
    static int count = 0;  // Static local, initialized once
    count++;
    cout << count << endl;
}

int main() {
    func();  // Output: 1
    func();  // Output: 2
    func();  // Output: 3
    return 0;
}
```

### 7.3 Scope Resolution Operator

```cpp
int x = 10;  // Global variable

int main() {
    int x = 5;  // Local variable
    cout << x;        // Output: 5 (local)
    cout << ::x;      // Output: 10 (global)
    return 0;
}
```

## 8. Namespaces

### 8.1 Define Namespace

```cpp
namespace myspace {
    int add(int a, int b) {
        return a + b;
    }
}
```

### 8.2 Use Namespace

```cpp
// Method 1: Scope resolution
myspace::add(3, 4);

// Method 2: using declaration
using myspace::add;
add(3, 4);

// Method 3: using directive
using namespace myspace;
add(3, 4);
```

### 8.3 Nested Namespaces

```cpp
namespace outer {
    namespace inner {
        int value = 42;
    }
}

// Access
outer::inner::value;
```

## 9. Practical Examples

### Example 1: Calculator Functions

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }
int divide(int a, int b) { 
    if (b == 0) {
        cout << "Division by zero!" << endl;
        return 0;
    }
    return a / b;
}

int main() {
    int choice, a, b;
    cout << "1. Add\n2. Subtract\n3. Multiply\n4. Divide\n";
    cin >> choice >> a >> b;
    
    switch(choice) {
        case 1: cout << add(a, b); break;
        case 2: cout << subtract(a, b); break;
        case 3: cout << multiply(a, b); break;
        case 4: cout << divide(a, b); break;
        default: cout << "Invalid choice";
    }
    return 0;
}
```

### Example 2: Recursive Binary Search

```cpp
#include <iostream>
using namespace std;

int binarySearch(int arr[], int left, int right, int target) {
    if (right >= left) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        }
        
        if (arr[mid] > target) {
            return binarySearch(arr, left, mid - 1, target);
        }
        
        return binarySearch(arr, mid + 1, right, target);
    }
    
    return -1;  // Not found
}

int main() {
    int arr[] = {2, 5, 8, 12, 16, 23, 38, 56, 72, 91};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 23;
    
    int result = binarySearch(arr, 0, n - 1, target);
    if (result != -1) {
        cout << "Element found at index " << result;
    } else {
        cout << "Element not found";
    }
    return 0;
}
```

## 10. Summary

| Concept | Description |
|---------|-------------|
| Function Declaration | Tells compiler function name, return type, parameters |
| Function Definition | Actual implementation |
| Parameter Passing | Pass by value, reference, pointer |
| Function Overloading | Same name, different parameters |
| Recursion | Function calls itself |
| Inline Function | Reduce function call overhead |
| Scope | Visibility range of variables |
| Namespace | Organize code, avoid naming conflicts |

Understanding functions and scope is fundamental for writing structured and maintainable code.
