
# C++ Header Files Explained

## 1. Definition and Purpose of Header Files

### 1.1 What is a Header File

A header file is a file in C++ that declares functions, classes, constants, and templates. It typically uses `.h`, `.hpp`, or `.hxx` extensions. Header files contain interface declarations, not implementation code.

### 1.2 Core Functions of Header Files

- **Interface Declaration**: Declare interfaces for functions, classes, structs, enums, etc.
- **Code Reuse**: Share interface definitions across multiple source files
- **Compilation Optimization**: Reduce redundant compilation and improve efficiency
- **Modular Management**: Separate interface from implementation for easier maintenance

## 2. Naming Conventions

### 2.1 File Extensions

| Extension | Use Case | Description |
|-----------|----------|-------------|
| `.h` | C/C++ General | Traditional extension, compatible with C |
| `.hpp` | C++ Specific | Emphasizes C++ features like templates |
| `.hxx` | C++ Specific | Alternative C++ extension |
| `.tpp` | Template Implementation | Used with template header files |

### 2.2 Naming Rules

- Use lowercase letters and underscores
- File names should reflect content (e.g., `vector.hpp`, `socket.h`)
- Avoid spaces and special characters
- Internal project headers are usually placed in an `include` directory

## 3. Header Guard Mechanisms

### 3.1 Necessity of Preventing Duplicate Inclusion

When a header file is included multiple times, it causes redefinition errors:

```cpp
// Error: Redefinition
class MyClass {};  // First inclusion
class MyClass {};  // Second inclusion - compilation error
```

### 3.2 Three Protection Methods

#### Method 1: Preprocessor Macro Guard (Most Common)

```cpp
#ifndef MY_HEADER_H
#define MY_HEADER_H

// Header content

#endif // MY_HEADER_H
```

**Advantages**:
- Cross-platform compatible
- Fast compilation

**Disadvantages**:
- Requires manual macro definition
- Risk of macro name conflicts

#### Method 2: `#pragma once` (Modern Compiler Support)

```cpp
#pragma once

// Header content
```

**Advantages**:
- Concise, no macro name needed
- Compiler optimization support

**Disadvantages**:
- Not supported by some older compilers
- May have issues with complex directory structures

#### Method 3: `#import` (MSVC Only)

```cpp
#import "my_header.h"
```

**Advantages**: Automatically handles duplicate inclusion

**Disadvantages**: Only works with Microsoft Visual Studio

### 3.3 Recommended Practice

- Use `#pragma once` for new projects
- Use macro guard for backward compatibility
- Macro naming format: `PROJECT_MODULE_FILE_H`

## 4. Header File Inclusion

### 4.1 Inclusion Syntax

```cpp
#include <iostream>    // Standard library header, use angle brackets
#include "my_header.h" // Custom header, use double quotes
```

### 4.2 Inclusion Order (Google Style)

1. The header file corresponding to the current source file (e.g., `foo.cpp` first includes `foo.h`)
2. C standard library headers (e.g., `<stdio.h>`)
3. C++ standard library headers (e.g., `<iostream>`)
4. Third-party library headers
5. Internal project headers

**Example**:
```cpp
#include "foo.h"            // 1. Current file's header
#include <cstdio>           // 2. C standard library
#include <iostream>         // 3. C++ standard library
#include <boost/array.hpp>  // 4. Third-party library
#include "base/logging.h"   // 5. Internal project headers
```

### 4.3 Forward Declaration

When you only need to know a type exists without its full definition:

```cpp
// Forward declare class
class MyClass;

// Forward declare function
void func();

// Forward declare template
template<typename T> class Vector;
```

**Advantages**:
- Reduce header dependencies
- Shorten compilation time

**Use Cases**:
- Declare pointers or references
- Declare function parameters or return types

**Not Suitable For**:
- Accessing class members
- Knowing the size of a class
- Inheriting from the class

## 5. Header File Content Guidelines

### 5.1 Content That Should Be in Headers

- **Class/struct/enum declarations**
- **Function prototypes**
- **Constant definitions**
- **Type aliases**
- **Template declarations**
- **Inline functions**

### 5.2 Content That Should NOT Be in Headers

