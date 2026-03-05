---
name: spec-driven-development
# description 是技能路由信号，采用中英双语确保路由匹配。
description: 规范驱动开发技能。帮助开发者从 Vibe Coding 转向 Agentic Coding：先写规范文档，再按规范进行 TDD 实现。Use this skill when starting any new feature, module, or significant change — write the spec first, then implement with TDD.
---

# Spec-Driven Development（规范驱动开发）

<!--
## SKILL.md 结构说明

SKILL.md 由两部分组成：
1. **YAML frontmatter**：路由信号，由 Agent 系统读取以决定何时调用本技能。
2. **技能正文**：由 LLM 在被调用后读取，指导其如何执行任务。
   正文应包含：角色定义、本技能的作用、硬性门控、工作流程、模板、核心原则、使用示例。
-->

你是一位 Agentic Coding 实践专家。当用户开始一个新功能、模块或重要改动时，你的职责是：**在写任何代码之前，先帮用户把规范写清楚，经用户确认后，再逐条用 TDD 实现。**

## 本技能的作用

本技能通过以下方式帮助开发者落地 AI Coding：

1. **消除歧义**：通过对话把模糊想法转化为结构化的规范文档（接口契约 + 行为条目 + 错误处理 + 明确的 Out of Scope），让 Agent 有精确的任务书可以执行，而不是凭假设写代码。
2. **建立可追溯的契约**：每个行为规范条目（S-01、S-02 …）直接对应一个或多个自动化测试用例，代码和意图之间有明确的链接。
3. **驱动 TDD 实现**：规范确认后，Agent 针对每个条目严格执行 RED → GREEN → REFACTOR → COMMIT，确保每行生产代码都有对应测试覆盖。
4. **规范即文档**：规范文档在整个开发生命周期内保持更新，既是实现的起点，也是代码审查时的裁判。

<HARD-GATE>
在规范文档经用户逐节确认之前，禁止编写任何实现代码、脚手架或测试框架搭建代码。规范未确认 = 不开始实现。
</HARD-GATE>

---

## 工作流程

```
需求对话 → 编写规范文档 → 规范评审（用户逐节确认） → TDD 实现（每个规范条目） → 验收验证
```

### 第一步：需求对话（Requirement Dialogue）

在写任何规范或代码之前，先通过对话明确以下内容：

- **目标**：这个功能要解决什么问题？
- **用户**：谁会使用它？什么场景下使用？
- **边界**：哪些事情在本次范围内，哪些不在？
- **成功标准**：怎样算做好了？

> **规则**：每次只问一个问题，等用户回答后再问下一个。不要一次抛出问题清单。

### 第二步：编写规范文档（Write Spec）

使用下方的**规范文档模板**，生成 `docs/specs/YYYY-MM-DD-<功能名>.md` 文件（暂不提交，待评审通过后提交）。

规范必须包含：
- 功能概述（一句话）
- 接口契约（输入/输出/类型）
- 行为规范（每条用 Given/When/Then 格式）
- 错误处理规范
- 不在范围内的事项（明确排除）

### 第三步：规范评审（Spec Review）

将规范分节展示给用户，**每节确认后再继续**：
- "接口契约这部分，您看是否符合预期？"
- "这 5 个行为规范条目，有需要调整的吗？"

未经确认的规范不进入实现阶段。**规范获得用户确认后**，将文件状态更新为"已评审"，然后 `git commit`。

### 第四步：TDD 实现循环（TDD per Spec Item）

针对规范中**每一个行为条目**，严格执行：

```
RED  → 写一个针对该规范条目的失败测试
↓      确认测试因"功能缺失"而失败（而非语法错误）
GREEN → 写最少量的实现代码让测试通过
↓      确认测试通过且不破坏其他测试
REFACTOR → 清理代码，不增加功能
↓      确认测试仍然通过
COMMIT → git commit，附上规范条目编号
```

> **铁律**：先有测试，再有实现代码。没有先看到测试失败，就不知道测试是否真正在检验规范。

### 第五步：验收验证（Acceptance Verification）

所有规范条目实现完成后：

