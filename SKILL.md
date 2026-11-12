---
name: maintainability-review
license: MIT
description: "Review code for human handoff and maintenance cost, then make selective refactors when requested. Use for feature discoverability, state ownership, locality and abstraction reviews. Do not use for routine feature work or formatting alone."
---

# Maintainability review

Optimize for a competent developer who is new to the repository. They should find a feature, understand its behavior and change it without learning unrelated subsystems.

## Trace a real maintenance task

Use the requested scope and the repository's current behavior. For a UI application, start with visible controls or interactions.
Choose representative features that exercise different boundaries, such as timeline editing, project saving and preferences.

For each feature:

1. Find its entry point from its visible name or behavior.
2. Trace the interaction, state owner, mutation path and consumers.
3. Identify the files needed for a plausible local change.
4. Identify forwarding, duplicate rules or implicit synchronization that makes the change harder.

Use source locations and concrete maintenance scenarios as evidence. Size metrics and dependency counts identify candidates, but do not establish a need to refactor.
If a smell audit already exists, reassess its findings through these scenarios. Keep the audit evidence separate from the implementation decision.

## Choose boundaries for the reader

- Name components and modules after visible features or business concepts. Keep feature-specific UI, behavior and types near their owner.
- Keep code together when it changes together or maintains one invariant. Separate independent responsibilities, lifetimes or reuse boundaries.
- Preserve a clear entry point. A cohesive long file can be easier to maintain than many files connected by forwarding.
- Remove forwarding or configurable abstractions only when they add no useful policy, lifecycle or boundary. One consumer alone does not make an abstraction unnecessary.
- Give each business fact and editable state a clear owner. Derive projections where practical, and make necessary synchronization explicit.
- Share business rules and contracts when they represent the same fact. Keep small local duplication when sharing would couple independent behavior.
- Use types to express real states, units and boundaries. Avoid wrapper types or generic machinery that add no useful constraint.
- Keep comments that explain compatibility, non-obvious algorithms, platform differences and design tradeoffs. Remove narration of obvious code.

Preserve meaningful platform and audio-processing boundaries. Do not add a framework for speculative future features, including multi-track support.
Evaluate future requirements only when they expose a concrete obstacle in the current design.

## Make selective changes

For each candidate, explain the current cost, the maintenance task affected and why the proposed structure reduces that cost.
Check whether the proposal removes complexity or only moves it. Weigh the benefit against behavior risk and validation effort.
Leave code unchanged when the benefit is unclear.

Review-only requests produce findings. When changes are requested, implement the smallest coherent improvement within that scope.
Follow repository constraints. Preserve file compatibility, audio results and platform behavior unless the user explicitly requests an authorized change.
Keep unrelated features and UI redesigns out of the refactor.

## Verify the maintenance path

Trace the same scenario after the change. Confirm that its entry point, state owner and mutation path are easier to follow.
Test observable behavior through the implementation the application actually uses. Avoid preserving a second implementation solely to keep old tests passing.
For state changes, check the affected lifecycle boundaries, including undo, replacement and pending asynchronous work when relevant.
Use the repository's applicable checks. Report actual coverage, skips and failures. Claim CI success only for the exact commit it verified.

Update existing navigation or development documentation when the implementation paths change. Avoid a separate architecture manifesto.
Briefly report the maintenance costs addressed, changes made, abstractions retained or removed, feature entry points and verification limits.
