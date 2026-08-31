# 2ZZ Custom Harness — Connector Verification Register

**Vehicle basis:** 2000 US-spec Toyota Celica GT-S  
**Final controls direction:** ECUMaster EMU Black, purpose-built engine/control harness, DBW, flex fuel  
**Checkpoint:** 2026-08-31

## Purpose

This document is the controlled connector/BOM starting point for the final custom 2ZZ harness.

The objective is not merely to identify connectors that look correct. Each production connector should progress through a verification chain:

1. **FACTORY-DOC** — Toyota application and connector part number are supported by the applicable EWD.
2. **SUPPLIER-CROSS-REFERENCE** — a purchasable connector kit and its supplied terminal/seal set are cross-referenced to that Toyota housing.
3. **PHYSICAL-FIT** — the housing positively mates, keys, and latches to the actual device intended for the build.
4. **TERMINAL-VERIFIED** — terminal, seal, secondary-lock/cavity-plug requirements, and supported wire range are confirmed against the received hardware and intended wire.
5. **PRODUCTION-APPROVED** — wire specification, pin map, tooling/process, branch ownership, and quantity are frozen for the final harness BOM.

Do not promote a connector to `PRODUCTION-APPROVED` based on appearance or catalog cross-reference alone.

## Factory references

Primary factory references for this pass:

- **EWD399U** — 2000 Celica Electrical Wiring Diagram, ZZT230/ZZT231.
- **EWD590U** — 2005 Celica Electrical Wiring Diagram, ZZT230/ZZT231.

The 2000 EWD remains authoritative for the chassis/year baseline. The 2005 EWD is deliberately used where the final build adopts late 2ZZ hardware, especially DBW and the later knock-sensor architecture.

## First-pass OEM connector verification

The following **unwired Ballenger Motorsports connector kits** were ordered as physical samples. One kit per unique housing is sufficient for initial fit verification; shared housings are intentionally not duplicated in this purchase pass.

| Group | Intended device(s) | Toyota housing PN | 2000 EWD basis | 2005 EWD basis | Ballenger kit | Current status | Notes |
|---|---|---:|---|---|---|---|---|
| H01 | Crankshaft position sensor / camshaft position sensor | **90980-10947** | C4 crank; C1 cam | C4 crank; C1 cam | **CONN-75800** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Same housing used for crank and cam. Verify both actual sensors. |
| H02 | VVT oil-control valve / VVL oil-control valve | **90980-11162** | C3 VVT; C2 VVL | C3 VVT; C2 VVL | **CONN-76007** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Same housing used for both OCVs. Verify both devices. |
| H03 | VVTL oil-pressure switch / OEM oil-pressure switch | **90980-11363** | O2 oil-pressure switch; the 2000 EWD also assigns this housing to E7 engine-oil-level warning switch | E7 VVTL pressure switch; O2 oil-pressure switch | **CONN-76068** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Ballenger also explicitly lists the kit for 2ZZ OPS and lift/MOPS applications. Verify both intended final-build switches. |
| H04 | Engine coolant temperature sensor | **90980-11062** | E6 | E6 | **CONN-75820** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Retained OEM Toyota ECT sensor architecture. |
| H05 | Late 2ZZ knock sensor | **90980-11875** | **Not the 2000 knock housing**; 2000 K1 is 90980-11166 | K1 2ZZ-GE | **CONN-75757** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Final build intentionally targets the later 2-wire 2ZZ knock architecture. Toyota also uses this housing number for factory injectors. |
| H06 | Ignition coil / integrated igniter | **90980-11885** | I2-I5 | I2-I5 | **CONN-75727** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | One sample will validate housing fit; production harness requires four coil connectors. |
| H07 | 2005 2ZZ DBW throttle body / throttle-position assembly | **90980-11858** | Not part of the 2000 cable-throttle engine architecture | T1 2ZZ-GE | **CONN-75805** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Selected late-engine architecture; verify against the actual throttle body chosen for the final intake. |
| H08 | 2005 accelerator-pedal position sensor | **90980-11144** | Not part of the 2000 cable-throttle engine architecture | A17 | **CONN-76021** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Verify against the owned 2003-2005 Celica DBW pedal. |
| H09 | Alternator / generator control connector | **90980-11349** | G2 | G2 | **CONN-75736** | FACTORY-DOC + SUPPLIER-CROSS-REFERENCE; PHYSICAL-FIT PENDING | Verify against the alternator intended for the final drivetrain. |

## Supplier-reported housing / terminal / seal register

