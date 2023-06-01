# C++ Code Style Guide

## Introduction

This guide provides a comprehensive set of coding conventions and best practices for writing modern C++ code. It covers naming conventions, formatting guidelines, code organization, and other important aspects of code style. The examples below illustrate the recommended practices.

## Table of Contents

- [Naming Conventions](#naming-conventions)
- [Formatting Guidelines](#formatting-guidelines)
- [Code Organization](#code-organization)
- [Comments](#comments)
- [Error Handling](#error-handling)
- [Memory Management](#memory-management)
- [Performance Considerations](#performance-considerations)
- [Safety Considerations](#safety-considerations)
- [Correctness Considerations](#correctness-considerations)
- [Design Considerations](#design-considerations)
- [Tool Usage](#tool-usage)

## Naming Conventions

### General Guidelines

- Use descriptive and meaningful names for variables, functions, classes, and other entities.
- Follow a consistent naming style, such as camelCase for variables and functions, and PascalCase for classes and namespaces.
- Avoid using single-letter names, except for loop variables or other temporary situations.
- Don't name anything starting or ending with `_`. If you do, you risk colliding with names reserved for compiler and standard library implementation

```cpp
int calculateSum(int num1, int num2) {
    // Function body
}

class Car {
    // Class members and functions
};

std::string firstName;
```

### Constants

- Use uppercase letters and underscores for compile time constants.
- Runtime constants can follow the general naming style
- Prefer constexpr over #define for defining constants.

```cpp
const std::string logMsg = "[" + severityLevel + "] " + message;

constexpr PI = 3.14159;
```

### Namespaces

- Use `PascalCase` for namespace names.
- Use short, descriptive names for namespaces.

```cpp
namespace Math {
    // Namespace contents
}

namespace IO {
    // Namespace contents
}
```

### Classes

- Use `PascalCase` for class names.
- Use a noun or noun phrase that describes the class's purpose.

```cpp
class Circle {
    // Class members and functions
};

class Employee {
    // Class members and functions
};
```

### Functions and Methods

- Use camelCase for function and method names.
- Use a verb or verb phrase that describes the action performed by the function or method.

```cpp
void calculateSum(int num1, int num2) {
    // Function body
}

void printMessage(std::string message) {
    // Function body
}
```

### Variables

- Use camelCase for variable names.
- Use descriptive names that convey the purpose or meaning of the variable.
- Name private data with a `m_` prefix to distinguish it from public data. `m_` stands for `"member"` data.

```cpp
int numAttempts;
std::string firstName;

class MyClass
{
public:
    MyClass();
private:
    int m_width;
    int m_height;
    bool m_isReady;
}
```

### Enums

- Use PascalCase for enum names.
- Use uppercase letters and underscores for enum constants.

```cpp
enum EColor {
    RED,
    GREEN,
    BLUE,
    ROSE_GOLD
};
```

## Formatting Guidelines

### Indentation and Braces

- Use 4 spaces for indentation (no tabs).
- Use curly braces for all control structures, even if the body consists of a single statement.
- Place the opening brace on the same line as the control structure declaration.
- Align the closing brace with the declaration that introduced the block.

```cpp
if (condition) {
    // Code block
} else {
    // Code block
}

for (int i = 0; i < count; ++i) {
    // Code block
}
```

### Line Length

- Limit the maximum line length to 80 characters.
- Break long lines by using line continuation with proper indentation.

```cpp
std::string longString = "This is a very long string that exceeds the maximum line length "
                         "and needs to be split into multiple lines for readability.";
```

### Whitespace

- Use a single space between keywords, operators, and operands.
- Use a single space after commas and semicolons.

```cpp
int result = calculateSum(num1, num2);

for (int i = 0; i < count; ++i) {
    // Code block
}
```

### Function Declarations and Definitions

- Place a space between the function name and the opening parenthesis.
- Place the opening brace on the next line after the function declaration.
- Use default arguments in the function declaration, not in the definition.
- Name function parameters with an `t_` prefix to distinguish function parameters from other variables `t_` can be thought of as `"the"`, but the meaning is arbitrary. The point is to distinguish function parameters from other variables within a given scope while giving us a consistent naming strategy.

```cpp
void printMessage(std::string t_message);

void printMessage(std::string t_message = "Hello, world!") {
    std::string message = t_message;
    // ...
}
```

## Code Organization

### Include Statements

- Group include statements into logical blocks.
- Sort the include statements alphabetically within each block.
- Use relative paths for local project headers and standard library headers.
- Use `""` for local project headers and `<>` for standard and third party libraries

```cpp
#include "myproject/myheader.h"
#include <iostream>
#include <vector>
```

### Class Structure

- Order the class members in a logical and consistent manner.
- Place public members first, followed by protected and private members.
- Separate each section with a comment.

```cpp
class MyClass {
public:
    // Public members and functions

protected:
    // Protected members and functions

private:
    // Private members and functions
};
```

### Function Order

- Order functions within a class or namespace in a logical manner.
- Place function definitions in the same order as their corresponding declarations.

```cpp
void initialize();
void process();
void cleanup();

void MyClass::initialize() {
    // Function body
}

void MyClass::process() {
    // Function body
}

void MyClass::cleanup() {
    // Function body
}
```

## Comments

### General Comments

- Use comments to explain the code's intent, logic, or reasoning behind certain decisions.
- Write clear and concise comments that add value to the code.
    Keep comments up to date with the code changes.

#### [More Detail on Code Comments](Code_Comments.md)

## Error Handling

### Exceptions

- Use exceptions to handle exceptional or error conditions.
- Define custom exception classes to provide meaningful error messages.
- Catch exceptions at an appropriate level and handle or propagate them accordingly.

```cpp
class MyException : public std::exception {
public:
    explicit MyException(const std::string& message) : message_(message) {}

    const char* what() const noexcept override {
        return message_.c_str();
    }

private:
    std::string message_;
};

void someFunction() {
    try {
        // Code that may throw an exception
    } catch (const MyException& ex) {
        // Handle the exception
    }
}
```

### Error Codes

- Use error codes when exceptions are not appropriate or too costly.
- Define meaningful error codes as enum constants.
    Return error codes from functions and handle them accordingly.

```cpp
enum ErrorCode {
    OK = 0,
    FILE_NOT_FOUND,
    ACCESS_DENIED
};

ErrorCode openFile(const std::string& t_filename) {
    if (!fileExists(t_filename)) {
        return FILE_NOT_FOUND;
    }

    if (!hasPermission(t_filename)) {
        return ACCESS_DENIED;
    }

    // File open success
    return OK;
}
```

## Memory Management

### Resource Acquisition Is Initialization (RAII)

- Use RAII principles to manage resources and avoid manual memory management.
- Allocate resources in constructors and release them in destructors.

```cpp
class Resource {
public:
    Resource() {
        std::cout << "Resource acquired." << std::endl;
    }

    ~Resource() {
        std::cout << "Resource released." << std::endl;
    }

    void DoSomething() {
        std::cout << "Doing something with the resource." << std::endl;
    }
};

int main() {
    {
        Resource r; // Resource acquired via instantiation.
        r.DoSomething(); // Doing something with the resource.
    } // Resource released.

    std::cout << "End of program." << std::endl;
    return 0;
}
```

### Smart Pointers

- Prefer using smart pointers, such as std::unique_ptr and std::shared_ptr, over raw pointers.
- Use std::unique_ptr for exclusive ownership and std::shared_ptr for shared ownership.

```cpp
std::unique_ptr<MyClass> myObject = std::make_unique<MyClass>();

std::shared_ptr<MyClass> sharedObject = std::make_shared<MyClass>();
```

## Performance Considerations

### Use const and constexpr

- Use const for variables that should not be modified after initialization.
- Use constexpr for values that can be computed at compile-time.

```cpp
const std::string logMsg = "[" + severityLevel + "] " + error;
constexpr double PI = 3.14159;
```

### Avoid Unnecessary Copies

- Use references or const references when passing large objects as function parameters.
- Use move semantics (std::move()) to transfer ownership instead of making unnecessary copies.

```cpp
// Function that takes a large object by reference
void processObject(const std::string& obj) {
    // Function body
}

// Function that takes a large object by move
void transferOwnership(std::string&& obj) {
    // Function body
}

int main() {
    // Example 1: Using const reference to avoid unnecessary copies
    std::string greeting = "Hello, world!";
    printMessage(greeting);  // Passing by const reference
    // The original string is not modified, and no copies are made

    // Example 2: Using move semantics to transfer ownership
    std::string data = "Some large data...";
    processMessage(std::move(data));  // Transfer ownership using std::move()
    // The ownership of 'data' is transferred to the function, avoiding unnecessary copies
    // 'data' is left in a valid but unspecified state after the move

    return 0;
}
```

### Avoid Expensive Operations in Loops

- Move expensive operations outside of loops when possible.
- Cache loop conditions and variables to minimize redundant calculations.

```cpp
const int size = expensiveComputation();
for (int i = 0; i < size; ++i) {
    // Loop body
}
```

## Safety Considerations

### Memory Safety

- Avoid buffer overflows, null pointer dereferences, and memory leaks.
- Avoid Raw Memory Access. Use smart pointers, containers, and RAII to manage and access memory.
- Const as Much as Possible. `const` tells the compiler that a variable or method is immutable. This helps the compiler optimize the code and helps the developer know if a function has a side effect. Also, using const& prevents the compiler from copying data unnecessarily.
- Carefully Consider Your Return Types.
  - Returning by `&` or `const &` can have significant performance savings when the normal use of the returned value is for observation
  - Returning by value is better for thread safety and if the normal use of the returned value is to make a copy anyhow, there's no performance lost
        - If your API uses covariant return types, you must return by `&` or `*`
  - Temporaries and local values should always return by value.

- Perform boundary checks and validate user input to prevent access violations.

```cpp
std::vector<int> numbers = {1, 2, 3, 4, 5};
if (index >= 0 && index < numbers.size()) {
    int value = numbers[index];
    // Perform operations with value
}
```

### Thread Safety

- Synchronize access to shared data and resources in multi-threaded environments.
- Use mutexes, condition variables, and atomic operations to enforce thread safety.
- Avoid data races and race conditions by properly managing concurrent access.

```cpp
std::mutex mutex;
std::vector<int> sharedData;

// Thread 1
{
    std::lock_guard<std::mutex> lock(mutex);
    sharedData.push_back(42);
}

// Thread 2
{
    std::lock_guard<std::mutex> lock(mutex);
    int value = sharedData.back();
    // Perform operations with value
}
```

## Correctness Considerations

### Input Validation

- Use assertions and pre/post-conditions to enforce design contracts.
- Validate assumptions and inputs to ensure correct behavior.
- Fail fast by terminating the program if contract violations occur.

```cpp
#include <cassert>

int divide(int dividend, int divisor) {
    assert(divisor != 0 && "Divisor must be non-zero");
    return dividend / divisor;
}
```

### Unit Testing

- Write comprehensive unit tests to verify the correctness of your code.
- Test boundary conditions, edge cases, and failure scenarios.
- Automate tests and run them frequently to catch regressions.

```cpp
#include <gtest/gtest.h>

TEST(MyClassTest, SumTest) {
    EXPECT_EQ(calculateSum(2, 2), 4);
    EXPECT_EQ(calculateSum(0, 0), 0);
    // Additional test cases
}

int main(int argc, char** argv) {
    testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

## Design Considerations

### KISS (Keep It Simple, Stupid)

KISS is a principle that encourages simplicity in software design and development. The idea is to keep things as simple as possible, avoiding unnecessary complexity. Here's a C++ code example that follows the KISS principle

```cpp
// Bad example
int multiply(int a, int b) {
  int result = 0;
  for (int i = 0; i < b; i++) {
    result += a;
  }
  return result;
}

// Good example
int multiply(int a, int b) {
  return a * b;
}
```

### DRY (Don't Repeat Yourself)

DRY is a principle that promotes code reuse and reducing duplication. It encourages developers to avoid writing duplicate code by abstracting common functionality into reusable components

```cpp
// BAD implementation
void printName() {
  std::cout << "John Doe" << std::endl;
}

void printAddress() {
  std::cout << "123 Main Street" << std::endl;
}

// Good
void printInfo(const std::string& info) {
  std::cout << info << std::endl;
}

// Usage
printInfo("John Doe");
printInfo("123 Main Street");
```

### SOLID (Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion)

- ***Single Responsibility:*** A class should have only one reason to change. It means that a class should have only one responsibility.

```cpp
// Example: A class that handles file operations
class FileManager {
public:
    void readFile(const std::string& fileName);
    void writeFile(const std::string& fileName);
    void deleteFile(const std::string& fileName);
};
```

- ***Open/Closed:*** Software entities (classes, modules, functions) should be open for extension but closed for modification. This means that the behavior of a module can be extended without modifying its source code.

```cpp
// Example: A class that calculates the area of different shapes
class Shape {
public:
    virtual double calculateArea() const = 0;
};

class Rectangle : public Shape {
public:
    double calculateArea() const override {
        // Calculation logic for a rectangle
    }
};

class Circle : public Shape {
public:
    double calculateArea() const override {
        // Calculation logic for a circle
    }
};
```

- ***Liskov Substitution:*** Subtypes must be substitutable for their base types without affecting the correctness of the program.

```cpp
// Example: A function that takes a Shape parameter and prints its area
void printArea(const Shape& shape) {
    double area = shape.calculateArea();
    std::cout << "Area: " << area << std::endl;
}

// Usage with different subtypes of Shape
Rectangle rectangle;
Circle circle;

printArea(rectangle); // Prints area of the rectangle
printArea(circle);    // Prints area of the circle
```

- ***Interface Segregation:*** Clients should not be forced to depend on interfaces they do not use. Instead of one large interface, smaller and more specific interfaces should be preferred.

```cpp
// Example: A class that performs different operations on a database
class DatabaseHandler {
public:
    virtual void connect() = 0;
    virtual void insert() = 0;
    virtual void update() = 0;
    virtual void remove() = 0;
};

// Better approach using smaller interfaces
class Connectable {
public:
    virtual void connect() = 0;
};

class Insertable {
public:
    virtual void insert() = 0;
};

class Updatable {
public:
    virtual void update() = 0;
};

class Removable {
public:
    virtual void remove() = 0;
};

// Implementing specific interfaces where needed
class Database : public Connectable, public Insertable, public Updatable, public Removable {
    // Implement interface methods
};
```

- ***Dependency Inversion:*** High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

```cpp
// Example: A high-level class using a low-level class directly
class HighLevelClass {
private:
    LowLevelClass lowLevelObj;

public:
    void performOperation() {
        // Use the low-level object directly
        lowLevelObj.operation();
    }
};

// Better approach using abstractions and dependency injection
class AbstractClass {
public:
    virtual void operation() = 0;
};

class LowLevelClass : public AbstractClass {
public:
    void operation() override {
        // Implementation details
    }
};

class HighLevelClass {
private:
    AbstractClass& abstractObj;

public:
    HighLevelClass(AbstractClass& obj) : abstractObj(obj) {}

    void performOperation() {
        // Use the abstraction through dependency injection
        abstractObj.operation();
    }
};
```

### Composition over Inherritance

Composition over inheritance is a principle in programming that suggests using object composition instead of class inheritance. It promotes creating objects by combining instances of different classes rather than relying on a hierarchical inheritance structure. This provides benefits such as flexibility, loose coupling, selective code reuse, and clearer interfaces. By favoring composition, you can achieve a more modular and adaptable codebase that is easier to understand and maintain.

```cpp
// BAD
class Engine {
public:
    void start() {
        // Code to start the engine
    }
};

class Car : public Engine {
public:
    void startCar() {
        start();  // Inherits the start() method from the Engine class
        // Code to start the car
    }
};

// GOOD
class Engine {
public:
    void start() {
        // Code to start the engine
    }
};

class Car {
private:
    Engine engine;

public:
    void startCar() {
        engine.start();
        // Code to start the car
    }
};
```

### YAGNI (You Ain't Gonna Need It)

YAGNI is a coding principle that suggests developers should not add functionality until it is necessary. It promotes simplicity and avoids over-engineering. The idea behind YAGNI is to focus on the current requirements and only implement the features that are needed immediately, rather than speculating about potential future requirements. By adhering to the YAGNI principle, developers can avoid wasting time and effort on unnecessary features and keep the codebase clean and manageable. It allows for flexibility and adaptability to future changes, as the code remains lean and focused on the immediate requirements.

```cpp
// Unnecessary Functionality
class Rectangle {
private:
    int length;
    int width;
    int area;  // Unnecessary field

public:
    Rectangle(int l, int w) : length(l), width(w), area(l * w) {}

    int getArea() { return area; }  // Unnecessary getter

    void doubleArea() { area *= 2; }  // Unnecessary method
};
```

```cpp
// Without YAGNI
class Calculator {
public:
    int add(int a, int b) {
        // Complex addition logic
        return a + b;
    }

    int multiply(int a, int b) {
        // Complex multiplication logic
        return a * b;
    }

    int divide(int a, int b) {
        // Complex division logic
        return a / b;
    }

    // Many more unnecessary methods...
};

// With YAGNI
class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }
};
```

## Tool Usage

### IDE Extensions (e.g., Visual Assist, ReSharper++, etc.)

- Utilize IDE extensions to enhance productivity and code quality.
- Configure code completion, refactoring tools, and code analysis plugins.
- Take advantage of features like code templates and code generation.
- I recommend Visual Assist, ReSharper++ and Copilot for Visual Studio

### Source Control

- Use a version control system (e.g., Git) to manage source code.
- Commit changes regularly and write meaningful commit messages.
- Collaborate with team members through branching and merging.

### Compiler Flags

- Enable appropriate compiler flags to catch potential issues and optimize code.
- Enable warnings and treat them as errors
- Consider using the treat warnings as errors setting. `/WX` with MSVC, `-Werror` with GCC / Clang

### Static Analyzers

- Utilize static analyzers (e.g., Clang-Tidy, PVS-Studio) to catch code issues.
- Configure analyzers to match your project's coding standards.
- Address and fix reported issues promptly.

### Debugging and Profiling

- Use breakpoints, watch variables, and step-through debugging for bug investigation.
- Profile code performance to identify bottlenecks and optimize critical sections.
- Leverage tools like GDB, Visual Studio Debugger, and performance profilers.

```cpp
void someFunction() {
    int value = 42;
    // Set breakpoint on the next line to view the value and debug the code
    std::cout << "Value: " << value << std::endl;
    // Rest of the function
}
```

## Conclusion

This guide covers the essential aspects of coding style in C++. Adhering to these guidelines will enhance code readability, maintainability, and collaboration within a development team. Remember to prioritize consistency and choose meaningful and descriptive names for your code elements.
