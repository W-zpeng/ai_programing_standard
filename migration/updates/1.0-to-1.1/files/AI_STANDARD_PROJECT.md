# AI_STANDARD_PROJECT

> AI 编程标准 1.1 专用规范，按 AI_STANDARD.md 的任务路由按需读取。

> 前置：CORE、ENTITIES；输出：项目配置和拓扑。

## 第 3 部分：项目配置模型、文档拓扑、变更分级、软门禁与豁免

> 经验入口与详情属于可选项目辅助活文档，放置、链接和来源规则统一见 `AI_STANDARD_LEARNING.md` 第 2 节；不新增实体类型，不改变历史记录权威性。

项目配置只定义当前项目采用的规则和能力。技术栈特有的约束由需求分析阶段判断后，记录到适用实体中，不得未经分析写成全局标准。

### 3.1 项目配置模型

项目配置存放在 `.ai/PROJECT_PROFILE.md`，由引导协议生成，AI 维护，通常由 AI 按用户决定维护。
配置只描述**项目本身**，不记录任何 AI 工具、模型、能力、插件、IDE 信息。

#### 3.1.1 配置字段

```yaml
standard_version: "1.1"

project:
  name: ""
  goal: ""
  scope: ""
  maturity: greenfield | evolving | maintenance
  platforms: []

build:
  system: cmake | bazel | make | msbuild | custom | none | undecided
  configure_command: ""
  build_command: ""
  run_command: ""
  clean_command: ""
  notes: ""

verification:
  policy: none | manual | build | test | full
  evidence_level: E0 | E1 | E2 | E3
  methods: []
  notes: ""

docs:
  root: docs
  roles:
    current: docs/current
    work: docs/work
    records: docs/records
    evidence: docs/evidence
  audit_shard_period: monthly | quarterly | yearly
  archive_after: 3m
  change_log_threshold: 50

entities:
  enabled: [REQ, ARC, ADR, DES, IF, TASK, BUG, CHG, VER, OPEN, ASM, SESSION, INC, OVR]
  disabled: [REL, WAIVER]

gates:
  policy: soft | human | optional_ci
  require_confirmation: [L2, L3]
  allow_silent: [L0, L1]
  silent_default: false

visibility:
  receipt_granularity: changeset | file | task
  quiet_mode_default: false
  session_summary: true
```

#### 3.1.2 字段规则

| 字段                            | 说明               |
| ----------------------------- | ---------------- |
| `standard_version`            | 所用标准版本           |
| `project.maturity`            | 决定引导深度和默认启用实体    |
| `build.system`                | 用户指定，标准不假设       |
| `build.*_command`             | 可由 AI 检测并建议，按用户授权采用       |
| `verification.policy`         | 决定验证强度，可为 `none` |
| `verification.evidence_level` | 见 3.5            |
| `docs.roles`                  | 逻辑角色到物理路径的映射     |
| `entities.enabled`            | 启用哪些实体，未启用不得创建   |
| `gates.policy`                | 门禁强度，默认 `soft`   |
| `gates.allow_silent`          | 允许静默的级别，默认 L0/L1 |
| `visibility.*`                | 可见性偏好            |

规则：

- 配置中未确定的字段保留 undecided；需要持续追踪、影响行动或需复审时才创建或关联 OPEN。只澄清当前动作所需配置。

- 已建立配置的变化走 CHG 或已确认的等价记录；首次初始化先确认必要字段和实体启用范围，在首个 CHGSET 中登记，不要求创建配置前已经存在 CHG。

- 配置不记录工具、模型、插件。

- 配置不记录用户个人信息。

- 配置不记录与项目无关的元数据。

---

### 3.2 文档拓扑

标准定义**逻辑角色**，不强制物理路径。项目通过 `docs.roles` 映射。

#### 3.2.1 四个逻辑角色

