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

# Jupyter kernel
An exmaple of jupuyter :

<!-- more -->
为了在新电脑上最便捷地运行 `dp2031_control.ipynb`，你不需要拷贝所有文件（特别是不要拷贝 `cp_test` 文件夹，因为它包含绝对路径，复制到新电脑无法使用）。

以下是具体的操作清单和步骤：

### 1. 需要拷贝的文件（仅需这 3 个）
请将以下文件复制到优盘或通过网络发送到新电脑的同一个文件夹中：
1.  **`dp2031_control.ipynb`** (核心脚本文件 [1])
2.  **`requirement.txt`** (依赖包列表，用于快速安装环境)
3.  **`setup_kernel.bat`** (你刚才编写的自动配置脚本，非常方便)

*注意：`cp_test` 文件夹是虚拟环境，拷贝过去也无法使用，请在新电脑上重新生成。`dp2031_power_on_current.png` 是旧的测试结果图片，不需要拷贝。*

### 2. 新电脑上的便捷部署步骤

**第一步：准备基础环境**
1.  确保新电脑已安装 **Python**。
2.  **关键：** 安装 **VISA 驱动**（如 NI-VISA 或 Keysight IO Libraries），因为脚本需要通过 `pyvisa` 连接电源 [1]。

**第二步：一键恢复环境**
在新电脑上打开存放上述 3 个文件的文件夹，在文件地址栏输入 `cmd` 或 `powershell` 打开终端，依次运行以下 3 行命令：

1.  **创建新环境：**
    ```bash
    python -m venv cp_test
    ```
2.  **激活环境：**
    ```bash
    .\cp_test\Scripts\activate
    ```
3.  **一键安装依赖（包含Jupyter）：**
    ```bash
    pip install -r requirement.txt
    ```

    ```txt 
    jupyterlab==4.5.1
    numpy==2.3.3
    matplotlib==3.10.7
    pyvisa==1.15.0
    ipykernel==7.1.0
    pyserial==3.5
    pandas==2.3.3
    ```
    *(注：如果你的 `requirement.txt` 里没有包含 ipykernel，最好手动补一句 `pip install ipykernel`，或者依靠下一步的 bat 脚本)*

**第三步：配置 Jupyter 内核**
确保环境处于激活状态（命令行前有 `(cp_test)`），直接运行你拷贝过来的批处理文件：

```bash
.\setup_kernel.bat
```

```bash
@echo off
echo Installing ipykernel...
pip install ipykernel

echo.
echo Registering the current environment to Jupyter...
python -m ipykernel install --user --name=python_dp2031 --display-name "Python (DP2031)"

echo.
echo === Done! ===
echo You can now open Jupyter Notebook and select the "Python (DP2031)" kernel.
pause
```

这会自动安装 `ipykernel` 并将名为 "Python (DP2031)" 的内核注册到新电脑的 Jupyter 中。

**第四步：运行与修改**
1.  输入 `jupyter notebook` 打开脚本。
2.  **重要提示：** 打开 `dp2031_control.ipynb` 后，务必在 **第 2 部分 (连接DP2031电源)** 中，运行 `rm.list_resources()` 查看新电脑分配的 USB 地址，并修改 `dp2031_visa_address` 变量 [1]，因为新电脑的硬件 ID 肯定与旧电脑不同。