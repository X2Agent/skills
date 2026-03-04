---
name: code-review
description: 代码规范审查技能（Code Standards Review Skill）。当用户需要检查代码是否符合编码规范时使用本技能。支持 C#、Python、Golang 和 JavaScript 语言，以及各语言的主流框架（ASP.NET Core、Entity Framework、Django、FastAPI、Flask、Gin、Echo、GORM、React、Vue、Angular、Express 等）。本技能只进行编码规范检查，不进行代码逻辑审查。Use this skill when asked to review code style, naming conventions, formatting, or standards compliance for C#, Python, Go, or JavaScript projects.
---

# Code Standards Review Skill（代码规范审查技能）

你是一位专业的代码规范审查专家。当用户请求检查代码规范时，请严格遵循以下规范手册，逐条进行检查。

## 重要原则

- **只检查编码规范**，不进行代码逻辑分析、性能分析或安全审计
- 识别代码使用的语言和框架后，加载对应的规范进行检查
- 报告中需注明违规的文件名、行号（如可知）、违规类型和修正建议
- 输出格式为结构化报告，按语言/框架分类列出问题

---

## 审查工作流程

1. **识别语言和框架**：扫描文件扩展名、import/using 语句、配置文件（如 `package.json`、`go.mod`、`*.csproj`、`requirements.txt`）
2. **加载对应规范**：根据识别的语言和框架，选用以下对应的规范章节
3. **逐项检查**：针对每个规范要点逐一检查
4. **生成报告**：以结构化格式输出所有违规点

---

## C# 编码规范

参考：Microsoft C# 编码约定、.NET 设计准则

### 命名规范

- **类、接口、枚举、结构、委托**：使用 PascalCase（大驼峰），例：`CustomerOrder`、`IRepository`
- **方法、属性**：使用 PascalCase，例：`GetUserById()`、`FirstName`
- **私有字段**：使用 camelCase 加下划线前缀，例：`_userId`、`_connectionString`
- **局部变量、参数**：使用 camelCase，例：`userId`、`orderCount`
- **常量**：使用 PascalCase，例：`MaxRetryCount`（不使用全大写加下划线）
- **接口**：必须以 `I` 开头，例：`IUserService`、`IRepository<T>`
- **泛型类型参数**：以 `T` 开头，例：`TKey`、`TValue`，单参数时直接用 `T`
- **命名空间**：使用 PascalCase，反映公司/项目/功能层次结构，例：`Company.Product.Module`
- **异步方法**：以 `Async` 结尾，例：`GetUserAsync()`、`SaveChangesAsync()`
- **布尔属性/变量**：使用 `Is`、`Has`、`Can`、`Should` 前缀，例：`IsActive`、`HasPermission`

### 代码格式规范

- 缩进使用 4 个空格（不使用 Tab）
- 左大括号 `{` 另起一行（Allman 风格）
- 每个文件顶部的 `using` 语句按字母排序，`System.*` 命名空间排在最前面
- 每行代码不超过 160 个字符
- 类成员排列顺序：常量 → 静态字段 → 实例字段 → 构造函数 → 析构函数 → 属性 → 方法 → 嵌套类型
- 访问修饰符排列顺序：`public` → `protected internal` → `protected` → `internal` → `private protected` → `private`
- 每个类、接口、枚举独立一个文件，文件名与类型名一致

### 注释规范

- 公共 API（类、方法、属性）必须使用 XML 文档注释（`///`）
- 注释语言保持一致（中文或英文选择一种）
- 避免注释重复解释明显的代码；注释应说明"为什么"而非"是什么"
- TODO 注释格式：`// TODO: [作者] 描述`

### 语言特性使用规范

- 优先使用 `var` 当类型可从右侧推断时；类型不明显时使用显式类型
- 字符串格式化优先使用内插字符串（`$""`），而非 `string.Format()`
- `null` 检查优先使用 `?.`（空条件运算符）和 `??`（空合并运算符）
- 使用对象/集合初始化器简化初始化代码
- 优先使用 `async`/`await` 而非直接使用 `Task.ContinueWith`
- 避免在循环中使用 `string` 拼接，应使用 `StringBuilder`
- 属性应优先使用自动属性（`{ get; set; }`）

### ASP.NET Core 框架规范

- 控制器类名以 `Controller` 结尾，例：`UserController`
- 控制器继承 `ControllerBase`（API）或 `Controller`（MVC）
- 路由使用特性路由（`[Route]`、`[HttpGet]`、`[HttpPost]` 等）
- Action 方法返回类型使用 `IActionResult` 或 `ActionResult<T>`
- 使用依赖注入而非 `new` 直接实例化服务
- 服务注册遵循生命周期约定：`AddSingleton`、`AddScoped`、`AddTransient`
- Middleware 类名以 `Middleware` 结尾
- 配置类名以 `Options` 结尾，并使用 `IOptions<T>` 注入

