# Analog IC technical review checklist

Apply only the relevant checks. The purpose is to prevent a polished note from preserving a wrong metric or hidden assumption.

## Definitions and domains

- Is the analysis DC, small-signal AC/SP, transient, periodic steady state, noise, distortion, stability, mismatch, or Monte Carlo?
- Are voltage and power conventions explicit: peak, RMS, Vpp, dBV, dBm, single-ended, or differential?
- Are reference impedances and terminations consistent with the signal-power and S-parameter definitions?
- Are linear quantities converted before integration or averaging? Do not average dB when physical powers must be summed.
- For noise, distinguish PSD from integrated noise, input-referred from output-referred noise, one-sided from two-sided PSD, and physical bandwidth from equivalent noise bandwidth.
- For ratios, verify whether the plotted quantity is voltage gain, transducer/available/operating power gain, or \(|S_{21}|^2\).

## Equations and approximations

- Check dimensional consistency of every final equation.
- Confirm sign convention for \(\Delta x\), mismatch, current direction, and feedback polarity.
- Name the approximation and its validity condition, such as \(|\Delta I|\ll I\), \(g_mr_o\gg1\), \(\beta\gg1\), or a dominant-pole assumption.
- Test at least one limiting case: zero degeneration, infinite loop gain, zero mismatch, flat gain, or zero bandwidth as appropriate.
- Separate exact nonlinear relations from linearization and from design heuristics.

## Noise and SNR

- Define the signal waveform. \(V_{pp}^2/(8R)\) is a sinusoidal-power relation, not a generic NRZ/PAM power formula.
- Integrate noise in the linear power domain and weight it by the relevant transfer function.
- State \(T_0\), bandwidth limits, source temperature/impedance, and whether the simulator's NF definition matches the calculation.
- A single-tone signal uses gain at its signal frequency; a broadband data signal generally requires integrating its signal PSD.

## Mismatch and variation

- Separate deterministic/systematic error, local random mismatch, global process variation, and temperature/bias variation.
- State whether quoted values are nominal, \(1\sigma\), \(3\sigma\), worst case, or a fitted/typical range.
- Combine independent random contributions by variance/RSS. State correlations when devices or resistors share geometry or gradients.
- Connect schematic mismatch assumptions to layout choices, device area, orientation, spacing, common centroid/interdigitation, and parasitics when relevant.

## Simulation reproducibility

- Record simulator and analysis type, model/PDK version when known, testbench hierarchy, ports, terminations, bias point, temperature, corner, sweep range, and integration range.
- Keep simulator equations aligned with dataset names and units. Label unverified syntax as pseudocode.
- Cross-check the result with a hand estimate, conservation law, limiting case, alternate measurement, or a second simulation setup.
- Treat interpolation, insufficient frequency resolution, convergence, hidden default bandwidth, and reference-plane choices as possible numerical artifacts.

## Engineering conclusion

- State what was demonstrated, not merely what was calculated.
- Separate the general physical principle from the result at the present operating point.
- Capture the main tradeoff: headroom, power, gain, bandwidth, noise, linearity, stability, area, or matching.
- Give a concrete next check when the conclusion still depends on PVT, Monte Carlo, large-signal behavior, or layout extraction.
