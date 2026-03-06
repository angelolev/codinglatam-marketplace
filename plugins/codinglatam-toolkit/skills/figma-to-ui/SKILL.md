---
name: figma-to-ui
description: Genera componentes React + TypeScript + CSS Modules a partir de una URL de Figma. Usa el MCP de Figma para obtener el diseno y produce ComponentName.tsx + ComponentName.module.css. Usala siempre que el usuario comparta una URL de Figma y pida implementar, crear, codificar o traducir ese diseno a codigo.
---

# Figma to UI

Convierte disenos de Figma en componentes React + TypeScript + CSS Modules listos para produccion.

## When This Skill Applies

This skill activates when the user:
- Shares a Figma URL (contains `figma.com/design/` or `figma.com/file/`)
- Asks to "implement this design", "convert this to code", "create this component from Figma"
- Says "figma to code", "figma to UI", "figma to react"
- Wants to translate a visual design into frontend components

## Prerequisites

This skill requires a Figma MCP server configured to fetch design data. If no Figma MCP is available, instruct the user to add one to their `.mcp.json`:

```json
{
  "figma": {
    "command": "npx",
    "args": ["-y", "figma-developer-mcp", "--figma-api-key=<FIGMA_TOKEN>"]
  }
}
```

The user needs a Figma Personal Access Token from https://www.figma.com/developers/api#access-tokens

## Process

### Step 1: Extract Figma Data

1. Parse the Figma URL to get the file key and node ID
   - URL format: `https://www.figma.com/design/<fileKey>/<fileName>?node-id=<nodeId>`
   - Also supports: `https://www.figma.com/file/<fileKey>/...`
2. Use the Figma MCP tools to fetch the design data:
   - Get the file/node structure
   - Extract component properties (colors, typography, spacing, dimensions)
   - Identify the component hierarchy (parent/child relationships)
3. If a screenshot tool is available, take a screenshot of the Figma design for visual reference

### Step 2: Analyze the Design

Before writing code, analyze and document:

- **Component tree**: Identify each distinct component and sub-component
- **Layout model**: Determine if the design uses flex (row/column), grid, or absolute positioning
- **Design tokens**: Extract colors, font sizes, font weights, border radius, shadows, spacing
- **States**: Identify hover, active, disabled, or other interactive states if visible
- **Responsive hints**: Note if the design suggests breakpoints or fluid behavior
- **Assets**: Identify icons, images, or illustrations that need to be handled

Present a brief summary to the user:

> **Componentes detectados:** [list]
> **Layout:** [flex/grid/etc]
> **Tokens principales:** [key colors, fonts, spacing]

Ask the user to confirm before generating code, or proceed if they asked for a direct conversion.

### Step 3: Generate Components

For each component identified, generate two files:

#### `ComponentName.tsx`

```tsx
import React from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  // Props derived from Figma component properties or variants
}

export const ComponentName: React.FC<ComponentNameProps> = ({ ...props }) => {
  return (
    <div className={styles.container}>
      {/* Structure matching Figma layer hierarchy */}
    </div>
  );
};
```

#### `ComponentName.module.css`

```css
.container {
  /* Styles extracted from Figma */
  display: flex;
  /* Use exact values from the design */
}
```

### Step 4: Review and Refine

After generating the code:
1. Compare the generated structure against the Figma design
2. If a browser MCP is available, offer to render and screenshot the component for visual comparison
3. List any design details that couldn't be automatically translated (animations, complex interactions, custom assets)

## Code Conventions

Follow these rules strictly when generating components:

### File Structure
- One component per file pair (`.tsx` + `.module.css`)
- Use PascalCase for component names: `ProductCard`, `HeroSection`
- Place files in the appropriate directory based on the project structure

### TypeScript
- Use `React.FC<Props>` for component typing
- Define explicit interfaces for all props
- Use descriptive prop names that match Figma layer or property names when possible
- Export components as named exports (not default)

