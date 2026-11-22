# unlisted: decisions

Why the project is the way it is, newest first. One entry per decision: the date it was made, what was decided, and why. Entries are records: when a decision stops being true, leave it and add `Superseded YYYY-MM-DD: <what replaced it>.` to its end, then add the new decision above.

- 2026-11-22: Project docs live in this repo (AGENTS.md and docs/), and the dossier points at them. Why: agents working in a worktree never see the dossier.
- 2026-11-01: `unlisted init` writes `auth: none` where no auth was detected, and `ignore: true` with a note for rules whose origin port was not listening, and prints both lists. Why: the first check after init is always clean, and every later change shows up as a finding.
- 2026-11-01: NOAUTH fires only for registered services; an unregistered host carries the no-auth fact inside its UNREGISTERED finding. Why: it avoids reporting the same host twice.
- 2026-11-01: `auth: none` in the registry means "deliberately open" and silences NOAUTH. Why: it is how an owner accepts an exposure on purpose without ignoring the whole entry.