| 角色         | 职责                       | 是否活文档 |
| ---------- | ------------------------ | ----- |
| `current`  | 当前真相：架构、不变量、接口、设计基线      | 是     |
| `work`     | 进行中：TASK、BUG、看板、OPEN、ASM | 是     |
| `records`  | 历史：ADR、CHG、SESSION、AUDIT | 否，追加  |
| `evidence` | 验证证据：命令输出、日志、人工审查        | 是     |

规则：

- 当前真相与历史记录严格分离。

- 已确定设计/决策的变更关联 CHG 或 ADR（按启用配置处理）；USER_INTENT 和经验指导等辅助当前文档关联相关实体、SESSION 和 CHGSET，不强制每次探索都创建变更实体。

- records 中历史条目只追加，受控纠错按 AUDIT；其中 CHG/SESSION 等实体的当前状态可按状态机更新并留痕，不能因此重写旧决定。

- `evidence` 可追加，可标记失效，不删除。

- 每个实体一个文件，文件名带 ID。

#### 3.2.2 默认物理结构

```text
项目根/
├── .ai/
│   ├── AI_STANDARD.md
│   ├── AI_STANDARD_*.md
│   ├── STANDARD_RELEASE.yaml
│   ├── RELEASE_NOTES.md
│   ├── migration/（更新材料，项目执行时只读）
│   ├── AI_ANCHOR.md
│   ├── AI_ENTRY.md
│   ├── PROJECT_PROFILE.md
│   └── index.yaml
└── docs/
    ├── current/
    │   ├── ARCH.md
    │   ├── INVARIANTS.md
    │   ├── IF-0001.md
    │   └── DES-0001.md
    ├── work/
    │   ├── board.md
    │   ├── TASK-0001.md
    │   ├── BUG-0042.md
    │   ├── OPEN-0003.md
    │   └── ASM-0001.md
    ├── records/
    │   ├── adr/
    │   │   └── ADR-0001.md
    │   ├── chg/
    │   │   └── CHG-0001.md
    │   ├── sessions/
    │   │   ├── SESSION-0012.md
    │   │   └── SESSION-0013.md
    │   └── audit/
    │       ├── 2026-09.yaml
    │       ├── 2026-10.yaml
    │       └── archive/
    │           └── 2026-Q2.yaml
    └── evidence/
        ├── VER-0001.md
        └── logs/
```

规则：

- 路径可由 `docs.roles` 修改，AI 必须按配置读写。

- `INC` 实体存放于 `docs/records/incidents/`，`OVR` 实体存放于 `docs/records/overrides/`；两者与其他实体一样受 `entities.enabled` 控制。

- 上述目录为 `records` 角色目录下的默认子目录；若 `docs.roles.records` 被配置修改，必须随配置迁移。

- 若协议要求创建 `INC` 或 `OVR` 而项目未启用对应实体，必须记录配置冲突，不得静默跳过。

- `history.yaml` 为实体的历史辅助文件，与实体同目录，命名为 `<ID>.history.yaml`，不是独立实体，也不纳入 `index.yaml` 实体索引。


- ARCH、INVARIANTS、board 为派生视图，不是实体。分别从项目启用的架构/设计来源、其中明确的不变量、TASK 聚合；无 ARC/DES 时可使用已确认 REQ/CHG 等来源并列出来源 ID，不虚构或自动启用实体；无适用来源时不创建空聚合。修改源记录后同步视图。

- 聚合失败不改变实体源数据，必须记录验证失败或创建 `INC-xxxx`，恢复后重新生成。

- `board.md` 至少展示 TASK ID、标题、状态、优先级、阻塞原因、依赖和最近更新时间。

- 禁止在角色目录之外创建实体文件。

- 禁止为单个实体创建子目录。

- 禁止按实体类型创建大量平级目录。

#### 3.2.3 三层历史结构

历史查询不能遍历全库。分三层：

