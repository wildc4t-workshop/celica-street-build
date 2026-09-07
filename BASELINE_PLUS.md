# Baseline Plus — Controls, Fuel, DBW, and Protection Package

**Vehicle:** 2000 US-spec Toyota Celica GT-S / current 2ZZ-GE  
**ECU:** legacy ECUMaster EMU Black, V3.061  
**Vehicle interface:** MWR EMU Black adapter + short black/gray jumper harness  
**Status:** SELECTED architecture; physical fit/routing verification still required where explicitly noted  
**Checkpoint:** 2026-09-07

## 1. Purpose

Baseline Plus is the controlled current-engine commissioning stage before the built-2ZZ/E153/final-custom-harness swap.

It intentionally validates, on the running current engine:

- the MWR return-style fuel system;
- larger injectors and a fresh tune;
- EMU Black through the MWR adapter;
- OEM-style 2ZZ DBW;
- flex-fuel sensing;
- native Bosch LSU wideband control;
- fuel-pressure protection;
- oil-pressure protection;
- EMU-native electronic boost control using the existing AEM Tru-Boost MAC solenoid;
- retained Celica body/cluster integration through the MWR PCB.

The MWR PCB remains the integration bridge. Selected EMU pins are bypassed or reassigned in the short EMU-to-MWR jumper, allowing new functions to be added without cutting the MWR PCB or requiring the final custom engine harness now.

Before removal of the PowerFC, complete `PRE_EMU_BASELINE.md` capture sets, including the native map archive and controlled datalogs.

---

## 2. Locked decisions

### DEC-STREET-001 — Baseline Plus uses the MWR adapter as an integration bridge

**Decision:** Retain the MWR PCB and OEM 2000 engine harness for Baseline Plus. Modify only the short EMU-to-MWR jumper where necessary and add a dedicated controls sub-harness.

**Reason:** Preserves known Celica integration while allowing DBW, flex fuel, pressure sensing, native wideband, and boost control to be validated before the final harness.

### DEC-STREET-002 — Pull OEM DBW forward into Baseline Plus

**Decision:** Use the ordered 2003–2005 Celica GT-S 2ZZ electronic throttle body if it physically fits the 2000 intake manifold and passes bench validation. Use the already-owned 2003–2005 Celica accelerator pedal.

**Output strategy:**

- H-Bridge 1A + 1B -> ETB motor;
- move VVT from H-Bridge 1A to AUX6, using the MWR PCB's existing VVT downstream path after the required low-side conversion is verified;
- delete the cable-throttle IAC function, freeing AUX6;
- leave VVL on H-Bridge 2A unless physical verification disproves the MWR-map assumption.

### DEC-STREET-003 — Standardize Baseline Plus pressure sensors on Link MIPS

**Decision:** Use Link ECU / Honeywell MIPS `101-0325` 150-PSI media-isolated pressure sensors for both fuel pressure and oil pressure.

Manufacturer characteristics:

- 5 V supply, 4.75–5.25 V allowed;
- 0.5 V at 0 PSI to 4.5 V at 150 PSI output;
- 0–150 PSI / 10 bar range;
- 1/8 NPT male;
- stainless media-isolated construction;
- -40 to 125 °C rating;
- compatible with oil, petrol, ethanol, diesel, coolant, and water;
- 3-way Metri-Pack 150 connector kit included.

This signal architecture is directly compatible with EMU Black 0–5 V analog inputs. Fuel and oil use the same sensor type so one calibration, connector family, and spare strategy can be maintained.

Linear transfer function:

`Pressure [psi] = 37.5 × Voltage [V] - 18.75`

**Superseded error:** an earlier Baseline Plus summary accidentally substituted AEM `30-2130-150` pressure sensors after the Link MIPS sensors had already been selected. The Link `101-0325` sensors are the authoritative Baseline Plus selection.

### DEC-STREET-004 — Use a current GM/Continental flex-fuel sensor in Radium split-flow housing

**Decision:** Use Radium Engineering Split-Flow Flex Fuel Sensor Adapter `20-0589` with current GM/Continental sensor `13507129`.

`13507129` supersedes `13577429`; Radium explicitly lists both as compatible with the adapter. EMU Black provides a dedicated flex-fuel frequency input.

Preferred placement: fuel feed, upstream of injectors, subject to final hose/fitting verification.

Do **not** lock the Radium 10AN-ORB-to-line-size end adapters until the actual Baseline Plus feed-hose size is physically confirmed.

