---
description: 通过交互或提供的原则输入创建/更新项目宪法，并确保所有依赖模板同步。
---

用户输入可由代理直接提供或作为命令参数传入——你**必须**在处理提示前考虑它（如不为空）。

User input:

$ARGUMENTS

你正在更新 `/memory/constitution.md` 项目宪法。该文件为模板，包含如 [PROJECT_NAME]、[PRINCIPLE_1_NAME] 等方括号占位符。你的任务是：(a) 收集/推导具体值，(b) 精确填充模板，(c) 将修订同步到所有依赖文档。

执行流程：

1. 加载 `/memory/constitution.md` 模板。
   - 识别所有 [ALL_CAPS_IDENTIFIER] 形式的占位符。
   **重要**：如用户要求原则数量与模板不同，须遵循用户要求，按通用模板更新。

2. 收集/推导占位符值：
   - 用户输入有则用，无则从现有仓库上下文（README、docs、旧版宪法等）推断。
   - 治理日期：`RATIFICATION_DATE` 为首次采纳日（如未知请标 TODO），`LAST_AMENDED_DATE` 如有变更为今日，否则保留原值。
   - `CONSTITUTION_VERSION` 遵循语义化版本号：
     * MAJOR：破坏性治理/原则移除或重定义
     * MINOR：新增原则/章节或实质性扩展
     * PATCH：澄清、措辞、错别字、非语义修订
   - 如版本类型不明，先说明理由再定稿。

3. 草拟更新内容：
   - 替换所有占位符（除非项目明确保留未定义项，须注明理由）。
   - 保持标题层级，注释如已替换可移除，仍有指导意义可保留。
   - 每条原则：简明名称+段落（或要点）阐明不可协商规则，必要时加理由。
   - 治理节须列修订流程、版本策略、合规审查要求。

4. 一致性同步检查：
   - 检查 `/templates/plan-template.md` 是否与新原则一致。
   - 检查 `/templates/spec-template.md` 是否与新原则对齐（如有新增/删减强制部分）。
   - 检查 `/templates/tasks-template.md` 是否反映原则驱动的任务类型变更（如可观测性、版本、测试纪律等）。
   - 检查 `/templates/commands/*.md`（含本文件）是否有过时引用（如特定代理名），如需通用则修正。
   - 检查运行时指导文档（如 `README.md`、`docs/quickstart.md` 等），如有原则变更需同步。

5. 生成同步影响报告（更新后加 HTML 注释于宪法文件顶部）：
   - 版本变更：old → new
   - 修改原则列表（如有重命名）
   - 新增章节
   - 删除章节
   - 需更新模板（✅已更新/⚠️待更新）及路径
   - 如有占位符延后，列出 TODO

6. 校验：
   - 无未解释方括号占位符
   - 版本号与报告一致
   - 日期为 ISO 格式 YYYY-MM-DD
   - 原则具声明性、可测试、无模糊措辞（“should”→MUST/SHOULD 并说明）

7. 写回 `/memory/constitution.md`（覆盖）。

8. 向用户输出摘要：
   - 新版本及升级理由
   - 需手动跟进的文件
   - 建议提交信息（如 `docs: amend constitution to vX.Y.Z (principle additions + governance update)`）

格式与风格要求：
- 标题层级与模板一致（不升降级）
- 长理由建议换行（<100 字），但不强制断句
- 各节间空一行
- 无多余空格

如用户仅部分修订（如只改一条原则），仍需校验与版本决策。

如关键信息缺失（如采纳日未知），插入 `TODO(<FIELD_NAME>): 说明`，并在同步报告中列出。

不得新建模板，只能操作现有 `/memory/constitution.md`。
