# 安装指南

## 前置条件

- **Linux/macOS**（或 Windows；PowerShell 脚本现已支持，无需 WSL）
- AI 编码代理：[Claude Code](https://www.anthropic.com/claude-code)、[GitHub Copilot](https://code.visualstudio.com/)、或 [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [uv](https://docs.astral.sh/uv/) 包管理工具
- [Python 3.11+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)

## 安装

### 初始化新项目

最简单的方式是初始化一个新项目：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

或在当前目录初始化：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init .
# 或使用 --here 参数
uvx --from git+https://github.com/github/spec-kit.git specify init --here
```

### 指定 AI 代理

初始化时可主动指定 AI 代理：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --ai claude
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --ai gemini
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --ai copilot
```

### 指定脚本类型（Shell vs PowerShell）

所有自动化脚本现有 Bash（.sh）和 PowerShell（.ps1）两种版本。

自动行为：
- Windows 默认：ps
- 其他系统默认：sh
- 交互模式：如未指定 --script，会提示选择

强制指定脚本类型：
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --script sh
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --script ps
```

### 忽略代理工具检查

如只想获取模板而不检查工具：

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <project_name> --ai claude --ignore-agent-tools
```

## 验证

初始化后，你的 AI 代理应能使用以下命令：
- `/specify` - 创建规范
- `/plan` - 生成实现计划
- `/tasks` - 拆解为可执行任务

`.specify/scripts` 目录下会有 .sh 和 .ps1 脚本。

## 故障排查

### Linux 下 Git 凭证管理器

如在 Linux 上遇到 Git 认证问题，可安装 Git Credential Manager：

```bash
#!/usr/bin/env bash
set -e
echo "下载 Git Credential Manager v2.6.1..."
wget https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb
echo "安装 Git Credential Manager..."
sudo dpkg -i gcm-linux_amd64.2.6.1.deb
echo "配置 Git 使用 GCM..."
git config --global credential.helper manager
echo "清理..."
rm gcm-linux_amd64.2.6.1.deb
```