### CSS Modules
- Use camelCase for class names: `.cardTitle`, `.priceContainer`
- Map Figma styles directly:
  - Figma "Auto layout" -> `display: flex` with matching `flex-direction`, `gap`, `padding`
  - Figma "Fill container" -> `flex: 1` or `width: 100%`
  - Figma "Fixed width/height" -> exact `px` values
  - Figma "Hug contents" -> no explicit width/height (intrinsic sizing)
- Use CSS custom properties (`var(--color-primary)`) for design tokens when the project has a token system
- Otherwise, use exact hex/rgb values from Figma
- Avoid `!important`

### Figma-to-CSS Mapping Reference

| Figma Property | CSS Equivalent |
|---|---|
| Auto layout (horizontal) | `display: flex; flex-direction: row;` |
| Auto layout (vertical) | `display: flex; flex-direction: column;` |
| Gap (item spacing) | `gap: Xpx;` |
| Padding | `padding: top right bottom left;` |
| Fill container | `flex: 1;` or `width: 100%;` |
| Hug contents | intrinsic (no width/height) |
| Fixed size | `width: Xpx; height: Ypx;` |
| Corner radius | `border-radius: Xpx;` |
| Drop shadow | `box-shadow: Xpx Ypx Zpx rgba(...);` |
| Background blur | `backdrop-filter: blur(Xpx);` |
| Opacity | `opacity: 0.X;` |
| Stroke | `border: Xpx solid color;` |
| Clip content | `overflow: hidden;` |

### Images and Icons
- For icons: use inline SVG or an icon component system if the project has one
- For images: use `<img>` with descriptive `alt` text; use placeholder `src` with a TODO comment if the actual asset is not available
- For decorative backgrounds: use CSS `background-image` or gradients

### Accessibility
- Use semantic HTML: `<button>`, `<nav>`, `<header>`, `<main>`, `<section>`, `<article>`
- Add `alt` text to images based on Figma layer names or context
- Ensure interactive elements are focusable and have appropriate ARIA attributes

## Example Output

For a Figma card component:

> **Componentes detectados:** ProductCard, PriceTag, RatingStars
> **Layout:** Flex vertical
> **Tokens principales:** #1A1A2E (text), #E94560 (accent), 16px border-radius, Inter font

**ProductCard.tsx:**
```tsx
import React from 'react';
import styles from './ProductCard.module.css';

interface ProductCardProps {
  imageUrl: string;
  title: string;
  price: number;
  rating: number;
}

export const ProductCard: React.FC<ProductCardProps> = ({
  imageUrl,
  title,
  price,
  rating,
}) => {
  return (
    <article className={styles.card}>
      <img className={styles.image} src={imageUrl} alt={title} />
      <div className={styles.content}>
        <h3 className={styles.title}>{title}</h3>
        <span className={styles.price}>${price.toFixed(2)}</span>
      </div>
    </article>
  );
};
```

**ProductCard.module.css:**
```css
.card {
  display: flex;
  flex-direction: column;
  width: 280px;
  border-radius: 16px;
  overflow: hidden;
  background: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 16px;
}

.title {
  font-family: 'Inter', sans-serif;
  font-size: 16px;
  font-weight: 600;
  color: #1a1a2e;
  margin: 0;
}

.price {
  font-family: 'Inter', sans-serif;
  font-size: 18px;
  font-weight: 700;
  color: #e94560;
}
```

## Guidelines

- Use Spanish for communication with the user unless they write in English
- Always show the design analysis summary before generating code
- Generate pixel-perfect CSS from Figma values — do not approximate
- If the design has variants (hover, active, etc.), generate all states
- If the Figma component has nested components, generate them as separate files and compose them
- Prefer composition over monolithic components: if a section has 3+ distinct parts, split them
- Do not invent functionality not present in the design (no fake click handlers, no mock data beyond what Figma shows)
