# Handoff examples

## complete

```text
Status
complete

Outcome
The footer logo has alt text and the footer no longer links to /integrations, which doesn't exist.

Changes made
src/components/Footer.astro: alt="Acme" on the logo; the /integrations link removed. Two lines, no new component.

Verification
- Logo alt text → audit of staging after the deploy (run f37b…): Resolved on 31 of 31 pages.
- No link to a missing page → same audit: Resolved; no page links to /integrations.
- Nothing else changed → comparison with the audit before (run 9be5…): 0 New, 0 Reappeared.
- npm run build: passed.

Risks / limitations
None known. Performance was not measured in either audit (PageSpeed is off for this project).

Out-of-scope findings
Backlog candidate: 5 pages have no meta description (medium). Not requested, not changed.
```

## unverified

```text
Status
unverified

Outcome
The alt text and the link are fixed in the code and committed. The deployed site has not been checked.

Verification
- Logo alt text → not verified: the staging deploy is behind a login I have no access to.
- npm run build: passed. This shows the site builds, not what it serves.

Follow-up
Give the audit access to staging (or deploy to an open preview), then one audit and its comparison settle both criteria.
```

## needs decision

```text
Status
needs decision

Decision needed
The footer links to /integrations, which doesn't exist. Remove the link, or is the page coming? Removing is two lines; a redirect to /for/ is the alternative.

Done meanwhile
The alt text is fixed and verified (audit run f37b…: Resolved).
```

## blocked

```text
Status
blocked

Obstacle
The audit can't start: Docker is not running on this machine, and I'm not allowed to start it.

Done meanwhile
Both fixes are committed on the branch fix/footer. Nothing is verified.

Follow-up
Start Docker, then run the audit again.
```
