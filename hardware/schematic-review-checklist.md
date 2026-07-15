# Plantie Hardware — Schematic Review Checklist

Electrical review of `control-unit` and `base-station` schematics. Netlists traced from the `.kicad_sch` files.
Date: 2026-07-13

---

## Must-fix before fab

- [ ] **Base station: add 5.1 kΩ Rd pull-downs on USB-C CC pins.** `CC1` (J1.A5) and `CC2` (J1.B5) are floating. Without Rd, a USB-C source / C-to-C cable won't apply VBUS and the base won't power up from many chargers. Add 5.1 kΩ from CC1→GND and CC2→GND.
- [ ] **Control unit: pull up ESP32-C3 GPIO2.** Must be high at boot (normal + download); currently floating. Add ~10 kΩ to 3V3.
- [ ] **Control unit: pull up ESP32-C3 GPIO8.** Must be high for reliable flashing; currently floating. Add ~10 kΩ to 3V3. *(GPIO9 is fine — internal pull-up.)*
- [ ] **Control unit: reconcile the LDO.** Schematic symbol = TPS7A02 (200 mA); BOM = AP7366EA-33W5-7 (600 mA). ESP32-C3 Wi-Fi TX peaks ~350 mA, so 200 mA browns out. Use the 600 mA AP7366 and update the schematic value to match (SOT-23-5 pinout is compatible).

## Finalize placeholder values

- [ ] **C7, C8 (control unit) = TBD.** These sit on the dock DATA lines = native USB D-/D+ (GPIO18/19). Keep to a few pF or mark DNP — real capacitance corrupts USB.
- [ ] **R7, R8 (control unit).** 0 Ω in schematic vs "?" in BOM — reconcile.
- [ ] **R11 (control unit) = "100k/1M".** Sets Q1 gate divider for dock-detect power-down. 1 MΩ = lower docked quiescent current. Pick one.

## Confirm design intent

- [ ] **GPIO6↔GPIO18 and GPIO7↔GPIO19 are hard-tied** (routed to dock via R7/R8). Looks intentional (dock = USB programming + data), but it's a direct GPIO-to-GPIO short. Ensure firmware never drives both at once and GPIO6/7 stay hi-Z during USB flashing.
- [ ] **Dock-detect powers the MCU *down*.** When docked (V_IN present), Q1 disables the LDO → 3V3/ESP off while charging. Confirm this is intended (inverted if you wanted the MCU alive to report status while docked).
- [ ] **Improve 3V3 decoupling.** Only one 4.7 µF (C2) and no 100 nF at pin 1. Add ≥10 µF bulk + 100 nF close to the ESP 3V3 pin.
- [ ] **Verify sensor front-end.** FET2 high-side switch (GPIO10) → R5/R6 ≈ ÷11 divider → IO5 ADC with C5+C6 ≈ 9.4 µF (~1 s RC). Check attenuation and settling time vs ADC range/sample rate.

## Minor

- [ ] **Charge-path sensing.** Charger BAT → connector → LM66100 → cell. CE-to-VOUT wiring is correct (dock→battery, blocks reverse), but connector + diode sit between the charger's sense point and the cell → cell tops off slightly below 4.2 V. Acceptable given split enclosure.
- [ ] **Base station charge LED brightness.** LED anodes share R4 = 10 kΩ from VBUS → ~0.3 mA (very dim). Drop to ~1–2 kΩ for visible indication.
- [ ] **Base station PWR_FLAG placement (ERC).** PWR_FLAGs sit on I_REF/I_MIN nets rather than V_EXT/V_BAT; harmless but may raise ERC power warnings on the supply rails.

## Verified OK

- Connector pinout (VIN / DATA+ / DATA- / GND) consistent between both boards.
- USB D+/D- polarity consistent (DATA+ → GPIO19/D+, DATA- → GPIO18/D-).
- Base-station charge programming correctly wired: I_REF = 1.5 kΩ, I_MIN = 240 kΩ on separate nets (not shorted).
- LM66100 ideal-diode enable (CE → VOUT) is the correct reverse-blocking configuration.

---

**References**
- LM66100 datasheet (TI): https://www.ti.com/lit/ds/symlink/lm66100.pdf
- ESP32-C3 boot-mode / strapping (esptool): https://docs.espressif.com/projects/esptool/en/latest/esp32c3/advanced-topics/boot-mode-selection.html
- ESP32-C3 hardware design guidelines: https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c3/esp-hardware-design-guidelines-en-master-esp32c3.pdf
