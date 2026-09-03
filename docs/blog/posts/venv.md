---
date:
  created: 2025-01-07
categories:
  - CS
tags:
  - Python
  - venv
authors:
  - why

---
# Python 虚拟环境：Linux 快速创建

## 安装 `venv`

```bash
sudo apt update
sudo apt install python3-venv
```
<!-- more -->

## 创建并激活环境

```bash
python3 -m venv .venv
source .venv/bin/activate
```

退出环境时运行 `deactivate`。虚拟环境目录应加入 `.gitignore`，依赖则通过 `requirements.txt` 或项目的依赖配置文件保存。

