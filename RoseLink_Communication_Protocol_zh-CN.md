# RoseLink 上位机通信协议参考（全部型号配置）

版本：1.0 · 整理日期：2026-10-22 · 分析对象：Windows **RoseLink 3.8.2+5409**。

本文依据 RoseLink 的 Dart AOT 主逻辑、运行时配置和 OpenFun 实际通信整理。它是一份逆向工程参考，不是厂商发布的协议规范。“全部型号”指该程序版本中提取到的 **46 个具体型号配置**（包括芯片变体与 AURAMIC 麦克风），另列 2 个通用模板，不代表所有历史/未来产品。

证据等级：**H**＝OpenFun 实机收发验证；**S**＝程序常量/控制流程直接确认；**P**＝部分结构已知，剩余字段或设备行为待确认。除明确标 H 的 OpenFun 电量通信外，其他型号和控制命令均未逐台实测。命令表中的名称采用程序标识符；函数名表示软件的意图，不能保证每款同族设备都支持。

十六进制均表示实际字节；偏移从 0 开始；`n` 为十进制参数，`NN` 为其一个字节的十六进制编码；`CS` 为加和校验，不是 CRC。

## 目录

1. [型号与协议](#1-型号与协议)
2. [连接与 UUID](#2-连接与-uuid)
3. [RS Common（含 OpenFun）](#3-rs-common含-openfun)
4. [F8](#4-f8)
5. [RS Common V2](#5-rs-common-v2)
6. [ZT 与派生发送器](#6-zt-与派生发送器)
7. [电量解析对照](#7-电量解析对照)
8. [完整固定命令目录](#8-完整固定命令目录)
9. [动态命令与别名目录](#9-动态命令与别名目录)
10. [初始化流程、OTA 与实现边界](#10-初始化流程ota-与实现边界)
11. [证据定位与后续验证](#11-证据定位与后续验证)

## 1. 型号与协议

数量：F8 **6**、RS Common **7**、RS Common V2 **30**、ZT **3**。SPP 列是程序配置标记，并非逐台枚举物理服务的结果。`—` 表示所检查型号配置中没有显式赋值。`tws`＝真无线、`head`＝头戴式、`mic`＝麦克风。

### F8

| 显示名称 | 内部名称 | 形态 | SPP | 配置类 | ELF 地址 |
| --- | --- | --- | --- | --- | --- |
| AGATE | `agate` | tws | false | `AgateDeviceConfig` | `0xbd88bc` |
| AGATE X | `agatex` | tws | false | `AgateXDeviceConfig` | `0xbd8908` |
| CERAMICS | `ceramics` | tws | false | `CeramicsDeviceConfig` | `0xbd8954` |
| CERAMICS-X | `CERAMICS-X-7034` | tws | true | `CeramicsX7034DeviceConfig` | `0xbd89a0` |
| CERAMICS-X | `CERAMICS-X` | tws | false | `CeramicsXDeviceConfig` | `0xbd8ba0` |
| EXPLORER | `EXPLORER` | tws | true | `ExplorerDeviceConfig` | `0xbd8c20` |


### RS Common

| 显示名称 | 内部名称 | 形态 | SPP | 配置类 | ELF 地址 |
| --- | --- | --- | --- | --- | --- |
| BudsFree i | `budsfree` | tws | false | `BudsfreeDeviceConfig` | `0xbd8c9c` |
| Cambrian | `cambrian` | head | true | `CambrianDeviceConfig` | `0xbd8ce8` |
| OpenFeel | `OpenFree` | tws | false | `OpenfreeDeviceConfig` | `0xbd8d7c` |
| OpenFun | `openfun` | tws | true | `OpenfunDeviceConfig` | `0xbd8dc8` |
| OpenLink | `openlink` | tws | true | `OpenlinkDeviceConfig` | `0xbd8e20` |
| SportFeel | `sportfree` | tws | true | `SportfreeDeviceConfig` | `0xbd8ec8` |
| SWIFT | `swift` | head | false | `SwiftDeviceConfig` | `0xbd8f20` |


### RS Common V2

| 显示名称 | 内部名称 | 形态 | SPP | 配置类 | ELF 地址 |
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

| 显示名称 | 内部名称 | 形态 | SPP | 配置类 | ELF 地址 |
| --- | --- | --- | --- | --- | --- |
| EarFree-i3 | `earfree-i3` | tws | false | `EarfreeI3DeviceConfig` | `0xbda008` |
| EarFeel-i5 | `EARFREE-i5` | tws | false | `EarfreeI5DeviceConfig` | `0xbda084` |
| HeadFeel | `headfree` | head | true | `HeadfreeDeviceConfig` | `0xbda0f0` |

两个通用配置为 `RsCommonDeviceConfig`（`RS-Common`，rs-common，SPP=false）和 `RsCommonV2DeviceConfig`（`RS-Common-v2`，rs-common-v2，SPP=true）。AURAMIC 先继承 V2 配置，再设置麦克风身份及 SPP=false；昵称为 `AURAMIC1`，地址格式有效时追加规范化地址最后四字符组成的 `_xxxx`。

保留程序原名：`OpenFree` 显示为 **OpenFeel**，`sportfree` 为 **SportFeel**，`EARFREE-i5` 为 **EarFeel-i5**，`speakfeel2` 为 **SPEAKFEEL**。CERAMICS-X/MK2/U 的芯片变体不能合并。这些内部名称也不等于完整的蓝牙名称匹配规则。

## 2. 连接与 UUID

软件存在经典蓝牙 SPP/RFCOMM 和 BLE 两条路径。OpenFun 已在服务 UUID `0cf12d31-fac3-4553-bd80-d6832e7b3931` 上通过 RFCOMM 实测。Windows 应通过服务发现解析通道，不应硬编码 RFCOMM 通道号；另一客户端连接相同服务前应释放 RoseLink 已占用的连接。BLE 路径写入写特征，并订阅读/通知特征；一次通知或一次 socket 读取不一定等于一帧。

`uuid`（SPP 身份）、`serviceUUID`（普通 BLE）、`otaUuid` 和 `otaServiceUUID` 是不同字段，不能互相替代。

| 字段 | 短 UUID | 完整 UUID |
| --- | --- | --- |
| 普通 BLE 服务 | 00FE | 000000fe-0000-1000-8000-00805f9b34fb |
| 写特征 | 00F1 | 000000f1-0000-1000-8000-00805f9b34fb |
| 读/通知特征 | 00F2 | 000000f2-0000-1000-8000-00805f9b34fb |

上述 BLE 字段来自 OpenFun 运行时对象（S），未独立枚举其物理 BLE 服务，也不能作为全部型号已验证通用的服务列表。

### 显式 SPP / OTA 配置

`G`＝`0cf12d31-fac3-4553-bd80-d6832e7b3931`。UUID 大小写等价。`—` 表示该处没有显式赋值，不等于不支持 OTA；OTA 列仅表示接口配置。

| 型号 | uuid / SPP | otaUuid | OTA BLE service | OTA write | OTA read |
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

通用 BLE OTA 辅助函数还包含 `7033 / 2001 / 2002`、`ff12 / ff15 / ff14`，以及 `65786365-6c70-6f69-6e74-2e636f6d0000 / 65786365-6c70-6f69-6e74-2e636f6d0001 / 65786365-6c70-6f69-6e74-2e636f6d0001` 三组服务/写/读配置。其选择依赖 OTA 枚举；这里不为未核实型号分配这几组值。AURAMIC 还调用 USB HID OTA 配置函数，设置 `otaForwardType=abDongleGuid`；本文未确认完整 HID Report 格式。

## 3. RS Common（含 OpenFun）

### 3.1 发送格式（S）

```text
FF 00 LEN BODY AA
LEN = len(BODY)
```

固定命令没有 V2 的额外校验字节；不能把 `00` 自动解释为与 V2 相同的递增序号。常见设置 Body 为 `TYPE VALUE`；批量查询 Body 为 `FA ITEM...`。具体命令以第 8 节的原始数组为准，未实测的单项查询不能仅按相似格式当作已验证命令。

### 3.2 OpenFun 最小读取流程（H）

1. 连接 `ROSE OpenFun` 的上述 RFCOMM 服务。
2. 开始持续接收；耳机也可能主动推送电量。
3. 发送下列完整 getInfo，包含电量信息项 `0C`。
4. 缓存并重组接收流，处理 `0C` 类型。

```text
TX: FF 00 0F FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 33 AA
RX: DD 00 04 0C 40 3D 00 AA
```

实际回包对应左耳 **64%**、右耳 **61%**、盒字段 **0**。盒字段 0 应保留，不能据此推断充电盒一定没电；其新鲜度和产品语义需结合设备行为。

| 偏移 | 字段 | 说明 |
| --- | --- | --- |
| 0 | `DD` | OpenFun 实际接收帧首 |
| 1 | `00` | 已观察值；通用语义未确认 |
| 2 | `04` | 后续 TYPE+VALUE 字节数 |
| 3 | `0C` | 电量类型 |
| 4 | 左耳 | 低 7 位百分比、最高位充电 |
| 5 | 右耳 | 低 7 位百分比、最高位充电 |
| 6 | 盒字段 | 原程序按完整字节解析 |
| 7 | `AA` | 帧尾 |

在这一已验证格式中，总长为 `frame[2]+4`。只看到 `AA` 就截断会误切 payload 内的同值字节；应先按长度判断，再核对尾字节。其他型号的接收帧首尚未逐台确认。

### 3.3 接收消息类型（S）

`RsCommonReceiver.process` 依据完整帧 byte[3] 分派。表中含义来自接收处理函数，具体多字节 Value 仍需对应字段解码。

| 类型 | 含义 / 处理入口 | ELF 地址 |
| --- | --- | --- |
| `01` | 按键映射 | `0xf76180` |
| `07` | 触控开关 | `0xf777ec` |
| `08` | 入耳检测 | `0xf776f4` |
| `09` | 降噪模式 | `0xf759f0` |
| `0C` | 左右耳及充电盒电量 | `0xf772a0` |
| `0D` | 固件版本 | `0xf77070` |
| `0E` | 游戏模式 | `0xf76f78` |
| `2A` | EQ 模式 | `0xf76324` |
| `2B` | 编码/连接模式 | `0xf75ae0` |
| `2C` | 降噪强度 | `0xf75900` |
| `2D` | 通透强度 | `0xf75810` |
| `2E` | 提示音音量 | `0xf75724` |
| `33` | 空间音频开关 | `0xf76e80` |


## 4. F8

F8 的发送字节与 RS 两族不同。多数固定帧形如 `00 CMD 01 00 LEN DATA...`；这里 `LEN` 对应其后 DATA 长度，但这是已提取命令的结构，不是经过所有固件验证的全局规则。例如查询和 EQ 设置分别为：

```text
getBattery: 00 27 01 00 01 01
getVersion: 00 27 01 00 01 02
getEq:      00 27 01 00 01 04
getKey:     02 27 01 00 01 05
setEqHifi:  00 20 01 00 02 00 00
```

`getKey` 的首字节确实为 `02`，不要统一改成 `00`。本次提取的固定数组没有另附校验和。接收函数先检查完整帧 byte[1] 为 `27` 或 `28`，再依据 byte[5] 分派；电量取 byte[7]/[8]/[9]。接收头尾、完整长度策略及所有固件的分片行为尚未实机确认（P）。

Explorer 有独立 `ExplorerSender.setNoiseModeNormal`，实际委托给 `F8Sender.setNoiseModeWind`，得到 `00 2C 01 00 01 00`；不能用普通 F8 的“Normal”常量替换。F8 EQ 接收还有 `0404` 特殊分支；下面的 `04` 为主子类型，特殊判定需要单独保留。

| 类型 | 含义 / 处理入口 | ELF 地址 |
| --- | --- | --- |
| `01` | 左右耳及充电盒电量 | `0xf7653c` |
| `02` | 固件版本 | `0xf76414` |
| `04 (+0404)` | EQ 模式 | `0xf76324` |
| `05` | 按键映射 | `0xf75ebc` |
| `08` | 游戏模式 | `0xf75dc4` |
| `09` | 入耳检测 | `0xf75ccc` |
| `0A` | 提示语言 | `0xf75bcc` |
| `0B` | 编码/连接模式 | `0xf75ae0` |
| `0C` | 降噪模式 | `0xf759f0` |
| `11` | 降噪强度 | `0xf75900` |
| `12` | 通透强度 | `0xf75810` |
| `13` | 提示音音量 | `0xf75724` |
| `16` | 触控开关 | `0xf7562c` |


## 5. RS Common V2

### 5.1 发送封包（S）

发送器返回的是 **Body**，需要 `transform` 追加外层。第 8 节 V2 命令列不能直接当成完整蓝牙报文发送。

```text
FF SEQ LEN BODY CS AA
LEN = len(BODY)
CS  = sum(FF, SEQ, LEN, BODY...) & 0xFF
```

SEQ 是 8 位递增序号，回绕到 0。`LEN` 是一个字节；实现应拒绝无法用单字节表示的普通 Body，而不是静默截断。已提取的批量 getInfo Body 为：

```text
FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 2F 31 32 33 36 37 38 39 3A 3B 3C 3D 3F 45 46 49
```

SEQ=0 时完整帧（由程序算法计算，非实测）：

```text
FF 00 1E FA 01 07 08 09 0C 0D 0E 12 2A 2B 2C 2D 2E 2F 31 32 33 36 37 38 39 3A 3B 3C 3D 3F 45 46 49 E9 AA
```

### 5.2 接收 LTV、校验与 ACK（S）

接收支持 `DD` 或 `FF` 开头，并由独立流式 FrameDecoder 重组与重同步。常规接收结构为：

```text
HEADER SEQ [L TYPE VALUE...] [L TYPE VALUE...] ... CS AA
L = 1 + len(VALUE)             # TYPE 包含在 L 中，L 本身不包含
next_offset = offset + 1 + L
```

一个接收帧可以含多个 LTV；不能把第一个 L 当成整帧唯一长度。加和校验的主路径覆盖头、SEQ 和全部 LTV，低 8 位应等于 CS；`CommandUtil._isValidPacket` 也兼容从 byte[2] 开始求和的历史形式。解析器遇到失败会尝试恢复，不应把兼容路径理解为可以忽略校验。

`L=00` 的特殊处理只允许星芒相关类型 `41/42`，通过专用帧尾搜索与校验处理；普通电量等类型不接受这种无限长度解释。源码中的分帧上限为 8192 个十六进制字符，约 4096 字节，不是 8192 字节。

`TYPE=FE` 走 ACK 路径，Value 必须为 2 字节；程序记录收到的 SEQ 和状态码。本文未确认全部状态码定义或重发时序，不能将任何 ACK 都视作成功。

类型 `49` 按 Value 长度重载：2 字节走独立麦克风模式，4/6 字节走扩展固件版本处理；其他长度记录异常。`E0–E4` 和 `40` 进入 Opus 扩展处理，`42` 进入星芒 SDK 处理；这些内部载荷未完整还原。

### 5.3 接收消息类型（S）

下表中的类型来自 LTV 的 TYPE，电量 Value 的偏移以 Value 起点为基准。麦克风 `50–7C` 扩展已收录；处理入口的识别不代表其每个子字段都已确认。

| 类型 | 含义 / 处理入口 | ELF 地址 |
| --- | --- | --- |
| `2A` | EQ 模式 | `0xf853bc` |
| `09` | 降噪模式 | `0xf85194` |
| `0C` | 左右耳及充电盒电量 | `0xf84ccc` |
| `01` | 按键映射 | `0xf84b78` |
| `07` | 触控开关 | `0xf849c0` |
| `08` | 入耳检测 | `0xf84808` |
| `0D` | 固件版本 | `0xf84524` |
| `0E` | 游戏模式 | `0xf8436c` |
| `2B` | 编码/连接模式 | `0xf75ae0` |
| `32` | 多设备连接 | `0xf841b4` |
| `2C` | 降噪强度 | `0xf75900` |
| `2D` | 通透强度 | `0xf75810` |
| `2E` | 提示音音量 | `0xf75724` |
| `33` | 空间音频开关 | `0xf83ffc` |
| `34` | 耳机 MAC | `0xf83c8c` |
| `35` | 时间同步 | `0xf83bf8` |
| `36` | 降噪模式列表 | `0xf838c0` |
| `2F` | 寻找耳机 | `0xf837f0` |
| `31` | 提示语言 | `0xf83704` |
| `37` | 陀螺仪模式 | `0xf83638` |
| `38` | 空间音频模式 | `0xf8346c` |
| `39` | 头部运动模式 | `0xf832b4` |
| `3A` | 自适应佩戴 | `0xf830fc` |
| `3B` | 轻拍控制开关 | `0xf82f44` |
| `3C` | 轻拍按键映射 | `0xf82ab0` |
| `3D` | 空间音频内容模式 | `0xf828d0` |
| `3E` | 自定义 EQ 数据 | `0xf8272c` |
| `3F` | 接收器固件版本 | `0xf82448` |
| `45` | 增益档位 | `0xf82260` |
| `46` | 听力保护 | `0xf82078` |
| `47` | 耳塞贴合测试 | `0xf81ec0` |
| `48` | AI 命令 | `0xf81b3c` |
| `49` | 独立麦克风模式 / 扩展固件版本，按长度区分 | `0xf816f8` |
| `49` | 独立麦克风模式 / 扩展固件版本，按长度区分 | `0xf81410` |
| `4A` | 组模式 | `0xf81258` |
| `50` | 麦克风基本信息 | `0xf806dc` |
| `51` | 麦克风四槽电量 | `0xf8002c` |
| `52` | 麦克风电平 | `0xf7fe1c` |
| `53` | 发射器状态 | `0xf7f318` |
| `54` | 发射器降噪 | `0xf7eff8` |
| `55` | 麦克风增益 | `0xf7ec98` |
| `56` | 录音格式 | `0xf7e8b8` |
| `57` | 低切开关 | `0xf7e600` |
| `58` | 提示音 | `0xf7e178` |
| `59` | 指示灯 | `0xf7def0` |
| `5A` | 自动关机 | `0xf7dbac` |
| `5B` | 静音 | `0xf7d924` |
| `5C` | 自动录音 | `0xf7d69c` |
| `5D` | 接收器声道 | `0xf7d47c` |
| `5E` | 相机供电 | `0xf7d30c` |
| `5F` | 增益配置 | `0xf7cdac` |
| `60` | 手机扬声器 | `0xf7cc3c` |
| `61` | BLE 电源模式 | `0xf7ca38` |
| `62` | 监听音 | `0xf7c860` |
| `63` | 时间码 | `0xf7bce4` |
| `72` | 远端目标 | `0xf7b868` |
| `77` | 录音控制 | `0xf7b1ac` |
| `78` | 发射器 MAC | `0xf7a984` |
| `79` | 低切频率 | `0xf7a5dc` |
| `7A` | TF 卡时长 | `0xf7a164` |
| `7B` | 循环录音 | `0xf79800` |
| `7C` | 设备日志 | `0xf79584` |
| `F0` | 主从切换 | `0xf79474` |
| `40` | Opus/扩展音频命令 | `0xf79354` |
| `42` | 星芒 SDK 命令 | `0xf790c0` |
| `FE` | 命令 ACK，2 字节 Value | `0xf85a50` |
| `E0–E4 / 40` | Opus 扩展 | `0xf79354` |


## 6. ZT 与派生发送器

### 6.1 常见完整帧（S）

```text
08 EE 00 00 00 GROUP OP LEN_LO LEN_HI DATA... CS
CS = sum(除 CS 外的全部帧字节) & 0xFF
```

已核对的普通帧中 byte[7:9] 是包含校验字节的总长度，小端；无 DATA 的查询通常长 10 字节。头部保留字节的通用语义未确认。不能套用 RS 的 `AA` 帧尾，也不能给它再包 V2 外层。

```text
getInfo:    08 EE 00 00 00 01 01 0A 00 02
getBattery: 08 EE 00 00 00 01 03 0A 00 04   # Zt231Sender
```

`ZtSender` 外还有 `Zt219Sender`、`Zt229Sender`、`Zt231Sender` 覆盖部分命令。型号配置已确认三款属于 ZT；数字后缀与具体型号的完整构造绑定未在本文完成核验，因此命令目录保留发送器类别，不能将所有派生命令混发给任一 ZT 设备。

一个保留的异常常量：`Zt219Sender.getHideSearch` 提取值为 **9 字节** `08 EE 00 00 0B 01 0A 00 0C`，与普通头部/长度规则不同。文档忠实保留，不补一个 `00`，也不宣称它已经实机可用。

### 6.2 接收消息（S/P）

`ZTReceiver.process` 将完整帧 byte[5:7] 作为两个按顺序拼接的消息字节；例如 `01 01`，不是未经说明的小端整数。下表仅列直接明确的主分派；`0607`、`060C` 等强度分支和提示音分支还有长度/上下文条件，不按相邻字符串自动推断。

| 类型 | 含义 / 处理入口 | ELF 地址 |
| --- | --- | --- |
| `0101` | 左右耳及充电盒电量 | `0xf8c230` |
| `0103` | 头戴式单电量 | `0xf8c0d8` |
| `0D01` | 固件版本 | `0xf8bea8` |
| `0201` | EQ 模式 | `0xf8bdb0` |
| `0202` | 自定义 EQ 数据 | `0xf8bcd0` |
| `0203` | 增益档位 | `0xf8bbb4` |
| `0401` | 按键映射 | `0xf8b8e8` |
| `0603` | 游戏模式 | `0xf8b7f0` |
| `0C01` | 入耳检测 | `0xf8b6f8` |
| `0801` | 编码/连接模式 | `0xf8b604` |
| `0602` | 降噪模式 | `0xf8b400` |
| `0604` | 通透强度 | `0xf8b220` |
| `0E03` | 提示音音量 | `0xf8b110` |
| `0A01` | 触控开关 | `0xf8af0c` |
| `0E02` | 寻找耳机 | `0xf8ae3c` |
| `0605` | 降噪模式列表 | `0xf8ab04` |
| `0802` | 多设备连接 | `0xf8aa0c` |
| `0803` | 多设备名称列表 | `0xf8a7f8` |
| `0B01` | 隐藏搜索 | `0xf8a700` |


## 7. 电量解析对照

| 协议 | 输入与最小字段范围 | 左耳 | 右耳 | 盒/单电量 | 充电 |
| --- | --- | --- | --- | --- | --- |
| RS Common | 完整帧，类型 byte[3]=0C，至少覆盖 byte[6] | [4]&7F | [5]&7F | [6] 整字节 | 左右 bit7；盒未解析 |
| F8 | 完整帧，byte[1]=27/28，byte[5]=01，至少覆盖 byte[9] | [7]&7F | [8]&7F | [9] 整字节 | 左右 bit7；盒未解析 |
| V2 0C | 已拆 LTV Value，3 字节 | V[0]&7F | V[1]&7F | V[2]&7F | 三者均 bit7 |
| ZT 0101 | 完整帧，至少覆盖 byte[15] | [11] | [12] | [15] | [13]、[14] 非零为左右充电 |
| ZT 0103 | 完整帧，至少覆盖 byte[9] | — | — | [9] 单电量 | 软件设左右充电为 false |
| V2 MIC 51 | 已拆 LTV Value，严格 4 字节 | 四槽表示，不能按 TWS 解释 | — | 每槽 low7 | 每槽 bit7；FF＝不可用 |

ZT 0103 的单电量在软件里存入 `RBatteryData.batteryCase`，不表示真实充电盒。MIC 51 除 `FF` 外，任何槽的 low7 大于 100 都会使程序丢弃整条 Value；四槽对应的具体硬件通道尚未核实，不标成左右耳或盒。

普通耳机分支不具有 MIC 同样的 `FF` 特殊处理。保留未知/超范围原始值，客户端可以将其显示为不可用，不应钳位为 100%。区分“本次新数据”“缓存”“设备未在线”，充电盒显示可能来自缓存；0% 应保留为有效数值候选，而不是自动判离线。

以下为可独立运行的离线字段示例，只处理已经完成分帧/验长的输入，不包含蓝牙连接；3 个解析入口故意保持不同：
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

## 8. 完整固定命令目录

本目录覆盖全部已提取发送器方法中的 **166 条常量数组命令项**，不同协议间不去重。地址为该版本 ELF 代码地址。为易读省略 getter 的 `get:` 前缀；名称以 set 开头的 getter 仍然是写控制命令。F8、RS Common、ZT 列是完整数组；V2 列是需要第 5 节封包的 Body。别名及参数构造器见第 9 节。

### F8Sender

| 命令 | 完整数组 | ELF |
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

| 命令 | 完整数组 | ELF |
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

| 命令 | Body | ELF |
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

| 命令 | 完整数组 | ELF |
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

| 命令 | 完整数组 | ELF |
| --- | --- | --- |
| `getHideSearch` | `08 EE 00 00 0B 01 0A 00 0C` | `0xb4a144` |
| `setHideSearchOff` | `08 EE 00 00 00 0B 81 0B 00 00 8D` | `0x8f2648` |
| `setHideSearchOn` | `08 EE 00 00 00 0B 81 0B 00 01 8E` | `0x7f954c` |
| `setNoiseModeAnc` | `08 EE 00 00 00 06 82 0D 00 01 00 00 8C` | `0x8dfd00` |
| `setNoiseModeNormal` | `08 EE 00 00 00 06 82 0D 00 00 00 01 8C` | `0x8e701c` |
| `setNoiseModeTrans` | `08 EE 00 00 00 06 82 0D 00 00 01 00 8C` | `0x8f24c0` |


### Zt229Sender

| 命令 | 完整数组 | ELF |
| --- | --- | --- |
| `getAncTransLevel` | `08 EE 00 00 00 06 04 0A 00 0A` | `0xad7c20` |
| `getDingLevel` | `08 EE 00 00 00 0E 03 0A 00 11` | `0x8ff860` |


### Zt231Sender

| 命令 | 完整数组 | ELF |
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


## 9. 动态命令与别名目录

### 9.1 已解码参数模板（S）

以下来自构造逻辑，不是实机抓包。NN 表示完整一个字节，不要原样发送字母 N。范围是对应发送器的检查范围，不代表设备的通用能力。

| 协议 | 方法 | 模板 | n 范围 |
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

寻找包装入口传入 1＝左、2＝右、3＝双耳；关闭值在 RS/V2 中为 4，在 F8 中为 0。所检查 F8 通用构造器没有相同的显式范围检查。F8 降噪/通透强度的 NN 是重映射结果：输入 1→2、3→1、5→0，其他输入原样保留；不能直接复制 n。

### 9.2 触控映射（S）

| 方法 | F8 完整帧 | RS Common 完整帧 | V2 Body |
| --- | --- | --- | --- |
| `setLeftTouchOne` | `00 22 01 00 03 01 01 NN` | `FF 00 03 01 01 NN AA` | `01 01 NN` |
| `setLeftTouchTwo` | `00 22 01 00 03 03 01 NN` | `FF 00 03 01 02 NN AA` | `01 02 NN` |
| `setLeftTouchThree` | `00 22 01 00 03 05 01 NN` | `FF 00 03 01 03 NN AA` | `01 03 NN` |
| `setLeftTouchLong` | `00 22 01 00 03 07 01 NN` | `FF 00 03 01 04 NN AA` | `01 04 NN` |
| `setRightTouchOne` | `00 22 01 00 03 02 01 NN` | `FF 00 03 01 11 NN AA` | `01 11 NN` |
| `setRightTouchTwo` | `00 22 01 00 03 04 01 NN` | `FF 00 03 01 12 NN AA` | `01 12 NN` |
| `setRightTouchThree` | `00 22 01 00 03 06 01 NN` | `FF 00 03 01 13 NN AA` | `01 13 NN` |
| `setRightTouchLong` | `00 22 01 00 03 08 01 NN` | `FF 00 03 01 14 NN AA` | `01 14 NN` |

旧 F8/RS 方法在十六进制字符串中的字面量 `0` 后插入十进制动作编号，例如 n=5 得到 05；不能假设多位十进制数与通用十六进制单字节编码等价。动作编号对应功能尚未完整映射。V2 通过 _setTouchKey 直接写动作字节。轻拍映射使用另一个 _setTapKey 构造器，不能与触控映射互换。

V2 轻拍 Body 为 `3C KEY NN`：KEY=02（左双击）、03（左三击）、12（右双击）、13（右三击）；动作枚举仍需确认。

### 9.3 ZT 参数构造（S/P）

通用提示音模板为 `08 EE 00 00 00 0E 03 0B 00 NN CS`，ZtSender 检查 n=1..8，Zt231Sender 为 n=1..5；Zt229Sender 使用 OP=83。CS 对之前全部帧字节求和取低 8 位。ZT 触控构造器使用 `08 EE 00 00 00 04 81 0D 00 SIDE GESTURE NN CS`，SIDE=00/01，GESTURE=00..03；实现前仍需核对派生发送器绑定及动作语义。

Zt231Sender._buildEqCommand 构造 `08 EE 00 00 00 02 OP TOTAL_LEN 00 DATA... CS`，TOTAL_LEN=10+len(DATA)。getCustomEq 使用 OP=02，getGainLevel 使用 OP=03，setEqCustom 使用 OP=81、DATA=03；setGainLevel(n) 使用 OP=83、DATA=n+1，n=0..2。自定义 EQ 写入使用 OP=82，要求十个输入频段；增益编码、每频段字节转换、频率顺序和设备限值仍需确认。
### 9.4 其余方法完整索引

本表与第 8 节合计覆盖本次提取的 **274 个具名发送器方法**，包括辅助函数、别名及空入口。“委托”仅记录调用目标，有参数时不保证字节完全相同。“字符串片段”仅作为证据，不能直接发送。“参数化/辅助逻辑”表示本文未给出完整通用编码器，但第 9.1–9.3 节已明确的模板除外。

| 方法 | 已知行为 / 边界 | ELF |
| --- | --- | --- |
| `ExplorerSender.setNoiseModeNormal` | 委托：`F8Sender.setNoiseModeWind` | `0x8e6ec8` |
| `F8Sender.getConnectMul` | 委托：`F8Sender.getAudioLdac` | `0x8f4448` |
| `F8Sender.setAncLevel` | 参数化 / 辅助逻辑；字符串片段：`00 30 01 00 01 ` | `0xad0c68` |
| `F8Sender.setAudioMulDevice` | 委托：`F8Sender.setConnectMulOn` | `0x815ca8` |
| `F8Sender.setConnectMulOff` | 委托：`F8Sender.setAudioAacSbc` | `0xf5af9c` |
| `F8Sender.setDingLevel` | 参数化 / 辅助逻辑；字符串片段：`00 32 01 00 01 0` | `0x7f8578` |
| `F8Sender.setFindDevice` | 参数化 / 辅助逻辑；字符串片段：`00 2a 01 00 01 ` | `0x824c38` |
| `F8Sender.setFindDeviceBoth` | 委托：`F8Sender.setFindDevice` | `0xf74ac8` |
| `F8Sender.setFindDeviceLeft` | 委托：`F8Sender.setFindDevice` | `0xf5f358` |
| `F8Sender.setFindDeviceRight` | 委托：`F8Sender.setFindDevice` | `0xf5f518` |
| `F8Sender.setFindOff` | 委托：`F8Sender.setFindDevice` | `0x824c14` |
| `F8Sender.setLeftTouchLong` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 07 01 0` | `0x7f6a5c` |
| `F8Sender.setLeftTouchOne` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 01 01 0` | `0x7fa16c` |
| `F8Sender.setLeftTouchThree` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 05 01 0` | `0x7f6f08` |
| `F8Sender.setLeftTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 03 01 0` | `0x7f70b8` |
| `F8Sender.setRightTouchLong` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 08 01 0` | `0x7f66e4` |
| `F8Sender.setRightTouchOne` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 02 01 0` | `0x7f6534` |
| `F8Sender.setRightTouchThree` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 06 01 0` | `0x7f5f1c` |
| `F8Sender.setRightTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`00 22 01 00 03 04 01 0` | `0x7f61bc` |
| `F8Sender.setTransLevel` | 参数化 / 辅助逻辑；字符串片段：`00 31 01 00 01 ` | `0xadc414` |
| `RCommandLostSender.setConnectMulOn` | 参数化 / 辅助逻辑 | `0xfb3c94` |
| `RsCommonSender.setAncLevel` | 参数化 / 辅助逻辑 | `0xad0d24` |
| `RsCommonSender.setAudioMulDevice` | 委托：`RsCommonSender.setConnectMulOn` | `0x815ccc` |
| `RsCommonSender.setConnectMulOff` | 委托：`RsCommonSender.setAudioAacSbc` | `0xf5afc0` |
| `RsCommonSender.setDingLevel` | 参数化 / 辅助逻辑 | `0x7f85ec` |
| `RsCommonSender.setFindDevice` | 参数化 / 辅助逻辑 | `0x824cd8` |
| `RsCommonSender.setFindDeviceBoth` | 委托：`RsCommonSender.setFindDevice` | `0xf74af0` |
| `RsCommonSender.setFindDeviceLeft` | 委托：`RsCommonSender.setFindDevice` | `0xf5f380` |
| `RsCommonSender.setFindDeviceRight` | 委托：`RsCommonSender.setFindDevice` | `0xf5f540` |
| `RsCommonSender.setFindOff` | 委托：`RsCommonSender.setFindDevice` | `0x824cb0` |
| `RsCommonSender.setLeftTouchLong` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 04 0` | `0x7f6ad0` |
| `RsCommonSender.setLeftTouchOne` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 01 0` | `0x7fa1e0` |
| `RsCommonSender.setLeftTouchThree` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 03 0` | `0x7f6f7c` |
| `RsCommonSender.setLeftTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 02 0` | `0x7f712c` |
| `RsCommonSender.setRightTouchLong` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 14 0` | `0x7f6758` |
| `RsCommonSender.setRightTouchOne` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 11 0` | `0x7f65a8` |
| `RsCommonSender.setRightTouchThree` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 13 0` | `0x7f5f90` |
| `RsCommonSender.setRightTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`FF 00 03 01 12 0` | `0x7f6230` |
| `RsCommonSender.setSpatialAudioModeHead` | 委托：`RsCommonSender.setSpatialAudioSwitchOn` | `0x946054` |
| `RsCommonSender.setSpatialAudioModeOff` | 委托：`RsCommonSender.setSpatialAudioSwitchOff` | `0x924ea8` |
| `RsCommonSender.setTransLevel` | 参数化 / 辅助逻辑 | `0xadc4d0` |
| `RsCommonV2Sender._setTapKey` | 参数化 / 辅助逻辑 | `0x8238d8` |
| `RsCommonV2Sender._setTouchKey` | 参数化 / 辅助逻辑 | `0x7f603c` |
| `RsCommonV2Sender.getTouch` | 空/共享入口，未确认命令字节 | `0xf8dae0` |
| `RsCommonV2Sender.intToEqList` | 参数化 / 辅助逻辑 | `0x9851b0` |
| `RsCommonV2Sender.setAiPageState` | 参数化 / 辅助逻辑 | `0xac4b10` |
| `RsCommonV2Sender.setAncLevel` | 参数化 / 辅助逻辑 | `0xad0dd0` |
| `RsCommonV2Sender.setAncLine` | 参数化 / 辅助逻辑 | `0x7f2e1c` |
| `RsCommonV2Sender.setAudioMulDevice` | 委托：`RsCommonV2Sender.setConnectMulOn` | `0x815cf0` |
| `RsCommonV2Sender.setDingLevel` | 参数化 / 辅助逻辑 | `0x7f8698` |
| `RsCommonV2Sender.setEqBassPlus` | 参数化 / 辅助逻辑 | `0x989ac8` |
| `RsCommonV2Sender.setEqCustomData` | 委托：`RsCommonV2Sender.intToEqList` | `0x9850e4` |
| `RsCommonV2Sender.setFindDevice` | 参数化 / 辅助逻辑 | `0xf5f3d0` |
| `RsCommonV2Sender.setFindDeviceBoth` | 委托：`RsCommonV2Sender.setFindDevice` | `0xf74b18` |
| `RsCommonV2Sender.setFindDeviceLeft` | 委托：`RsCommonV2Sender.setFindDevice` | `0xf5f3a8` |
| `RsCommonV2Sender.setFindDeviceRight` | 委托：`RsCommonV2Sender.setFindDevice` | `0xf5f568` |
| `RsCommonV2Sender.setGainLevel` | 参数化 / 辅助逻辑 | `0x7fc370` |
| `RsCommonV2Sender.setHearingProtection` | 参数化 / 辅助逻辑 | `0x7fc4bc` |
| `RsCommonV2Sender.setLeftTapThree` | 委托：`RsCommonV2Sender._setTapKey` | `0x8fe0bc` |
| `RsCommonV2Sender.setLeftTapTwo` | 委托：`RsCommonV2Sender._setTapKey` | `0x8238ac` |
| `RsCommonV2Sender.setLeftTouchLong` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f6b50` |
| `RsCommonV2Sender.setLeftTouchOne` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7fa260` |
| `RsCommonV2Sender.setLeftTouchThree` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f6ffc` |
| `RsCommonV2Sender.setLeftTouchTwo` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f71ac` |
| `RsCommonV2Sender.setRightTapThree` | 委托：`RsCommonV2Sender._setTapKey` | `0x912ee4` |
| `RsCommonV2Sender.setRightTapTwo` | 委托：`RsCommonV2Sender._setTapKey` | `0x907c1c` |
| `RsCommonV2Sender.setRightTouchLong` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f67d8` |
| `RsCommonV2Sender.setRightTouchOne` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f6628` |
| `RsCommonV2Sender.setRightTouchThree` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f6010` |
| `RsCommonV2Sender.setRightTouchTwo` | 委托：`RsCommonV2Sender._setTouchKey` | `0x7f62b0` |
| `RsCommonV2Sender.setStarburstSdk` | 参数化 / 辅助逻辑 | `0xa41150` |
| `RsCommonV2Sender.setTime` | 委托：`RsCommonV2Sender.timeCommand` | `0xa3b17c` |
| `RsCommonV2Sender.setTransLevel` | 参数化 / 辅助逻辑 | `0xadc57c` |
| `RsCommonV2Sender.timeCommand` | 参数化 / 辅助逻辑 | `0x924ae4` |
| `RsCommonV2Sender.transform` | 参数化 / 辅助逻辑 | `0xf9e4f8` |
| `Zt219Sender.setNoiseModeWind` | 委托：`Zt219Sender.setNoiseModeNormal` | `0x8e4de0` |
| `Zt229Sender.setAncLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 06 87 0b 00` | `0xad0e5c` |
| `Zt229Sender.setDingLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 83 0b 00 ` | `0x7f87ec` |
| `Zt229Sender.setFindDevice` | 空/共享入口，未确认命令字节 | `0x7c4974` |
| `Zt229Sender.setFindOff` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 82 0c 00 03 00` | `0x824dc4` |
| `Zt229Sender.setTransLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 06 84 0b 00` | `0xadc608` |
| `Zt231Sender._buildEqCommand` | 参数化 / 辅助逻辑 | `0x7f8b04` |
| `Zt231Sender.getCustomEq` | 委托：`Zt231Sender._buildEqCommand` | `0x986294` |
| `Zt231Sender.getGainLevel` | 委托：`Zt231Sender._buildEqCommand` | `0x7f8ad4` |
| `Zt231Sender.setDingLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 03 0b 00 ` | `0x7f88b8` |
| `Zt231Sender.setEqCustom` | 委托：`Zt231Sender._buildEqCommand` | `0x988ca0` |
| `Zt231Sender.setEqCustomData` | 委托：`Zt231Sender._buildEqCommand` | `0x985418` |
| `Zt231Sender.setFindOff` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 02 0c 00 03 00` | `0x824e10` |
| `Zt231Sender.setGainLevel` | 委托：`Zt231Sender._buildEqCommand` | `0x7fc3fc` |
| `ZtSender.getConnectMul` | 委托：`Zt231Sender.getLoseLoss` | `0x8f446c` |
| `ZtSender.setAncLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 06 87 0b 00` | `0xad0f38` |
| `ZtSender.setAncLine` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 06 85 0E 00 ` | `0x7f3044` |
| `ZtSender.setAudioAacSbc` | 委托：`ZtSender.setConnectMulOn` | `0x8a22b0` |
| `ZtSender.setConnectMulOff` | 委托：`Zt231Sender.setAudioLdac` | `0xf5b034` |
| `ZtSender.setDingLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 03 0b 00 ` | `0x7f8724` |
| `ZtSender.setFindDevice` | 空/共享入口，未确认命令字节 | `0x7c4974` |
| `ZtSender.setFindDeviceBoth` | 委托：`ZtSender.setFindDeviceLeft` | `0xf74b40` |
| `ZtSender.setFindDeviceLeft` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 82 0c 00 ` / `00 01` / `08 EE 00 00 00 0E 02 0c 00 ` | `0xf5f44c` |
| `ZtSender.setFindDeviceRight` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 0E 82 0c 00 ` / `01 01` / `08 EE 00 00 00 0E 02 0c 00 ` | `0xf5f590` |
| `ZtSender.setLeftTouchLong` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 00 03 0` | `0x7f6b7c` |
| `ZtSender.setLeftTouchOne` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 00 00 0` | `0x7fa28c` |
| `ZtSender.setLeftTouchThree` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 00 02 0` | `0x7f7028` |
| `ZtSender.setLeftTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 00 01 0` | `0x7f71d8` |
| `ZtSender.setRightTouchLong` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 01 03 0` | `0x7f6804` |
| `ZtSender.setRightTouchOne` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 01 00 0` | `0x7f6654` |
| `ZtSender.setRightTouchThree` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 01 02 0` | `0x7f612c` |
| `ZtSender.setRightTouchTwo` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 04 81 0D 00 01 01 0` | `0x7f62dc` |
| `ZtSender.setTransLevel` | 参数化 / 辅助逻辑；字符串片段：`08 EE 00 00 00 06 84 0b 00` | `0xadc6e4` |


## 10. 初始化流程、OTA 与实现边界

### 初始化读取（S）

下表是型号初始化函数中的读取调用顺序；其他型号可能继承已有方法，没有单独列出不表示没有初始化。查询名应通过正确发送器解析。部分 V2 路径还根据界面功能选择查询，并非始终发送一份通用列表。

| 型号配置 | 读取顺序 | ELF |
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

### OTA 与尚未还原的部分（P）

第 2 节 OTA UUID 和转发配置只标识传输接口。固件文件封装、型号/芯片校验、Flash 地址、擦写指令、分块大小、应答、签名/校验规则、恢复和重启握手尚未完整解码；因此本文不是完整的刷机协议规范。

其他未完整还原部分包括 ZT 数字派生器的精确型号绑定、多设备名称编码、全部按键动作枚举、V2 日期/时区序列化、自定义 EQ 转换、ANC 列表参数、AI 子命令、Opus/星芒载荷、AURAMIC HID 帧、所有 MIC 字段枚举和错误重试时序。方法/类型索引保留这些入口，不虚构字节。

### 客户端实现要点

- 先选择准确型号/芯片配置，再选择协议和发送器覆盖逻辑。
- 先开启接收再查询，允许设备主动推送状态。
- 同时处理分片和粘包，索引前校验字段边界。
- 分开处理 V2 整帧校验、LTV 长度、ACK 和历史兼容例外。
- 通过实际状态回读确认设置；蓝牙写入完成只表示本地传输提交。
- 同时记录原始帧、解析值与时间戳，不将盒电量缓存当作新测量。
- 界面功能与发送器方法存在不代表硬件一定支持；本文只有 OpenFun 电量读取有实机证据。

## 11. 证据定位与后续验证

二进制：`data/app.so`（ELF x86-64，Dart AOT）。SHA-256：

```text
b997646e1e1dc51e7271aba5fb989f6e5439f0000eaba3152c842422665072c2
```

以下为该 ELF 内的代码位置，不是稳定 API 标识，也不是进程运行时内存地址；升级版本后可能变化。

| 证据入口 | ELF 地址 |
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

本版核对：48 个配置入口归并为 46 个具体型号配置和 2 个模板；274 个发送器方法均有记录；166 条完整常量数组核对数组长度；离线核对 RS 长度、普通 ZT 长度/加和校验及 V2 示例校验，执行文中 Python 示例。9 字节 ZT219 异常保持显式标记，不据此扩大实机验证范围。
