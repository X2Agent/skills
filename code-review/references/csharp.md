# C# 编码规范参考手册

## 官方参考来源

以下规范均直接源自官方权威文档，所有条目均可在原始文档中找到对应依据：

| 规范领域 | 官方文档 URL |
|----------|-------------|
| C# 编码约定 | <https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions> |
| .NET 命名准则 | <https://learn.microsoft.com/dotnet/standard/design-guidelines/naming-guidelines> |
| .NET 框架设计准则 | <https://learn.microsoft.com/dotnet/standard/design-guidelines/> |
| ASP.NET Core 文档 | <https://learn.microsoft.com/aspnet/core/> |
| Entity Framework Core 文档 | <https://learn.microsoft.com/ef/core/> |

---

## 命名规范

- **类、接口、枚举、结构、委托**：PascalCase，例：`CustomerOrder`、`IRepository`
- **方法、属性**：PascalCase，例：`GetUserById()`、`FirstName`
- **私有字段**：camelCase 加下划线前缀，例：`_userId`、`_connectionString`
- **局部变量、参数**：camelCase，例：`userId`、`orderCount`
- **常量**：PascalCase，例：`MaxRetryCount`（不使用全大写加下划线）
- **接口**：必须以 `I` 开头，例：`IUserService`、`IRepository<T>`
- **泛型类型参数**：以 `T` 开头，例：`TKey`、`TValue`，单参数时直接用 `T`
- **命名空间**：PascalCase，反映公司/项目/功能层次，例：`Company.Product.Module`
- **异步方法**：以 `Async` 结尾，例：`GetUserAsync()`、`SaveChangesAsync()`
- **布尔属性/变量**：`Is`、`Has`、`Can`、`Should` 前缀，例：`IsActive`、`HasPermission`

---

## 代码格式规范

- 缩进使用 4 个空格（不使用 Tab）
- 左大括号 `{` 另起一行（Allman 风格）
- 文件顶部 `using` 语句按字母排序，`System.*` 命名空间排在最前面
- 每行不超过 160 个字符
- 类成员排列顺序：常量 → 静态字段 → 实例字段 → 构造函数 → 析构函数 → 属性 → 方法 → 嵌套类型
- 访问修饰符排列顺序：`public` → `protected internal` → `protected` → `internal` → `private protected` → `private`
- 每个类、接口、枚举独立一个文件，文件名与类型名一致

---

## 注释规范

- 公共 API（类、方法、属性）必须使用 XML 文档注释（`///`）
- 注释语言保持一致（中文或英文选择一种）
- 注释说明"为什么"而非重复解释"是什么"
- TODO 注释格式：`// TODO: [作者] 描述`

---

## 语言特性使用规范

- 优先使用 `var`（类型可从右侧推断时）；类型不明显时使用显式类型
- 字符串格式化优先使用内插字符串（`$""`），而非 `string.Format()`
- `null` 检查优先使用 `?.` 和 `??` 运算符
- 使用对象/集合初始化器简化初始化代码
- 优先使用 `async`/`await`，而非 `Task.ContinueWith`
- 循环内避免字符串拼接，应使用 `StringBuilder`
- 属性优先使用自动属性（`{ get; set; }`）

---

## ASP.NET Core 框架规范

- 控制器类名以 `Controller` 结尾，例：`UserController`
- 控制器继承 `ControllerBase`（API）或 `Controller`（MVC）
- 路由使用特性路由（`[Route]`、`[HttpGet]`、`[HttpPost]` 等）
- Action 方法返回类型使用 `IActionResult` 或 `ActionResult<T>`
- 使用依赖注入，而非 `new` 直接实例化服务
- 服务生命周期约定：`AddSingleton`、`AddScoped`、`AddTransient`
- Middleware 类名以 `Middleware` 结尾
- 配置类名以 `Options` 结尾，使用 `IOptions<T>` 注入

---

## Entity Framework 规范

- DbContext 类名以 `Context` 或 `DbContext` 结尾
- DbSet 属性名使用复数形式，例：`public DbSet<User> Users { get; set; }`
- 实体类不应包含数据访问逻辑
- 实体映射优先使用 Fluent API（`IEntityTypeConfiguration<T>`），而非 Data Annotations
- 迁移文件名应描述变更内容，例：`AddUserEmailIndex`

---

## 异常处理规范

- 抛出具体异常类型，而非基类 `Exception`，例：`throw new ArgumentNullException(nameof(param))`
- 只捕获能处理的异常；避免空的 `catch` 块吞掉异常
- 捕获范围从小到大排列（派生异常在基类异常之前）
- 使用 `when` 过滤器限定捕获条件：`catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)`
- 不使用异常控制正常流程（例：不用 `try/catch` 代替 `TryParse` 检查）
- 最终清理资源优先使用 `using` 声明而非 `try/finally`
- 异常消息应包含足够的上下文信息，便于排查

---

## 模式匹配与现代 C# 特性规范

- 优先使用模式匹配（`switch expression`、`is` 模式）代替冗长的 `if-else` 链
- 使用 `record` 类型表示不可变数据传输对象（DTO）
- 使用可空引用类型（Nullable Reference Types，`#nullable enable`）并消除所有警告
- 使用 `init` 访问器创建初始化后不可变的属性
- 条件性 `null` 合并赋值：`collection ??= new List<T>()`
