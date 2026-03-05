# Golang 编码规范参考手册

## 官方参考来源

以下规范均直接源自官方权威文档，所有条目均可在原始文档中找到对应依据：

| 规范领域 | 官方文档 URL |
|----------|-------------|
| Effective Go | <https://go.dev/doc/effective_go> |
| Go Code Review Comments | <https://go.dev/wiki/CodeReviewComments> |
| Google Go Style Guide | <https://google.github.io/styleguide/go/> |
| Gin 框架文档 | <https://gin-gonic.com/docs/> |
| Echo 框架文档 | <https://echo.labstack.com/docs/> |
| GORM 文档 | <https://gorm.io/docs/> |

---

## 命名规范

- **包名**：全小写，简短，无下划线，例：`httputil`、`userservice`
- **导出标识符（公有）**：PascalCase，例：`UserService`、`GetUserByID()`
- **未导出标识符（私有）**：camelCase，例：`userCount`、`parseConfig()`
- **接口名**：单方法接口以方法名加 `er` 结尾，例：`Reader`、`Writer`、`Stringer`
- **常量**：PascalCase（导出）或 camelCase（未导出），不使用全大写加下划线
- **错误变量**：以 `Err` 开头，例：`ErrNotFound`、`ErrInvalidInput`
- **错误类型**：以 `Error` 结尾，例：`ValidationError`、`NotFoundError`
- **缩写词**：统一大写或小写，例：`userID`（不是 `userId`）、`parseURL`（不是 `parseUrl`）
- **Receiver 名称**：使用类型名的首字母缩写（1-2 个字符），全文件保持一致，例：`func (u *User) Save()`

---

## 代码格式规范

- 必须使用 `gofmt` 格式化代码（Tab 缩进）
- 导入分组：标准库 → 第三方库 → 本地包，各组用空行分隔
- 使用 `goimports` 自动管理导入
- 每行不超过 120 个字符（建议值）
- 多个同类型变量使用组声明 `var (...)` 或 `const (...)`

---

## 注释规范

- 导出的包、函数、类型、变量必须有文档注释
- 注释以被注释的名称开头，例：`// UserService handles user operations.`
- 包注释放在 `package` 语句之前，例：`// Package userservice provides...`
- 注释使用完整句子，以句号结尾

---

## 错误处理规范

- 错误必须被处理，不得用 `_` 忽略（除非有明确说明理由）
- 错误信息使用小写，不以标点结尾，例：`"user not found"`（不是 `"User not found."`）
- 自定义错误使用 `fmt.Errorf("...: %w", err)` 包装（Go 1.13+）
- 不使用 `panic` 处理正常错误流程；`panic` 只用于不可恢复的程序错误
- 返回错误时，其他返回值应为零值

---

## 代码组织规范

- 接口应在使用方定义，而非实现方（Go 接口隐式实现原则）
- 避免过深嵌套；提前返回错误（"happy path" 在最后）
- 不导出内部实现细节
- 测试文件以 `_test.go` 结尾，测试函数以 `Test` 开头

---

## Gin 框架规范

- 路由定义按 HTTP 方法和功能模块分组
- Handler 函数名以 `Handle` 开头或以功能命名，例：`HandleCreateUser()`、`GetUser()`
- 使用 `c.ShouldBindJSON()` 或 `c.ShouldBind()` 绑定请求体（避免 `c.BindJSON()`，后者自动写入 400 响应头）
- 响应使用 `c.JSON()`、`c.String()` 等方法，并显式传递 HTTP 状态码
- 中间件函数名以 `Middleware` 结尾或以功能命名，例：`AuthMiddleware()`

---

## Echo 框架规范

- Handler 函数签名：`func(c echo.Context) error`
- 路由分组使用 `e.Group()` 按模块组织
- 使用 `c.Bind()` 绑定请求，`c.Validate()` 验证
- 错误返回使用 `echo.NewHTTPError(code, message)`

---

## GORM 规范

- 模型结构体嵌入 `gorm.Model` 或手动声明主键
- 表名约定：结构体名的蛇形复数（`User` → `users`）
- 字段标签格式：`gorm:"column:user_name;type:varchar(100);not null"`
- 检查操作结果，不忽略 `db.Error` 返回值
- 软删除使用 `DeletedAt gorm.DeletedAt` 字段（已包含在 `gorm.Model` 中）

---

## 并发与 Goroutine 规范

- Goroutine 必须有明确的退出机制，禁止创建后无法停止的 goroutine（goroutine 泄漏）
- 使用 `context.Context` 传递截止时间、取消信号和请求范围的值；函数第一个参数应为 `ctx context.Context`
- 共享状态通过 channel 或 `sync.Mutex` 保护；优先使用 channel 而非共享内存
- 使用 `sync.WaitGroup` 等待一组 goroutine 完成
- `sync.Once` 用于只需执行一次的初始化逻辑

---

## defer 使用规范

- `defer` 用于资源释放（关闭文件、解锁、关闭连接），紧跟资源获取语句之后
- 循环体内避免使用 `defer`（每次迭代都会新增 defer，资源不会在循环内及时释放）
- 注意 defer 中使用命名返回值可以修改函数的返回结果，需显式注释说明意图

---

## 测试规范

- 优先使用**表驱动测试**（table-driven tests）组织多个用例：

  ```go
  tests := []struct {
      name  string
      input int
      want  int
  }{
      {"positive", 1, 2},
      {"zero", 0, 1},
  }
  for _, tt := range tests {
      t.Run(tt.name, func(t *testing.T) { ... })
  }
  ```

- 使用 `t.Run()` 创建子测试，便于单独运行和并行化
- 测试辅助函数使用 `t.Helper()` 标记，使失败信息指向调用处而非辅助函数内部
- 使用 `t.Parallel()` 启用可并行的测试用例