### Entity Framework 规范

- DbContext 类名以 `Context` 或 `DbContext` 结尾
- DbSet 属性名使用复数形式，例：`public DbSet<User> Users { get; set; }`
- 实体类不应包含数据访问逻辑
- 配置实体映射应优先使用 Fluent API（`IEntityTypeConfiguration<T>`）而非 Data Annotations
- 迁移文件名应描述变更内容，例：`AddUserEmailIndex`

---

## Python 编码规范

参考：PEP 8、PEP 257、PEP 484

### 命名规范

- **模块、包**：全小写，单词间用下划线，例：`user_service.py`、`data_models`
- **类**：PascalCase（大驼峰），例：`UserProfile`、`OrderManager`
- **函数、方法、变量**：全小写加下划线（snake_case），例：`get_user_by_id()`、`user_count`
- **常量**：全大写加下划线，例：`MAX_RETRY_COUNT`、`DEFAULT_TIMEOUT`
- **私有属性/方法**：单下划线前缀 `_`，例：`_cache`、`_validate()`
- **名称修饰（禁止外部访问）**：双下划线前缀 `__`，例：`__secret_key`
- **魔术方法**：双下划线包围，例：`__init__`、`__str__`
- **类型变量**：PascalCase 或单个大写字母，例：`T`、`KT`、`VT`

### 代码格式规范

- 缩进使用 4 个空格（不使用 Tab）
- 每行不超过 79 个字符（注释/文档字符串不超过 72 个字符）
- 顶级定义之间空 2 行，类内方法之间空 1 行
- `import` 排序：标准库 → 第三方库 → 本地包，各组之间空 1 行
- 每个 `import` 语句只导入一个模块（`import os, sys` 不合规）
- 不使用通配符导入（`from module import *`）
- 运算符两侧、逗号后、冒号后应有空格；函数调用括号内侧无空格
- 列表/字典/集合的最后一个元素后可加逗号（trailing comma）

### 注释和文档规范

- 公共模块、类、函数、方法必须有 docstring（PEP 257）
- 单行 docstring：`"""返回用户对象。"""`（首尾同行）
- 多行 docstring：摘要行、空行、详细描述，结尾 `"""` 独占一行
- 使用 Google 风格或 NumPy 风格 docstring，同一项目内保持一致
- 行内注释与代码至少间隔 2 个空格，以 `# ` 开头

### 类型注解规范（PEP 484）

- 函数参数和返回值应有类型注解
- 变量注解在赋值时使用，例：`name: str = "Alice"`
- 使用 `Optional[T]` 表示可能为 `None` 的值（Python 3.9 以下），或 `T | None`（Python 3.10+）
- 集合类型注解使用 `list[T]`、`dict[K, V]`（Python 3.9+）或 `List[T]`、`Dict[K, V]`（需从 `typing` 导入）

### Django 框架规范

- 视图函数/类放在 `views.py`，URL 配置放在 `urls.py`，数据模型放在 `models.py`
- 模型类名使用单数 PascalCase，例：`UserProfile`（不用 `UserProfiles`）
- 模型字段名使用 snake_case
- 视图类继承适当的通用视图（`ListView`、`DetailView`、`CreateView` 等）
- URL 命名使用 app_name 命名空间，例：`app_name = 'users'`
- 模板标签和过滤器放在 `templatetags/` 目录
- 使用 `get_object_or_404` 代替手动处理 `DoesNotExist` 异常
- 表单类以 `Form` 结尾，例：`UserRegistrationForm`
- 序列化器类以 `Serializer` 结尾（DRF），例：`UserSerializer`

### FastAPI 框架规范

- 路由函数使用 `async def`（I/O 密集）或 `def`（CPU 密集）
- 路径操作装饰器的 HTTP 动词与语义匹配：`@app.get`、`@app.post`、`@app.put`、`@app.delete`
- 请求体模型继承 `pydantic.BaseModel`，字段名使用 snake_case
- 响应模型通过 `response_model` 参数声明
- 依赖注入使用 `Depends()`
- 路由器使用 `APIRouter` 并通过 `app.include_router()` 注册
- 状态码使用 `status` 模块常量，例：`status.HTTP_201_CREATED`

### Flask 框架规范

- 应用工厂模式：使用 `create_app()` 函数创建应用实例
- 蓝图（Blueprint）名称使用小写，例：`auth_bp = Blueprint('auth', __name__)`
- 视图函数名称与路由路径语义一致
- 配置使用类继承方式，例：`class ProductionConfig(Config):`
- 使用 `current_app` 和 `g` 而非全局变量

---

## Golang 编码规范

参考：Effective Go、Go Code Review Comments、Google Go Style Guide

### 命名规范

