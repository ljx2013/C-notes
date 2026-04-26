
# Advanced Input/Output in C++ (scanf/printf)

## 1. Overview

In C++, besides using C++-style stream operations (`cin`/`cout`), you can also use C-style `scanf` and `printf` functions. These functions are more efficient and flexible in certain scenarios, especially when precise format control is required.

### 1.1 Header File

To use `scanf` and `printf`, include the C standard library header:

```cpp
#include <cstdio>  // C++-style include
// or
#include <stdio.h> // C-style include
```

### 1.2 Performance Comparison

| Feature | cin/cout | scanf/printf |
|---------|----------|--------------|
| Speed | Slower (by default synchronized) | Faster |
| Format Control | Complex | Concise and flexible |
| Type Safety | Yes | No |
| Ease of Use | High | Medium |

## 2. printf Function Detailed

### 2.1 Basic Syntax

```cpp
printf("format string", arg1, arg2, ...);
```

### 2.2 Format Specifiers

Format specifiers start with `%` and specify the output format:

| Specifier | Meaning | Example |
|-----------|---------|---------|
| `%d` | Decimal integer | 42 |
| `%i` | Decimal integer (same as %d) | 42 |
| `%u` | Unsigned decimal integer | 42 |
| `%o` | Octal integer | 52 |
| `%x` | Hexadecimal (lowercase) | 2a |
| `%X` | Hexadecimal (uppercase) | 2A |
| `%f` | Floating-point (6 decimal places) | 3.141593 |
| `%e` | Scientific notation (lowercase) | 3.141593e+00 |
| `%E` | Scientific notation (uppercase) | 3.141593E+00 |
| `%g` | Auto-select %f or %e | 3.14159 |
| `%c` | Single character | A |
| `%s` | String | Hello |
| `%p` | Pointer address | 0x7fff5fbff8ac |
| `%%` | Output percent sign | % |

### 2.3 Basic Usage Examples

```cpp
#include <cstdio>

int main() {
    int num = 42;
    double pi = 3.1415926;
    char ch = 'A';
    const char* str = "Hello";
    
    printf("Integer: %d\n", num);
    printf("Double: %f\n", pi);
    printf("Character: %c\n", ch);
    printf("String: %s\n", str);
    
    return 0;
}
```

### 2.4 Width and Precision Control

#### Setting Width

```cpp
printf("%5d\n", 42);      // Output: "   42" (width 5, right-aligned)
printf("%-5d\n", 42);     // Output: "42   " (width 5, left-aligned)
printf("%10s\n", "Hi");   // Output: "        Hi"
```

#### Setting Precision

```cpp
printf("%.2f\n", 3.14159);  // Output: 3.14 (2 decimal places)
printf("%.5s\n", "Hello");  // Output: Hello (max 5 characters)
printf("%10.2f\n", 3.14159); // Output: "      3.14" (width 10, precision 2)
```

### 2.5 Comprehensive Example

```cpp
#include <cstdio>

int main() {
    printf("|%5d|\n", 123);     // |  123|
    printf("|%-5d|\n", 123);    // |123  |
    printf("|%05d|\n", 123);    // |00123| (zero-padding)
    printf("|%+d|\n", 123);     // |+123| (show sign)
    printf("|% d|\n", 123);     // | 123| (space for positive)
    
    double num = 12345.6789;
    printf("|%10.2f|\n", num);  // |  12345.68|
    printf("|%e|\n", num);      // |1.234568e+04|
    printf("|%g|\n", num);      // |12345.7|
    
    return 0;
}
```

## 3. scanf Function Detailed

### 3.1 Basic Syntax

```cpp
scanf("format string", &var1, &var2, ...);
```

**Note**: Always use `&` address-of operator before variables (except arrays and pointers).

### 3.2 Format Specifiers

| Specifier | Meaning | Corresponding Type |
|-----------|---------|--------------------|
| `%d` | Decimal integer | int* |
| `%i` | Integer (auto-detect base) | int* |
| `%u` | Unsigned integer | unsigned int* |
| `%o` | Octal integer | int* |
| `%x` | Hexadecimal integer | int* |
| `%f` | Float | float* |
| `%lf` | Double | double* |
| `%c` | Single character | char* |
| `%s` | String | char* |
| `%p` | Pointer address | void** |

### 3.3 Basic Usage Examples

```cpp
#include <cstdio>

int main() {
    int num;
    double pi;
    char ch;
    char str[100];
    
    printf("Enter an integer: ");
    scanf("%d", &num);
    
    printf("Enter a double: ");
    scanf("%lf", &pi);
    
    printf("Enter a character: ");
    scanf(" %c", &ch);  // Note the space to skip whitespace
    
    printf("Enter a string: ");
    scanf("%s", str);   // Array name is already an address
    
    printf("num=%d, pi=%.2f, ch=%c, str=%s\n", num, pi, ch, str);
    
    return 0;
}
```

### 3.4 Input Width Limit

```cpp
#include <cstdio>

int main() {
    char str[10];
    scanf("%9s", str);  // Read at most 9 chars, leave space for '\0'
    printf("You entered: %s\n", str);
    return 0;
}
```

### 3.5 Mixed Input

```cpp
#include <cstdio>

int main() {
    int day, month, year;
    printf("Enter date (dd/mm/yyyy): ");
    scanf("%d/%d/%d", &day, &month, &year);
    printf("Date: %02d-%02d-%04d\n", day, month, year);
    return 0;
}
```

