
# Pointers and References in C++

## 1. Overview of Pointers

A pointer is a variable that stores the memory address of another variable. Pointers allow direct memory manipulation and are powerful features in C++.

### 1.1 Memory Address

```cpp
int x = 10;
cout << &x;  // Output the memory address of x
```

### 1.2 Pointer Declaration

```cpp
int* ptr;    // Pointer to int
char* cptr;  // Pointer to char
double* dptr; // Pointer to double
```

### 1.3 Basic Pointer Operations

```cpp
int x = 42;
int* ptr = &x;  // Get address

*ptr = 100;     // Dereference, modify x
cout << *ptr;   // Dereference, read x
```

## 2. Pointer Arithmetic

### 2.1 Pointer Math

```cpp
int arr[] = {10, 20, 30, 40, 50};
int* p = arr;   // Array name is address of first element

cout << *p;     // 10
cout << *(p+1); // 20
cout << *(p+2); // 30
```

### 2.2 Pointer Comparison

```cpp
int arr[5] = {1, 2, 3, 4, 5};
int* p = arr;
int* q = arr + 3;

if (p < q) {
    cout << "p is before q";
}
```

### 2.3 Pointers and Arrays

```cpp
int arr[5] = {10, 20, 30, 40, 50};

// Array subscript notation
cout << arr[2];  // 30

// Pointer notation
int* p = arr;
cout << *(p + 2); // 30
```

## 3. Dynamic Memory Allocation

### 3.1 Allocate with new

```cpp
// Allocate single variable
int* p = new int;
*p = 42;

// Allocate array
int* arr = new int[5];
for (int i = 0; i < 5; i++) {
    arr[i] = i + 1;
}
```

### 3.2 Deallocate with delete

```cpp
// Deallocate single variable
delete p;

// Deallocate array
delete[] arr;
```

### 3.3 Null Pointer Check

```cpp
int* p = new (nothrow) int;
if (p == nullptr) {
    cout << "Memory allocation failed";
}
```

## 4. References

### 4.1 Reference Definition

A reference is an alias for a variable and must be initialized at declaration.

```cpp
int x = 10;
int& ref = x;  // ref is an alias for x

ref = 20;      // Modify ref, x is also modified
cout << x;     // Output: 20
```

### 4.2 Pointer vs Reference

| Feature | Pointer | Reference |
|---------|---------|-----------|
| Null Value | Can be null | Cannot be null |
| Reassignment | Can point to another variable | Always refers to initial variable |
| Operators | Uses * and -> | Used directly |
| Memory Overhead | Occupies memory | No extra memory |

### 4.3 Uses of References

#### As Function Parameters

```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

// Usage
int x = 5, y = 10;
swap(x, y);
```

#### As Function Return Value

```cpp
int& getElement(int arr[], int index) {
    return arr[index];
}

// Usage
int arr[5] = {1, 2, 3, 4, 5};
getElement(arr, 2) = 100;  // Modify third element
```

## 5. Advanced Pointer/Reference Techniques

### 5.1 Pointer to Pointer

```cpp
int x = 10;
int* p = &x;
int** pp = &p;

cout << **pp;  // Output: 10
**pp = 20;     // Modify x
```

### 5.2 Array of Pointers

```cpp
int a = 1, b = 2, c = 3;
int* arr[3] = {&a, &b, &c};

cout << *arr[0];  // Output: 1
cout << *arr[1];  // Output: 2
```

### 5.3 Pointer to Array

```cpp
int arr[3][4];
int (*p)[4] = arr;  // p points to array of 4 ints

p++;  // Move to next row
```

## 6. const with Pointers/References

### 6.1 const Pointers

```cpp
int x = 10;

// Pointer to const (value cannot be modified)
const int* p = &x;
// *p = 20;  // Error

// const pointer (pointer itself cannot be modified)
int* const p = &x;
// p = &y;  // Error

// const pointer to const
const int* const p = &x;
```

### 6.2 const References

```cpp
int x = 10;
const int& ref = x;
// ref = 20;  // Error
```

## 7. Smart Pointers

### 7.1 unique_ptr

Smart pointer with exclusive ownership.

```cpp
#include <memory>

std::unique_ptr<int> p = std::make_unique<int>(42);
std::unique_ptr<int[]> arr = std::make_unique<int[]>(5);
```

### 7.2 shared_ptr

Smart pointer with shared ownership.

```cpp
std::shared_ptr<int> p1 = std::make_shared<int>(10);
std::shared_ptr<int> p2 = p1;  // Reference count increases
```

### 7.3 weak_ptr

Weak reference smart pointer, doesn't increase reference count.

```cpp
std::weak_ptr<int> wp = p1;
if (auto sp = wp.lock()) {
    // Use sp
}
```

## 8. Common Pitfalls

### 8.1 Wild Pointer

```cpp
int* p;           // Uninitialized pointer (wild pointer)
*p = 10;          // Dangerous! Undefined behavior
```

### 8.2 Dangling Pointer

```cpp
int* p = new int(10);
delete p;
*p = 20;          // Dangerous! Pointer already freed
```

### 8.3 Null Pointer Dereference

```cpp
int* p = nullptr;
*p = 10;          // Error! Dereferencing null pointer
```

## 9. Practical Examples

### Example 1: Reverse Array Using Pointers

```cpp
#include <iostream>
using namespace std;

void reverse(int* arr, int size) {
    int* start = arr;
    int* end = arr + size - 1;
    
    while (start < end) {
        int temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }
}

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    reverse(arr, size);
    
    for (int i = 0; i < size; i++) {
        cout << arr[i] << " ";
    }
    return 0;
}
```

### Example 2: Dynamic 2D Array

```cpp
#include <iostream>
using namespace std;

int main() {
    int rows = 3, cols = 4;
    
    // Allocate row pointers
    int** matrix = new int*[rows];
    
    // Allocate each row
    for (int i = 0; i < rows; i++) {
        matrix[i] = new int[cols];
    }
    
    // Fill data
    int count = 1;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            matrix[i][j] = count++;
        }
    }
    
    // Output
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }
    
    // Deallocate
    for (int i = 0; i < rows; i++) {
        delete[] matrix[i];
    }
    delete[] matrix;
    
    return 0;
}
```

## 10. Summary

| Concept | Description |
|---------|-------------|
| Pointer | Variable that stores memory address |
| Dereference | Use * to access pointed value |
| Reference | Alias for a variable, must be initialized |
| Dynamic Memory | Managed with new/delete |
| Smart Pointer | Automatic memory management |
| const Pointer | Protects pointed value or pointer itself |

Pointers and references are core concepts in C++. Mastering them is essential for writing efficient and flexible code.
