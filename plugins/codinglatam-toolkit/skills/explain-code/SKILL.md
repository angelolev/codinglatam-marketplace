---
name: explain-code
description: This skill should be used when the user asks to "explain this code", "what does this code do", "how does this work", "explain the logic", mentions "explain", "understand code", or wants a breakdown of any code snippet, function, or file.
---

# Explain Code

Explica cualquier fragmento de codigo en 3 niveles de profundidad para facilitar la comprension.

## When This Skill Applies

This skill activates when the user:
- Asks to explain code, a function, a file, or a code snippet
- Wants to understand how something works
- Asks "what does this do?" about any piece of code
- Needs a breakdown of logic or flow

## Process

When asked to explain code, follow these 3 levels:

### Level 1: One-Line Summary

Provide a single sentence that captures what the code does at the highest level. Be concise and clear.

**Format:**
> **Resumen:** [One sentence describing the purpose]

### Level 2: Step-by-Step Breakdown

Walk through the code line by line or block by block, explaining what each part does. Use numbered steps.

**Format:**
**Paso a paso:**
1. [First block/line explanation]
2. [Second block/line explanation]
3. ...

Include:
- What each variable holds and why
- Control flow decisions (if/else, loops)
- Function calls and their purpose
- Return values and side effects

### Level 3: Real-World Analogy

Create an analogy from everyday life that maps to the code's behavior. This helps cement understanding for visual/conceptual learners.

**Format:**
> **Analogia:** Imagina que [real-world scenario that mirrors the code's logic]...

## Guidelines

- Use Spanish for explanations (the team's primary language) unless the user writes in English
- Adapt the technical depth to the code's complexity
- For simple code (< 10 lines), keep Level 2 brief
- For complex code, add sub-steps in Level 2
- Make analogies relatable: use cooking, traffic, mail delivery, libraries, or other everyday concepts
- If the code has bugs or anti-patterns, mention them after the 3 levels as a bonus note
- Reference specific line numbers when explaining

## Example Output

For a debounce function:

> **Resumen:** Esta funcion retrasa la ejecucion de otra funcion hasta que el usuario deje de llamarla por un tiempo determinado.

**Paso a paso:**
1. `debounce(fn, delay)` recibe la funcion a ejecutar y el tiempo de espera en ms
2. Declara `timer` para guardar referencia al timeout activo
3. Retorna una nueva funcion que cada vez que se llama...
4. Limpia el timeout anterior con `clearTimeout(timer)`
5. Crea un nuevo timeout que ejecutara `fn` despues de `delay` ms

> **Analogia:** Imagina un ascensor: cada vez que alguien presiona el boton, el ascensor reinicia su cuenta regresiva para cerrar las puertas. Solo cuando nadie presiona por suficiente tiempo, las puertas finalmente se cierran y el ascensor se mueve.
