# Work tracking and authorization

Applies when the user or the repository tracks work in issues, commits and pushes. Without such a policy, don't infer permission for any external write.

- **Before substantive work**, direct fixes included: find and read the existing issue, or create the right record when standing authorization covers it. Carry its identity through shaping and implementation. A required tracking gate is not optional because the repair is small.
- Don't ask again for routine issue updates, commits or pushes the policy already authorizes.
- Keep decisions, the verification that actually ran, remaining limitations and the commit or pull-request links in the same issue.
- After each coherent verified change: update the durable documentation it affects, then commit and push only that change, as the repository requires. Look at what is staged and what is outgoing, leave unrelated work alone, check the remote result and report a failed sync.
- Never let an automatic closing keyword close an issue before its acceptance criteria have evidence.
- Documentation is versioned with the change. A chat summary or a temporary local file is not durable evidence.
- A backlog candidate (SKILL.md, step 4) gets its own linked record only when the standing policy authorizes and requires it; otherwise report it without an external write.
