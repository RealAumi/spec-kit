---
description: 使用计划模板执行实现规划流程，生成设计文档。
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
---

用户输入可由代理直接提供或作为命令参数传入——你**必须**在处理提示前考虑它（如不为空）。

User input:

$ARGUMENTS

根据提供的实现细节：

1. 从仓库根运行 `{SCRIPT}`，解析 JSON 获取 FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH。所有后续路径须为绝对路径。
   - **继续前**，检查 FEATURE_SPEC 是否有 `## Clarifications` 节且含至少一个 `Session` 子标题。如缺失或明显歧义（模糊形容词、未决关键选项），**暂停**并提示先运行 `/clarify` 以减少返工。仅在 (a) 已有澄清 或 (b) 用户明确要求“跳过澄清”时继续。**不得**自行编造澄清。
2. 阅读并分析功能规范，理解：
   - 功能需求与用户故事
   - 功能与非功能需求
   - 成功与验收标准
   - 提及的技术约束或依赖

3. 阅读 `/memory/constitution.md` 理解宪法要求。

4. 执行实现计划模板：
   - 加载 `/templates/plan-template.md`（已复制到 IMPL_PLAN 路径）
   - 设置 Input 路径为 FEATURE_SPEC
   - 按 Execution Flow (main) 步骤 1-9 执行
   - 模板自包含可执行
   - 按模板指引在 $SPECS_DIR 生成文档：
     * Phase 0 生成 research.md
     * Phase 1 生成 data-model.md、contracts/、quickstart.md
     * Phase 2 生成 tasks.md
   - 用户参数补充到 Technical Context: {ARGS}
   - 完成每阶段更新进度追踪

5. 校验执行完成：
   - 检查进度追踪所有阶段已完成
   - 所有必需文档已生成
   - 无 ERROR 状态

6. 报告结果：分支名、文件路径、生成文档。

所有文件操作须用仓库根绝对路径，避免路径问题。