- [ ] 每个规范条目都有对应的自动化测试
- [ ] 所有测试通过，无警告
- [ ] 回顾规范文档，确认没有遗漏条目
- [ ] 错误处理场景均已覆盖
- [ ] 代码可以指向具体规范条目（可追溯性）

---

## 规范文档模板

将以下模板保存为 `docs/specs/YYYY-MM-DD-<功能名>.md`：

```markdown
# [功能名] 规范文档

**日期**：YYYY-MM-DD  
**状态**：草稿 / 已评审 / 实现中 / 已完成  
**作者**：[姓名]

> **状态流转**：草稿（编写中）→ 已评审（用户确认后提交 git）→ 实现中（开始 TDD 循环时更新）→ 已完成（验收通过后更新）

---

## 1. 功能概述

[一句话描述：这个功能做什么，解决什么问题。]

---

## 2. 接口契约

### 输入

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `param1` | `string` | 是 | 说明 |
| `param2` | `number` | 否 | 说明，默认值 X |

### 输出

| 字段名 | 类型 | 说明 |
|--------|------|------|
| `result` | `string` | 说明 |
| `error` | `string | null` | 错误信息，无错误时为 null |

---

## 3. 行为规范

> 每条规范对应一个或多个自动化测试用例。

### S-01：[条目名称]

- **Given**：[前置条件]
- **When**：[触发动作]
- **Then**：[期望结果]

### S-02：[条目名称]

- **Given**：[前置条件]
- **When**：[触发动作]
- **Then**：[期望结果]

---

## 4. 错误处理规范

### E-01：[错误场景名称]

- **触发条件**：[什么情况触发此错误]
- **期望行为**：[应返回/抛出什么]
- **错误信息**：`"[错误信息文本]"`

---

## 5. 不在范围内（Out of Scope）

以下事项**明确排除**于本次范围：

- [排除事项 1]
- [排除事项 2]

---

## 6. 实现进度追踪

| 规范条目 | 测试文件 | 状态 |
|----------|----------|------|
| S-01 | `tests/xxx_test.py` | ✅ 已完成 |
| S-02 | `tests/xxx_test.py` | 🔄 实现中 |
| E-01 | `tests/xxx_test.py` | ⬜ 待开始 |
```

---

## 重要原则

- **规范优先**：没有规范文档，不开始写实现代码
- **逐条验证**：每个规范条目对应至少一个自动化测试
- **规范即文档**：规范文档在整个生命周期内保持更新
- **先看失败**：TDD 中先确认测试失败，再写实现，否则测试无效
- **最小实现**：只实现规范要求的内容（YAGNI），不提前设计
- **可追溯性**：commit message 引用规范条目编号（如 `feat: implement S-01 email validation`）

---

## 触发示例

- "我想给用户系统加一个邮件验证功能"
- "帮我为这个 API 端点写规范"
- "按规范驱动的方式实现这个模块"
- "开始一个新功能前，先帮我梳理规范"
- "我们来做一个支付流程，从规范开始"

---

## 完整交互示例

以下演示用户说"我想给用户系统加一个邮件验证功能"后，Agent 按本技能执行的完整交互流程。

### 第一步：需求对话

> 👤 **用户**：我想给用户系统加一个邮件验证功能

> 🤖 **Agent**：好的，我们先把需求梳理清楚再开始实现。第一个问题：这个邮件验证功能具体要解决什么问题——是在注册时校验邮箱格式，还是发送验证邮件让用户点击链接激活账号？

> 👤 **用户**：发送验证邮件，用户注册后必须点击邮件中的链接才能激活账号。

> 🤖 **Agent**：明白了。谁会触发这个发送流程——是用户完成注册时系统自动发送，还是用户手动点击"发送验证邮件"才发，还是两种都有？

> 👤 **用户**：两种都有：注册时自动发送，用户也可以手动重新发送。

> 🤖 **Agent**：好的。验证链接的有效期是多久？用户重新发送时，旧的验证 token 是否应该立即失效？

> 👤 **用户**：24 小时有效，重新发送时旧 token 立即失效。

> 🤖 **Agent**：最后一个问题：邮件发送本身——是要实现完整的邮件发送逻辑，还是调用项目中现有的邮件服务接口？

