---
name: engineering-orchestrator
description: Implement an agreed software feature or diagnose and fix a defect, and hand it off with evidence for every acceptance criterion. Use for “implement this as specified”, “fix it”, integrations or release preparation; not for exploring an idea, only preparing an implementation brief, or a read-only review.
---

# Engineering Orchestrator

Own the change from the user's requested outcome to an evidence-backed handoff. Optimize for the smallest maintainable change that meets the acceptance criteria, not for code volume, process theatre or speculative infrastructure.

## The rule everything else serves

**Work is `complete` only when every acceptance criterion has actual evidence in the delivered state.** Code that looks right is not evidence. A passing build is not evidence of behavior. A limitation you wrote down is a record of missing evidence, not a pass.

So before you change anything, know the criteria; while you work, keep each one tied to how it will be shown; and at the end, report one of four statuses:

| Status | Use when |
|---|---|
| `complete` | Every agreed criterion and required regression check has evidence for the delivered state. |
| `unverified` | The change is made, but material behavior still lacks evidence (a deploy you can't reach, a real integration you can't run). Name the missing check and what it needs. |
| `needs decision` | A product, business or authorization choice is the user's and would change the solution. |
| `blocked` | An obstacle prevents the remaining work. |

The last two describe a handoff. They are not a reason to stop while useful authorized work remains. A filled-in example of each: [references/handoff-examples.md](references/handoff-examples.md).

## Entry

- **Implement an assignment:** reuse the supplied outcome, confirmed decisions and scope. Check its technical assumptions against the current repository. Don't ask for another planning round or for approval of ordinary implementation choices.
- **Fix a defect:** “fix it” authorizes diagnosis, a bounded repair and verification. Recover the failing scenario from context and the project; the user doesn't have to write a specification. How to diagnose: [references/defects.md](references/defects.md).
- **Explore or prepare a brief only:** that is shaping, not engineering. Use the `implementation-shaping` skill when it is installed and stay within the requested phase. A request to prepare and implement authorizes both.

Engineering owns implementation and verification. Use a specialist skill or person only for a concrete missing decision or capability, and bring the result back. Don't reopen settled product or design decisions without new evidence. A missing companion skill is not a reason to stop, and it doesn't waive the evidence that skill would have required.

If the user or the repository tracks work in issues, commits and pushes, that policy applies before you start, small fixes included: [references/tracking.md](references/tracking.md).

## 1. Before work

1. Read the repository's instructions and only the product, architecture or operations documents the change touches. They take precedence over this skill.
2. Look at the working tree. Preserve unrelated work in a dirty tree: don't reset it, rewrite it or absorb it into your change.
3. Choose the depth of evidence:

| Tier | Use when | Minimum evidence |
|---|---|---|
| Focused | Local, reversible or documentation-only, no behavioral risk | Inspect the affected area; run the relevant check or say why none is needed |
| Standard | User-visible behavior, business logic, several files, an integration, persisted state or real regression risk | Contract, impact analysis, chosen approach, relevant automated and manual verification |
| Critical | Security, credentials, permissions, privacy, a migration, a destructive operation, money, a public API, production, a release or anything irreversible | Standard, plus explicit authorization for the sensitive action, a rollback or containment plan and the strongest practical verification. See [references/critical-changes.md](references/critical-changes.md) |

In doubt, take the higher tier. A tier sets the depth of evidence; it never permits an action nobody authorized.

## 2. The contract

Read the authoritative assignment. Before you edit code, be able to state:

- the user and the observable outcome;
- the acceptance criteria;
- what is in scope and what is explicitly not;
- assumptions, dependencies and the appetite or risk boundary.

Reuse a prepared brief; don't write a second one. Find missing facts in the code, tests, design and documentation before you ask. Ask the user only for a real product, business or authorization decision that would change the solution, and don't write code around that decision while it is open.

## 3. Inspect and decide

For Standard and Critical work, find before you implement: the affected data flow and modules, the existing patterns, related tests, persisted data and compatibility, the credible regressions, the smallest design that works and any real alternative.

Trace shared components, call sites and data contracts beyond the files you edit. Name the existing behaviors that must survive and pick regression scenarios from that. A short diff does not mean a small impact.

Take a baseline where a behavior is about to change: reproduce the defect or run the focused existing checks, and note any failure that was there before you. Otherwise you can't later tell your regression from an old one.

Say which approach you chose, why it fits the system, which files you expect to touch and how you will verify. Ask for confirmation only when the design holds a product choice, widens the scope or has an irreversible or external effect. Otherwise go on.

## 4. Implement inside the boundary

Make only the approved or clearly implied change, with the project's existing patterns and dependencies. A refactor, a new abstraction or a new dependency is not a substitute for a small direct solution. If the implementation disproves the design, stop and redesign instead of layering a workaround.

An appetite can drop refinements that were marked optional. It can't silently drop an acceptance criterion, a compatibility commitment, error handling or required verification.

When you meet a separate problem, classify it and stay in scope:

- **Note:** trivial or not actionable. Mention it only if useful.
- **Backlog candidate:** worth fixing on its own. Report the problem, impact, evidence and why it was left out. Recording it does not authorize fixing it.
- **Blocker:** prevents a safe or correct result. Show the evidence and get the direction you need.

Never publish, deploy, change production, expose credentials, delete data, create external work items or widen access without the user's authority. Permission to commit and push is not permission to deploy.

## 5. Verify the outcome

Map every acceptance criterion and regression scenario to evidence that fits the risk:

| Criterion | Evidence | Result |
|---|---|---|
| The footer logo has alt text on every page | Audit of the deployed site after the change | Resolved on 31 of 31 pages |
| Existing pages keep their titles | `npm test`, title snapshot | 42 passed |
| The sign-in e-mail arrives | Not run: needs the staging mail key | **missing**, so `unverified` |

- **Use the right verifier for the surface.** Tests show that code behaves. They don't show what a website serves, what an API returns over the network or what an app does on a device. If a skill or tool is installed that verifies this kind of result (a website audit, an API contract check, a device run), use it as the source of evidence, and follow its own rules for what counts as verified.
- Run the strongest practical checks and the commands the repository requires. Report what actually ran and what it returned. Never infer a pass from a check you didn't run.
- Exercise changed user-visible behavior through the real application or integration path, failure states included. A mock shows your code's side only; mark the real integration `unverified` until it ran.
- For a meaningful bug fix, add a regression test where practical and show that it fails without the fix.
- Evidence must be about the final state. After a later edit, rerun the checks it could have invalidated.
- **Know when to stop.** If the same criterion still fails after two attempts at a fix, stop changing code. Report what you tried, what each attempt showed and what you now think the cause is. A third guess without a new idea costs the user more than the report.
- Never weaken an assertion or delete a failing test to get a pass. If the intended behavior changed, show that the revised check still catches wrong behavior.
- Read the final diff, new files included, for anything outside the boundary, debug leftovers and accidental contract changes. Then read the original assignment again and compare.

## 6. Handoff

```text
Status
complete | unverified | needs decision | blocked

Outcome / decision
What now works, or which decision is needed.

Changes made
Main files or modules, and why this was the smallest suitable approach.

Verification
Criterion → actual evidence. Checks not run, and why.

Risks / limitations
What could still regress; what you are not sure of.

Out-of-scope findings
Only findings worth acting on, with their classification.

Follow-up
A concrete next decision or action, if one remains.
```

If the user scoped the task to a prototype or to preparing code, report completion of that scope only, never of the unverified feature or release.
