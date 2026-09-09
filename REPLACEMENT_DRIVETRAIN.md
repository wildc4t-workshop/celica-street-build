# Replacement Drivetrain — Build Record and Reassembly Control

**Vehicle:** 2000 US-spec Toyota Celica GT-S  
**Assembly:** MWR-built 2ZZ-GE + E153 conversion drivetrain  
**Record checkpoint:** 2026-09-09

## 1. Purpose

This document is the durable engineering record for the replacement 2ZZ/E153 package: historical build provenance, current physical state, reassembly sequence, and hardware control.

The drivetrain was previously assembled and run as a complete MWR-converted package. This is **not** a first-time E153 conversion. The immediate goal is to identify what was separated, restore missing hardware deliberately, and re-mate the known package without repeating conversion work unnecessarily.

Evidence labels used here:

- `MANUFACTURER-INVOICE` — original MWR invoice 107098 dated 2010-05-07;
- `MANUFACTURER-INSTRUCTIONS` — MWR E153 Transmission Kit installation instructions revised 2011-03-21;
- `USER-REPORTED` — current physical state reported during 2026 reassembly planning;
- `SEPARATELY-ESTABLISHED` — existing project state supported outside the invoice.

The original invoice is not committed to this public repository because it contains personal billing/shipping information. The engineering-relevant line items are transcribed below.

## 2. Original MWR engine build record

`MANUFACTURER-INVOICE` — MWR invoice 107098, 2010-05-07.

| Invoice item | Part number | Documented configuration |
|---|---|---|
| MWR Built Engine — Toyota 2ZZ-GE | MWR-22XXXX | MWR-built 2ZZ-GE assembly |
| Mahle piston set | MAH-21090-820 | 82 mm, **9.0:1 compression** |
| MWR main bearing set | MWR-300415 | coated 1ZZ/2ZZ main bearings |
| MWR rod bearing set | MWR-300421 | 2ZZ rod bearings |
| MWR stainless valve set | MWR-300820 | 16 stainless valves |
| MWR titanium valve retainer set | MWR-300602 | 16 titanium retainers |
| MWR 4340 H-beam connecting rods | MWR-350121 | forged H-beam rod set |
| Circuitworx oil-pump gear set | CWX-OP-2ZZ | upgraded 2ZZ oil-pump gears |

The invoice includes a credit for **re-use of the water pump and oil pan** at the time of the 2010 build. That is historical build-state evidence only; it does not override later/current hardware changes such as the present Moroso pan.

### What this invoice resolves

- Compression ratio is **9.0:1**, not the previous ~9.5:1 recollection.
- Pistons are Mahle 82 mm MAH-21090-820.
- Rods are MWR 4340 H-beam units.
- Stainless valves and titanium retainers are directly documented.
- Main/rod bearing sets and Circuitworx oil-pump gears are directly documented.

### What this invoice does not prove

The invoice does not explicitly enumerate:

- block sleeving;
- ARP main/head/rod hardware;
- valve springs;
- head-gasket specification;
- timing-chain/lift-bolt/thermostat replacements;
- current water pump;
- current Moroso oil pan;
- current harmonic balancer.

Those items remain `SEPARATELY-ESTABLISHED` or require physical/record verification rather than being inferred from this invoice.

## 3. Original E153 conversion record

`MANUFACTURER-INVOICE` — the same invoice documents the E153 conversion work after a transmission failure during dyno work.

| Invoice item | Part number | Documented configuration |
|---|---|---|
| MWR E153 Install Kit — Celica 2000-05 | MWR-410101 | E153 conversion/install package |
| MWR forged-steel flywheel | MWR-400372 | 2ZZ-GE to E153 flywheel |
| ACT clutch | ACT-TM1-XTSS | E153/MR2-T clutch, XTMM designation on invoice |
| Speed Source clutch push rod | SSI-CLROD-ZZ | ZZ-vehicle clutch pushrod |
| Speed Source stainless clutch line | SSI-CLL-CELI7 | Celica 2000+ clutch line |

The invoice also documents 10 hours of transmission/clutch installation labor.

The repository's existing state identifies the transmission as an E153 with factory LSD. The invoice corroborates the E153 conversion package but does **not** independently identify the LSD variant.

