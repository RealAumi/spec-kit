# 中文翻译 / Chinese Translation

> **注意**: 本文档是 [README.md](README.md) 的中文翻译版本。
>
> **Note**: This document is the Chinese translation of [README.md](README.md).

---

<!-- 以下内容需要人工翻译 / Content below needs manual translation -->

# Documentation

This folder contains the documentation source files for Spec Kit, built using [DocFX](https://dotnet.github.io/docfx/).

## Building Locally

To build the documentation locally:

1. Install DocFX:
   ```bash
   dotnet tool install -g docfx
   ```

2. Build the documentation:
   ```bash
   cd docs
   docfx docfx.json --serve
   ```

3. Open your browser to `http://localhost:8080` to view the documentation.

## Structure

- `docfx.json` - DocFX configuration file
- `index.md` - Main documentation homepage
- `toc.yml` - Table of contents configuration
- `installation.md` - Installation guide
- `quickstart.md` - Quick start guide
- `_site/` - Generated documentation output (ignored by git)

## Deployment

Documentation is automatically built and deployed to GitHub Pages when changes are pushed to the `main` branch. The workflow is defined in `.github/workflows/docs.yml`.
