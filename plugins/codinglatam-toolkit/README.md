# codinglatam-toolkit

Plugin para Claude Code del equipo CodingLatam. Incluye herramientas de desarrollo para el dia a dia.

## Componentes

### Skill: explain-code

Explica cualquier codigo en 3 niveles:
1. **Resumen** - Una linea con el proposito del codigo
2. **Paso a paso** - Desglose linea por linea
3. **Analogia** - Comparacion con algo del mundo real

Se activa automaticamente cuando pides explicar codigo. Ejemplos:
- "Explica esta funcion"
- "Que hace este codigo?"
- "Como funciona esto?"

### Skill: figma-to-ui

Convierte disenos de Figma en componentes React + TypeScript + CSS Modules.

1. **Extrae** datos del diseno via Figma MCP
2. **Analiza** la estructura: componentes, layout, tokens de diseno
3. **Genera** archivos `.tsx` + `.module.css` por componente
4. **Revisa** el resultado comparando con el diseno original

Se activa cuando compartes una URL de Figma. Ejemplos:
- "Implementa este diseno: https://figma.com/design/..."
- "Convierte este Figma a React"
- "Crea el componente de esta URL de Figma"

Requiere un MCP de Figma configurado. Ver instrucciones en la skill.

### Agent: refactor-coach

Analiza codigo buscando oportunidades de refactoring y las aplica una por una.

Detecta:
- Funciones largas (20+ lineas)
- Codigo duplicado
- Naming pobre
- Nesting profundo (3+ niveles)
- Magic numbers

Usa el agente con: "Usa el agente refactor-coach para analizar este archivo"

### MCP: chrome-devtools

Integra Chrome DevTools Protocol para interactuar con el navegador. Requiere Chrome o Edge corriendo con depuracion remota.

**Iniciar Chrome con depuracion:**
```bash
# Windows
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222

# Mac
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```

**Tools disponibles:** `take_screenshot`, `navigate_page`, `click`, `fill`, `evaluate_script`, `list_pages`, entre otras.

## Prerequisitos

- Claude Code instalado
- Node.js (para el MCP de chrome-devtools)
- Chrome o Edge (para usar chrome-devtools)
