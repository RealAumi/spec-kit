---
description: "实现计划模板（用于功能开发）"
scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

# 实施计划： [FEATURE]

**分支**：`[###-feature-name]` | **日期**： [DATE] | **规格**： [link]
**输入**：来自 `/specs/[###-feature-name]/spec.md` 的功能规范

## 执行流程（/plan 命令范围）
```
1. 从输入路径加载功能规范
	→ 若未找到：ERROR "No feature spec at {path}"
2. 填写技术上下文（扫描标记为 NEEDS CLARIFICATION 的项）
	→ 根据文件系统结构或上下文检测项目类型（例如 web = frontend+backend, mobile = app+api）
	→ 根据项目类型确定结构决策
3. 根据宪法文档内容填写 Constitution Check（合规性检查）部分
4. 评估下方的 Constitution Check 部分
	→ 若存在违背点：在 Complexity Tracking 中记录
	→ 若无法给出合理说明：ERROR "Simplify approach first"
	→ 更新进度追踪：Initial Constitution Check
5. 执行 Phase 0 → 生成 research.md
	→ 若仍有 NEEDS CLARIFICATION：ERROR "Resolve unknowns"
6. 执行 Phase 1 → 生成 contracts、data-model.md、quickstart.md，以及代理特定的模板文件（例如：Claude Code 使用 `CLAUDE.md`，GitHub Copilot 使用 `.github/copilot-instructions.md`，Gemini CLI 使用 `GEMINI.md`，Qwen Code 使用 `QWEN.md`，opencode 使用 `AGENTS.md`）。
7. 重新评估 Constitution Check 部分
	→ 若出现新的违背：重构设计并返回 Phase 1
	→ 更新进度追踪：Post-Design Constitution Check
8. 规划 Phase 2 → 描述任务生成方法（不要创建 tasks.md）
9. 停止 - 准备运行 /tasks 命令
```

**重要**：/plan 命令在第 7 步停止。Phase 2-4 由其他命令执行：
- Phase 2：/tasks 命令会创建 tasks.md
- Phase 3-4：实现与执行（人工或通过工具）

## 摘要
[从功能规范提取：核心需求 + 基于 research 的技术路线]

## 技术上下文
**语言/版本**： [例如 Python 3.11、Swift 5.9、Rust 1.75 或 NEEDS CLARIFICATION]
**主要依赖**： [例如 FastAPI、UIKit、LLVM 或 NEEDS CLARIFICATION]
**存储**： [如适用：PostgreSQL、CoreData、文件或 N/A]
**测试框架**： [例如 pytest、XCTest、cargo test 或 NEEDS CLARIFICATION]
**目标平台**： [例如 Linux server、iOS 15+、WASM 或 NEEDS CLARIFICATION]
**项目类型**： [single/web/mobile —— 决定源码结构]
**性能目标**： [域相关，例如 1000 req/s、10k lines/sec、60 fps 或 NEEDS CLARIFICATION]
**约束**： [域相关，例如 <200ms p95、<100MB 内存、离线可用 或 NEEDS CLARIFICATION]
**规模/范围**： [域相关，例如 10k users、1M LOC、50 screens 或 NEEDS CLARIFICATION]

## 合规性检查（Constitution Check）
*GATE：必须在 Phase 0 研究前通过。Phase 1 设计完成后需重新检查。*

[基于宪法文件确定的门控项]

## 项目结构

### 文档（针对该功能）
```
specs/[###-feature]/
├── plan.md              # 本文件（/plan 命令的输出）
├── research.md          # Phase 0 输出（/plan 命令生成）
├── data-model.md        # Phase 1 输出（/plan 命令生成）
├── quickstart.md        # Phase 1 输出（/plan 命令生成）
├── contracts/           # Phase 1 输出（/plan 命令生成）
└── tasks.md             # Phase 2 输出（/tasks 命令生成 — /plan 不创建）
```

### 源代码（仓库根目录）
<!--
  操作说明：用该功能的实际目录结构替换下面的占位树。删除不使用的选项，并用真实路径展开所选结构（例如 apps/admin、packages/something）。交付的计划不得包含 Option 标签。
-->
```
# [若不使用请删除] 选项 1：单一项目（默认）
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [若不使用请删除] 选项 2：Web 应用（检测到 "frontend" + "backend" 时）
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [若不使用请删除] 选项 3：移动应用 + API（检测到 iOS/Android 时）
api/
└── [同上面的 backend]

ios/ 或 android/
└── [平台特定结构：功能模块、UI 流、平台测试]
```

