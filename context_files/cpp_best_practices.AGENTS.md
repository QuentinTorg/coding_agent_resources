# Modern C++ Best Practices Context

*This is an `AGENTS.md` snippet. Copy the contents of this file into your workspace `AGENTS.md` (or `GEMINI.md`) file to establish a strict, modern baseline for a C++ project. **Note for users:** The rules below represent modern industry consensus for C++17/20, but please feel free to modify these rules to reflect your specific project's standards.*

---

## ⚙️ Modern C++ Architecture & Rules

You are acting as a Senior Systems Engineer. When writing or modifying C++ code in this repository, you MUST strictly adhere to the following modern C++ (C++17/20) best practices:

### 1. Memory Management & Pointers
- **No Manual Memory Management:** The use of raw `new` and `delete` is strictly forbidden. 
- **Smart Pointers:** Exclusively use `std::unique_ptr` for exclusive ownership and `std::shared_ptr` for shared ownership. Prefer `std::make_unique` and `std::make_shared` for instantiation.
- **Raw Pointers:** Raw pointers (`T*`) may *only* be used as non-owning observers (e.g., passing a reference to a function that does not take ownership). Never delete a raw pointer.

### 2. Standard Library & Modern Features
- **Prefer `<algorithm>`:** Do not write raw `for` loops for common operations (like finding, sorting, or accumulating). Always prefer the appropriate function from `<algorithm>` (e.g., `std::find_if`, `std::transform`).
- **`auto` Keyword:** Use `auto` when the type is obvious from the right-hand side of the assignment (e.g., `auto ptr = std::make_unique<MyClass>();`) or when dealing with complex iterator types. Do not use `auto` if it obscures the logic.
- **`constexpr`:** Use `constexpr` for values that are known at compile time to optimize performance.

### 3. Class Design & State
- **Rule of Zero / Five:** If a class does not manage resources, do not declare a destructor, copy/move constructors, or copy/move assignment operators (Rule of Zero). If it does manage a custom resource, you must explicitly define or `= delete` all five.
- **Const Correctness:** Apply `const` wherever possible. Member functions that do not modify class state MUST be marked `const`. Pass large objects by `const Type&` (const reference) rather than by value.
- **Initialization:** Always initialize member variables in the class definition or using member initializer lists in the constructor. Never leave basic types uninitialized.

### 4. Headers & Compilation
- **Header Guards:** Always use `#pragma once` at the top of header files.
- **Includes:** Keep `#include` directives to an absolute minimum in header files. Use forward declarations (`class MyClass;`) in headers whenever possible to reduce compile times, and include the full header in the `.cpp` file.
- **Namespaces:** Avoid `using namespace std;` (or any other `using namespace` directive) in global scope, especially in header files, to prevent namespace pollution.

### 5. Error Handling
- **Exceptions:** Use exceptions for exceptional, unrecoverable errors. Do not use exceptions for expected control flow.
- **`std::optional` / `std::expected`:** For functions that can fail cleanly, return a `std::optional<T>` (C++17) or `std::expected<T, E>` (C++23) rather than returning null pointers or magic error codes (like `-1`).
