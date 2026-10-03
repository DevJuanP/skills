---
title: OpenCode - Skill design-extractor (guia de diseño desde URL)
area: desarrollo
tags: [desarrollo, desarrollo/ia, buenas-practicas]
created: 2026-10-02
updated: 2026-10-03
related: ["./opencode-agents-skills-mcp-commands.md", "../buenas-practicas/refero-styles-design-md.md"]
---

# OpenCode - Skill design-extractor (guia de diseño desde URL)

Skill reutilizable que, dada la URL de una web, explora el sitio completo y genera `design_[dominio].md` con paleta, tipografías, botones, formularios, login y resto de la guía de diseño.

## Instalación en PC

1. En tu proyecto crea `.opencode/skills/design-extractor/SKILL.md`.
2. Copia en ese archivo solo el bloque de la sección `SKILL.md lista para copiar`.
3. Verifica con `/` o diciendo `extrae el diseño de https://...`.

## SKILL.md lista para copiar

```markdown
---
name: design-extractor
description: Extrae la guia de diseño de una web desde su URL y genera design_[dominio].md con paleta, tipografias, botones, formularios y login. Usa MCP browser si existe, si no pregunta nivel Simple o Completo.
---

# Design Extractor

Objetivo: dada una URL, explorar el sitio completo y generar `design_[dominio].md`.

## 1. Normalizar entrada

1. Acepta cualquier URL (`https://ejemplo.com/ruta` o `ejemplo.com`).
2. Extrae el dominio limpio: minusculas, sin `www.`, sin puerto, sin ruta. Ej: `https://www.ejemplo.com/login` -> `ejemplo.com`.
3. Nombre de salida: `design_ejemplo_com.md` (puntos a guion bajo). Guardalo en el directorio de trabajo salvo que el usuario indique otro.

## 2. Detectar capacidades (adaptativo)

1. Lista recursos MCP con `list_mcp_resources`. Busca servidores tipo `chrome-devtools`, `playwright`, `browser`, `puppeteer`.
2. Si hay MCP browser: usalo (navegacion real, estilos computados, screenshots).
3. Si NO hay MCP browser: pregunta al usuario con opciones:
   - `Simple (Recommended)`: solo `webfetch` + observacion. Sin dependencias. Paleta aproximada, componentes principales.
   - `Completo`: descarga HTML/CSS con `shell` (`curl`/Python), parsea HEX, `font-family`, clases de botones/inputs. Requiere internet y shell.
4. No avances sin saber el nivel.

## 3. Crawl sitio completo

Visita como minimo (omitir con justificacion si da 404):

- `/` (home)
- `/login`, `/signin`, `/signup`, `/register`
- `/pricing`, `/planes`, `/contact`, `/contacto`
- 1 pagina de contenido (`/blog`, `/docs`, `/about`)
- Cualquier formulario visible (busqueda, newsletter, checkout)

Registra URL visitada + estado. Si una ruta falla, prueba 1 alternativa y sigue.

Modo paralelo (opt-in, solo Completo/webfetch sin MCP browser): por defecto secuencial. Usa subagentes solo si hay 4+ paginas por visitar. Fan-out por paginas, nunca por componentes (separar botones vs formularios duplica descargas y genera HEX inconsistentes). Ej: A: `/ + /pricing`, B: `/login + /signup`, C: `/blog|/docs|/about + /contact`. Max 3 subagentes. Cada subagente devuelve evidencia cruda (URL, estado, HEX + selector, font-family, clases) y no escribe `.md`.

## 4. Extraccion (que mirar en cada pagina)

- **Paleta:** primarios, secundarios, acento, fondo, texto, estados (hover/disabled/error). Siempre en HEX (`#RRGGBB`). Si es Simple, estima desde lo visible y marca `~aprox`.
- **Tipografias:** familias, tamaños de h1-h6/body, pesos, line-height.
- **Botones:** primario/secundario/ghost/danger, radius, padding, estados.
- **Formularios:** inputs, labels, placeholders, validacion, focus ring.
- **Login/signup:** layout (centrado/split), OAuth social, recordar/clave olvidada.
- **Navegacion:** header, footer, menu movil.
- **Espaciado/layout:** max-width container, grid, radius base, sombras.
- **Iconografia e imagenes:** estilo (outline/solid), bordes de fotos.
- **Responsive y accesibilidad:** breakpoints, contraste evidente, foco visible.

En modo Completo o con MCP, respalda cada color con selector CSS o estilo computado citado.

## 5. Generar design_[dominio].md

Merge centralizado con un solo escritor: si hubo fan-out, el agente principal deduplica primero (misma HEX con distinto nombre -> elige 1 y cita paginas) y despues genera el archivo. El auditor de estilos sin extraer va al final y en secuencial, comparando clases CSS recolectadas vs tokens.

Usa exactamente esta plantilla, sin inventar secciones nuevas salvo `Notas`:

```markdown
# Design - [Dominio] ([URL])
> Fuente: [URL] | Fecha: [YYYY-MM-DD] | Nivel: Simple/Completo/MCP | Paginas: [lista]

## 1. Paleta
| Rol | HEX | Uso |
|-----|-----|-----|
| Primario | #... | ... |

## 2. Tipografias
- ...

## 3. Botones
- ...

## 4. Formularios e inputs
- ...

## 5. Login / Signup
- ...

## 6. Navegacion (header/footer)
- ...

## 7. Espaciado, radius y sombras
- ...

## 8. Iconografia e imagenes
- ...

## 9. Responsive y accesibilidad
- ...

## 10. Tokens listos para usar (CSS)
```css
:root { --color-primary: #...; }
```

## Notas
- ...
```

Si un dato no se pudo verificar, escribe `No verificado en [pagina]` en vez de inventarlo.

## 6. Reglas

- No inventes HEX: sin evidencia escribe `No verificado`.
- Cita de que pagina salio cada patron (`/login`, `/pricing`).
- Archivo final: solo el `design_[dominio].md`, nada mas.
- Paralelo solo por paginas, prohibido por componentes.
- Con MCP browser trabaja secuencial (max 2 subagentes coordinados, riesgo de pisar pestanas).
```

## Notas de uso

- En este repo el archivo final `design_[dominio].md` de cada web va como nota normal en su carpeta temática, no junto a esta skill.
- Sin MCP browser el nivel Simple falla en sitios 100% JS (dashboards, SPA con login). En ese caso recomienda reintentar en PC con MCP.
- Paralelo opt-in solo por paginas (nunca por componentes) y solo en Completo/webfetch sin MCP browser; Simple y MCP van en secuencial.

## Ver también

* [OpenCode - AGENTS.md vs skill vs agent vs MCP vs command](opencode-agents-skills-mcp-commands.md)
* [Refero Styles y DESIGN.md](../buenas-practicas/refero-styles-design-md.md)
* [IA (MOC)](_index.md)
