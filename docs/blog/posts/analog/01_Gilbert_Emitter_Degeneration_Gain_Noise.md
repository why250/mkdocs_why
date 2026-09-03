---
date:
  created: 2026-09-03
draft: true
categories:
  - Analog Circuit
tags:
  - Gilbert Cell
  - Emitter Degeneration
  - Gain
  - Noise
---

# 发射极退化电阻对 Gilbert 单元增益与噪声的影响

## 1. 目的

本文分析 Gilbert steering pair 中发射极退化电阻 $R_E$ 对 DC steering、AC 信号增益以及噪声的影响。

## 2. 模型与定义

定义单个 steering pair 的总电流：

$$
I_T=I_1+I_2
$$

定义 DC steering coefficient：

$$
\boxed{
m=\frac{I_1-I_2}{I_T}
}
\tag{1}
$$

因此：

$$
I_1=\frac{I_T}{2}(1+m),\qquad
I_2=\frac{I_T}{2}(1-m)
\tag{2}
$$

控制电压定义为：

$$
V_C=V_{B1}-V_{B2}
$$

两侧发射极分别串联相同退化电阻 $R_E$。

假设两只 BJT 匹配，忽略 Early effect，$\beta$ 足够大，仅分析一阶小信号和白噪声。

## 3. DC steering 特性

由 BJT 指数关系及退化电阻压降：

$$
\boxed{
V_C=
2V_T\operatorname{atanh}(m)+I_TR_Em
}
\tag{3}
$$

当 $R_E=0$ 时：

$$
\boxed{
m=\tanh\left(\frac{V_C}{2V_T}\right)
}
\tag{4}
$$

因此固定 $V_C$ 时：

$$
\boxed{
R_E\uparrow\Rightarrow |m|\downarrow
}
\tag{5}
$$

即 $R_E$ 会使 Gilbert switching curve 变缓、控制范围变宽，并提高控制特性的线性度。

## 4. AC steering gain

输出差分电流：

$$
\boxed{
I_{od}=I_1-I_2=mI_T
}
\tag{6}
$$

固定 $V_C$，对式 (3) 求全微分：

$$
0=
\left[
\frac{2V_T}{1-m^2}+I_TR_E
\right]dm
+
R_Em\,dI_T
\tag{7}
$$

所以：

$$
\boxed{
\frac{dm}{dI_T}
=
-\frac{R_Em}
{
\dfrac{2V_T}{1-m^2}+I_TR_E
}
}
\tag{8}
$$

又因为：

$$
dI_{od}=m\,dI_T+I_T\,dm
\tag{9}
$$

定义 AC steering current gain：

$$
K\equiv
\left.
\frac{\partial I_{od}}{\partial I_T}
\right|_{V_C}
$$

得到：

$$
\boxed{
K=
m
\frac{
\dfrac{2V_T}{1-m^2}
}{
\dfrac{2V_T}{1-m^2}+I_TR_E
}
}
\tag{10}
$$

等价地：

$$
\boxed{
K=
\frac{m}
{
1+\dfrac{I_TR_E(1-m^2)}{2V_T}
}
}
\tag{11}
$$

因此：

$$
\boxed{
R_E=0\Rightarrow K=m
}
\tag{12}
$$

而：

$$
\boxed{
R_E>0\Rightarrow |K|<|m|
}
\tag{13}
$$

所以存在退化电阻时，需要区分 DC steering coefficient $m$ 与真正的 AC signal gain $K$。

## 5. 退化电阻热噪声

单个电阻的 thermal-noise PSD：

$$
\boxed{
S_{v,R}=4kTR_E
}
\tag{14}
$$

一个 steering pair 中两个电阻互不相关，因此差分噪声为：

$$
\boxed{
S_{v,R,\mathrm{diff}}=8kTR_E
}
\tag{15}
$$

只考虑 $R_E$ 噪声且令 $dI_T=0$，可得：

$$
\boxed{
dm=
-
\frac{dv_{nR}}
{
\dfrac{2V_T}{1-m^2}+I_TR_E
}
}
\tag{16}
$$

于是单个 steering pair 的输出差分电流噪声：

$$
\boxed{
S_{i,R,\mathrm{pair}}
=
8kTR_E
\left[
\frac{I_T}
{
\dfrac{2V_T}{1-m^2}+I_TR_E
}
\right]^2
}
\tag{17}
$$

