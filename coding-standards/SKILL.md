---
name: coding-standards
description: Mandatory coding standards for all code generation, refactoring, and file creation tasks. Automatically apply these instructions to all code modifications across all workspaces.
triggers:
  - auto
context:
  - file_change
  - code_generation
  - refactor
---

# Coding Standards

Mandatory coding standards for all code generation, refactoring, and file creation tasks. Automatically apply these instructions to all code modifications across all workspaces.

## 1. Code Formatting & Whitespace

- **EditorConfig Check**: Before formatting code, check if an `.editorconfig` file exists in the repository root. If it does, strictly follow its rules for the matching file types
- **Fallback Formatting**: If no `.editorconfig` is present, apply these defaults:
  - **Indentation**: Use **2 spaces** per indentation level. Do not use tab characters
  - **Trailing Whitespace**: Do not leave trailing whitespace at the end of any line. Always trim lines
  - **End-of-File Newline**: Every file must end with exactly one trailing newline character
  - **Line Wrapping**: Keep all instructions, statements, and expressions wrapped to a limit of **72 characters** (Note: this line wrapping limit does not apply to markdown files or regular text files, except for comments and code blocks within them)
  - **Wrapping Style**: Prefer wrapping across lines using natural, clean syntactic structures (such as parentheses in Python) rather than line continuation indicators like backslashes
  - **Operator Wrapping**: When a comparison, assignment, or expression operator follows a closing delimiter (like a parenthesis `)`) of a multi-line expression, do not wrap the operator to a new line. Keep the operator on the same line as the closing delimiter (e.g. `) == True` or `) == If(...)`) if the resulting line fits within length limits
  - **Single-Line Nesting**: Do not wrap nested function calls or
    expressions onto multiple lines if the entire collapsed expression
    fits on a single line within the 72-character limit
  - **Blank Line Limit**: Limit spacing between code blocks (such as definitions, declarations, or functions) to at most one blank line regardless of syntactic nesting level, and do not insert blank lines between individual statements within a block to avoid spurious whitespace


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

- **File/Module Headers**: Every file must start with a header docstring explaining the file's architectural role and contents
- **Header Spacing**: Include exactly one blank line immediately after the file's header/module docstring (before any imports or code declarations)
- **Definition Docstrings**: Every function, method, class, and custom type definition must use this format
- **Test Docstring Exception**: For test functions that accept no parameters, return nothing, and raise no exceptions, the `Args:`, `Returns:`, and `Raises:` subsections of the docstring can be omitted
- **Language-Specific Syntax**: Use standard language-specific comment delimiters while keeping the same structured Google/PEP-257 sections
- **Test Docstrings**: Every test function (including inline or functions and external integration test suites) must still include a docstring explaining the purpose, scenario, or expectation of the test
- **Traits and Interfaces**: Every trait definition, interface definition, and all of their internal method definitions must be thoroughly documented using doc comments
- **Trait Implementations**: Every trait implementation block must have a doc comment explaining the concrete implementation details, and its individual methods must also have doc comments if they exhibit any implementation-specific behavior or constraints
- **No Redundant Field or Variant Comments**: Avoid writing doc comments for struct fields or enum variants if the comment simply repeats the name and type information without providing any additional semantic context, design invariants, or non-obvious constraints

## 3. Expression Grouping & Explicit Programming

- **Explicit Parentheses**: Group all sub-expressions explicitly in parentheses, but do not parenthesize the top-level expression of a statement (such as a variable assignment, if condition, or assert condition) unless required for multi-line wrapping. Only the nested sub-expressions with operators need explicit parenthesizing, which reduces visual clutter from unnecessary punctuation
  - *Correct*: `x = y + (z * w)`
  - *Incorrect*: `x = (y + (z * w))` or `if (x == y):` (when it fits on one line)
