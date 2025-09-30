# 快速上手指南

本指南将帮助你用 Spec Kit 开始规范驱动开发。

> 新特性：所有自动化脚本现有 Bash（.sh）和 PowerShell（.ps1）两种版本。`specify` CLI 会根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 四步流程

### 1. 安装 Specify

根据你用的编码代理初始化项目：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

可显式指定脚本类型（可选）：
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script ps  # 强制 PowerShell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME> --script sh  # 强制 POSIX shell
```

### 2. 创建规范

用 `/specify` 命令描述你要构建的内容。关注**做什么**和**为什么**，不要涉及技术栈。

```bash
/specify 构建一个能帮我将照片按相册分组管理的应用。相册按日期分组，可在主页面拖拽重新排序。相册不嵌套。每个相册内照片以平铺方式预览。
```

### 3. 创建技术实现计划

用 `/plan` 命令提供你的技术栈和架构选择。

```bash
/plan 应用使用 Vite，依赖库最少。尽量用原生 HTML、CSS、JavaScript。图片不上传，元数据存本地 SQLite。
```

### 4. 拆解并实现

用 `/tasks` 生成可执行任务清单，然后让你的代理实现该功能。

## 详细示例：构建 Taskify

以下是构建团队协作平台 Taskify 的完整示例：

### 步骤 1：用 `/specify` 定义需求

```text
开发 Taskify 团队协作平台。支持用户创建项目、添加成员、分配任务、评论、在看板上拖拽任务。初始阶段，用户预定义：1 个产品经理、4 个工程师。创建 3 个示例项目。看板有 "To Do"、"In Progress"、"In Review"、"Done" 四列。无需登录，仅用于基础功能测试。任务卡可切换状态、无限评论、分配用户。首次进入显示用户列表，点击进入主界面，显示项目列表，点击项目进入看板。可拖拽任务卡，当前用户的卡片高亮。可编辑/删除自己评论，不能操作他人评论。
```

### 步骤 2：细化规范

初始规范后，澄清遗漏需求：

```text
每个项目应有 5-15 个任务，随机分布于不同状态，每个状态至少有一个任务。
```

还要校验规范清单：

```text
阅读验收清单，逐项勾选符合项，不符合留空。
```

### 步骤 3：用 `/plan` 生成技术方案

明确技术栈和技术要求：

```text
用 .NET Aspire，数据库用 Postgres。前端用 Blazor server，支持拖拽、实时更新。需 REST API：projects、tasks、notifications。
```

### 步骤 4：校验并实现

让 AI 代理审核实现计划：

```text
请审核实现计划和实现细节文件，检查任务序列是否合理，是否有遗漏。
```

最后实现方案：

```text
implement specs/002-create-taskify/plan.md
```

## 关键原则

- **明确**你要做什么和为什么
- **规范阶段不聚焦技术栈**
- **迭代细化**规范再实现
- **实现前校验方案**
- **让 AI 代理处理实现细节**

## 下一步

- 阅读完整方法论获取详细指导
- 查看更多示例
- 在 GitHub 浏览源码
