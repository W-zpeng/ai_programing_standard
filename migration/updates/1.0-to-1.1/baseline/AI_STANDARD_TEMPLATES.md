# AI_STANDARD_TEMPLATES

> AI 编程标准 1.0 专用规范，按 AI_STANDARD.md 的任务路由按需读取。

> 前置：ENTITIES、PROJECT；输出：文件模板。

## 第 7 部分：附录

### 7.1 项目实例文件模板

以下模板由引导协议生成，AI 维护，用户不直接编辑。

---

#### 7.1.1 `AI_ANCHOR.md` 模板

```markdown
# AI_ANCHOR

> 记忆锚点。压缩后必须重读本文件。
> 本文件必须极短，不超过 15 行有效内容。

标准版本：1.0
标准路径：.ai/AI_STANDARD.md
入口路径：.ai/AI_ENTRY.md
配置路径：.ai/PROJECT_PROFILE.md
索引路径：.ai/index.yaml

当前阶段：F0 项目启动
当前任务：无
当前缺陷：无
最近会话：无
最近交接：无
下一步：进入引导协议

已知阻塞：无
已知陷阱：无
```

规则：

- 本文件由 AI 维护，每次交接更新。

- 本文件不写详细内容，只写指针和当前状态。

- 本文件超长时，AI 必须精简。

---

#### 7.1.2 `AI_ENTRY.md` 模板

```markdown
# AI_ENTRY

> 详细入口。正常接手时读本文件。
> 第一动作：读 `.ai/AI_ANCHOR.md`。

## 项目使命

<一段话>

## 当前里程碑

<当前里程碑和状态>

## 必读顺序

1. `.ai/AI_ANCHOR.md`
2. `.ai/PROJECT_PROFILE.md`
3. `.ai/index.yaml`
4. `docs/current/ARCH.md`
5. `docs/current/INVARIANTS.md`
6. 当前 TASK/BUG/CHG
  
  ## 构建与验证
  

- 配置命令：`<从 PROJECT_PROFILE 引用>`
- 构建命令：`<从 PROJECT_PROFILE 引用>`
- 运行命令：`<从 PROJECT_PROFILE 引用>`
- 验证策略：`<从 PROJECT_PROFILE 引用>`
  
  ## 最近交接
  
  <最近一次交接摘要，或指向 SESSION>
  
  ## 当前已知陷阱
  

1. <陷阱>
  
  ## 下一步
  
2. <下一步>
```

规则：

- 本文件不写详细历史，只写当前状态和指针。

- 本文件由 AI 维护。

---

#### 7.1.3 `PROJECT_PROFILE.md` 模板

markdown

# PROJECT_PROFILE

> 项目配置。由引导协议生成，AI 维护，用户不直接编辑。
> 本文件不记录 AI 工具、模型、能力、插件信息。

```yaml
standard_version: "1.0"
project:
 name: ""
 goal: ""
 scope: ""
 maturity: undecided
 platforms: []
build:
 system: undecided
 configure_command: ""
 build_command: ""
 run_command: ""
 clean_command: ""
 notes: ""
verification:
 policy: undecided
 evidence_level: undecided
 methods: []
 notes: ""
docs:
 root: docs
 roles:
 current: docs/current
 work: docs/work
 records: docs/records
 evidence: docs/evidence
 audit_shard_period: monthly
 archive_after: 3m
 change_log_threshold: 50
entities:
 enabled: []
 disabled: []
gates:
 policy: soft
 require_confirmation: [L2, L3]
 allow_silent: [L0, L1]
 silent_default: false
visibility:
 receipt_granularity: changeset
 quiet_mode_default: false
 session_summary: true
```

规则：

- 未确定字段写 `undecided`，并创建 `OPEN`。
- 配置变化走 `CHG`。
- 不记录与项目无关的信息。

---

#### 7.1.4 `index.yaml` 模板

```yaml
standard_version: "1.0"
current_stage: F0
current_task: null
last_session: null
entities: {}
audit_shards: []
archive_shards: []
```

规则：

- 自动生成，AI 不手写。

- 实体变更时同步更新。

- 不存正文，不存详细历史。

---

### 7.2 实体模板

以下模板用于新建实体。每个实体文件以 frontmatter 开头，正文按模板展开。

---

#### 7.2.1 `REQ` 模板

```markdown
---

id: REQ-0001
type: req
status: draft
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# REQ-0001 <标题>

## 摘要

<一段话>

## 背景

<为什么需要>

## 目标

<做什么>

## 范围

<包含什么，不包含什么>

## 验收标准

1. <可验证标准>
  
  ## 开放问题
  

- OPEN-xxxx
  
  ## 假设
  
- ASM-xxxx
  
  ## 会话记录
  
  ## 关联
```

---

#### 7.2.2 `ARC` 模板