- **包名**：全小写，简短，无下划线，例：`httputil`、`userservice`
- **导出标识符（公有）**：PascalCase，例：`UserService`、`GetUserByID()`
- **未导出标识符（私有）**：camelCase，例：`userCount`、`parseConfig()`
- **接口名**：单方法接口以方法名加 `er` 结尾，例：`Reader`、`Writer`、`Stringer`
- **常量**：PascalCase（导出）或 camelCase（未导出），不使用全大写加下划线
- **错误变量**：以 `Err` 开头，例：`ErrNotFound`、`ErrInvalidInput`
- **错误类型**：以 `Error` 结尾，例：`ValidationError`、`NotFoundError`
- **缩写词**：统一大写或小写，例：`userID`（不是 `userId`）、`parseURL`（不是 `parseUrl`）
- **Receiver 名称**：使用类型名的首字母缩写（1-2个字符），保持一致，例：`func (u *User) Save()`

### 代码格式规范

- 必须使用 `gofmt` 格式化代码（使用 Tab 缩进）
- 导入分组：标准库 → 第三方库 → 本地包，各组用空行分隔
- 使用 `goimports` 自动管理导入
- 每行不超过 120 个字符（建议）
- 变量声明：多个同类型变量使用组声明 `var (...)` 或 `const (...)`

### 注释规范

- 导出的包、函数、类型、变量必须有文档注释
- 注释以被注释的名称开头，例：`// UserService handles user operations.`
- 包注释放在 `package` 语句之前，例：`// Package userservice provides...`
- 使用完整句子，以句号结尾

### 错误处理规范

- 错误必须被处理，不能用 `_` 忽略（除非有明确理由）
- 错误信息使用小写，不以标点结尾，例：`"user not found"`（不是 `"User not found."`）
- 自定义错误使用 `fmt.Errorf("...: %w", err)` 包装（Go 1.13+）
- 不使用 `panic` 处理正常错误流程；`panic` 只用于不可恢复的程序错误
- 返回错误时，其他返回值应为零值

### 代码组织规范

- 接口应在使用方定义，而非实现方（Go 接口隐式实现原则）
- 避免过深的嵌套；提前返回错误（"happy path"在最后）
- 不导出内部实现细节
- 测试文件以 `_test.go` 结尾，测试函数以 `Test` 开头

### Gin 框架规范

- 路由定义按 HTTP 方法和功能模块分组
- Handler 函数名以 `Handle` 开头或以功能命名，例：`HandleCreateUser()`、`GetUser()`
- 使用 `c.ShouldBindJSON()` 或 `c.ShouldBind()` 绑定请求体（而非 `c.BindJSON()`，后者会写入 400 响应头）
- 响应使用 `c.JSON()`、`c.String()` 等方法，显式传递 HTTP 状态码
- 中间件函数名以 `Middleware` 结尾或以功能命名，例：`AuthMiddleware()`

### Echo 框架规范

- Handler 函数签名：`func(c echo.Context) error`
- 路由分组使用 `e.Group()` 按模块组织
- 使用 `c.Bind()` 绑定请求，`c.Validate()` 验证
- 错误返回使用 `echo.NewHTTPError(code, message)`

### GORM 规范

- 模型结构体嵌入 `gorm.Model` 或手动声明主键
- 表名约定：结构体名的蛇形复数（`User` → `users`）
- 字段标签格式：`gorm:"column:user_name;type:varchar(100);not null"`
- 使用 `db.Error` 检查操作结果，而非忽略返回值
- 软删除使用 `DeletedAt gorm.DeletedAt` 字段（已包含在 `gorm.Model` 中）

---

## JavaScript / TypeScript 编码规范

参考：Airbnb JavaScript Style Guide、Google JavaScript Style Guide、TypeScript 官方规范

### 命名规范

- **变量、函数**：camelCase，例：`getUserById()`、`totalCount`
- **类、构造函数**：PascalCase，例：`UserService`、`EventEmitter`
- **常量（不可变值）**：全大写加下划线，例：`MAX_RETRY_COUNT`、`API_BASE_URL`
- **私有类成员**（TypeScript）：使用 `private` 关键字或 `#` 前缀
- **接口名**（TypeScript）：PascalCase，不加 `I` 前缀（除非项目约定），例：`UserProfile`
- **类型别名**（TypeScript）：PascalCase，例：`UserId`、`ApiResponse<T>`
- **枚举**（TypeScript）：PascalCase，例：`enum UserStatus { Active, Inactive }`
- **文件名**：与默认导出的内容名称一致，使用 camelCase 或 PascalCase（组件文件用 PascalCase）

### 代码格式规范