完整 Gilbert quad 有两个对称 steering pair，因此：

$$
\boxed{
S_{i,R,\mathrm{quad}}
=
16kTR_E
\left[
\frac{I_T}
{
\dfrac{2V_T}{1-m^2}+I_TR_E
}
\right]^2
}
\tag{18}
$$

这里的 $I_T$ 表示单个 steering pair 的总电流。

## 6. $m=0$ 时的简化结果

在中点：

$$
m=0
$$

每只 steering BJT 的电流约为 $I_T/2$，因此：

$$
\boxed{
g_m=\frac{I_T}{2V_T}
}
\tag{19}
$$

式 (17) 可化为：

$$
\boxed{
S_{i,R,\mathrm{pair}}
=
8kTR_E
\left(
\frac{g_m}{1+g_mR_E}
\right)^2
}
\tag{20}
$$

可见虽然 $R_E$ 自身产生热噪声，但局部反馈因子 $1+g_mR_E$ 会抑制其传递到输出端的噪声。

## 7. steering BJT shot noise + RE noise

在 $m=0$ 附近，如果仅考虑 steering BJT collector shot noise 和 $R_E$ thermal noise，则完整 quad 的输出差分电流噪声近似为：

$$
\boxed{
S_{i,\mathrm{quad}}
\approx
\frac{
4qI_T+16kTR_Eg_m^2
}{
(1+g_mR_E)^2
}
}
\tag{21}
$$

利用 $I_T=2V_Tg_m$、$kT=qV_T$：

$$
\boxed{
S_{i,\mathrm{quad}}
\approx
8kTg_m
\frac{1+2g_mR_E}
{(1+g_mR_E)^2}
}
\tag{22}
$$

在这一阶模型下，增大 $R_E$ 会降低 Gilbert core 的局部输出噪声。

## 8. Input-Referred Noise

对于 $m\neq0$：

$$
\boxed{
S_{i,\mathrm{in,eq}}
=
\frac{S_{i,\mathrm{out}}}{|K|^2}
}
\tag{23}
$$

仅考虑四个 $R_E$ 的噪声时：

$$
\boxed{
S_{i,\mathrm{in,eq},R}
=
16kTR_E
\left[
\frac{I_T(1-m^2)}
{2mV_T}
\right]^2
}
\tag{24}
$$

所以在固定 $I_T,m$ 条件下：

$$
\boxed{
S_{i,\mathrm{in,eq},R}\propto R_E
}
\tag{25}
$$

而噪声幅度：

$$
\boxed{
i_{n,\mathrm{in,eq},R}\propto\sqrt{R_E}
}
\tag{26}
$$

因此，output noise 下降并不等价于 input-referred noise 改善，因为 signal gain $K$ 也同时下降。

## 9. 重要结论

$$
\boxed{
R_E\uparrow
\Rightarrow
\begin{cases}
\text{控制曲线更平缓}\\
\text{控制范围更宽}\\
\text{控制线性度提高}\\
|K|\downarrow\\
\text{Gilbert core 局部输出噪声可能下降}\\
\text{input-referred noise 通常不改善}
\end{cases}
}
\tag{27}
$$

因此 $R_E$ 本质上实现的是：

$$
\boxed{
\text{控制线性度 / 控制范围}
\quad\Longleftrightarrow\quad
\text{AC 增益 / 输入等效噪声}
}
$$

的权衡。

一个实用设计尺度为：

$$
\boxed{
R_E\sim\frac1{g_m}
}
\tag{28}
$$

当 $g_mR_E\sim1$ 时，退化已经很明显。

例如 $I_T=10\,\mathrm{mA}$，在 $m=0$ 附近单管 $I_C\approx5\,\mathrm{mA}$：

$$
g_m\approx0.193\,\mathrm{S},\qquad
\frac1{g_m}\approx5.2\,\Omega
$$

因此 $5\sim10\,\Omega$ 已属于明显退化。

## 10. 仿真建议

建议 sweep：

$$
R_E=0,\ 2,\ 5,\ 10,\ 20\,\Omega
$$

重点观察：

1. $m$ vs. $V_C$；
2. $K$ vs. $V_C$；
3. output noise density；
4. input-referred noise；
5. 不同 $m$ 工作点下的 Gain–Noise trade-off。

最终应结合完整 BJT 模型、基区电阻、偏置网络、负载和高频寄生进行仿真验证。
