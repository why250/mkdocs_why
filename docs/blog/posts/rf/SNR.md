---
date:
  created: 2025-02-28
draft: true
categories:
  - RF
tags:
  - PA
authors:
  - why
---

# 0–55 GHz 宽带输出 SNR 的计算方法

## 1. 问题

目标是评估一个宽带 VGA 在输入信号为 **0.89 Vpp**、噪声带宽为 **0–55 GHz** 时的输出信噪比 `SNRout`。

已知 ADS SP/Noise 仿真可以得到：

* 噪声系数 \(NF(f)\)
* 小信号增益 \(S_{21}(f)\)

最初采用的方法是：

$$
SNR_{out}=SNR_{in}-NF
$$

其中输入信号按 0.89 Vpp 正弦波、100 Ω 计算：

$$
P_{sig,in}=\frac{V_{pp}^2}{8R}
$$

输入端 0–55 GHz 热噪声功率：

$$
P_{n,in}=kT_0B
$$

或写成 dBm：

$$
P_{n,in,dBm}
=
-174+10\log_{10}(55\times10^9)
$$

因此：

$$
SNR_{in}\approx66.55\text{ dB}
$$

然后取 0–55 GHz 中最大的：

$$
NF_{max}\approx22.0\text{ dB}
$$

得到：

$$
SNR_{out}\approx66.55-22.0
\approx44.5\text{ dB}
$$

问题在于：

> \(NF\) 和 \(S_{21}\) 都随频率变化，直接取最大 NF 是否合理？是否应该对 0–55 GHz 的 NF 或 SNR 求平均？

---

# 2. 简化算法

最简单的方法仍然是：

$$
\boxed{
SNR_{out}\approx SNR_{in}-NF_{max}
}
$$

这种方法的优点是简单，而且通常比较保守。

但它隐含了一个假设：

$$
G(f)\approx G(f_s)
$$

即整个噪声带宽内的噪声与信号经历近似相同的增益。

如果电路频响比较平坦，这种近似通常没有太大问题。

但对于当前 VGA：

$$
S_{21}(0)\approx-17.0\text{ dB}
$$

而：

$$
S_{21}(55G)\approx-12.2\text{ dB}
$$

0–55 GHz 内增益变化接近 5 dB，因此这个假设已经不太成立。

另外，也不推荐直接计算：

$$
\operatorname{average}(NF_{dB})
$$

或者：

$$
\operatorname{average}(SNR_{dB})
$$

因为噪声功率需要在**线性功率域**进行累加，而不能直接对 dB 求平均。

因此：

> `NFmax` 法适合快速估算，但不适合作为严格的 0–55 GHz 宽带 SNR 定义。

---

# 3. 严格算法

严格计算应该从**输出噪声功率谱密度**开始。

ADS 给出的 NF 为 dB，需要先转换为线性噪声因子：

$$
\boxed{
F(f)=10^{NF(f)/10}
}
$$

电路的线性功率增益为：

$$
\boxed{
G(f)=|S_{21}(f)|^2
}
$$

对于标准噪声温度：

$$
T_0=290\text{ K}
$$

Boltzmann 常数：

$$
k=1.380649\times10^{-23}\text{ J/K}
$$

因此输入端热噪声功率谱密度为：

$$
kT_0
$$

约等于：

$$
-174\text{ dBm/Hz}
$$

输出端噪声 PSD 为：

$$
\boxed{
N_{out}(f)
=
kT_0G(f)F(f)
}
$$

单位：

$$
W/Hz
$$

所以 0–55 GHz 内总输出噪声为：

$$
\boxed{
P_{n,out}
=
\int_0^{55G}
kT_0G(f)F(f)\,df
}
$$

这一步同时考虑了：

* NF 随频率变化；
* 增益随频率变化；
* 每个频点的噪声在输出端受到不同增益。

---

对于 0.89 Vpp 正弦信号：

$$
\boxed{
P_{sig,in}
=
\frac{V_{pp}^2}{8R}
}
$$

假设信号位于 \(f_s\)，则输出信号功率为：

$$
\boxed{
P_{sig,out}(f_s)
=
P_{sig,in}G(f_s)
}
$$

最终：

$$
\boxed{
SNR_{out}(f_s)
=
10\log_{10}
\left[
\frac
{P_{sig,in}G(f_s)}
{\displaystyle
\int_0^{55G}
kT_0G(f)F(f)\,df}
\right]
}
$$

这就是当前问题更严格的 SNR 定义。

---

# 4. ADS 实现

在 ADS Data Display 中可以直接定义：

```text
NF = nf(2)

F = 10**(NF/10)

G = mag(S(2,1))**2

k  = 1.380649e-23
T0 = 290

Nout_PSD = k*T0*G*F

Pn_Out_W = integrate(Nout_PSD,0,55e9)

Pn_Out_dBm = 10*log10(Pn_Out_W)+30
```

当前仿真得到：

$$
P_{n,out}
\approx8.03\times10^{-10}W
$$

即：

$$
\boxed{
P_{n,out}\approx-60.95\text{ dBm}
}
$$

---

信号功率计算：

