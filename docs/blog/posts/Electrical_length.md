---
date:
  created: 2025-02-28
draft: true
categories:
  - RF
tags:
  - Transmission Line
  - Electrical Length
  - ADS
authors:
  - why

---

# 传输线电长度与 $90^\circ$ 频率估算

> **已知：** Line Designer 在 $100\,\text{GHz}$ 给出 $65\,\mu\text{m}$ 线段的电长度约 $18.6568^\circ$、延时约 $0.518244\,\text{ps}$。  
> **假设：** 频率范围内有效介电常数近似不变。  
> **待验证：** 到数百 GHz 时必须以工艺的色散模型扫频确认。

计算传输线达到 $90^\circ$（即 quarter-wavelength，$\lambda/4$）的频率，可先使用电长度和频率的线性比例关系，再以色散仿真复核。

---

## 一、 一阶近似计算（工程常用）

在常规毫米波频段内，有效介电常数 $\varepsilon_{eff}$ 变化较小，传输线的**电长度（Electrical Length, $E$）与频率（$f$）成正比**：

$$E \propto f \implies \frac{E_1}{f_1} = \frac{E_2}{f_2}$$

已知条件：

* $f_1 = 100\text{ GHz}$
* $E_1 \approx 18.6568^\circ$
* 目标电长度 $E_2 = 90^\circ$

代入推导目标频率 $f_2$：

$$f_2 = f_1 \times \frac{E_2}{E_1} = 100\text{ GHz} \times \frac{90^\circ}{18.6568^\circ} \approx \mathbf{482.4\text{ GHz}}$$

---

## 二、 物理量交叉验证（从延时与相速推导）

从你截图中的 Line Designer 提取数据同样可以进行验证：

1. **利用时间延迟（Delay, $\tau$）计算**：
* 截图给出总延时 $\tau = 0.000518244\text{ ns} = 0.518244\text{ ps}$
* 相移与延时的关系为：$\theta = 360^\circ \times f \times \tau$
* 当 $\theta = 90^\circ$ 时：

$$f = \frac{90^\circ}{360^\circ \times \tau} = \frac{1}{4 \tau} = \frac{1}{4 \times 0.518244\text{ ps}} \approx \mathbf{482.4\text{ GHz}}$$




2. **利用相位常数（$\beta$）计算**：
* 截图中的 $\mathrm{Imag}(\mathrm{Gamma}) = \beta = 5009.58\text{ rad/m}$
* 物理长度 $L = 65\,\mu\text{m} = 6.5 \times 10^{-5}\text{ m}$
* 当前弧度相移 $\theta_{\text{rad}} = \beta \cdot L = 5009.58 \times 6.5 \times 10^{-5} \approx 0.32562\text{ rad}$
* 换算成角度：$0.32562 \times \frac{180^\circ}{\pi} \approx 18.657^\circ$
* 按比例放大至 $90^\circ$ ($\pi/2\text{ rad}$) 同样得出约 **$482.4\text{ GHz}$**。



---

## 三、 工程实操注意事项

1. **色散效应（Dispersion）**：
* 在数百 GHz，$\varepsilon_{eff}$ 和损耗通常呈现频率依赖，方向与幅度取决于工艺与结构。因此线性估算仅用于初值，不能据此给出精确的 $90^\circ$ 频率。


2. **ADS 工具快速验证**：
* 如果你需要精准值，可以在该 Line Designer 界面直接将单选框由 **Analyze** 切到 **Sweep**（扫频），或者在表格顶部的 **freq** 变量处将输入值直接改为 **$482.4\text{ GHz}$** 单点重新 Analyze 验证。

电长度（Electrical Length）本质上是用来衡量信号传播这段物理距离时，经历了多少相移（Phase Shift）的物理量。

它可以分别用角度（度 $\text{deg}$ 或 弧度 $\text{rad}$）**或者**波长倍数（$\lambda$ 的比例）来表达。

---

## 1. 核心计算公式

传输线的电长度 $\theta$（也就是你看到的 $E$）主要有以下几种计算和推导形式：

### ① 基本定义公式（基于波长 $\lambda$）

电长度等于传输线的物理长度 $l$ 相对于介质中信号波长 $\lambda_g$ 的比值：


$$\theta_{\text{rad}} = 2\pi \cdot \frac{l}{\lambda_g} \qquad (\text{单位：rad})$$


换算成角度 $\text{deg}$：


$$E = 360^\circ \cdot \frac{l}{\lambda_g} \qquad (\text{单位：deg})$$

---

### ② 展开公式（引入频率 $f$ 与有效介电常数 $\varepsilon_{eff}$）

因为介质中的波长为 $\lambda_g = \frac{v_p}{f} = \frac{c}{f \sqrt{\varepsilon_{eff}}}$，将它代入上面的公式，即可得到与频率 $f$ 的关系：

$$E = 360^\circ \cdot \frac{l \cdot f \sqrt{\varepsilon_{eff}}}{c}$$

其中：

* $l$：传输线物理长度（单位：$\text{m}$）
* $f$：工作频率（单位：$\text{Hz}$）
* $c$：真空中光速（$\approx 3 \times 10^8\text{ m/s}$）
* $\varepsilon_{eff}$：传输线的有效介电常数（Effective Dielectric Constant）

> **为什么说“电长度与频率成正比”？**
> 从这个公式可以直接看出：当传输线结构固定（即 $l$ 和 $\varepsilon_{eff}$ 固定）时，$E$ 与 $f$ 是完全成一次线性正比关系的（即 $E \propto f$）。

---

### ③ 基于相位常数 $\beta$ 的计算

在传输线理论（比如前面的 $\gamma = \alpha + j\beta$）中，电长度其实就是**相位常数与物理长度的乘积**：

$$\theta_{\text{rad}} = \beta \cdot l$$

* 其中 $\beta = \frac{2\pi}{\lambda_g} = \frac{\omega}{v_p} = \frac{2\pi f \sqrt{\varepsilon_{eff}}}{c}$。
* 将弧度转换为角度：

$$E = \beta \cdot l \cdot \frac{180^\circ}{\pi}$$



---

### ④ 基于信号延时 $\tau$ 的计算

如果已知信号通过这段传输线的**时间延迟（Time Delay, $\tau$）**：


$$\tau = \frac{l}{v_p} = \frac{l \sqrt{\varepsilon_{eff}}}{c}$$


那么电长度为：


$$E = 360^\circ \cdot f \cdot \tau$$

---

## 2. 结合你之前 Line Designer 截图的数值套入验证

我们在上一张 Line Designer 截图里的数据：

* $f = 100\text{ GHz} = 10^{11}\text{ Hz}$
* $l = 65\ \mu\text{m} = 6.5 \times 10^{-5}\text{ m}$
* $\varepsilon_{eff} = 5.71326$

**代入公式进行计算：**


$$E = 360^\circ \times \frac{6.5 \times 10^{-5} \times 10^{11} \times \sqrt{5.71326}}{3 \times 10^8}$$

$$E = 360^\circ \times \frac{6.5 \times 10^6 \times 2.39024}{3 \times 10^8} \approx 360^\circ \times 0.051788 \approx \mathbf{18.64^\circ}$$

*这与软件计算出的 `18.6568 deg` 几乎一致（微弱差距源于软件内部 $c$ 取精细值或包含轻微色散）。*

---

## 总结

电长度计算其实就记住一句话：**“物理长度占了介质波长（$\lambda_g$）的几分之几，再乘以 $360^\circ$”**。
