---
date:
  created: 2025-02-28
draft: true
categories:
  - Analog Circuit
tags:
  - Current Mirror
  - BJT
  - mismatch
authors:
  - why

---

# 退化电阻对 BJT 电流镜拷贝精度的影响

> **结论依据：** 下文基于匹配 BJT、小信号线性化与忽略 Early effect 的近似推导；实际设计还需结合器件模型和 Monte Carlo 仿真验证。

## 1. 电路与分析假设

考虑两个匹配的 BJT 电流镜，Q1 为参考支路，Q2 为输出支路，两管基极相连，发射极分别串联相同退化电阻 $R_E$。

假设：

* 忽略 Early effect；
* $\beta$ 足够大，因此 $I_E \approx I_C$；
* 两支路退化电阻相等；
* 只分析 BJT 的 $V_{BE}$ 失配对电流拷贝精度的影响。

定义：

* 参考支路电流为 $I$；
* 输出支路电流为 $I+\Delta I$；
* $\Delta V_{BE,\mathrm{mis}}$ 为两个 BJT 在**相同集电极电流下**的固有 $V_{BE}$ 失配。

---

## 2. 两个支路的直流关系

由于两个 BJT 基极相连，且退化电阻下端连接同一个电位，因此有：

$$
V_{BE1}+I_1R_E = V_{BE2}+I_2R_E
$$

令：

$$
I_1=I
$$

$$
I_2=I+\Delta I
$$

则：

$$
V_{BE1}+IR_E = V_{BE2}+(I+\Delta I)R_E
$$

---

## 3. 引入 BJT 的 $V_{BE}$ 失配

理想 BJT 的关系为：

$$
I_C=I_S e^{V_{BE}/V_T}
$$

因此：

$$
V_{BE}=V_T\ln\left(\frac{I_C}{I_S}\right)
$$

对于 Q1：

$$
V_{BE1} = V_T\ln\left(\frac{I}{I_S}\right)
$$

假设 Q2 在相同电流下存在额外的 $V_{BE}$ 失配 $\Delta V_{BE,\mathrm{mis}}$，则：

$$
V_{BE2} = V_T\ln\left(\frac{I+\Delta I}{I_S}\right) + \Delta V_{BE,\mathrm{mis}}
$$

代入两支路电压关系：

$$
V_T\ln\left(\frac{I}{I_S}\right)+IR_E = V_T\ln\left(\frac{I+\Delta I}{I_S}\right) + \Delta V_{BE,\mathrm{mis}} + (I+\Delta I)R_E
$$

消去公共项后：

$$
V_T\ln\left(\frac{I}{I+\Delta I}\right) = \Delta V_{BE,\mathrm{mis}} + \Delta I R_E
$$

整理为：

$$
V_T\ln\left(1+\frac{\Delta I}{I}\right) + \Delta I R_E + \Delta V_{BE,\mathrm{mis}} = 0
$$

---

## 4. 小失配条件下的一阶近似

当：

$$
|\Delta I| \ll I
$$

有：

$$
\ln(1+x)\approx x
$$

因此：

$$
\ln\left(1+\frac{\Delta I}{I}\right) \approx \frac{\Delta I}{I}
$$

代入可得：

$$
V_T\frac{\Delta I}{I} + R_E\Delta I + \Delta V_{BE,\mathrm{mis}} = 0
$$

提取 $\Delta I$：

$$
\Delta I \left( \frac{V_T}{I}+R_E \right) = -\Delta V_{BE,\mathrm{mis}}
$$

因此：

$$
\boxed{
\Delta I = -\frac{\Delta V_{BE,\mathrm{mis}}}{R_E+\frac{V_T}{I}}
}
$$

两边除以 $I$：

$$
\boxed{
\frac{\Delta I}{I} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T+IR_E}
}
$$

这就是退化电阻存在时，$V_{BE}$ 失配引起的电流拷贝误差。

---

## 5. 用 $g_m$ 表示

BJT 的跨导为：

$$
g_m=\frac{I}{V_T}
$$

因此：

$$
\frac{V_T}{I} = \frac{1}{g_m}
$$

上式可写为：

$$
\boxed{
\Delta I = -\frac{\Delta V_{BE,\mathrm{mis}}}{\frac{1}{g_m}+R_E}
}
$$

相对误差为：

$$
\boxed{
\frac{\Delta I}{I} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T} \cdot \frac{1}{1+g_mR_E}
}
$$

---

## 6. 退化电阻对拷贝精度的改善

当没有退化电阻时：

$$
R_E=0
$$

因此：

$$
\left. \frac{\Delta I}{I} \right|_{R_E=0} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T}
$$

加入退化电阻后：

$$
\left. \frac{\Delta I}{I} \right|_{R_E} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T} \cdot \frac{1}{1+g_mR_E}
$$

所以误差改善比例为：

