---
name: implementation-shaping
description: Develop software or product ideas and turn an existing brief or conversation into a bounded implementation assignment using Shape Up principles. Use for “I have an idea, let’s develop it” or “prepare an implementation assignment for my coding agent”; not for implementing an agreed feature, directly fixing a bug, or pricing a client proposal.
---

# Implementation Shaping

Help the user move from an idea or existing brief to a feasible, bounded assignment that another agent can implement without reconstructing the conversation. Preserve decisions and their reasons; keep implementation latitude where the evidence allows it.

## Choose the requested mode

Infer the mode from the user's words and current conversation. Do not make the user choose a skill or repeat settled context.

| User intent | Action |
| --- | --- |
| “I have no brief, only an idea. Let’s develop it.” | Explore the problem and possible solutions. Return a recommendation, uncertainty and the next meaningful decision. Do not silently turn exploration into an approved specification or start implementation. |
| “I have a brief. Prepare the implementation assignment.” | Shape the supplied brief and agreed conversation into a self-contained assignment. Reuse settled decisions; investigate gaps before asking. |
| “Implement this feature as specified.” | Hand the assignment to `$engineering-orchestrator` if available, preserving existing implementation authorization. Do not make shaping a mandatory extra stage when the assignment is already usable. |
| “Fix it.” | Use `$engineering-orchestrator` if available to diagnose and fix the problem. Do not require the user to prepare a separate brief or issue. The agent still satisfies any mandatory repository tracking gate before the repair. |

If a named companion skill is unavailable, follow its role using the repository instructions and current authorization rather than stopping solely because of the missing skill. A request to prepare a brief does not authorize implementation. External tracking writes follow existing user or repository authorization, including a standing policy; without that authorization, do not post to an external service. A combined request to prepare and implement does authorize both within the stated scope.

## Work with other skills

One workflow owns the current phase. Use a specialist skill or person only for a concrete unresolved question (the target user, an interaction or accessibility decision, a commercial commitment) and bring the answer back as evidence, a decision or a hypothesis. Check a claim that drives a decision against its source; a polished handoff is not independent proof. Don't reopen confirmed strategy or an agreed design just to write an assignment.

Engineering owns technical planning, implementation and verification. A failed technical assumption returns here only when it changes the product solution or the agreed boundary; ordinary implementation choices stay with engineering. When an issue workflow called this skill, return the shaped content to it instead of starting that workflow again.

## Tracking policy

Before substantive shaping, follow the user’s and repository’s work-tracking policy. When issues are mandatory and routinely authorized, search and reuse or create the appropriate record even for exploration; record an unresolved idea as a question, not an approved implementation. Pass the same issue through specialist and engineering handoffs. Do not wait for a separate “create issue” instruction when standing authorization already covers it. Preliminary reads needed to identify the correct issue are allowed. If no standing tracking policy applies, keep the user’s requested output format and authorization boundary.

Preserve material decisions, evidence and readiness in the authoritative issue or its linked versioned specification. Follow required commit/push steps for document changes and verify synchronization; a local brief is not automatically available on GitHub. Pure investigation can be recorded in the issue without an empty commit.

## Establish evidence and boundaries

Read repository instructions and relevant current product contracts. Inspect the affected implementation, data flow and tests when access is available. Resolve facts from these sources before asking the user. Check current primary documentation for consequential external API or platform assumptions.

Distinguish confirmed user decisions, verified facts, proposed choices, technical hypotheses and open questions. Cite concrete files, source links or observed behavior where they matter. Record the inspected revision and relevant uncommitted changes when the brief depends on the current code. If the repository is unavailable, say so; do not present guessed paths or architecture as verified.

Ask only for a material product, business or authorization decision that cannot be recovered from context. Continue independent investigation while it is pending. Routine technical choices do not require approval; do not treat your own proposed product change as user-approved.

## Shape with an appetite

Use Shape Up's problem, appetite, solution, rabbit holes and no-gos as decision tools, not a mandatory ceremony or a fixed-length template.

- Start with a specific user situation, current behavior or workaround, and the observable improvement.
- Preserve a stated appetite: how much time or effort the user is willing to invest. If none was given, do not invent a deadline, budget or six-week cycle. Propose a bounded first version and label it as a proposal; ask about appetite only when it materially changes the solution.
- Describe a coherent solution, including the important interaction or data flow. A problem statement alone is not a shaped assignment. Leave routine implementation details open unless an inspected constraint justifies prescribing them.
- Investigate the risks that could invalidate the solution before committing to it. If a decisive feasibility question remains, identify the smallest permitted investigation or prototype that would resolve it. A list of risks does not establish feasibility. Do not mutate the product or external configuration merely to shape a brief.
- Specify exclusions and distinguish essential behavior from optional refinements. Only explicitly discretionary scope can be traded off. Safety, data preservation, compatibility commitments and required verification are not expendable scope.

When work is too broad for a reviewable outcome, propose coherent slices with their own observable results and dependencies. Avoid splitting into isolated “UI done”, “backend done” tasks that cannot demonstrate useful behavior. Create external tracking records only under an explicit request or applicable standing authorization; do not invent schedules.

## Prepare the handoff

For an implementation assignment, use [the brief structure](references/implementation-brief.md), scaling detail to the change. It is an information checklist, not a demand for every heading or a numbered coding recipe.

Before handing off, check that:

- the essential outcome, confirmed decisions, rationale, scope and no-gos survived the conversation;
- technical suggestions are distinguished from binding product or compatibility requirements;
- affected existing behavior and credible regression scenarios are identified, with engineering responsible for completing the dependency analysis;
- state transitions, precedence rules and migration behavior have no known material contradiction;
- acceptance criteria describe observable behavior, including relevant failure cases, and identify how to obtain evidence;
- mocked tests, actual integration checks and release prerequisites are clearly distinguished;
- no unresolved decision or unverified assumption is presented as settled merely to finish the document.

Mark the assignment **ready for implementation** when the core solution is sufficiently grounded and no unresolved product or feasibility question can materially invalidate it. Ordinary coding details may remain open. Otherwise state the specific **decision or investigation needed**, while completing the parts that do not depend on it.

External configuration may remain a documented dependency; say what implementation can proceed and which integration acceptance checks remain unavailable. Never imply that mock coverage will establish real integration success. Engineering must revalidate change-sensitive assumptions and verify the resulting behavior.

Deliver the requested format: a concise answer, a local Markdown file when requested or clearly intended, or content returned to the calling issue workflow. A mandatory repository tracking policy still requires the corresponding issue even when the user asks for a local file. Use a clear outcome-based title. Do not create parallel copies of the same brief or publish it without authorization.

## Source principles

- [Shape Up: Set Boundaries](https://basecamp.com/shapeup/1.2-chapter-03)
- [Shape Up: Write the Pitch](https://basecamp.com/shapeup/1.5-chapter-06)

The evidence and verification handoff above adapts those shaping principles to agent implementation; it is not a claim that Shape Up itself supplies a software verification protocol.