> 👤 **用户**：调用现有的 `EmailService`，不需要实现发送逻辑本身。

> 🤖 **Agent**：信息收集完毕，现在开始编写规范文档。

---

### 第二步：编写规范文档

Agent 生成 `docs/specs/2026-03-05-email-verification.md`（草稿，待评审）：

````markdown
# 邮件验证 规范文档

**日期**：2026-03-05
**状态**：草稿
**作者**：[待填写]

> **状态流转**：草稿 → 已评审 → 实现中 → 已完成

---

## 1. 功能概述

用户注册后系统自动发送验证邮件，用户点击邮件中的链接激活账号；未激活用户也可手动重新发送验证邮件，重新发送时旧 token 立即失效。

---

## 2. 接口契约

### `send_verification_email(user_id, email)`

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `user_id` | `str` | 是 | 用户唯一 ID |
| `email` | `str` | 是 | 目标邮箱地址 |

| 返回字段 | 类型 | 说明 |
|----------|------|------|
| `success` | `bool` | 是否发送成功 |
| `token` | `str \| null` | 生成的验证 token（失败时为 null）|
| `error` | `str \| null` | 错误信息（成功时为 null）|

### `verify_email_token(token)`

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `token` | `str` | 是 | 验证 token |

| 返回字段 | 类型 | 说明 |
|----------|------|------|
| `success` | `bool` | 验证是否通过 |
| `user_id` | `str \| null` | 通过时返回对应用户 ID |
| `error` | `str \| null` | 错误信息（成功时为 null）|

---

## 3. 行为规范

### S-01：注册时自动发送验证邮件

- **Given**：新用户完成注册，账号尚未激活
- **When**：调用 `send_verification_email(user_id, email)`
- **Then**：通过 `EmailService` 发送验证邮件，返回 `success=True` 和 64 位 hex token

### S-02：用户点击链接激活账号

- **Given**：存在一个未过期的有效 token
- **When**：调用 `verify_email_token(token)`
- **Then**：账号状态变为已激活，返回 `success=True` 和对应 `user_id`

### S-03：重新发送时旧 token 立即失效

- **Given**：用户已有一个未过期的待验证 token
- **When**：再次调用 `send_verification_email(user_id, email)`
- **Then**：旧 token 失效，生成并返回新 token，发送新验证邮件

### S-04：token 过期后验证失败

- **Given**：token 已超过 24 小时
- **When**：调用 `verify_email_token(token)`
- **Then**：返回 `success=False`，`error="token_expired"`

---

## 4. 错误处理规范

### E-01：token 不存在

- **触发条件**：传入的 token 在数据库中不存在
- **期望行为**：返回 `success=False`
- **错误信息**：`"token_not_found"`

### E-02：账号已激活时再次验证

- **触发条件**：token 对应的用户账号已处于激活状态
- **期望行为**：返回 `success=False`
- **错误信息**：`"already_verified"`

---

## 5. 不在范围内（Out of Scope）

- 邮件发送实现（使用现有 `EmailService`，不实现发送逻辑）
- 更改邮箱地址功能
- 邮件模板的 HTML 样式设计
- 发送频率限制（防刷）

---

## 6. 实现进度追踪

| 规范条目 | 测试文件 | 状态 |
|----------|----------|------|
| S-01 | `tests/test_email_verification.py` | ⬜ 待开始 |
| S-02 | `tests/test_email_verification.py` | ⬜ 待开始 |
| S-03 | `tests/test_email_verification.py` | ⬜ 待开始 |
| S-04 | `tests/test_email_verification.py` | ⬜ 待开始 |
| E-01 | `tests/test_email_verification.py` | ⬜ 待开始 |
| E-02 | `tests/test_email_verification.py` | ⬜ 待开始 |
````

---

### 第三步：规范评审

> 🤖 **Agent**：规范草稿已生成。我们逐节确认，先看接口契约：
>
> `send_verification_email` 接收 `user_id` 和 `email`，返回 `{success, token, error}`；
> `verify_email_token` 接收 `token`，返回 `{success, user_id, error}`。
>
> 这两个接口是否符合预期？

> 👤 **用户**：符合，继续。

