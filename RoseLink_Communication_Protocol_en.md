# RoseLink Host Communication Protocol Reference (All Model Profiles)

Version: 1.0 · Compiled: 2026-10-22 · Target: Windows **RoseLink 3.8.2+5409**.

This reverse engineering reference is based on RoseLink's Dart AOT logic, runtime configuration, and OpenFun hardware communication. It is not a manufacturer specification. “All models” means the **46 concrete model profiles** found in this application version, including chip variants and the AURAMIC microphone. Two generic templates are listed separately. It does not claim coverage of every past or future product. 

Evidence levels: **H** = OpenFun hardware transaction; **S** = directly recovered program constants or control flow; **P** = partially decoded, with remaining fields or device behavior unconfirmed. Except for OpenFun battery communication explicitly marked H, models and control commands have not been tested individually. Command identifiers retain the application's spelling. Their names describe software intent; a shared family does not establish that every model supports every command.

Hexadecimal strings represent actual bytes. Offsets are zero-based. `n` is a decimal parameter; `NN` is its one-byte hexadecimal encoding. `CS` is an additive checksum, not a CRC. 

## Contents

1. [Models and protocol families](#1-models-and-protocol-families)
2. [Transports and UUIDs](#2-transports-and-uuids)
3. [RS Common, including OpenFun](#3-rs-common-including-openfun)
4. [F8](#4-f8)
5. [RS Common V2](#5-rs-common-v2)
6. [ZT and sender variants](#6-zt-and-sender-variants)
7. [Battery decoding comparison](#7-battery-decoding-comparison)
8. [Complete fixed command catalogue](#8-complete-fixed-command-catalogue)
9. [Parameterized commands and aliases](#9-parameterized-commands-and-aliases)
10. [Initialization, OTA, and implementation boundaries](#10-initialization-ota-and-implementation-boundaries)
11. [Evidence locations and further validation](#11-evidence-locations-and-further-validation)

## 1. Models and protocol families

Counts: F8 **6**, RS Common **7**, RS Common V2 **30**, ZT **3**. SPP entries reproduce the configuration flag, not a physical service discovery result. A dash means no explicit assignment in the inspected model configuration. `tws` = true wireless; `head` = headset; `mic` = microphone.

### F8

| Display name | Internal name | Type | SPP | Configuration class | ELF address |
| --- | --- | --- | --- | --- | --- |
| AGATE | `agate` | tws | false | `AgateDeviceConfig` | `0xbd88bc` |
| AGATE X | `agatex` | tws | false | `AgateXDeviceConfig` | `0xbd8908` |
| CERAMICS | `ceramics` | tws | false | `CeramicsDeviceConfig` | `0xbd8954` |
| CERAMICS-X | `CERAMICS-X-7034` | tws | true | `CeramicsX7034DeviceConfig` | `0xbd89a0` |
| CERAMICS-X | `CERAMICS-X` | tws | false | `CeramicsXDeviceConfig` | `0xbd8ba0` |
| EXPLORER | `EXPLORER` | tws | true | `ExplorerDeviceConfig` | `0xbd8c20` |


### RS Common

| Display name | Internal name | Type | SPP | Configuration class | ELF address |
| --- | --- | --- | --- | --- | --- |
| BudsFree i | `budsfree` | tws | false | `BudsfreeDeviceConfig` | `0xbd8c9c` |
| Cambrian | `cambrian` | head | true | `CambrianDeviceConfig` | `0xbd8ce8` |
| OpenFeel | `OpenFree` | tws | false | `OpenfreeDeviceConfig` | `0xbd8d7c` |
| OpenFun | `openfun` | tws | true | `OpenfunDeviceConfig` | `0xbd8dc8` |
| OpenLink | `openlink` | tws | true | `OpenlinkDeviceConfig` | `0xbd8e20` |
| SportFeel | `sportfree` | tws | true | `SportfreeDeviceConfig` | `0xbd8ec8` |
| SWIFT | `swift` | head | false | `SwiftDeviceConfig` | `0xbd8f20` |


### RS Common V2

| Display name | Internal name | Type | SPP | Configuration class | ELF address |
| --- | --- | --- | --- | --- | --- |
| Astro X | `astrox` | tws | true | `AstroXDeviceConfig` | `0xbd8f6c` |
| AURAMIC1[_xxxx] | `AURAMIC` | mic | false | `AuramicDeviceConfig` | `0xbd8ff8` |
| BudsFeel MK2 | `budsfeel_mk2` | tws | true | `BudsfeelMk2DeviceConfig` | `0xbd9140` |
| CERAMICS-MK2 | `ceramics-mk2-5736` | tws | true | `CeramicsMk25736DeviceConfig` | `0xbd91ec` |
| CERAMICS-MK2 | `ceramics-mk2-8912f` | tws | true | `CeramicsMk28912fDeviceConfig` | `0xbd9278` |
| CERAMICS-MK2 | `ceramics-mk2` | tws | true | `CeramicsMk2DeviceConfig` | `0xbd9304` |
| CERAMICS-U | `CERAMICS-U` | tws | true | `CeramicsUDeviceConfig` | `0xbd9390` |
| CERAMICS-U | `CERAMICS-U-HSC-2710` | tws | true | `CeramicsUHsc2710DeviceConfig` | `0xbd9434` |
| Coral | `coral` | tws | true | `CoralDeviceConfig` | `0xbd94d0` |
| EarFeel i7 | `earfeel-i7` | tws | true | `EarfeelI7DeviceConfig` | `0xbd955c` |
| FIRST ANGEL | `first-angel` | head | true | `FirstAngelDeviceConfig` | `0xbd95f8` |
| FURINA | `furina` | tws | true | `FurinaDeviceConfig` | `0xbd96b0` |
| Hyacinth | `hyacinth` | tws | true | `HyacinthDeviceConfig` | `0xbd973c` |
| Jasmine | `jasmine` | tws | true | `JasmineDeviceConfig` | `0xbd97a4` |
| KY01 | `ky01` | tws | true | `Ky01DeviceConfig` | `0xbd980c` |
| Agumon | `luminous_agumon` | tws | true | `LuminousAgumonDeviceConfig` | `0xbd9898` |
| Gabumon | `luminous_gabumon` | tws | true | `LuminousGabumonDeviceConfig` | `0xbd9900` |
| Patamon | `luminous_patamon` | tws | true | `LuminousPatamonDeviceConfig` | `0xbd9968` |
| Tailmon | `luminous_tailmon` | tws | true | `LuminousTailmonDeviceConfig` | `0xbd99d0` |
| Magician | `magician` | head | true | `MagicianDeviceConfig` | `0xbd9a38` |
| Mirror | `mirror` | tws | true | `MirrorDeviceConfig` | `0xbd9ac4` |
| MOKA | `moka` | head | true | `MokaDeviceConfig` | `0xbd9b2c` |
| Opal | `opal` | tws | true | `OpalDeviceConfig` | `0xbd9bb8` |
| OpenFeel L | `openfeel-l` | tws | true | `OpenfeelLDeviceConfig` | `0xbd9c20` |
| OpenFeel MK2 | `openfeel_mk2` | tws | true | `OpenfeelMk2DeviceConfig` | `0xbd9cac` |
| SPEAKFEEL | `speakfeel2` | head | true | `Speakfeel2DeviceConfig` | `0xbd9d38` |
| SpeakFeel | `speakfree` | head | true | `SpeakfreeDeviceConfig` | `0xbd9df0` |
| SportFeel | `sportfeel-8912f` | tws | true | `Sportfeel8912fDeviceConfig` | `0xbd9e5c` |
| SWIFT 2 | `swift2` | head | true | `Swift2DeviceConfig` | `0xbd9ee8` |
| Zircon | `zircon` | tws | true | `ZirconDeviceConfig` | `0xbd9f7c` |


### ZT

| Display name | Internal name | Type | SPP | Configuration class | ELF address |
| --- | --- | --- | --- | --- | --- |
| EarFree-i3 | `earfree-i3` | tws | false | `EarfreeI3DeviceConfig` | `0xbda008` |
| EarFeel-i5 | `EARFREE-i5` | tws | false | `EarfreeI5DeviceConfig` | `0xbda084` |
| HeadFeel | `headfree` | head | true | `HeadfreeDeviceConfig` | `0xbda0f0` |

The generic profiles are `RsCommonDeviceConfig` (`RS-Common`, rs-common, SPP=false) and `RsCommonV2DeviceConfig` (`RS-Common-v2`, rs-common-v2, SPP=true). AURAMIC inherits V2 and then sets mic identity and SPP=false. Its nickname is `AURAMIC1`, with `_xxxx` appended from the last four characters of a valid normalized address.

Names are preserved exactly: `OpenFree` displays as **OpenFeel**, `sportfree` as **SportFeel**, `EARFREE-i5` as **EarFeel-i5**, and `speakfeel2` as **SPEAKFEEL**. CERAMICS-X/MK2/U variants must remain distinct. These strings alone are not a complete Bluetooth name matching algorithm.

## 2. Transports and UUIDs

The application has Bluetooth Classic SPP/RFCOMM and BLE paths. OpenFun RFCOMM with service UUID `0cf12d31-fac3-4553-bd80-d6832e7b3931` was confirmed on hardware. Windows must resolve the service channel rather than assume a fixed RFCOMM channel. Release an existing RoseLink connection before opening the same service from another client. BLE uses a write characteristic and a subscribed read/notify characteristic; one notification or socket read is not necessarily one protocol frame.

The `uuid` (SPP identity), `serviceUUID` (normal BLE), `otaUuid`, and `otaServiceUUID` fields are distinct. Do not substitute one for another.

| Field | Short UUID | Expanded UUID |
| --- | --- | --- |
| Normal BLE service | 00FE | 000000fe-0000-1000-8000-00805f9b34fb |
| Write characteristic | 00F1 | 000000f1-0000-1000-8000-00805f9b34fb |
| Read/notify characteristic | 00F2 | 000000f2-0000-1000-8000-00805f9b34fb |

The above BLE values were observed in the OpenFun runtime object (S); its physical BLE services were not independently enumerated. They are not a verified universal service list for all models.

### Explicit SPP / OTA configuration

`G` = `0cf12d31-fac3-4553-bd80-d6832e7b3931`. UUID spelling is case-insensitive. A dash means not explicitly assigned here; it does not prove lack of OTA support. OTA columns are interface configuration only.

| Model | uuid / SPP | otaUuid | OTA BLE service | OTA write | OTA read |
| --- | --- | --- | --- | --- | --- |
| `agate` | — | — | — | — | — |
| `agatex` | — | — | — | — | — |
| `ceramics` | — | — | — | — | — |
| `CERAMICS-X-7034` | G | 00007034-0000-1000-8000-00805f9b34fb | — | — | — |
| `CERAMICS-X` | — | 00007034-0000-1000-8000-00805f9b34fb | — | — | — |
| `EXPLORER` | G | 00007034-0000-1000-8000-00805f9b34fb | — | — | — |
| `budsfree` | — | — | — | — | — |
| `cambrian` | G | G | ff12 | ff15 | ff14 |
| `OpenFree` | — | — | — | — | — |
| `openfun` | G | — | — | — | — |
| `openlink` | G | — | — | — | — |
| `sportfree` | G | — | — | — | — |
| `swift` | — | — | — | — | — |
| `astrox` | G | G | — | — | — |
| `AURAMIC` | G | — | — | — | — |
| `budsfeel_mk2` | G | 00001101-0000-1000-8000-00805F9B34FB | 0000AE00-0000-1000-8000-00805F9B34FB | 0000AE01-0000-1000-8000-00805F9B34FB | 0000AE02-0000-1000-8000-00805F9B34FB |
| `ceramics-mk2-5736` | G | G | — | — | — |
| `ceramics-mk2-8912f` | G | G | — | — | — |
| `ceramics-mk2` | G | G | — | — | — |
| `CERAMICS-U` | G | 00007034-0000-1000-8000-00805f9b34fb | 7033 | 2001 | 2002 |
| `CERAMICS-U-HSC-2710` | G | 66666666-6666-6666-6666-666666666666 | 66666666-6666-6666-6666-666666666666 | 77777777-7777-7777-7777-777777777777 | 77777777-7777-7777-7777-777777777777 |
| `coral` | G | G | — | — | — |
| `earfeel-i7` | G | 66666666-6666-6666-6666-666666666666 | 66666666-6666-6666-6666-666666666666 | 77777777-7777-7777-7777-777777777777 | 77777777-7777-7777-7777-777777777777 |
| `first-angel` | G | G | — | — | — |
| `furina` | G | G | — | — | — |
| `hyacinth` | G | — | — | — | — |
| `jasmine` | G | — | — | — | — |
| `ky01` | G | G | — | — | — |
| `luminous_agumon` | G | — | — | — | — |
| `luminous_gabumon` | G | — | — | — | — |
| `luminous_patamon` | G | — | — | — | — |
| `luminous_tailmon` | G | — | — | — | — |
| `magician` | G | G | — | — | — |
| `mirror` | G | — | — | — | — |
| `moka` | G | G | — | — | — |
| `opal` | G | — | — | — | — |
| `openfeel-l` | G | G | — | — | — |
| `openfeel_mk2` | G | G | — | — | — |
| `speakfeel2` | G | G | — | — | — |
| `speakfree` | G | G | — | — | — |
| `sportfeel-8912f` | G | G | — | — | — |
| `swift2` | G | 0cf12d32-fac3-4553-bd80-d6832e7b3931 | — | — | — |
| `zircon` | G | G | — | — | — |
| `earfree-i3` | — | — | — | — | — |
| `EARFREE-i5` | — | — | — | — | — |
| `headfree` | G | 66666666-6666-6666-6666-666666666666 | 66666666-6666-6666-6666-666666666666 | 77777777-7777-7777-7777-777777777777 | 77777777-7777-7777-7777-777777777777 |

The generic BLE OTA helper also contains service/write/read sets `7033 / 2001 / 2002`, `ff12 / ff15 / ff14`, and `65786365-6c70-6f69-6e74-2e636f6d0000 / 65786365-6c70-6f69-6e74-2e636f6d0001 / 65786365-6c70-6f69-6e74-2e636f6d0001`. Selection depends on an OTA enum; no unverified model mapping is implied. AURAMIC also calls the USB HID OTA helper and sets `otaForwardType=abDongleGuid`; a complete HID report layout is not established here.

## 3. RS Common, including OpenFun

### 3.1 Transmit format (S)

```text
FF 00 LEN BODY AA
LEN = len(BODY)
```

The fixed commands have no extra V2 checksum byte. Do not assume that `00` is an incrementing V2-style sequence number. Typical settings use `TYPE VALUE` as the Body; bulk queries use `FA ITEM...`. Use the exact recovered arrays in section 8. Shortened single-item queries inferred from the pattern are not hardware-verified commands.

### 3.2 Minimal OpenFun reading sequence (H)

1. Connect to the above RFCOMM service on `ROSE OpenFun`.
2. Start receiving continuously; battery notifications can also arrive unsolicited.
3. Send the following complete getInfo request, which includes battery item `0C`.
4. Buffer and reassemble the input stream, then process type `0C`.

```text
TX: FF 00 0F FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 33 AA
RX: DD 00 04 0C 40 3D 00 AA
```

The real response represents **64% left**, **61% right**, and **0 in the case field**. Preserve that zero. It does not establish that the physical case is empty; freshness and product-specific meaning require device context.

| Offset | Field | Meaning |
| --- | --- | --- |
| 0 | `DD` | Actual OpenFun receive header |
| 1 | `00` | Observed value; general semantics unconfirmed |
| 2 | `04` | Number of following TYPE+VALUE bytes |
| 3 | `0C` | Battery type |
| 4 | Left | Low 7 bits: percentage; high bit: charging |
| 5 | Right | Low 7 bits: percentage; high bit: charging |
| 6 | Case field | Parsed as the entire byte by the application |
| 7 | `AA` | Terminator |

For this verified format, total length is `frame[2]+4`. Splitting on the first `AA` can cut a payload containing that byte; determine the length first and then verify the terminator. Receive headers on other models have not been confirmed individually.

### 3.3 Receive message types (S)

`RsCommonReceiver.process` dispatches on full-frame byte[3]. Meanings below come from receive handlers. Multi-byte Values still require the corresponding field decoder.

| Type | Meaning / handler | ELF address |
| --- | --- | --- |
| `01` | Key data | `0xf76180` |
| `07` | Touch Control | `0xf777ec` |
| `08` | In Ear Detection | `0xf776f4` |
| `09` | ANCdata | `0xf759f0` |
| `0C` | Battery data | `0xf772a0` |
| `0D` | Firmware Version | `0xf77070` |
| `0E` | Game Mode | `0xf76f78` |
| `2A` | EQdata | `0xf76324` |
| `2B` | Connection Mode | `0xf75ae0` |
| `2C` | ANCLevel | `0xf75900` |
| `2D` | Transparency Level | `0xf75810` |
| `2E` | Prompt Tone Level | `0xf75724` |
| `33` | Spatial Audio | `0xf76e80` |


## 4. F8

F8 transmit bytes differ from both RS families. Most recovered fixed frames resemble `00 CMD 01 00 LEN DATA...`, with LEN matching the following DATA length. This describes the recovered commands, not a universal rule verified across firmware versions. Examples:

```text
getBattery: 00 27 01 00 01 01
getVersion: 00 27 01 00 01 02
getEq:      00 27 01 00 01 04
getKey:     02 27 01 00 01 05
setEqHifi:  00 20 01 00 02 00 00
```

The first byte of getKey really is `02`; do not normalize it to `00`. These recovered fixed arrays do not append a checksum. The receiver first checks full-frame byte[1] for `27` or `28`, then dispatches using byte[5]. Battery fields are byte[7], [8], and [9]. Receive envelope boundaries, complete length handling, and fragmentation behavior across firmware versions remain unverified on hardware (P).

Explorer has an `ExplorerSender.setNoiseModeNormal` override that delegates to `F8Sender.setNoiseModeWind`, producing `00 2C 01 00 01 00`. Do not replace it with the ordinary F8 Normal constant. F8 EQ reception also has a special `0404` branch; `04` below is the main subtype and the special condition must be retained separately.

| Type | Meaning / handler | ELF address |
| --- | --- | --- |
| `01` | Battery data | `0xf7653c` |
| `02` | Firmware Version | `0xf76414` |
| `04 (+0404)` | EQdata | `0xf76324` |
| `05` | Key data | `0xf75ebc` |
| `08` | Game Mode | `0xf75dc4` |
| `09` | In Ear Detection | `0xf75ccc` |
| `0A` | Ear Language | `0xf75bcc` |
| `0B` | Connection Mode | `0xf75ae0` |
| `0C` | ANCdata | `0xf759f0` |
| `11` | ANCLevel | `0xf75900` |
| `12` | Transparency Level | `0xf75810` |
| `13` | Prompt Tone Level | `0xf75724` |
| `16` | Touch Control | `0xf7562c` |


## 5. RS Common V2

### 5.1 Transmit envelope (S)

The sender returns a **Body**, and transform adds its envelope. V2 entries in section 8 must not be transmitted as complete Bluetooth packets without wrapping them.

```text
FF SEQ LEN BODY CS AA
LEN = len(BODY)
CS  = sum(FF, SEQ, LEN, BODY...) & 0xFF
```

SEQ increments modulo 256. LEN is one byte. An implementation should reject an ordinary Body too large for that field rather than silently truncate it. The recovered bulk getInfo Body is:

```text
FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 2F 31 32 33 36 37 38 39 3A 3B 3C 3D 3F 45 46 49
```

Complete packet for SEQ=0, calculated from the application's algorithm rather than captured on hardware:

```text
FF 00 1E FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 2F 31 32 33 36 37 38 39 3A 3B 3C 3D 3F 45 46 49 E9 AA
```

### 5.2 Receive LTVs, checksum, and ACK (S)

The receiver accepts `DD` or `FF` headers and uses a separate streaming FrameDecoder for reassembly and resynchronization. The ordinary receive layout is:

```text
HEADER SEQ [L TYPE VALUE...] [L TYPE VALUE...] ... CS AA
L = 1 + len(VALUE)             # Includes TYPE, excludes L itself
next_offset = offset + 1 + L
```

A receive frame can contain several LTVs. The first L is not necessarily the only length for the whole frame. The primary checksum path sums the header, SEQ, and all LTV bytes modulo 256. `CommandUtil._isValidPacket` also accepts a legacy form that sums from byte[2]. Failure recovery does not imply that checksum verification is optional.

The special L=00 handling is limited to Starburst types `41/42`, using dedicated terminator search and validation. Ordinary types such as battery do not acquire an unlimited payload from a zero length. The decoder's limit is 8192 hexadecimal characters, approximately 4096 bytes, not 8192 bytes.

TYPE=FE enters the ACK handler and requires a two-byte Value. The application logs received SEQ and a status code. All status meanings and retry timing have not been established; an arbitrary ACK is not proof of success.

Type `49` is overloaded by Value length: two bytes select independent microphone mode, while four or six bytes select extended firmware version handling. Other lengths are logged as anomalies. `E0–E4` and `40` enter the Opus extension handler; `42` enters the Starburst SDK handler. Their internal payloads have not been fully decoded.

### 5.3 Receive message types (S)

These are TYPE values within LTVs. Battery Value offsets start at the Value itself. The microphone extensions `50–7C` are included; identifying a handler does not establish every internal field.

| Type | Meaning / handler | ELF address |
| --- | --- | --- |
| `2A` | EQdata | `0xf853bc` |
| `09` | ANCdata | `0xf85194` |
| `0C` | Battery data | `0xf84ccc` |
| `01` | Key data | `0xf84b78` |
| `07` | Touch Control | `0xf849c0` |
| `08` | In Ear Detection | `0xf84808` |
| `0D` | Firmware Version | `0xf84524` |
| `0E` | Game Mode | `0xf8436c` |
| `2B` | Connection Mode | `0xf75ae0` |
| `32` | Multi Device | `0xf841b4` |
| `2C` | ANCLevel | `0xf75900` |
| `2D` | Transparency Level | `0xf75810` |
| `2E` | Prompt Tone Level | `0xf75724` |
| `33` | Spatial Audio | `0xf83ffc` |
| `34` | Ear Mac | `0xf83c8c` |
| `35` | Time Sync | `0xf83bf8` |
| `36` | Anc List | `0xf838c0` |
| `2F` | Find earbuds | `0xf837f0` |
| `31` | Ear Lang | `0xf83704` |
| `37` | Gyroscope Mode | `0xf83638` |
| `38` | Spatial Audio Mode | `0xf8346c` |
| `39` | Head Movement Mode | `0xf832b4` |
| `3A` | Ear Adaptive Mode | `0xf830fc` |
| `3B` | Tap Ctl Mode | `0xf82f44` |
| `3C` | Tap Ctl Key data | `0xf82ab0` |
| `3D` | Spatial Audio Music Mode | `0xf828d0` |
| `3E` | Custom EQList | `0xf8272c` |
| `3F` | Dongle Firmware Version | `0xf82448` |
| `45` | Gain Level | `0xf82260` |
| `46` | Hearing Protection | `0xf82078` |
| `47` | Ear Tip Fit Test | `0xf81ec0` |
| `48` | Ai Command | `0xf81b3c` |
| `49` | Independent mic mode / extended firmware, by length | `0xf816f8` |
| `49` | Independent mic mode / extended firmware, by length | `0xf81410` |
| `4A` | Group Mode | `0xf81258` |
| `50` | Microphone Basic Info | `0xf806dc` |
| `51` | Microphone Battery | `0xf8002c` |
| `52` | Microphone Meter | `0xf7fe1c` |
| `53` | Microphone Tx Status | `0xf7f318` |
| `54` | Microphone Tx Noise | `0xf7eff8` |
| `55` | Microphone Gain | `0xf7ec98` |
| `56` | Microphone Record Format | `0xf7e8b8` |
| `57` | Microphone Low Cut | `0xf7e600` |
| `58` | Microphone Tone | `0xf7e178` |
| `59` | Microphone Led | `0xf7def0` |
| `5A` | Microphone Auto Power Off | `0xf7dbac` |
| `5B` | Microphone Mute | `0xf7d924` |
| `5C` | Microphone Auto Record | `0xf7d69c` |
| `5D` | Microphone Rx Channel | `0xf7d47c` |
| `5E` | Microphone Camera Power | `0xf7d30c` |
| `5F` | Microphone Gain Profile | `0xf7cdac` |
| `60` | Microphone Phone Speaker | `0xf7cc3c` |
| `61` | Microphone Ble Power Mode | `0xf7ca38` |
| `62` | Microphone Monitor Tone | `0xf7c860` |
| `63` | Microphone Timecode | `0xf7bce4` |
| `72` | Microphone Remote Target | `0xf7b868` |
| `77` | Microphone Record Control | `0xf7b1ac` |
| `78` | Microphone Tx Mac | `0xf7a984` |
| `79` | Microphone Low Cut Frequency | `0xf7a5dc` |
| `7A` | Microphone Tf Card Time | `0xf7a164` |
| `7B` | Microphone Loop Record | `0xf79800` |
| `7C` | Microphone Device Log | `0xf79584` |
| `F0` | Master Slave Switch | `0xf79474` |
| `40` | Opus Command | `0xf79354` |
| `42` | Starburst Sdk Command | `0xf790c0` |
| `FE` | Command ACK, two-byte Value | `0xf85a50` |
| `E0–E4 / 40` | Opus extension | `0xf79354` |


## 6. ZT and sender variants

### 6.1 Common complete frame (S)

```text
08 EE 00 00 00 GROUP OP LEN_LO LEN_HI DATA... CS
CS = sum(all frame bytes except CS) & 0xFF
```

In the ordinary frames reviewed, byte[7:9] is the little-endian total length including the checksum. Queries without DATA are generally ten bytes long. The general meaning of the reserved header bytes remains unconfirmed. There is no RS-style AA terminator, and no V2 envelope should be added.

```text
getInfo:    08 EE 00 00 00 01 01 0A 00 02
getBattery: 08 EE 00 00 00 01 03 0A 00 04   # Zt231Sender
```

In addition to ZtSender, Zt219Sender, Zt229Sender, and Zt231Sender override selected commands. The three model profiles are confirmed as ZT; the complete constructor binding from numeric sender suffix to model has not been verified in this document. Sender classes therefore remain separate. Do not mix all variant commands on an arbitrary ZT device.

One anomalous constant is preserved: Zt219Sender.getHideSearch contains **nine bytes**, `08 EE 00 00 0B 01 0A 00 0C`, differing from the ordinary header/length pattern. No missing 00 is silently inserted, and hardware usability is not claimed.

### 6.2 Receive messages (S/P)

ZTReceiver.process uses full-frame byte[5:7] as two consecutive message bytes, e.g. `01 01`, not an implicitly little-endian integer. The table lists unambiguous primary dispatches only. Level branches such as `0607`/`060C` and prompt-tone branches include additional length/context conditions and are not inferred from adjacent string constants.

| Type | Meaning / handler | ELF address |
| --- | --- | --- |
| `0101` | Battery data | `0xf8c230` |
| `0103` | Headfree Battery data | `0xf8c0d8` |
| `0D01` | Firmware Version | `0xf8bea8` |
| `0201` | EQdata | `0xf8bdb0` |
| `0202` | Custom Eq data | `0xf8bcd0` |
| `0203` | Gain Level | `0xf8bbb4` |
| `0401` | Key data | `0xf8b8e8` |
| `0603` | Game Mode | `0xf8b7f0` |
| `0C01` | In Ear Detection | `0xf8b6f8` |
| `0801` | Connection Mode | `0xf8b604` |
| `0602` | ANCdata | `0xf8b400` |
| `0604` | Transparency Level | `0xf8b220` |
| `0E03` | Prompt Tone Level | `0xf8b110` |
| `0A01` | Touch Control | `0xf8af0c` |
| `0E02` | Find earbuds | `0xf8ae3c` |
| `0605` | Anc List | `0xf8ab04` |
| `0802` | Multi Device | `0xf8aa0c` |
| `0803` | Multi Device Name List | `0xf8a7f8` |
| `0B01` | Hide Search | `0xf8a700` |


## 7. Battery decoding comparison

| Family | Input and minimum field coverage | Left | Right | Case / single battery | Charging |
| --- | --- | --- | --- | --- | --- |
| RS Common | Full frame, byte[3]=0C; must cover byte[6] | [4]&7F | [5]&7F | Entire [6] | Left/right bit7; no case flag parsed |
| F8 | Full frame, byte[1]=27/28, byte[5]=01; must cover byte[9] | [7]&7F | [8]&7F | Entire [9] | Left/right bit7; no case flag parsed |
| V2 0C | Extracted LTV Value, three bytes | V[0]&7F | V[1]&7F | V[2]&7F | Bit7 on all three |
| ZT 0101 | Full frame; must cover byte[15] | [11] | [12] | [15] | [13] and [14] nonzero for left/right |
| ZT 0103 | Full frame; must cover byte[9] | — | — | Single battery at [9] | Application sets left/right charging false |
| V2 MIC 51 | Extracted LTV Value, exactly four bytes | Four slots; not a TWS layout | — | Low7 per slot | Bit7 per slot; FF means unavailable |

ZT 0103 stores its single battery in RBatteryData.batteryCase; that does not make it a physical charging case. For MIC 51, any non-FF slot whose low7 exceeds 100 causes the application to discard the entire Value. The hardware meaning of the four slots has not been confirmed, so they are not labeled left/right/case.

Ordinary earbud branches do not share the MIC-specific FF handling. Preserve unknown and out-of-range raw values; a client may display them as unavailable instead of clamping to 100%. Distinguish fresh data, cached data, and offline devices. A case reading may be cached. Preserve 0% as a candidate valid measurement rather than automatically declaring the device offline.

The following standalone offline examples accept already reassembled, length-checked input. They do not connect to Bluetooth. The three parsing entry points are deliberately separate:
```python
def rs_common_battery(frame: bytes):
    if (len(frame) < 8 or frame[0] != 0xDD or frame[3] != 0x0C
            or len(frame) != frame[2] + 4 or frame[-1] != 0xAA):
        raise ValueError("Not an OpenFun battery frame")
    left, right, case = frame[4:7]
    return dict(left=left & 0x7F, right=right & 0x7F, case=case,
                left_charging=bool(left & 0x80),
                right_charging=bool(right & 0x80))

def v2_battery_value(value: bytes):
    if len(value) != 3:
        raise ValueError("Expected three-byte battery Value")
    return [(b & 0x7F, bool(b & 0x80)) for b in value]

def v2_mic_battery_value(value: bytes):
    if len(value) != 4:
        raise ValueError("Expected four battery slots")
    if any(b != 0xFF and (b & 0x7F) > 100 for b in value):
        raise ValueError("Invalid microphone battery Value")
    return [None if b == 0xFF else (b & 0x7F, bool(b & 0x80))
            for b in value]

def wrap_v2(body: bytes, seq: int) -> bytes:
    if not 0 <= seq <= 255 or not 1 <= len(body) <= 255:
        raise ValueError("SEQ or Body length out of range")
    prefix = bytes([0xFF, seq, len(body)]) + body
    return prefix + bytes([sum(prefix) & 0xFF, 0xAA])

assert rs_common_battery(bytes.fromhex("DD 00 04 0C 40 3D 00 AA"))["left"] == 64
assert wrap_v2(bytes.fromhex("FA 45"), 0).hex(" ") == "ff 00 02 fa 45 40 aa"
```

## 8. Complete fixed command catalogue

This catalogue contains **166 constant-array command entries** recovered from all named sender methods. Entries are not deduplicated across families. Addresses are ELF code addresses in this exact binary. `get:` is removed from labels for readability, but set-prefixed getters still perform writes. F8, RS Common, and ZT columns are complete arrays; V2 columns are Bodies requiring the envelope in section 5. Aliases and parameterized builders appear in section 9.

### F8Sender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getAnc` | `00 27 01 00 01 0C` | `0xac8e28` |
| `getAncLevel` | `00 27 01 00 01 11` | `0xac85b8` |
| `getAncTransLevel` | `00 27 01 00 01 12` | `0xad7bb0` |
| `getAudioLdac` | `00 27 01 00 01 0B` | `0x8a56d4` |
| `getBattery` | `00 27 01 00 01 01` | `0x82141c` |
| `getDingLevel` | `00 27 01 00 01 13` | `0x8ff7f0` |
| `getEarDetection` | `00 27 01 00 01 09` | `0x7f525c` |
| `getEarLanguage` | `00 27 01 00 01 0A` | `0x7f509c` |
| `getEq` | `00 27 01 00 01 04` | `0xf5fec8` |
| `getGameMode` | `00 27 01 00 01 08` | `0x91231c` |
| `getKey` | `02 27 01 00 01 05` | `0xf74b64` |
| `getTouch` | `00 27 01 00 01 16` | `0xf8da70` |
| `getVersion` | `00 27 01 00 01 02` | `0xbf0204` |
| `setAudioAacSbc` | `00 2B 01 00 01 02` | `0x8a2180` |
| `setAudioLdac` | `00 2B 01 00 01 01` | `0x824a04` |
| `setAudioLhdc` | `00 2B 01 00 01 03` | `0x82151c` |
| `setConnectMulOn` | `00 2B 01 00 01 00` | `0xfb3c24` |
| `setEarDetectionOff` | `00 26 01 00 01 00` | `0x906570` |
| `setEarDetectionOn` | `00 26 01 00 01 01` | `0x7f636c` |
| `setEarLanguageChinese` | `00 29 01 00 01 00` | `0x7f6894` |
| `setEarLanguageEnglish` | `00 29 01 00 01 01` | `0x815ae0` |
| `setEqHifi` | `00 20 01 00 02 00 00` | `0xe7cd5c` |
| `setEqPop` | `00 20 01 00 02 00 01` | `0xf5fcbc` |
| `setEqRock` | `00 20 01 00 02 00 02` | `0xbe7878` |
| `setGameModeOff` | `00 25 01 00 01 00` | `0x907630` |
| `setGameModeOn` | `00 25 01 00 01 01` | `0x90b68c` |
| `setNoiseModeAnc` | `00 2C 01 00 01 01` | `0x8dfbd0` |
| `setNoiseModeNormal` | `00 2C 01 00 01 03` | `0x8e6eec` |
| `setNoiseModeTrans` | `00 2C 01 00 01 02` | `0x8f2390` |
| `setNoiseModeWind` | `00 2C 01 00 01 00` | `0x8e4cb0` |
| `setTouchOff` | `00 33 01 00 01 00` | `0x815918` |
| `setTouchOn` | `00 33 01 00 01 01` | `0x7fc1a8` |


### RsCommonSender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getEarLanguage` | `FF 00 02 FA 31 AA` | `0x7f510c` |
| `getInfo` | `FF 00 0F FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 33 AA` | `0xb4e700` |
| `setAudioAacSbc` | `FF 00 02 2B 00 AA` | `0x8a21f0` |
| `setAudioLc3` | `FF 00 02 2B 03 AA` | `0xbe670c` |
| `setAudioLdac` | `FF 00 02 2B 01 AA` | `0x824a74` |
| `setAudioLhdc` | `FF 00 02 2B 02 AA` | `0x82158c` |
| `setConnectMulOn` | `FF 00 02 2B 04 AA` | `0xfb3cb0` |
| `setEarDetectionOff` | `FF 00 02 08 01 AA` | `0x9065e0` |
| `setEarDetectionOn` | `FF 00 02 08 00 AA` | `0x7f63dc` |
| `setEarLanguageChinese` | `FF 00 02 2F 00 AA` | `0x7f6904` |
| `setEarLanguageEnglish` | `FF 00 02 2F 01 AA` | `0x815b50` |
| `setEqHifi` | `FF 00 02 2A 00 AA` | `0xe7cdd4` |
| `setEqPop` | `FF 00 02 2A 01 AA` | `0xf5fd34` |
| `setEqRock` | `FF 00 02 2A 02 AA` | `0xbe78f0` |
| `setGameModeOff` | `FF 00 02 0E 00 AA` | `0x9076a0` |
| `setGameModeOn` | `FF 00 02 0E 01 AA` | `0x90b6fc` |
| `setNoiseModeAnc` | `FF 00 02 09 01 AA` | `0x8dfc40` |
| `setNoiseModeNormal` | `FF 00 02 09 02 AA` | `0x8e6f5c` |
| `setNoiseModeTrans` | `FF 00 02 09 03 AA` | `0x8f2400` |
| `setNoiseModeWind` | `FF 00 02 09 04 AA` | `0x8e4d20` |
| `setSpatialAudioSwitchOff` | `FF 00 02 33 00 AA` | `0x94a8ac` |
| `setSpatialAudioSwitchOn` | `FF 00 02 33 01 AA` | `0x9805dc` |
| `setTouchOff` | `FF 00 02 07 01 AA` | `0x815988` |
| `setTouchOn` | `FF 00 02 07 00 AA` | `0x7fc218` |


### RsCommonV2Sender

| Command | Body | ELF |
| --- | --- | --- |
| `getCustomEq` | `FA 3E` | `0x986244` |
| `getEarLanguage` | `FA 31` | `0x7f517c` |
| `getEarMac` | `FE 34` | `0x8e6e30` |
| `getEq` | `FA 2A` | `0xf5ff38` |
| `getGainLevel` | `FA 45` | `0x7f8a84` |
| `getInfo` | `FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 2F 31 32 33 36 37 38 39 3A 3B 3C 3D 3F 45 46 49` | `0xb4e7e8` |
| `setAncAutoDynamicOn` | `09 07` | `0xe7b680` |
| `setAncAutoOn` | `09 05` | `0xbf0688` |
| `setAudioAacSbc` | `2B 00` | `0x8a2260` |
| `setAudioAptx` | `2B 04` | `0x8213cc` |
| `setAudioAptxAdaptive` | `2B 06` | `0x815e7c` |
| `setAudioAptxHd` | `2B 05` | `0x82137c` |
| `setAudioLc3` | `2B 03` | `0xbe677c` |
| `setAudioLdac` | `2B 01` | `0x824ae4` |
| `setAudioLhdc` | `2B 02` | `0x8215fc` |
| `setConnectMulOff` | `32 00` | `0xf5afe4` |
| `setConnectMulOn` | `32 01` | `0xfb3d20` |
| `setDeepAncOn` | `09 06` | `0xe7d3c8` |
| `setEarAdaptiveOff` | `3A 00` | `0x806ca8` |
| `setEarAdaptiveOn` | `3A 01` | `0x806c58` |
| `setEarDetectionOff` | `08 01` | `0x906650` |
| `setEarDetectionOn` | `08 00` | `0x7f644c` |
| `setEarLanguageChinese` | `31 00` | `0x7f6974` |
| `setEarLanguageEnglish` | `31 01` | `0x815bc0` |
| `setEarTipFitTest` | `FA 47` | `0x7fc548` |
| `setEqCustom` | `2A 04` | `0x988c50` |
| `setEqEap` | `2A 06` | `0xafb810` |
| `setEqGame1` | `2A A0` | `0xaed790` |
| `setEqGame2` | `2A A1` | `0x9eb9c8` |
| `setEqGame3` | `2A A2` | `0x9aae44` |
| `setEqGame4` | `2A A3` | `0x9a03e8` |
| `setEqGame5` | `2A A4` | `0x98ebb8` |
| `setEqHifi` | `2A 00` | `0xe7ce44` |
| `setEqLight` | `2A 03` | `0xb44228` |
| `setEqPop` | `2A 01` | `0xf5fda4` |
| `setEqRock` | `2A 02` | `0xbe7960` |
| `setFindOff` | `2F 04` | `0x824d74` |
| `setGameModeOff` | `0E 00` | `0x907710` |
| `setGameModeOn` | `0E 01` | `0x90b76c` |
| `setHeadMoveModeOff` | `39 00` | `0x9ed418` |
| `setHeadMoveModeOn` | `39 01` | `0x8f7f0c` |
| `setIndependentMicModeActivateOff` | `49 00 00` | `0x8f7fa4` |
| `setIndependentMicModeActivateOn` | `49 00 01` | `0x8fb4f8` |
| `setIndependentMicModeOff` | `49 01 00` | `0x8fd2e8` |
| `setIndependentMicModeOn` | `49 01 01` | `0x906820` |
| `setNoiseModeAnc` | `09 01` | `0x8dfcb0` |
| `setNoiseModeNormal` | `09 02` | `0x8e6fcc` |
| `setNoiseModeTrans` | `09 03` | `0x8f2470` |
| `setNoiseModeWind` | `09 04` | `0x8e4d90` |
| `setSpatialAudioModeFixed` | `38 02` | `0x929518` |
| `setSpatialAudioModeHead` | `38 01` | `0x946078` |
| `setSpatialAudioModeOff` | `38 00` | `0x924ecc` |
| `setSpatialAudioMusicModeGame` | `3D 03` | `0x91307c` |
| `setSpatialAudioMusicModeMovie` | `3D 02` | `0x91d02c` |
| `setSpatialAudioMusicModeMusic` | `3D 01` | `0x91e768` |
| `setSpatialAudioMusicModeTV` | `3D 00` | `0x920db8` |
| `setSpatialAudioSwitchOff` | `33 00` | `0x94a91c` |
| `setSpatialAudioSwitchOn` | `33 01` | `0x98064c` |
| `setTapOff` | `3B 00` | `0x824864` |
| `setTapOn` | `3B 01` | `0x912f90` |
| `setTouchOff` | `07 01` | `0x8159f8` |
| `setTouchOn` | `07 00` | `0x7fc288` |


### ZtSender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getAnc` | `08 EE 00 00 00 06 02 0A 00 08` | `0xac8e98` |
| `getAncList` | `08 EE 00 00 00 06 05 0A 00 0B` | `0x823eb4` |
| `getAncTransLevel` | `08 EE 00 00 00 06 07 0A 00 0D` | `0xad7cb0` |
| `getDingLevel` | `08 EE 00 00 00 0E 06 0A 00 14` | `0x8ff8f0` |
| `getEarDetection` | `08 EE 00 00 00 0C 01 0A 00 0D` | `0x7f52cc` |
| `getEq` | `08 EE 00 00 00 02 01 0A 00 03` | `0xf5ff88` |
| `getGameMode` | `08 EE 00 00 00 06 03 0A 00 09` | `0x91238c` |
| `getInfo` | `08 EE 00 00 00 01 01 0A 00 02` | `0xb4e948` |
| `getKey` | `08 EE 00 00 00 04 01 0A 00 05` | `0xf74bd4` |
| `getTouch` | `08 EE 00 00 00 0A 01 0A 00 0B` | `0xf8dae8` |
| `getVersion` | `08 EE 00 00 00 0D 01 0A 00 0E` | `0xbf0274` |
| `setAncAutoOn` | `08 EE 00 00 00 06 82 0E 00 02 00 00 00 8E` | `0xbf06d8` |
| `setAudioMulDevice` | `08 EE 00 00 00 08 82 0B 00 01 8C` | `0x815d14` |
| `setConnectMulOn` | `08 EE 00 00 00 08 81 0B 00 00 8A` | `0xfb3d70` |
| `setEarDetectionOff` | `08 EE 00 00 00 0C 81 0B 00 01 8F` | `0x9066a0` |
| `setEarDetectionOn` | `08 EE 00 00 00 0C 81 0B 00 00 8E` | `0x7f649c` |
| `setEqHifi` | `08 EE 00 00 00 02 81 0B 00 01 85` | `0xe7ce94` |
| `setEqLight` | `08 EE 00 00 00 02 81 0B 00 03 87` | `0xb44278` |
| `setEqPop` | `08 EE 00 00 00 02 81 0B 00 00 84` | `0xf5fdf4` |
| `setEqRock` | `08 EE 00 00 00 02 81 0B 00 02 86` | `0xbe79b0` |
| `setGameModeOff` | `08 EE 00 00 00 06 83 0B 00 01 8B` | `0x907760` |
| `setGameModeOn` | `08 EE 00 00 00 06 83 0B 00 00 8A` | `0x90b7bc` |
| `setNoiseModeAnc` | `08 EE 00 00 00 06 82 0E 00 01 00 00 00 8D` | `0x8dfda8` |
| `setNoiseModeNormal` | `08 EE 00 00 00 06 82 0E 00 00 01 00 00 8D` | `0x8e70c4` |
| `setNoiseModeTrans` | `08 EE 00 00 00 06 82 0E 00 00 00 00 01 8D` | `0x8f2568` |
| `setNoiseModeWind` | `08 EE 00 00 00 06 82 0E 00 00 00 01 00 8D` | `0x8e4e04` |
| `setTouchOff` | `08 EE 00 00 00 0A 81 0B 00 01 8D` | `0x815a48` |
| `setTouchOn` | `08 EE 00 00 00 0A 81 0B 00 00 8C` | `0x7fc2d8` |


### Zt219Sender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getHideSearch` | `08 EE 00 00 0B 01 0A 00 0C` | `0xb4a144` |
| `setHideSearchOff` | `08 EE 00 00 00 0B 81 0B 00 00 8D` | `0x8f2648` |
| `setHideSearchOn` | `08 EE 00 00 00 0B 81 0B 00 01 8E` | `0x7f954c` |
| `setNoiseModeAnc` | `08 EE 00 00 00 06 82 0D 00 01 00 00 8C` | `0x8dfd00` |
| `setNoiseModeNormal` | `08 EE 00 00 00 06 82 0D 00 00 00 01 8C` | `0x8e701c` |
| `setNoiseModeTrans` | `08 EE 00 00 00 06 82 0D 00 00 01 00 8C` | `0x8f24c0` |


### Zt229Sender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getAncTransLevel` | `08 EE 00 00 00 06 04 0A 00 0A` | `0xad7c20` |
| `getDingLevel` | `08 EE 00 00 00 0E 03 0A 00 11` | `0x8ff860` |


### Zt231Sender

| Command | Complete array | ELF |
| --- | --- | --- |
| `getBattery` | `08 EE 00 00 00 01 03 0A 00 04` | `0x82148c` |
| `getConnectMul` | `08 EE 00 00 00 08 02 0A 00 0A` | `0x8f4490` |
| `getEarLanguage` | `08 EE 00 00 00 0D 02 0A 00 0F` | `0x7f51cc` |
| `getLoseLoss` | `08 EE 00 00 00 08 01 0A 00 09` | `0xbe08dc` |
| `getMulDeviceNameList` | `08 EE 00 00 00 08 03 0A 00 0B` | `0x8df3bc` |
| `setAudioLdac` | `08 EE 00 00 00 08 81 0B 00 01 8B` | `0x824b34` |
| `setAudioLdacLhdc` | `08 EE 00 00 00 08 81 0B 00 03 8D` | `0x7fc598` |
| `setAudioLhdc` | `08 EE 00 00 00 08 81 0B 00 02 8C` | `0x82164c` |
| `setConnectMulOff` | `08 EE 00 00 00 08 82 0B 00 00 8B` | `0xf5b058` |
| `setConnectMulOn` | `08 EE 00 00 00 08 82 0B 00 01 8C` | `0xfb3e08` |
| `setEarLanguageChinese` | `08 EE 00 00 00 0D 81 0B 00 00 8F` | `0x7f69c4` |
| `setEarLanguageEnglish` | `08 EE 00 00 00 0D 81 0B 00 01 90` | `0x815c10` |


## 9. Parameterized commands and aliases

### 9.1 Decoded parameter templates (S)

The following templates are decoded from construction logic, not hardware captures. NN is a full byte. Do not literally send the letter N. Parameter limits below are those enforced by the corresponding sender, not universal device limits.

| Family | Method | Template | n range |
| --- | --- | --- | --- |
| F8 | `setDingLevel` | `00 32 01 00 01 NN` | — |
| F8 | `setAncLevel` | `00 30 01 00 01 NN` | — |
| F8 | `setTransLevel` | `00 31 01 00 01 NN` | — |
| F8 | `setFindDevice` | `00 2A 01 00 01 NN` | — |
| RS Common | `setDingLevel` | `FF 00 02 2E NN AA` | 1..5 |
| RS Common | `setAncLevel` | `FF 00 02 2C NN AA` | 1..5 |
| RS Common | `setTransLevel` | `FF 00 02 2D NN AA` | 1..5 |
| RS Common | `setFindDevice` | `FF 00 02 2F NN AA` | 1..4 |
| V2 Body | `setDingLevel` | `2E NN` | 0..5 |
| V2 Body | `setAncLevel` | `2C NN` | 1..5 |
| V2 Body | `setTransLevel` | `2D NN` | 1..5 |
| V2 Body | `setFindDevice` | `2F NN` | 1..4 |
| V2 Body | `setGainLevel` | `45 NN` | 0..2 |
| V2 Body | `setHearingProtection` | `46 NN` | 0..6 |
| V2 Body | `setEqBassPlus` | `2A 05 NN` | 0..4 |

Find-device wrappers pass 1=left, 2=right, and 3=both. Off is 4 in RS/V2 but 0 in F8. F8 has no matching explicit range guard in the inspected generic builder. For F8 ANC/transparency strength, NN is the remapped value: input 1→2, 3→1, 5→0; other inputs pass through. It is not a direct copy of n.

### 9.2 Touch mapping (S)

| Method | F8 complete | RS Common complete | V2 Body |
| --- | --- | --- | --- |
| `setLeftTouchOne` | `00 22 01 00 03 01 01 NN` | `FF 00 03 01 01 NN AA` | `01 01 NN` |
| `setLeftTouchTwo` | `00 22 01 00 03 03 01 NN` | `FF 00 03 01 02 NN AA` | `01 02 NN` |
| `setLeftTouchThree` | `00 22 01 00 03 05 01 NN` | `FF 00 03 01 03 NN AA` | `01 03 NN` |
| `setLeftTouchLong` | `00 22 01 00 03 07 01 NN` | `FF 00 03 01 04 NN AA` | `01 04 NN` |
| `setRightTouchOne` | `00 22 01 00 03 02 01 NN` | `FF 00 03 01 11 NN AA` | `01 11 NN` |
| `setRightTouchTwo` | `00 22 01 00 03 04 01 NN` | `FF 00 03 01 12 NN AA` | `01 12 NN` |
| `setRightTouchThree` | `00 22 01 00 03 06 01 NN` | `FF 00 03 01 13 NN AA` | `01 13 NN` |
| `setRightTouchLong` | `00 22 01 00 03 08 01 NN` | `FF 00 03 01 14 NN AA` | `01 14 NN` |

The older F8/RS methods interpolate the decimal action number after a literal `0` in a hex string, e.g. n=5 becomes 05. Multi-digit numbers must not be assumed equivalent to a generic hex-byte encoder. The action-number semantics have not been fully mapped. V2 writes the action as a byte through _setTouchKey. Tap mappings use a separate _setTapKey builder and are not interchangeable with touch mappings.

V2 tap Bodies are `3C KEY NN`: KEY=02 (left double), 03 (left triple), 12 (right double), 13 (right triple). Their action enums still require confirmation.

### 9.3 ZT parameterized construction (S/P)

The common prompt-tone template is `08 EE 00 00 00 0E 03 0B 00 NN CS`, with n=1..8 in ZtSender and n=1..5 in Zt231Sender. Zt229Sender uses OP=83. CS sums the complete preceding bytes modulo 256. ZT touch builders begin `08 EE 00 00 00 04 81 0D 00 SIDE GESTURE NN CS`, where SIDE is 00/01 and GESTURE is 00..03; verify variant binding and action semantics before implementation.

Zt231Sender._buildEqCommand builds `08 EE 00 00 00 02 OP TOTAL_LEN 00 DATA... CS`, where TOTAL_LEN=10+len(DATA). getCustomEq uses OP=02, getGainLevel uses OP=03, setEqCustom uses OP=81 with DATA=03, and setGainLevel(n) uses OP=83 with DATA=n+1 for n=0..2. Custom EQ writing uses OP=82 and requires ten input bands. Gain encoding, per-band byte conversion, frequency order, and device limits require further confirmation.
### 9.4 Remaining method inventory

Together with section 8, this inventory accounts for **all 274 named sender methods** recovered in this analysis, including helpers, aliases, and empty entries. “Delegates” records a call target and does not claim identical bytes when parameters differ. “String fragments” are evidence only, not complete packets. “Parameterized/helper” means that a complete general encoder has not been established here, except where section 9.1–9.3 provides a reviewed template.

| Method | Recovered behavior / boundary | ELF |
| --- | --- | --- |
| `ExplorerSender.setNoiseModeNormal` | Delegates: `F8Sender.setNoiseModeWind` | `0x8e6ec8` |
| `F8Sender.getConnectMul` | Delegates: `F8Sender.getAudioLdac` | `0x8f4448` |
| `F8Sender.setAncLevel` | Parameterized / helper logic; string fragments: `00 30 01 00 01 ` | `0xad0c68` |
| `F8Sender.setAudioMulDevice` | Delegates: `F8Sender.setConnectMulOn` | `0x815ca8` |
| `F8Sender.setConnectMulOff` | Delegates: `F8Sender.setAudioAacSbc` | `0xf5af9c` |
| `F8Sender.setDingLevel` | Parameterized / helper logic; string fragments: `00 32 01 00 01 0` | `0x7f8578` |
| `F8Sender.setFindDevice` | Parameterized / helper logic; string fragments: `00 2a 01 00 01 ` | `0x824c38` |
| `F8Sender.setFindDeviceBoth` | Delegates: `F8Sender.setFindDevice` | `0xf74ac8` |
| `F8Sender.setFindDeviceLeft` | Delegates: `F8Sender.setFindDevice` | `0xf5f358` |
| `F8Sender.setFindDeviceRight` | Delegates: `F8Sender.setFindDevice` | `0xf5f518` |
| `F8Sender.setFindOff` | Delegates: `F8Sender.setFindDevice` | `0x824c14` |
| `F8Sender.setLeftTouchLong` | Parameterized / helper logic; string fragments: `00 22 01 00 03 07 01 0` | `0x7f6a5c` |
| `F8Sender.setLeftTouchOne` | Parameterized / helper logic; string fragments: `00 22 01 00 03 01 01 0` | `0x7fa16c` |
| `F8Sender.setLeftTouchThree` | Parameterized / helper logic; string fragments: `00 22 01 00 03 05 01 0` | `0x7f6f08` |
| `F8Sender.setLeftTouchTwo` | Parameterized / helper logic; string fragments: `00 22 01 00 03 03 01 0` | `0x7f70b8` |
| `F8Sender.setRightTouchLong` | Parameterized / helper logic; string fragments: `00 22 01 00 03 08 01 0` | `0x7f66e4` |
| `F8Sender.setRightTouchOne` | Parameterized / helper logic; string fragments: `00 22 01 00 03 02 01 0` | `0x7f6534` |
| `F8Sender.setRightTouchThree` | Parameterized / helper logic; string fragments: `00 22 01 00 03 06 01 0` | `0x7f5f1c` |
| `F8Sender.setRightTouchTwo` | Parameterized / helper logic; string fragments: `00 22 01 00 03 04 01 0` | `0x7f61bc` |
| `F8Sender.setTransLevel` | Parameterized / helper logic; string fragments: `00 31 01 00 01 ` | `0xadc414` |
| `RCommandLostSender.setConnectMulOn` | Parameterized / helper logic | `0xfb3c94` |
| `RsCommonSender.setAncLevel` | Parameterized / helper logic | `0xad0d24` |
| `RsCommonSender.setAudioMulDevice` | Delegates: `RsCommonSender.setConnectMulOn` | `0x815ccc` |
| `RsCommonSender.setConnectMulOff` | Delegates: `RsCommonSender.setAudioAacSbc` | `0xf5afc0` |
| `RsCommonSender.setDingLevel` | Parameterized / helper logic | `0x7f85ec` |
| `RsCommonSender.setFindDevice` | Parameterized / helper logic | `0x824cd8` |
| `RsCommonSender.setFindDeviceBoth` | Delegates: `RsCommonSender.setFindDevice` | `0xf74af0` |
| `RsCommonSender.setFindDeviceLeft` | Delegates: `RsCommonSender.setFindDevice` | `0xf5f380` |
| `RsCommonSender.setFindDeviceRight` | Delegates: `RsCommonSender.setFindDevice` | `0xf5f540` |
| `RsCommonSender.setFindOff` | Delegates: `RsCommonSender.setFindDevice` | `0x824cb0` |
| `RsCommonSender.setLeftTouchLong` | Parameterized / helper logic; string fragments: `FF 00 03 01 04 0` | `0x7f6ad0` |
| `RsCommonSender.setLeftTouchOne` | Parameterized / helper logic; string fragments: `FF 00 03 01 01 0` | `0x7fa1e0` |
| `RsCommonSender.setLeftTouchThree` | Parameterized / helper logic; string fragments: `FF 00 03 01 03 0` | `0x7f6f7c` |
| `RsCommonSender.setLeftTouchTwo` | Parameterized / helper logic; string fragments: `FF 00 03 01 02 0` | `0x7f712c` |
| `RsCommonSender.setRightTouchLong` | Parameterized / helper logic; string fragments: `FF 00 03 01 14 0` | `0x7f6758` |
| `RsCommonSender.setRightTouchOne` | Parameterized / helper logic; string fragments: `FF 00 03 01 11 0` | `0x7f65a8` |
| `RsCommonSender.setRightTouchThree` | Parameterized / helper logic; string fragments: `FF 00 03 01 13 0` | `0x7f5f90` |
| `RsCommonSender.setRightTouchTwo` | Parameterized / helper logic; string fragments: `FF 00 03 01 12 0` | `0x7f6230` |
| `RsCommonSender.setSpatialAudioModeHead` | Delegates: `RsCommonSender.setSpatialAudioSwitchOn` | `0x946054` |
| `RsCommonSender.setSpatialAudioModeOff` | Delegates: `RsCommonSender.setSpatialAudioSwitchOff` | `0x924ea8` |
| `RsCommonSender.setTransLevel` | Parameterized / helper logic | `0xadc4d0` |
| `RsCommonV2Sender._setTapKey` | Parameterized / helper logic | `0x8238d8` |
| `RsCommonV2Sender._setTouchKey` | Parameterized / helper logic | `0x7f603c` |
| `RsCommonV2Sender.getTouch` | Empty/shared entry; no command bytes confirmed | `0xf8dae0` |
| `RsCommonV2Sender.intToEqList` | Parameterized / helper logic | `0x9851b0` |
| `RsCommonV2Sender.setAiPageState` | Parameterized / helper logic | `0xac4b10` |
| `RsCommonV2Sender.setAncLevel` | Parameterized / helper logic | `0xad0dd0` |
| `RsCommonV2Sender.setAncLine` | Parameterized / helper logic | `0x7f2e1c` |
| `RsCommonV2Sender.setAudioMulDevice` | Delegates: `RsCommonV2Sender.setConnectMulOn` | `0x815cf0` |
| `RsCommonV2Sender.setDingLevel` | Parameterized / helper logic | `0x7f8698` |
| `RsCommonV2Sender.setEqBassPlus` | Parameterized / helper logic | `0x989ac8` |
| `RsCommonV2Sender.setEqCustomData` | Delegates: `RsCommonV2Sender.intToEqList` | `0x9850e4` |
| `RsCommonV2Sender.setFindDevice` | Parameterized / helper logic | `0xf5f3d0` |
| `RsCommonV2Sender.setFindDeviceBoth` | Delegates: `RsCommonV2Sender.setFindDevice` | `0xf74b18` |
| `RsCommonV2Sender.setFindDeviceLeft` | Delegates: `RsCommonV2Sender.setFindDevice` | `0xf5f3a8` |
| `RsCommonV2Sender.setFindDeviceRight` | Delegates: `RsCommonV2Sender.setFindDevice` | `0xf5f568` |
| `RsCommonV2Sender.setGainLevel` | Parameterized / helper logic | `0x7fc370` |
| `RsCommonV2Sender.setHearingProtection` | Parameterized / helper logic | `0x7fc4bc` |
| `RsCommonV2Sender.setLeftTapThree` | Delegates: `RsCommonV2Sender._setTapKey` | `0x8fe0bc` |
| `RsCommonV2Sender.setLeftTapTwo` | Delegates: `RsCommonV2Sender._setTapKey` | `0x8238ac` |
| `RsCommonV2Sender.setLeftTouchLong` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f6b50` |
| `RsCommonV2Sender.setLeftTouchOne` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7fa260` |
| `RsCommonV2Sender.setLeftTouchThree` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f6ffc` |
| `RsCommonV2Sender.setLeftTouchTwo` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f71ac` |
| `RsCommonV2Sender.setRightTapThree` | Delegates: `RsCommonV2Sender._setTapKey` | `0x912ee4` |
| `RsCommonV2Sender.setRightTapTwo` | Delegates: `RsCommonV2Sender._setTapKey` | `0x907c1c` |
| `RsCommonV2Sender.setRightTouchLong` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f67d8` |
| `RsCommonV2Sender.setRightTouchOne` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f6628` |
| `RsCommonV2Sender.setRightTouchThree` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f6010` |
| `RsCommonV2Sender.setRightTouchTwo` | Delegates: `RsCommonV2Sender._setTouchKey` | `0x7f62b0` |
| `RsCommonV2Sender.setStarburstSdk` | Parameterized / helper logic | `0xa41150` |
| `RsCommonV2Sender.setTime` | Delegates: `RsCommonV2Sender.timeCommand` | `0xa3b17c` |
| `RsCommonV2Sender.setTransLevel` | Parameterized / helper logic | `0xadc57c` |
| `RsCommonV2Sender.timeCommand` | Parameterized / helper logic | `0x924ae4` |
| `RsCommonV2Sender.transform` | Parameterized / helper logic | `0xf9e4f8` |
| `Zt219Sender.setNoiseModeWind` | Delegates: `Zt219Sender.setNoiseModeNormal` | `0x8e4de0` |
| `Zt229Sender.setAncLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 06 87 0b 00` | `0xad0e5c` |
| `Zt229Sender.setDingLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 83 0b 00 ` | `0x7f87ec` |
| `Zt229Sender.setFindDevice` | Empty/shared entry; no command bytes confirmed | `0x7c4974` |
| `Zt229Sender.setFindOff` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 82 0c 00 03 00` | `0x824dc4` |
| `Zt229Sender.setTransLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 06 84 0b 00` | `0xadc608` |
| `Zt231Sender._buildEqCommand` | Parameterized / helper logic | `0x7f8b04` |
| `Zt231Sender.getCustomEq` | Delegates: `Zt231Sender._buildEqCommand` | `0x986294` |
| `Zt231Sender.getGainLevel` | Delegates: `Zt231Sender._buildEqCommand` | `0x7f8ad4` |
| `Zt231Sender.setDingLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 03 0b 00 ` | `0x7f88b8` |
| `Zt231Sender.setEqCustom` | Delegates: `Zt231Sender._buildEqCommand` | `0x988ca0` |
| `Zt231Sender.setEqCustomData` | Delegates: `Zt231Sender._buildEqCommand` | `0x985418` |
| `Zt231Sender.setFindOff` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 02 0c 00 03 00` | `0x824e10` |
| `Zt231Sender.setGainLevel` | Delegates: `Zt231Sender._buildEqCommand` | `0x7fc3fc` |
| `ZtSender.getConnectMul` | Delegates: `Zt231Sender.getLoseLoss` | `0x8f446c` |
| `ZtSender.setAncLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 06 87 0b 00` | `0xad0f38` |
| `ZtSender.setAncLine` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 06 85 0E 00 ` | `0x7f3044` |
| `ZtSender.setAudioAacSbc` | Delegates: `ZtSender.setConnectMulOn` | `0x8a22b0` |
| `ZtSender.setConnectMulOff` | Delegates: `Zt231Sender.setAudioLdac` | `0xf5b034` |
| `ZtSender.setDingLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 03 0b 00 ` | `0x7f8724` |
| `ZtSender.setFindDevice` | Empty/shared entry; no command bytes confirmed | `0x7c4974` |
| `ZtSender.setFindDeviceBoth` | Delegates: `ZtSender.setFindDeviceLeft` | `0xf74b40` |
| `ZtSender.setFindDeviceLeft` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 82 0c 00 ` / `00 01` / `08 EE 00 00 00 0E 02 0c 00 ` | `0xf5f44c` |
| `ZtSender.setFindDeviceRight` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 0E 82 0c 00 ` / `01 01` / `08 EE 00 00 00 0E 02 0c 00 ` | `0xf5f590` |
| `ZtSender.setLeftTouchLong` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 00 03 0` | `0x7f6b7c` |
| `ZtSender.setLeftTouchOne` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 00 00 0` | `0x7fa28c` |
| `ZtSender.setLeftTouchThree` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 00 02 0` | `0x7f7028` |
| `ZtSender.setLeftTouchTwo` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 00 01 0` | `0x7f71d8` |
| `ZtSender.setRightTouchLong` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 01 03 0` | `0x7f6804` |
| `ZtSender.setRightTouchOne` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 01 00 0` | `0x7f6654` |
| `ZtSender.setRightTouchThree` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 01 02 0` | `0x7f612c` |
| `ZtSender.setRightTouchTwo` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 04 81 0D 00 01 01 0` | `0x7f62dc` |
| `ZtSender.setTransLevel` | Parameterized / helper logic; string fragments: `08 EE 00 00 00 06 84 0b 00` | `0xadc6e4` |


## 10. Initialization, OTA, and implementation boundaries

### Initialization reads (S)

The following lists are call order recovered from model initialization methods. An inherited initialization method may serve other models too; absence here does not imply no initialization. Query names must be resolved through the correct sender. Some V2 paths also query features selected by visible widgets rather than always sending a universal list.

| Model configuration | Read sequence | ELF |
| --- | --- | --- |
| `CeramicsDeviceConfig` | `getBattery` → `getAnc` → `getEq` → `getGameMode` → `getAudioLdac` → `getVersion` → `getKey` → `getEarDetection` → `getDingLevel` → `getAncLevel` → `getTouch` | `0xbe32d4` |
| `ExplorerDeviceConfig` | `getBattery` → `getAnc` → `getEq` → `getGameMode` → `getAudioLdac` → `getVersion` → `getKey` → `getEarDetection` → `getDingLevel` → `getAncLevel` → `getTouch` → `getEarLanguage` | `0xbe36d4` |
| `BudsfreeDeviceConfig` | `getInfo` → `_sendTransformed` | `0xbe3810` |
| `CambrianDeviceConfig` | `getInfo` | `0xbe3980` |
| `OpenfreeDeviceConfig` | `getInfo` → `_sendTransformed` | `0xbe3a08` |
| `AstroXDeviceConfig` | `getInfo` → `setTime` | `0xbe3ae0` |
| `RsCommonV2DeviceConfig` | `getInfo` | `0xbe3b9c` |
| `EarfreeI3DeviceConfig` | `getInfo` → `getAnc` → `getEq` → `getGameMode` → `getAudioLdac` → `getKey` → `getTouch` → `getHideSearch` | `0xbe3dcc` |
| `EarfreeI5DeviceConfig` | `getInfo` → `getAnc` → `getEq` → `getGameMode` → `getAudioLdac` → `getVersion` → `getKey` → `getTouch` → `getAncList` → `getEarDetection` → `getAncLevel` → `getAncTransLevel` → `getDingLevel` | `0xbe3ed4` |
| `HeadfreeDeviceConfig` | `getInfo` → `getAnc` → `getEq` → `getCustomEq` → `getGainLevel` → `getGameMode` → `getAudioLdac` → `getVersion` → `getKey` → `getTouch` → `getBattery` → `getLoseLoss` → `getConnectMul` → `getMulDeviceNameList` → `getAncTransLevel` → `getDingLevel` → `getAncList` | `0xbe406c` |

### OTA and undecoded areas (P)

The OTA UUIDs and forwarding configuration in section 2 identify transport interfaces only. Firmware container formats, model/chip checks, flash addresses, erase/write commands, chunk sizes, acknowledgements, signature/checksum rules, recovery, and reboot handshakes have not been fully decoded. This reference is therefore not a complete firmware flashing specification.

Other incomplete areas include exact ZT numeric variant binding, multi-device name encoding, all button-action enums, V2 time-zone/date serialization, custom EQ conversion, ANC-list parameters, AI subcommands, Opus/Starburst payloads, AURAMIC HID framing, all MIC field enums, and error/retry timing. The method/type inventories preserve these entries without fabricating bytes.

### Client implementation notes

- Select the exact model/chip profile first, then its protocol and sender overrides.
- Enable reception before querying; accept unsolicited status updates.
- Buffer split reads and coalesced frames. Validate bounds before indexing.
- Keep V2 packet checksum, LTV lengths, ACKs, and legacy exceptions as separate concerns.
- Use actual status readback to confirm a setting; a completed Bluetooth write only confirms local transport submission.
- Retain raw frames alongside parsed values and timestamps. Do not treat cached case data as a new measurement.
- Feature visibility and sender method availability are not proof of hardware support. Only OpenFun battery reading has hardware evidence here.

## 11. Evidence locations and further validation

Binary: `data/app.so` (ELF x86-64, Dart AOT). SHA-256:

```text
b997646e1e1dc51e7271aba5fb989f6e5439f0000eaba3152c842422665072c2
```

Addresses below are code locations in this exact ELF, not stable API identifiers or process-memory addresses. They change between releases.

| Evidence entry | ELF address |
| --- | --- |
| OpenFun model mapping | `0xbd8dc8` |
| AURAMIC V2 inheritance | `0xbd8ff8` |
| RS getInfo / battery | `0xb4e700 / 0xf772a0` |
| F8 getBattery / battery | `0x82141c / 0xf7653c` |
| V2 getInfo / transform | `0xb4e7e8 / 0xf9e4f8` |
| V2 checksum / packet validation | `0x7f337c / 0xf88c00` |
| V2 streaming decoder / LTV split | `0xf8758c / 0xf85fc8` |
| V2 LTV type dispatch / ACK | `0xf77c00 / 0xf85a50` |
| V2 battery / MIC battery | `0xf84ccc / 0xf8002c` |
| ZT getInfo / ZT231 getBattery | `0xb4e948 / 0x82148c` |
| ZT receive / TWS battery / headset battery | `0xf89e48 / 0xf8c230 / 0xf8c0d8` |
| ZT231 EQ builder | `0x7f8b04` |
| BLE OTA / HID OTA configuration | `0xbd8a34 / 0xbd9074` |

Validation performed for this edition: 48 configuration entries reconciled as 46 concrete profiles plus two templates; all 274 sender methods accounted for; 166 complete constant-array entries checked for array length; RS lengths, ordinary ZT lengths/checksums, and the V2 example checksum checked offline. The embedded Python examples were executed. The nine-byte ZT219 anomaly remains explicitly flagged. No additional hardware coverage is implied.
