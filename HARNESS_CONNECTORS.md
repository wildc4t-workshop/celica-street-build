# 2ZZ Custom Harness — Connector Verification Register

**Vehicle basis:** 2000 US-spec Toyota Celica GT-S  
**Final controls direction:** ECUMaster EMU Black, purpose-built engine/control harness, DBW, flex fuel  
**Checkpoint:** 2026-08-31

## Purpose

This document is the controlled connector/BOM starting point for the final custom 2ZZ harness.

The objective is not merely to identify connectors that look correct. Each production connector should progress through a verification chain:

1. **FACTORY-DOC** — Toyota application and connector part number are supported by the applicable EWD.
2. **SUPPLIER-CROSS-REFERENCE** — a purchasable connector kit is cross-referenced to that Toyota housing.
3. **PHYSICAL-FIT** — the housing positively mates, keys, and latches to the actual device intended for the build.
4. **TERMINAL-VERIFIED** — terminal, seal, secondary-lock/cavity-plug requirements, and supported wire range are confirmed.
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
| H01 | Crankshaft position sensor / camshaft position sensor | **90980-10947** | C4 crank; C1 cam | C4 crank; C1 cam | **CONN-75800** | ORDERED — PHYSICAL-FIT PENDING | Same housing used for crank and cam. Verify both actual sensors. |
| H02 | VVT oil-control valve / VVL oil-control valve | **90980-11162** | C3 VVT; C2 VVL | C3 VVT; C2 VVL | **CONN-76007** | ORDERED — PHYSICAL-FIT PENDING | Same housing used for both OCVs. Verify both devices. |
| H03 | VVTL oil-pressure switch; OEM oil-pressure switch where applicable | **90980-11363** | 2000 application requires device-level verification | E7 VVTL pressure switch; O2 oil-pressure switch | **CONN-76068** | ORDERED — PHYSICAL-FIT PENDING | Verify both target switches before treating as shared production housing. |
| H04 | Engine coolant temperature sensor | **90980-11062** | E6 | E6 | **CONN-75820** | ORDERED — PHYSICAL-FIT PENDING | Retained OEM Toyota ECT sensor architecture. |
| H05 | Late 2ZZ knock sensor | **90980-11875** | **Not the 2000 knock housing**; 2000 K1 is 90980-11166 | K1 2ZZ-GE | **CONN-75757** | ORDERED — PHYSICAL-FIT PENDING | Final build intentionally targets the later 2-wire 2ZZ knock architecture. Toyota also uses this housing number for factory injectors; do not assume terminal usage is identical without inspection. |
| H06 | Ignition coil / integrated igniter | **90980-11885** | I2-I5 | I2-I5 | **CONN-75727** | ORDERED — PHYSICAL-FIT PENDING | One sample will validate housing fit; production harness requires four coil connectors. |
| H07 | 2005 2ZZ DBW throttle body / throttle-position assembly | **90980-11858** | Not part of the 2000 cable-throttle engine architecture | T1 2ZZ-GE | **CONN-75805** | ORDERED — PHYSICAL-FIT PENDING | Selected late-engine architecture; verify against the actual throttle body chosen for the final intake. |
| H08 | 2005 accelerator-pedal position sensor | **90980-11144** | Not part of the 2000 cable-throttle engine architecture | A17 | **CONN-76021** | ORDERED — PHYSICAL-FIT PENDING | Verify against the owned 2003-2005 Celica DBW pedal. |
| H09 | Alternator / generator control connector | **90980-11349** | G2 | G2 | **CONN-75736** | ORDERED — PHYSICAL-FIT PENDING | Verify against the alternator intended for the final drivetrain. |

## Arrival inspection procedure

For each kit/device pair:

- keep the Ballenger bag/label with the connector during inspection;
- photograph the connector and label beside the target device;
- inspect keying and cavity count before mating;
- verify full insertion and positive latch engagement without forcing the connector;
- verify reasonable removal/unlatch behavior;
- record all molded manufacturer, family, and cavity markings visible on the housing;
- compare the supplied terminals, seals, and secondary-lock features with the housing;
- record Ballenger terminal/seal SKUs from the kit label/catalog where supplied;
- do **not** install loose terminals merely for fit testing unless the correct extraction method/tool is available;
- mark the row `PHYSICAL-FIT VERIFIED` only after the intended actual device has been tested.

If a shared housing is intended for multiple devices, test every intended device before promoting the housing to production use.

## Terminal / seal register

Do not bulk-purchase terminals or seals from this table yet. Populate it from the physical kits and supplier documentation, then reconcile it against the actual wire sizes selected for the harness.

| Housing group | OEM connector manufacturer | Manufacturer housing PN | Terminal PN / supplier SKU | Seal PN / supplier SKU | Wire range | Secondary lock / cavity plug | Status |
|---|---|---|---|---|---|---|---|
| H01 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H02 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H03 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H04 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H05 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H06 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H07 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H08 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |
| H09 | TBD | TBD | TBD | TBD | TBD | TBD | VERIFY FROM KIT |

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
