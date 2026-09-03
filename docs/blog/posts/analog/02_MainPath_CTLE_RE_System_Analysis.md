---
date:
  created: 2026-09-03
draft: true
categories:
  - Analog Circuit
tags:
  - Gilbert Cell
  - CTLE
  - Emitter Degeneration
  - Gain
  - Noise
---

# 主信号通路 + 可调 CTLE 辅助通路下 Gilbert 退化电阻的增益与噪声分析

## 1. 场景

本文讨论以下系统：

$$
\boxed{
\text{固定增益主信号通路}
+
\text{可调 CTLE 辅助通路}
}
$$

CTLE 辅助通路采用 Gilbert steering core，控制电压为：

$$
V_C=
V_{\mathrm{BIAS\_CTLE\_P}}
-
V_{\mathrm{BIAS\_CTLE\_N}}
$$

Gilbert core 中的发射极退化电阻记为：

$$
R_{E,\mathrm{CTLE}}
$$

目标是分析其对整个系统的增益、输出噪声和 SNR 的影响。

## 2. 系统模型

总输出信号：

$$
\boxed{
V_{out,s}
=
[A_0(f)+A_{\mathrm{CTLE}}(f)]V_{in}
}
\tag{1}
$$

其中：

- $A_0(f)$：固定主通路；
- $A_{\mathrm{CTLE}}(f)$：可调 CTLE 辅助通路。

因此：

$$
\boxed{
A_{\mathrm{total}}(f)
=
A_0(f)+A_{\mathrm{CTLE}}(f)
}
\tag{2}
$$

若两条通路内部噪声近似不相关：

$$
\boxed{
S_{n,out}
=
S_{n,0}
+
S_{n,\mathrm{CTLE}}
}
\tag{3}
$$

所以系统 SNR：

$$
\boxed{
\mathrm{SNR}(f)
=
\frac{
|A_0(f)+A_{\mathrm{CTLE}}(f)|^2S_{in}(f)
}{
S_{n,0}(f)+S_{n,\mathrm{CTLE}}(f)
}
}
\tag{4}
$$

式 (4) 是本场景的核心评价公式。

## 3. Gilbert CTLE 的增益

定义：

$$
\boxed{
m=\frac{I_1-I_2}{I_T}
}
\tag{5}
$$

DC steering 关系：

$$
\boxed{
V_C
=
2V_T\operatorname{atanh}(m)
+
I_TR_{E,\mathrm{CTLE}}m
}
\tag{6}
$$

AC steering gain：

$$
\boxed{
K=
\frac{m}
{
1+\dfrac{
I_TR_{E,\mathrm{CTLE}}(1-m^2)
}{
2V_T
}
}
}
\tag{7}
$$

可将 CTLE 增益近似表示为：

$$
\boxed{
A_{\mathrm{CTLE}}(f)
=
A_{\mathrm{CTLE},0}(f)K
}
\tag{8}
$$

因此固定 $m$ 时：

$$
\boxed{
R_{E,\mathrm{CTLE}}\uparrow
\Rightarrow
|A_{\mathrm{CTLE}}|\downarrow
}
\tag{9}
$$

## 4. CTLE OFF：$m=0$

当：

$$
m=0
$$

则：

$$
\boxed{
K=0
}
\tag{10}
$$

所以：

$$
\boxed{
A_{\mathrm{CTLE}}=0
}
\tag{11}
$$

总信号增益仅由主通路决定：

$$
\boxed{
A_{\mathrm{total}}=A_0
}
\tag{12}
$$

这里需要区分两类噪声：

1. 位于 Gilbert steering core 之前、与输入信号走相同传递路径的噪声：理想情况下也会受到 $K=0$ 的抑制；
2. Gilbert core 内部产生的 local noise：不会因为 CTLE 信号抵消而自动消失。

后一类包括：

- steering BJT collector shot noise；
- $R_{E,\mathrm{CTLE}}$ thermal noise；
- BJT base resistance noise；
- bias network noise；
- 后级负载噪声。

因此：

$$
\boxed{
\mathrm{SNR}_{m=0}
=
\frac{
|A_0|^2S_{in}
}{
S_{n,0}+S_{n,\mathrm{CTLE}}
}
}
\tag{13}
$$

此时主通路信号不受 $R_{E,\mathrm{CTLE}}$ 影响。

## 5. $m=0$ 时的 CTLE local noise

在中点：

$$
\boxed{
g_m=\frac{I_T}{2V_T}
}
\tag{14}
$$

若只考虑 steering BJT collector shot noise 和四个退化电阻 thermal noise，则：

$$
\boxed{
S_{i,\mathrm{CTLE}}
\approx
\frac{
4qI_T+
16kTR_{E,\mathrm{CTLE}}g_m^2
}{
(1+g_mR_{E,\mathrm{CTLE}})^2
}
}
\tag{15}
$$

等价地：

$$
\boxed{
S_{i,\mathrm{CTLE}}
\approx
8kTg_m
\frac{
1+2g_mR_{E,\mathrm{CTLE}}
}{
(1+g_mR_{E,\mathrm{CTLE}})^2
}
}
\tag{16}
$$

在该一阶模型中：

$$
\boxed{
R_{E,\mathrm{CTLE}}\uparrow
\Rightarrow
S_{n,\mathrm{CTLE}}\downarrow
\qquad(m=0)
}
\tag{17}
$$

由于此时主通路信号增益 $A_0$ 不变，因此：

$$
\boxed{
R_{E,\mathrm{CTLE}}\uparrow
\Rightarrow
\mathrm{SNR}_{\mathrm{system}}\uparrow
\qquad(m=0)
}
\tag{18}
$$

