# Meshtastic 26 MHz ESP32 T-LoRa32 V1 record

This repository records one custom Meshtastic firmware build for a tested
T-LoRa32 V1 specimen. It is not an official Meshtastic release and is not
necessarily based on the latest Meshtastic firmware.

## Compatibility warning

Do not flash this build based only on a “26 MHz” reading or a similar-looking
board. Before recommending it, ask the user to verify all of the following
for their individual board:
<img width="800" height="400" alt="LoRa32V10" src="https://github.com/user-attachments/assets/c2d350c4-2d20-44fb-b978-9f41c7df31a0" />

- Original ESP32 family, not ESP32-S2/S3/C3/etc.
- 26 MHz **main crystal** (not the SPI flash clock).
- Compatible OLED, power, and LoRa pin mapping for the exact PCB revision.
- Correct SX127x radio configuration and any required DIO1 wiring.
- Adequate physical flash and compatibility with the recorded partition
  layout.
- Appropriate antenna, frequency, and legal operating configuration.

The recorded build uses a 4 MB flash layout; the reported 16 MB physical flash
capacity is an owner observation, not independently confirmed here. Do not
recommend this build to every 26 MHz board.

Chip identity is not board identity. ESP32 detection does not identify an
external SX1278 or confirm its band, antenna, pin mapping, or suitability.
Common SX1278 hardware is not suitable for the usual US 915 MHz Meshtastic
band; do not automatically recommend `EU_433` to users in the United States.
See [Identify your board](docs/identify-your-board.md) before considering any
firmware.

The initial repository documents how to identify hardware without writing
flash and records the one board photograph currently supplied. Additional
specimen views and component markings are pending; no board identity is
inferred from the photo alone.

## Status

- Firmware is recorded as Meshtastic 2.8.2, PlatformIO environment
  `tlora-v1`, source commit
  [`c39728e3fb9304fc3b1160ebf14801b3b5f0a213`](https://github.com/meshtastic/firmware/commit/c39728e3fb9304fc3b1160ebf14801b3b5f0a213).
- Recorded build settings: 26 MHz main crystal, DIO flash mode, 40 MHz SPI
  flash clock, and a 4 MB configured flash layout. SPI flash clock and main
  crystal frequency are separate settings.
- A local successful build log and the exact named firmware images were
  inspected while preparing the release record. Their release is pending
  corresponding-source and third-party license/notice review, followed by
  separate approval. This repository does not currently distribute firmware
  binaries.
- Upload/hash verification and boot/desktop connection are owner-reported,
  not independently evidenced here.
- LoRa message exchange, range, Wi-Fi, and Bluetooth are not verified.

## Contents

- [Read-only hardware identification](docs/identify-your-board.md)
- [Board photo checklist and specimen record](docs/board-photo-checklist.md)

Do not post individual MAC addresses, credentials, encryption keys, or full
security dumps in issues or logs.
