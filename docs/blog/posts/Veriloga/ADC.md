---
date:
  created: 2025-10-14
draft: 
categories:
  - Analog Circuit
tags:
  - Verilg-A
authors:
  - why

---


### 代码功能简介

这是一个 N 位理想模数转换器（ADC）的 Verilog-A 行为级模型。它的核心功能是在时钟信号 (`clk`) 的有效边沿（上升沿或下降沿）对模拟输入电压 (`in`) 进行采样，然后将其线性转换为一个 N 位的数字码，并以模拟电压的形式（`vdd` 或 `vss`）驱动 N 位并行输出总线 (`out`)。

该模型具有很强的可配置性，您可以通过参数调整其分辨率（位数）、输入电压范围、输出逻辑电平、时序特性（延时和翻转时间）以及时钟触发边沿。

---

### 带有详细注释的代码

```verilog
// N-bit Analog to Digital Converter
// LSB is <0>
// Change binary_bits variable for your needs!
// Author: A. Sidun
// Source: AnalogHub.ie
// -- 注释由AI添加 --

// 包含预定义的物理常量 (如 π) 和 discipline (如 electrical)
`include "constants.vams"
`include "disciplines.vams"

// 使用 `define 定义ADC的位数（分辨率）。修改此处的数值即可改变ADC的位数。
`define bits 12 

// 定义名为 ADC 的模块，包含输出(out)、输入(in)和时钟(clk)三个端口
module ADC (out, in, clk);
    // --- 参数定义 ---
    // 通过 parameter 可以在实例化模块时对这些值进行修改
    parameter real vmin = 0.0;                       // 模拟输入的最小电压 (V)
    parameter real vmax = 1.0 from (vmin:inf);       // 模拟输入的最大电压 (V)，必须大于vmin
    parameter real td = 0 from [0:inf);              // 从时钟有效边沿到输出开始变化的时间延迟 (s)
    parameter real tt = 0 from [0:inf);              // 输出信号的翻转时间 (上升/下降时间) (s)
    parameter real vdd = 5;                          // 代表逻辑 '1' 的电压电平 (V)
    parameter real vss = 0;                          // 代表逻辑 '0' 的电压电平 (V)
    parameter real thresh = (vdd+vss)/2;             // 时钟信号的逻辑阈值电平 (V)，用于判断边沿
    parameter integer dir = +1 from [-1:1] exclude 0; 
                                                     // 触发方向: +1 代表上升沿, -1 代表下降沿
    
    // --- 局部参数 ---
    // localparam 是在模块内部计算得出的常量，无法从外部修改
    // levels 计算了ADC的总量化级数，等于 2 的 `bits` 次方
    localparam integer levels = 1<<`bits;
    
    // --- 端口声明 ---
    input in, clk;               // 模拟输入和时钟输入
    output [`bits-1:0] out;      // N位并行数字输出总线
    
    // --- 端口/节点类型声明 ---
    voltage in, clk;             // 将 in 和 clk 节点定义为 voltage 类型
    voltage [`bits-1:0] out;     // 将 out 总线定义为 voltage 类型
    
    // --- 内部变量 ---
    integer result;              // 用于存储量化后的整数结果
    genvar i;                     // 循环变量，必须使用 genvar 类型，因为循环体内有模拟操作符 (transition)

    // --- 模拟行为块 ---
    analog begin
        // --- 采样与量化 ---
        // @(event) 结构用于在特定事件发生时执行代码块
        // cross(V(clk)-thresh, dir) 用于检测时钟信号 V(clk) 穿过 thresh 电平的事件
        // 'or initial_step' 确保在仿真开始时 (t=0) 也有一个初始的输出值
        @(cross(V(clk)-thresh, dir) or initial_step) begin
            // 核心量化公式：将输入电压线性映射到 [0, levels-1] 的整数范围
            result = levels*((V(in) - vmin))/(vmax - vmin);

            // --- 饱和处理 ---
            // 如果输入电压超出范围，将结果限制在有效范围内
            if (result > levels-1)
                result = levels-1; // 上限饱和
            else if (result < 0)
                result = 0;        // 下限饱和
        end

        // --- 输出驱动 ---
        // for 循环遍历每一位输出
        for (i=0; i<`bits; i=i+1)
            // V(out[i]) <+ ... 是一个贡献语句，用于驱动输出节点的电压
            // transition() 是一个平滑函数，用于模拟真实的信号翻转，避免理想阶跃带来的收敛问题
            // (result & (1<<i)) ? vdd : vss 是一个三元操作符
            // (1<<i) 生成一个掩码，用于检查 result 的第 i 位是否为1
            // 如果第 i 位是 1，输出 vdd；否则输出 vss
            V(out[i]) <+ transition(result & (1<<i) ? vdd : vss, td, tt);
    end
endmodule
```

---

### 代码工作原理解析

1.  **参数化与可配置性**
    *   通过 `define bits`，用户可以轻松地将这个模型配置为任意位数（例如8位、10位、12位）的ADC。
    *   `parameter` 声明使得ADC的各种关键特性，如输入电压范围 (`vmin`, `vmax`)、输出逻辑电平 (`vdd`, `vss`) 和时序 (`td`, `tt`)，都可以在电路仿真时从外部指定，极大地增强了模型的复用性。

2.  **采样触发机制**
    *   ADC的核心动作——采样和转换，是由 `@(event)` 语句块触发的。
    *   `cross(V(clk)-thresh, dir)` 是一个事件函数，它精确地检测时钟信号 `V(clk)` 穿过阈值 `thresh` 的时刻 [1, p. 120]。参数 `dir` 控制了检测的是上升沿（`+1`）还是下降沿（`-1`）。
    *   `or initial_step` 确保了在仿真开始的第一个时间点（t=0），即使没有时钟边沿，ADC也会进行一次转换并产生一个有效的初始输出 [1, p. 119]。

3.  **理想线性量化**
    *   `result = levels*((V(in) - vmin))/(vmax - vmin);` 这一行是量化的核心。它将输入电压 `V(in)` 从 `[vmin, vmax]` 的模拟范围，线性地映射到 `[0, levels]` 的整数范围。这是一个理想的线性转换，没有考虑非线性、噪声等实际效应。
    *   随后的 `if/else if` 语句对结果进行 **饱和处理**，确保即使输入电压超出了定义的范围，输出的数字码也不会超过 `0` 到 `levels-1` 的有效范围。

4.  **数字码输出**
    *   `for` 循环和 `genvar`：由于循环体内使用了 `transition` 这样的模拟操作符，循环变量 `i` 必须被声明为 `genvar` 类型 [1, p. 66]。
    *   **位检查**: `(result & (1<<i))` 是一个标准的位操作技巧。`1<<i` 会生成一个只有第 `i` 位是1的二进制数（例如，i=2时为 `...00100`）。通过与 `result` 进行按位与（`&`）运算，可以判断 `result` 的第 `i` 位是0还是1。
    *   **电压驱动**: 三元操作符 `? :` 根据位检查的结果选择输出 `vdd`（逻辑1）还是 `vss`（逻辑0）。
    *   **`transition()` 函数**: 这是行为级建模中非常重要的一个函数。它将一个理想的阶跃信号（从 `vss` 到 `vdd` 或反之）转换为一个具有指定延迟 (`td`) 和翻转时间 (`tt`) 的平滑斜坡信号 [1, p. 168]。这不仅使输出波形更接近实际器件，还能有效避免仿真中的收敛问题。