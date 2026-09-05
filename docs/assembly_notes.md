Assembly Notes and Silkscreen Requirements

1) Silkscreen
- Every component must have its designator and exact value printed on the top silkscreen. Example: R1: 10K, C1: 100uF/63V, Q1: 2SC5200
- For all power and driver transistors, print pinouts B, C, E under the footprint exactly oriented to the footprint pin order.
- Polarity markings: mark + and - for all electrolytic capacitors, diodes, bridge rectifier, and LEDs.
- Terminal labels: AC IN, GND (0V), SPK+, SPK-, AUDIO IN, GND.

2) Mechanical & PCB
- PCB dimensions: 200 mm x 100 mm, 1.6 mm FR4, 2 oz copper.
- Use wide copper traces / fills for V+, V-, GND and speaker out (3.5–4.5 mm). Leave unmasked tinned areas for manual solder beefing.
- Heatsink mounting: footprint with insulated mounting holes for TO-3P / TO-247 style power devices. Keep mechanical drill holes aligned with standard heatsink hardware.

3) Power & Safety
- PSU sized for transformer secondaries from 24-0-24 VAC up to 45-0-45 VAC @ 5–10 A.
- Design for worst-case ±64 V DC rails. Use 100 V diodes where needed and 80 V electrolytics or higher rated caps.
- Include inrush NTC footprint and glass fuse holder for mains input.

4) Protection
- Speaker protection relay footprint (12 V coil, 30 A contacts). Include DC-detect circuit footprint and manual mute/soft-start footprints.

5) Files to be generated next
- KiCad v7 schematic (.kicad_sch) across multiple sheets: power supply, preamp, driver, output, protection.
- KiCad PCB (.kicad_pcb) with full top silkscreen and copper pours.
- Assembly placement PDF (top/bottom), silk-only PDF.
- Gerber RS-274X + NC drill, and pick-and-place CSV.