## 4. Historical turbo note

The invoice also records a Garrett **GT3082R / GT30/40** turbocharger, part GPP-GT3082R, with a GT3076-style compressor housing and no turbine housing, plus installation and dyno/street tuning labor.

This is historical provenance only. It does not supersede the current turbo/hot-side architecture or current fallback hardware documented elsewhere in the project.

## 5. Current physical state before reassembly

`USER-REPORTED` — 2026-09-09:

- MWR adapter plate remains bolted to the built 2ZZ.
- Flywheel and clutch have been removed from the engine.
- Starter is believed to be absent/unaccounted for.
- E153 remains substantially complete and strapped to a pallet.
- Only the hardware/components necessary to separate the engine and transmission were removed.

This means the reassembly should be treated as **re-mating a previously converted engine/transmission package**, not performing the MWR conversion from scratch.

## 6. Reassembly sequence

### Phase A — inventory before assembly

1. Photograph the complete adapter-plate face of the 2ZZ before disturbing anything.
2. Photograph the E153 bellhousing and all remaining attached brackets/components.
3. Collect every loose drivetrain fastener, spacer, dowel, retainer, and clutch component.
4. Sort hardware into the labeled bins in Section 8.
5. Verify both stock 10 mm engine-block dowels are present.
6. Verify the MWR 10 mm plate-to-transmission dowel is present.
7. Verify the MWR stepped plate-to-transmission dowel is present.
8. Confirm whether the starter is actually missing.
9. Do not begin assembly until the mating hardware inventory is complete.

### Phase B — engine side

1. Inspect adapter plate seating and accessible fasteners.
2. Obtain/prepare the correct Celica/2ZZ starter if missing.
3. MWR instructions require both starter mounting holes to be opened with a **7/16 in drill bit** for the conversion.
4. Install the MWR forged-steel E153 flywheel using the fastener specification appropriate to the actual flywheel bolts in hand.
5. Install the ACT E153 clutch with the correct alignment tool and verified pressure-plate fastener specification.

MWR's generic flywheel-bolt values are:

- ARP flywheel bolts with oil: **115 N·m**;
- stock flywheel bolts: **49 N·m + 90°**.

Do not apply either value until the actual installed bolt type is identified.

### Phase C — transmission side

Before mating, verify the E153 still has the previously converted components expected from the MWR installation:

- converted shifter shaft/crank arrangement;
- clutch release fork;
- release bearing;
- VSS or VSS-hole plug as applicable;
- shifter-cable adapter hardware;
- stub axle/intermediate-axle hardware as applicable.

### Phase D — mate engine and transmission

1. Align the E153 to the MWR adapter using the plate-to-transmission dowels.
2. Do not use bolts to force the transmission onto the engine.
3. Install transmission-to-adapter fasteners only in their documented locations.
4. Verify every bolt reaches clamp load without bottoming.
5. Torque only after the assembly is fully seated on the dowels.

## 7. MWR torque references

`MANUFACTURER-INSTRUCTIONS`:

| Fastener size | MWR torque |
|---|---:|
| 12 mm bolt | 65 N·m |
| 10 mm bolt | 48 N·m |
| 8 mm bolt | 24 N·m |
| 5/8 in bolt | 100 N·m |
| 1/2 in bolt | 70 N·m |
| 7/16 in bolt | 50 N·m |

Special case: the twelve M10×50 inner-halfshaft flange socket-head bolts are specified at **65 N·m with Loctite**, not the generic 48 N·m M10 value.

MWR recommends re-torquing the conversion fasteners after a couple of heat cycles.

## 8. Permanent hardware-bin system

MWR explicitly warns that incorrect bolt lengths and washer stacks can strip threaded holes or allow a bolt to bottom before clamping. Keep this hardware as a dedicated drivetrain kit rather than mixing it into general shop fasteners.

### E153-01 — ADAPTER TO 2ZZ

`MANUFACTURER-INSTRUCTIONS` / MWR plate diagram:

| Qty | Hardware | Washer stack | Location |
|---:|---|---|---|
| 2 | 12 mm bolts | no washer | upper counterbored adapter holes into engine |
| 1 | 7/16 × 2 in | 1 washer | upper starter location |
| 1 | 7/16 × 2 in | 3 washers | lower starter location |
| 2 | 7/16 × 1.500 in | 1 washer each | right-side adapter-to-engine locations |
| 2 | 7/16 × 1.125 in | 1 washer each | lower adapter-to-engine locations |