```text
Vin_Vpp = 0.89

R = 100

Pin_Sig_W = Vin_Vpp**2/(8*R)

G_Sig = mag(S(2,1))**2

Pout_Sig_W = Pin_Sig_W*G_Sig

Pout_Sig_dBm = 10*log10(Pout_Sig_W)+30

SNRout = 10*log10(Pout_Sig_W/Pn_Out_W)
```

这样得到的 `SNRout` 是一个随信号频率变化的曲线：

$$
SNR_{out}(f_s)
$$

其中：

* 分母始终是 **0–55 GHz 的全部输出积分噪声**；
* 分子是信号在当前频率 \(f_s\) 上的输出功率。

---

还可以进一步定义一个**等效宽带 NF**。

设：

$$
B=55\text{ GHz}
$$

则：

$$
\boxed{
F_{eff}(f_s)
=
\frac
{\displaystyle\int_0^B G(f)F(f)df}
{B\,G(f_s)}
}
$$

对应：

$$
\boxed{
NF_{eff}(f_s)
=
10\log_{10}F_{eff}(f_s)
}
$$

ADS 中：

```text
BW = 55e9

F_eff = Pn_Out_W/(k*T0*BW*G_Sig)

NF_eff = 10*log10(F_eff)
```

于是仍然可以写成：

$$
\boxed{
SNR_{out}
=
SNR_{in}-NF_{eff}
}
$$

只是这里的 \(NF_{eff}\) 已经同时包含了 NF 和频率响应 \(S_{21}\) 的影响。

---

# 5. 仿真现象解释

最初使用最大 NF：

$$
NF_{max}\approx22.0\text{ dB}
$$

得到：

$$
SNR_{out}\approx44.5\text{ dB}
$$

而采用积分方法以后，0 Hz 附近得到：

$$
\boxed{
SNR_{out}(0)\approx43.9\text{ dB}
}
$$

反而比原来的 44.5 dB 更低。

这个结果是合理的。

原因在于当前 VGA 的增益随频率升高：

$$
S_{21}(0)\approx-17.0\text{ dB}
$$

而：

$$
S_{21}(55G)\approx-12.2\text{ dB}
$$

也就是说：

> 0 Hz 信号只获得约 -17 dB 的增益，但 0–55 GHz 内相当一部分噪声却获得了更高的增益。

因此对于低频信号：

$$
\text{噪声平均增益}
>
\text{信号增益}
$$

最终得到的等效宽带 NF 可以超过单纯的：

$$
NF_{max}
$$

当前 0 Hz 附近大约为：

$$
NF_{eff}\approx22.6\text{ dB}
$$

于是：

$$
SNR_{out}
\approx66.5-22.6
\approx43.9\text{ dB}
$$

与 ADS 积分计算结果一致。

另一方面，当信号频率升高时，\(S_{21}(f_s)\) 增大，而总积分噪声 \(P_{n,out}\) 不变，因此：

$$
P_{sig,out}(f_s)\uparrow
$$

最终：

$$
SNR_{out}(f_s)\uparrow
$$

这正是仿真中右下角 `SNRout` 随频率上升的原因。

---

# 6. 结论

对于 **0–55 GHz 宽带噪声条件下的单音 SNR**：

### 简化计算

可以使用：

$$
\boxed{
SNR_{out}\approx SNR_{in}-NF_{max}
}
$$

适合：

* 快速估算；
* 电路频响比较平坦；
* 设计初期做保守检查。

当前得到约：

$$
44.5\text{ dB}
$$

---

### 推荐的严格计算

应计算整个带宽的输出积分噪声：

$$
\boxed{
P_{n,out}
=
\int_0^{55G}
kT_0
|S_{21}(f)|^2
10^{NF(f)/10}
df
}
$$

然后：

$$
\boxed{
SNR_{out}(f_s)
=
10\log_{10}
\frac{
P_{sig,in}|S_{21}(f_s)|^2
}{
P_{n,out}
}
}
$$

这种方法同时考虑：

$$
\boxed{
NF(f)+S_{21}(f)+Noise\ Bandwidth
}
$$

因此更适合当前这种**超宽带且增益随频率明显变化的 VGA**。

当前积分计算得到：

$$
\boxed{
P_{n,out}\approx-60.95\text{ dBm}
}
$$

以及低频：

$$
\boxed{
SNR_{out}(0)\approx43.9\text{ dB}
}
$$

这个结果是合理的。

> **最终建议：报告中将积分法作为正式的 0–55 GHz SNR 计算方法，将 \(SNR_{in}-NF_{max}\) 保留为快速估算/交叉检查指标。**

### 补充说明

以上推导中的

$$
P_{sig}=\frac{V_{pp}^2}{8R}
$$

成立的前提是 **0.89 Vpp 表示正弦单音信号**。

如果后续 0.89 Vpp 实际表示的是 **NRZ、PAM4 等宽带数据信号**，则信号本身也具有频谱，此时应进一步升级为：

$$
P_{sig,out}
=
\int S_{sig,in}(f)G(f)df
$$

即对**信号 PSD 和噪声 PSD 分别积分**后，再计算真正的数据链路 SNR。
