# Design System: AEC Spatial Matrix (Brainrot Report 777)

`DESIGN.md` specification defining the design system, tokens, and component guidelines for **Brainrot Report 777** (`brainrotreport-777`), an autonomous AI construction intelligence and spatial MEP coordination platform.

---

## 1. Visual Theme & Atmosphere

**Industrial Precision Matrix**: High-contrast, dark-mode engineering HUD (Heads-Up Display) engineered for jobsite field trailers, high-glare tablet screens, and multi-monitor desktop takeoff workstations.

Surfaces evoke precision CAD/BIM drafting environments: deep carbon canvas (`#090D16`), sub-pixel blueprint gridlines (`#1E293B`), glowing vector polylines, and razor-sharp numeric telemetry. Visual density is intentionally high, prioritizing informational throughput, spatial coordinate readouts, and immediate color-coded hazard detection.

---

## 2. Color Palette & Roles

### Primary Foundation
- **Matrix Void (`#090D16`)**: Deepest root background surface. Absorbs eye fatigue during long coordination sessions.
- **Carbon Container (`#0F172A`)**: Primary card, panel, and drawer container surface.
- **Plenum Slate (`#1E293B`)**: Secondary container background, table row borders, and subtle CAD gridline overlays.
- **Blueprint Hairline (`#334155`)**: Structural dividers, coordinate rulers, and inactive element borders.

### Discipline Accents & Spatial Vectors
- **Cyan Vector (`#06B6D4`)**: **Mechanical (HVAC)** systems, supply/return duct runs, CFM flow vectors, and diffuser markers.
- **Electric Emerald (`#10B981`)**: **Plumbing** systems, sanitary waste DAGs, domestic water lines, and fixture nodes.
- **Amber Voltage (`#F59E0B`)**: **Electrical** distribution, panel schedules, homeruns, and NEC 110.26 working clearance envelopes.
- **Structural Titanium (`#94A3B8`)**: Architectural walls, column gridlines ($A\text{--}Z, 1\text{--}99$), and slab elevations.

### Functional Clash & Severity States
- **Laser Red (`#EF4444`)**: **Hard 3D Clash** (physical solid intersection) and structural beam penetrations.
- **Warning Amber (`#F59E0B`)**: **Code / Clearance Violation** (NEC 110.26 working space or minimum ceiling height breach).
- **Info Cobalt (`#3B82F6`)**: **Maintenance & Accessibility Flag** (valve or VAV box damper access obstructed).
- **Verified Green (`#22C55E`)**: Coordinated & resolved spatial conflict.

### Typography Hierarchy
- **Pure White (`#F8FAFC`)**: Primary headings, critical dimension callouts, and active tab labels.
- **Muted Steel (`#94A3B8`)**: Secondary labels, sheet names (`M101`, `P102`), and schedule headers.
- **Ghost Carbon (`#64748B`)**: Timestamps, minor coordinate ticks, and disabled actions.

---

## 3. Typography Rules

- **Display & Headings (`Inter`, Sans-Serif)**:
  - `Display / Sheet Title`: `700` weight, `32px / 40px`, tracking `-0.02em`.
  - `H1 / Section Header`: `600` weight, `20px / 28px`, tracking `-0.01em`.
  - `H2 / Card Header`: `600` weight, `15px / 22px`, uppercase, tracking `0.05em`.
- **Telemetry & Engineering Numbers (`JetBrains Mono`, Monospace)**:
  - `Coordinate / Dimension Readout`: `500` weight, `13px / 18px`, tracking `0.0em`.
  - `Table Metrics (CFM, DFU, Amps)`: `600` weight, `12px / 16px`, tabular numbers (`font-variant-numeric: tabular-nums`).
  - `RFI Code Callouts`: `400` weight, `12px / 16px`, enclosed in `#1E293B` containers.

---

## 4. Component Stylings

### Plan Viewport (Spatial Canvas)
- **Container**: Full-bleed responsive canvas with Deep Zoom tiling support (`1024x1024` slippy-map tiles).
- **HUD Layer Bar**: Floating translucent bar (`#0F172A` at `85%` opacity, `12px` backdrop blur, `rounded-lg`, border `1px solid #334155`).
- **Discipline Filter Toggles**: Interactive pill chips with discipline accent glows (Cyan for HVAC, Emerald for Plumbing, Amber for Electrical).

### 3D Clash Inspector Panel
- **Drawer**: Right-anchored sliding drawer (`420px` width on desktop, full-width bottom sheet on mobile) with `#0F172A` background and `#1E293B` left border.
- **Clash Card**: `rounded-md` card with a `3px` left indicator border matching severity (`Laser Red` or `Warning Amber`).
- **Entity Comparison Pill**: Dual-badge chips showing `[M101: SF-4 24x12]` vs `[P101: 4" SAN]`.
- **RFI Quick-Action**: Prominent button with `Cyan Vector` hover glow to generate a Claude 3.5 Sonnet contractual RFI draft.

### Takeoff & Material Tables
- **Header**: `#090D16` sticky header with uppercase `Muted Steel` text and monospace sorting arrows.
- **Rows**: Alternating `#0F172A` and `#111827` zebra striping with `1px solid #1E293B` bottom border; hover highlight at `#1E293B`.
- **Totals Row**: `#1E293B` container with bold tabular numbers for aggregate linear footage and fixture units.

---

## 5. Layout Principles

- **Desktop (Takeoff Station)**:
  - Left Sidebar (`64px` collapsed / `240px` expanded): Sheet directory, project switcher, agent telemetry.
  - Center Viewport (Flex-1): High-res Deep Zoom plan viewer with coordinate ruler overlays.
  - Right Inspector (`420px` fixed): Multi-agent clash log, RFI generator, and takeoff bill of materials.
- **Mobile (Field Super & Apprentice View)**:
  - Fullscreen drawing and camera homography capture screen.
  - Floating bottom action bar (`56px` height, `#0F172A` backdrop blur) with quick capture, clash counter, and audio memo recorder.
- **Base Rhythm**: Strict `4px` / `8px` spatial grid with `16px` inner panel padding.

---

## 6. Design System Notes for Stitch Generation

- **Atmosphere Keywords**: Industrial dark-mode CAD HUD, precision engineering telemetry, deep carbon `#090D16`, glowing vector polylines, laser red clash accents.
- **Component Prompts**:
  - `Dark CAD plan viewer with floating frosted glass discipline layer toggles (HVAC cyan, Plumbing emerald, Electrical amber)`.
  - `AEC 3D clash inspection card with high-contrast badge comparing duct dimensions against sanitary pipe elevation with Claude 3.5 RFI generate button`.
  - `High-density takeoff table with tabular monospace numbers for CFM, linear pipe footage, and fixture counts`.
