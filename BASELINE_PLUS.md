# Baseline Plus — Controls, Fuel, DBW, and Protection Package

**Vehicle:** 2000 US-spec Toyota Celica GT-S / current 2ZZ-GE  
**ECU:** legacy ECUMaster EMU Black, V3.061  
**Vehicle interface:** MWR EMU Black adapter + short black/gray jumper harness  
**Status:** SELECTED architecture; explicit physical-verification gates remain  
**Checkpoint:** 2026-09-07

## 1. Purpose

Baseline Plus is the controlled current-engine commissioning stage before the built-2ZZ/E153/final-custom-harness swap.

It exists to prove the systems worth carrying into the final build while the current engine still runs:

- MWR return fuel system;
- EMU Black through the MWR adapter;
- late-2ZZ OEM-style DBW;
- flex fuel;
- native Bosch LSU 4.9 lambda;
- fuel- and oil-pressure protection;
- EMU-native boost control using the existing Tru-Boost MAC valve;
- retained Celica body/cluster integration through the MWR PCB.

The MWR PCB remains an interim integration bridge. The PCB itself is not modified; selected functions are bypassed or reassigned in the removable EMU-to-MWR jumper/sub-harness.

Before PowerFC removal, complete the evidence capture in [`PRE_EMU_BASELINE.md`](PRE_EMU_BASELINE.md).

---

## 2. Locked decisions

| ID | Decision | Selected implementation | Remaining verification |
|---|---|---|---|
| **DEC-STREET-001** | Keep MWR as Baseline Plus integration bridge | MWR PCB + OEM 2000 harness retained; removable jumper/sub-harness carries modifications | MWR continuity and vehicle-side routing |
| **DEC-STREET-002** | Pull DBW forward | 2003–2005 Celica GT-S ETB + owned late-Celica pedal; H-Bridge 1 controls ETB | ETB manifold fit, connector fit, DBW wizard/failsafe |
| **DEC-STREET-003** | Standardize pressure sensors | **2 × Link ECU/Honeywell MIPS 101-0325, 0–150 PSI** | Physical installation and calibration sanity check |
| **DEC-STREET-004** | Add flex fuel | **Radium 20-0589** split-flow housing + **GM/Continental 13507129** ethanol sensor | Actual feed-line size/end adapters |
| **DEC-STREET-005** | Use native wideband | **Bosch LSU 4.9 0 258 017 025 / 17025** directly to EMU Black | Harness termination and sensor commissioning |
| **DEC-STREET-006** | Separate oil measurement from turbo feed | Factory 2ZZ pressure port -> remote Link MIPS; MWR `MWR-901465` sandwich plate -> turbo feed + warning switch | Hose/bracket geometry and switch adapter |
| **DEC-STREET-007** | Move boost control into EMU | Existing Tru-Boost MAC valve; **G22 / Injector 6** low-side PWM | Coil/suppression check and de-energized base-boost failsafe |

### Pressure-sensor calibration

Link MIPS `101-0325` characteristics used for both fuel and oil:

- 5 V supply;
- 0.5–4.5 V output;
- 0–150 PSI / 10 bar range;
- 1/8 NPT male;
- media-isolated stainless construction;
- -40 to 125 °C rating;
- Metri-Pack 150 connector kit.

Linear transfer function:

`Pressure [psi] = 37.5 × Voltage [V] - 18.75`

### Boost-control electrical architecture

The existing Tru-Boost MAC valve is retained as the pneumatic actuator only. The Tru-Boost controller must not remain connected as a second solenoid driver.

```text
fused EFI-switched +12 V
          |
          +---- MAC valve ---- G22 / Injector 6
          |                    low-side PWM
          +----|<--------------+
             flyback diode

cathode/stripe -> +12 V side
anode          -> G22 side
```

Injector 6 is unused in the recovered MWR map. Before EMU connection, verify coil continuity/resistance and that the valve does not contain an unexpected suppression diode or case connection. With the valve de-energized, verify the pneumatic system produces mechanical wastegate/base boost.

---

## 3. Baseline Plus buy list

