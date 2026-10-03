---
name: design-extractor
description: Extracts the design guide of a website from its URL and generates design_[domain].md with palette, typography, buttons, forms and login. Use when user says extrae el diseño de, saca los estilos de, dame la guía de diseño de, design from this site or needs design tokens from a URL.
license: MIT
metadata:
  author: Juan Pajuelo (DevJuanP)
  repository: https://github.com/DevJuanP/skills
  version: "0.1.0"
---

# Design Extractor

Given a URL, explore the site and generate `design_[domain].md` with its design guide.

## Instructions

1. **Normalize input:** accept any URL (`https://ejemplo.com/ruta` or `ejemplo.com`). Extract the clean domain: lowercase, no `www.`, no port, no path. Example: `https://www.ejemplo.com/login` -> `ejemplo.com`. Output name: `design_ejemplo_com.md` (dots to underscores). Save in the working directory unless the user says otherwise.

2. **Detect capabilities (adaptive):** check if a browser-automation MCP is available (`chrome-devtools`, `playwright`, `browser`, `puppeteer`). If yes, use it (real navigation, computed styles, screenshots). If not, ask the user before continuing:
   - `Simple (Recommended)`: fetch rendered pages + observation only. No dependencies. Approximate palette, main components.
   - `Completo`: download HTML/CSS (`curl`/Python), parse HEX, `font-family`, button/input classes. Requires internet and shell.
   Do not proceed without knowing the level.

3. **Crawl the full site:** visit at minimum (skip with justification on 404):
   - `/` (home)
   - `/login`, `/signin`, `/signup`, `/register`
   - `/pricing`, `/planes`, `/contact`, `/contacto`
   - 1 content page (`/blog`, `/docs`, `/about`)
   - any visible form (search, newsletter, checkout)
   Log visited URL + status. On failure, try 1 alternative and move on.

4. **Parallel only by pages (opt-in):** default is sequential. Use subagents only when there are 4+ pages to visit AND there is no browser MCP (fetch-only mode). Fan-out by pages, never by components (splitting buttons vs forms duplicates downloads and produces inconsistent HEX). Example: A: `/ + /pricing`, B: `/login + /signup`, C: `/blog|/docs|/about + /contact`. Max 3 subagents. Each subagent returns raw evidence (URL, status, HEX + selector, font-family, classes) and never writes the `.md`.

5. **Extract (what to look at on each page):**
   - **Palette:** primary, secondary, accent, background, text, states (hover/disabled/error). Always HEX (`#RRGGBB`). If Simple, estimate from visible output and mark `~aprox`.
   - **Typography:** families, h1-h6/body sizes, weights, line-height.
   - **Buttons:** primary/secondary/ghost/danger, radius, padding, states.
   - **Forms:** inputs, labels, placeholders, validation, focus ring.
   - **Login/signup:** layout (centered/split), social OAuth, remember/forgot password.
   - **Navigation:** header, footer, mobile menu.
   - **Spacing/layout:** container max-width, grid, base radius, shadows.
   - **Iconography and images:** style (outline/solid), photo borders.
   - **Responsive and accessibility:** breakpoints, evident contrast, visible focus.
   In Completo or MCP mode, back each color with a cited CSS selector or computed style.

6. **Generate `design_[domain].md`:** single-writer merge. If there was fan-out, the main agent deduplicates first (same HEX with different name -> pick 1 and cite pages), then generates the file. Use exactly the template in `Output Format`, no new sections except `Notas`. If a datum could not be verified, write `No verificado en [pagina]` instead of inventing it.

7. **Report** in 2 lines: generated file path + pages visited and level (Simple/Completo/MCP).

## Output Format

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

## Safety and Limitations

- ALWAYS cite which page each pattern came from (`/login`, `/pricing`).
- NEVER invent HEX: without evidence write `No verificado`.
- Final output is only the `design_[domain].md` file, nothing else.
- With browser MCP work sequentially (max 2 coordinated subagents, risk of clobbering tabs).
- This skill cannot render 100% JS sites (dashboards, SPA behind login) without a browser MCP; in that case recommend retrying where one is available.

---

© 2026 Juan Pajuelo (DevJuanP). Licencia MIT. Repo canónico: https://github.com/DevJuanP/skills.
