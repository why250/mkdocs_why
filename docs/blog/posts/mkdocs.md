---
date:
  created: 2025-09-03
categories:
  - CS
tags:
  - MkDocs
  - documentation
authors:
  - why

---

# MkDocs Material：本仓库本地预览与构建

本仓库使用 MkDocs Material。依赖安装在项目根目录的 `.venv` 中，避免污染系统 Python。

<!-- more -->

## 本地预览

```powershell
.\.venv\Scripts\python.exe -m mkdocs serve
```

默认访问 `http://127.0.0.1:8000/`。修改 Markdown 后页面会自动刷新。

## 严格构建检查

```powershell
.\.venv\Scripts\python.exe -m mkdocs build --strict
```

提交前运行严格构建；警告和错误都应先处理。首次在新机器配置环境时，可参考 [Python 虚拟环境](venv.md)。
```
