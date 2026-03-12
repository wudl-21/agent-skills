# Copilot Agent Skills

🌐 [English](README.md) | **中文**

VS Code GitHub Copilot Agent Skills 合集，涵盖文档处理、提交规范辅助与游戏 Mod 制作。

Agent Skills 遵循[开放 Agent Skills 标准](https://agentskills.io)，可被 VS Code 中的 GitHub Copilot 自动发现。每个 Skill 都是一个文件夹，其中包含一个 `SKILL.md` 文件，用于告诉 Copilot 该 Skill 的功能及使用方式。

本仓库按开放 Agent Skills 格式编写，并以 VS Code 中的 GitHub Copilot 作为主要测试宿主。

## 速览

| Skill | 斜杠命令 | 描述 |
|---|---|---|
| [`conventional-commits`](#conventional-commits) | `/conventional-commits` | 按 Conventional Commits 1.0.0 规范生成、校验、规范化、分类并解释 Git 提交信息 |
| [`knowledge-optimizer`](#knowledge-optimizer) | `/knowledge-optimizer` | 将原始文档（HTML、XML、冗长 Markdown）转换为高密度、结构化 Markdown，适用于 AI 知识库 |
| [`teardown-scripting`](#teardown-scripting) | `/teardown-scripting` | 为 Teardown 游戏引擎生成、调试和修改 Lua 5.1 脚本（单人 & 多人模式） |

## 安装

选择以下任意一种方式：

### 个人使用（推荐）

将本仓库克隆到标准个人 Skills 目录，Copilot 会自动发现所有 Skill。

**Windows**
```powershell
git clone https://github.com/wudl-21/agent-skills.git "$env:USERPROFILE\.copilot\skills"
```

**macOS / Linux**
```bash
git clone https://github.com/wudl-21/agent-skills.git ~/.copilot/skills
```

> 如果该目录下已有其他文件，请先克隆到子目录，再手动将所需 Skill 文件夹复制过去。

### 项目级使用

将需要的 Skill 文件夹复制到项目的 `.github/skills/` 目录：

```bash
cp -r conventional-commits /path/to/your-project/.github/skills/
cp -r knowledge-optimizer /path/to/your-project/.github/skills/
```

### 自定义路径

如果 Skills 存储在非标准路径，可在 VS Code 设置中添加：

```json
// .vscode/settings.json 或用户设置
{
  "chat.agentSkillsLocations": ["/your/custom/path/to/skills"]
}
```

## 使用方式

### 自动调用

在 Copilot Chat 中用自然语言描述任务。兼容 Skill 的客户端会根据各 Skill 的 `description` 字段决定是否加载它，因此任务表述越清晰，匹配通常越稳定。

> *"将这段 HTML API 参考文档转换为干净的知识库 Markdown 文件。"*  
> *"把这条提交信息改写成 Conventional Commits 格式，并告诉我它是否属于 breaking change。"*  
> *"写一个 Teardown 多人模式 Mod，让玩家击杀后获得速度加成。"*

### 手动调用

在 Copilot Chat 中输入斜杠命令以显式调用 Skill：

```
/conventional-commits
/knowledge-optimizer
/teardown-scripting
```

---

## Skill 详情

### conventional-commits

**用途：** 按 Conventional Commits 1.0.0 规范生成、校验、规范化、分类并解释 Git 提交信息。

**架构设计**
- 主 `SKILL.md` 保持精简，专注任务路由和执行规则
- 使用较小的 `_INDEX.md` 作为结构说明和入口
- 将细节拆分到独立参考文件：
  - `spec.md`：规范语法、解析、校验与 SemVer 规则
  - `faq.md`：策略判断、歧义处理与工作流边界情况

**支持内容**
- 按标准格式撰写或改写提交信息：`<type>[optional scope][optional !]: <description>`
- 校验 header、body、footer 和 breaking-change 标记
- 推断 SemVer 版本变更级别：
  - `!` 或 `BREAKING CHANGE` 为 `MAJOR`
  - `feat` 为 `MINOR`
  - `fix` 为 `PATCH`
- 解释提交各组成部分、footer 解析规则及常见无效写法

**示例提示词**
```
/conventional-commits 把这句话改写成 Conventional Commit：updated auth and removed the legacy login path
```
```
/conventional-commits `feat(api)!: remove v1 endpoints` 合规吗？它对应什么版本升级？
```

---

### knowledge-optimizer

**用途：** 将杂乱的、面向人类阅读的文档，转化为高信息密度的本地知识库，针对 LLM 和 Agent 消费进行优化。

**接受的输入格式**
- 原始 HTML / XML（直接粘贴或提供文件内容）
- PDF 提取文本
- 冗长或结构混乱的 Markdown

**输出内容**
- 去噪后的 Markdown，移除所有 HTML/XML 标签、广告和填充文字
- 标准化标题规范，便于 Agent 进行 regex 或 grep 搜索：
  - `### [API] FunctionName(args)` — 函数/方法
  - `### [Event] EventName(args)` — 事件/回调
  - `### [Class] ClassName` — 类/模块
  - `### [Concept] ConceptName` — 核心概念
- 将长段落压缩为简洁的要点列表
- 代码块中包含内联注释以提供上下文
- `> **[CONSTRAINTS]**` 块，突出显示已知陷阱和版本限制
- 对于大型文档：拆分为多个文件 + 生成 `_INDEX.md` 导航文件

**示例提示词**
```
/knowledge-optimizer 以下是 Stripe API 参考的原始 HTML，请将其转换为结构化知识库。
```
```
/knowledge-optimizer 清理这份冗长的 Markdown 文档，并将其优化为适合 Agent 使用的知识库。
```

---

### teardown-scripting

**用途：** 为 [Teardown](https://teardowngame.com) 游戏引擎生成、调试和修改 Lua 5.1 脚本，涵盖单人和多人 Mod。

**核心行为**
- 始终先确认目标模式 —— **单人（`sp`）** 和 **多人（`mp`）** 的 API 互不兼容，如未指定，Skill 会主动询问。
- 从内置的顶层索引开始，再按需跟进 `docs/` 下的相关参考资料：
  - **SP（单人）：** 23 个 API 章节文件 + 脚本技巧
  - **MP（多人）：** 25 个 API 章节文件 + mplib 模块库 + 脚本技巧
- 严格遵守 Lua 5.1 规则（1-based 索引、`--` 注释、不使用位运算符）

**多人模式自动强制规则**
- 每个 MP 脚本顶部必须有 `#version 2`
- 逻辑分离到 `server.*` 和 `client.*` 表中
- 无 `update(dt)` 回调（MP 只有 `tick(dt)`）
- 在适用场景下使用 mplib 模块（hud、stats、teams、spawn 等）

**示例提示词**
```
/teardown-scripting 单人 Mod：按下 E 键时，将玩家 5 米范围内的所有 Shape 涂成红色。
```
```
/teardown-scripting 多人 Mod：追踪每位玩家的击杀数，并在屏幕上显示排行榜。
```

---

## 兼容性与校验

- 目标格式：开放 Agent Skills 标准
- 主要测试宿主：VS Code 中的 GitHub Copilot
- 外部运行时要求：Skill 加载本身无额外要求；但 `teardown-scripting` 生成的代码在执行时仍依赖 Teardown 的实际运行时与 Mod 格式

可使用参考校验器验证各个 Skill：

```bash
skills-ref validate ./conventional-commits
skills-ref validate ./knowledge-optimizer
skills-ref validate ./teardown-scripting
```

## 适用范围与限制

- `conventional-commits` 依据 Conventional Commits 1.0.0 规范和通用最佳实践工作；如果仓库自身定义了额外的 type、scope 或发布规则，应以仓库策略为准。
- `knowledge-optimizer` 负责重构你提供或在上下文中暴露的文档，不会把未被文档支持的内容包装成权威事实。
- `teardown-scripting` 依赖本仓库内置的 Markdown 知识库。如果 Teardown 的 API 或多人模式行为发生变化，生成结果仍应结合当前游戏版本和运行时表现进行核对。

---

## 许可证

MIT — 详见 [LICENSE](LICENSE)。
