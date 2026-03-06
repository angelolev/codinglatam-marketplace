---
name: dependency-docs
description: Analiza y documenta las dependencias de un proyecto. Se activa cuando el usuario pide "documentar dependencias", "que dependencias usa este proyecto", "analizar paquetes", "listar librerias", "dependency report", o quiere entender que dependencias tiene y por que.
---

# Dependency Docs

Analiza las dependencias de un proyecto y genera un reporte claro con proposito, uso y estado de cada una.

## When This Skill Applies

This skill activates when the user:
- Asks to document, list, or analyze project dependencies
- Wants to understand what packages/libraries the project uses and why
- Asks "what dependencies does this project have?"
- Wants a dependency audit or health check
- Asks about outdated, unused, or redundant dependencies

## Process

### Step 1: Detect Package Manager and Dependencies

Scan the project root and subdirectories for dependency manifests:

| File | Ecosystem |
|---|---|
| `package.json` | Node.js (npm/yarn/pnpm/bun) |
| `requirements.txt` / `pyproject.toml` / `Pipfile` | Python |
| `go.mod` | Go |
| `Cargo.toml` | Rust |
| `pom.xml` / `build.gradle` | Java/Kotlin |
| `Gemfile` | Ruby |
| `composer.json` | PHP |

If multiple manifests exist (e.g., monorepo), document each one separately.

### Step 2: Classify Dependencies

For each dependency, determine:

1. **Nombre** — nombre del paquete
2. **Version** — version instalada o rango definido
3. **Tipo** — produccion (`dependencies`) o desarrollo (`devDependencies`)
4. **Categoria** — clasificar segun su funcion:
   - Framework / Runtime
   - UI / Componentes
   - Estilos / CSS
   - State Management
   - Routing
   - HTTP / API Client
   - Base de datos / ORM
   - Autenticacion
   - Testing
   - Linting / Formatting
   - Build / Bundler
   - Tipos / TypeScript
   - Utilidades
5. **Proposito** — una linea explicando para que se usa en el proyecto (no la descripcion generica del paquete, sino su rol especifico en este proyecto)

### Step 3: Generate Report

Produce the report in the following format:

```markdown
# Dependencias del Proyecto

> Generado el [fecha]. Basado en [archivo(s) de dependencias encontrado(s)].

## Resumen

- **Total:** X dependencias (Y produccion, Z desarrollo)
- **Ecosistema:** [Node.js / Python / etc.]
- **Package Manager:** [npm / yarn / pnpm / pip / etc.]

## Dependencias de Produccion

### Framework / Runtime
| Paquete | Version | Proposito |
|---|---|---|
| next | ^14.0.0 | Framework principal, SSR y routing |

### UI / Componentes
| Paquete | Version | Proposito |
|---|---|---|
| @radix-ui/react-dialog | ^1.0.0 | Modales accesibles |

[...mas categorias...]

## Dependencias de Desarrollo

### Testing
| Paquete | Version | Proposito |
|---|---|---|
| vitest | ^1.0.0 | Runner de tests unitarios |

[...mas categorias...]
```

### Step 4: Analysis (Optional)

If the user asks for a deeper analysis, include these sections:

#### Dependencias potencialmente redundantes
Paquetes que hacen lo mismo o se solapan. Ejemplo: tener tanto `axios` como `node-fetch`.

#### Dependencias desactualizadas
Comparar versiones instaladas con las ultimas disponibles. Marcar con:
- **Al dia** — version actual o minor detras
- **Desactualizada** — major version detras
- **Deprecada** — paquete marcado como deprecated

Para verificar versiones, usar el comando apropiado:
- Node.js: `npm outdated` o `yarn outdated`
- Python: `pip list --outdated`

#### Dependencias sin uso aparente
Paquetes listados en el manifiesto pero que no se importan en el codigo fuente. Buscar imports/requires en el proyecto para verificar.

#### Seguridad
Si hay herramientas disponibles, correr:
- `npm audit` / `yarn audit`
- `pip-audit`
- `cargo audit`

Reportar vulnerabilidades encontradas con su severidad.

## Output Options

The user can request the report in different formats:
- **Markdown file** — genera un archivo `DEPENDENCIES.md` en la raiz del proyecto
- **Inline** — muestra el reporte directamente en la conversacion
- **JSON** — genera un `dependencies.json` estructurado

Default: mostrar inline en la conversacion. Solo crear archivo si el usuario lo pide.

## Guidelines

- Use Spanish for the report unless the user writes in English
- Always read the actual dependency files — never guess versions
- For the "Proposito" column, search the codebase to understand how each dependency is actually used, not just its npm/PyPI description
- Group by category to make the report scannable
- If a dependency's purpose is unclear after searching the code, mark it with "Uso no identificado — verificar si es necesaria"
- Keep the report concise — one line per dependency in the table
- For monorepos, clearly separate dependencies by workspace/package