注意：这里讨论的是整个“主通路 + CTLE”系统的 SNR，而不是 CTLE 单元自身的 input-referred noise。

## 6. CTLE ON：$m\neq0$

当：

$$
m\neq0
$$

CTLE 开始提供有效信号：

$$
A_{\mathrm{CTLE}}\neq0
$$

此时：

$$
R_{E,\mathrm{CTLE}}\uparrow
\Rightarrow
|K|\downarrow
\Rightarrow
|A_{\mathrm{CTLE}}|\downarrow
\tag{19}
$$

另一方面，退化又可能降低 Gilbert core 的 local output noise。

于是形成：

$$
\boxed{
\begin{cases}
|A_{\mathrm{CTLE}}|\downarrow\\
S_{n,\mathrm{CTLE}}\downarrow\quad\text{(可能)}
\end{cases}
}
\tag{20}
$$

真正应评价：

$$
\boxed{
\mathrm{SNR}(R_E,m,f)
=
\frac{
|A_0(f)+A_{\mathrm{CTLE}}(R_E,m,f)|^2S_{in}(f)
}{
S_{n,0}(f)+S_{n,\mathrm{CTLE}}(R_E,m,f)
}
}
\tag{21}
$$

因此：

$$
\boxed{
m\neq0:
\quad
R_{E,\mathrm{CTLE}}\uparrow
\text{ 后 SNR 不一定改善}
}
\tag{22}
$$

## 7. Boost 与 Cut 状态

由于：

$$
m>0
\quad\text{或}\quad
m<0
$$

CTLE 通路可以与主通路同相或反相叠加：

$$
\boxed{
A_{\mathrm{total}}
=
A_0+A_{\mathrm{CTLE}}
}
\tag{23}
$$

因此：

- boost 状态下，CTLE 增强目标频段；
- cut 状态下，CTLE 抵消目标频段；
- 当 $A_0+A_{\mathrm{CTLE}}\approx0$ 时，即使 absolute output noise 较低，SNR 仍可能明显恶化。

对于真正的 CTLE，更严格的分析应保留复数频率响应：

$$
\boxed{
A_{\mathrm{total}}(f)
=
A_0(f)+A_{\mathrm{CTLE}}(f)
}
\tag{24}
$$

## 8. 系统级结论

### CTLE OFF：$m\approx0$

$$
\boxed{
\begin{cases}
A_{\mathrm{CTLE}}\approx0\\
A_{\mathrm{total}}\approx A_0\\
R_E\uparrow\Rightarrow\text{Gilbert local noise 可能下降}\\
\therefore\text{系统 SNR 可能改善}
\end{cases}
}
\tag{25}
$$

### CTLE ON：$|m|>0$

$$
\boxed{
\begin{cases}
R_E\uparrow\\
|A_{\mathrm{CTLE}}|\downarrow\\
S_{n,\mathrm{CTLE}}\downarrow\ \text{(可能)}\\
\mathrm{SNR}\text{ 取决于两者竞争}
\end{cases}
}
\tag{26}
$$

因此 $R_{E,\mathrm{CTLE}}$ 的目标不是单纯最小化噪声或最大化 CTLE 增益，而是：

$$
\boxed{
\text{OFF 状态低噪声}
+
\text{ON 状态足够增益}
+
\text{合适的控制线性度}
}
\tag{27}
$$

## 9. 当前电流下的设计尺度

若单个 steering pair：

$$
I_T\approx10\,\mathrm{mA}
$$

则 $m=0$ 时单管约为 $5\,\mathrm{mA}$：

$$
g_m\approx0.19\,\mathrm{S}
$$

所以：

$$
\boxed{
\frac1{g_m}\approx5.2\,\Omega
}
\tag{28}
$$

这意味着：

- $R_E\approx2\,\Omega$：弱到中等退化；
- $R_E\approx5\,\Omega$：已经明显；
- $R_E\approx10\,\Omega$：较强退化；
- $R_E\approx20\,\Omega$：强退化。

## 10. ADS 仿真建议

建议 sweep：

$$
\boxed{
R_{E,\mathrm{CTLE}}
=
0,\ 2,\ 5,\ 10,\ 20\,\Omega
}
\tag{29}
$$

至少检查以下控制状态：

1. $m=0$：CTLE OFF；
2. 中等 boost；
3. 最大 boost；
4. 中等 cut；
5. 最大 cut。

每个状态观察：

- $A_{\mathrm{CTLE}}(f)$；
- $A_{\mathrm{total}}(f)$；
- output noise density；
- 系统 input-referred noise / SNR。

系统 input-referred noise 可用：

$$
\boxed{
v_{n,\mathrm{in,eq}}(f)
=
\frac{v_{n,out}(f)}
{|A_{\mathrm{total}}(f)|}
}
\tag{30}
$$

进行统一比较。

## 11. 最终结论

对于“固定主通路 + 可调 CTLE 辅助通路”，不能只用 Gilbert 单元自身的 input-referred noise 判断 $R_{E,\mathrm{CTLE}}$ 是否合适。

最终应围绕：

$$
\boxed{
\mathrm{SNR}
=
\frac{
|A_0+A_{\mathrm{CTLE}}|^2S_{in}
}{
S_{n,0}+S_{n,\mathrm{CTLE}}
}
}
$$

进行系统级评价。

核心结论：

- $m=0$ 时，CTLE 不提供有效信号；若增大 $R_E$ 能降低 Gilbert local noise，则系统 SNR 可改善；
- $m\neq0$ 时，增大 $R_E$ 同时降低 CTLE gain 与 local noise，SNR 不一定改善；
- boost/cut 状态应分别评价；
- 最终 $R_E$ 应通过多个控制码、多个频率点下的 Gain–Noise trade-off 确定。