**结构决策**： [记录所选结构并引用上方捕获到的真实目录]

## Phase 0：大纲与研究
1. **从技术上下文中提取未知项**：
	- 对每个标记为 NEEDS CLARIFICATION 的事项 → 创建 research 任务
	- 对每个依赖项 → 创建最佳实践研究任务
	- 对每个集成点 → 创建模式研究任务

2. **生成并派发研究代理（agents）**：
	```
	对于技术上下文中的每个未知项：
	  任务："为 {feature context} 研究 {unknown}"
	对于每个技术选择：
	  任务："在 {domain} 中为 {tech} 寻找最佳实践"
	```

3. **在 `research.md` 中整合发现**，采用以下格式：
	- 决策： [选择了什么]
	- 理由： [为什么选择]
	- 考虑的备选方案： [还评估了哪些]

**输出**：research.md，解决所有 NEEDS CLARIFICATION

## Phase 1：设计与合约
*前提：research.md 已完成*

1. **从功能规范中提取实体** → 生成 `data-model.md`：
	- 实体名称、字段、关系
	- 来自需求的验证规则
	- 若适用，状态迁移

2. **根据功能需求生成 API 合约**：
	- 每个用户操作 → 一个端点
	- 使用标准的 REST / GraphQL 模式
	- 将 OpenAPI / GraphQL 模式输出到 `/contracts/`

3. **根据合约生成合约测试**：
	- 每个端点一个测试文件
	- 断言请求/响应模式
	- 测试应失败（因为尚无实现）

4. **从用户故事中提取测试场景**：
	- 每个故事 → 一个集成测试场景
	- Quickstart 测试 = 故事的验证步骤

5. **增量更新代理文件（O(1) 操作）**：
	- 运行 `{SCRIPT}`
	  **重要**：按上面精确执行。不要添加或删除任何参数。
	- 若文件已存在：只添加当前计划中新增的技术项
	- 保留手动添加的内容（在标记之间）
	- 更新最近变更（仅保留最近 3 条）
	- 控制文件长度在 150 行以内以节省 token
	- 输出到仓库根目录

**输出**：data-model.md、/contracts/*、失败的测试、quickstart.md、代理特定文件

## Phase 2：任务规划方法
*本节描述 /tasks 命令将执行的操作 — 在 /plan 期间不要执行它*

**任务生成策略**：
- 以 `.specify/templates/tasks-template.md` 作为基础模板
- 从 Phase 1 的设计文档（合约、数据模型、quickstart）生成任务
- 每个合约 → 合约测试任务 [P]
- 每个实体 → 模型创建任务 [P]
- 每个用户故事 → 集成测试任务
- 实现任务以让测试通过

**排序策略**：
- TDD 顺序：先写测试再实现
- 依赖顺序：模型 → 服务 → UI
- 对于相互独立的文件标记为 [P] 可并行执行

**估计输出**：在 tasks.md 中生成 25-30 个编号且有序的任务

**重要**：此阶段由 /tasks 命令执行，而非 /plan

## Phase 3+：后续实现
*这些阶段超出 /plan 命令的范围*

**Phase 3**：执行任务（/tasks 命令创建 tasks.md）
**Phase 4**：实现（按宪法原则执行 tasks.md）
**Phase 5**：验证（运行测试、执行 quickstart.md、性能验证）

## 复杂度跟踪（Complexity Tracking）
*仅在 Constitution Check 存在需要说明的违背时填写*

| 违背项 | 为什么需要 | 为什么拒绝更简单的替代方案 |
|--------|------------|-------------------------------|
| [例如，第 4 个项目] | [当前需求] | [为什么 3 个项目不够] |
| [例如，仓库模式] | [具体问题] | [为什么直接 DB 访问不够] |


## 进度追踪
*此清单在执行流程中更新*

**阶段状态**：
- [ ] Phase 0：研究完成（/plan 命令）
- [ ] Phase 1：设计完成（/plan 命令）
- [ ] Phase 2：任务规划完成（/plan 命令 — 只描述方法）
- [ ] Phase 3：任务已生成（/tasks 命令）
- [ ] Phase 4：实现完成
- [ ] Phase 5：验证通过

**门控状态**：
- [ ] 初始 Constitution Check：PASS
- [ ] 设计后 Constitution Check：PASS
- [ ] 所有 NEEDS CLARIFICATION 已解决
- [ ] 复杂度偏离已记录

---
*基于 Constitution v2.1.1 — 参见 `/memory/constitution.md`*