```markdown
---

id: ARC-0001
type: arc
status: provisional
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# ARC-0001 <标题>

## 摘要

<一段话>

## 分层

<系统如何分层>

## 依赖规则

1. <允许/禁止的依赖>
  
  ## 不变量
  
2. <必须始终成立的约束>
  
  ## 关键决策
  

- ADR-xxxx
  
  ## 会话记录
  
  ## 关联
```

---

#### 7.2.3 `ADR` 模板

```markdown
---

id: ADR-0001
type: adr
status: proposed
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# ADR-0001 <标题>

## 背景

<为什么需要决策>

## 决策

<选了什么>

## 替代方案

1. <方案，为什么没选>
  
  ## 后果
  
  <正面、负面、风险>
  
  ## 影响分析
  
  | 维度  | 影响  |
  | --- | --- |
  | 需求  |     |
  | 架构  |     |
  | 接口  |     |
  | 不变量 |     |
  | 测试  |     |
  | 迁移  |     |
  | 回滚  |     |
  
  ## 会话记录
  
  ## 关联
```

---

#### 7.2.4 `DES` 模板

```markdown
---

id: DES-0001
type: des
status: sketch
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# DES-0001 <标题>

## 摘要

<一段话>

## 模块职责

<做什么，不做什么>

## 内部结构

<关键类型、组件、数据流>

## 关键流程

<关键路径>

## 所有权与生命周期

<对象所有权、生命周期>

## 错误处理

<错误策略>

## 并发与线程

<线程模型、锁策略>

## 会话记录

## 关联
```

---

#### 7.2.5 `IF` 模板

```markdown
---

id: IF-0001
type: if
status: draft
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# IF-0001 <标题>

## 摘要

<一段话>

## 契约

<公开函数、类型、语义>

## 所有权

<参数、返回值所有权>

## 错误处理

<错误类型、传播方式>

## 线程模型

<线程安全保证>

## ABI

<兼容性、导出符号、PIMPL>

## 版本

<当前版本、兼容策略>

## 会话记录

## 关联
```

---

#### 7.2.6 `TASK` 模板

```markdown
---

id: TASK-0001
type: task
status: todo
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: [REQ-0001]
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# TASK-0001 <标题>

## 目标

<做什么>

## 步骤

1. <步骤>
  
  ## 依赖
  

- TASK-xxxx
- IF-xxxx
  
  ## 退出标准
  

1. <可验证标准>
  
  ## 证据
  

- VER-xxxx
  
  ## 会话记录
  
  ## 关联
```

---

#### 7.2.7 `BUG` 模板

```markdown
---

id: BUG-0001
type: bug
status: new
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# BUG-0001 <标题>

## 复现

1. <步骤>
  
  ## 环境
  
  <平台、编译器、版本>
  
  ## 期望
  
  <应该发生什么>
  
  ## 实际
  
  <实际发生什么>
  
  ## 根因
  
  <直接原因、根本原因、为什么没发现>
  
  ## 影响
  
  <影响范围>
  
  ## 修复
  
  <修复内容>
  
  ## 回归
  
  <回归证据>
  
  ## 会话记录
  
  ## 关联
```

---

#### 7.2.8 `CHG` 模板

```markdown
---

id: CHG-0001
type: chg
status: proposed
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# CHG-0001 <标题>

## 变更内容

<改什么>

## 原因

<为什么改>

## 影响矩阵

| 维度  | 影响  |
| --- | --- |
| 需求  |     |
| 架构  |     |
| 接口  |     |
| 不变量 |     |
| 测试  |     |
| 迁移  |     |
| 回滚  |     |

## 迁移

<如何迁移>

## 回滚

<如何回滚>

## 会话记录

## 关联
```

---

#### 7.2.9 `VER` 模板

```markdown
---

id: VER-0001
type: ver
status: planned
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# VER-0001 <标题>

## 验证目标

<验证什么>

## 方法

<如何验证>

## 证据等级

E0 / E1 / E2 / E3

## 结果

<通过/失败/不确定>

## 证据

<命令输出、日志、人工审查>

## 置信度

低 / 中 / 高

## 会话记录

## 关联
```

---

#### 7.2.10 `OPEN` 模板

```markdown
---

id: OPEN-0001
type: open
status: open
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# OPEN-0001 <标题>

## 问题

<还不知道什么>

## 影响

<不确定会影响什么>

## 再确认时机

<什么时候需要知道>

## 是否阻塞

是 / 否

## 会话记录

## 关联
```

---

#### 7.2.11 `ASM` 模板

```markdown
---

id: ASM-0001
type: asm
status: active
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# ASM-0001 <标题>

## 假设

<临时假设是什么>

## 依据

<为什么这样假设>

## 验证时机

<什么时候验证>

## 若不成立的影响

<如果不成立会怎样>

## 会话记录

## 关联
```

---

#### 7.2.12 `WAIVER` 模板

