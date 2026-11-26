# Implementation assignment

Keep one authoritative assignment. Include the existing issue identity and required tracking/commit/push policy when applicable. Preserve the requested phase and its authorization in the handoff; a brief request alone does not authorize implementation. Use only sections that carry useful information; a small change may need a few paragraphs. This is not a fixed sequence of implementation steps.

## Problem and outcome

Describe the user, trigger, current behavior or workaround, and desired observable result. Include a concrete example when it distinguishes the intended behavior.

## Decisions and boundaries

Capture confirmed decisions and why they were chosen. Separate proposed choices from confirmed ones. State the appetite if supplied, essential behavior, optional refinements, and explicit no-gos. Preserve relevant reasons for rejected alternatives without copying the conversation history.

## Solution

Explain the core flow or mechanism at a useful level of detail. Specify state transitions, failure behavior and precedence where ambiguity would change the product. Prescribe files, abstractions or libraries only when evidence justifies the constraint; otherwise label them as technical suggestions.

## Evidence and dependencies

Identify the repository, relevant instructions and canonical documents. Record inspected code/test locations and the revision or dirty-state context when significant. Separate verified facts from assumptions. List blocking uncertainties, how to resolve them, and required external configuration without including secrets.

## Existing behavior to preserve

Name affected contracts and representative existing flows that must survive. Cover persisted data, disabled states, shared consumers and compatibility as relevant. For migrations, identify representative old-state fixtures and expected results. These are starting points for engineering's impact analysis, not an assertion that no other consumers exist.

## Acceptance and evidence

For each material criterion, state the scenario, observable expected result and suitable verification. Include credible error and regression cases. Use repository-required commands where applicable, but do not substitute a command list for behavioral acceptance.

Distinguish automated tests with mocks, actual application/integration checks, and distribution requirements. Identify any check requiring user interaction or external setup and the precise claim that cannot be made before it passes. Merely documenting that limitation does not satisfy the criterion.

## Readiness and handoff

State whether the assignment is ready or needs a specific decision/investigation. Separate nonblocking implementation details from uncertainties that could invalidate the solution. If splitting the work, give each slice a coherent result and its dependencies without claiming the whole feature is complete after one slice.

Engineering may refine the technical plan while preserving the requested outcome and confirmed constraints. If evidence invalidates a material product decision, report that evidence and obtain the needed decision; do not silently shrink acceptance criteria or broaden scope.