| System | Item | Selected part | Qty | State |
|---|---|---|---:|---|
| Flex fuel | Split-flow housing | **Radium 20-0589** | 1 | BUY |
| Flex fuel | Ethanol-content sensor | **GM/Continental 13507129** | 1 | BUY |
| Flex fuel | Housing end adapters | 10AN ORB -> actual feed-line size | 2 | **HOLD until feed size measured** |
| Fuel pressure | Pressure sensor | **Link MIPS 101-0325, 150 PSI** | 1 | BUY |
| Lambda | Wideband sensor | **Bosch LSU 4.9 0 258 017 025 / 17025** | 1 | BUY |
| Lambda | Mating connector/terminals | LSU 4.9 connector kit | 1 | BUY unless already supplied |
| Oil | Filter sandwich adapter | **MWR MWR-901465** | 1 | BUY |
| Oil pressure | Pressure sensor | **Link MIPS 101-0325, 150 PSI** | 1 | BUY |
| Oil pressure | Block adapter | true Toyota 1/8 BSPT -> -3AN male | 1 | BUY / verify fit |
| Oil pressure | Remote hose | short -3AN PTFE/braided line | 1 | **HOLD length until bracket location** |
| Oil pressure | Sensor-end adapter | -3AN male -> 1/8 NPT female | 1 | BUY |
| Oil pressure | Sensor mount | rubber-lined clamp + bracket | 1 | FAB/BUY after mockup |
| Oil system | Warning-switch relocation | NPT-compatible switch or correct BSP/NPT adapter | 1 | VERIFY before buy |
| Boost control | MAC 3-port valve from Tru-Boost | existing hardware | 1 | **OWNED** |
| Boost control | Flyback diode | automotive diode suitable for MAC coil | 1 | BUY during harness build |
| DBW | 2003–2005 Celica GT-S ETB | OEM late-Celica ETB | 1 | **PURCHASED 2026-09-07** |
| DBW | 2003–2005 Celica pedal | OEM late-Celica pedal | 1 | **OWNED** |
| DBW | ETB/pedal connector samples | Ballenger kits | 2 | **OWNED / fit verification pending** |
| Harness | EMU terminals/wire | existing printed flying-lead harness | as needed | **OWNED** |

---

## 4. Selected interim EMU I/O allocation

This allocation is authoritative for **Baseline Plus only**. The final custom harness gets its own I/O freeze later.

| Function | EMU channel | Baseline Plus use |
|---|---|---|
| ETB motor A | **G2 / H-Bridge 1A** | direct/bypass to ETB motor |
| ETB motor B | **G10 / H-Bridge 1B** | direct/bypass to ETB motor |
| VVT | **G4 / AUX6** | low-side PWM after OCV power conversion |
| VVL | **G3 / H-Bridge 2A** | retain MWR mapping |
| Boost solenoid | **G22 / Injector 6** | MAC valve low-side PWM + external flyback diode |
| ETB TPS main | **B31 / Analog 1** | redundant ETB position |
| ETB TPS check | **B4 / Analog 2** | redundant ETB position |
| A/C pressure | **B17 / Analog 3** | retain MWR mapping |
| Pedal PPS check | **B30 / Analog 4** | redundant pedal position |
| Fuel pressure | **B35 / Analog 5** | Link MIPS 101-0325 |
| Oil pressure | **B37 / Analog 6** | Link MIPS 101-0325 |
| Pedal PPS main | **B18 / TPS input** | primary pedal position |
| Flex fuel | **B9 / Flex Fuel input** | Continental frequency signal |
| WBO VS | **B6** | LSU 4.9 native circuit |
| WBO IP | **B19** | LSU 4.9 native circuit |
| WBO RCAL | **B22** | LSU 4.9 native circuit |
| WBO VGND | **B33** | LSU 4.9 native circuit |
| WBO heater low side | **G19** | LSU heater control |
| +5 V sensor reference | **B26/B34 as allocated** | TPS/PPS + pressure sensors |
| sensor ground | **B29/B38/B39 as allocated** | sensor returns; not chassis ground |

The MWR map leaves physical switch inputs B10, B23, and B36 available unless later continuity/configuration work shows otherwise. Do not allocate them merely because they are free.

---

## 5. DBW / VVT jumper modification

### MWR cable-throttle baseline

- G2 / H-Bridge 1A -> VVT;
- G10 / H-Bridge 1B -> unassigned;
- G3 / H-Bridge 2A -> VVL;
- G4 / AUX6 -> IAC.

### Baseline Plus

- **G2 / H-Bridge 1A -> ETB motor A**;
- **G10 / H-Bridge 1B -> ETB motor B**;
- **G4 / AUX6 -> VVT low-side PWM**;
- **G3 / H-Bridge 2A -> VVL unchanged**;
- old IAC path -> unused.

The factory 2000 2ZZ VVT OCV is a two-wire ECU-driven actuator (`OCV+` / `OCV−`). AUX6 low-side control therefore requires an electrical conversion, not merely a one-wire repin:

```text
EFI-relay-switched +12 V -> one factory OCV conductor -> VVT solenoid
VVT solenoid -> other factory OCV conductor -> G4 / AUX6 low-side PWM
```

Identify the actual OCV+/OCV− paths through the MWR jumper/PCB before construction. Keep the OEM engine-side connector/wiring intact where practical.

DBW motor polarity remains provisional until the EMU DBW wizard confirms direction.

---

## 6. Baseline Plus sub-harness wiring diagram

