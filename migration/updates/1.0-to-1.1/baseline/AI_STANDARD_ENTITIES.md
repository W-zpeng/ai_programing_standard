# AI_STANDARD_ENTITIES

> AI 编程标准 1.0 专用规范，按 AI_STANDARD.md 的任务路由按需读取。

> 前置：CORE；输出：实体语义和结构。

## 第 2 部分：实体模型、关系、状态、ID 规则、frontmatter 规范

实体字段和验证要求按项目适用性决定。模板提供通用结构，项目可在需求分析阶段为适用的实体增加具体约束；不适用的技术属性不应强行填入。

### 2.1 实体总览

标准定义 14 类实体。每类实体对应一种信息角色，不是每个项目都要启用全部。

| 实体   | 前缀        | 回答什么             | 是否活文档 |
| ---- | --------- | ---------------- | ----- |
| 需求   | `REQ`     | 做什么、为什么、验收标准     | 是     |
| 架构   | `ARC`     | 系统如何分层、依赖规则、不变量  | 是     |
| 决策   | `ADR`     | 为什么这样选，替代方案，后果   | 否，追加  |
| 设计   | `DES`     | 模块内部如何实现         | 是     |
| 接口   | `IF`      | 对外契约、ABI、头文件契约   | 是     |
| 任务   | `TASK`    | 当前要做什么，做到哪       | 是     |
| 缺陷   | `BUG`     | 复现、根因、修复、回归      | 混合    |
| 变更   | `CHG`     | 改什么、影响谁、怎么迁移     | 否，追加  |
| 验证   | `VER`     | 如何验证，证据是什么       | 是     |
| 发布   | `REL`     | 发布什么、兼容性、迁移      | 否，追加  |
| 开放问题 | `OPEN`    | 还不知道什么，何时需要知道    | 是     |
| 假设   | `ASM`     | 临时假设是什么，何时验证     | 是     |
| 豁免   | `WAIVER`  | 偏离标准的原因、范围、有效期   | 否，追加  |
| 会话   | `SESSION` | 一次对话里发生了什么，交接了什么 | 否，追加  |
| 违规事件 | `INC` | 协议违规的时间、范围、影响、纠正、预防 | 否，追加 |
| 覆盖 | `OVR` | 用户强制覆盖门禁或执行决策 | 否，追加 |

启用哪些实体由 `PROJECT_PROFILE.md` 决定。未启用的实体，AI 不得创建。

---

### 2.2 实体之间的关系

关系是双向可查的。每个实体在 frontmatter 中声明它指向别人的关系，反向关系由 `index.yaml` 自动维护。

| 关系                 | 含义     | 示例                                  |
| ------------------ | ------ | ----------------------------------- |
| `derives_from`     | 由某实体派生 | `TASK` derives_from `REQ`           |
| `implements`       | 实现某实体  | `DES` implements `ARC`              |
| `depends_on`       | 依赖某实体  | `TASK` depends_on `IF`              |
| `affects`          | 影响某实体  | `CHG` affects `IF`                  |
| `verifies`         | 验证某实体  | `VER` verifies `BUG`                |
| `supersedes`       | 取代某实体  | `ADR-0002` supersedes `ADR-0001`    |
| `superseded_by`    | 被某实体取代 | `ADR-0001` superseded_by `ADR-0002` |
| `blocks`           | 阻塞某实体  | `OPEN-0003` blocks `TASK-0007`      |
| `blocked_by`       | 被某实体阻塞 | `TASK-0007` blocked_by `OPEN-0003`  |
| `related_to`       | 一般关联   | `BUG-0042` related_to `IF-0007`     |
| `parent` / `child` | 层级关系   | `TASK-0007` parent `TASK-0005`      |

规则：

- 关系只能通过 ID 引用。
- 禁止自由文本引用。
- 反向关系由 `index.yaml` 自动生成，AI 不手写。
- 关系变化视为结构性变更，级别至少 L2。

---

### 2.3 实体状态机

每类实体有允许状态集合和允许转移。禁止跳状态而不记录原因。

