# ALWAYS_ACTIVE: true
# Coding Standards Skill

This skill defines the mandatory coding standards for all code generation, refactoring, and file creation tasks. As an Antigravity agent, you must automatically apply these instructions to all code modifications across all workspaces.

## 1. Code Formatting & Whitespace
* **EditorConfig Check**: Before formatting code, check if an `.editorconfig` file exists in the repository root. If it does, strictly follow its rules for the matching file types
* **Fallback Formatting**: If no `.editorconfig` is present, apply these defaults:
  * **Indentation**: Use **2 spaces** per indentation level. Do not use tab characters
  * **Trailing Whitespace**: Do not leave trailing whitespace at the end of any line. Always trim lines
  * **End-of-File Newline**: Every file must end with exactly one trailing newline character

## 2. Google / PEP-257 Docstring Specification
All modules, functions, methods, classes, and types must be thoroughly documented. You must explicitly structure docstrings using the following Google/PEP-257 style format, regardless of the target programming language:

```text
"""One-line summary starting with capital letter and ending with a period.

A longer, detailed description detailing the behavior of the component,
its side-effects, and any design invariants.

Args:
    param1 (type): Description of param1.
    param2 (type): Description of param2.

Returns:
    return_type: Description of the return value.

Raises:
    ErrorType: Description of the circumstances that trigger this error.
"""
```

* **File/Module Headers**: Every file must start with a header docstring explaining the file's architectural role and contents
* **Definition Docstrings**: Every function, method, class, and custom type definition must use this format

## 3. Expression Grouping & Boolean Evaluations
* **Explicit Parentheses**: Group all sub-expressions explicitly in parentheses below the primary/top level of expression. Do not rely on implicit operator precedence
  * *Correct*: `x = y + (z * w)`
  * *Incorrect*: `x = y + z * w`
* **Explicit Boolean Comparisons**: Do not rely on implicit truthiness. Always check boolean conditions explicitly
  * *Correct*: `if x == True:` or `if y == False:`
  * *Incorrect*: `if x:` or `if not y:`
  * For non-boolean objects, explicitly evaluate their state (e.g., `if len(arr) > 0:` or `if obj is not None:` instead of implicit list/object truthiness checks)

## 4. Variables, Naming & Scope
* **Variable Casing**: Use `snake_case` (underscore separators) for variable, function, and method names unless the target language's style community dictates otherwise (e.g., camelCase in JavaScript/Go)
* **Import Localization**: Always import and use explicit symbols directly (e.g., `from math import sqrt` and use `sqrt(...)`) rather than long scope resolution identifiers (e.g., `import math` and `math.sqrt(...)`), unless doing so causes namespace clashes or ambiguity
* **Import Scope**: Keep imports as localized as possible. If an import is only used within a specific function or class, import it inside that function's/class's scope rather than at the top level

## 5. Type Safety & Defensive Checking
* **Type Annotations**: Use type hints/annotations for all function signatures where the language supports them
* **Defensive Error Handling**: Prefer representing validation and safety checks directly in the language's runtime logic using standard error handling (e.g., raising structured exceptions or returning error types/results). This ensures safety checks remain active in production

## 6. Control Flow & Return Checking
* **Check Return Values**: All functions that return a value *must* have their return values checked or handled by the caller. Do not discard return values
* **Structured Exit**: All functions must return control back to the caller (bubbling up to the program's main entry point) instead of terminating execution abruptly (e.g., do not call `sys.exit()` in nested utilities) unless using standard exception handling

## 7. Standard Directory Layout
When implementing testing and verification in a project, organize the workspace using the following structured directories:
* `/src/`: Reserved for all core algorithm implementation and source code
* `/test/`: Reserved for traditional unit tests, integration tests, and edge-case test suites
* `/verify/`: Reserved for formal methods, verification models (e.g., SMT/Z3 solvers), and constructive mathematical proofs (e.g., Lean) if they are applicable to the task

## 8. Commit Messages
When asked to author git commit messages, strictly follow Linux-style conventional commit messages using the 50/72 rule:
* **Subject Line**: Limit to 50 characters, capitalized, written in the imperative mood, starting with a conventional prefix (e.g., `feat:`, `fix:`, `docs:`), and never ending in a period or other punctuation
* **Separation**: Include a single blank line between the subject and the body
* **Body Casing**: Wrap all lines in the body at 72 characters max, explaining what changed and why (rather than how)
* **No Punctuation in Lists**: If the body contains bulleted or numbered lists, ensure that no list item ends with any punctuation (such as a period)

## 9. Source Code Comments
* **Line Length**: Ensure all comments (inline, block, or header) are wrapped at a maximum of 72 characters where applicable
* **No Punctuation in Lists**: If a comment block contains any listed items, never put ending punctuation (such as a period) at the end of list item lines
