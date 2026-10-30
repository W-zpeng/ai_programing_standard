# AI_STANDARD

> AI 编程标准 1.0 · 按项目状态和用户意图路由的规范入口。

2. 读取本入口，判断当前状态和任务。
3. 只读取相关专用规范。
4. 规则实际违反时创建 INC-xxxx；需求未决创建 OPEN-xxxx；临时前提创建 ASM-xxxx；主动偏离创建 WAIVER-xxxx 或 OVR-xxxx；规范文本冲突时暂停并创建 OPEN-xxxx。

## 路由

- 未配置项目：CORE、PROJECT、ONBOARDING、TEMPLATES、CHECKLIST。
- 首次接手：CORE、PROJECT、EXECUTION、CHECKLIST。
- 新需求/任务/缺陷：ENTITIES、EXECUTION、CHANGE。
- 修改代码或文档：CORE、ENTITIES、EXECUTION、CHANGE。
- 仅查询项目状态或历史：PROJECT、ENTITIES、AUDIT。
- 仅执行验证：ENTITIES、EXECUTION、AUDIT、CHECKLIST。
- 协议违规或异常：CORE、CHANGE、AUDIT。
- 交接/压缩恢复：CORE、EXECUTION、TEMPLATES。
- 发布前：PROJECT、CHANGE、AUDIT、CHECKLIST。
- 无法分类或多意图请求：CORE、PROJECT、ENTITIES、EXECUTION，先完成状态判断再扩展读取。
- 所有最小读取集合必须叠加当前任务直接关联的实体及其未关闭依赖。

## 文件职责

- CORE：核心原则、等级、版本。
- ENTITIES：实体、关系、状态机、ID、frontmatter。
- PROJECT：配置、目录、索引、聚合。
- ONBOARDING：引导协议。
- EXECUTION：接手、路由、执行、验证、交接。
- CHANGE：变更集、写入、静默、回滚。
- AUDIT：审计、违规、异常、并行冲突。
- TEMPLATES：所有模板。
- CHECKLIST：阶段检查与自检。


## 依赖与权威边界

- PROJECT 负责项目配置、目录、索引和聚合视图；CHANGE 负责具体变更、写入、静默和回滚。
- EXECUTION 负责工作流程和状态推进；AUDIT 负责违规、异常和审计记录。EXECUTION 不重新定义审计规则。
- TEMPLATES 只定义文件格式；实体语义以 ENTITIES 为准，协议语义以 ONBOARDING、EXECUTION、CHANGE 或 AUDIT 为准。
- CHECKLIST 只提供检查项；检查项引用的规范文件是具体规则的唯一权威来源。






## 读取名称

路由中的 CORE、PROJECT 等简写分别指向同名的 `AI_STANDARD_*.md` 文件；执行时使用完整文件名。