```text
                                   EMU BLACK
                                      |
                 +--------------------+----------------------+
                 |                                           |
          MWR JUMPER / PCB                           NEW SUB-HARNESS
                 |                                           |
 EFI +12V -----------> VVT OCV conductor                    |
 G4 AUX6 ------------> other OCV conductor / VVT low side   |
 G3 HB2A ------------> MWR VVL route                        |
 old IAC route ------> UNUSED                               |
                                                             |
              +----------------------+-----------------------+----------------+
              |                      |                       |                |
             ETB                   PEDAL                 FLEX FUEL        PRESSURE
              |                      |                       |                |
 G2 HB1A ----> motor A               |                       |                |
 G10 HB1B ---> motor B               |                       |                |
 B31 AIN1 <--- TPS main              |                       |                |
 B4 AIN2  <--- TPS check             |                       |                |
 +5V -------> ETB ref                |                       |                |
 SGND ------> ETB ground             |                       |                |
                                     |                       |                |
 B18 TPS <--------------------------- PPS main               |                |
 B30 AIN4 <-------------------------- PPS check              |                |
 +5V -------------------------------- pedal refs             |                |
 SGND ------------------------------- pedal grounds          |                |
                                                             |                |
 B9 FLEX <--------------------------------------------------- frequency      |
 switched/fused +12V ---------------------------------------> FF power       |
 SGND ------------------------------------------------------> FF ground      |
                                                                              |
 B35 AIN5 <------------------------------------------------ Link fuel signal |
 B37 AIN6 <------------------------------------------------ Link oil signal  |
 +5V ------------------------------------------------------> Link +5V        |
 SGND -----------------------------------------------------> Link grounds    |

 LSU 4.9: B19 IP / B33 VGND / G19 heater low / B22 RCAL / B6 VS
           + fused switched +12 V to LSU heater high side

 BOOST CONTROL:
 fused EFI +12 V -> MAC coil -> G22 / INJ6 low-side PWM
                    flyback diode across coil; stripe toward +12 V
```

### Construction rules

- DBW motor and WBO heater conductors must be sized for actuator/heater current.
- Use automotive TXL/GXL/XLPE-class wire and sealed automotive connector practices.
- Keep 5-V sensor returns on EMU sensor ground; do not substitute chassis ground.
- Keep WBO VGND dedicated to the native WBO circuit.
- Preserve the Toyota pedal's dual reference and ground conductors individually through the branch until the final sensor-reference splice topology is verified.
- Keep sensor wiring away from coil/injector power wiring where practical.
- Build the added harness as a removable sub-harness independent of the MWR/OEM harness.
- Label every modified MWR-jumper cavity by EMU pin and original MWR function.
- Disconnect Tru-Boost as the MAC electrical driver before EMU boost control is enabled.

---

## 7. Protection and control strategy enabled by Baseline Plus

Sensor hardware selection does **not** freeze protection thresholds. Thresholds, delays, hysteresis, actions, and recovery behavior require real commissioning data and tuner review.

Baseline Plus enables these high-value relationships:

1. **lambda actual vs target** — native LSU 4.9;
2. **fuel pressure vs MAP** — effective injector differential pressure;
3. **oil pressure vs RPM** — main-gallery analog protection;
4. **MAP vs boost ceiling** — overboost protection;
5. **boost target vs MAC duty** — ECU-integrated boost control;
6. **knock vs RPM/load**;
7. **CLT/IAT vs permitted load/boost**.

EGT is intentionally deferred because meaningful pre-turbine installation on the current manifold would expand Baseline Plus into engine/manifold removal work.

---

## 8. Open physical-verification gates

These are **verification gates, not reopened architecture decisions**:

1. ETB -> 2000 manifold fit: bolt pattern, bore/gasket alignment, motor clearance, charge-pipe orientation.
2. MWR jumper routing: identify G2/G4/G10/G3 destinations and factory OCV+/OCV− paths before VVT/DBW repinning.
3. Fuel-feed hose size before buying Radium end adapters.
4. Oil-sender bracket location before choosing hose length/end geometry.
5. Low-pressure warning-switch adapter/switch arrangement on the sandwich plate.
6. Actual ETB/pedal connector cavity verification against the 2005 EWD and purchased hardware.
7. MAC coil continuity/resistance/suppression behavior before G22 connection.
8. MAC de-energized plumbing -> mechanical spring/base boost.
9. Baseline Plus IAT source/path; current assumption is the temperature element in the factory MAF assembly.

---

## 9. Final-build handoff

Do not grow this document into the final custom-harness design. The final MAP/IAT/MAF-delete/CAN-expansion direction is owned by [`FINAL_SENSOR_TOPOLOGY.md`](FINAL_SENSOR_TOPOLOGY.md); final connector production approval is owned by [`HARNESS_CONNECTORS.md`](HARNESS_CONNECTORS.md); ECU commissioning evidence belongs in [`EMU_COMMISSIONING.md`](EMU_COMMISSIONING.md).

### Primary manufacturer references

- Link MIPS `101-0325`: https://dealers.linkecu.com/HWPS150
- Radium split-flow adapter: https://www.radiumauto.com/products/split-flow-flex-fuel-sensor-adapter
- EMU Black legacy pinout: https://www.ecumaster.com/wp/wp-content/uploads/2020/05/EMU_Black_pinout.pdf
- MWR sandwich plate: https://www.monkeywrenchracing.com/product/mwr-oil-filter-sandwich-adapter-oil-temp-pressure-turbo-feed/
- Bosch LSU 4.9: https://www.bosch-motorsport.com/content/downloads/Raceparts/en-GB/51865867208058251.html