| 层        | 位置                                      | 回答什么             | 谁维护       |
| -------- | --------------------------------------- | ---------------- | --------- |
| L1 实体内嵌  | 每个实体 frontmatter 的 `change_log`         | 这个实体发生过什么        | AI 写实体时同步 |
| L2 变更集日志 | `docs/records/audit/<period>.yaml`      | 谁在什么时候改了什么、为什么   | 可见性协议自动写  |
| L3 会话记录  | `docs/records/sessions/SESSION-xxxx.md` | 一次对话里发生了什么、交接了什么 | 关键决定、理解纠正或工作切换时追加检查点；结束时总结 |

查询路径：

| 查询         | 读取路径                          |
| ---------- | ----------------------------- |
| 这个实体的历史    | 读实体 `change_log`              |
| 这个实体最近发生什么 | 读 `change_log` 最后 N 条         |
| 谁改过这个文件    | 读相关时间分片，过滤 `files`            |
| 所有 ADR 历史  | 读时间分片，过滤 `entities` 含 `ADR-`  |
| 这个决策为什么被改  | 读原 ADR + 过滤含该 ADR ID 的变更集     |
| 这个时间段发生什么  | 读对应时间分片                       |
| 某个变更集详情    | 按 changeset ID 在对应时间分片定位      |
| 最近一次交接     | 读 `AI_ANCHOR.md`，再按需读 SESSION |

规则：

- 优先每批读取 1 个实体及 1–2 个相关分片，发现关联问题可分批扩展并说明依据。

- 不无目的遍历历史；审核或纠错确需全范围时先明确范围并分批处理。

- 禁止把全部历史加载进上下文。

- 归档分片默认不读；解决已授权问题确需其证据时按关联加载并说明，不重复要求授权。

#### 3.2.4 `index.yaml` 职责

`index.yaml` 只保留当前状态和指针，不存全部历史：

```yaml
standard_version: "1.1"
current_stage: F1
current_task: TASK-0003
last_session: SESSION-0012
entities:
  BUG-0042:
    path: docs/work/BUG-0042.md
    status: fixed
    last_changeset: CHGSET-2026-09-14-009
    latest_session: SESSION-0012
audit_shards:
  - period: 2026-09
    path: docs/records/audit/2026-09.yaml
archive_shards:
  - period: 2026-Q2
    path: docs/records/audit/archive/2026-Q2.yaml
```

规则：

- index.yaml 由 AI 或工具依据实体事实派生维护，不独立决定实体状态。无脚本时 AI 可直接同步并校验。

- 实体变更时同步更新。

- 不存正文，不存详细历史。

- AI 接手先读 `index.yaml`，再按需读实体。

#### 3.2.5 归档策略

- 时间分片超过 `docs.archive_after`（默认 3 个月）移入 `archive/`。

- 归档不删除，只移位置。

- `index.yaml` 记录归档区间。

- 归档分片可合并为季度或年度文件。

- 归档分片默认不读。

---

### 3.3 变更分级

所有文档和代码变更分四级。级别决定可见性、确认要求、是否可静默。

| 级别  | 名称    | 例子                               | 用户可见性            | 需确认    | 可静默 |
| --- | ----- | -------------------------------- | ---------------- | ------ | --- |
| L0  | 追加记录  | SESSION 追加、状态更新、时间戳、索引同步         | 最小回执             | 否      | 是   |
| L1  | 实质内容  | REQ/DES/TASK 内容更新、BUG Session 追加 | 回执 + 摘要          | 默认否    | 是   |
| L2  | 结构性变更 | 新增实体、删除字段、改变关系、新 ADR             | 回执 + 影响分析        | 是      | 否   |
| L3  | 关键变更  | 架构不变量、ADR 决策、接口契约、废弃旧决策          | 回执 + 影响分析 + 显式批准 | 是，必须显式 | 否   |

规则：

- 级别由变更内容决定，不由文件决定。

- 一次变更集只允许一个级别；混合时取最高级别。

- L2/L3 禁止静默。

- 静默授权必须绑定具体变更集，单次有效，自动失效。

- 静默授权登记禁止使用“下一个变更集”这类模糊描述。