```markdown
---

id: WAIVER-0001
type: waiver
status: active
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# WAIVER-0001 <标题>

## 范围

<豁免覆盖什么>

## 规则

<豁免哪条规则>

## 原因

<为什么豁免>

## 影响

<有什么风险>

## 有效期

<到什么时候>

## 责任人

<谁负责复审>

## 会话记录

## 关联
```

---

#### 7.2.13 `INC` 模板

```markdown
---
id: INC-0001
type: inc
status: open
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
  derives_from: []
  implements: []
  depends_on: []
  affects: []
  verifies: []
  supersedes: []
  superseded_by: []
  blocks: []
  blocked_by: []
  related_to: []
  parent: null
  child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []
---

# INC-0001 <标题>

> 前置：ENTITIES、PROJECT；输出：文件模板。

## 时间与范围
## 影响
## 纠正措施
## 预防措施
## 会话记录
## 关联
```

#### 7.2.14 `OVR` 模板

```markdown
---
id: OVR-0001
type: ovr
status: active
created: <ISO8601>
updated: <ISO8601>
owners: [human, ai]
related:
  derives_from: []
  implements: []
  depends_on: []
  affects: []
  verifies: []
  supersedes: []
  superseded_by: []
  blocks: []
  blocked_by: []
  related_to: []
  parent: null
  child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []
---

# OVR-0001 <标题>

> 前置：ENTITIES、PROJECT；输出：文件模板。

## 覆盖对象
## 被覆盖规则或门禁
## 用户授权
## 范围与有效期
## 风险与待补事项
## 会话记录
## 关联
```

#### 7.2.15 `SESSION` 模板

```markdown
---

id: SESSION-0001
type: session
status: active
created: <ISO8601>
updated: <ISO8601>
owners: [ai]
related:
 derives_from: []
 implements: []
 depends_on: []
 affects: []
 verifies: []
 supersedes: []
 superseded_by: []
 blocks: []
 blocked_by: []
 related_to: []
 parent: null
 child: []
tags: []
last_changeset: null
last_modified_by: ai
last_modified_at: <ISO8601>
change_log: []

---

# SESSION-0001

## 接手声明

<见执行协议>

## 目标

<本次目标>

## 涉及实体

- TASK-xxxx
- BUG-xxxx
  
  ## 变更集
  
- CHGSET-xxxx
  
  ## 交接摘要
  
  <见执行协议>
  
  ## 阻塞和未决
  
  ## 关联
```

---

### 7.3 审计分片模板

```yaml
period: <period>
changesets: 0
first: null
last: null
entities_touched: []
levels: {L0: 0, L1: 0, L2: 0, L3: 0}
records: []
```

每条记录见 6.6.2。

---

### 7.4 写入声明与回执模板

见 6.4.1、6.5.1、6.7.2、6.7.3。

---

### 7.5 `docs/` 四个角色目录存放规则

| 目录               | 存放   | 示例                                                                   |
| ---------------- | ---- | -------------------------------------------------------------------- |
| `docs/current/`  | 当前真相 | `ARCH.md`、`INVARIANTS.md`、`IF-xxxx.md`、`DES-xxxx.md`                 |
| `docs/work/`     | 进行中  | `board.md`、`TASK-xxxx.md`、`BUG-xxxx.md`、`OPEN-xxxx.md`、`ASM-xxxx.md` |
| `docs/records/`  | 历史   | `adr/`、`chg/`、`sessions/`、`audit/`                                   |
| `docs/evidence/` | 验证证据 | `VER-xxxx.md`、`logs/`                                                |

规则：

- 禁止在角色目录之外创建实体文件。

- 禁止为单个实体创建子目录。

- 禁止按实体类型创建大量平级目录。

- 路径可由 `docs.roles` 修改，AI 必须按配置读写。

---

### 7.6 标准版本迁移

- 标准有版本号 `MAJOR.MINOR`。

- `MAJOR` 变化表示不兼容变更，需要迁移。

- `MINOR` 变化表示兼容增强，无需迁移。

- 迁移必须记录为 `CHG`。

- 迁移不删除历史，不覆盖旧记录。

- 旧项目可以继续使用旧版本，也可以迁移。

- AI 接手时先确认标准版本，再按对应版本执行。

---

### 7.7 标准落地检查清单

项目完成配置后，AI 应确认以下项：


- `.ai/AI_STANDARD.md` 已就位

- `.ai/AI_ANCHOR.md` 已生成

- `.ai/AI_ENTRY.md` 已生成

- `.ai/PROJECT_PROFILE.md` 已生成

- `.ai/index.yaml` 已生成

- `docs/current/`、`docs/work/`、`docs/records/`、`docs/evidence/` 已创建

- `PROJECT_PROFILE` 关键字段已定或标 `OPEN`

- 至少一个 `REQ` 已创建

- 至少一个 `TASK` 已创建

- `AI_ANCHOR.md` 已更新当前阶段

- 已完成已有代码检测并记录




