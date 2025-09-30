# 本地开发指南

本指南介绍如何在不发布 release 或提交 main 分支的情况下，本地迭代 `specify` CLI。

> 所有脚本现有 Bash（.sh）和 PowerShell（.ps1）两种版本。CLI 会根据操作系统自动选择，除非你传递 `--script sh|ps`。

## 1. 克隆并切换分支

```bash
git clone https://github.com/github/spec-kit.git
cd spec-kit
# 新建功能分支
git checkout -b your-feature-branch
```

## 2. 直接运行 CLI（最快反馈）

可通过模块入口直接运行 CLI，无需安装：

```bash
# 在仓库根目录
python -m src.specify_cli --help
python -m src.specify_cli init demo-project --ai claude --ignore-agent-tools --script sh
```

如需用脚本文件风格（shebang）：

```bash
python src/specify_cli/__init__.py init demo-project --script ps
```

## 3. 可编辑安装（隔离环境）

用 `uv` 创建隔离环境，依赖与用户环境一致：

```bash
uv venv
source .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
uv pip install -e .
# 现在可用 specify 命令
specify --help
```

可编辑模式下代码改动无需重装。

## 4. 用 uvx 直接从 Git 运行（当前分支）

`uvx` 可从本地路径（或 Git 分支）运行，模拟用户流程：

```bash
uvx --from . specify init demo-uvx --ai copilot --ignore-agent-tools --script sh
```

也可指定分支：

```bash
# 先推送分支
git push origin your-feature-branch
uvx --from git+https://github.com/github/spec-kit.git@your-feature-branch specify init demo-branch-test --script ps
```

### 4a. 绝对路径 uvx（任意目录运行）

在其他目录时用绝对路径：

```bash
uvx --from /mnt/c/GitHub/spec-kit specify --help
uvx --from /mnt/c/GitHub/spec-kit specify init demo-anywhere --ai copilot --ignore-agent-tools --script sh
```

可设环境变量：
```bash
export SPEC_KIT_SRC=/mnt/c/GitHub/spec-kit
uvx --from "$SPEC_KIT_SRC" specify init demo-env --ai copilot --ignore-agent-tools --script ps
```

（可选）定义 shell 函数：
```bash
specify-dev() { uvx --from /mnt/c/GitHub/spec-kit specify "$@"; }
# 然后
specify-dev --help
```

## 5. 测试脚本权限逻辑

init 后检查 shell 脚本在 POSIX 系统下可执行：

```bash
ls -l scripts | grep .sh
# 期望有执行权限（如 -rwxr-xr-x）
```
Windows 下用 .ps1 脚本（无需 chmod）。

## 6. 运行 Lint/基础检查（可自定义）

目前未强制 Lint 配置，可快速检查可导入性：
```bash
python -c "import specify_cli; print('Import OK')"
```

## 7. 本地构建 wheel 包（可选）

发布前验证打包：

```bash
uv build
ls dist/
```
可在新环境测试安装。

## 8. 用临时工作区

测试 `init --here` 时可建临时目录：

```bash
mkdir /tmp/spec-test && cd /tmp/spec-test
python -m src.specify_cli init --here --ai claude --ignore-agent-tools --script sh
```
或只复制 CLI 相关部分做轻量沙箱。

## 9. 调试网络/TLS 跳过

如需跳过 TLS 校验：

```bash
specify check --skip-tls
specify init demo --skip-tls --ai gemini --ignore-agent-tools --script ps
```
（仅限本地实验）

## 10. 快速编辑循环总结

| 操作 | 命令 |
|------|------|
| 直接运行 CLI | `python -m src.specify_cli --help` |
| 可编辑安装 | `uv pip install -e .` 后 `specify ...` |
| 本地 uvx（仓库根） | `uvx --from . specify ...` |
| 本地 uvx（绝对路径） | `uvx --from /mnt/c/GitHub/spec-kit specify ...` |
| Git 分支 uvx | `uvx --from git+URL@branch specify ...` |
| 构建 wheel | `uv build` |

## 11. 清理

快速移除构建产物/虚拟环境：
```bash
rm -rf .venv dist build *.egg-info
```

## 12. 常见问题

| 症状 | 解决方法 |
|------|----------|
| `ModuleNotFoundError: typer` | 运行 `uv pip install -e .` |
| 脚本不可执行（Linux） | 重新 init 或 `chmod +x scripts/*.sh` |
| Git 步骤跳过 | 你传了 `--no-git` 或未装 Git |
| 下载了错误脚本类型 | 显式传 `--script sh` 或 `--script ps` |
| 公司网络 TLS 错误 | 试用 `--skip-tls`（勿用于生产）|

## 13. 下一步

- 更新文档并用修改后的 CLI 跑一遍快速上手
- 满意后提交 PR
- （可选）合入 main 后打 tag 发布