- 缩进使用 2 个空格（不使用 Tab，除非 `.editorconfig` 另有规定）
- 使用单引号（`'`）而非双引号（`"`），除非字符串内含单引号
- 语句末尾加分号（遵循 Airbnb 风格）
- 对象字面量的最后一个属性后加尾逗号（trailing comma）
- 每行不超过 100 个字符
- 使用严格相等（`===`）而非宽松相等（`==`）
- 变量声明优先使用 `const`，需要重新赋值时使用 `let`，避免 `var`

### 注释规范

- 函数使用 JSDoc 注释，例：`/** @param {string} id @returns {User} */`
- TypeScript 中优先通过类型注解而非 JSDoc 表达类型
- 行内注释以 `// ` 开头，与代码同行时至少间隔 2 个空格

### TypeScript 特定规范

- 启用严格模式（`"strict": true`）
- 避免使用 `any` 类型；必要时使用 `unknown` 并做类型收窄
- 接口优先于类型别名（用于对象结构定义）；类型别名用于联合类型等复杂场景
- 非空断言（`!`）需有充分理由，尽量通过类型收窄代替
- 枚举优先使用 `const enum` 以减少运行时开销
- 泛型名称使用单个大写字母（`T`、`U`）或描述性 PascalCase（`TKey`、`TValue`）

### React 框架规范

- 组件文件以 `.jsx` 或 `.tsx` 结尾，组件名使用 PascalCase
- 每个文件只定义一个 React 组件（HOC 和辅助组件除外）
- 函数组件优先于类组件
- Props 接口名以组件名加 `Props` 结尾，例：`interface UserCardProps`
- Hooks 命名以 `use` 开头，例：`useUserData()`、`useAuthState()`
- 事件处理函数以 `handle` 开头，例：`handleSubmit()`、`handleChange()`
- JSX 属性：字符串使用双引号，JS 表达式使用大括号，例：`className="active"` vs `style={{ color: 'red' }}`
- 避免在 JSX 中内联定义函数（除非使用 `useCallback`）
- Key prop 不使用数组索引（`index`），应使用唯一 ID

### Vue.js 框架规范

- 组件文件以 `.vue` 结尾，名称使用 PascalCase（多词组件名避免与 HTML 标签冲突）
- `<template>`、`<script>`、`<style>` 块顺序保持一致（推荐此顺序）
- Composition API 优先于 Options API（Vue 3）
- Props 定义使用对象形式，指定类型和是否必须
- Emit 事件名使用 camelCase，在模板中使用 kebab-case
- `v-for` 必须配合 `:key`，且不使用数组索引作为 key
- `v-if` 和 `v-for` 不在同一元素上使用

### Angular 框架规范

- 遵循 Angular 官方风格指南（Style Guide）
- 组件文件命名：`feature.component.ts`，选择器：`app-feature`（kebab-case）
- 服务命名：`feature.service.ts`，类名：`FeatureService`，选择器标注 `@Injectable({ providedIn: 'root' })`
- 模块命名：`feature.module.ts`，类名：`FeatureModule`
- 管道（Pipe）命名：`feature.pipe.ts`，类名：`FeaturePipe`
- 指令（Directive）命名：`feature.directive.ts`，选择器使用 camelCase（`[appHighlight]`）
- 组件属性应声明访问修饰符（`public`/`private`）
- 使用 `OnPush` 变更检测策略以提升性能（推荐）

### Node.js / Express 框架规范

- 路由文件按功能模块组织，放在 `routes/` 目录
- 中间件函数签名：`(req, res, next) => {}`
- 错误处理中间件使用 4 个参数：`(err, req, res, next) => {}`
- 异步路由处理使用 `async`/`await`，并使用 try/catch 或 `next(err)` 传递错误
- 环境变量通过 `process.env` 访问，使用 `dotenv` 加载本地配置
- 避免在路由处理器中直接操作数据库，应通过服务层（Service Layer）访问

---

## 审查报告格式

完成审查后，输出以下格式的报告：

```
## 代码规范审查报告

**项目语言/框架**：[识别到的语言和框架]
**审查范围**：[标准规范检查，不含逻辑审查]

---

### 违规项汇总

| 序号 | 文件 | 行号 | 违规类型 | 说明 | 修正建议 |
|------|------|------|----------|------|----------|
| 1    | xxx.cs | 12 | 命名规范 | 私有字段 `userId` 应使用 `_userId` | 改为 `_userId` |
| ...  | ...  | ... | ...      | ...  | ...      |

---

### 分类统计

- 命名规范违规：X 处
- 格式规范违规：X 处
- 注释规范违规：X 处
- 框架使用规范违规：X 处

---

### 总体评估

[简要说明代码整体规范遵循情况，以及最需要优先改进的方面]
```

---

## 使用示例

- "请检查这段 C# 代码的规范"
- "审查当前项目是否符合 PEP 8 规范"
- "检查这个 Go 文件的命名和注释规范"
- "这段 React 代码是否符合编码规范？"
- "对整个项目进行代码规范审查"
