# C++ Code Comment Style Guide

## Introduction

This guide outlines the recommended style for writing code comments in C++. Consistent and clear code comments are crucial for enhancing code readability, maintainability, and collaboration within a project.

### General Guidelines

* Be concise and clear: Keep comments brief, focusing on important information and providing context where necessary.

* Use complete sentences: Begin comments with an uppercase letter and end with a period. This helps maintain consistency and readability.

* Use proper grammar and punctuation: Ensure that comments are well-formed and grammatically correct.

* Use punctuation appropriately to enhance clarity.

* Avoid unnecessary comments: Remove or update outdated or redundant comments to avoid confusion and maintain code integrity.

## Function and Method Comments

### Function description

Provide a brief description of the function's purpose, including any inputs, outputs, and side effects.

```cpp
/**
 * Calculates the sum of two integers.
 *
 * @param t_a The first integer.
 * @param t_b The second integer.
 * @return The sum of the two integers.
 */
int calculateSum(int t_a, int t_b);
```

### Parameter comments

Comment each parameter to describe its purpose, type, and any constraints or expectations.

```cpp
/**
 * Calculates the product of two numbers.
 *
 * @param t_x The first number.
 * @param t_y The second number.
 * @return The product of the two numbers.
 */
double calculateProduct(double t_x, double t_y);
```

### Return value comments

Explain the return value of a function or method, including any special conditions or error handling.

```cpp
/**
 * Finds the maximum element in the given array.
 *
 * @param t_arr The array of integers.
 * @param t_size The size of the array.
 * @return The maximum element in the array. Returns INT_MIN if the array is empty.
 */
    int findMax(const int* t_arr, size_t t_size);
```

## Class and Struct Comments

### Class/Struct description

 Provide a brief overview of the purpose and functionality of the class or struct.

```cpp
/**
 * Represents a person's contact information.
 */
struct ContactInfo {
    // ...
};
```

### Member variable comments

Comment each member variable to describe its purpose, type, and any constraints or relationships.

```cpp
struct Circle {
    double radius; // The radius of the circle.
    // ...
};
```

### Member function comments

Follow the same guidelines as function and method comments.

```cpp
struct Vector2D {
    /**
     * Calculates the magnitude of the vector.
     *
     * @return The magnitude of the vector.
     */
    double calculateMagnitude() const;
    // ...
};
```

## File Comments

### File overview

Provide a brief overview of the file's purpose, including its main components and usage.

```cpp
/**
 * @file math_utils.cpp
 * @brief Contains utility functions for mathematical operations.
 */
```

### Author and version information

Include the author name, creation date, and version information.

```cpp
/**
 * @file math_utils.cpp
 * @brief Contains utility functions for mathematical operations.
 *
 * @author John Doe
 * @date 01-06-2023
 * @version 1.0
 */
 ```

## In-line Comments

### Clarification comments

Use comments to clarify complex or non-obvious parts of the code.

```cpp
int result = calculateSum(a, b); // Calculate the sum of integers a and b.
```

### TODO and FIXME comments

Use TODO comments to highlight future improvements or additions and FIXME comments to mark problematic or incorrect code.

```cpp
// TODO: Implement error handling for edge case scenarios.
// FIXME: This code produces incorrect results for negative input values.
```

## Conclusion

Remember to adapt this style guide to your team's needs and conventions. Consistency is key when it comes to code comments, so make sure to follow the agreed-upon guidelines throughout the project.
