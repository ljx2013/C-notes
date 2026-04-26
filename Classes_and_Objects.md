
# Classes and Objects in C++

## 1. Overview of Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm that encapsulates data and methods that operate on that data together. Core concepts include: class, object, encapsulation, inheritance, and polymorphism.

### 1.1 Class vs Object

- **Class**: Defines the blueprint or template for objects
- **Object**: Instance of a class

## 2. Class Definition

### 2.1 Basic Syntax

```cpp
class ClassName {
public:
    // Public members (accessible from outside)
private:
    // Private members (accessible only within class)
protected:
    // Protected members (accessible within class and derived classes)
};
```

### 2.2 Example: Simple Class

```cpp
class Person {
private:
    std::string name;
    int age;

public:
    void setName(std::string n) {
        name = n;
    }
    
    std::string getName() {
        return name;
    }
    
    void setAge(int a) {
        if (a >= 0) {
            age = a;
        }
    }
    
    int getAge() {
        return age;
    }
};
```

## 3. Creating and Using Objects

### 3.1 Creating Objects

```cpp
Person p;                    // Create on stack
Person* p = new Person();    // Create on heap
```

### 3.2 Accessing Members

```cpp
Person p;
p.setName("Alice");
p.setAge(25);

std::cout << p.getName() << " is " << p.getAge() << " years old";
```

## 4. Constructors and Destructors

### 4.1 Constructors

Constructors are called automatically when an object is created to initialize it.

```cpp
class Person {
private:
    std::string name;
    int age;

public:
    // Default constructor
    Person() {
        name = "Unknown";
        age = 0;
    }
    
    // Parameterized constructor
    Person(std::string n, int a) {
        name = n;
        age = a;
    }
};
```

### 4.2 Destructors

Destructors are called automatically when an object is destroyed to clean up resources.

```cpp
class Person {
public:
    ~Person() {
        // Clean up resources
    }
};
```

### 4.3 Constructor Initialization List

```cpp
class Person {
private:
    std::string name;
    int age;

public:
    Person(std::string n, int a) : name(n), age(a) {
        // Initialization list
    }
};
```

## 5. Encapsulation

### 5.1 Access Control

| Access Modifier | Inside Class | Derived Class | Outside |
|-----------------|--------------|---------------|---------|
| public | Yes | Yes | Yes |
| protected | Yes | Yes | No |
| private | Yes | No | No |

### 5.2 Benefits of Encapsulation

- Data hiding: Protect data from unauthorized access
- Interface simplification: Only expose necessary methods
- Ease of maintenance: Internal changes don't affect external code

## 6. Static Members

### 6.1 Static Member Variables

Belong to the class, not to any object. Shared by all objects.

```cpp
class Counter {
private:
    static int count;  // Static member variable

public:
    Counter() {
        count++;
    }
    
    static int getCount() {
        return count;
    }
};

// Static member must be initialized outside the class
int Counter::count = 0;
```

### 6.2 Static Member Functions

Don't depend on objects, can be called directly via class name.

```cpp
Counter::getCount();  // Call directly
```

## 7. Friends

### 7.1 Friend Functions

Allow external functions to access private members.

```cpp
class Box {
private:
    double width;
    
public:
    Box(double w) : width(w) {}
    
    friend double getWidth(Box box);
};

double getWidth(Box box) {
    return box.width;  // Can access private member
}
```

### 7.2 Friend Classes

Allow another class to access private members.

```cpp
class Box {
private:
    double width;
    
public:
    Box(double w) : width(w) {}
    
    friend class BoxPrinter;
};

class BoxPrinter {
public:
    void printWidth(Box box) {
        std::cout << box.width;  // Can access private member
    }
};
```

## 8. this Pointer

### 8.1 Using this Pointer

`this` is a pointer to the current object.

```cpp
class Person {
private:
    std::string name;

public:
    void setName(std::string name) {
        this->name = name;  // Distinguish parameter from member
    }
};
```

## 9. Object Arrays

```cpp
class Person {
private:
    std::string name;

public:
    Person(std::string n = "Unknown") : name(n) {}
    
    std::string getName() {
        return name;
    }
};

int main() {
    Person people[3] = {Person("Alice"), Person("Bob"), Person("Charlie")};
    
    for (int i = 0; i < 3; i++) {
        std::cout << people[i].getName() << std::endl;
    }
    
    return 0;
}
```

## 10. Practical Examples

### Example 1: Student Class

```cpp
#include <iostream>
#include <string>
using namespace std;

class Student {
private:
    string name;
    int id;
    double gpa;

public:
    Student(string n, int i, double g) : name(n), id(i), gpa(g) {}
    
    void display() {
        cout << "Name: " << name << endl;
        cout << "ID: " << id << endl;
        cout << "GPA: " << gpa << endl;
    }
    
    bool isHonors() {
        return gpa >= 3.5;
    }
};

int main() {
    Student s("Alice", 12345, 3.8);
    s.display();
    
    if (s.isHonors()) {
        cout << "Honors student!" << endl;
    }
    
    return 0;
}
```

### Example 2: Bank Account Class

```cpp
#include <iostream>
#include <string>
using namespace std;

class BankAccount {
private:
    string accountNumber;
    double balance;

public:
    BankAccount(string num, double bal) : accountNumber(num), balance(bal) {}
    
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    double getBalance() {
        return balance;
    }
};

int main() {
    BankAccount account("123456", 1000.0);
    account.deposit(500.0);
    account.withdraw(200.0);
    cout << "Balance: " << account.getBalance() << endl;
    return 0;
}
```

## 11. Summary

| Concept | Description |
|---------|-------------|
| Class | Blueprint defining properties and methods |
| Object | Instance of a class |
| Constructor | Initializes objects |
| Destructor | Cleans up resources |
| Encapsulation | Data hiding and interface simplification |
| Static Member | Belongs to class, shared by all objects |
| Friend | Allows external access to private members |
| this | Pointer to current object |

Classes and objects are the foundation of OOP. Mastering them is essential for writing modular and maintainable code.
