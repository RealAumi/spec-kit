# 任务：[FEATURE NAME]

**输入**: `/specs/[###-feature-name]/` 下的设计文档
**前置条件**: plan.md（必需）、research.md、data-model.md、contracts/

## 执行流程（main）
```
1. 从功能目录加载 plan.md
   → 若未找到：ERROR "未找到实现计划"
   → 提取：技术栈、库、结构
2. 加载可选设计文档：
   → data-model.md：提取实体 → 建模任务
   → contracts/：每个文件 → 合约测试任务
   → research.md：提取决策 → 环境搭建任务
3. 按类别生成任务：
   → 环境搭建：项目初始化、依赖、代码规范
   → 测试：合约测试、集成测试
   → 核心：模型、服务、CLI 命令
   → 集成：数据库、中间件、日志
   → 优化：单元测试、性能、文档
4. 应用任务规则：
   → 不同文件 = 标记 [P] 可并行
   → 同一文件 = 顺序执行（无 [P]）
   → 测试先于实现（TDD）
5. 任务顺序编号（T001, T002...）
6. 生成依赖图
7. 创建并行执行示例
8. 校验任务完整性：
   → 所有合约有测试？
   → 所有实体有模型？
   → 所有端点已实现？
9. 返回：SUCCESS（任务可执行）
```

## 格式: `[ID] [P?] 描述`
- **[P]**: 可并行（不同文件、无依赖）
- 描述中包含精确文件路径

## 路径约定
- **单项目**: `src/`、`tests/` 在仓库根目录
- **Web 应用**: `backend/src/`、`frontend/src/`
- **移动端**: `api/src/`、`ios/src/` 或 `android/src/`
- 下方路径假设为单项目结构——如有不同请参考 plan.md

## 阶段 3.1：环境搭建
- [ ] T001 按实现计划创建项目结构
- [ ] T002 用 [language] 和 [framework] 初始化项目
- [ ] T003 [P] 配置代码规范和格式化工具

## 阶段 3.2：测试优先（TDD） ⚠️ 必须在 3.3 前完成
**关键：这些测试必须先写且必须先失败，之后才能实现**
- [ ] T004 [P] 合约测试 POST /api/users，文件：tests/contract/test_users_post.py
- [ ] T005 [P] 合约测试 GET /api/users/{id}，文件：tests/contract/test_users_get.py
- [ ] T006 [P] 集成测试用户注册，文件：tests/integration/test_registration.py
- [ ] T007 [P] 集成测试认证流程，文件：tests/integration/test_auth.py

## 阶段 3.3：核心实现（仅在测试失败后进行）
- [ ] T008 [P] 用户模型，文件：src/models/user.py
- [ ] T009 [P] UserService CRUD，文件：src/services/user_service.py
- [ ] T010 [P] CLI --create-user，文件：src/cli/user_commands.py
- [ ] T011 POST /api/users 端点
- [ ] T012 GET /api/users/{id} 端点
- [ ] T013 输入校验
- [ ] T014 错误处理与日志

## 阶段 3.4：集成
- [ ] T015 连接 UserService 到数据库
- [ ] T016 认证中间件
- [ ] T017 请求/响应日志
- [ ] T018 CORS 与安全头

## 阶段 3.5：优化
- [ ] T019 [P] 校验相关单元测试，文件：tests/unit/test_validation.py
- [ ] T020 性能测试（<200ms）
- [ ] T021 [P] 更新文档 docs/api.md
- [ ] T022 移除重复
- [ ] T023 运行 manual-testing.md

## 依赖关系
- 测试（T004-T007）先于实现（T008-T014）
- T008 阻塞 T009、T015
- T016 阻塞 T018
- 实现先于优化（T019-T023）

## 并行示例
```
# 同时启动 T004-T007：
任务: "合约测试 POST /api/users，文件：tests/contract/test_users_post.py"
任务: "合约测试 GET /api/users/{id}，文件：tests/contract/test_users_get.py"
任务: "集成测试注册，文件：tests/integration/test_registration.py"
任务: "集成测试认证，文件：tests/integration/test_auth.py"
```

## 说明
- [P] 任务 = 不同文件、无依赖
- 实现前先确保测试失败
- 每完成一个任务请提交
- 避免：任务模糊、同文件冲突

## 任务生成规则
*main() 执行时应用*

1. **来自合约**：
   - 每个合约文件 → 合约测试任务 [P]
   - 每个端点 → 实现任务
   
2. **来自数据模型**：
   - 每个实体 → 建模任务 [P]
   - 关系 → 服务层任务
   
3. **来自用户故事**：
   - 每个故事 → 集成测试 [P]
   - 快速验证场景 → 校验任务

4. **排序**：
   - 环境搭建 → 测试 → 模型 → 服务 → 端点 → 优化
   - 有依赖的任务不能并行

## 校验清单
*main() 返回前检查*

- [ ] 所有合约有对应测试
- [ ] 所有实体有建模任务
- [ ] 所有测试先于实现
- [ ] 并行任务真正独立
- [ ] 每个任务指定精确文件路径
- [ ] 无任务与其他 [P] 任务修改同一文件
