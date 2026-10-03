# design-extractor

Skill de agente que extrae la guía de diseño de una web desde su URL y genera un `design_[dominio].md`.

## Qué hace

1. Normaliza la URL (`https://www.ejemplo.com/login` → `ejemplo.com`) y define la salida `design_ejemplo_com.md`.
2. Detecta capacidades: si hay MCP de navegador lo usa; si no, pregunta nivel `Simple` (solo fetch + observación) o `Completo` (descarga HTML/CSS y parsea HEX, `font-family`, clases).
3. Recorre el sitio completo: `/`, `/login`, `/signup`, `/pricing`, `/contact` y 1 página de contenido (`/blog`, `/docs`, `/about`).
4. Extrae paleta en HEX, tipografías, botones, formularios, login/signup, navegación, espaciado, iconografía y responsive.
5. Genera solo el `design_[dominio].md` con plantilla fija + tokens CSS. Lo no verificable se marca `No verificado`, nunca se inventa.

Paralelo solo por páginas (máx. 3 subagentes, solo en modo fetch sin MCP). Con MCP de navegador trabaja secuencial.

## Instalación

```bash
npx skills add DevJuanP/skills --skill design-extractor
```

## Uso

Pídele a tu agente, por ejemplo:

- «extrae el diseño de https://ejemplo.com»
- «saca los estilos de ejemplo.com»
- «dame la guía de diseño de https://ejemplo.com/pricing»

Te preguntará nivel `Simple` o `Completo` si no tienes MCP de navegador.

## Licencia

MIT — © 2026 DevJuanP. Ver `LICENSE` en la raíz del repo.
