# 2ZZ Custom Harness — Connector Verification Register

**Vehicle basis:** 2000 US-spec Toyota Celica GT-S  
**Final controls direction:** ECUMaster EMU Black, purpose-built engine/control harness, DBW, flex fuel  
**Checkpoint:** 2026-09-16

## Purpose

This document owns **connector/terminal/seal verification and the production-harness BOM path**. Hardware selection belongs in the relevant subsystem document; a selected sensor is not automatically a production-approved connector.

Each production connector progresses through:

1. **FACTORY-DOC** — application/housing supported by the applicable EWD where OEM.
2. **SUPPLIER-CROSS-REFERENCE** — purchasable connector/terminal/seal set identified.
3. **PHYSICAL-FIT** — actual housing mates, keys, and latches to the selected device.
4. **TERMINAL-VERIFIED** — terminals, seals, locks/plugs, and wire range confirmed.
5. **PRODUCTION-APPROVED** — wire, pin map, tooling, branch ownership, and quantity frozen.

Do not promote a connector to `PRODUCTION-APPROVED` based on appearance or catalog cross-reference alone.

## Current physical status

- First-pass Ballenger OEM-style connector kits received **2026-09-01**.
- Kits are grouped in a large ziplock bag on top of the built engine.
- Physical-fit and terminal verification remain pending.
- Baseline Plus has since selected several non-OEM sensors/actuators; their connector families are recorded below but are not yet final-harness production-approved.
- **AEM 30-2130-50 MAP sensor, Ballenger CONN-75963 spare sensor connector kit, Ballenger CONN-86036 TPS extension, TXL wire, and open-barrel splice terminals were purchased 2026-09-16 for the PowerFC/CeliTune MAP development harness.**

## Factory references

- **EWD399U** — 2000 Celica EWD; authoritative chassis/year baseline.
- **EWD590U** — 2005 Celica EWD; used deliberately for selected late-2ZZ hardware such as DBW and later knock architecture.

## First-pass OEM connector verification

| Group | Intended device(s) | Toyota housing PN | EWD basis | Ballenger kit | Current status | Notes |
|---|---|---:|---|---|---|---|
| H01 | crank + cam position sensors | **90980-10947** | 2000/2005 C4 crank; C1 cam | **CONN-75800** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | verify both actual sensors |
| H02 | VVT + VVL oil-control valves | **90980-11162** | 2000/2005 C3 VVT; C2 VVL | **CONN-76007** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | shared housing; verify both devices |
| H03 | OEM oil-pressure / VVTL-pressure-switch family | **90980-11363** | 2000/2005 pressure-switch applications | **CONN-76068** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | final switch use depends on oil-system layout |
| H04 | engine coolant temperature | **90980-11062** | 2000/2005 E6 | **CONN-75820** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | retain OEM ECT architecture |
| H05 | late 2ZZ knock sensor | **90980-11875** | 2005 K1; not 2000 knock housing | **CONN-75757** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | final build intentionally targets later architecture |
| H06 | ignition coils | **90980-11885** | 2000/2005 I2–I5 | **CONN-75727** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | production qty 4 |
| H07 | 2003–2005 2ZZ DBW throttle | **90980-11858** | 2005 T1 | **CONN-75805** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | verify against purchased ETB |
| H08 | 2003–2005 accelerator pedal | **90980-11144** | 2005 A17 | **CONN-76021** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | verify against owned pedal |
| H09 | alternator/generator control | **90980-11349** | 2000/2005 G2 | **CONN-75736** | FACTORY-DOC + SUPPLIER-XREF; KIT RECEIVED; FIT PENDING | verify final alternator |
| H10 | 2000 cable-throttle TPS development pass-through | **90980-11261** | 2000 T1 | **CONN-86036 extension assembly** | FACTORY-DOC + SUPPLIER-XREF; PURCHASED; FIT PENDING | temporary removable PowerFC/CeliTune MAP T-harness; not final DBW harness hardware |

## Supplier-reported terminal/seal register

Supplier data is procurement evidence, not physical verification.

| Group | Ballenger housing SKU | Supplier attribution | Terminal | Seal | Reported wire range |
|---|---|---|---|---|---|
| H01 | CONN-100510 | Sumitomo TS | CONN-11856 | CONN-00145 | 20–16 AWG |
| H02 | CONN-100927 | Japanese VVT housing; OEM maker TBD | CONN-11856 | CONN-00145 | 20–16 AWG |
| H03 | CONN-100724 | Toyota 090/TS-type pressure-switch application | CONN-11856 | CONN-00145 | 20–16 AWG class |
| H04 | CONN-100317 | 1.8 mm SSC / Econoseal-type application | CONN-00127 | CONN-00119 | 20–16 AWG |
| H05 | CONN-11855 | Toyota-style 2.3 mm sealed application | CONN-11856 | CONN-00119 | 20–16 AWG |
| H06 | CONN-100307 | Toyota coil 2.3 mm sealed application | CONN-11856 | CONN-00119 | 20–16 AWG |
| H07 | CONN-100647 | Toyota/Mazda sealed TPS/DBW application | CONN-11856 | CONN-00145 | 20–16 AWG |
| H08 | CONN-100959 | older Toyota 090-I | CONN-11656 | CONN-00145 | 22–16 AWG class |
| H09 | CONN-100300 | Sumitomo TS; 6189-0443 / 6189-0442 | CONN-11856 | CONN-00145 | 20–16 AWG |
| H10 | CONN-86036 complete extension | Toyota 90980-11261 TPS extension / male-female pair | supplied assembled | supplied assembled | inspect actual received wire/termination before modification |

### Emerging commonality — do not bulk-buy yet

