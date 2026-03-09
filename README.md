# CeTZ Skill for Claude Code

A comprehensive skill for high-quality scientific diagramming using [CeTZ](https://github.com/johnny-dev14/cetz) 0.4.2 in [Typst](https://typst.app).

## Installation

One-line install:

```bash
npx skills add https://github.com/yourusername/claude-skill-cetz
```

Manual install:

```bash
git clone https://github.com/yourusername/claude-skill-cetz.git ~/.claude/skills/cetz
```

## Quick Start

```typst
#import "@preview/cetz:0.4.2"

#cetz.canvas({
  import cetz.draw: *

  circle((0,0), radius: 1, fill: blue.lighten(80%), name: "c")
  line("c.east", (2,0), mark: (end: ">"))
  content((1, 0.5), [Hello CeTZ!])
})
```

Compile:

```bash
typst compile /path/to/your/file.typ
```

## Contents

| File / Folder           | Description                                      |
| ---------------------- | ------------------------------------------------ |
| `SKILL.md`             | Main entry point with quick reference            |
| `references/`          | Reference documentation                          |
| `api-cheatsheet.md`    | CeTZ 0.4.2 API cheatsheet with common pitfalls   |
| `assets/`              | Runnable diagram examples                        |

### Diagram Examples (in `assets/`)

| Category | Diagram                        |
| -------- | ----------------------------- |
| ML       | Self-Attention, Autoencoder, 2D Convolution, VAE |
| QM       | Bloch Sphere, Feynman Diagram |
| STAT     | Maxwell-Boltzmann, Bose-Einstein, Statistical Energy |
| CHEM     | Periodic Table                 |
| COMMON   | Heatmap, K-Nearest Neighbors  |

## Usage

Once installed, Claude Code will automatically activate this skill when:

- Working with `.typ` files using CeTZ
- User mentions "cetz", "diagram", or related terms
- Creating or modifying scientific diagrams in Typst documents

## Examples

```bash
# Compile included examples
typst compile ~/.claude/skills/cetz/assets/ml-self-attention.typ
typst compile ~/.claude/skills/cetz/assets/qm-bloch-sphere.typ
typst compile ~/.claude/skills/cetz/assets/common-heatmap.typ
```

## Key Features

- **CeTZ 0.4.2** compatible syntax
- **Common pitfalls** documented (stroke style, mark syntax, import patterns)
- **Scientific diagrams**: ML architectures, quantum mechanics, statistics, chemistry
- **API reference** with detailed syntax examples

## Requirements

- [Typst CLI](https://typst.app) installed

## License

MIT

## Credits

- **CeTZ**: [johnny-dev14/cetz](https://github.com/johnny-dev14/cetz) (LGPL-3.0)
- **Diagram Examples**: Based on [janosh/diagrams](https://github.com/janosh/diagrams) (MIT License)
