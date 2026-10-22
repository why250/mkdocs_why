---
title: Kelvin 四线法的本质：低电阻测量、芯片端电压检测与 Remote Sense 的区别
date:
  created: 2026-10-22
draft: false
categories:
  - Analog Circuit
tags:
  - kelvin-connection
  - four-wire-measurement
  - resistance-measurement
  - voltage-sensing
  - dm3068
---

# Kelvin 四线法的本质：低电阻测量、芯片端电压检测与 Remote Sense 的区别

Kelvin（四线）连接并非一种独立的“电压档”，而是一种**将通电路径（Force）与检测路径（Sense）分开**的接线原则。对于低电阻测量，它能显著减小引线及接触电阻的影响；对于芯片供电，则可直接检测负载端电压，必要时配合电源的 Remote Sense 实现压降补偿。**RIGOL DM3068 有 4WR（四线电阻）功能，但没有单独的四线 DCV 档。**

<!-- more -->

## 1. 基本原理：为什么要分离 Force 和 Sense

### 两线电阻测量

万用表用同一对导线注入测试电流 $I$ 并读取端电压。若被测电阻为 $R_x$，两根导线及相关接触电阻的等效串联值为 $R_{L+}$、$R_{L-}$，则在直流、无额外并联负载的条件下：

$$
R_{\mathrm{2WR}}=\frac{V_{\mathrm{meter}}}{I}
=R_x+R_{L+}+R_{L-}. \tag{1}
$$

因此，$R_x$ 越小，串联寄生电阻造成的相对误差越大。

### 四线电阻测量

Force+、Force− 负责让测试电流 $I$ 流过被测物；Sense+、Sense− 分别接在希望定义的电阻测量边界上。电压测量输入阻抗远高于 Sense 导线电阻，使 Sense 电流 $I_{\mathrm{sense}}$ 近似为零：

$$
\Delta V_{\mathrm{sense\ lead}}
=I_{\mathrm{sense}}R_{\mathrm{sense\ lead}}\approx0. \tag{2}
$$

于是，在 Sense 接点覆盖被测电阻本体、且忽略测量偏置与热电势时：

$$
\boxed{R_{\mathrm{4WR}}\approx\frac{V_{\mathrm{Sense+}}-V_{\mathrm{Sense-}}}{I}=R_x.} \tag{3}
$$

**物理意义**：Force 导线上的压降仍然存在，只是没有被计入 Sense 两端所定义的电阻。四线法也不能消除 **Sense 接点之间** 的焊点、金属走线和器件本体电阻；测量边界由 Sense 探针的位置决定。

举例（理想化，手算）：$R_x=0.10\,\Omega$，两侧引线及接触串联电阻各 $0.05\,\Omega$。则

$$
R_{\mathrm{2WR}}=0.20\,\Omega,\qquad
R_{\mathrm{4WR}}\approx0.10\,\Omega. \tag{4}
$$

两线法在这个例子中产生 $+100\%$ 误差，四线法则主要受仪器精度、热电势、接点布置与电阻自热等非理想因素限制。**分辨率不等于准确度**。

## 2. RIGOL DM3068：一台仪器即可进行 4WR

DM3068 内部有电阻测量所需的测试电流源以及电压检测电路，因此四线测电阻 **不需要两台万用表**。

| DM3068 前面板端子 | 四线电阻测量时的作用 | DUT 连接位置 |
|---|---|---|
| `INPUT HI` | Force+，测试电流路径 | 电阻一端 |
| `INPUT LO` | Force−，测试电流返回 | 电阻另一端 |
| `HI SENSE` | Sense+，电压采样 | 紧靠电阻正侧的测量边界 |
| `LO SENSE / 200 mA` | Sense−，电压采样 | 紧靠电阻负侧的测量边界 |
| `10 A` | 不使用 | 不接线 |

**操作步骤**：先确保 DUT 断电、储能元件已安全放电；将四根线接到相应端子，按面板 `Ω` 键进入/切换电阻测量功能，确认屏幕显示 `4WR`，再选择合适量程和分辨率。4WR 要使用两对独立触点；仅把四根线在仪器端并在一起，不会得到 Kelvin 测量效果。

**仪器限制**：DM3068 官方手册限定 `HI SENSE` 与 `LO SENSE / 200 mA` 之间不超过 200 Vpk，且 `LO SENSE / 200 mA` 对 `INPUT LO` 不超过 **0.5 Vpk**。这些端子不是可任意使用的第二组电压输入。务必遵守手册的其他端子保护和连接限制。

## 3. 测量电压时，“四线”究竟意味着什么？

需要区分三种情况：

| 场景 | 实际接法 | DM3068 模式 | 是否自动补偿供电线压降 |
|---|---|---|---|
| 测偏置点、基准源、芯片节点 DC 电压 | DM3068 两根表笔直接接目标节点和参考节点 | `DCV`（`INPUT HI/LO`） | 否 |
| 电源给芯片供电，同时测芯片端真实电压 | 电源两根供电线（Force）+ DM3068 两根芯片端检测线（Sense） | `DCV`（`INPUT HI/LO`） | 否，只负责测量 |
| 使芯片端电压在供电线压降下仍维持设定值 | 带 Remote Sense 的电源：两根供电线 + 电源自己的 Sense± 线 | 电源的远端反馈功能；可再用 DMM 验证 | 是，在电源的补偿范围内 |