| 实体        | 状态集合                                                                              | 终态                                 |
| --------- | --------------------------------------------------------------------------------- | ---------------------------------- |
| `REQ`     | `draft → exploring → provisional → accepted → stable → deprecated → archived`     | `archived`                         |
| `ARC`     | `provisional → stable → deprecated → archived`                                    | `archived`                         |
| `ADR`     | `proposed → accepted → superseded / deprecated`                                   | `superseded/deprecated`            |
| `DES`     | `sketch → refined → stable → deprecated → archived`                               | `archived`                         |
| `IF`      | `draft → provisional → stable → deprecated → archived`                            | `archived`                         |
| `TASK`    | `todo → doing → blocked → doing / review → doing / done → verified / cancelled`                   | `verified/cancelled`               |
| `BUG`     | `new → reproduced → root_caused → fixing → fixed → verified → reopened → fixing / closed / rejected`  | `closed/rejected`                  |
| `CHG`     | `proposed → approved → in_progress → applied → verified / rolled_back / rejected` | `verified/rolled_back/rejected`    |
| `VER`     | `planned → executed → passed / failed / inconclusive`                             | `passed/failed/inconclusive`       |
| `REL`     | `planned → prepared → released → archived`                                        | `archived`                         |
| `OPEN`    | `open → answered → closed`                                                        | `closed`                           |
| `ASM`     | `active → validated → invalidated → superseded`                                   | `validated/invalidated/superseded` |
| `WAIVER`  | `active → expired → revoked`                                                      | `expired/revoked`                  |
| `SESSION` | `active → closed`                                                                 | `closed`                           |
| `INC` | `open → investigating → resolved → closed` | `closed` |
| `OVR` | `active → expired` 或 `active → revoked` | `expired/revoked` |

规则：

- 状态转移必须记录时间、原因、变更集 ID。
- 跳过状态必须在该实体的 Session 中说明原因。
- 终态实体不再修改内容，只能新增关系或标记 `superseded_by`。
- 终态实体不删除。

---

### 2.4 ID 规则

格式：

```text
<前缀>-<四位数字>
```

示例：

```text
REQ-0001
ARC-0001
ADR-0001
DES-0001
IF-0001
TASK-0001
BUG-0001
CHG-0001
VER-0001
REL-0001
OPEN-0001
ASM-0001
WAIVER-0001
SESSION-0001
```

规则：

- ID 一旦分配，永不改变，永不复用。

- 编号按实体类型独立递增，不跨类型共享。

- 超过 9999 时扩展为五位，但已有 ID 不变。

- 代码注释、提交信息、文档、审计、回执中引用实体必须用 ID。

- 禁止用文件名代替 ID 引用。

- 禁止用自然语言描述代替 ID 引用。

- `CHGSET` 是变更集标识，不是实体，格式见 6.3.2，不受本节实体 ID 规则约束。

---

### 2.5 frontmatter 通用规范

每个实体文件以 YAML frontmatter 开头。通用字段如下：

```yaml
---
id: BUG-0042
type: bug
status: fixing
created: 2026-09-01T10:00:00+08:00
updated: 2026-09-14T15:00:00+08:00
owners: [human, ai]
related:
  implements: []
  depends_on: [IF-0007]
  affects: []
  verifies: []
  supersedes: []
  superseded_by: []
  blocks: []
  blocked_by: []
  related_to: [TASK-0101]
  parent: null
  child: []
tags: [cpp, memory, concurrency]
last_changeset: CHGSET-2026-09-14-009
last_modified_by: ai
last_modified_at: 2026-09-14T17:20:00+08:00
change_log:
  - changeset: CHGSET-2026-09-01-001
    at: 2026-09-01T10:00:00+08:00
    action: created
    summary: "初始报告"
  - changeset: CHGSET-2026-09-14-007
    at: 2026-09-14T15:00:00+08:00
    action: status_changed
    summary: "reproduced → fixing"
  - changeset: CHGSET-2026-09-14-009
    at: 2026-09-14T17:20:00+08:00
    action: fixed
    summary: "根因修复，追加 Session-3"
---
```

字段说明：

| 字段                 | 必填  | 说明                   |
| ------------------ | --- | -------------------- |
| `id`               | 是   | 实体 ID                |
| `type`             | 是   | 实体类型，小写              |
| `status`           | 是   | 当前状态                 |
| `created`          | 是   | 创建时间，ISO 8601 带时区    |
| `updated`          | 是   | 最后更新时间               |
| `owners`           | 是   | 责任方，至少一个             |
| `related`          | 是   | 关系集合，未使用的键留空数组或 null |
| `tags`             | 否   | 自由标签，用于检索            |
| `last_changeset`   | 是   | 最后一次变更集 ID           |
| `last_modified_by` | 是   | `human` 或 `ai`       |
| `last_modified_at` | 是   | 最后修改时间               |
| `change_log`       | 是   | 本实体的内嵌历史，见 2.6       |

