# Defensive Programming Skill

This skill defines the rules for writing highly robust, self-validating, and up-armored code using defensive programming and dynamic assertions. Use these guidelines only when explicitly invoked.

## 1. Up-Armoring & Defensive Error Handling
* **Defensive Design**: Prefer representing safety checks and parameter validation directly in the language's core logic using standard error handling (e.g., throwing/raising structured exceptions, returning error objects/results). This ensures validation is always active in production
* **Fail Early**: Validate inputs immediately at the boundary of a function (prologue) to prevent corrupted state from propagating

## 2. Debug Assertions
* **Assertion Type**: When writing assertions, use **debug assertions**—assertions designed to be compiled away or disabled in production environments (e.g., `assert` in Python, standard C `assert` when `NDEBUG` is defined, or debug-gated assertions in Go/Rust)
* **When to Use Assertions**:
  1. **Functional Backup**: Use assertions as a secondary layer of validation to document and check complex or computationally expensive invariants that standard runtime error-checking might not perform or which is already implicitly assumed to be true
  2. **Internal Invariant Validation**: Use assertions to verify assumptions about internal code state that should mathematically never be violated if the code is correct (e.g., verifying loop invariants or algorithm-specific branch expectations)
  3. **Development Checks**: Catch programming errors (such as type mismatching in dynamic languages) during development and testing before deployment

## 3. Pre & Post-Conditions
* **Pre-Conditions (Prologue)**: Use debug assertions at the start of a function to enforce constraints on parameters and incoming state that cannot easily be checked via standard compile-time type systems
* **Post-Conditions (Epilogue)**: Use debug assertions at the end of a function (immediately before returning) to guarantee that the output values and resulting state changes strictly conform to the function's contract

## 4. Test Coverage & Frameworks
* **Coverage Requirement**: Ensure that all newly introduced or modified code has comprehensive test coverage at least at the method/function level
* **Standard Testing Frameworks**: Write tests using the standard, community-accepted testing framework for the target programming language. Do not pull in arbitrary third-party testing dependencies unless explicitly asked:
  * **Python**: Use the built-in `unittest` module
  * **C/C++**: Use simple assert-driven test scripts or the language-standard testing framework
  * **JavaScript/TypeScript**: Use built-in or standard runtime-native test runners
