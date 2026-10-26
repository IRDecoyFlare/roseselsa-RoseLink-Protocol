# roseselsa RoseLink Protocol / 弱水时砂RoseLink 通信协议

Reverse-engineered communication protocol reference for **roseselsa** devices, based on **Windows RoseLink 3.8.2+5409**.

针对 **roseselsa** 厂商设备，基于 **Windows RoseLink 3.8.2+5409** 逆向整理的通信协议参考。

## Documentation / 协议文档

- [简体中文](RoseLink_Communication_Protocol_zh-CN.md)
- [English](RoseLink_Communication_Protocol_en.md)

Document version: **1.0** · Compiled: **2026-10-22**.

文档版本：**1.0** · 整理日期：**2026-10-22**。

## Scope / 覆盖范围

The reference records **46 concrete model profiles** and **2 generic templates** found in the analyzed application version. It covers Bluetooth SPP/RFCOMM and BLE configuration, framing, battery decoding, fixed and parameterized commands, initialization, and known OTA boundaries.

文档记录该应用版本中的 **46 个具体型号配置**及 **2 个通用模板**，包含蓝牙 SPP/RFCOMM 与 BLE 配置、帧格式、电量解析、固定与动态命令、初始化流程及已知 OTA 边界。

| Protocol family / 协议族 | Model profiles / 型号配置数 |
| --- | ---: |
| F8 | 6 |
| RS Common | 7 |
| RS Common V2 | 30 |
| ZT | 3 |

## Evidence / 证据等级

| Level / 等级 | Meaning / 含义 |
| --- | --- |
| **H** | OpenFun hardware transaction / OpenFun 实机收发验证 |
| **S** | Recovered program constants or control flow / 程序常量或控制流程直接确认 |
| **P** | Partially decoded; fields or behavior remain unconfirmed / 部分结构已知，字段或行为仍待确认 |

Except for OpenFun battery communication explicitly marked **H**, model behavior and control commands have not been tested individually. Shared protocol families do not establish support for every command on every device. “All model profiles” refers to the analyzed application version, not all past or future products.

除明确标 **H** 的 OpenFun 电量通信外，其他型号行为和控制命令未逐台实测。同一协议族不代表每款设备支持所有命令。“全部型号配置”仅指本次分析的应用版本，不代表全部历史或未来产品。

## Contributing / 补充与勘误

Please open an issue or pull request with the model and chip variant, firmware and application versions, transport, raw TX/RX bytes, expected and observed behavior, and evidence level. Redact Bluetooth addresses, serial numbers, and other personal identifiers from captures.

欢迎通过 Issue 或 Pull Request 补充实测与勘误。请提供型号及芯片变体、固件与应用版本、传输方式、原始收发字节、预期与实际行为和证据等级；分享抓包前请隐去蓝牙地址、序列号等个人标识。

This is an independent reverse-engineering reference, not an official roseselsa specification. See the protocol documents for implementation limits and unconfirmed areas.

本项目为独立逆向工程参考，并非 roseselsa 官方协议规范。实现边界与未确认部分请参阅协议正文。
