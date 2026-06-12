# 硬件

Sesame Robot 的所有机械和电子源文件位于此处。硬件支持多种构建方案：

- **手工接线 / Lolin S2 Mini 方案**适用于简单的万用板搭建（推荐 DIY 构建者使用）。
- **Sesame Distro Board V3 定制方案**（当前版本）专业 SMD 设计，支持 USB-C PD 12V 和 Bambu Lab 电池专用接口（新的 Sesame 构建套件中包含，已预刷固件）。
- **Sesame Distro Board V2 定制方案**（旧版）专业 SMD 设计，但在电池供电时容易出现电压骤降问题。推荐仅用于 USB 供电，或绕过降压转换器使用。
- **Sesame Distro Board V1 定制方案**（旧版）用于 ESP32-DevKitC-32E 堆叠（已逐步淘汰但仍受支持）。

使用以下部分跳转到与您要组装的版本匹配的文件。

## 目录指南

| 文件夹 | 内容说明 |
| --- | --- |
| [bom](bom/README.md) | 涵盖两种接线方案的完整物料清单，以及功耗预算说明和构建教程链接。在打印或焊接之前从这里开始，收集所有组件。 |
| [cad](cad/README.md) | 每个打印零件的参数化 STEP 和 Fusion 360 源模型。适合对关节几何结构进行二次创作或修改外壳——请注意关于某些特征可能无法在不同 CAD 软件之间转换的提醒。 |
| [pcb](pcb/README.md) | Sesame Distro Board V3（当前）、V2（旧版）和 V1（旧版）的原理图、布局文件、Gerber 文件、BOM 和贴片坐标文件。包含通过 PCBway 进行制造和组装服务的订购说明。 |
| [printing](printing/README.md) | 实际的 PLA 打印设置、支撑说明以及关节方向和顶盖手动支撑的图片参考。 |

## 入门指南

1. **确定接线方案。** 查看 [docs/wiring-guide/README.md](../docs/wiring-guide/README.md) 中的对比部分，在万用板和定制 PCB 方案之间做出选择。推荐 DIY 构建者使用 Lolin S2 Mini 方案。如果您有 Sesame 构建套件，您的 V3（或早期 V2）Distro Board 已经预刷并可直接使用。
2. **打印外壳和关节。** 按照 [printing/README.md](printing/README.md) 中的预设参数，准备 `hardware/printing/stl` 中的 3D 打印文件。
3. **采购电子元件。** 使用 [bom/README.md](bom/README.md) 中的逐项清单，并验证您的电源是否满足 5V/3A 的要求。
4. **组装电子元件。** 参考手工接线步骤或相应的 Distro Board 指南，以及 `docs/wiring-guide/` 中的接线图。

> [!TIP]
> 保留接线过程中的照片。这对后续的故障排查非常宝贵，也有助于在社区中分享构建笔记。

## 参考文档

- [docs/build-guide/README.md](../docs/build-guide/README.md) — 端到端装配流程。
- [docs/wiring-guide/README.md](../docs/wiring-guide/README.md) — 接线图、安全注意事项和布线技巧。

欢迎提交改进、二次创作和测试报告。如果您在构建过程中有任何发现，请提交 issue 或 PR。<3
