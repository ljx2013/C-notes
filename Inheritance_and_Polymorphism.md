
# Inheritance and Polymorphism in C++

## 1. Overview of Inheritance

Inheritance is a core feature of OOP that allows one class (derived class) to acquire properties and methods from another class (base class).

### 1.1 Benefits of Inheritance

- Code reuse
- Hierarchical organization
- Easy extension

## 2. Basic Inheritance Syntax

### 2.1 Defining Derived Class

```cpp
class BaseClass {
    // Base class members
};

class DerivedClass : access_specifier BaseClass {
    // Derived class members
};
```

### 2.2 Access Specifiers

| Inheritance | Base public | Base protected | Base private |
|-------------|-------------|----------------|--------------|
| public | public | protected | inaccessible |
| protected | protected | protected | inaccessible |
| private | private | private | inaccessible |

### 2.3 Example: Simple Inheritance

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
protected:
    string name;
    int age;

public:
    Person(string n, int a) : name(n), age(a) {}
    
    void introduce() {
        cout << "Name: " << name << ", Age: " << age << endl;
    }
};

class Student : public Person {
private:
    int studentId;

public:
    Student(string n, int a, int id) : Person(n, a), studentId(id) {}
    
    void study() {
        cout << name << " is studying" << endl;
    }
};
```

## 3. Constructor/Destructor Execution Order

### 3.1 Construction Order

1. Base class constructor
2. Member object constructors
3. Derived class constructor

### 3.2 Destruction Order (Reverse of Construction)

1. Derived class destructor
2. Member object destructors
3. Base class destructor

```cpp
class Base {
public:
    Base() { cout << "Base constructor\n"; }
    ~Base() { cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    Derived() { cout << "Derived constructor\n"; }
    ~Derived() { cout << "Derived destructor\n"; }
};

// Output:
// Base constructor
// Derived constructor
// Derived destructor
// Base destructor
```

## 4. Polymorphism

### 4.1 Concept of Polymorphism

Polymorphism allows objects of different classes to respond differently to the same message.

### 4.2 Virtual Functions

Virtual functions are key to implementing polymorphism.

```cpp
class Shape {
public:
    virtual double getArea() {
        return 0;
    }
};

class Circle : public Shape {
private:
    double radius;

public:
    Circle(double r) : radius(r) {}
    
    double getArea() override {
        return 3.14159 * radius * radius;
    }
};

class Rectangle : public Shape {
private:
    double width, height;

public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double getArea() override {
        return width * height;
    }
};
```

### 4.3 Pure Virtual Functions and Abstract Classes

```cpp
class Shape {
public:
    virtual double getArea() = 0;  // Pure virtual function
};
```

Classes with pure virtual functions are abstract and cannot be instantiated.

## 5. Virtual Destructors

### 5.1 Why Virtual Destructors

```cpp
class Base {
public:
    virtual ~Base() {  // Virtual destructor
        cout << "Base destructor\n";
    }
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Derived destructor\n";
    }
};

// Usage
Base* ptr = new Derived();
delete ptr;  // Correctly calls both destructors
```

## 6. Static vs Dynamic Polymorphism

### 6.1 Static Polymorphism (Compile-time)

Implemented through function overloading and templates.

```cpp
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }
```

### 6.2 Dynamic Polymorphism (Runtime)

Implemented through virtual functions.

```cpp
Shape* shapes[] = {new Circle(5), new Rectangle(4, 6)};
for (int i = 0; i < 2; i++) {
    cout << shapes[i]->getArea() << endl;  // Dynamic binding
}
```

## 7. Function Override vs Overload

### 7.1 Function Override

Redefine a virtual function in derived class.

```cpp
class Base {
public:
    virtual void show() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void show() override { cout << "Derived\n"; }
};
```

### 7.2 Function Overload

Define multiple functions with same name but different parameters.

```cpp
class MyClass {
public:
    void print(int x) { cout << x << endl; }
    void print(string s) { cout << s << endl; }
};
```

## 8. protected Members

### 8.1 Purpose of protected

Protected members are accessible to derived classes but not to the outside.

```cpp
class Base {
protected:
    int value;
};

class Derived : public Base {
public:
    void setValue(int v) {
        value = v;  // Can access protected member
    }
};
```

## 9. Multiple Inheritance

### 9.1 Syntax

```cpp
class A { /* ... */ };
class B { /* ... */ };
class C : public A, public B { /* ... */ };
```

### 9.2 Diamond Problem

```cpp
class A { public: int x; };
class B : public A { };
class C : public A { };
class D : public B, public C { };

D d;
// d.x;  // Ambiguous! B::x or C::x?
d.B::x = 1;
d.C::x = 2;
```

### 9.3 Virtual Inheritance

```cpp
class A { public: int x; };
class B : virtual public A { };
class C : virtual public A { };
class D : public B, public C { };

D d;
d.x = 1;  // Correct, only one x
```

## 10. Practical Examples

### Example 1: Shape Calculator

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual double getArea() = 0;
    virtual double getPerimeter() = 0;
};

class Circle : public Shape {
private:
    double radius;
public:
    Circle(double r) : radius(r) {}
    
    double getArea() override {
        return 3.14159 * radius * radius;
    }
    
    double getPerimeter() override {
        return 2 * 3.14159 * radius;
    }
};

class Rectangle : public Shape {
private:
    double width, height;
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double getArea() override {
        return width * height;
    }
    
    double getPerimeter() override {
        return 2 * (width + height);
    }
};

int main() {
    Shape* shapes[] = {new Circle(5), new Rectangle(4, 6)};
    
    for (auto shape : shapes) {
        cout << "Area: " << shape->getArea() << endl;
        cout << "Perimeter: " << shape->getPerimeter() << endl;
    }
    
    return 0;
}
```

### Example 2: Animal Simulator

```cpp
#include <iostream>
using namespace std;

class Animal {
protected:
    string name;
public:
    Animal(string n) : name(n) {}
    virtual void makeSound() = 0;
    virtual void move() = 0;
};

class Dog : public Animal {
public:
    Dog(string n) : Animal(n) {}
    
    void makeSound() override {
        cout << name << " says: Woof!\n";
    }
    
    void move() override {
        cout << name << " runs.\n";
    }
};

class Bird : public Animal {
public:
    Bird(string n) : Animal(n) {}
    
    void makeSound() override {
        cout << name << " says: Chirp!\n";
    }
    
    void move() override {
        cout << name << " flies.\n";
    }
};

int main() {
    Animal* animals[] = {new Dog("Buddy"), new Bird("Tweet")};
    
    for (auto animal : animals) {
        animal->makeSound();
        animal->move();
    }
    
    return 0;
}
```

## 11. Summary

| Concept | Description |
|---------|-------------|
| Inheritance | Derived class acquires base class properties |
| Access Specifiers | public, protected, private |
| Virtual Function | Enables dynamic polymorphism |
| Pure Virtual Function | Defines abstract interface |
| Virtual Destructor | Properly destroys derived objects |
| Multiple Inheritance | Class inherits from multiple bases |
| Virtual Inheritance | Solves diamond problem |

Inheritance and polymorphism are core to OOP. Mastering them enables flexible and extensible code design.
