---
name: ship
description: Commits and pushes current git changes with a Conventional Commits message in Spanish. Use when user says ship, sube los cambios, push the changes or asks to upload current changes.
license: MIT
metadata:
  author: Juan Pajuelo (DevJuanP)
  repository: https://github.com/DevJuanP/skills
  version: "0.1.0"
---

# Ship

Commit and push the current repo changes, in this order:

Current repo state:

!`git status --short`
!`git diff --stat HEAD`

## Instructions

1. **Inspect:** if there are no changes, say so and finish without doing anything.
2. **Safety:** review the full `git diff HEAD`. If there are secrets (tokens, passwords, private keys, `.env`), do NOT continue and warn the user.
3. **Stage:** `git add -A`.
4. **Message:** generate a Conventional Commits message in Spanish, based on the real diff (not the stat):
   - Format: `<tipo>: <descripción breve en minúsculas, sin punto final>`
   - Types: `docs` (notas .md), `feat` (contenido nuevo), `fix` (correcciones), `chore` (tooling/config), `refactor`
   - If the user gives extra context ($ARGUMENTS), use it as the base and polish it into the format.
5. **Commit:** `git commit -m "<mensaje>"`. If it fails due to hooks, show the error and stop (never use `--no-verify` without asking).
6. **Push:** check upstream with `git rev-parse --abbrev-ref --symbolic-full-name @{u}`. If it fails, `git push -u origin <rama-actual>`; otherwise plain `git push`.
7. **Report** in 2 lines: commit message + push result.

## Safety and Limitations

- ALWAYS check the full diff for secrets before staging.
- NEVER amend, rebase, or force-push.
- If the diff mixes two unrelated topics, ask before splitting into two commits.

---

© 2026 Juan Pajuelo (DevJuanP). Licencia MIT. Repo canónico: https://github.com/DevJuanP/skills.