### DEC-STREET-005 — Replace external UEGO controller with native LSU 4.9

**Decision:** Use genuine Bosch LSU 4.9 `0 258 017 025` / `17025` wired directly to the EMU Black native wideband controller.

The AEM UEGO controller/gauge is not the primary ECU lambda source in Baseline Plus. EMU Black supports LSU 4.2 and LSU 4.9 directly; LSU 4.9 is selected as the standardized current sensor architecture.

### DEC-STREET-006 — Oil-pressure architecture

**Decision:**

- factory 2ZZ main-gallery oil-pressure port -> dedicated remote-mounted Link MIPS `101-0325` analog sensor;
- MWR oil-filter sandwich plate `MWR-901465` -> turbo oil feed + low-pressure warning switch;
- do not tee the primary analog oil-pressure measurement with the turbo feed.

Remote sender line uses a true Toyota-port-compatible BSP connection, a short -3AN flexible line, and an NPT adapter at the sensor. Final bracket/hose length is determined on the car.

### DEC-STREET-007 — Move boost control from Tru-Boost to EMU Black

**Decision:** Reuse the existing MAC boost-control solenoid supplied with the AEM Tru-Boost system, but move PWM control to the EMU Black.

- EMU output: **G22 / Injector 6**;
- Injector 6 is unused in the recovered MWR base map and can be assigned as a low-side auxiliary PWM output;
- solenoid high side: fused EFI-relay-switched +12 V;
- solenoid low side: G22 / Injector 6;
- install an external flyback diode across the MAC coil because Injector 6 does not have the internal flyback diode provided on AUX3–AUX6;
- diode cathode/striped end -> switched +12 V side of the coil;
- diode anode -> G22 / low-side coil terminal;
- disconnect the Tru-Boost gauge/controller from the solenoid driver circuit before EMU control is enabled;
- the Tru-Boost gauge may remain temporarily as a display only if desired;
- retain the existing pneumatic plumbing initially, but verify that the de-energized solenoid state produces mechanical wastegate/base boost before closed-loop boost control is commissioned.

No MAC-valve part-number lookup is required for the architecture. Before connection to the EMU, verify basic coil continuity/resistance and absence of an unexpected internal suppression diode/connection to the valve body.

---

## 3. Baseline Plus buy list

| System | Item | Selected part | Qty | Status / installation note |
|---|---|---|---:|---|
| Flex fuel | Split-flow housing | **Radium 20-0589** | 1 | BUY; 10AN ORB end ports; feed-line preferred |
| Flex fuel | Ethanol-content sensor | **GM/Continental 13507129** | 1 | BUY; current replacement for 13577429; dedicated EMU flex input |
| Flex fuel | Radium end adapters | **10AN ORB -> actual feed-line size** | 2 | HOLD until feed-hose size is measured |
| Fuel pressure | Pressure transducer | **Link MIPS 101-0325, 150 PSI** | 1 | BUY; install in MWR rail's 1/8-NPT pressure port |
| Lambda | Wideband sensor | **Bosch LSU 4.9 0 258 017 025 / 17025** | 1 | BUY; direct to EMU native WBO circuit |
| Lambda | LSU 4.9 mating connector / terminals | matching Bosch connector kit | 1 | BUY unless supplied with harness solution |
| Oil | Oil-filter sandwich adapter | **MWR MWR-901465** | 1 | BUY; 3 × 1/8 NPT ports; 1ZZ/2ZZ fit |
| Oil pressure | Pressure transducer | **Link MIPS 101-0325, 150 PSI** | 1 | BUY; remote-mounted from factory gallery port |
| Oil pressure | Block adapter | **1/8 BSPT/BSP Toyota port -> -3AN male** | 1 | BUY; verify true fit before tightening into aluminum casting |
| Oil pressure | Remote hose | **short -3AN PTFE/braided pressure hose** | 1 | BUY after bracket location; target roughly 6–18 in |
| Oil pressure | Sender-end adapter | **-3AN male -> 1/8-NPT female** | 1 | BUY; steel/stainless preferred |
| Oil pressure | Sender bracket | rubber-lined P-clamp + simple bracket | 1 | FAB/BUY after sensor arrives |
| Oil system | Low-pressure switch relocation | NPT-compatible switch or correct NPT/BSP adapter strategy | 1 | VERIFY before ordering; lives on sandwich plate |
| Boost control | MAC 3-port boost-control solenoid from AEM Tru-Boost | existing hardware | 1 | **OWNED**; EMU takes over PWM control |
| Boost control | External flyback diode | suitable automotive diode sized for MAC-coil current | 1 | BUY during harness build; cathode to +12 V, anode to G22 side |
| DBW | 2003–2005 Celica GT-S 2ZZ ETB | OEM late-Celica ETB | 1 | **PURCHASED 2026-09-07**; manifold fit test pending |
| DBW | 2003–2005 Celica accelerator pedal | OEM late-Celica pedal | 1 | **OWNED** |
| DBW | ETB connector kit | Ballenger verification kit | 1 | **OWNED / verify fit** |
| DBW | Pedal connector kit | Ballenger verification kit | 1 | **OWNED / verify fit** |
| Harness | EMU flying-lead terminals/wires | existing printed flying-lead harness | as needed | **OWNED**; harvest/repurpose for bypass/sub-harness |

