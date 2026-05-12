# Claude 记忆架构设计

> 2026-05-11 对话中演化

## 从"两层查找"到"三层模型"

### 最初状态

- Memory（`raw/memory/`）：存行为调教信息
- 知识库：存用户的普通笔记
- 查询逻辑：先查 memory → 没有才查知识库

### 关键转折

**"知识库"改名为"数据库"** — 里面存的不只是"知识"，而是关于用户的所有数据。

**目录从 `knowledge` 改名为 `huochao`** — 从"知识库"变成"霍超的个人数据库"。

**"数据库就是我的记忆"** — 数据库不是外挂资料库，而是 Claude 自己的记忆。

### 最终架构（三层模型）

| 层 | 位置 | 角色 |
|---|---|---|
| memory（启动态） | `D:\Claude\memory\` | 自动加载，行为指令和现场感 |
| Obsidian（知识态） | `D:\Obsidian\huochao\` | 持久存储，需要时主动查询 |
| 训练知识 | — | 通用兜底，编程/推理/常识 |

### 查询决策树

```
用户问题 → memory 有答案？→ 直接用
         → memory 不够？→ 翻 Obsidian（目录 + 关键词搜索）
         → 都没有？→ 用训练知识
```

### memory 保留标准

**是不是每次对话都需要它来让工作正常进行？**
- 是 → memory（行为指令/当前上下文）
- 否 → Obsidian（需要时才查阅）

### 迁移结果（2026-05-11）

- memory：23 → 8 个文件（纯行为指令）
- Obsidian：35 → 36 个文件（补写 cyber-diary）
- 15 个知识参考文件从 memory 删除

## 演进（2026-05-12）

### user-profile 分层

user-profile.md 从 95 行全量档案拆为两层：

- **memory 摘要** — 身份、沟通风格、思维模式的最小轮廓，稳定不变
- **Obsidian 详版** — 工具路径、投资、软件版本等常变数据

原则：一份文件同时包含行为层和事实层时，memory 保留行为核心，详情放 Obsidian。两处都由 Claude 维护，不需要用户手动同步。

### model-switching-strategy 迁移

与 Claude Code 无关的外部模型策略从 memory 移入 Obsidian（`references/model-switching-strategy.md`）。

## 相关

- [[../profile/full-profile]]
- [[knowledge-base-sync]]
- [[../index]]
