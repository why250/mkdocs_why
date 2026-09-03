---
date:
  created: 2026-01-15
categories:
  - CS
tags:
  - Git
  - Python
authors:
  - why

---

# 用 Git 迁移 Python 项目与虚拟环境

<!-- more -->
使用 Git 管理代码和配置，比直接拷贝文件更安全，也方便后续同步与回溯。

**关键原则：不要提交虚拟环境目录。** 环境会携带本机解释器和绝对路径，应在目标机器重新创建；需要纳入版本控制的是代码、配置和依赖清单。

以下是使用 Git 进行迁移的最佳实践步骤：

### 1. 在当前电脑上初始化 Git (关键步骤：忽略虚拟环境)

在提交之前，你需要告诉 Git **忽略** 掉那些不需要甚至**不能**迁移的文件（如虚拟环境文件夹和临时文件）。

在项目根目录打开终端，按以下步骤操作：

**第一步：初始化仓库**
```powershell
git init
```

**第二步：创建 `.gitignore` 文件 (最重要的一步)**
你需要创建一个名为 `.gitignore` 的文件，防止 Git 追踪庞大且不可迁移的虚拟环境。
在终端运行：
```powershell
@'
.venv/
.ipynb_checkpoints/
__pycache__/
'@ | Set-Content .gitignore
```
*   `.venv/`: 忽略虚拟环境文件夹（其中的解释器路径在迁移后会失效）。
*   `.ipynb_checkpoints/`: 忽略 Jupyter 的自动存档。

**第三步：提交文件**
现在 Git 只会识别你的代码和配置文件。
```powershell
git add .
git commit -m "Initial setup for DP2031 control project"
```
此时，Git 只会打包代码、配置、`requirements.txt`（如有）以及 `.gitignore` 本身。

**第四步：推送到远程仓库**
你需要在 GitHub、Gitee 或公司的 GitLab 上建一个空仓库，然后关联推送：
```powershell
git remote add origin <你的远程仓库地址>
git push -u origin main
```

---

### 2. 在新电脑上恢复环境

到了新电脑上，你只需要拉取代码，然后利用我们准备好的脚本**重建环境**。

**第一步：克隆仓库**
```powershell
git clone <你的远程仓库地址> <项目目录>
cd <项目目录>
```

**第二步：一键重建环境**
因为我们忽略了 `.venv` 文件夹，现在需要重新生成它。依次运行以下命令：

1.  **创建环境：**
    ```powershell
    python -m venv .venv
    ```
2.  **激活环境：**
    ```powershell
    .\.venv\Scripts\activate
    ```
3.  **安装依赖：**
    ```powershell
    python -m pip install -r requirements.txt
    ```
4.  **如使用 Jupyter，再配置内核：** 使用项目提供的脚本，或从已激活的环境安装并注册 `ipykernel`。

### 3. 为什么这样做更好？
*   **体积小：** 不需要传输几百 MB 的虚拟环境，只需传输代码与文本配置。
*   **兼容性好：** 虚拟环境是在新电脑上利用本地 Python 重新构建的，完全避免了“路径乱码”或“找不到解释器”的错误。
*   **可回溯：** 修改后提交并推送，其他机器拉取即可同步。

**项目相关配置仍需检查：** 硬件地址、密钥和本机路径不应假定能随 Git 自动适配；其中敏感信息不应提交到仓库。
