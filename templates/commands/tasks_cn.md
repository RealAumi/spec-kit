---
description: 基于现有设计文档为功能生成可执行、依赖有序的 tasks.md。
scripts:
  sh: scripts/bash/check-prerequisites.sh --json
  ps: scripts/powershell/check-prerequisites.ps1 -Json
---

用户输入可由代理直接提供或作为命令参数传入——你**必须**在处理提示前考虑它（如不为空）。

User input:

$ARGUMENTS

1. 从仓库根运行 `{SCRIPT}`，解析 FEATURE_DIR 和 AVAILABLE_DOCS。所有路径须为绝对路径。
2. 加载并分析现有设计文档：
   - 必读 plan.md 获取技术栈和库
   - 如有 data-model.md 读取实体
   - 如有 contracts/ 读取 API 端点
   - 如有 research.md 读取技术决策
   - 如有 quickstart.md 读取测试场景

   注意：并非所有项目都有全部文档。例如：
   - CLI 工具可能无 contracts/
   - 简单库可能无 data-model.md
   - 任务按可用文档生成

3. 按模板生成任务：
   - 以 `/templates/tasks-template.md` 为基础
   - 用实际任务替换示例，依据：
     * **环境搭建**：项目初始化、依赖、代码规范
     * **测试任务 [P]**：每个合约/集成场景各一
     * **核心任务**：每个实体、服务、CLI 命令、端点
     * **集成任务**：数据库、中间件、日志
     * **优化任务 [P]**：单元测试、性能、文档

4. 任务生成规则：
   - 每个合约文件→合约测试任务 [P]
   - 每个数据模型实体→建模任务 [P]
   - 每个端点→实现任务（如共用文件则不可并行）
   - 每个用户故事→集成测试 [P]
   - 不同文件=可并行 [P]
   - 同一文件=顺序（无 [P]）

5. 按依赖排序：
   - 环境搭建优先
   - 测试先于实现（TDD）
   - 模型先于服务
   - 服务先于端点
   - 核心先于集成
   - 所有任务先于优化

6. 包含并行执行示例：
   - 分组展示可并行 [P] 任务
   - 展示实际 Task agent 命令

7. 在 FEATURE_DIR/tasks.md 中生成：
   - 正确功能名（来自实现计划）
   - 顺序编号（T001, T002...）
   - 每个任务精确文件路径
   - 依赖说明
   - 并行执行指引

Context for task generation: {ARGS}

生成的 tasks.md 应可直接执行——每个任务都要足够具体，LLM 无需额外上下文即可完成。