- 写入失败、冲突、回滚、不变量违反、待确认项，必须立即正常通知。

---

### 3.4 软门禁

标准默认软门禁，不用 CI 阻断开发。

#### 3.4.1 自检点

AI 在三个时刻必须自检：

| 时刻  | 自检内容                             | 不通过处理           |
| --- | -------------------------------- | --------------- |
| 接手时 | 是否读了 `AI_ANCHOR`、`AI_ENTRY`、相关实体 | 补读，或记 `WAIVER`  |
| 写入前 | 是否已声明、级别是否判断、是否需确认               | 补声明，或调整级别       |
| 结束前 | 是否已回执、是否更新 `change_log`、是否写交接    | 补回执，或记 `WAIVER` |

#### 3.4.2 不通过的三选一

自检不通过时，AI 不能默默继续，必须三选一：

1. 修复：补做缺失步骤。

2. 记 `WAIVER`：说明原因、范围、影响、有效期。

3. 标记 `blocked`：等待用户决策。

用户可强制继续，但必须创建并记录 `OVR-xxxx`。

#### 3.4.3 门禁强度

由 `gates.policy` 决定：

| 值             | 含义                           |
| ------------- | ---------------------------- |
| `soft`        | 默认，自检 + 声明 + 回执 + 豁免         |
| `human`       | 在 `soft` 基础上，L2/L3 必须人类确认    |
| `optional_ci` | 在 `human` 基础上，CI 可选运行，仅报告不阻断 |

规则：

- 默认 `soft`。

- 切换强度走 `CHG`。

- CI 失败不阻断，但必须记录。

- 禁止用 CI 替代自检。

---

### 3.5 证据等级

验证策略可为 `none`，但必须记录证据等级和置信度。

| 等级   | 含义               |
| ---- | ---------------- |
| `E0` | 无验证              |
| `E1` | 编译通过 / 静态检查      |
| `E2` | 人工审查 / 运行探针 / 日志 |
| `E3` | 自动化测试 / 形式化验证    |

规则：

- `verification.policy` 与 `evidence_level` 独立。

- 无测试时，可用 `E1` 或 `E2` 记录证据。

- 证据写入 `VER` 或对应实体的 Session。

- 证据必须可复现，或标注不可复现原因。

- 证据不删除，失效标记 `invalidated`。

---

### 3.6 豁免机制

豁免用于合法偏离标准，不用于绕过标准。

#### 3.6.1 `WAIVER`

用于放宽某条 `MUST` 或 `SHOULD`。

```yaml
id: WAIVER-0001
type: waiver
status: active
scope: "verification.policy = none 时，TASK-0007 不写单元测试"
rule: "3.5 证据等级"
reason: "该模块为临时探针，生命周期短"
impact: "无自动化回归，依赖人工审查"
expires: 2026-12-31
owner: human
change_log:

- changeset: CHGSET-2026-09-14-010
  at: 2026-09-14T18:00:00+08:00
  action: created
  summary: "临时探针豁免自动化测试"
```

规则：

- `WAIVER` 必须写明 scope、rule、reason、impact、expires、owner。

- 到期必须复审，复审走 `CHG`。

- `WAIVER` 不删除历史，不覆盖标准。

- `WAIVER` 不改变实体状态机。

#### 3.6.2 `OVERRIDE`

用于用户强制继续，覆盖 AI 的自检或门禁。

```yaml
id: OVR-0001
type: ovr
status: active
scope: "本次 L2 变更未经确认"
rule: "3.3 变更分级"
reason: "用户要求立即推进"
impact: "待补确认"
expires: "本变更集结束"
owner: human
change_log:

- changeset: CHGSET-2026-09-14-011
  at: 2026-09-14T18:30:00+08:00
  action: created
  summary: "用户强制推进 L2 变更"
```

规则：

- `OVERRIDE` 必须由用户明确发起。

- `OVERRIDE` 只覆盖本次，不延续。

