# Pre-EMU Baseline Capture

Before removing the known-good Apexi PowerFC architecture, preserve the vehicle-side behavior and calibration evidence that may be difficult to reconstruct later.

This is an **evidence-capture gate**, not an attempt to fully decode Toyota body networking or directly translate the PowerFC calibration into EMU Black tables before the EMU installation.

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

## Capture set C — PowerFC calibration reference

Before removing or materially changing the current PowerFC setup, preserve as much of the running calibration as practical.

Preferred evidence, in descending order of value:

1. A complete FC-Edit / FC-Datalogit `Read All` followed by `Save As` to preserve a native calibration file if the required interface/software is available.
2. Screenshots/exports of the complete ignition map, VVT map, fuel/base-injection map, map axes, VTLI/VVL changeover settings, rev/idle/boost-related limits, injector settings and correction data.
3. Useful datalogs from known-good operating conditions, especially idle, cruise, transient response, VVT/VVL transition and representative boost/load regions.
4. Handheld Commander values/screens only if more complete electronic extraction is not available.

Treat the PowerFC calibration as **REFERENCE**, not as a table-for-table EMU Black import source. The highest-value information is the behavior and shape of a tune already proven on this exact engine/car: ignition strategy, VVT strategy, VVL changeover, limits, idle targets/behavior, injector assumptions and useful logs. Fuel-table numbers are lower-confidence transfer material because the interim EMU stage also changes fuel-system hardware and may change injector characterization/fuel pressure; EMU Black also uses a different fueling model and correction structure.

Where values are preserved, record the original PowerFC table name/axis units and the car state/hardware they applied to. Do not silently reinterpret PowerFC units as EMU Black units.

## Scope boundary

Do not delay the interim EMU conversion for open-ended BEAN/MPX reverse engineering or an attempted perfect PowerFC-to-EMU table translation. Capture enough baseline evidence to preserve the known-good reference state, then use the MWR Celica base map, the Lotus 2ZZ DBW reference map, actual hardware characterization and tuner validation to commission the EMU.

Cross-project research conclusions may also be summarized in `Celica-engineering-knowledge`, but executable capture work is owned by this Street Build repository.
