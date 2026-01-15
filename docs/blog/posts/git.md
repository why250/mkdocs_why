---
date:
  created: 2026-01-15
categories:
  - CS
tags:
  - Python
authors:
  - why

---

# Git 
An exmaple of git :

<!-- more -->
使用 Git 管理代码和配置，比直接拷贝文件更安全，也方便后续的版本控制。

但是，**有一个关键点必须注意**：不能把虚拟环境文件夹（`cp_test`）直接提交到 Git 仓库中。

以下是使用 Git 进行迁移的最佳实践步骤：

### 1. 在当前电脑上初始化 Git (关键步骤：忽略虚拟环境)

在提交之前，你需要告诉 Git **忽略** 掉那些不需要甚至**不能**迁移的文件（如虚拟环境文件夹和临时文件）。

在 `D:\SVN\usr\weihaoyu\Rigel\cp_test` 目录下打开终端，按以下步骤操作：

**第一步：初始化仓库**
```powershell
git init
```

**第二步：创建 `.gitignore` 文件 (最重要的一步)**
你需要创建一个名为 `.gitignore` 的文件，防止 Git 追踪庞大且不可迁移的虚拟环境。
在终端运行：
```powershell
# 创建 .gitignore 文件并写入规则
echo "cp_test/" > .gitignore
echo ".ipynb_checkpoints/" >> .gitignore
echo "__pycache__/" >> .gitignore
```
*   `cp_test/`: 忽略虚拟环境文件夹（因为里面的 exe 路径是绝对路径，迁移后会失效，必须在新电脑重装）。
*   `.ipynb_checkpoints/`: 忽略 Jupyter 的自动存档。

**第三步：提交文件**
现在 Git 只会识别你的代码和配置文件。
```powershell
git add .
git commit -m "Initial setup for DP2031 control project"
```
此时，Git 只会打包：`dp2031_control.ipynb` [1], `requirement.txt`, `setup_kernel.bat` 以及 `.gitignore` 本身。

**第四步：推送到远程仓库**
你需要在 GitHub、Gitee 或公司的 GitLab 上建一个空仓库，然后关联推送：
```powershell
git remote add origin <你的远程仓库地址>
git push -u origin master
```

---

### 2. 在新电脑上恢复环境

到了新电脑上，你只需要拉取代码，然后利用我们准备好的脚本**重建环境**。

**第一步：克隆仓库**
```powershell
git clone <你的远程仓库地址> dp2031_project
cd dp2031_project
```

**第二步：一键重建环境**
因为我们忽略了 `cp_test` 文件夹，现在需要重新生成它。依次运行以下命令：

1.  **创建环境：**
    ```powershell
    python -m venv cp_test
    ```
2.  **激活环境：**
    ```powershell
    .\cp_test\Scripts\activate
    ```
3.  **安装依赖：**
    ```powershell
    pip install -r requirement.txt
    ```
4.  **配置内核 (运行你提交的脚本)：**
    ```powershell
    .\setup_kernel.bat
    ```

### 3. 为什么这样做更好？
*   **体积小：** 你不需要传输几百兆的 `cp_test` 文件夹，只需要传输几十 KB 的代码和文本文件。
*   **兼容性好：** 虚拟环境是在新电脑上利用本地 Python 重新构建的，完全避免了“路径乱码”或“找不到解释器”的错误。
*   **可回溯：** 如果你在新电脑上修改了 `dp2031_control.ipynb`（比如修改了 VISA 地址 [1]），你可以提交更改，回家后拉取代码，两边同步。

**最后提醒：** 即使使用 Git 迁移，在新电脑打开 Notebook 后，依然不要忘记修改代码中的 `dp2031_visa_address` [1]，因为硬件连接地址是不会随 Git 同步自动变更的。