规则：

- frontmatter 必须是合法 YAML。

- 时间必须带时区。

- 未使用的字段必须保留并留空，不得删除键。

- 禁止在 frontmatter 中写自然语言长段落。

- 正文承担详细内容，frontmatter 只承担结构。

---

### 2.6 `change_log` 规范

`change_log` 是实体的内嵌历史，是 L1 历史层。它只记录与该实体直接相关的变更摘要。

每条记录字段：

| 字段          | 必填  | 说明              |
| ----------- | --- | --------------- |
| `changeset` | 是   | 变更集 ID，指向 L2 审计 |
| `at`        | 是   | 时间              |
| `action`    | 是   | 动作类型，见下表        |
| `summary`   | 是   | 一句话摘要           |

动作类型：

| 动作                 | 含义   |
| ------------------ | ---- |
| `created`          | 创建   |
| `status_changed`   | 状态变化 |
| `content_updated`  | 内容更新 |
| `relation_changed` | 关系变化 |
| `superseded`       | 被取代  |
| `deprecated`       | 废弃   |
| `archived`         | 归档   |
| `session_appended` | 追加会话 |
| `rolled_back`      | 回滚   |

规则：

- `change_log` 只追加，不删除，不修改历史条目。

- 每条记录必须指向一个已存在的变更集。

- 详细内容在 L2 审计分片，`change_log` 只存摘要。

- 查询实体历史时，先读 `change_log`，再按需读 L2。

- `change_log` 条目数超过阈值（默认 50）时，旧条目移入该实体对应的 `history.yaml`，`change_log` 保留最近 N 条和指针。

---

### 2.7 实体文件正文结构

每个实体文件正文建议遵循统一结构：

```markdown
# <ID> <标题>

## 摘要
一段话说明本实体是什么。

## 详情
按实体类型展开。例如：
- REQ：背景、目标、验收标准
- ARC：分层、依赖规则、不变量
- ADR：背景、决策、替代方案、后果
- DES：模块职责、内部结构、关键流程
- IF：契约、所有权、错误处理、线程模型、ABI
- TASK：目标、步骤、依赖、退出标准
- BUG：复现、环境、根因、修复、回归
- CHG：变更内容、影响矩阵、迁移、回滚

## 会话记录
按时间追加。每条会话：
### Session-N · <时间> · <changeset>
- 目标
- 操作
- 证据
- 结论
- 下一步

## 关联
列出相关实体 ID 和一句话说明。
```

规则：

- 正文可以长，frontmatter 必须短。

- 会话记录只追加，不删除。

- 正文中的关键结论必须回写 `change_log` 摘要。

- 正文不得替代 frontmatter 的结构字段。

---




### 实体依赖与适用性

- 实体依赖由项目目标和实施计划决定，不要求固定线性顺序；实现前必须记录当前批次所需的前置实体和不确定项。
- 从完成、验证或审查状态返回工作状态时，必须记录新证据、新问题或新需求，并追加新的 SESSION 和 CHGSET。

### 条件式实体依赖

- 实体依赖不采用所有项目统一的固定线性顺序，但必须按实体类型和项目目标建立条件式依赖：实现型 TASK 必须有目标来源；影响对外契约的变更必须关联 IF 或明确决策；进入 done 必须满足退出标准；进入 verified 必须有验证证据。

### 依赖不满足时的处理

- 缺少目标来源时不得进入 `doing`。
- 缺少接口或决策时创建 `OPEN-xxxx`，必要时进入 `blocked`。
- 缺少退出标准时不得进入 `done`。
- 缺少验证证据时不得进入 `verified`。
- 用户要求继续时必须创建 `OVR-xxxx`。
- 同一验证目标和方法的重试追加原 `VER` 的 Session；方法或目标变化时创建新的 `VER` 并关联原验证。


### 条件依赖的目标来源

- 实现型 TASK 的目标来源可以是 REQ、BUG、CHG、已解决 OPEN、维护任务或用户明确授权，不限定为 REQ。
- OVR 必须绑定具体实体、变更集、操作范围和有效期限；新的任务、批次或变更集不得自动继承旧 OVR。

### 依赖未满足

- 依赖关系应记录在适用实体中；发现依赖未满足时，不得猜测或擅自绕过。
- AI 必须说明依赖对象、阻塞原因和可选处理方式，由用户或指定责任方决定后续方案。



