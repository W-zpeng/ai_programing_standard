# AI_STANDARD

> AI 编程标准 1.1 · 正式发布 · 2026-10-30。
> 文件身份以 STANDARD_RELEASE.yaml 为准；1.0 → 1.1 包含 VER 语义调整，先读 RELEASE_NOTES.md 和 migration/MIGRATION.md，禁止只覆盖版本字段。

## 接手与路由

先判断用户当前表达、项目是否已配置及是否存在未完成迁移。已配置项目读取 AI_ANCHOR、AI_ENTRY、PROJECT_PROFILE、index 以及当前动作相关的记录；缺失时核实并恢复，不假装已读。纯讨论不要求完成初始化。

以下为相关章节导航，不要求整份全部加载；已在上下文且未变化的内容不重复读。

| 情境 | 规范 |
|---|---|
| 不确定意图、共同探索、局部明确、授权原型 | AI_STANDARD_INTENT.md；需要配置时 ONBOARDING |
| 项目首次配置 | PROJECT、ONBOARDING；创建具体记录时 ENTITIES/TEMPLATES |
| 已配置项目接手、需求/任务/缺陷 | EXECUTION、相关实体及 ENTITIES |
| 代码/文档写入、回滚 | CHANGE；按项目配置处理确认 |
| 验证与证据判断 | PROJECT 3.5、ENTITIES 的 VER、EXECUTION |
| 批次/里程碑收束审核 | EXECUTION“阶段收束审核”、CHECKLIST；修补读 CHANGE |
| 复盘提炼或经验适用性复审 | AI_STANDARD_LEARNING.md，按入口展开 |
| 违规、并行冲突、标识纠错 | AUDIT，相关变更读 CHANGE |
| 历史查询 | PROJECT 3.2、EXECUTION 5.8；歧义标识读 AUDIT |
| 交接、恢复上下文 | EXECUTION、实际当前意图和任务相关记录 |
| 版本升级或恢复迁移 | migration/MIGRATION.md、适用 UPDATE.yaml/UPDATE.md |

## 文件职责与唯一出处

简称 CORE/ENTITIES/PROJECT/ONBOARDING/EXECUTION/CHANGE/AUDIT/TEMPLATES/CHECKLIST/INTENT/LEARNING 分别指同目录 AI_STANDARD_<简称>.md；MIGRATION 指 migration/MIGRATION.md。

- CORE：原则、约束等级、版本与用户决策边界。
- ENTITIES：实体身份、关系、状态机、记录结构，含 VER 生命周期与执行记录。
- PROJECT：配置、拓扑、索引、派生视图、默认分级与门禁、证据类别。
- ONBOARDING：渐进配置和问题库；INTENT：贯穿所有阶段的理解与探索。
- EXECUTION：任务流程、交接、依赖及阶段收束；CHECKLIST：适用检查项。
- CHANGE：声明、回执、CHGSET、静默、审计格式、回滚。
- AUDIT：异常和受控纠错；LEARNING：经验入口、详情与来源。
- TEMPLATES：格式示例，不另行定义状态机或覆盖项目决策。

明确规范冲突时说明影响并暂停相关动作，按已启用记录承载待决；不可自行猜测。项目选择在标准允许的可配置范围内生效。现有用户授权不重复请求，但不能用理解摘要扩大授权或伪造事实。
