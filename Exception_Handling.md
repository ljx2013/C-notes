
# Exception Handling in C++

## 1. Overview of Exception Handling

Exception handling is a mechanism for dealing with runtime errors, allowing programs to gracefully recover or report errors when they occur.

### 1.1 Components of Exception Handling

- **try**: Identifies code blocks that may throw exceptions
- **catch**: Catches and handles exceptions
- **throw**: Throws an exception

## 2. Basic Syntax

### 2.1 try-catch Structure

```cpp
try {
    // Code that may throw exceptions
    if (error_condition) {
        throw exception_object;
    }
} catch (exception_type1 e) {
    // Handle type1 exception
} catch (exception_type2 e) {
    // Handle type2 exception
}
```

### 2.2 Example: Basic Exception Handling

```cpp
#include <iostream>
using namespace std;

int divide(int a, int b) {
    if (b == 0) {
        throw "Division by zero!";
    }
    return a / b;
}

int main() {
    try {
        int result = divide(10, 0);
        cout << "Result: " << result << endl;
    } catch (const char* msg) {
        cout << "Error: " << msg << endl;
    }
    return 0;
}
```

## 3. Standard Exception Classes

### 3.1 Exception Class Hierarchy

```
exception
├── bad_alloc
├── bad_cast
├── bad_typeid
├── bad_exception
└── logic_error
    ├── domain_error
    ├── invalid_argument
    ├── length_error
    └── out_of_range
└── runtime_error
    ├── range_error
    ├── overflow_error
    └── underflow_error
```

### 3.2 Using Standard Exceptions

```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

void validateAge(int age) {
    if (age < 0) {
        throw invalid_argument("Age cannot be negative");
    }
    if (age > 120) {
        throw out_of_range("Age out of valid range");
    }
}

int main() {
    try {
        validateAge(-5);
    } catch (const invalid_argument& e) {
        cout << "Invalid argument: " << e.what() << endl;
    } catch (const out_of_range& e) {
        cout << "Out of range: " << e.what() << endl;
    }
    return 0;
}
```

## 4. Custom Exception Classes

### 4.1 Creating Custom Exceptions

```cpp
#include <exception>
#include <string>
using namespace std;

class MyException : public exception {
private:
    string message;
public:
    MyException(const string& msg) : message(msg) {}
    
    const char* what() const noexcept override {
        return message.c_str();
    }
};

void doSomething() {
    throw MyException("Something went wrong");
}
```

### 4.2 Exception Inheritance Hierarchy

```cpp
class NetworkException : public exception {
protected:
    string message;
public:
    NetworkException(const string& msg) : message(msg) {}
    const char* what() const noexcept override {
        return message.c_str();
    }
};

class ConnectionException : public NetworkException {
public:
    ConnectionException(const string& msg) : NetworkException(msg) {}
};
```

## 5. Advanced Exception Features

### 5.1 Catch All Exceptions

```cpp
try {
    // Code that may throw various exceptions
} catch (...) {
    cout << "Unknown exception caught" << endl;
}
```

### 5.2 Re-throwing Exceptions

```cpp
void process() {
    try {
        // Code
    } catch (const exception& e) {
        // Partial handling
        throw;  // Re-throw the original exception
    }
}
```

### 5.3 noexcept Specifier

```cpp
void func() noexcept {
    // Promises not to throw exceptions
}

void func() noexcept(false) {
    // May throw exceptions (default)
}
```

## 6. Exception Safety

### 6.1 RAII Principle

Resource Acquisition Is Initialization - use destructors to ensure resource cleanup.

```cpp
class FileHandler {
private:
    FILE* file;
public:
    FileHandler(const char* filename) {
        file = fopen(filename, "r");
        if (!file) {
            throw runtime_error("Failed to open file");
        }
    }
    
    ~FileHandler() {
        if (file) {
            fclose(file);  // Guaranteed cleanup
        }
    }
};
```

### 6.2 Exception Safety Levels

| Level | Guarantee |
|-------|-----------|
| no-throw | Guaranteed not to throw |
| strong | State unchanged on exception |
| basic | No resource leaks |

## 7. Best Practices

### 7.1 When to Use Exceptions

- Handle unexpected errors
- Error occurs at subsystem boundaries
- Need to propagate errors across function calls

### 7.2 When Not to Use Exceptions

- Performance-critical code (exceptions have overhead)
- Simple parameter validation (use return values)
- Compile-time errors (use static assertions)

### 7.3 Guidelines

1. Use exceptions only for exceptional conditions
2. Provide meaningful error messages
3. Avoid resource leaks
4. Use RAII for resource management
5. Don't throw exceptions from destructors

## 8. Practical Examples

### Example 1: Complete Exception Handling

```cpp
#include <iostream>
#include <stdexcept>
#include <vector>
using namespace std;

int getElement(const vector<int>& vec, int index) {
    if (index < 0 || index >= vec.size()) {
        throw out_of_range("Index out of bounds");
    }
    return vec[index];
}

int main() {
    vector<int> nums = {1, 2, 3, 4, 5};
    
    try {
        cout << getElement(nums, 2) << endl;
        cout << getElement(nums, 10) << endl;
    } catch (const out_of_range& e) {
        cout << "Error: " << e.what() << endl;
    }
    
    cout << "Program continues..." << endl;
    return 0;
}
```

### Example 2: Exceptions and Resource Management

```cpp
#include <iostream>
#include <memory>
#include <stdexcept>
using namespace std;

class Database {
public:
    Database() {
        cout << "Connecting to database..." << endl;
    }
    
    ~Database() {
        cout << "Disconnecting from database..." << endl;
    }
    
    void query() {
        throw runtime_error("Query failed");
    }
};

int main() {
    try {
        unique_ptr<Database> db = make_unique<Database>();
        db->query();
    } catch (const exception& e) {
        cout << "Error: " << e.what() << endl;
    }
    // db automatically destroyed, resources properly released
    return 0;
}
```

## 9. Summary

| Concept | Description |
|---------|-------------|
| try | Marks code that may throw exceptions |
| catch | Catches and handles exceptions |
| throw | Raises an exception |
| Standard Exceptions | exception and its derived classes |
| Custom Exceptions | Inherit from exception class |
| noexcept | Declares whether function throws |
| RAII | Automatic resource management |

Exception handling is essential for writing robust programs. Proper use significantly improves code reliability and maintainability.
