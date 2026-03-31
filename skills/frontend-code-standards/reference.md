# 前端代码生成规范

## 核心原则

1. **数据处理采用结构化方式**：使用解构赋值、展开运算符等现代语法处理数据
2. **使用现代语法**：采用 ES6+ 语法，兼容现代浏览器（Chrome 90+, Firefox 88+, Safari 14+, Edge 90+）
3. **Vue 组件化开发**：将功能封装成独立、可复用的组件
4. **代码简洁高效**：遵循 DRY 原则，减少冗余，提高代码质量
5. **CSS 样式优先级**：**如果项目使用了 Tailwind CSS、UnoCSS 等原子化 CSS 库，优先使用原子化 CSS 写法；否则使用传统 CSS 规范（BEM 命名、scoped 样式等）**
6. **遵循行业规范**：参考 [LoongZero 前端开发规范](https://doc.loongzero.com/get-started/)

---

## 1. 数据处理规范

### 1.1 使用解构赋值

```javascript
// ✅ 推荐：使用解构赋值
const { name, age, email } = user;
const [first, second, ...rest] = items;

// ❌ 避免：直接访问属性
const name = user.name;
const age = user.age;
```

### 1.2 对象和数组操作

```javascript
// ✅ 推荐：使用展开运算符
const newObj = { ...oldObj, newProp: value };
const newArr = [...oldArr, newItem];

// ✅ 推荐：使用数组方法
const filtered = items.filter(item => item.active);
const mapped = items.map(item => ({ ...item, processed: true }));
```

### 1.3 函数参数处理

```javascript
// ✅ 推荐：使用解构参数和默认值
function processUser({ name, age = 18, email = '' } = {}) {
  // ...
}

// ✅ 推荐：使用剩余参数
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}
```

---

## 2. 现代语法规范

### 2.1 ES6+ 特性使用

- **箭头函数**：优先使用箭头函数，保持上下文简洁
- **模板字符串**：使用模板字符串替代字符串拼接
- **async/await**：异步操作优先使用 async/await
- **可选链和空值合并**：使用 `?.` 和 `??` 运算符

```javascript
// ✅ 推荐
const message = `Hello, ${user.name}!`;
const value = obj?.prop?.nested ?? 'default';
const result = await fetchData();

// ❌ 避免
const message = 'Hello, ' + user.name + '!';
const value = obj && obj.prop && obj.prop.nested || 'default';
```

### 2.2 浏览器兼容性

- 目标浏览器：Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- 使用 Babel 转译不兼容的语法
- 避免使用已废弃的 API

---

## 3. Vue 开发规范

### 3.1 组件化原则

- **单一职责**：每个组件只负责一个功能
- **可复用性**：提取公共逻辑为独立组件
- **组件命名**：使用 PascalCase，文件名与组件名一致

```vue
<!-- ✅ 推荐：独立的功能组件 -->
<!-- components/UserCard.vue -->
<template>
  <div class="user-card">
    <img :src="avatar" :alt="name" />
    <h3>{{ name }}</h3>
    <p>{{ email }}</p>
  </div>
</template>

<script setup>
defineProps({
  name: String,
  email: String,
  avatar: String
});
</script>
```

### 3.2 Composition API 优先

```vue
<script setup>
// ✅ 推荐：使用 Composition API (Vue 3)
import { ref, computed, onMounted } from 'vue';

const count = ref(0);
const doubleCount = computed(() => count.value * 2);

onMounted(() => {
  // 初始化逻辑
});
</script>
```

### 3.3 组件结构规范

```vue
<template>
  <!-- 模板内容 -->
</template>

<script setup>
// 1. 导入依赖
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';

// 2. Props 定义
const props = defineProps({
  // ...
});

// 3. Emits 定义
const emit = defineEmits(['update', 'delete']);

// 4. 响应式数据
const state = ref({});

// 5. 计算属性
const computedValue = computed(() => {
  // ...
});

// 6. 方法
const handleClick = () => {
  // ...
};

// 7. 生命周期
onMounted(() => {
  // ...
});
</script>

<style scoped>
/* 样式 */
</style>
```

### 3.4 Props 和 Emits 规范

```vue
<script setup>
// ✅ 推荐：明确的类型定义
defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  },
  items: {
    type: Array,
    default: () => []
  }
});

// ✅ 推荐：明确的 emits
const emit = defineEmits<{
  (e: 'update', value: string): void;
  (e: 'delete', id: number): void;
}>();
</script>
```

---

## 4. 代码简洁性规范

### 4.1 DRY 原则

```javascript
// ✅ 推荐：提取公共逻辑
const formatDate = (date) => {
  return new Intl.DateTimeFormat('zh-CN').format(new Date(date));
};

// ❌ 避免：重复代码
const formatDate1 = (date) => { /* ... */ };
const formatDate2 = (date) => { /* ... */ };
```

### 4.2 函数式编程

```javascript
// ✅ 推荐：使用函数式方法
const activeUsers = users
  .filter(user => user.active)
  .map(user => ({ ...user, processed: true }))
  .sort((a, b) => a.name.localeCompare(b.name));

// ❌ 避免：冗长的循环
const activeUsers = [];
for (let i = 0; i < users.length; i++) {
  if (users[i].active) {
    // ...
  }
}
```

### 4.3 条件判断简化

```javascript
// ✅ 推荐：使用三元运算符和逻辑运算符
const status = isActive ? 'active' : 'inactive';
const value = data?.value ?? defaultValue;

// ✅ 推荐：提前返回
function processData(data) {
  if (!data) return null;
  if (!data.valid) return null;
  // 处理逻辑
}
```

---

## 5. HTML 规范

### 5.1 代码格式

- 使用 2 个空格缩进
- 标签名小写
- 属性使用双引号
- 自闭合标签使用 `/` 结尾

```html
<!-- ✅ 推荐 -->
<div class="container">
  <img src="image.jpg" alt="描述" />
  <input type="text" name="username" />
</div>
```

### 5.2 语义化标签

```html
<!-- ✅ 推荐：使用语义化标签 -->
<header>
  <nav>
    <ul>
      <li><a href="/">首页</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>标题</h1>
    <p>内容</p>
  </article>
</main>
<footer>页脚</footer>
```

---

## 6. CSS 规范

### 6.0 原子化 CSS 优先级规则

**重要：优先使用原子化 CSS 框架**

- **如果项目使用了 Tailwind CSS、UnoCSS 等原子化 CSS 库，优先使用原子化 CSS 的写法**
- **如果项目没有使用原子化 CSS 库，则使用传统的 CSS 规范写法（BEM 命名、scoped 样式等）**

#### 6.0.1 原子化 CSS 写法（Tailwind CSS / UnoCSS）

```vue
<!-- ✅ 推荐：使用原子化 CSS -->
<template>
  <div class="flex flex-col p-5 rounded-lg bg-white shadow-md hover:shadow-lg transition-shadow">
    <div class="flex items-center justify-center mb-4 relative">
      <img 
        :src="avatar" 
        :alt="name" 
        class="w-20 h-20 rounded-full object-cover border-2 border-gray-200"
      />
      <span v-if="isVip" class="absolute top-0 right-0 px-2 py-1 bg-red-500 text-white text-xs rounded-full">
        VIP
      </span>
    </div>
    <div class="text-center mb-4">
      <h3 class="text-lg font-semibold text-gray-800 mb-2">{{ name }}</h3>
      <p class="text-sm text-gray-500 mb-3">{{ email }}</p>
    </div>
    <div class="flex gap-2 justify-center">
      <button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors">
        编辑
      </button>
      <button class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600 transition-colors">
        删除
      </button>
    </div>
  </div>
</template>

<script setup>
// 组件逻辑
</script>
```

#### 6.0.2 传统 CSS 写法（无原子化 CSS 库时）

```vue
<!-- ✅ 推荐：使用传统 CSS（项目未使用原子化 CSS 时） -->
<template>
  <div class="user-card">
    <div class="user-card__header">
      <img :src="avatar" :alt="name" class="user-card__avatar" />
    </div>
    <div class="user-card__body">
      <h3 class="user-card__name">{{ name }}</h3>
      <p class="user-card__email">{{ email }}</p>
    </div>
  </div>
</template>

<script setup>
// 组件逻辑
</script>

<style scoped>
.user-card {
  display: flex;
  flex-direction: column;
  padding: 20px;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.user-card__header {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
}

.user-card__avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
}
</style>
```

#### 6.0.3 混合使用（特殊情况）

```vue
<!-- ✅ 推荐：原子化 CSS + 少量自定义样式 -->
<template>
  <div class="flex items-center p-4 custom-card">
    <!-- 使用原子化 CSS 处理布局和基础样式 -->
    <div class="w-12 h-12 rounded-full bg-gray-200"></div>
    <div class="ml-4 flex-1">
      <h3 class="text-lg font-semibold">{{ title }}</h3>
    </div>
  </div>
</template>

<style scoped>
/* 仅在需要复杂样式或动画时使用自定义 CSS */
.custom-card {
  /* 复杂的动画或特殊效果 */
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
```

**判断项目是否使用原子化 CSS 的方法：**
- 检查 `package.json` 中是否包含 `tailwindcss`、`unocss` 等依赖
- 检查项目根目录是否有 `tailwind.config.js`、`uno.config.js` 等配置文件
- 检查 `main.js` 或入口文件中是否导入了原子化 CSS 的样式文件

### 6.1 命名规范

- 使用 BEM 命名规范或语义化命名
- 类名使用小写字母和连字符

```css
/* ✅ 推荐：BEM 命名 */
.block {}
.block__element {}
.block--modifier {}

/* ✅ 推荐：语义化命名 */
.user-card {}
.user-card__avatar {}
.user-card--highlighted {}
```

### 6.2 Vue 样式规范

```vue
<style scoped>
/* ✅ 推荐：使用 scoped 避免样式污染 */
.container {
  padding: 20px;
}

/* ✅ 推荐：使用深度选择器（需要时） */
:deep(.external-class) {
  color: red;
}
</style>
```

---

## 7. JavaScript 规范

### 7.1 变量声明

```javascript
// ✅ 推荐：使用 const，需要时使用 let
const API_URL = 'https://api.example.com';
let count = 0;

// ❌ 避免：使用 var
var oldVariable = 'avoid';
```

### 7.2 函数定义

```javascript
// ✅ 推荐：箭头函数（简单函数）
const add = (a, b) => a + b;

// ✅ 推荐：函数声明（复杂函数）
function processComplexData(data) {
  // 复杂逻辑
}

// ✅ 推荐：async 函数
const fetchData = async () => {
  const response = await fetch(url);
  return response.json();
};
```

### 7.3 错误处理

```javascript
// ✅ 推荐：使用 try-catch
try {
  const data = await fetchData();
  return data;
} catch (error) {
  console.error('Error fetching data:', error);
  throw error;
}
```

---

## 8. 命名规范

### 8.1 文件命名

- 组件文件：PascalCase，如 `UserCard.vue`
- 工具文件：camelCase，如 `formatDate.js`
- 常量文件：UPPER_SNAKE_CASE，如 `API_CONFIG.js`

### 8.2 变量和函数命名

```javascript
// ✅ 推荐：camelCase
const userName = 'John';
const getUserData = () => {};

// ✅ 推荐：常量使用 UPPER_SNAKE_CASE
const MAX_COUNT = 100;
const API_BASE_URL = 'https://api.example.com';
```

### 8.3 组件命名

```vue
<!-- ✅ 推荐：PascalCase -->
<script setup>
// 组件名：UserProfileCard
</script>
```

---

## 9. 代码组织规范

### 9.1 导入顺序

```javascript
// 1. Vue 相关
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';

// 2. 第三方库
import axios from 'axios';
import dayjs from 'dayjs';

// 3. 工具函数
import { formatDate } from '@/utils/date';

// 4. 组件
import UserCard from '@/components/UserCard.vue';

// 5. 类型定义（TypeScript）
import type { User } from '@/types/user';
```

### 9.2 代码注释

```javascript
/**
 * 格式化用户数据
 * @param {Object} user - 用户对象
 * @param {string} user.name - 用户名
 * @param {number} user.age - 年龄
 * @returns {Object} 格式化后的用户对象
 */
const formatUser = ({ name, age }) => {
  // 处理逻辑
  return { name, age };
};
```

---

## 10. 性能优化规范

### 10.1 Vue 性能优化

```vue
<script setup>
// ✅ 推荐：使用 computed 缓存计算结果
const expensiveValue = computed(() => {
  return heavyCalculation(props.data);
});

// ✅ 推荐：使用 v-memo 优化列表渲染
// <div v-for="item in list" :key="item.id" v-memo="[item.id, item.name]">
</script>
```

### 10.2 代码分割

```javascript
// ✅ 推荐：动态导入
const HeavyComponent = () => import('@/components/HeavyComponent.vue');
```

---

## 11. 代码检查工具

- 使用 ESLint 进行代码检查
- 使用 Prettier 进行代码格式化
- 使用 Vue 官方推荐的 ESLint 插件

---

## 总结

生成代码时，请确保：

1. ✅ 数据处理使用解构赋值和现代语法
2. ✅ 使用 ES6+ 语法，兼容现代浏览器
3. ✅ Vue 组件独立、可复用
4. ✅ 代码简洁高效，无冗余
5. ✅ **CSS 样式优先级：如果项目使用了 Tailwind CSS、UnoCSS 等原子化 CSS 库，优先使用原子化 CSS 写法；否则使用传统 CSS 规范（BEM 命名、scoped 样式等）**
6. ✅ 遵循以上所有规范要求

参考文档：[LoongZero 前端开发规范](https://doc.loongzero.com/get-started/)