- **Non-inline function implementations**
- **Global variable definitions** (declarations are OK)
- **Static variables**
- **Complex initialization logic**

### 5.3 Example: Well-Structured Header

```cpp
#pragma once

#include <string>
#include <vector>

namespace myproject {

using StringVector = std::vector<std::string>;

extern const int kMaxSize;

class DataProcessor;

enum class Status {
    kSuccess,
    kError,
    kPending
};

class Logger {
public:
    Logger();
    ~Logger();
    
    void Log(const std::string& message);
    Status GetStatus() const;

private:
    Status status_;
};

std::string FormatMessage(const std::string& prefix, int code);

inline int Add(int a, int b) {
    return a + b;
}

} // namespace myproject
```

## 6. Header File Classification

### 6.1 By Source

| Category | Examples | Inclusion Method |
|----------|----------|------------------|
| Standard Library | `<iostream>`, `<vector>` | `#include <>` |
| System Headers | `<windows.h>`, `<unistd.h>` | `#include <>` |
| Third-party Library | `<boost/asio.hpp>` | `#include <>` |
| Internal Project | `"utils/log.h"` | `#include ""` |

### 6.2 By Function

- **Interface Headers**: Public interfaces exposed to users
- **Implementation Headers**: Private interfaces for internal use
- **Template Headers**: Contain template definitions (usually all in header)

## 7. Common Issues and Solutions

### 7.1 Circular Dependency

**Problem**: Header A includes header B, and header B includes header A

**Solutions**:
1. Use forward declaration instead of inclusion
2. Extract common parts into a third header
3. Redesign class dependencies

**Example**:

```cpp
// a.h
#pragma once
class B;  // Forward declaration

class A {
    B* b_;  // Use pointer, no full definition needed
};

// b.h
#pragma once
#include "a.h"  // Now safe to include

class B {
    A a_;  // Needs full definition of A
};
```

### 7.2 Header Bloat

**Problem**: Headers include too many other headers, causing long compilation times

**Solutions**:
1. Use forward declarations
2. Split headers into smaller modules
3. Use Pimpl idiom to hide implementation details

**Pimpl Idiom Example**:

```cpp
// widget.h
#pragma once
#include <memory>

class WidgetImpl;  // Forward declaration

class Widget {
public:
    Widget();
    ~Widget();
    void DoSomething();

private:
    std::unique_ptr<WidgetImpl> impl_;
};

// widget.cpp
#include "widget.h"
#include "widget_impl.h"

Widget::Widget() : impl_(std::make_unique<WidgetImpl>()) {}
Widget::~Widget() = default;
void Widget::DoSomething() {
    impl_->DoSomething();
}
```

### 7.3 Linker Error: Multiple Definition

**Problem**: Defining non-inline functions or global variables in headers

**Solutions**:
- Move function implementations to `.cpp` files
- Use `inline` keyword for functions
- Use `static` or anonymous namespace to limit scope

## 8. Practical Recommendations

### 8.1 Header Organization Strategy

```
project/
├── include/
│   ├── project_name/
│   │   ├── core/          # Core modules
│   │   ├── utils/         # Utility classes
│   │   ├── network/       # Network modules
│   │   └── config.h       # Configuration header
│   └── project_name.h     # Unified entry header
└── src/
    └── ...
```

### 8.2 Unified Entry Header

Create a unified entry header for easy inclusion:

```cpp
// project_name.h
#pragma once

#include "project_name/core/base.h"
#include "project_name/utils/logging.h"
#include "project_name/network/client.h"
// ... other core headers
```

### 8.3 Avoid Over-Inclusion

Use compiler options to check for unnecessary includes:
- GCC/Clang: `-Wunused-include`
- MSVC: `/showIncludes`

## 9. Summary

| Key Point | Description |
|-----------|-------------|
| Protection | Prefer `#pragma once`, use macro guard for compatibility |
| Inclusion Order | Current header → C std → C++ std → Third-party → Internal |
| Content | Declarations only, no implementations; use forward declarations |
| Circular Dependency | Solve with forward declarations and Pimpl idiom |
| Organization | Modular structure with unified entry point |

Header files are the foundation of C++ modular programming. Well-designed headers significantly improve code maintainability and compilation efficiency.
