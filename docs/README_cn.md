# 文档说明

本文件夹包含 Spec Kit 的文档源码，使用 [DocFX](https://dotnet.github.io/docfx/) 构建。

## 本地构建

本地构建文档步骤：

1. 安装 DocFX：
   ```bash
   dotnet tool install -g docfx
   ```

2. 构建文档：
   ```bash
   cd docs
   docfx docfx.json --serve
   ```

3. 浏览器访问 `http://localhost:8080` 查看文档。

## 结构说明

- `docfx.json` - DocFX 配置文件
- `index.md` - 主文档首页
- `toc.yml` - 目录配置
- `installation.md` - 安装指南
- `quickstart.md` - 快速上手
- `_site/` - 生成的文档输出（git 忽略）

## 部署

当推送到 main 分支时，文档会自动构建并部署到 GitHub Pages。工作流定义在 `.github/workflows/docs.yml`。
