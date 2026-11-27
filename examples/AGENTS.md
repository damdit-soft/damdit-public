## Definition of done for website changes

A change to what the website serves is done only when a Damdit audit of the deployed site confirms it.

1. Before the change: `damdit-cli summary <project>` (run `damdit-cli audit <project>` if it is older than the last deploy). Note the runId.
2. Make the smallest change on the page and element the audit named.
3. After the deploy is live (check that the address serves the change): `damdit-cli audit <project>`, then `damdit-cli compare <project>`.
4. Report per issue: Resolved, Still detected, Inconclusive, plus anything New or Reappeared.

Say "fixed" only for Resolved. Inconclusive means not verified. If the audit can't run, say the change is unverified and why. Never judge the result from the code alone.

If `damdit-cli status` says `edition` `free`, or `compare` answers `licence_feature`, this is the free version: report what the newest audit shows for each rule you touched and call the change "unverified: comparing audits needs a Damdit licence or the 7-day trial", never "fixed".
