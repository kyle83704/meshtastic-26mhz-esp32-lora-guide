# Identify your board before flashing

> **Read-only identification guide.** The commands below identify the ESP chip
> and SPI flash; they do not intentionally write firmware, erase flash, or
> burn eFuses. Connecting can reset the board and esptool may temporarily load
> a helper stub into RAM.

## Chip identity is not board identity

An ESP32 identification result describes the MCU, not the PCB. Identify these
as separate facts:

1. **MCU family, model, and silicon revision**
2. **Main crystal frequency** reported by the tool, if shown
3. **Physical SPI flash capacity** detected from the flash chip
4. **PCB manufacturer, model, revision, pin mapping, and wiring**
5. **External LoRa radio model and intended RF band**

CPU revision is not PCB revision. A 26 MHz crystal reading does not establish
that OLED, power-control, or radio pins match this firmware.

## Install esptool in an isolated Python environment

These Windows PowerShell commands create a separate virtual environment in
your user profile. They do not modify PlatformIO's Python environment:

```powershell
py -m venv "$HOME\esptool-venv"
& "$HOME\esptool-venv\Scripts\python.exe" -m pip install --upgrade pip
& "$HOME\esptool-venv\Scripts\python.exe" -m pip install "esptool==5.4.0"
& "$HOME\esptool-venv\Scripts\python.exe" -m esptool version
```

If `py` is not installed, install Python with its Windows launcher or replace
`py` with the path to the Python executable you intend to use. Keep the
virtual-environment path unchanged in the commands below, or substitute the
path where you created it.

Record the version command's output. This guide uses the esptool 5.4.0
command-line syntax.

## Read chip connection details and flash ID

Close applications that may hold the serial port, connect the board, and
replace `COM4` with the port shown by Windows Device Manager. `COM4` is only
an example:

```powershell
& "$HOME\esptool-venv\Scripts\python.exe" -m esptool --port COM4 flash-id
```

Do not pass `--chip` for initial identification: esptool should auto-detect
the chip. The connection output identifies the detected chip and may report
its model/revision, features, and crystal frequency. The `flash-id` command
reports the flash manufacturer ID, device ID, and detected flash capacity.
Record only fields actually shown; do not infer missing values.

The official esptool documentation describes the
[`flash-id` command](https://docs.espressif.com/projects/esptool/en/latest/esp32/esptool/basic-commands.html#read-spi-flash-id-flash-id)
and
[automatic chip detection](https://docs.espressif.com/projects/esptool/en/latest/esp32/esptool/basic-options.html#chip-type-chip-c).
Command behavior and output can vary by esptool version and chip.

The normal connection process can reset the board. Depending on the chip and
esptool, it can also load a temporary flasher helper into RAM. These
identification commands do not intentionally program flash, erase flash, or
burn eFuses. This guide does not require or recommend an eFuse dump.

## Record the observed results

Keep the output with the board specimen's photos and notes. Redact any MAC
address before sharing logs publicly; a MAC address identifies an individual
device, not a PCB model. Never post credentials, encryption keys, or full
security dumps.

| Field | Observation |
|---|---|
| esptool version | Record the `version` output |
| MCU family/model/revision | Record the connection output |
| Features | Record the connection output |
| Detected crystal frequency | Record if reported |
| Flash manufacturer ID | Record the `flash-id` output |
| Flash device ID | Record the `flash-id` output |
| Detected physical flash capacity | Record the `flash-id` output |
| PCB manufacturer/model/revision | Read from the PCB or reliable board documentation; otherwise unknown |
| External LoRa radio and markings | Read from the radio IC/module; otherwise unknown |
| Antenna/frequency label | Photograph and transcribe; otherwise unknown |
| Specimen and wiring notes | Keep separate for each physical board |

Detected flash capacity is not the same as the firmware's configured flash
layout. A chip reporting a capacity larger than the build layout does not
make a different layout, partition table, or firmware image safe to use.

## Identify the external radio and PCB separately

ESP32 identification does **not** identify an external SX1278 or another
LoRa radio. Read the radio IC/module markings and consult documentation for
the exact PCB revision. A RadioLib/SX127x register-detection result may not
uniquely distinguish every SX127x variant. Do not treat a detected radio
family as proof of the board's RF band, antenna, wiring, or regulatory
suitability.

Before recommending the recorded custom firmware to a board, verify every
item in the [compatibility checklist](../README.md#compatibility-warning).
In particular, common SX1278 hardware is not suitable for the usual US
915 MHz Meshtastic band. Do not automatically recommend `EU_433` to a user
in the United States; region and frequency must match the hardware and
applicable regulations.

## Advanced diagnostics

No eFuse command is needed to identify the MCU or flash. eFuse output can
contain sensitive security-provisioning information; do not publicly share a
full eFuse dump.
