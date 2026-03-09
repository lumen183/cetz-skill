# CeTZ 0.4.2 API Cheatsheet

## 1. 导入方式 (Importing)

**正确方式 (Recommended):**
```typst
#import "@preview/cetz:0.4.2": canvas, draw
```

**导入特定子模块:**
```typst
#import "@preview/cetz:0.4.2": canvas, draw, palette
#import "@preview/cetz:0.4.2": chart, plot, angle // 某些高级功能需显式导入
```

---

## 2. 基本绘图元素 (Basic Elements)

所有绘图命令必须在 `canvas` 函数的闭包内，并通过 `draw` 调用。

| 命令 | 语法示例 | 说明 |
| :--- | :--- | :--- |
| `line` | `draw.line((0,0), (1,1), stroke: red)` | 线段 |
| `circle` | `draw.circle((0,0), radius: 1, fill: blue)` | 圆形 |
| `rect` | `draw.rect((0,0), (2,2), stroke: 2pt)` | 矩形 |
| `content` | `draw.content((0,0), [Hello])` | 渲染 Typst 内容 |
| `bezier` | `draw.bezier((0,0), (2,0), (1,1))` | 贝塞尔曲线 (起点, 终点, 控制点) |

---

## 3. 坐标系统与锚点 (Coordinates & Anchors)

- **绝对坐标:** `(x, y)` 或 `(x, y, z)` - 如 `(0, 0)`
- **相对坐标:**
    - `(rel: (dx, dy))` - 相对于上一个点移动。
    - `(rel: (dx, dy), to: (px, py))` - 相对于指定点 `to` 移动。
- **极坐标:** `(angle: 45deg, radius: 2)`
- **锚点引用 (Named Elements):**
    - 首先定义名称: `draw.rect((0,0), (2,2), name: "box")`
    - 引用锚点: `"box.top"`, `"box.bottom-right"`, `"box.center"`
    - 常见锚点: `.north`, `.south`, `.east`, `.west`, `.center`, `.top-left` 等。

## 4. 样式指南 (Styling Guide)

所有绘图函数均接受以下样式参数：

| 属性 | 语法示例 | 说明 |
| :--- | :--- | :--- |
| **Stroke** | `stroke: 1pt` | 线宽 |
| | `stroke: red` | 颜色 |
| | `stroke: (paint: blue, dash: "dashed")` | 虚线 ("dashed", "dotted") |
| **Fill** | `fill: red` | 填充色 |
| | `fill: blue.lighten(50%)` | 半透明填充 |
| **Mark** | `mark: (end: ">")` | 箭头 |
| | `mark: (start: "o", end: "stealth")` | 双向标记 |

---

## 5. 常见 AI 错误与坑 (Pitfalls & Warnings)

### ⚠️ Mark 语法
**错误:** `mark: ">"` 或 `mark: "stealth"`
**正确:** 必须使用字典格式。
```typst
draw.line((0,0), (1,0), mark: (end: ">"))
draw.line((0,0), (1,0), mark: (start: "stealth", end: "latex", fill: red))
```

### ⚠️ 填充颜色 (Fill Color)
CeTZ 的 `fill` 属性通常需要明确的颜色。如果想要透明度，建议使用 `.lighten()`。
```typst
draw.circle((0,0), fill: blue.lighten(80%))
```

### ⚠️ 子模块导入
`cetz.angle` 等复杂模块不会在 `draw` 中自动导出。
```typst
#import "@preview/cetz:0.4.2": angle
// 使用 angle.angle(...) 而非 draw.angle(...)
```

### ⚠️ Stroke 样式 (Stroke Style)
**问题:** 复杂的 stroke 字典可能导致 "Failed to resolve coordinate system" 错误

| 场景 | 正确语法 | 错误语法 |
| :--- | :--- | :--- |
| 简单线条 | `stroke: 0.8pt` | - |
| 彩色线条 | `stroke: red` | - |
| 带粗细颜色 | `stroke: (paint: red, thickness: 2pt)` | 仅在简单语法失败时使用 |

```typst
// ✅ 正确 - 简单语法 (推荐)
draw.line((0,0), (1,1), stroke: 0.8pt)
draw.line((0,0), (1,1), stroke: red)

// ❌ 错误 - 复杂字典可能导致错误
let style = (stroke: (paint: red, thickness: 1pt), mark: (end: ">"))
draw.line((0,0), (1,1), style)  // ERROR!
```

### ⚠️ "Failed to resolve coordinate system" 错误排查
1. 检查 stroke 样式字典是否格式错误
2. 尝试简化为 `stroke: 0.8pt`
3. 确保使用了 `draw.` 前缀
4. 检查是否有悬空的逗号或括号

### ⚠️ 总是使用 draw. 前缀 (推荐)
为避免名称冲突和奇怪的错误,建议始终使用 `draw.` 前缀:
```typst
// ✅ 推荐
draw.line(...)
draw.rect(...)
draw.content(...)
draw.circle(...)

// ⚠️ 可能出错 (取决于导入方式)
line(...)
rect(...)
content(...)
```

---

## 6. 常用标记类型表 (Mark Types)

| 名称 | 预览描述 |
| :--- | :--- |
| `">"` | 标准箭头 |
| `"|"` | 垂直线 |
| `"stealth"` | 隐身/流线型箭头 |
| `"latex"` | 类 LaTeX 箭头 |
| `"circle"` | 圆点 |

---

## 7. CeTZ-Plot 入门 (Plotting)

绘图需要使用 `plot.plot` 环境。

```typst
#import "@preview/cetz:0.4.2": canvas, plot

#canvas({
  plot.plot(size: (4, 4),
    x-tick-step: 1,
    y-tick-step: 1,
    {
      plot.add(domain: (-2, 2), x => calc.pow(x, 2))
    })
})
```

---

## 8. 布局与变换 (Layout & Groups)

使用 `draw.group` 封装局部坐标系，方便模块化绘图。

```typst
draw.group({
  draw.scale(0.5)           // 仅影响组内
  draw.rotate(30deg)        // 仅影响组内
  draw.translate((2, 0))    // 移动组的原点
  
  // 在局部坐标系绘图
  draw.rect((-1, -1), (1, 1), stroke: blue)
})
```

---

## 9. 连线技巧 (Connecting Nodes)

绘制流程图时，利用锚点自动连接最方便。

```typst
// 1. 定义节点
draw.content((0, 0), [Start], name: "start")
draw.content((0, -2), [Process], name: "proc")

// 2. 自动连接边缘
draw.line("start.south", "proc.north", mark: (end: ">"))

// 3. 复杂路径连接
draw.line("start.east", (rel: (1,0)), "proc.east", mark: (end: ">"))
```

---

## 10. 数学函数库 (simple-plot)

基于 CeTZ 0.4.2 开发，简化了数学函数的绘制流程。

**导入方式:**
```typst
#import "@preview/simple-plot:0.3.0": plot
```

**关键语法示例:**
```typst
#import "@preview/cetz:0.4.2": canvas
#import "@preview/simple-plot:0.3.0": plot

#canvas({
  plot.plot(
    data: ((0, 0), (1, 1), (2, 4)),
    size: (6, 4),
    x-label: [Time],
    y-label: [Value],
    title: [Simple Plot Example]
  )
})
```
