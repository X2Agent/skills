# Python 编码规范参考手册

## 官方参考来源

以下规范均直接源自官方权威文档，所有条目均可在原始文档中找到对应依据：

| 规范领域 | 官方文档 URL |
|----------|-------------|
| PEP 8 — 代码风格指南 | <https://peps.python.org/pep-0008/> |
| PEP 257 — 文档字符串约定 | <https://peps.python.org/pep-0257/> |
| PEP 484 — 类型注解 | <https://peps.python.org/pep-0484/> |
| Django 编码风格 | <https://docs.djangoproject.com/en/stable/internals/contributing/writing-code/coding-style/> |
| FastAPI 文档 | <https://fastapi.tiangolo.com/> |
| Flask 文档 | <https://flask.palletsprojects.com/> |

---

## 命名规范

- **模块、包**：全小写加下划线（snake_case），例：`user_service.py`、`data_models`
- **类**：PascalCase，例：`UserProfile`、`OrderManager`
- **函数、方法、变量**：snake_case，例：`get_user_by_id()`、`user_count`
- **常量**：全大写加下划线，例：`MAX_RETRY_COUNT`、`DEFAULT_TIMEOUT`
- **私有属性/方法**：单下划线前缀 `_`，例：`_cache`、`_validate()`
- **名称修饰（禁止外部访问）**：双下划线前缀 `__`，例：`__secret_key`
- **魔术方法**：双下划线包围，例：`__init__`、`__str__`
- **类型变量**：PascalCase 或单个大写字母，例：`T`、`KT`、`VT`

---

## 代码格式规范（PEP 8）

- 缩进使用 4 个空格（不使用 Tab）
- 每行不超过 79 个字符（注释/文档字符串不超过 72 个字符）
- 顶级定义之间空 2 行，类内方法之间空 1 行
- `import` 排序：标准库 → 第三方库 → 本地包，各组之间空 1 行
- 每条 `import` 语句只导入一个模块（`import os, sys` 不合规）
- 禁止通配符导入（`from module import *`）
- 运算符两侧、逗号后、冒号后有空格；函数调用括号内侧无空格
- 列表/字典/集合末尾可加尾逗号（trailing comma）

---

## 注释和文档规范（PEP 257）

- 公共模块、类、函数、方法必须有 docstring
- 单行 docstring：`"""返回用户对象。"""`（首尾同行）
- 多行 docstring：摘要行、空行、详细描述，结尾 `"""` 独占一行
- 同一项目内 docstring 风格保持一致（Google 风格或 NumPy 风格）
- 行内注释与代码至少间隔 2 个空格，以 `# ` 开头

---

## 类型注解规范（PEP 484）

- 函数参数和返回值应有类型注解
- 变量注解在赋值时使用，例：`name: str = "Alice"`
- 可为 `None` 的值：`Optional[T]`（Python 3.9 以下）或 `T | None`（Python 3.10+）
- 集合类型：`list[T]`、`dict[K, V]`（Python 3.9+）或 `List[T]`、`Dict[K, V]`（需从 `typing` 导入）

---

## Django 框架规范

- 视图放在 `views.py`，URL 配置放在 `urls.py`，模型放在 `models.py`
- 模型类名使用单数 PascalCase，例：`UserProfile`（不用 `UserProfiles`）
- 模型字段名使用 snake_case
- 视图类继承适当的通用视图（`ListView`、`DetailView`、`CreateView` 等）
- URL 命名使用 app_name 命名空间，例：`app_name = 'users'`
- 模板标签和过滤器放在 `templatetags/` 目录
- 使用 `get_object_or_404` 代替手动处理 `DoesNotExist` 异常
- 表单类以 `Form` 结尾，例：`UserRegistrationForm`
- 序列化器类以 `Serializer` 结尾（DRF），例：`UserSerializer`

---

## FastAPI 框架规范

- 路由函数使用 `async def`（I/O 密集）或 `def`（CPU 密集）
- 路径操作装饰器与 HTTP 语义一致：`@app.get`、`@app.post`、`@app.put`、`@app.delete`
- 请求体模型继承 `pydantic.BaseModel`，字段名使用 snake_case
- 响应模型通过 `response_model` 参数声明
- 依赖注入使用 `Depends()`
- 路由器使用 `APIRouter` 并通过 `app.include_router()` 注册
- 状态码使用 `status` 模块常量，例：`status.HTTP_201_CREATED`

---

## Flask 框架规范

- 使用应用工厂模式（`create_app()` 函数），而非全局 `app` 实例
- 蓝图（Blueprint）名称使用小写，例：`auth_bp = Blueprint('auth', __name__)`
- 视图函数名称与路由路径语义一致
- 配置使用类继承方式，例：`class ProductionConfig(Config):`
- 使用 `current_app` 和 `g`，而非直接引用全局变量
