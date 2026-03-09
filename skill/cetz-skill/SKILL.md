---
name: cetz-skill
description: High-quality scientific diagramming for Typst using CeTZ 0.4.2
---

# CeTZ Skill

CeTZ (CeTZ, ein Typst Zeichenpaket) is a powerful library for drawing with [Typst](https://typst.app). This skill focuses on high-quality scientific diagramming using CeTZ 0.4.2.


## 工作流程 (Workflow)

为了确保生成高质量的图表，请遵循以下步骤：

1.  **分析与规划 (Analyze & Plan)**
    *   理解用户需求：是流程图、科学示意图、电路图还是数据图表？
    *   规划布局：确定核心元素的相对位置（如 "Process A 在 (0,0)，Process B 在 (2,0)"）。
    *   选择模板：参考 "案例目录" 中最接近的示例代码。

2.  **编写与生成 (Draft Code)**
    *   使用 `cetz` 0.4.2 语法编写 `.typ` 文件。
    *   **关键**：优先使用 `import "@preview/cetz:0.4.2": canvas, draw` 导入方式以确保稳定性。
    *   对于复杂图形，先画骨架（nodes），再画连接（edges）。

3.  **编译与验证 (Compile & Verify)**
    *   运行 Typst 编译命令（如 `typst compile main.typ`）。
    *   检查输出：
        *   ❌ **编译错误**：检查是否混用了不兼容的语法（参考 "常见错误"）。
        *   ❌ **布局错乱**：检查坐标计算或 anchor 使用是否正确。
        *   ✅ **成功**：进入下一步。

4.  **优化与迭代 (Refine)**
    *   **样式调整**：调整颜色、线条粗细、字体大小以符合科学出版标准。
    *   **自我修正**：如果发现元素重叠，调整坐标间距。

5.  **最终交付 (Finalize)**
    *   输出最终的 `.typ` 代码块。
    *   确保代码可直接复制运行，不包含未定义的变量。



## 快速入门 (Quick Start)

To use CeTZ in a Typst document, import the library and use the `canvas` function:

```typ
#import "@preview/cetz:0.4.2"

#cetz.canvas({
  import cetz.draw: *
  
  // Draw a simple circle
  circle((0,0), radius: 1, fill: blue.lighten(80%), name: "c")
  
  // Draw a line with a marker relative to the circle
  line("c.east", (2,0), mark: (end: ">"))
  
  // Add some text
  content((1, 0.5), [Hello CeTZ!])
})
```

## 工作流程 (Workflow - CRITICAL!)

遵循此流程可大幅提高一次性生成的成功率：

1.  **分析与规划 (Analyze & Plan)**
    *   **布局**: 确定是层级结构（自上而下）、中心辐射还是网格布局。
    *   **坐标系**: 对于复杂图，先在草稿纸（或脑海中）建立 $(0,0)$ 原点。
    *   **组件化**: 将重复的结构（如神经元节点）定义为 Typst 函数。

2.  **编写骨架 (Draft Structure)**
    *   先画节点 (Nodes)，暂时忽略连线。
    *   使用 `draw.content` 或 `draw.rect` 放置核心元素。
    *   *Tip*: 给每个关键元素设置 `name: "id"`，方便后续引用。

3.  **连接与修饰 (Connect & Style)**
    *   使用锚点连接节点：`draw.line("nodeA.east", "nodeB.west")`。
    *   添加标记 (Marks)：箭头、刻度。
    *   应用样式：统一颜色、线宽。

4.  **编译与验证 (Compile & Verify)**
    *   检查是否有重叠。
    *   检查箭头方向是否正确。

5.  **优化 (Refine)**
    *   调整 `canvas(length: ...)` 缩放整体大小。
    *   微调坐标以获得完美的对齐。

## 正确语法速查 (CRITICAL!)

| Feature | Correct Syntax (0.4.2) | Note |
| :--- | :--- | :--- |
| **Import** | `#import "@preview/cetz:0.4.2"` | Always use 0.4.2 for stability |
| **Canvas** | `#cetz.canvas({ ... })` | Drawing logic goes inside the block |
| **Coordinate** | `(x, y)` or `(x, y, z)` | Cartesian coordinates |
| **Relative** | `(rel: (1, 0))` | Relative to the previous point |
| **Anchors** | `"name.anchor"` | e.g., `"rect.north-east"` |
| **Styling** | `stroke: (paint: blue, thickness: 1pt)` | Detailed stroke control |
## 常见错误与修复 (Common Pitfalls - CRITICAL!)

### ❌ WRONG - Causes "Failed to resolve coordinate system"
```typ
// Complex stroke dictionary often fails
let arrow-style = (stroke: (paint: red, thickness: 1pt), mark: (end: ">"))
line("a.east", "b.west", arrow-style)  // ERROR!

// Missing draw. prefix (sometimes works, sometimes doesn't)
rect((0,0), (1,1))  // May fail
content((0,0), [Text])  // May fail
```

### ✅ CORRECT - Working Patterns
```typ
// Simple stroke - WORKS
draw.line("a.east", "b.west", stroke: 0.8pt)

// Named imports + draw. prefix - ALWAYS WORKS
#import "@preview/cetz:0.4.2": canvas, draw

draw.rect((0,0), (1,1), fill: blue, stroke: red)
draw.content((0,0), [Text], anchor: "center")
draw.circle((0,0), radius: 1)
draw.line((0,0), (1,1))
```

### 🔧 Import Patterns (Choose One)

**Pattern A: Full import (recommended for beginners)**
```typ
#import "@preview/cetz:0.4.2": *

canvas({
  content((0,0), [Works!])
  rect(...)  
})
```

**Pattern B: Named imports (recommended for reliability)**
```typ
#import "@preview/cetz:0.4.2": canvas, draw

canvas({
  draw.content((0,0), [More explicit, safer])
  draw.rect(...)
})
```

**Pattern C: Mixed (what examples often show)**
```typ
#import "@preview/cetz:0.4.2": canvas
#import "@preview/cetz:0.4.2": draw
#import draw: content, line, rect

canvas({
  content((0,0), [Works])  // from imported content
  draw.rect(...)            // from draw module
})
```

### 📋 Stroke Style Guide

| Style Need | Correct Syntax | Example |
| :--- | :--- | :--- |
| Simple line | `stroke: 0.8pt` | `draw.line((0,0), (1,1), stroke: 0.8pt)` |
| Colored line | `stroke: red` | `draw.line((0,0), (1,1), stroke: red)` |
| Custom thickness | `stroke: (paint: red, thickness: 2pt)` | Use only when simple version fails |
| With arrow marker | Use separate mark parameter | See examples |

### 🐛 Debugging Tips

1. **If you get "Failed to resolve coordinate system":**
   - Check if stroke style dictionary is malformed
   - Try simplifying to `stroke: 0.8pt`
   - Make sure you're using `draw.` prefix

2. **If content/rect/line not found:**
   - Add `#import draw: content, line, rect`
   - Or use `draw.content()`, `draw.rect()`

3. **If canvas not found:**
   - Use `#import "@preview/cetz:0.4.2": canvas`

## 案例目录 (Categorized)

All examples are sourced from [janosh/diagrams](https://github.com/janosh/diagrams) (MIT License).

### Machine Learning (ML)
- [Self-Attention Mechanism](../../assets/ml-self-attention.typ)
- [Autoencoder Architecture](../../assets/ml-autoencoder.typ)
- [2D Convolution Operation](../../assets/ml-2d-convolution.typ)
- [Variational Autoencoder (VAE)](../../assets/ml-variational-autoencoder.typ)

### Quantum Mechanics (QM)
- [Bloch Sphere](../../assets/qm-bloch-sphere.typ)
- [Feynman Diagram](../../assets/qm-feynman-diagram.typ)

### Statistics (STAT Figure)
- [Maxwell-Boltzmann Distribution](../../assets/stat-maxwell-boltzmann.typ)
- [Bose-Einstein Distribution](../../assets/stat-bose-einstein.typ)
- [Statistical Energy Distribution](../../assets/stat-statistical-energy.typ)


### Chemistry (CHEM)
- [Periodic Table Component](../../assets/chem-periodic-table.typ)

### Common Diagrams (COMMON)
- [Heatmap](../../assets/common-heatmap.typ)
- [K-Nearest Neighbors (KNN)](../../assets/common-knn.typ)

## 关键 API 参考 (API Reference)

For a detailed list of functions, coordinates, and styling options, see the [API Cheatsheet](../../references/api-cheatsheet.md).

## 版权声明 (Copyright & License)

- **CeTZ**: LGPL-3.0 License.
- **Diagram Examples**: Based on [janosh/diagrams](https://github.com/janosh/diagrams), MIT License.
- **Skill**: MIT License.
