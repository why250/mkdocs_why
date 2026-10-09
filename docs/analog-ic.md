# 模拟 IC

用于回顾模拟电路分析、误差来源与设计权衡。这里聚合了电流镜、ADC、噪声及其他模拟电路笔记。

## 增益、噪声与偏置

- [Gilbert 单元：发射极退化电阻对增益与噪声的影响](blog/posts/analog/01_Gilbert_Emitter_Degeneration_Gain_Noise.md)
- [主通路与 CTLE 辅助通路的增益、噪声分析](blog/posts/analog/02_MainPath_CTLE_RE_System_Analysis.md)
- [伪差分放大器：共享 cascode emitter 与输出噪声](blog/posts/analog/pseudo-differential-tail-cascode-noise/index.md)

## 数据转换器

- [Verilog-A ADC 行为模型](blog/posts/analog/ADC.md)
- [DAC、零阶保持与奈奎斯特频带](blog/posts/analog/Nyquist_Rate_DAC_ZOH_Operation.md)

[查看全部文章](blog/index.md) · [按标签查找](tags.md)

## 专题资料

- [Data Converters 参考目录](<book/eetop.cn_Analysis and Design of Data Converters - Razavi - 目录.pdf>)
- [Sigma-Delta Converter 基础与架构](<book/ADC/CMOS Sigma-Delta Converters-- Basic Concepts and Architectures(ppt).pdf>)

相关问题如涉及 RF 前端、链路增益或 S 参数，可继续查看 [RF](rf.md)。
