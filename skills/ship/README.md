# ship

Skill de agente que hace commit y push de los cambios actuales del repo en un solo paso.

## Qué hace

1. Inspecciona el estado del repo (`git status`, `git diff`). Si no hay cambios, lo dice y termina sin hacer nada.
2. Revisa el diff completo por seguridad: si detecta secretos (tokens, passwords, claves privadas, `.env`), se detiene y avisa.
3. Añade los cambios (`git add -A`), genera un mensaje **Conventional Commits en español** basado en el diff real y hace commit.
4. Pushea a la rama actual (configura upstream con `-u` si aún no existe).
5. Reporta en 2 líneas: mensaje del commit + resultado del push.

Nunca hace amend, rebase ni force-push. Si el diff mezcla dos temas no relacionados, pregunta antes de dividir en dos commits.

## Instalación

```bash
npx skills add DevJuanP/skills --skill ship
```

## Uso

Pídele a tu agente, por ejemplo:

- «ship»
- «sube los cambios»
- «haz commit y push»

Puedes añadir contexto extra para el mensaje del commit («ship: estuve trabajando en el login»).

## Licencia

MIT — © 2026 DevJuanP. Ver `LICENSE` en la raíz del repo.
