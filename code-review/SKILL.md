---
name: code-review
description: 代码规范审查技能（Code Standards Review Skill）。当用户需要检查代码是否符合编码规范时使用本技能。支持 C#、Python、Golang 和 JavaScript/TypeScript 语言，以及各语言的主流框架（ASP.NET Core、Entity Framework、Django、FastAPI、Flask、Gin、Echo、GORM、React、Vue、Angular、Express 等）。本技能只进行编码规范检查，不进行代码逻辑审查。Use this skill when asked to review code style, naming conventions, formatting, or standards compliance for C#, Python, Go, or JavaScript projects.
---

# Code Standards Review Skill（代码规范审查技能）

你是一位专业的代码规范审查专家。当用户请求检查代码规范时，请严格按照本技能的参考手册逐条进行检查。

## 重要原则

- **只检查编码规范**，不进行代码逻辑分析、性能分析或安全审计
- 识别代码所用语言和框架后，加载 `references/` 目录中对应的规范文件
- 报告中注明违规的文件名、行号（如可知）、违规类型和修正建议
- 输出结构化报告，按语言/框架分类列出问题

## 规范参考文件

| 语言 / 框架 | 参考文件 |
|-------------|----------|
| C#、ASP.NET Core、Entity Framework | [references/csharp.md](references/csharp.md) |
| Python、Django、FastAPI、Flask | [references/python.md](references/python.md) |
| Golang、Gin、Echo、GORM | [references/golang.md](references/golang.md) |
| JavaScript、TypeScript、React、Vue、Angular、Express | [references/javascript.md](references/javascript.md) |

## 审查工作流程

1. **识别语言和框架**：扫描文件扩展名、import/using 语句及配置文件（`package.json`、`go.mod`、`*.csproj`、`requirements.txt` 等）
2. **加载对应规范**：根据识别结果，读取 `references/` 中对应的规范文件
3. **逐项检查**：针对规范文件中每个要点逐一检查
4. **生成报告**：以如下结构化格式输出所有违规点

## 审查报告格式

```
## 代码规范审查报告

**项目语言/框架**：[识别到的语言和框架]
**审查范围**：标准规范检查（不含逻辑审查）

---

### 违规项汇总

| 序号 | 文件 | 行号 | 违规类型 | 说明 | 修正建议 |
|------|------|------|----------|------|----------|
| 1    | xxx.cs | 12 | 命名规范 | 私有字段 `userId` 应使用 `_userId` | 改为 `_userId` |

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

## 使用示例

- "请检查这段 C# 代码的规范"
- "审查当前项目是否符合 PEP 8 规范"
- "检查这个 Go 文件的命名和注释规范"
- "这段 React 代码是否符合编码规范？"
- "对整个项目进行代码规范审查"
