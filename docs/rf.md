# RF

用于回顾 PA、噪声、传输线、匹配、滤波器及 ADS 仿真相关的设计记录。

## 传输线与匹配

- [特征阻抗的推导与参考文献](blog/posts/rf/TL_Z0.md)
- [Smith Chart：从 S22 判断特征阻抗](blog/posts/rf/TL_SmithChart.md)
- [传输线谐振器](blog/posts/rf/TL_resonators.md)

## 滤波器与功率分配

- [Richards 变换：用传输线实现电感与电容](blog/posts/rf/micro_filter.md)
- [低阻抗开路线的并联电容等效](blog/posts/rf/TL_filter.md)
- [J 变换器与滤波器耦合](blog/posts/rf/J_inverter.md)
- [Wilkinson 功分器](blog/posts/rf/wilkinson.md)
- [AWG 输出功率合成器选择](blog/posts/rf/Combiner.md)

## 放大器与系统指标

- [功率放大器稳定性：栅极电阻](blog/posts/rf/PA_notes_1.md)
- [级联系统噪声系数：Friis 公式](blog/posts/rf/NF.md)
- [IM3、IIP3 与 P1dB](blog/posts/rf/OIP3.md)

[查看全部文章](blog/index.md) · [按标签查找](tags.md)

## 专题资料

- [RF Microelectronics（Razavi）](<book/Razavi - 2012 - RF microelectronics.PDF>)
- [ADS/TRX 模型资料](<ADS_TRX_Model/Razavi - 2012 - RF microelectronics.PDF>)

若问题的重点转为电路级误差、ADC 或偏置设计，可继续查看 [模拟 IC](analog-ic.md)。
