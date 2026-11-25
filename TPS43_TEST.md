# TPS43 test firmware

The `tps43-test-left` and `tps43-test-right` artifacts are an opt-in split pair.
Flash both halves together. The existing `normal-left` and `normal-right`
artifacts remain the known-working keyboard pair.

This test assumes an Azoteq TPS43 (IQS5xx) on the **right** half, at I2C
address `0x74`, with these nice!nano connections:

| TPS43 signal | nice!nano pin | Status |
| --- | --- | --- |
| SDA | D2 / P0.17 | Assumed |
| SCL | D3 / P0.20 | Assumed |
| RDY | D9 / P1.06 | Assumed |
| NRST | D10 / P0.09 | Assumed |
| VDDHI | 3.3 V | Must verify |
| GND | GND | Must verify |

The right key matrix already uses D8 / P1.04, so do not use example wiring
that puts TPS43 RDY on D8. Azoteq's TPS43 connector pin order is 1 RDY,
2 NRST, 3 GND, 4 VDDHI, 5 SCL, 6 SDA. Verify the actual breakout or FPC
orientation before connecting power or flashing this test pair.

This firmware can build even when the assumed touchpad pins do not match the
PCB. A successful build does not validate the hardware wiring. Once the
wiring is confirmed, update the GPIOs in `tps43_right.overlay` and retest.
Because pointing changes the BLE HID descriptor, remove the old Bluetooth
pairing and pair the central half again before testing mouse input.

References:
- https://www.azoteq.com/wp-content/uploads/2023/11/proxsense_i2c_trackpad_datasheet_v1.05_v1.05.pdf
- https://github.com/AYM1607/zmk-driver-azoteq-iqs5xx
- https://zmk.dev/docs/hardware-integration/pointing
