---
description: Añade, commitea con mensaje conventional y pushea los cambios actuales
---

Haz commit y push de los cambios actuales del repo, en este orden:

Estado actual del repo:

!`git status --short`
!`git diff --stat HEAD`

Pasos:

1. **Inspecciona**: si no hay cambios, dilo y termina sin hacer nada.
2. **Seguridad**: revisa `git diff HEAD` completo. Si hay secretos (tokens, passwords, claves privadas, `.env`), NO continúes y avísame.
3. **Añade**: `git add -A`.
4. **Mensaje**: genera un mensaje Conventional Commits en español, basado en el diff real (no en el stat):
   - Formato: `<tipo>: <descripción breve en minúsculas, sin punto final>`
   - Tipos: `docs` (notas .md), `feat` (contenido nuevo), `fix` (correcciones), `chore` (tooling/config), `refactor`
   - Si hay contexto extra del usuario: $ARGUMENTS. Úsalo como base del mensaje y pule al formato.
5. **Commit**: `git commit -m "<mensaje>"`. Si falla por hooks, muestra el error y detente (no uses `--no-verify` sin preguntar).
6. **Push**: comprueba upstream con `git rev-parse --abbrev-ref --symbolic-full-name @{u}`. Si falla, `git push -u origin <rama-actual>`; si no, `git push` a secas.
7. **Reporta** en 2 líneas: mensaje del commit + resultado del push.

Reglas: si el diff mezcla dos temas no relacionados, pregunta antes de dividir en dos commits. Nunca hagas amend, rebase ni force-push.
