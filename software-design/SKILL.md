---
name: software-design
description: Mandatory guidelines and structured requirements for all software planning, implementation, and design artifacts. Must be applied to all proposals and plans.
triggers:
  - auto
context:
  - planning
  - design
  - implementation_plan
---

# Software Design Guidelines

Mandatory guidelines for creating software planning, implementation, and design artifacts. These instructions must be automatically applied whenever formulating a plan, proposing a design, or writing an implementation proposal.

## 1. Existing State & Context Verification

To prevent hallucinated dependencies and incorrect assumptions, all plans must explicitly state:

- **Verified Context**: A confirmation that the agent has reviewed the existing code, KI (Knowledge Items), and current architecture related to the proposal.

## 2. Edge Cases and Defensive Programming

Focusing solely on the "happy path" is a common deficiency. Every plan must include defensive programming principles:

- **Zero-Trust Boundaries**: Treat all incoming inputs, especially those crossing trust boundaries (network layers, deserializers, user inputs), as potentially malicious. Make absolutely no assumptions about safety.
- **Fail Early and Defensively**: Detail how inputs will be explicitly validated at the boundary, ensuring corrupted state cannot propagate. 
- **Resource Exhaustion**: Ensure bounding is applied to recursive structures, arrays, and loops to prevent Denial of Service (DoS), OOMs, and stack overflows.
- **Robust Error Handling**: Design systems to gracefully handle and return structured errors or exceptions rather than crashing or panicking when a dependency fails or times out.

## 3. Counterfactual Thinking

For any significant design decision, explicitly engage in counterfactual reasoning. Document multiple approaches and synthesize into a clear comparative analysis:

- **Alternative Approaches & Counterfactuals**: Detail several alternative paths or counterfactual scenarios that were considered.
- **Pros and Cons**: Provide a balanced list of pros and cons for every approach, including the primary solution one that is not considered an alternative.
- **Descending Order of Desirability**: When presenting the options (including the primary proposed solution), rank and present them in descending order of desirability. The most desirable solution should be listed first, followed by the next best alternative, down to the least desirable.

## 4. Testability and Observability

A plan is incomplete if it cannot be verified. Include:

- **Testing Strategy**: How the changes will be validated (unit, integration, or end-to-end tests), including "negative" test coverage using fuzzed or intentionally corrupted data to ensure robust error boundaries.
- **Observability**: What metrics, logging, or monitoring will be added to track the behavior of this change in production.

## 5. Impact Analysis

Whenever a proposal is offered during design, an **Impact Analysis** must be included. This helps decision-makers evaluate options and trade-offs.

At the start of the Impact Analysis section, explicitly rate the proposal on an **Impact Scale** from `0` to `10`:
- `0`: No impact (e.g., pure documentation changes, no behavioral change)
- `10`: A complete rewrite of the entire project or a fundamentally breaking architecture change

The impact analysis must concisely but informatively analyze the following dimensions:
- **Edit Distance**: The breadth and volume of code changes required (e.g., number of files affected).
- **Merge Conflicts**: The potential for merge conflicts with other ongoing work.
- **Semantic Drift**: The likelihood that this change shifts the original meaning, architecture, or intent of existing components.
- **Feature or Scope Creep**: The risk that this proposal naturally expands into unrelated areas or triggers cascading changes.
- **Performance**: High-level analysis of expected effects on runtime performance, memory usage, latency, and system load.
- **Overall Effects**: A high-level summary of the broader consequences on the codebase, developer experience, and system maintainability.
- **Maintainability**: Will these changes make the project more difficult to maintain or easier to maintain? Explain and justify.

## 6. Architectural Shifts and Deprecations

Whenever a plan proposes replacing, deprecating, or rendering obsolete an existing architectural component (e.g., removing a phase, ripping out a module, or shifting from dynamic to static analysis), it must be given its own dedicated, top-level section.

This section must include:
- **Detailed Subsections**: Break down the exact mechanisms of the old vs. the proposed new architecture.
- **Deep Analysis**: A thorough investigation of why the old component is no longer needed, addressing any edge cases that the old component handled.
- **Runtime vs. Static Implications**: If shifting phases (e.g., runtime to static), explicitly detail how the downstream consumers will adapt.

## 7. No Speculative Code Generation (Token Conservation)

To save tokens and maintain focus on high-level architecture, **do not generate speculative code blocks** in the implementation plan unless explicitly prompted to do so by the user. 
- Use text-based, high-level algorithmic design descriptions instead of writing out data structures or implementation logic in code, unless specifically asked.
