# spiroField
模糊边界可视化从属关系的词云工具

这是一个基于 **Vue 3** 和 **D3.js** 构建的高性能、语义化标签云排版引擎。它结合了地理空间算法与传统的词云布局技术，旨在实现具有层级关系的结构化数据展示。

## 核心架构与技术栈

- **前端框架**：Vue 3 (Composition API) + Vite，提供响应式的用户界面和高效的开发体验。
- **渲染与计算引擎**：D3.js (v7)，负责所有的数学运算、坐标转换、Voronoi 图计算以及 SVG/Canvas 的矢量绘图。
- **状态管理**：Pinia，管理全局配置、POI 数据及实验状态。
- **UI 组件库**：Element Plus，用于构建交互式控制面板、文件上传及加载进度展示。
- **工程化工具**：使用 ESLint/Oxlint 进行代码规范约束，Vitest 进行逻辑模块的单元测试。

## 理论核心与算法实现

该项目的排版逻辑超越了简单的螺旋分布，引入了以下核心理论来实现**受限空间下的多中心层级分布**：

1. **阿基米德螺线 (Archimedean Spiral)**
   - **原理**：标签以其语义类别的中心点为原点，沿 $r = a + b\theta$ 的轨道进行探测。
   - **应用**：确保高权重的标签（如核心 POI）能够优先占据中心区域，次要标签依次向外扩散。

2. **距离场 (Signed Distance Field, SDF)**
   - **实现**：通过 `tagCloudTool.js` 实现栅格化区域的距离计算，得到每个像素点到边界的最短距离。
   - **应用**：系统利用 SDF 寻找“最空旷”的区域作为候选放置点，从而在复杂多边形区域内实现均衡的标签分布，有效防止边界挤压。

3. **Voronoi 图与语义分区**
   - **原理**：利用 `d3-delaunay` 计算多中心点的沃罗诺伊划分。
   - **应用**：将画布根据数据类别划分为不同的“领地”（扇区），使同类标签在视觉上具有明显的空间聚集性。

4. **高性能碰撞检测 (QuadTree)**
   - **实现**：利用四叉树（QuadTree）优化矩形和位图的重叠检查。
   - **应用**：由 `overlaps` 函数调用，确保在成百上千个标签高速布局时，依然能保持像素级的排斥精度。

## 工作流程

1. **解析层**：通过 SheetJS 解析 Excel/JSON 等格式的数据，提取层级（Domain -> Type -> Member）及权重。
2. **布局层**：结合 SDF 与 Voronoi 计算每个类别的布局范围，并确定各标签的初始坐标。
3. **渲染层**：D3.js 根据计算结果在 SVG 上进行分层渲染，并动态调整标签的大小、透明度及颜色映射。

---

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