> 🤖 **Agent**：好的。以下是 4 个行为规范条目（S-01 到 S-04），有需要调整的吗？
>
> [展示 S-01 ~ S-04 内容]

> 👤 **用户**：没问题，但 Out of Scope 里加一条：不支持发送频率限制（防刷）。

> 🤖 **Agent**：已加入 Out of Scope。错误处理（E-01、E-02）和最终的 Out of Scope 列表，是否还有需要补充的？

> 👤 **用户**：没有了，可以开始实现。

> 🤖 **Agent**：规范已获确认，更新状态为"已评审"并提交。
>
> ```
> git commit -m "spec: add email-verification spec (reviewed)"
> ```
>
> 现在开始 TDD 实现，从 **S-01** 开始。

---

### 第四步：TDD 实现（S-01 示例）

**S-01：注册时自动发送验证邮件**

**🔴 RED — 写失败测试**

```python
# tests/test_email_verification.py
from unittest.mock import Mock
from services.email_verification import EmailVerificationService

def test_s01_send_verification_email_on_registration():
    """S-01: 注册时自动发送验证邮件"""
    mock_email_service = Mock()
    service = EmailVerificationService(email_service=mock_email_service)

    result = service.send_verification_email(user_id="u-001", email="alice@example.com")

    assert result["success"] is True
    assert result["error"] is None
    assert len(result["token"]) == 64          # 64 位 hex token
    mock_email_service.send.assert_called_once()
    call_kwargs = mock_email_service.send.call_args.kwargs
    assert call_kwargs["to"] == "alice@example.com"
    assert call_kwargs["subject"] == "请验证您的邮箱"
    assert result["token"] in call_kwargs["body"]  # body 中包含 token
```

运行测试 → **FAIL**：`ModuleNotFoundError: No module named 'services.email_verification'` ✓（预期失败，原因是功能缺失）

**🟢 GREEN — 最少量实现**

```python
# services/email_verification.py
import secrets

class EmailVerificationService:
    def __init__(self, email_service):
        self._email_service = email_service

    def send_verification_email(self, user_id: str, email: str) -> dict:
        token = secrets.token_hex(32)  # 64 位 hex
        self._email_service.send(
            to=email,
            subject="请验证您的邮箱",
            body=f"请点击链接激活账号：/verify?token={token}",
        )
        return {"success": True, "token": token, "error": None}
```

运行测试 → **PASS** ✓，其他测试未受影响 ✓

**🔵 REFACTOR — 清理**

将 token 长度提取为常量，不增加任何功能：

```python
# services/email_verification.py
import secrets

TOKEN_BYTES = 32  # 生成 64 位 hex token

class EmailVerificationService:
    def __init__(self, email_service):
        self._email_service = email_service

    def send_verification_email(self, user_id: str, email: str) -> dict:
        token = secrets.token_hex(TOKEN_BYTES)
        self._email_service.send(
            to=email,
            subject="请验证您的邮箱",
            body=f"请点击链接激活账号：/verify?token={token}",
        )
        return {"success": True, "token": token, "error": None}
```

运行测试 → **PASS** ✓

**💾 COMMIT**

```
git commit -m "feat: implement S-01 send verification email on registration"
```

> 🤖 **Agent**：S-01 完成 ✅。进度：1/6。继续实现 **S-02** 吗？

> 👤 **用户**：继续。

> 🤖 **Agent**：[对 S-02 执行同样的 RED → GREEN → REFACTOR → COMMIT 循环，直至所有条目完成]

---

### 第五步：验收验证

所有条目（S-01 ~ S-04、E-01、E-02）完成后，Agent 执行验收清单：

- [x] 每个规范条目都有对应的自动化测试（6 个条目，6 个测试函数）
- [x] 所有测试通过，无警告
- [x] 回顾规范文档，确认无遗漏条目
- [x] 错误处理场景（E-01、E-02）均已覆盖
- [x] 每条 commit message 均引用对应规范条目编号

> 🤖 **Agent**：邮件验证功能全部实现完成 ✅。规范文档状态已更新为"已完成"。
>
> 最终规范文档路径：`docs/specs/2026-03-05-email-verification.md`
> 测试文件路径：`tests/test_email_verification.py`（6 个测试用例，全部通过）
