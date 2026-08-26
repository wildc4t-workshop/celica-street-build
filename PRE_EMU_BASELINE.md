# Pre-EMU Baseline Capture

Before removing the known-good Apexi PowerFC architecture, preserve the vehicle-side behavior that may be difficult to reconstruct later.

This is an **evidence-capture gate**, not an attempt to fully decode Toyota body networking before the EMU installation.

## Capture set A — ECU / MPX / cluster electrical baseline

Record on the actual 2000 US-spec GT-S:

- PowerFC MPX1 electrical behavior;
- PowerFC MPX2 electrical behavior;
- KOEO comparison with the PowerFC connected versus disconnected where safe/practical;
- tach waveform at ECM E3-27 and at the combination meter as applicable;
- MIL (`W`) electrical behavior.

Preserve test point, vehicle/ignition state, scope settings, waveform/log filename, and the useful conclusion.

## Capture set B — retained vehicle functions

Record:

- cooling-fan relay truth table;
- A/C request/output behavior with the PowerFC;
- cluster oil-warning behavior and source;
- coolant-gauge sender topology on the actual car.

The purpose is to know what must be reproduced, retained, or deliberately changed when EMU Black replaces the current ECU architecture.

## Scope boundary

Do not delay the interim EMU conversion for open-ended BEAN/MPX reverse engineering. Capture enough baseline evidence to prevent destroying a known-good reference state. Deeper decoding can proceed only where a real integration requirement demands it.

Cross-project research conclusions may also be summarized in `Celica-engineering-knowledge`, but executable capture work is owned by this Street Build repository.