- `OVERRIDE` 必须记录待补事项。

- `OVERRIDE` 不删除历史。

---


### 配置冲突与聚合生成

- 工作流程需要未启用实体时，必须记录配置冲突，由用户决定启用、调整流程或创建豁免。
- 聚合视图在关联变更集完成后生成；生成失败不回滚源实体，但必须记录验证失败并在交接摘要中列为未决。

### 项目状态的文档来源

- 项目状态必须来自标准要求产出的文档及其状态字段，不得仅凭对话记忆或目录猜测。
- AI_ANCHOR.md 提供当前阶段和最近交接导航；PROJECT_PROFILE.md 提供配置状态；实体文件提供任务、缺陷、变更、验证和未决事项状态；SESSION 与审计提供过程证据。
- 状态判断必须检查当前阶段、活动实体、阻塞、未验证变更、未回答问题、活动豁免或覆盖、配置冲突和最近交接是否完整。

### 状态来源权威顺序

索引不同步时应核对实体正文、状态变更、审计和已确认项目安排，再同步索引、锚点、入口和相关看板。不能机械照抄头部或以最新时间戳自动裁决；文档间尚有实质分歧时显式记录未知和影响。

区分当前状态与条件性计划：暂停与计划稍后恢复可以并存，必要时补充状态原因、前置工作、恢复条件及后续安排。不根据状态名自行推断业务顺序；索引修补和待决项按收束审核向用户报告。

- 实体 frontmatter 状态和审计记录是事实来源。
- `AI_ANCHOR.md` 是导航缓存，不覆盖实体状态。
- `SESSION` 是过程证据；`PROJECT_PROFILE.md` 是配置来源。
- 发现冲突时暂停修改，读取最近变更集和会话；无法判断则创建 `OPEN-xxxx`，已发生错误写入则创建 `INC-xxxx`。
- 只有关键状态完整且无未处理异常时，项目才能判定为正常推进。
- 聚合失败不阻止安全交接，但必须写入 `SESSION`、`AI_ANCHOR.md` 和交接摘要；若聚合是下一步必要输入，任务进入 `blocked`。


### 状态同步与异常

- 阶段切换、当前任务切换、变更集完成、验证结束和交接完成后，必须更新 AI_ANCHOR.md。
- 状态不明时暂停依赖不明状态的代码修改，读取相关实体和审计并说明不确定项；独立工作可在授权内继续，持久未决按启用配置记录。
- 规则文本冲突、项目配置选择和实际违规分别处理：文本冲突创建 OPEN；配置选择按项目配置执行并记录理由；违反核心原则创建 WAIVER/OVR；违反已确定配置创建 INC。

### 项目基线

项目基线是聚合视图，不是新实体；按需记录当前目标、阶段、批次、活动需求和任务、关键依赖、未决问题、未验证变更、活动豁免或覆盖、风险和限制。


### 项目责任与验证配置

项目是否设置需求/设计责任方、分领域验证责任或总体验证责任，由用户在需求分析阶段决定；标准只要求责任和验收范围明确、可追溯，不预设组织结构。


### 当前意图理解

唯一 USER_INTENT.md 的位置、来源、更新和引用方式以 AI_STANDARD_INTENT.md 第 3 节为准，不新增实体，不自动将解释视为用户决定。

### 配置示例与等级解释

3.1.1 中竖线分隔的值表示可选项说明，实例必须选一个合法值，不将整串写入配置。日期、数组和空值保持各自类型；未知字段按实际字段类型留空或标 undecided，不把所有字段都改成字符串。实体 enabled/disabled 按项目确认，不以示例自动启用。

3.3 为默认确认策略；已有用户授权且覆盖当前具体变更时不重复询问。L3 始终需要明确授权；L2 默认需确认，soft 可按 require_confirmation 调整，human/optional_ci 至少保留 L2/L3。静默仍需具体声明和授权。证据等级分类不代表范围越大或结果必然可信，必须结合实际目标、条件和局限。