### Current primary-source references

- Link MIPS `101-0325`: https://dealers.linkecu.com/HWPS150
- Radium split-flow adapter: https://www.radiumauto.com/products/split-flow-flex-fuel-sensor-adapter
- EMU Black product/pinout: https://www.ecumaster.com/products/emu-black/ and https://www.ecumaster.com/wp/wp-content/uploads/2020/05/EMU_Black_pinout.pdf
- MWR sandwich plate: https://www.monkeywrenchracing.com/product/mwr-oil-filter-sandwich-adapter-oil-temp-pressure-turbo-feed/
- MWR fuel rail: https://www.monkeywrenchracing.com/product/mwr-billet-fuel-rail-toyota-lotus/
- Bosch LSU 4.9: https://www.bosch-motorsport.com/content/downloads/Raceparts/en-GB/51865867208058251.html
- AEM Tru-Boost controller/solenoid documentation: https://documents.aemelectronics.com/techlibrary_30-4350_tru-boost_controller_gauge.pdf

---

## 4. Selected interim EMU I/O allocation

This is the **Baseline Plus allocation**, not the final custom-harness allocation.

| Function | EMU physical channel | Baseline Plus assignment |
|---|---|---|
| ETB motor A | **G2 / H-Bridge 1A** | bypass MWR PCB -> ETB motor |
| ETB motor B | **G10 / H-Bridge 1B** | bypass MWR PCB -> ETB motor |
| VVT | **G4 / AUX6** | low-side VVT control after OCV power/routing conversion is verified |
| VVL | **G3 / H-Bridge 2A** | retain MWR mapping |
| Boost-control solenoid | **G22 / Injector 6** | existing AEM/MAC valve; low-side PWM with external flyback diode |
| ETB TPS main | **B31 / Analog 1** | redundant ETB position channel |
| ETB TPS check | **B4 / Analog 2** | redundant ETB position channel |
| A/C pressure | **B17 / Analog 3** | retain MWR mapping |
| Pedal PPS check | **B30 / Analog 4** | redundant pedal position channel |
| Fuel pressure | **B35 / Analog 5** | Link MIPS 101-0325 |
| Oil pressure | **B37 / Analog 6** | Link MIPS 101-0325 |
| Pedal PPS main | **B18 / TPS input** | main accelerator-pedal channel |
| Flex fuel | **B9 / Flex Fuel input** | GM/Continental frequency signal |
| WBO VS | **B6** | LSU 4.9 native connection |
| WBO IP | **B19** | LSU 4.9 native connection |
| WBO RCAL | **B22** | LSU 4.9 native connection |
| WBO VGND | **B33** | LSU 4.9 native connection |
| WBO heater low side | **G19** | LSU 4.9 heater control |
| +5 V sensor reference | **B26/B34 as allocated** | TPS/PPS and pressure sensors |
| sensor ground | **B29/B38/B39 as allocated** | sensor returns; do not use chassis ground |

### Remaining physical switch inputs

The MWR map leaves the three legacy EMU Black switched-to-ground inputs available unless later continuity/configuration work shows otherwise:

- B10 / Switch 1
- B23 / Switch 2
- B36 / Switch 3

Do not assign these merely because they are available.

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
- **G4 / AUX6 -> VVT low-side PWM** through the appropriate factory OCV conductor;
- **G3 / H-Bridge 2A -> VVL unchanged**;
- old MWR IAC destination -> unused.

The 2000 2ZZ factory VVT oil-control valve is originally driven by two ECU conductors, OCV+ and OCV−. Moving VVT to AUX6 therefore requires more than a one-wire repin: one factory OCV conductor must become EFI-relay-switched +12 V and the other must become the AUX6 low-side PWM conductor. The actual MWR jumper/PCB continuity and OCV+/OCV− ownership must be identified before construction.