$$
\boxed{
\frac{(\Delta I/I)_{R_E}}{(\Delta I/I)_{R_E=0}} = \frac{1}{1+g_mR_E}
}
$$

也就是说：

$$
\boxed{
\text{电流失配被抑制约 } 1+g_mR_E \text{ 倍}
}
$$

$R_E$ 越大，BJT 的 $V_{BE}$ mismatch 对电流拷贝精度的影响越小。

---

## 7. 强退化情况下

如果：

$$
g_mR_E \gg 1
$$

等价于：

$$
IR_E \gg V_T
$$

则：

$$
V_T+IR_E\approx IR_E
$$

于是：

$$
\frac{\Delta I}{I} \approx -\frac{\Delta V_{BE,\mathrm{mis}}}{IR_E}
$$

即：

$$
\boxed{
\Delta I \approx -\frac{\Delta V_{BE,\mathrm{mis}}}{R_E}
}
$$

此时电流误差主要由退化电阻将 $V_{BE}$ mismatch 转换成电流误差。

---

## 8. 数值示例

假设：

$$
I=5.9\,\mathrm{mA}
$$

$$
R_E=100\,\Omega
$$

$$
V_T\approx 25.9\,\mathrm{mV}
$$

则：

$$
g_m = \frac{5.9\,\mathrm{mA}}{25.9\,\mathrm{mV}} \approx 0.228\,\mathrm{S}
$$

因此：

$$
g_mR_E\approx 22.8
$$

误差抑制因子为：

$$
1+g_mR_E\approx 23.8
$$

若：

$$
\Delta V_{BE,\mathrm{mis}}=1\,\mathrm{mV}
$$

没有退化电阻时：

$$
\frac{\Delta I}{I} \approx -\frac{1}{25.9} \approx -3.86\%
$$

加入 $100\,\Omega$ 退化电阻后：

$$
\frac{\Delta I}{I} = -\frac{1\,\mathrm{mV}}{25.9\,\mathrm{mV} + 5.9\,\mathrm{mA}\times 100\,\Omega}
$$

因此：

$$
\boxed{
\frac{\Delta I}{I} \approx -0.162\%
}
$$

对应：

$$
\Delta I\approx -9.6\,\mu\mathrm{A}
$$

可见 $100\,\Omega$ 退化电阻将该类电流失配降低了约 24 倍。

---

## 9. 物理意义

从小信号角度看，BJT 的增量发射结电阻为：

$$
r_e=\frac{1}{g_m}
$$

因此 $V_{BE}$ mismatch 要引起电流变化，需要作用在总增量阻抗：

$$
\frac{1}{g_m}+R_E
$$

上。

所以：

$$
\Delta I = -\frac{\Delta V_{BE,\mathrm{mis}}}{\frac{1}{g_m}+R_E}
$$

加入 $R_E$ 后，电流发生变化会产生额外的：

$$
\Delta V_E=\Delta I R_E
$$

这个电压会改变 $V_{BE}$，从而抑制原来的电流变化，形成局部负反馈。

因此，退化电阻改善电流镜拷贝精度的本质是：

> **利用发射极局部负反馈降低电流对 BJT $V_{BE}$ 失配的敏感度。**

最终最重要的结论为：

$$
\boxed{
\frac{\Delta I}{I} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T+IR_E} = -\frac{\Delta V_{BE,\mathrm{mis}}}{V_T} \cdot \frac{1}{1+g_mR_E}
}
$$

一般建议不要直接按“多少欧姆”选，而是看退化强度：

$$
\boxed{g_mR_E \approx 5\sim 20}
$$

等价地：

$$
\boxed{I_CR_E \approx 0.1\sim 0.5\,\mathrm{V}}
$$

对约 $5.9\,\mathrm{mA}$ 的电流镜，对应大致：

$$
\boxed{R_E\approx 20\sim 100\,\Omega}
$$

其中 $30\sim 50\,\Omega$ 比较均衡，$75\sim 100\,\Omega$ 属于较强退化，$V_{BE}$ mismatch 抑制更好，但会占用更多 headroom。

同时要注意，$R_E$ 越大，电流误差会越来越受电阻 mismatch 限制。两类主要误差近似为：

$$
\left.\frac{\Delta I}{I}\right|_{V_{BE}} \approx \frac{\Delta V_{BE}}{V_T+IR_E}
$$

$$
\left.\frac{\Delta I}{I}\right|_{R} \approx \frac{IR_E}{V_T+IR_E}\frac{\Delta R}{R}
$$

可以用“两项误差相等”作为第一版设计点：

$$
\boxed{
R_E \approx \frac{\sigma_{V_{BE}}}{I\left(\sigma_R/R\right)}
}
$$

也就是说，**把 $R_E$ 增大到 BJT mismatch 和电阻 mismatch 贡献大致相当，通常是比较合理的起点**。你现在的 $100\,\Omega$ 已经属于较强退化。