- **Explicit Programming**: In general, prefer explicit code over implicit behavior. Do not rely on implicit magical behavior, auto-casting, or hidden side effects unless it is a strongly enforced idiom of the specific programming language. Code should always clearly state its intentions
- **Explicit Truthiness Evaluations**: Do not rely on implicit truthiness for any types (booleans, integers, lists, strings, objects). Always check truthiness conditions explicitly by comparing against a definitive state (e.g., length, nullability, or strict boolean values), unless working in specific languages that strictly reject this practice
  - **Language Exceptions**:
    - **Rust**: Clippy rejects comparisons with literal booleans (`== true`, `== false`) via the `clippy::bool_comparison` and `clippy::bool_assert_comparison` lints. When writing Rust, you must use implicit truthiness for booleans (e.g., `if x`, `if !y`, `assert!(x)`) instead of explicit checks (e.g., `if x == true` or `assert_eq!(x, true)`) across all contexts. Note that this exception applies only to booleans; for other types in Rust, you must still explicitly check state (e.g., `if !arr.is_empty()`)
  - **Standard Rule (for all other languages)**:
    - *Booleans*: Explicitly compare to True/False (e.g., `if x == True:` or `if y == False:` instead of `if x:` or `if not y:`)
    - *Collections*: Explicitly check length or emptiness (e.g., `if len(arr) > 0:` instead of `if arr:`)
    - *Objects/Nullables*: Explicitly check against null/None (e.g., `if obj is not None:` instead of `if obj:`)
    - *Integers*: Explicitly check against zero (e.g., `if count > 0:` instead of `if count:`)

## 4. Variables, Naming & Scope

- **Variable Casing**: Use `snake_case` (underscore separators) for variable, function, and method names unless the target language's style community dictates otherwise (e.g., camelCase in JavaScript/Go)
- **Import Localization**: Always import and use explicit symbols directly (e.g., `from math import sqrt` and use `sqrt(...)`) rather than long scope resolution identifiers (e.g., `import math` and `math.sqrt(...)`), unless doing so causes namespace clashes or ambiguity
- **Import Prefix Restrictions**: Avoid inline path prefixes (e.g. `crate::`, `super::`, or crate-root names) within signatures or expressions, and instead bring modules or types into scope using import declarations
- **One-Level Qualification**: For ambiguous names, qualify using exactly one level of namespace (e.g. `ast::NetValue` vs `unambiguous::Value`). For unambiguous names, import them directly and use them unqualified
- **Import Scope**: Keep imports as localized as possible. If an import is only used within a specific function or class, import it inside that function's/class's scope rather than at the top level
  - **Python override**: All imports must be placed at the top of the file (per PEP 8). Do **not** use function- or class-level imports in Python, even for imports used in a single scope
- **Import Spacing**: Include exactly one blank line immediately after the top-level imports block (before any function, class, or other code declarations)
- **No Absolute System Paths**: Never hardcode or write absolute system
  paths (such as `/Users/...`) in source code files, configurations, or
  repository documentation. Always use relative paths or retrieve paths
  dynamically from the environment

## 5. Type Safety & Defensive Checking

- **Type Annotations**: Use type hints/annotations for all function signatures where the language supports them
- **Defensive Error Handling**: Prefer representing validation and safety checks directly in the language's runtime logic using standard error handling (e.g., raising structured exceptions or returning error types/results). This ensures safety checks remain active in production
- **Warning and Error Suppression**: Never automatically insert suppression of warnings or errors (e.g., `# type: ignore`, `pylint: disable`, `@SuppressWarnings`) for any target language unless explicitly directed to do so

## 6. Control Flow & Return Checking

