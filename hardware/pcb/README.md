# PCB 原理图

Sesame Robot 项目的电子原理图和 PCB 设计。Sesame Distro Board 有三个可选版本：V3（当前）、V2（旧版）和 V1（旧版）。

> [!NOTE]
> **Sesame 构建套件用户须知：** 所有 Sesame 构建套件均包含已预刷固件的 Sesame Distro Board V3（或早期 V2），因此您无需单独订购或组装电路板。

> [!TIP]
> **从零开始构建？** 如果您从零开始构建 Sesame Robot，我们推荐使用 **S2 Mini 配合手工接线方案**以获得最简单的组装体验。V2/V3 分电板使用先进的 SMD 元件，需要专业的焊接技能或 PCB 组装服务。

---

## Sesame Distro Board V3（当前版本）

**Sesame Distro Board V3** 是最新版本，具有以下特性：

- **ESP32-S3 处理器**
- **USB-C PD 12V**（需要高质量的 PD 充电线）
- **Bambu Lab 14500 电池接口**，用于经济高效的电池供电（优先特性）
- 通过 PCBway 一键订购链接：[在 PCBway 订购！](https://www.pcbway.com/project/shareproject/Sesame_Distro_Board_V3_377de6fe.html)
- 预组装单元限量供应：[Full Contact Engineering](https://fullcontactengineering.com/products/sesame-distro-board-v3-pcb)
  *注意：由于时间有限，USB-C PD 12V 协商芯片尚未经过充分测试，因此需要高质量的 PD 线缆。*

### V3 电路板详情和原理图：

<img src="distro-v3/assets/close-v3.png" alt="Sesame Distro Board V3 Close-up" width="70%">

<img src="distro-v3/assets/Schematic_Sesame-Distro-Board-V3_2026-05-30.png" alt="Schematic_Sesame-Distro-Board-V3" width="70%">

<img src="distro-v3/assets/layout-v3.png" alt="Sesame Distro Board V3 Layout" width="70%">

### V3 可用文件

所有文件位于 [`distro-v3/`](distro-v3/) 目录：

- **原理图源文件：** `SCH_Sesame-Distro-Board-V3.json` - V3 原理图的 EasyEDA 源设计文件
- **Gerber 文件：** `Gerber_Sesame-Distro-Board-V3_PCB.zip` - 用于 PCB 制造
- **BOM 文件：** `BOM_Sesame-Distro-Board-V3.csv` - SMD 元件物料清单
- **贴片坐标文件：** `PickAndPlace_PCB_Sesame-Distro-Board-V3.csv` - 组装用元件位置数据

## Sesame Distro Board V2（旧版）

**Sesame Distro Board V2** 是较旧的版本。*电池供电时存在严重不稳定性（由于降压转换器处理问题导致电压骤降），仅限 USB 供电使用，或者必须通过将单独的降压转换器直接焊接到电源线来绕过该芯片。我可能有额外的 V3 板可以寄给订购了 V2 的用户。*

### V2 组装选项（旧版）

V2 板完全由 SMD（表面贴装）元件组成，**手工焊接难度很高**。我们推荐：

1. **PCB 组装服务（推荐）：** 使用 [PCBway 的 PCB 组装服务](https://www.pcbway.com/pcb-assembly.html)让电路板得到专业组装。将 Gerber、BOM 和贴片坐标文件上传到他们的组装服务。
2. **手工焊接（仅限高级用户）：** 只有在您有 SMD 元件焊接经验且拥有适当工具（热风枪、细尖烙铁、助焊剂等）时才尝试手工焊接。

### V2 可用文件

所有文件位于 [`distro-v2/`](distro-v2/) 目录：

- **原理图源文件：** `SCH_Sesame-Distro-Board-V2_2026-03-06.json` - V2 原理图的 EasyEDA 源设计文件
- **Gerber 文件：** `Gerber_Sesame-Distro-V2_PCB.zip` - 用于 PCB 制造
- **BOM 文件：** `BOM_Sesame-Distro-V2.csv` - SMD 元件物料清单
- **贴片坐标文件：** `PickAndPlace_PCB_Sesame-Distro-V2.csv` - 组装用元件位置数据

### PCBway / JLCPCB 采购注意事项

将 BOM 上传到 PCBway 或 JLCPCB 进行组装时，请注意以下三个需要特殊处理的元件：

| 位号 | 问题 | 替换零件 | 立创编号 |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- | -------------------------------------------------------- |
| `U5`、`U7` | **焊盘**——这些仅是铜焊盘，不需要物理元件。订购组装时请从 BOM 中移除这些行。 | 无 | 无 |
| `5-12V` | 2 针 3.5 mm 螺丝端子。原始零件 `1984617`（Phoenix Contact）无法采购。 | `XY302V-3.5-2P` | [C784940](https://www.lcsc.com/product-detail/C784940.html) |
| `JST-XH` | 4 针 JST XH 连接器。原始 `JST-XH-4-PIN` 无法采购。 | `B4B-XH-A(LF)(SN)` | [C144395](https://www.lcsc.com/product-detail/C144395.html) |

BOM CSV 文件已更新为替换后的零件。焊盘行在名称字段中标记为 **"No Part Required"**，便于识别和排除。

---

## Sesame Distro Board V1（旧版）

> [!CAUTION]
> Sesame Distro Board V1 现已**停止更新**但仍受支持。V1 有一些局限性：无法通过 USB-C 等有线供电运行，组装难度也高于其他方案。V1 仍然有接线指南和固件支持，可以在电池供电下工作。如果您已经拥有 V1 板，仍然可以成功使用它。

Distro Board V1 与 ESP32-DevKitC-32E 搭配使用。

> [!IMPORTANT]
> **ESP32 排针要求：** Distro Board V1 堆叠在 ESP32-DevKitC-32E 上方，因此您需要一个**未预焊排针**的 ESP32 板。如果您的板子顶部已经焊接了排针，您需要将所有排针解焊并翻转到 ESP32 板的底部，以便 Distro Board 能够安装在上面。

### V1 电路板原理图：

<img src="distro-v1/Schematic_Sesame-Distro-Board.png" alt="Schematic_Sesame-Distro-Board" width="70%">

### V1 可用文件

所有文件位于 [`distro-v1/`](distro-v1/) 目录。

---

## PCBway 赞助

**PCBway 赞助**

本项目由 [PCBway](https://www.pcbway.com/) 赞助，他们制造了定制的分电板。PCBway 提供高质量的 PCB 制造服务，交货迅速，客户支持出色。

如果您正在构建自己的 Sesame Robot，PCBway 是以合理价格获得专业品质 PCB 制造和组装服务的绝佳选择：

- **PCB 制造：** 上传 Gerber 文件即可获得制造好的电路板
- **PCB 组装：** 上传 Gerber、BOM 和贴片坐标文件以获得完全组装好的电路板（推荐用于 V2）

<img src="pcbs.png" alt="pcbs-from-pcbway" width="70%">

---

## 订购方式

### 订购 V3 电路板（当前版本）

**方案 1：预组装单元（推荐）**
根据库存情况，您可以直接从 [Full Contact Engineering](https://fullcontactengineering.com/products/sesame-distro-board-v3-pcb) 购买已完全贴片并预刷固件的电路板。

**方案 2：PCBway 组装服务**

1. 访问 [PCBway 共享项目页面](https://www.pcbway.com/project/shareproject/Sesame_Distro_Board_V3_377de6fe.html)
2. 加入购物车即可订购完全组装的电路板！

**方案 3：手动 PCBway 组装服务上传**

1. 访问 [PCBway 的 PCB 组装服务](https://www.pcbway.com/pcb-assembly.html)
2. 上传 Gerber 文件：`distro-v3/Gerber_Sesame-Distro-Board-V3_PCB.zip`
3. 上传 BOM 文件：`distro-v3/BOM_Sesame-Distro-Board-V3.csv`
4. 上传贴片坐标文件：`distro-v3/PickAndPlace_PCB_Sesame-Distro-Board-V3.csv`
5. 确认电路板规格和元件可用性

### 订购 V2 电路板（已弃用）

**方案 1：PCB 组装服务（推荐）**

1. 访问 [PCBway 的 PCB 组装服务](https://www.pcbway.com/pcb-assembly.html)
2. 上传 Gerber 文件：`distro-v2/Gerber_Sesame-Distro-V2_PCB.zip`
3. 上传 BOM 文件：`distro-v2/BOM_Sesame-Distro-V2.csv`
4. 上传贴片坐标文件：`distro-v2/PickAndPlace_PCB_Sesame-Distro-V2.csv`
5. 确认电路板规格和元件可用性
6. 下单即可收到完全组装的电路板

**方案 2：仅制造（适合高级用户）**

1. 从 [`distro-v2/Gerber_Sesame-Distro-V2_PCB.zip`](distro-v2/Gerber_Sesame-Distro-V2_PCB.zip) 下载 Gerber 文件
2. 上传到 [PCBway](https://www.pcbway.com/) 或其他 PCB 制造商
3. 在预览中确认电路板规格
4. 订购裸板并自行手工焊接 SMD 元件（高级）

### 订购 V1 电路板（旧版）

1. 从 [`distro-v1/Gerber_Sesame-Distro-Board_PCB_Sesame-Distro-Board_V1.zip`](distro-v1/Gerber_Sesame-Distro-Board_PCB_Sesame-Distro-Board_V1.zip) 下载 Gerber 文件
2. 上传到 [PCBway](https://www.pcbway.com/) 或其他 PCB 制造商
3. 在预览中确认电路板规格
4. 下单即可收到 V1 裸板
