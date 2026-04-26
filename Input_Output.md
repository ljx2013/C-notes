
# C++ Input and Output

## 1. Overview

C++ input and output (I/O) operations are primarily implemented through streams provided by the standard library. The standard library defines several stream objects and operators that make data input and output simple and intuitive.

### 1.1 Standard Stream Objects

C++ standard library provides four predefined stream objects:

| Stream Object | Meaning | Associated Device |
|---------------|---------|-------------------|
| `cin` | Standard Input | Keyboard |
| `cout` | Standard Output | Display |
| `cerr` | Standard Error | Display |
| `clog` | Standard Log | Display |

### 1.2 Header File

To use standard I/O operations, include the header file:

```cpp
#include <iostream>
```

## 2. Basic Output Operations

### 2.1 Using cout for Output

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, World!" << endl;
    cout << "Value is: " << 42 << endl;
    return 0;
}
```

### 2.2 Output Operator `<<`

The `<<` operator is called the insertion operator and can be chained:

```cpp
int a = 10;
double b = 3.14;
cout << "a = " << a << ", b = " << b << endl;
```

### 2.3 Newline Characters

There are two ways to create a new line:

```cpp
cout << "Line 1\n";           // Using escape character
cout << "Line 2" << endl;     // Using endl manipulator
```

**Difference**:
- `\n` only outputs a newline character
- `endl` outputs a newline and flushes the buffer

## 3. Basic Input Operations

### 3.1 Using cin for Input

```cpp
#include <iostream>
using namespace std;

int main() {
    int num;
    cout << "Enter a number: ";
    cin >> num;
    cout << "You entered: " << num << endl;
    return 0;
}
```

### 3.2 Input Operator `>>`

The `>>` operator is called the extraction operator and can also be chained:

```cpp
int x, y;
cout << "Enter two numbers: ";
cin >> x >> y;
cout << "Sum: " << x + y << endl;
```

### 3.3 Inputting Strings

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    cout << "Enter your name: ";
    cin >> name;  // Reads until whitespace
    cout << "Hello, " << name << "!" << endl;
    return 0;
}
```

## 4. Formatted Output

### 4.1 Setting Output Width

```cpp
#include <iostream>
#include <iomanip>  // Required for manipulators
using namespace std;

int main() {
    cout << setw(10) << "Name" << setw(10) << "Age" << endl;
    cout << setw(10) << "Alice" << setw(10) << 25 << endl;
    return 0;
}
```

### 4.2 Setting Decimal Precision

```cpp
double pi = 3.1415926535;
cout << "Default: " << pi << endl;
cout << "Fixed: " << fixed << setprecision(2) << pi << endl;
cout << "Scientific: " << scientific << setprecision(3) << pi << endl;
```

### 4.3 Setting Alignment

```cpp
cout << left << setw(10) << "Left" << endl;
cout << right << setw(10) << "Right" << endl;
cout << internal << setw(10) << -123 << endl;
```

### 4.4 Number Base Conversion

```cpp
int num = 42;
cout << "Decimal: " << dec << num << endl;   // Decimal
cout << "Octal: " << oct << num << endl;     // Octal
cout << "Hex: " << hex << num << endl;       // Hexadecimal
cout << "Hex uppercase: " << uppercase << hex << num << endl;
```

## 5. Character Input/Output

### 5.1 Single Character Input

```cpp
char ch;
cin >> ch;           // Skips whitespace
cin.get(ch);         // Reads any character (including space)
ch = cin.get();      // Reads and returns character
```

### 5.2 Ignoring Characters

```cpp
cin.ignore();        // Ignore one character
cin.ignore(10);      // Ignore up to 10 characters
cin.ignore(numeric_limits<streamsize>::max(), '\n');  // Ignore entire line
```

## 6. String Input/Output

### 6.1 Reading Entire Lines

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string line;
    cout << "Enter a line: ";
    getline(cin, line);  // Reads entire line including spaces
    cout << "You entered: " << line << endl;
    return 0;
}
```

### 6.2 Mixed Input Considerations

When mixing `cin >>` and `getline`, be cautious:

```cpp
int age;
string name;

cin >> age;
cin.ignore();          // Ignore the newline character
getline(cin, name);    // Now reads correctly
```

## 7. File Input/Output

### 7.1 Writing to Files

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    ofstream outfile("example.txt");
    if (outfile.is_open()) {
        outfile << "Hello, File!" << endl;
        outfile << "Number: " << 42 << endl;
        outfile.close();
    } else {
        cout << "Unable to open file" << endl;
    }
    return 0;
}
```

### 7.2 Reading from Files

```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
    ifstream infile("example.txt");
    string line;
    if (infile.is_open()) {
        while (getline(infile, line)) {
            cout << line << endl;
        }
        infile.close();
    } else {
        cout << "Unable to open file" << endl;
    }
    return 0;
}
```

### 7.3 File Modes

| Mode | Meaning |
|------|---------|
| `ios::in` | Read mode (default for ifstream) |
| `ios::out` | Write mode (default for ofstream) |
| `ios::app` | Append mode |
| `ios::binary` | Binary mode |
| `ios::trunc` | Truncate mode (overwrite existing content) |
| `ios::ate` | Seek to end of file |

```cpp
ofstream outfile("data.txt", ios::out | ios::app);  // Append mode
```

## 8. Error Handling

### 8.1 Checking Stream State

```cpp
int num;
cin >> num;
if (cin.fail()) {
    cout << "Input error!" << endl;
    cin.clear();  // Clear error flags
    cin.ignore(numeric_limits<streamsize>::max(), '\n');  // Ignore bad input
}
```

### 8.2 Stream State Flags

| Flag | Meaning |
|------|---------|
| `good()` | Stream state is normal |
| `eof()` | End of file reached |
| `fail()` | Input/output operation failed |
| `bad()` | Serious stream error occurred |

## 9. Practical Examples

### Example 1: Simple Calculator

```cpp
#include <iostream>
using namespace std;

int main() {
    double num1, num2;
    char op;
    
    cout << "Enter expression (e.g., 3 + 4): ";
    cin >> num1 >> op >> num2;
    
    switch(op) {
        case '+': cout << num1 + num2 << endl; break;
        case '-': cout << num1 - num2 << endl; break;
        case '*': cout << num1 * num2 << endl; break;
        case '/': 
            if (num2 != 0) cout << num1 / num2 << endl;
            else cout << "Division by zero!" << endl;
            break;
        default: cout << "Invalid operator" << endl;
    }
    return 0;
}
```

### Example 2: File Copy

```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
    ifstream source("source.txt", ios::binary);
    ofstream dest("dest.txt", ios::binary);
    
    if (source && dest) {
        dest << source.rdbuf();  // Direct buffer copy
        cout << "File copied successfully" << endl;
    } else {
        cout << "Error copying file" << endl;
    }
    return 0;
}
```

## 10. Summary

| Operation Type | Tool | Purpose |
|----------------|------|---------|
| Standard Output | `cout` | Output data to screen |
| Standard Input | `cin` | Read data from keyboard |
| File Output | `ofstream` | Write data to files |
| File Input | `ifstream` | Read data from files |
| Formatting | `<iomanip>` | Control output format |

C++ I/O system provides rich functionality. Mastering basic input/output operations is essential for learning C++.
