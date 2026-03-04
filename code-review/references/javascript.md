# JavaScript / TypeScript 编码规范参考手册

## 官方参考来源

以下规范均直接源自官方权威文档，所有条目均可在原始文档中找到对应依据：

| 规范领域 | 官方文档 URL |
|----------|-------------|
| Airbnb JavaScript Style Guide | <https://github.com/airbnb/javascript> |
| Google JavaScript Style Guide | <https://google.github.io/styleguide/jsguide.html> |
| TypeScript 官方手册 | <https://www.typescriptlang.org/docs/handbook/> |
| TypeScript 严格模式 | <https://www.typescriptlang.org/tsconfig#strict> |
| React 官方文档 | <https://react.dev/> |
| Vue.js 官方风格指南 | <https://vuejs.org/style-guide/> |
| Angular 官方风格指南 | <https://angular.dev/style-guide> |
| Node.js / Express 文档 | <https://expressjs.com/en/guide/routing.html> |

---

## 命名规范

- **变量、函数**：camelCase，例：`getUserById()`、`totalCount`
- **类、构造函数**：PascalCase，例：`UserService`、`EventEmitter`
- **常量（不可变值）**：全大写加下划线，例：`MAX_RETRY_COUNT`、`API_BASE_URL`
- **私有类成员**（TypeScript）：使用 `private` 关键字或 `#` 前缀
- **接口名**（TypeScript）：PascalCase，不加 `I` 前缀（除非项目另有约定），例：`UserProfile`
- **类型别名**（TypeScript）：PascalCase，例：`UserId`、`ApiResponse<T>`
- **枚举**（TypeScript）：PascalCase，例：`enum UserStatus { Active, Inactive }`
- **文件名**：与默认导出内容名称一致，组件文件用 PascalCase，其余用 camelCase

---

## 代码格式规范

- 缩进使用 2 个空格（不使用 Tab，除非 `.editorconfig` 另有规定）
- 使用单引号（`'`）而非双引号（`"`），除非字符串内含单引号
- 语句末尾加分号（遵循 Airbnb 风格）
- 对象字面量的最后一个属性后加尾逗号（trailing comma）
- 每行不超过 100 个字符
- 使用严格相等（`===`），不使用宽松相等（`==`）
- 变量声明优先使用 `const`，需要重新赋值时使用 `let`，禁止使用 `var`

---

## 注释规范

- 函数使用 JSDoc 注释，例：`/** @param {string} id @returns {User} */`
- TypeScript 中优先通过类型注解表达类型，减少 JSDoc 中的重复类型说明
- 行内注释以 `// ` 开头，与代码同行时至少间隔 2 个空格

---

## TypeScript 特定规范

- 启用严格模式（`"strict": true`）
- 禁止使用 `any` 类型；必要时使用 `unknown` 并做类型收窄
- 对象结构定义优先使用 `interface`；联合类型等复杂场景使用 `type`
- 非空断言（`!`）需有充分理由，尽量通过类型收窄代替
- 枚举优先使用 `const enum` 以减少运行时开销
- 泛型名称使用单个大写字母（`T`、`U`）或描述性 PascalCase（`TKey`、`TValue`）

---

## React 框架规范

- 组件文件以 `.jsx` 或 `.tsx` 结尾，组件名使用 PascalCase
- 每个文件只定义一个 React 组件（HOC 和辅助组件除外）
- 函数组件优先于类组件
- Props 接口名以组件名加 `Props` 结尾，例：`interface UserCardProps`
- Hooks 命名以 `use` 开头，例：`useUserData()`、`useAuthState()`
- 事件处理函数以 `handle` 开头，例：`handleSubmit()`、`handleChange()`
- JSX 属性：字符串使用双引号，JS 表达式使用大括号
- 避免在 JSX 中内联定义函数（除非使用 `useCallback`）
- `key` prop 不使用数组索引，应使用唯一 ID

---

## Vue.js 框架规范

- 组件文件以 `.vue` 结尾，名称使用 PascalCase（多词组件名避免与 HTML 标签冲突）
- `<template>`、`<script>`、`<style>` 块顺序保持一致（推荐此顺序）
- Vue 3 中 Composition API 优先于 Options API
- Props 定义使用对象形式，指定类型和是否必须
- Emit 事件名使用 camelCase，在模板中使用 kebab-case
- `v-for` 必须配合 `:key`，且不使用数组索引作为 key
- `v-if` 和 `v-for` 不在同一元素上使用

---

## Angular 框架规范

- 遵循 Angular 官方风格指南
- 组件文件命名：`feature.component.ts`，选择器：`app-feature`（kebab-case）
- 服务命名：`feature.service.ts`，类名：`FeatureService`，标注 `@Injectable({ providedIn: 'root' })`
- 模块命名：`feature.module.ts`，类名：`FeatureModule`
- 管道（Pipe）命名：`feature.pipe.ts`，类名：`FeaturePipe`
- 指令（Directive）命名：`feature.directive.ts`，选择器使用 camelCase（`[appHighlight]`）
- 组件属性应声明访问修饰符（`public`/`private`）
- 推荐使用 `OnPush` 变更检测策略以提升性能

---

## Node.js / Express 框架规范

- 路由文件按功能模块组织，放在 `routes/` 目录
- 中间件函数签名：`(req, res, next) => {}`
- 错误处理中间件使用 4 个参数：`(err, req, res, next) => {}`
- 异步路由处理使用 `async`/`await`，并通过 try/catch 或 `next(err)` 传递错误
- 环境变量通过 `process.env` 访问，使用 `dotenv` 加载本地配置
- 路由处理器不直接操作数据库，应通过服务层（Service Layer）访问
