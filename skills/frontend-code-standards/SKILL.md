---
name: frontend-code-standards
description: Enforces frontend code generation standards for Vue 3, ES6+, and CSS (atomic or BEM). Use when writing or generating frontend code, Vue components, JavaScript/HTML/CSS, or when the user asks for frontend coding standards. Reference: https://github.com/beyondOurself/rules/blob/main/web.md
---

# 前端代码生成规范

开发前端代码（Vue、JS/TS、HTML、CSS）时，必须遵循本规范。完整细则与示例见 [reference.md](reference.md)。

## 核心原则

1. **数据处理**：解构赋值、展开运算符、数组方法（filter/map 等）
2. **现代语法**：ES6+，目标浏览器 Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
3. **Vue 组件化**：单一职责、可复用、Composition API 优先（`<script setup>`）
4. **简洁**：DRY、函数式写法、提前 return
5. **CSS 优先级**：若项目有 Tailwind/UnoCSS → 优先原子化 class；否则用 BEM + scoped
6. **行业参考**：[LoongZero 前端开发规范](https://doc.loongzero.com/get-started/)

## 使用前：判断 CSS 方案

- 查 `package.json` 是否含 `tailwindcss`、`unocss`
- 查根目录是否有 `tailwind.config.js`、`uno.config.js`
- **有** → 写样式用原子化 class（Tailwind/UnoCSS）
- **无** → 用 BEM + `<style scoped>`

## 快速检查清单

生成或修改前端代码时自检：

- [ ] 对象/数组用解构、`...`、`.filter`/`.map`，避免冗长循环
- [ ] 用 `const`/`let`、箭头函数、模板字符串、`?.`、`??`、async/await
- [ ] Vue：`<script setup>`、defineProps/defineEmits、ref/computed/onMounted 顺序规范
- [ ] 组件结构：template → script（导入 → props → emits → 数据 → 计算属性 → 方法 → 生命周期）→ style scoped
- [ ] 命名：组件/文件 PascalCase，变量/函数 camelCase，常量 UPPER_SNAKE_CASE
- [ ] 导入顺序：Vue → 第三方 → 工具 → 组件 → 类型
- [ ] 列表用 `:key`，重计算用 computed，大组件用动态 import()

## Vue 组件结构顺序（script setup）

1. 导入依赖
2. defineProps
3. defineEmits
4. 响应式数据 (ref/reactive)
5. 计算属性 (computed)
6. 方法
7. 生命周期 (onMounted 等)

## 命名与文件

| 类型     | 规范             | 示例            |
|----------|------------------|-----------------|
| 组件/文件 | PascalCase       | UserCard.vue    |
| 工具文件 | camelCase        | formatDate.js   |
| 常量文件 | UPPER_SNAKE_CASE | API_CONFIG.js   |

## 详细规范与示例

完整数据处理、现代语法、Vue、HTML、CSS、JS、性能优化及示例见 **[reference.md](reference.md)**。生成或审查前端代码时，遇具体写法不确定请查阅该文件。