### 3.6 Return Value

`scanf` returns the number of successfully read variables:

```cpp
#include <cstdio>

int main() {
    int a, b;
    printf("Enter two numbers: ");
    int result = scanf("%d %d", &a, &b);
    
    if (result == 2) {
        printf("Both numbers read successfully\n");
    } else if (result == 1) {
        printf("Only one number read\n");
    } else {
        printf("No numbers read\n");
    }
    
    return 0;
}
```

## 4. Advanced Usage

### 4.1 printf Flags

| Flag | Effect | Example |
|------|--------|---------|
| `-` | Left-align | `%-5d` |
| `+` | Show sign | `%+d` |
| ` ` | Space for positive | `% d` |
| `0` | Zero-padding | `%05d` |
| `#` | Show base prefix | `%#x` → 0x |

### 4.2 Variable Arguments Example

```cpp
#include <cstdio>
#include <cstdarg>

void printNumbers(int count, ...) {
    va_list args;
    va_start(args, count);
    
    for (int i = 0; i < count; i++) {
        int num = va_arg(args, int);
        printf("%d ", num);
    }
    
    va_end(args);
    printf("\n");
}

int main() {
    printNumbers(3, 10, 20, 30);  // Output: 10 20 30
    printNumbers(5, 1, 2, 3, 4, 5); // Output: 1 2 3 4 5
    return 0;
}
```

### 4.3 Format to String (sprintf)

```cpp
#include <cstdio>

int main() {
    char buffer[100];
    int num = 42;
    double pi = 3.14159;
    
    sprintf(buffer, "Number: %d, Pi: %.2f", num, pi);
    printf("%s\n", buffer);  // Output: Number: 42, Pi: 3.14
    
    return 0;
}
```

### 4.4 Read from String (sscanf)

```cpp
#include <cstdio>

int main() {
    const char* str = "John 25 175.5";
    char name[20];
    int age;
    double height;
    
    sscanf(str, "%s %d %lf", name, &age, &height);
    printf("Name: %s, Age: %d, Height: %.2f\n", name, age, height);
    
    return 0;
}
```

## 5. Common Issues

### 5.1 scanf Skips Whitespace

`scanf` automatically skips whitespace (spaces, newlines, tabs):

```cpp
// Input: "  42  "
int num;
scanf("%d", &num);  // Reads 42 correctly
```

### 5.2 Reading Characters

```cpp
int num;
char ch;

scanf("%d", &num);
scanf("%c", &ch);  // Reads the newline character '\n'

// Solution:
scanf("%d", &num);
scanf(" %c", &ch); // Add space before %c
```

### 5.3 Buffer Overflow Risk

```cpp
char str[10];
scanf("%s", str);  // Dangerous! Overflow if input > 9 chars

// Safe approach:
scanf("%9s", str);  // Limit to 9 characters
```

## 6. cin/cout vs scanf/printf

### 6.1 Performance Test

```cpp
#include <cstdio>
#include <iostream>
#include <time.h>

const int N = 1000000;

int main() {
    // printf test
    clock_t start = clock();
    for (int i = 0; i < N; i++) {
        printf("%d\n", i);
    }
    printf("printf time: %f\n", (double)(clock() - start) / CLOCKS_PER_SEC);
    
    // cout test (disable synchronization)
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    
    start = clock();
    for (int i = 0; i < N; i++) {
        std::cout << i << '\n';
    }
    printf("cout time: %f\n", (double)(clock() - start) / CLOCKS_PER_SEC);
    
    return 0;
}
```

### 6.2 Selection Recommendations

| Scenario | Recommended |
|----------|-------------|
| Large data output | printf (faster) |
| Precise format control | printf |
| Type safety critical | cin/cout |
| Simple I/O operations | Either |
| Mixed usage | Be cautious about synchronization |

## 7. Practical Examples

### Example 1: Format Table Output

```cpp
#include <cstdio>

int main() {
    printf("+--------+--------+--------+\n");
    printf("|  Name  |  Age   |  Score |\n");
    printf("+--------+--------+--------+\n");
    printf("| %-6s | %6d | %6.1f |\n", "Alice", 20, 95.5);
    printf("| %-6s | %6d | %6.1f |\n", "Bob", 22, 88.0);
    printf("| %-6s | %6d | %6.1f |\n", "Charlie", 19, 92.3);
    printf("+--------+--------+--------+\n");
    return 0;
}
```

### Example 2: Parse Command Line Arguments

```cpp
#include <cstdio>

int main(int argc, char* argv[]) {
    if (argc != 3) {
        printf("Usage: %s <width> <height>\n", argv[0]);
        return 1;
    }
    
    int width, height;
    if (sscanf(argv[1], "%d", &width) != 1 || 
        sscanf(argv[2], "%d", &height) != 1) {
        printf("Invalid arguments\n");
        return 1;
    }
    
    printf("Area: %d\n", width * height);
    return 0;
}
```

## 8. Summary

| Function | Purpose | Characteristics |
|----------|---------|-----------------|
| `printf` | Formatted output | Fast, flexible, strong format control |
| `scanf` | Formatted input | Fast, but requires address operator |
| `sprintf` | Format to string | Generate formatted strings |
| `sscanf` | Read from string | Parse data from strings |

Mastering `scanf` and `printf` is essential for C++ programmers, especially in performance-critical applications and when precise format control is needed.
