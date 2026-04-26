
# Template Programming in C++

## 1. Overview of Templates

Templates are C++'s generic programming tool, enabling type-independent code and improving code reuse.

### 1.1 Template Classification

- **Function Templates**: Create generic functions
- **Class Templates**: Create generic classes

## 2. Function Templates

### 2.1 Basic Syntax

```cpp
template <typename T>
T function_name(T parameter) {
    // Function body
}
```

### 2.2 Example: Generic Swap Function

```cpp
template <typename T>
void swap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}

// Usage
int x = 5, y = 10;
swap(x, y);

double a = 3.14, b = 2.71;
swap(a, b);
```

### 2.3 Multi-parameter Templates

```cpp
template <typename T, typename U>
auto add(T a, U b) {
    return a + b;
}

// Usage
auto result = add(5, 3.14);  // Returns double
```

### 2.4 Template Specialization

```cpp
// Generic template
template <typename T>
T max(T a, T b) {
    return a > b ? a : b;
}

// Specialization for const char*
template <>
const char* max<const char*>(const char* a, const char* b) {
    return strcmp(a, b) > 0 ? a : b;
}
```

## 3. Class Templates

### 3.1 Basic Syntax

```cpp
template <typename T>
class ClassName {
public:
    ClassName(T value);
    T getValue() const;
private:
    T data;
};
```

### 3.2 Example: Generic Container

```cpp
template <typename T>
class Box {
private:
    T item;
public:
    Box(T i) : item(i) {}
    T getItem() const { return item; }
    void setItem(T i) { item = i; }
};

// Usage
Box<int> intBox(42);
Box<std::string> strBox("Hello");
```

### 3.3 Member Function Definition

```cpp
template <typename T>
class MyClass {
public:
    void display(T value);
};

// Define member function outside class
template <typename T>
void MyClass<T>::display(T value) {
    std::cout << value << std::endl;
}
```

## 4. Template Parameters

### 4.1 Type Parameters

```cpp
template <typename T>  // T is a type parameter
T process(T value) { ... }
```

### 4.2 Non-type Parameters

```cpp
template <typename T, int size>
class Array {
private:
    T arr[size];
public:
    T& operator[](int index) { return arr[index]; }
};

// Usage
Array<int, 5> arr;  // size = 5
```

### 4.3 Default Template Parameters

```cpp
template <typename T = int>
class Container {
private:
    T data;
public:
    Container(T d = T()) : data(d) {}
};

// Usage
Container<> c1;      // T = int
Container<double> c2; // T = double
```

## 5. Advanced Template Features

### 5.1 Template Aliases

```cpp
template <typename T>
using Vec = std::vector<T>;

// Usage
Vec<int> numbers;  // Equivalent to std::vector<int>
```

### 5.2 Variadic Templates

```cpp
// Recursive termination
void print() {}

// Variadic template
template <typename T, typename... Args>
void print(T first, Args... args) {
    std::cout << first << " ";
    print(args...);  // Recursive call
}

// Usage
print(1, "hello", 3.14, 'A');
```

### 5.3 Fold Expressions (C++17)

```cpp
template <typename... Args>
auto sum(Args... args) {
    return (... + args);  // Fold sum
}

// Usage
sum(1, 2, 3, 4);  // Returns 10
```

## 6. Template Specialization

### 6.1 Class Template Specialization

```cpp
template <typename T>
class MyClass {
public:
    void display() { std::cout << "Generic\n"; }
};

// Specialization
template <>
class MyClass<int> {
public:
    void display() { std::cout << "Specialized for int\n"; }
};
```

### 6.2 Partial Specialization

```cpp
template <typename T, typename U>
class Pair {
public:
    void display() { std::cout << "Generic Pair\n"; }
};

// Partial specialization
template <typename T>
class Pair<T, T> {
public:
    void display() { std::cout << "Pair with same types\n"; }
};
```

## 7. SFINAE

### 7.1 Concept

SFINAE (Substitution Failure Is Not An Error): Substitution failures don't cause compilation errors.

```cpp
#include <type_traits>

template <typename T>
typename std::enable_if<std::is_integral<T>::value, T>::type
add(T a, T b) {
    return a + b;
}

template <typename T>
typename std::enable_if<std::is_floating_point<T>::value, T>::type
add(T a, T b) {
    return a + b;
}
```

### 7.2 constexpr if (C++17)

```cpp
template <typename T>
void process(T value) {
    if constexpr (std::is_integral_v<T>) {
        std::cout << "Integer: " << value << std::endl;
    } else if constexpr (std::is_floating_point_v<T>) {
        std::cout << "Float: " << value << std::endl;
    }
}
```

## 8. Practical Examples

### Example 1: Generic Stack Template

```cpp
#include <iostream>
#include <vector>
using namespace std;

template <typename T>
class Stack {
private:
    vector<T> elements;
public:
    void push(T element) {
        elements.push_back(element);
    }
    
    T pop() {
        T top = elements.back();
        elements.pop_back();
        return top;
    }
    
    bool empty() const {
        return elements.empty();
    }
};

int main() {
    Stack<int> intStack;
    intStack.push(1);
    intStack.push(2);
    cout << intStack.pop() << endl;  // 2
    
    Stack<string> strStack;
    strStack.push("Hello");
    strStack.push("World");
    cout << strStack.pop() << endl;  // World
    
    return 0;
}
```

### Example 2: Type Traits

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

template <typename T>
void printTypeInfo() {
    cout << "Type: " << typeid(T).name() << endl;
    cout << "Is integral: " << is_integral<T>::value << endl;
    cout << "Is pointer: " << is_pointer<T>::value << endl;
    cout << "Is reference: " << is_reference<T>::value << endl;
}

int main() {
    printTypeInfo<int>();
    printTypeInfo<double>();
    printTypeInfo<int*>();
    return 0;
}
```

## 9. Summary

| Concept | Description |
|---------|-------------|
| Function Template | Create type-independent functions |
| Class Template | Create type-independent classes |
| Type Parameter | typename T |
| Non-type Parameter | Constant expression |
| Default Parameter | Default template argument |
| Specialization | Custom implementation for specific types |
| SFINAE | Substitution failure is not an error |
| Fold Expression | Expand variadic parameter packs |

Templates are central to C++ generic programming. Mastering templates enables writing highly reusable code.
