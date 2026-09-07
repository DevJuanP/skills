# skill-template — cómo usarlo

Template de skill conforme a la spec oficial **Agent Skills** (`https://agentskills.io/specification`,
repo `agentskills/agentskills`, originalmente Anthropic) + patrones prácticos de
`anthropics/skills` (`skill-creator`) y del CLI `vercel-labs/skills` (`npx skills init`).

## Qué incluye

```text
skill-template/
  SKILL.template.md         # plantilla completa con TODOs (renómbrala a SKILL.md al copiar)
  references/
    REFERENCE.md            # detalle cargado bajo demanda (borrar si no hace falta)
    EXAMPLES.md             # ejemplos 3..N (en SKILL.md solo van 2)
  scripts/
    example.sh              # helper opcional (borrar carpeta si no hay código)
  assets/
    .gitkeep                # plantillas/recursos de salida (no se cargan al contexto)
```

> El archivo se llama `SKILL.template.md` (no `SKILL.md`) a propósito: así el CLI
> `npx skills` no lo detecta como skill real y skills.sh no lo indexa.

## Cómo crear una skill nueva

```bash
cp -r skill-template skills/mi-skill
mv skills/mi-skill/SKILL.template.md skills/mi-skill/SKILL.md
# 1. Renombra la carpeta == `name:` del frontmatter (kebab-case, 1-64 chars)
# 2. En skills/mi-skill/SKILL.md reemplaza todos los TODO
# 3. Borra secciones/archivos que no necesites (mínimo: frontmatter + Instructions + 1 ejemplo)
# 4. Mantén SKILL.md < 500 líneas; lo pesado va a references/
```

## Reglas duras (la validación falla si las rompes)

- Archivo exactamente `SKILL.md` (mayúsculas), carpeta == `name:`.
- `name:` solo `a-z 0-9 -`, sin empezar/terminar en `-`, sin `--`.
- `description:` 1-1024 chars, tercera persona, QUÉ hace + CUÁNDO usarlo.
- Campos reconocidos: `name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`. El resto se ignora.
- Referencias con rutas relativas de un nivel: `references/X.md`, `scripts/y.sh`.

## Validar antes de publicar

```bash
npx skills add TU-USUARIO/skills --list
# (opcional, validador de referencia)
skills-ref validate ./skills/mi-skill
```

## Fuentes

- Spec: `https://agentskills.io/specification`
- Ejemplo real: `https://github.com/anthropics/skills/tree/main/skills/skill-creator`
- CLI: `https://github.com/vercel-labs/skills` (`npx skills init my-skill`)
