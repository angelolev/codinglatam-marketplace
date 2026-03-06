# codinglatam-marketplace

Marketplace de plugins para Claude Code del equipo CodingLatam.

## Plugins disponibles

| Plugin | Descripcion |
|--------|-------------|
| **codinglatam-toolkit** | Skill de explicacion de codigo, agente de refactoring y MCP chrome-devtools |

## Instalacion

### 1. Agregar el marketplace

```
/plugin marketplace add tu-org/codinglatam-marketplace
```

Para probar localmente con la ruta del repo:
```
/plugin marketplace add D:/Proyectos/side-projects/codinglatam-marketplace
```

### 2. Instalar el plugin

```
/plugin install codinglatam-toolkit@codinglatam-marketplace
```

### 3. Verificar

```
/plugin list
```

## Prerequisitos

- **Claude Code** - CLI de Anthropic
- **Node.js** - Necesario para el MCP de chrome-devtools (`npx`)
- **Chrome o Edge** - Para usar las herramientas de chrome-devtools (iniciar con `--remote-debugging-port=9222`)

## Estructura

```
codinglatam-marketplace/
├── .claude-plugin/
│   └── marketplace.json           # Catalogo del marketplace
├── plugins/
│   └── codinglatam-toolkit/
│       ├── .claude-plugin/
│       │   └── plugin.json        # Metadata del plugin
│       ├── skills/
│       │   └── explain-code/
│       │       └── SKILL.md       # Skill: explica codigo en 3 niveles
│       ├── agents/
│       │   └── refactor-coach.md  # Agente: coach de refactoring
│       ├── .mcp.json              # MCP: chrome-devtools
│       └── README.md              # Docs del plugin
└── README.md                      # Este archivo
```

## Crear nuevos plugins

Para agregar un plugin al marketplace:

1. Crear carpeta en `plugins/tu-plugin/`
2. Agregar `.claude-plugin/plugin.json` con metadata
3. Agregar skills, agents, MCPs, hooks, o commands segun necesites
4. Registrar el plugin en `.claude-plugin/marketplace.json`
5. Commit y push

## Equipo

Mantenido por el equipo CodingLatam.