The MWR PCB itself is not cut or modified for the POC. The change occurs in the removable short jumper/sub-harness architecture.

DBW motor polarity is provisional until the EMU DBW wizard confirms direction.

---

## 6. Baseline Plus sub-harness wiring diagram

```text
                                  EMU BLACK
                                      |
                 +--------------------+--------------------+
                 |                                         |
          MWR JUMPER / PCB                         NEW SUB-HARNESS
                 |                                         |
 G4 AUX6 --------+--> VVT OCV low-side PWM                 |
 EFI-switched +12V --> other VVT OCV conductor             |
 G3 HB2A ------------> MWR VVL route                       |
 old AUX6/IAC route ---- UNUSED                            |
                                                           |
                         +---------------------------------+----------------+
                         |                                 |                |
                    ETB BRANCH                        PEDAL BRANCH      FF BRANCH
                         |                                 |                |
 G2  HB1A  ------------> Motor A                           |                |
 G10 HB1B  ------------> Motor B                           |                |
 B31 AIN1  <------------ TPS main                          |                |
 B4  AIN2  <------------ TPS check                         |                |
 +5V ref   ------------> ETB sensor reference              |                |
 sensor GND ------------> ETB sensor ground                |                |
                                                           |                |
 B18 TPS IN <--------------------------------------------- PPS main         |
 B30 AIN4   <--------------------------------------------- PPS check        |
 +5V ref    ---------------------------------------------- pedal refs       |
 sensor GND ---------------------------------------------- pedal grounds    |
                                                                            |
 B9 FLEX IN <-------------------------------------------------- frequency ---+
 switched/fused +12V ------------------------------------------ FF power
 sensor ground ------------------------------------------------ FF ground

                         +-------------------+-------------------+
                         |                                       |
                 FUEL-PRESSURE BRANCH                    OIL-PRESSURE BRANCH
                         |                                       |
 B35 AIN5 <------------- Link MIPS signal           B37 AIN6 <-- Link MIPS signal
 +5V ref  -------------> Link MIPS +5V              +5V ref  --> Link MIPS +5V
 sensor GND ------------> Link MIPS ground           sensor GND -> Link MIPS ground
                         |                                       |
                         |                                 remote -3AN line
                         |                                       |
                    MWR fuel rail                         2ZZ gallery port

                         LSU 4.9 BRANCH
                               |
 B19 WBO IP    ----------> LSU pin/function per Bosch/EMU diagram
 B33 WBO VGND  ----------> LSU virtual ground
 G19 WBO heater ----------> LSU heater low side
 switched/fused +12V ----> LSU heater +
 B22 WBO RCAL  ----------> LSU calibration resistor circuit
 B6  WBO VS    ----------> LSU Nernst/VS circuit

                    BOOST-CONTROL BRANCH
                               |
 EFI-relay switched/fused +12V +-------> MAC solenoid -------> G22 / INJECTOR 6
                               |               coil                    low-side PWM
                               |                |
                               +----|<----------+
                                    flyback diode
                         cathode/stripe toward +12V
                         anode toward G22 / low side

 Tru-Boost solenoid-driver wires: DISCONNECTED before EMU boost control is enabled
 Existing MAC pneumatic plumbing: RETAIN initially; verify de-energized = spring/base boost
```

### Construction rules

- DBW motor pair and WBO heater supply: size for actuator/heater current; use automotive TXL/GXL/XLPE-class wire and appropriate fuse/relay ownership.
- TPS/PPS/pressure/flex signals: automotive sensor wire; route away from coil/injector power where practical.
- Use EMU sensor ground for 5-V sensors; do not substitute chassis ground.
- Keep WBO VGND dedicated to the native WBO circuit.
- Preserve Toyota pedal dual reference/ground conductors individually through the pedal branch until actual calibration and plausibility behavior are verified.
- Build the sub-harness so it can be disconnected independently from the MWR/OEM harness.
- Label every bypassed MWR-jumper cavity by both physical EMU pin and original MWR function.
- MAC boost solenoid gets switched +12 V on one side and G22 low-side PWM on the other; do not leave the Tru-Boost controller electrically connected as a second driver.
- Use the required external flyback diode across the MAC coil because Injector 6 lacks the AUX3–AUX6 internal flyback circuit.

---

## 7. Protection and control strategy enabled by Baseline Plus