**关键区别**：在第二种接法里，整个测试系统使用四根线，但 **DM3068 本身仍只使用 `INPUT HI/LO` 两个电压端子**；它的 `HI SENSE/LO SENSE` 插孔应保留给 4WR。不能将 `4WR` 档用于有源芯片节点的正常电压检测。

### 供电压降示例

假设稳压电源端电压为 $5.00\,\mathrm V$，芯片消耗 $I_L=100\,\mathrm{mA}$，两根供电线的合计电阻为 $R_L=0.5\,\Omega$。不考虑动态负载和其他路径时：

$$
\boxed{V_{\mathrm{chip}}=V_{\mathrm{supply}}-I_LR_L
=5.00-0.10\times0.50=4.95\,\mathrm V.} \tag{5}
$$

- 将 DM3068 的 `DCV` 表笔接在电源输出端：读到约 $5.00\,\mathrm V$。
- 将表笔接在 PCB 上的**芯片供电焊盘附近**：读到约 $4.95\,\mathrm V$。
- 如果电源支持 Remote Sense，并把它的 Sense± 引到相同位置，电源可提高其输出端电压以补偿 $I_LR_L$，将被检测的负载端稳在约 $5.00\,\mathrm V$（前提是电源的输出和补偿能力足够）。

Remote Sense 只保证**采样点**附近的电压，不会消除采样点到芯片内部晶体管之间的片上 IR drop。

## 4. DC 电压精度还取决于输入阻抗

对于高输出阻抗的模拟 IC 偏置节点，值得优先检查的误差往往不是表笔线阻，而是**万用表输入阻抗造成的负载效应**。将 DUT 简化为开路电压 $V_{\mathrm{th}}$、输出电阻 $R_s$ 的戴维南源，DMM 输入电阻为 $R_{\mathrm{in}}$，则有：

$$
\boxed{V_{\mathrm{meas}}=V_{\mathrm{th}}\frac{R_{\mathrm{in}}}{R_s+R_{\mathrm{in}}}.} \tag{6}
$$

这在纯电阻、直流近似下成立。若 $R_s=1\,\mathrm{M}\Omega$：

- $R_{\mathrm{in}}=10\,\mathrm{M}\Omega$ 时，$V_{\mathrm{meas}}/V_{\mathrm{th}}\approx0.909$，低读约 $9.09\%$；
- $R_{\mathrm{in}}=10\,\mathrm{G}\Omega$ 时，该比值约 $0.99990$，低读约 $0.010\%$。

DM3068 官方规格说明，`DCV` 的 **200 mV、2 V、20 V** 量程可选 `10 MΩ` 或 `>10 GΩ` 输入阻抗；**200 V、1000 V** 量程的输入阻抗为 `10 MΩ`。出厂默认 DC 输入阻抗为 `10 MΩ`；在低电压量程、高阻节点测量时可通过 DCV 测量参数中的 `Res` 菜单选择 `>10 GΩ`。这里的输入电阻规格不应直接套用到 ACV 或 RF 节点测量。

## 5. 适用边界与实验建议

- **低阻值无源器件、焊接连接、PCB 走线**：优先考虑 4WR；较低阻值时还需注意探针接触稳定性、热电势、测试电流导致的自热及可用量程。
- **有源偏置节点电压**：用 `DCV`，尽量就地测量；高阻节点优先评估 `>10 GΩ` 模式下的负载误差。
- **芯片供电准确性**：把采样点放在 PCB DUT 端；若需要稳定 DUT 端电压，使用具备 Remote Sense 的电源，而非依赖 DMM 自动补偿。
- **上电晶体管输出阻抗**：普通 4WR 不能直接给出有源电路在某个偏置点的小信号输出阻抗。应维持正常偏置，注入受控小信号扰动并计算 $r_{\mathrm{out}}=\partial V/\partial I$，必要时使用交流测量方法。

## 结论

四线法的可复用原则是：**让大电流走 Force，让电压信息走 Sense；被测量由 Sense 的实际采样边界定义。** 对 DM3068 而言，`4WR` 是低电阻测量功能，`DCV` 是两端电压测量功能；一个“电源供电 + DMM 在负载端采样”的四线系统不等于 DM3068 有独立的四线电压档。对高阻模拟偏置节点，DMM 输入阻抗通常比表笔线阻更值得优先分析。

## 参考资料

- RIGOL, *DM3068 Digital Multimeter User Guide*, Jan. 2024, [官方 PDF](https://download.rigol.com/en/Manual/Digital%20Multimeters/DM3068/DM3068_UserGuide_EN.pdf)。重点：第 1-16 页（接线图）、第 2-14 页（2WR/4WR 操作）、第 2-42～2-43 页（DC 输入阻抗设置）、第 5-2 页（DCV 负载误差）、第 6-4 页（规格）；前部 *Input Terminal Protection Limit* 一节有 SENSE 端子额定限制。
