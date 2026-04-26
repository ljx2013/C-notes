
# File Operations in C++

## 1. Overview

File operations are essential for programs to interact with external storage. C++ provides two approaches:

1. **C-style**: Functions from `<cstdio>`
2. **C++-style**: Stream classes from `<fstream>`

## 2. C++-style File Operations

### 2.1 Header File

```cpp
#include <fstream>
```

### 2.2 File Stream Types

| Type | Purpose |
|------|---------|
| `ifstream` | Read files |
| `ofstream` | Write files |
| `fstream` | Read and write files |

### 2.3 File Open Modes

| Mode | Description |
|------|-------------|
| `ios::in` | Read mode |
| `ios::out` | Write mode |
| `ios::app` | Append mode |
| `ios::trunc` | Truncate mode |
| `ios::binary` | Binary mode |
| `ios::ate` | Seek to end |

## 3. Text File Operations

### 3.1 Writing to Text Files

```cpp
#include <fstream>
#include <iostream>
using namespace std;

int main() {
    ofstream outfile("example.txt");
    
    if (outfile.is_open()) {
        outfile << "Hello, World!" << endl;
        outfile << "This is a test file." << endl;
        outfile.close();
    } else {
        cout << "Unable to open file" << endl;
    }
    
    return 0;
}
```

### 3.2 Reading from Text Files

```cpp
#include <fstream>
#include <iostream>
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

### 3.3 Reading Word by Word

```cpp
#include <fstream>
#include <iostream>
#include <string>
using namespace std;

int main() {
    ifstream infile("example.txt");
    string word;
    
    while (infile >> word) {
        cout << word << endl;
    }
    
    return 0;
}
```

## 4. Binary File Operations

### 4.1 Writing Binary Files

```cpp
#include <fstream>
using namespace std;

struct Person {
    char name[50];
    int age;
    double height;
};

int main() {
    Person p = {"Alice", 25, 165.5};
    
    ofstream outfile("person.dat", ios::binary);
    if (outfile) {
        outfile.write(reinterpret_cast<char*>(&p), sizeof(p));
        outfile.close();
    }
    
    return 0;
}
```

### 4.2 Reading Binary Files

```cpp
#include <fstream>
#include <iostream>
using namespace std;

struct Person {
    char name[50];
    int age;
    double height;
};

int main() {
    Person p;
    
    ifstream infile("person.dat", ios::binary);
    if (infile) {
        infile.read(reinterpret_cast<char*>(&p), sizeof(p));
        cout << "Name: " << p.name << endl;
        cout << "Age: " << p.age << endl;
        cout << "Height: " << p.height << endl;
        infile.close();
    }
    
    return 0;
}
```

## 5. File Positioning

### 5.1 seekg and seekp

```cpp
#include <fstream>
#include <iostream>
using namespace std;

int main() {
    fstream file("example.txt", ios::in | ios::out);
    
    if (file) {
        // Seek to 10th byte
        file.seekg(10);
        
        // Get current position
        cout << "Current position: " << file.tellg() << endl;
        
        // Seek to end
        file.seekg(0, ios::end);
        
        // Get file size
        int size = file.tellg();
        cout << "File size: " << size << endl;
        
        file.close();
    }
    
    return 0;
}
```

### 5.2 Seek Directions

| Direction | Description |
|-----------|-------------|
| `ios::beg` | From beginning |
| `ios::cur` | From current position |
| `ios::end` | From end |

## 6. File Status Checking

### 6.1 Status Functions

```cpp
#include <fstream>
using namespace std;

int main() {
    ifstream file("example.txt");
    
    if (file.is_open()) {
        bool good = file.good();   // Status is good
        bool eof = file.eof();     // End of file
        bool fail = file.fail();   // Operation failed
        bool bad = file.bad();     // Serious error
    }
    
    return 0;
}
```

## 7. C-style File Operations

### 7.1 Opening and Closing

```cpp
#include <cstdio>

int main() {
    FILE* file = fopen("example.txt", "w");
    
    if (file != nullptr) {
        fprintf(file, "Hello, World!\n");
        fclose(file);
    }
    
    return 0;
}
```

### 7.2 File Modes

| Mode | Description |
|------|-------------|
| `"r"` | Read only |
| `"w"` | Write (create or truncate) |
| `"a"` | Append |
| `"rb"` | Binary read |
| `"wb"` | Binary write |
| `"ab"` | Binary append |

### 7.3 Read/Write Operations

```cpp
#include <cstdio>

int main() {
    // Write
    FILE* out = fopen("data.txt", "w");
    if (out) {
        fprintf(out, "%d %s %.2f\n", 42, "hello", 3.14);
        fclose(out);
    }
    
    // Read
    FILE* in = fopen("data.txt", "r");
    if (in) {
        int num;
        char str[100];
        double val;
        fscanf(in, "%d %s %lf", &num, str, &val);
        fclose(in);
    }
    
    return 0;
}
```

## 8. Practical Examples

### Example 1: File Copy

```cpp
#include <fstream>
#include <iostream>
using namespace std;

bool copyFile(const string& source, const string& dest) {
    ifstream src(source, ios::binary);
    ofstream dst(dest, ios::binary);
    
    if (!src || !dst) {
        return false;
    }
    
    dst << src.rdbuf();
    return true;
}

int main() {
    if (copyFile("source.txt", "dest.txt")) {
        cout << "File copied successfully" << endl;
    } else {
        cout << "Failed to copy file" << endl;
    }
    return 0;
}
```

### Example 2: Count Lines

```cpp
#include <fstream>
#include <iostream>
#include <string>
using namespace std;

int countLines(const string& filename) {
    ifstream file(filename);
    if (!file) return -1;
    
    int count = 0;
    string line;
    while (getline(file, line)) {
        count++;
    }
    return count;
}

int main() {
    int lines = countLines("example.txt");
    if (lines >= 0) {
        cout << "Number of lines: " << lines << endl;
    } else {
        cout << "Unable to open file" << endl;
    }
    return 0;
}
```

### Example 3: Read CSV File

```cpp
#include <fstream>
#include <iostream>
#include <sstream>
#include <vector>
#include <string>
using namespace std;

vector<string> split(const string& s, char delimiter) {
    vector<string> tokens;
    string token;
    istringstream tokenStream(s);
    while (getline(tokenStream, token, delimiter)) {
        tokens.push_back(token);
    }
    return tokens;
}

int main() {
    ifstream file("data.csv");
    string line;
    
    while (getline(file, line)) {
        vector<string> fields = split(line, ',');
        for (const string& field : fields) {
            cout << field << "\t";
        }
        cout << endl;
    }
    
    return 0;
}
```

## 9. Summary

| Operation | C++-style | C-style |
|-----------|-----------|---------|
| Open | `ofstream`, `ifstream` | `fopen` |
| Write | `<<` operator | `fprintf`, `fwrite` |
| Read | `>>` operator, `getline` | `fscanf`, `fread` |
| Close | `close()` | `fclose` |
| Seek | `seekg`, `seekp` | `fseek` |
| Status | `good()`, `eof()`, `fail()` | `feof`, `ferror` |

File operations are crucial for practical programming. Mastering C++ file streams is essential for building useful applications.
