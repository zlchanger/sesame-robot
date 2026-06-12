# 物料清单

组装 Sesame 所需的每个零件都在此处列出。选择适合您零件库存的接线方案，然后按照 [docs/build-guide/README.md](../../docs/build-guide/README.md) 中的构建流程进行操作。

> [!NOTE]
> 下方的 Amazon 链接指向代表性的搜索结果，以便您选择本地供应商或等效商品。价格和库存情况经常变动。如果您不介意等待运输时间，也可以直接从制造商订购，价格会低得多。


## 核心电子元件（两种方案通用）

| 物品 | 数量 | 备注 | 来源 |
| --- | --- | --- | --- |
| MG90S 全金属微型舵机（180度） | 8（建议买10个备用） | 主要的髋/腿执行器；附带舵机臂，但建议保留备用件 | [Amazon](https://www.amazon.com/s?k=mg90s+metal+gear+servo+pack+of+8) |
| 0.96" SSD1306 I2C OLED | 1 | 128x64 显示屏，滑入顶盖槽位 | [Amazon](https://www.amazon.com/s?k=0.96%22+I2C+OLED+SSD1306) |
| USB-C 数据/电源线 | 1 | 需要承载 5V/3A，用于刷写固件和有线供电模式 | [Amazon](https://www.amazon.com/s?k=usb+c+cable+60w) |
| 船型电源开关（KCD1，面板安装） | 1 | 卡入顶盖切口中 | [Amazon](https://www.amazon.com/s?k=KCD1+mini+rocker+switch+2+pin) |
| 22AWG 硅胶线套装 | 1 | 电源/地线总线 | [Amazon](https://www.amazon.com/s?k=22awg+silicone+wire+kit) |
| 30AWG 硅胶线套装 | 1 | 信号线和密集布线 | [Amazon](https://www.amazon.com/s?k=30awg+silicone+wire) |
| 热缩管套装 | 1 | 用于 OLED、开关和电池接头的绝缘 | [Amazon](https://www.amazon.com/s?k=heat+shrink+tubing+kit) |
| 小扎带 | 1 包 | 框架内部线束整理 | [Amazon](https://www.amazon.com/s?k=mini+zip+ties) |

## 接线方案 A – S2 Mini / 手工线束

| 物品 | 数量 | 备注 | 来源 |
| --- | --- | --- | --- |
| Lolin/WeMos ESP32-S2 Mini | 1 | 原生 USB-C，适合安装在万用板上用于手工接线方案 | [Amazon](https://www.amazon.com/s?k=esp32+s2+mini) |
| 小型万用板（约 5×7 cm） | 1 | 用于放置排针矩阵和电源导轨 | [Amazon](https://www.amazon.com/s?k=prototype+perfboard) |
| 3 针公排针 | 8 | 搭建舵机接口板；间距需与 MG90 插头匹配 | [Amazon](https://www.amazon.com/s?k=pin+header+strip) |
| 降压转换器（5–12 V 输入，稳定 5V/3A 输出） | 1 | 使用电池时为电机 + MCU 供电 | [Amazon](https://www.amazon.com/s?k=3a+dc+dc+buck+converter+module) |

## 接线方案 B – Sesame Distro Board V3/V2（构建套件中包含）

> [!NOTE]
> 如果您购买了 Sesame 构建套件，您的 V2/V3 Distro Board 已经组装好、预刷固件并包含在内。您不需要单独订购这些零件。

| 物品 | 数量 | 备注 | 来源 |
| --- | --- | --- | --- |
| Sesame Distro Board V3/V2 PCB | 1 | 全 SMD 设计。使用 PCBway 组装服务订购或尝试高级手工焊接。参见[订购指南](/hardware/pcb/README.md) | [GitHub](/hardware/pcb/README.md) |

## 接线方案 C – Sesame Distro Board V1 / ESP32-DevKitC-32E（旧版）

> [!CAUTION]
> V1 现已逐步淘汰但仍受支持。仅当您已拥有 V1 板时才选择此方案。

| 物品 | 数量 | 备注 | 来源 |
| --- | --- | --- | --- |
| ESP32-DevKitC-32E (ESP32-WROOM-32) | 1 | Distro Board V1 堆叠在其上的基础板。这个选择有些棘手，因为需要非常特定的板型。您可以使用带浮动 PCB 天线的 32E，也可以使用 32U，但需要在内部布置天线。 | [Amazon](https://www.amazon.com/s?k=ESP32+DevKitC+32) |
| Sesame Distro Board V1 PCB | 1 | 通过 PCBway 订购 `Gerber_Sesame-Distro-Board_PCB_Sesame-Distro-Board_V1.zip` | [GitHub](/hardware/pcb/README.md) |
| 5V 降压转换器（规格同上） | 1 | 安装在分电板焊盘上 | [Amazon](https://www.amazon.com/s?k=3a+dc+dc+buck+converter+module) |
| 1000 µF 电解电容 | 1 | 平滑降压转换器输出电压；建议额定电压 10V 以上 | [Amazon](https://www.amazon.com/s?k=1000uf+electrolytic+capacitor) |
| 4 针 JST-XH 或 PH 排针 | 1 | PCB 上可选的外部连接器接口 | [Amazon](https://www.amazon.com/s?k=jst+xh+4+pin+kit) |
| 2 针螺丝端子（2.54 mm 间距） | 1 | PCB 上可选电池输入 | [Amazon](https://www.amazon.com/s?k=2+pin+screw+terminal+block+2.54mm+pitch) |
| M2.5 × 5 mm 公母螺柱 | 4 | 将 PCB 抬高到 DevKit 安装孔上方 | [Amazon](https://www.amazon.com/s?k=m2.5+male+female+standoff+5mm) |

## 电源与连接器

| 物品 | 数量 | 备注 | 来源 |
| --- | --- | --- | --- |
| Bambu Lab 14500 7.4V 800mAh 锂离子电池 | 1 | 推荐的无线电池组；便宜、有效，专为配合新内部框架设计。 | [Bambu Lab](https://us.store.bambulab.com/products/14500-7-4v-800mah-li-ion-battery-1pcs) |
| Bambu Lab 7.4V 锂电池充电器 | 1 | 14500 电池配套充电器，带 XH2.54 连接器。 | [Bambu Lab](https://us.store.bambulab.com/products/7-4v-lithium-battery-charger-with-xh2-54-connector-1pcs?id=593290727051776002) |
| XH2.54 母头尾线 | 1 | 用于将电池连接到开关/PCB，无需剪断原装引线（V3 需要焊接）。 | [Amazon](https://www.amazon.com/s?k=xh2.54+pigtail+cable) |

## 紧固件与机械硬件

| 物品 | 数量 | 用途 | Amazon |
| --- | --- | --- | --- |
| M2 × 5 mm 自攻螺丝 | 约 40 个 | 所有塑料关节、OLED 固定、电机安装和盖板（可以直接买混合套装） | [Amazon](https://www.amazon.com/s?k=m2+self+tapping+screws+kit) |
| M2.5 × 5mm 机牙螺丝 | 10 个 | 仅用于将舵机臂固定到舵机轴上。舵机自带的舵机臂螺丝通常太短。 | [Amazon](https://www.amazon.com/s?k=m2+machine+screw+kit) |

## 3D 打印零件

打印 [printing/README.md](../printing/README.md) 中列出的 11 件零件套装。STL 和 CAD 源文件位于 `hardware/printing/` 目录下。

## 耗材和工具清单

| 物品 | 备注 | 来源 |
| --- | --- | --- |
| 含铅焊锡（0.6–0.8 mm） | 用于密集万用板作业，流动性更好 | [Amazon](https://www.amazon.com/s?k=63%2F37+solder+0.8mm) |
| 助焊剂笔 | 保护万用板和 PCB 上的焊盘 | [Amazon](https://www.amazon.com/s?k=flux+pen) |
| 吸锡带 / 吸锡器 | 用于 OLED 引脚的返工 | [Amazon](https://www.amazon.com/s?k=solder+wick) |
| 小型平口剪钳 | 修剪舵机引线、万用板走线或支撑结构 | [Amazon](https://www.amazon.com/s?k=flush+cutters) |
| 精密螺丝刀套装 | M2 自攻螺丝所需 | [Amazon](https://www.amazon.com/s?k=precision+screwdriver+set) |

## 电源与安全注意事项

- Sesame 需要至少 5 V 3 A 的电源供应到电源导轨。
- **Lolin S2 Mini：** 可通过 USB-C PD（需要支持 5V/3A）进行有线供电，或通过电池 + 降压转换器供电。
- **Distro Board V3/V2：** 支持 USB-C PD（5V/3A）有线供电和电池 + 降压转换器供电。所有 Sesame 构建套件中均包含。
- **Distro Board V1（旧版）：** 由于设计限制，无法通过 USB-C 有线供电运行。必须使用电池 + 降压转换器供电。
- 无论哪种方案使用电池供电，都应将电池组通过船型开关和降压转换器后再接入电源导轨，参考 [docs/wiring-guide/README.md](../../docs/wiring-guide/README.md) 中的原理图。
- **切勿剪断电池组上的原厂连接器。** 相反，应使用 XT30 或 JST RCY 引线制作转接尾线，以便电池组仍可充电。
- Bambu Lab 14500 7.4V 800mAh 锂离子电池适合新的标准电池仓（V3 需要打印新的内部框架）。
- **在所有连接器上务必使用热缩管。** 切割和焊接电池连接器时务必极度小心。如果您仍在使用旧版 10440 方案，请确保在焊接时取出电池。