No protection threshold is considered selected merely because the sensor is selected. Thresholds require real sensor calibration, tuner review, and running-engine validation.

Highest-value relationships:

1. **Lambda actual vs lambda target** — native LSU 4.9 feedback/protection.
2. **Fuel pressure vs MAP** — monitor effective injector differential pressure and detect pump/filter/regulator/supply failure.
3. **Oil pressure vs RPM** — analog main-gallery pressure; use RPM-dependent protection rather than one universal pressure threshold.
4. **MAP vs boost ceiling** — independent overboost boundary.
5. **EMU boost target vs MAC duty** — move boost control into the ECU so boost target/duty can participate in engine-state and protection strategies.
6. **Knock vs RPM/load** — commission the native knock strategy before relying on it.
7. **CLT/IAT vs load** — progressively reduce permitted load/boost where appropriate rather than depending only on ignition retard.

EGT is deliberately deferred in Baseline Plus because the current manifold installation makes a meaningful pre-turbine probe an unjustified engine-removal/manifold-removal scope expansion.

---

## 8. Open physical-verification gates

The architecture is selected, but these physical facts must be checked before fabrication/installation:

1. **Late-Celica ETB -> 2000 manifold:** bolt/stud pattern, bore/gasket alignment, motor clearance, charge-pipe orientation.
2. **MWR jumper / VVT routing:** continuity-confirm G2 existing VVT destination, G4 existing IAC destination, G10 state, G3 VVL destination, and identify factory OCV+ / OCV− paths before converting VVT to switched +12 V plus AUX6 low-side PWM.
3. **Feed hose size:** measure actual fuel-feed connection before buying Radium 10AN-ORB end adapters.
4. **Oil sender bracket:** choose hose length and end geometry only after a cool, serviceable mounting location is identified.
5. **Low-pressure switch relocation:** choose correct NPT-compatible switch or adapter arrangement for the sandwich plate after physical clearance is known.
6. **Toyota ETB/pedal connector cavities:** verify against the 2005 EWD and the actual purchased hardware before terminating the production sub-harness.
7. **MAC valve electrical check:** verify coil continuity/resistance and diode-test behavior before connecting G22; confirm no unexpected case connection/internal suppression behavior.
8. **Boost plumbing failsafe:** with the existing MAC valve de-energized, confirm the wastegate system produces mechanical spring/base boost before closed-loop EMU control.

These are verification gates, not reopened architecture decisions.

---

## 9. Final-harness MAP transition / ETB-adapter opportunity

Baseline Plus may continue using the EMU Black's internal MAP sensor and the existing manifold-pressure hose because the interim ECU/MWR installation does not justify rebuilding the intake interface solely to eliminate that hose.

When the EMU Black moves into the cabin and the full custom harness is built, the preferred final architecture is:

- **external MAP sensor mounted at the intake manifold / throttle-body interface**;
- a **very short local pressure path** rather than a long vacuum hose running across the engine bay and through the firewall;
- external MAP wired directly to an EMU analog input as a primary engine-control signal;
- EMU Black internal pressure sensor available for BARO/reference duty if the final calibration architecture benefits from it;
- lower-priority instrumentation can migrate to CAN expansion if an analog input must be freed for MAP.

### ETB adapter design opportunity

If the late-Celica electronic throttle body does **not** bolt directly to the 2000 intake manifold and an adapter plate is required, treat that adapter as a useful integration component rather than a simple bolt-pattern converter.

**Design direction:** incorporate a dedicated MAP-sensor boss/interface into the ETB adapter, inspired by the compact Honda-style arrangement that places the MAP sensor directly beside the throttle body/manifold entry. This would allow use of a compact, readily available external MAP sensor with essentially no remote vacuum plumbing.

Concept:

```text
charge pipe -> ETB -> adapter / MAP boss -> 2000 intake manifold
                          |
                          +-> compact external MAP sensor
                               |
                               +-> short pressure cavity
                               +-> 5 V / signal / sensor ground
                               +-> direct EMU analog input in final harness
```

**Not yet selected:** exact Honda/OEM MAP sensor part number, pressure range, bolt pattern, O-ring geometry, clocking, or adapter machining dimensions. Do not copy a Honda sensor interface from memory. Select a specific sensor first, obtain the dimensional/transfer-function data, and design the boss around that actual hardware.

This is a **final-build packaging direction**, not additional Baseline Plus scope and not a reason to fabricate an ETB adapter if the late-Celica throttle body already bolts directly to the 2000 manifold.
