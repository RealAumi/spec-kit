---
description: 从自然语言功能描述创建或更新功能规范。
scripts:
  sh: scripts/bash/create-new-feature.sh --json "{ARGS}"
  ps: scripts/powershell/create-new-feature.ps1 -Json "{ARGS}"
---

用户输入可由代理直接提供或作为命令参数传入——你**必须**在处理提示前考虑它（如不为空）。

User input:

$ARGUMENTS

用户在触发消息中 `/specify` 后输入的文本**即为**功能描述。即使下方出现 `{ARGS}`，也视为已获得，无需用户重复输入，除非命令为空。

根据该功能描述：

1. 从仓库根运行 `{SCRIPT}`，解析 JSON 输出 BRANCH_NAME 和 SPEC_FILE。所有路径须为绝对路径。
  **重要** 该脚本只能运行一次。JSON 输出在终端，需据此获取实际内容。
2. 加载 `templates/spec-template.md` 理解所需结构。
3. 按模板结构写入 SPEC_FILE，用功能描述（参数）具体内容替换占位符，保留原有节序和标题。
4. 报告完成情况：分支名、规范文件路径、是否可进入下一阶段。

注意：脚本会自动创建并切换新分支并初始化规范文件。