`CONN-11856` appears across H01/H02/H03/H05/H06/H07/H09, with seal families split mainly between `CONN-00145` and `CONN-00119`. That may simplify tooling/spares, but loose-terminal quantities wait for physical verification and final wire construction.

## Selected Baseline Plus / final non-OEM devices — connector status

These devices are selected in [`BASELINE_PLUS.md`](BASELINE_PLUS.md) and/or [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md). This table prevents older “deferred hardware” language from contradicting those selections while keeping connector approval properly separate.

| Device | Selected hardware | Known connector/interface | Connector status |
|---|---|---|---|
| **MAP sensor** | **AEM 30-2130-50, 50 PSIa / 3.5-bar absolute** | AEM specifies **Packard 3-pin**; mating connector/pin kit included with sensor; spare **Ballenger CONN-75963** purchased | **SELECTED DEVICE; PURCHASED 2026-09-16; physical fit + terminal/seal inspection pending** |
| Fuel-pressure sensor | **Link MIPS 101-0325** | supplied 3-way Metri-Pack 150 kit | SELECTED DEVICE; inspect supplied connector/terminals before production approval |
| Oil-pressure sensor | **Link MIPS 101-0325** | supplied 3-way Metri-Pack 150 kit | SELECTED DEVICE; same verification as fuel sensor |
| Flex-fuel sensor | **GM/Continental 13507129** | OEM-style 3-wire ethanol-sensor connector | SELECTED DEVICE; exact production housing/terminal PN still to document |
| Wideband lambda | **Bosch LSU 4.9 0 258 017 025 / 17025** | Bosch 6-way sensor interface | SELECTED DEVICE; mating connector/terminal kit still to verify/document |
| Boost-control solenoid | existing **Tru-Boost MAC valve** | existing 2-wire solenoid connection | SELECTED EXISTING DEVICE; characterize connector/coil before final harness decision |
| Oil-filter sandwich / switch | **MWR MWR-901465** + relocated warning switch | pressure-switch connector depends on switch/adaptor choice | INSTALLATION DETAIL OPEN |

### AEM 30-2130-50 electrical termination

Manufacturer-documented electrical roles:

- **Sensor Power** -> regulated +5 V sensor supply;
- **Signal Ground** -> ECU/DAQ sensor-signal ground, not chassis ground;
- **Signal** -> MAP / analog input.

Manufacturer specifications relevant to harness design:

- supply: 5 V ±0.5 V;
- supply current: less than 6 mA;
- output is analog and calibrated over the sensor's pressure range;
- electrical connection: Packard 3-pin;
- mechanical connection: 1/8 NPT male.

Do not freeze cavity numbering from memory or a generic Packard diagram. Confirm the actual AEM connector/pigtail documentation and received hardware before crimping the final harness.

## PowerFC / CeliTune MAP development T-harness

Temporary pre-EMU architecture:

```text
OEM 2000 TPS harness connector
        |
        +-> Ballenger CONN-86036 pass-through -> OEM TPS
        |
        +-> VC / 5 V tee -----------------------> AEM MAP power
        +-> E2 / sensor-ground tee -------------> AEM MAP ground
                                                   +-> Datalogit ground-reference sense

AEM MAP signal ----------------------------------> Datalogit auxiliary analog signal
```

Rules:

- modify only the removable extension/T-harness; do not cut the OEM engine harness;
- preserve all three TPS conductors straight-through;
- branch only VC and E2 to power/reference the MAP sensor;
- Datalogit signal/reference leads are development instrumentation, not part of the final EMU harness;
- verify the exact Datalogit auxiliary pinout and intended single-ended/differential setup before termination;
- bench continuity-check every pass-through and branch before connecting the PowerFC;
- KOEO verify TPS remains normal and VC-to-E2 remains in the expected approximately 5 V range after the MAP load is added;
- label the assembly as temporary development hardware so it is removed deliberately during ETB conversion.

## Final-build hardware still genuinely open

Connector selection should wait for actual hardware selection for:

- final injectors;
- dedicated IAT sensor;
- EMAP sensor;
- EGT thermocouple/CAN module;
- oil-temperature sensor;
- coolant-pressure sensor;
- E153 transmission switches/sensors;
- CAN expansion modules and temporary development instrumentation.

**External MAP is no longer open hardware.** The AEM 30-2130-50 is the selected sensor unless the verified operating-pressure envelope requires moving to a larger absolute-pressure range.

Do not inherit connector choices blindly from the factory harness for these functions.

## Arrival inspection procedure

For each kit/device pair:

- keep the supplier bag/label with the connector;
- photograph label + connector + target device;
- inspect keying/cavity count before mating;
- verify full insertion and positive latch without forcing;
- record molded manufacturer/family/cavity markings;
- compare supplied terminals/seals/secondary locks with supplier data;
- do not install loose terminals solely for fit testing unless correct extraction tooling is available;
- mark `PHYSICAL-FIT VERIFIED` only after testing the actual intended device;
- mark `TERMINAL-VERIFIED` only after received hardware and intended wire are reconciled.

Shared housings must be tested on every intended device before production approval.

## Production harness BOM target

For every production device preserve:

- device and quantity;
- year/application basis;
- housing/family;
- terminal/seal/cavity-plug/secondary-lock PN;
- pin and signal name;
- EMU/chassis destination;
- wire type, gauge, color, approximate length;
- twisted-pair/shielding requirements;
- sensor-ground/power-ground/splice ownership;
- branch protection and heat/abrasion/strain relief;
- crimp/extraction tooling;
- evidence and verification state.

Crimp tooling remains intentionally unselected until the terminal population is physically verified.
