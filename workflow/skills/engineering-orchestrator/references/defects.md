# Diagnosing a defect

1. **Establish the scenario:** the trigger, what was expected and what happened. Recover it from the conversation, logs and the project. If the target is truly ambiguous, ask the smallest question that identifies it.
2. **Reproduce it** where practical, before changing anything.
3. **Trace the failure** to the code, state, logs or tests that support it. Keep a demonstrated cause apart from a hypothesis.
4. **Repair the cause**, with the smallest bounded change.
5. **Prove it:** a regression test that fails without the fix and passes with it, where practical. For interactions automation can't cover reliably, a manual reproduction before and after is the evidence. Avoid tests that only mirror the new implementation.

If you can't reproduce it, get the closest reliable evidence and report what stays uncertain. Never claim that a speculative change fixed the bug.