- **Check Return Values**: All functions that return a value *must* have their return values checked or handled by the caller. Do not discard return values
- **Structured Exit**: All functions must return control back to the caller (bubbling up to the program's main entry point) instead of terminating execution abruptly (e.g., do not call `sys.exit()` in nested utilities) unless using standard exception handling
- **Pattern Matching Wildcards**: Do not use wildcard catch-all patterns (such as `_ => ...` or a bare `_` arm) when matching enums. You must explicitly match and expand all enum variants in match blocks to rely on the compiler for exhaustiveness checks. Wildcards in enum match arms are considered an anti-pattern. Note that this rule applies exclusively to match arms on enums; it does not forbid using wildcard during destructuring

## 7. Standard Directory Layout

When implementing testing and verification in a project, organize the workspace using the following structured directories:
- `/src/`: Reserved for all core algorithm implementation and source code
- `/test/`: Reserved for traditional unit tests, integration tests, and edge-case test suites
- `/verify/`: Reserved for formal methods, verification models (e.g., SMT/Z3 solvers), and constructive mathematical proofs (e.g., Lean) if they are applicable to the task

## 8. Commit Messages

When asked to author git commit messages, strictly follow Linux-style conventional commit messages using the 50/72 rule:
- **Subject Line**: Limit to 50 characters, written in the imperative mood, starting with a conventional prefix (e.g., `feat:`, `fix:`, `docs:`), and never ending in a period or other punctuation
- **Slug Casing**: The slug — the short description that follows the type tag colon (e.g., the `add login endpoint` part of `feat: add login endpoint`) — must **always be entirely lowercase**. Never capitalize the first word or any word in the slug. This applies without exception, even at the start of the subject line
- **Separation**: Include a single blank line between the subject and the body
- **Body Casing**: Wrap all lines in the body at 72 characters max, explaining what changed and why (rather than how)
- **No Punctuation in Lists**: If the body contains bulleted or numbered lists, ensure that no list item ends with any punctuation (such as a period)
- **Impersonal Voice**: Do not refer to the author, the entity writing the document, or the self (e.g., avoid using words like `we`, `I`, `us`, `our`, `my`). Describe only the changes made, the current behavior, or the intended outcome using an objective, impersonal tone

## 9. Source Code Comments & Markdown Formatting

- **Line Length**: Ensure all comments (inline, block, or header) are wrapped at a maximum of 72 characters where applicable (Note: this line wrapping limit does not apply to markdown files or regular text files, except for comments and code blocks within them)
- **No Punctuation in Lists**: If a comment block or markdown file contains any listed items, never put ending punctuation (such as a period) at the end of list item lines
- **List Bullet Style**: Always use `-` for bulleted list items (both in source code comments and markdown files). Never use `*`
- **Markdown Headings**: Always include exactly one blank line immediately after any markdown heading declaration (e.g., `# Heading`), regardless of the level of that heading
- **Code and Non-English Enclosure**: In all comments, markdown files, and git commit messages, always enclose any term that is not natural English (such as variable/function/class/type names, parameter names, file paths, directories, URLs, symbols, or data values) in backticks
- **Docstring Types and Parameters**: Inside all function, method, and trait docstrings, always wrap type annotations, parameter names, and return values/types in backticks
- **No Ending Punctuation**: The last sentence of a paragraph in all comments in source files (including inline comments, block comments, and docstrings) must never end with punctuation (such as a period)

## 10. Planning and Design Modes

- Never make any changes to a repository, workspace, artifacts, plans, or designs after execution on an implementation plan unless explicitly ordered.
- If the user asks questions or has a discussion, never making changes without asking first.
- Have a discussion in the chat or ask to create a new plan, making sure not to overwrite old artifacts. This is to protect the changes just made so the user can perform a review.
- Adhere to a strict set of modes with a read and write permission flow: `planning and design [read only] -> implementation [read/write] -> user review [read only]`. This cycle repeats.
- During user review mode, strictly adhere to the read only mode and just discuss potential changes, answer questions, and discuss ideas, never accidentally changing and ruining the review work of the user.

## 11. Test Honesty

- **No Shim Traits in Tests**: Never introduce test-only traits, wrapper types,
  or extension methods that reimplement, alias, or compose over production API
  methods to make old test assertions compile. If a public API is removed or
  renamed, the tests must be rewritten to call the new API directly. Shim
  traits mask API surface changes and create a false sense that the API is
  being exercised
- **No Test-Local Aliases**: Do not create local `type` aliases, `use`
  renames, or `let` rebindings purely to hide a renamed or restructured API
  from the test assertions. Tests must use the actual exported names

## 12. Post-Change Comment Integrity Review

After any code change, perform a deliberate sweep of all affected and
related source files to identify and correct comments rendered stale or
factually incorrect by the change. This review is mandatory and cannot
be skipped.

### 12.1 Temporal Ordering Constraint

This review is **strictly post-hoc**. The ground truth for whether a
comment is stale is established only once the code is in its final
state. The correct sequence is:

1. Plan the change
2. Execute all code edits
3. **Only then** perform the comment integrity sweep

Performing this check earlier yields false results because the code it
checks against is still in flux

### 12.2 Spatial Awareness Constraint

A stale comment is **not necessarily near the code that invalidated
it**. Do not limit the sweep to lines you touched. Actively search all
of the following locations:

- **File-level module/header docstrings**: Re-evaluate the header
  docstring of every file you modified
  - Check whether it still accurately describes the file's role,
    exported API surface, or behavioral contract
- **Calling sites and consumers**: If you changed a function or type's
  signature or semantics, inspect every caller
  - Callers may live in different files, modules, or packages
  - Their inline comments and docstring parameter descriptions may
    still describe the old behavior
- **Docstrings of dependents**: Walk one level up the call graph and
  inspect every direct consumer's documentation
  - A function documented as delegating to something you changed may
    now be a misdescription
- **Cross-file reference comments**: Search for comments that name
  entities you renamed, moved, or removed
  - Examples: `// See FooHandler for details` or
    `# mirrors parse_config()`
  - These become broken pointers if the referenced entity changed
- **Architecture and design comments**: High-level comments describing
  invariants or data flow can be silently invalidated
  - Example: `// only place X can occur` is wrong if your change
    introduces a second place X can occur
- **TODO, FIXME, HACK, and NOTE annotations**: Scan for these
  referencing any entity or behavior you changed
  - Remove or update any that were written against an assumption
    your change has resolved or made obsolete
- **Test docstrings**: If the behavior a test asserts has changed,
  update the docstring to reflect the new contract

### 12.3 What Constitutes a Stale Comment

A comment is stale and must be corrected if, after the change:

- It names an entity (function, type, variable, module, field, variant)
  that was renamed or removed
- It describes a behavior or contract the code no longer exhibits
  - Example: docstring says a function returns a sorted list, but the
    sort was removed
- It asserts an invariant the change violated
  - Example: `// always called with non-empty slice` when a new path
    passes an empty slice
- It references a file, module, or symbol path that no longer exists
  or has moved
- It describes a design rationale for a decision that was reversed
- It counts or enumerates things whose count changed
  - Example: `// handles 3 cases` when a case was added or removed
- It contains a `TODO` or `FIXME` that the change fully or partially
  resolved

### 12.4 Scope of the Sweep

Minimum scope scales with the type of change. When in doubt, sweep more
rather than less:

- Single function body edited
  - All docstrings in the same file
  - All direct callers in other files
- Function signature changed
  - All callers across the entire project
  - All files that import the symbol
- Type or struct definition changed
  - All files that construct, match on, or document the type
- Module or file renamed or deleted
  - All files whose comments reference it by name or path
- Behavioral invariant changed
  - All comments anywhere in the project asserting that invariant
- Public API removed
  - All files in the project; sweep all cross-reference comments

### 12.5 Execution Protocol

1. **Enumerate changed entities**: List every entity that was renamed,
   removed, semantically changed, or moved
   - Include functions, types, fields, modules, and invariants
   - This list is the input to the sweep
2. **Grep for references**: For each entity, run a text search across
   all source and test files
   - Do not rely on memory or assumption about where references exist
3. **Evaluate each hit**: Read each matching comment in full and in
   context
   - Do not assume proximity to unchanged code implies correctness
4. **Correct or remove**: Update stale comments to match current reality
   - If a comment describes something that no longer exists, remove it
   - If it partially applies, rewrite it without preserving false claims
5. **Do not silently skip**: If a stale comment is left uncorrected,
   surface it explicitly to the user with a reason
   - Silent omission of known stale comments is a standards violation

### 12.6 Prohibition on Scope Creep

The sweep is a correctness pass, not a quality or style pass. Do not
rewrite or improve comments that are still accurate. Confine changes
to genuinely stale content only