This table records what Ballenger says is contained in each ordered kit. It is useful procurement data, but it is **not yet physical verification** and does not by itself establish final production wire gauge or OEM connector manufacturer.

| Group | Ballenger housing SKU | Supplier family / attribution | Supplied terminal | Supplied seal | Supplier terminal/wire range | Status |
|---|---|---|---|---|---|---|
| H01 | **CONN-100510** | Sumitomo TS, supplier-identified | **CONN-11856** | **CONN-00145** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H02 | **CONN-100927** | Japanese VVT housing; exact OEM manufacturer TBD | **CONN-11856** | **CONN-00145** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H03 | **CONN-100724** | Toyota 090/TS-type pressure-switch application; OEM manufacturer TBD | **CONN-11856** | **CONN-00145** | terminal 20–16 AWG; connector listing 22–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H04 | **CONN-100317** | 1.8 mm SSC / Econoseal-type application; OEM manufacturer TBD | **CONN-00127** | **CONN-00119** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H05 | **CONN-11855** | Toyota-style 2.3 mm sealed application; OEM manufacturer TBD | **CONN-11856** | **CONN-00119** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H06 | **CONN-100307** | Toyota coil 2.3 mm sealed application; OEM manufacturer TBD | **CONN-11856** | **CONN-00119** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H07 | **CONN-100647** | Toyota/Mazda 2.3 mm sealed TPS/DBW application; OEM manufacturer TBD | **CONN-11856** | **CONN-00145** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H08 | **CONN-100959** | Older Toyota 090-I, supplier-identified | **CONN-11656** | **CONN-00145** | terminal 22–16 AWG; seal listed 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |
| H09 | **CONN-100300** | Sumitomo TS; supplier gives Sumitomo **6189-0443 / 6189-0442** | **CONN-11856** | **CONN-00145** | 20–16 AWG | SUPPLIER-CROSS-REFERENCE; VERIFY RECEIVED KIT |

### Emerging commonality — do not bulk-buy yet

Supplier data indicates that **CONN-11856** is used by H01, H02, H03, H05, H06, H07, and H09. H01/H02/H03/H07/H09 use **CONN-00145** seals, while H05/H06 use **CONN-00119**. H04 and H08 are the clear terminal-family outliers in this first pass.

That commonality is encouraging for final tooling and spares, but loose-terminal quantities should wait until the received kits are inspected and the final wire construction/gauge is chosen.

## Arrival inspection procedure

For each kit/device pair:

- keep the Ballenger bag/label with the connector during inspection;
- photograph the connector and label beside the target device;
- inspect keying and cavity count before mating;
- verify full insertion and positive latch engagement without forcing the connector;
- verify reasonable removal/unlatch behavior;
- record all molded manufacturer, family, and cavity markings visible on the housing;
- compare the supplied terminals, seals, and secondary-lock features with the supplier-reported register above;
- do **not** install loose terminals merely for fit testing unless the correct extraction method/tool is available;
- mark the row `PHYSICAL-FIT VERIFIED` only after the intended actual device has been tested;
- promote terminal/seal information to `TERMINAL-VERIFIED` only after received hardware and intended wire are reconciled.

If a shared housing is intended for multiple devices, test every intended device before promoting the housing to production use.

## Deferred connector families

The first-pass order intentionally does **not** freeze connectors for the following items:

- final injectors;
- dedicated external MAP sensor;
- dedicated EMAP / exhaust-pressure sensor;
- standalone IAT sensor;
- fuel-pressure sensor;
- oil-pressure transducer;
- oil-temperature sensor;
- flex-fuel sensor;
- LSU wideband lambda sensor/interface;
- EGT thermocouple/interface;
- boost-control solenoid;
- E153 transmission switches/sensors;
- any CAN expansion or additional development instrumentation.

These should be selected from the actual final sensor/actuator package and EMU I/O/CAN architecture rather than inherited blindly from either factory harness.

## Production harness BOM target

The eventual harness BOM should be rebuildable without chat history and should identify, for every device:

- device and quantity;
- year/application basis;
- housing PN and connector family;
- terminal and seal PN for the selected wire size;
- cavity plugs / secondary locks;
- pin number and signal name;
- EMU Black or chassis destination;
- wire type, gauge, color, and approximate length;
- twisted-pair / shielding requirements;
- sensor-ground, power-ground, and splice ownership;
- branch protection, heat/abrasion protection, and strain relief;
- required crimp/extraction tooling;
- evidence and verification state.

Crimp tooling is intentionally **not selected yet**. Connector-family and terminal verification comes first; tooling should follow the final terminal population rather than drive it.