Minimum loose 7/16-in washer count for this group: **8**.

**Bin label:** `DO NOT CHANGE WASHER STACKS — SEE MWR PLATE DIAGRAM`

### E153-02 — ADAPTER TO TRANSMISSION

| Qty | Hardware | Washer stack | Location |
|---:|---|---|---|
| 2 | 1/2 in bolts | 2 washers each | two upper transmission holes |
| 1 | 7/16 × 2.25 in | 2 washers | lower-front hole |
| 4 | M10×30 | no washer | remaining two lower + two rear holes |

**Bin label torques:** `1/2 = 70 N·m | 7/16 = 50 N·m | M10 = 48 N·m`

### E153-03 — DOWELS / SPECIAL PIECES

Keep separately bagged and labeled:

- 2 stock 10 mm engine-block dowels;
- MWR 10 mm plate-to-transmission dowel;
- MWR stepped plate-to-transmission dowel;
- shifter-arm dowel pin;
- slave-cylinder spacers;
- any axle retaining clips or unique conversion spacers discovered during inventory.

Do not treat these as commodity hardware.

### E153-04 — AXLES

MWR specifies:

- 12 × M10×50 Allen/socket-head inner-halfshaft flange bolts;
- **65 N·m + Loctite**.

After grade and pitch are verified from an original fastener, keep 4–6 additional identical spares.

### E153-05 — SHIFTER

MWR-specified conversion hardware includes:

- M10×45 bolt + washer;
- M10×30 bolt + washer;
- shifter-arm dowel pin;
- 2 × M8×20 bolts for the shifter-cable mount adapter;
- stock shifter-cable mount bolts and retainer clips.

### E153-06 — CLUTCH / SLAVE

Keep together:

- M8×30 slave-cylinder bolts;
- MWR slave-cylinder spacers;
- Speed Source pushrod;
- pressure-plate bolts;
- release-bearing/fork clips and related hardware;
- clutch alignment tool if dedicated to this clutch.

The spacers between the slave cylinder and transmission are mandatory in the MWR instructions.

### E153-07 — MOUNTS / INTERMEDIATE AXLE

MWR instructions identify:

- 2 × M10×55 bolts, nuts, and washers for the RH engine mount in addition to stock bolts;
- stock top transmission-mount hardware;
- 2 stock rear-transmission-mount bolts;
- supplied rear-isolator bolts and through-bolt — size not stated in the instructions;
- stock front-mount hardware;
- 3 stock bolts for intermediate-axle support to engine block;
- 2 stock bolts for intermediate axle to support.

Keep each mount location in its own inner bag inside this bin.

## 9. Spare-hardware purchasing policy

### Safe to stock after grade/pitch verification

Keep a small service quantity of:

- M10×30;
- M10×50 socket-head axle bolts;
- M10×55;
- M10×45;
- M8×20;
- M8×30;
- matching metric washers;
- medium-strength threadlocker.

### Do not bulk-order from nominal diameter alone

The MWR instructions do not fully specify thread pitch/TPI, strength grade, washer dimensions, or all bolt lengths for:

- 7/16-in adapter hardware;
- 1/2-in transmission hardware;
- 12 mm adapter bolts;
- rear-isolator hardware;
- special dowels;
- slave-cylinder spacers.

Identify one known-good original or obtain an MWR specification before defining the permanent spare-bin part number.

## 10. Open verification items

- Confirm starter disposition.
- If missing, identify the correct replacement 2ZZ/Celica starter and perform the MWR 7/16-in mounting-hole modification.
- Inventory all E153-01 through E153-07 hardware before reassembly.
- Verify actual flywheel bolt type before torqueing.
- Verify pressure-plate bolt specification.
- Verify thread pitch and strength grade for any replacement conversion fasteners before purchase.
- Confirm current harmonic-balancer make/model separately; the 2010 invoice does not document it.
- Keep separately established claims such as sleeving/ARP hardware/valve springs distinct from invoice-confirmed build content until their own evidence is located.
