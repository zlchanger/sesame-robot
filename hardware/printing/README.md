# 3D 打印指南

Sesame 设计用于 **PLA** 材料打印。大多数零件无需支撑即可打印，但顶盖需要在特定区域添加支撑。以下是每个组件的快速参考：

注意：目前有 3 种不同的顶盖样式可供选择。**推荐使用 Enclosed v91**，因为它是最现代的设计，带有磁吸帽子底座、隐藏的显示屏走线和多色细节支持。详情请参见[顶盖](stl/top-covers/)。

> [!NOTE]
> **电池升级：** 内部框架设计已稍作调整，以适应新的推荐电池（Bambu Lab 14500 锂离子电池）。如果您有旧版 Sesame 并想升级到此电池，只需打印新的内部框架并安装即可。

## 推荐设置

* **材料：** PLA / PLA+
* **填充率：** 8-10%
* **壁层数：** 2
* **填充图案：** 蜂窝

## 3D 打印组件支撑指南

| 组件 | 是否需要支撑 | STL 链接 |
| -------------- | ----------------- | --------------------------------------------------- |
| 关节 R1 | 否 | [R1-v117.stl](stl/R1-v117.stl) |
| 关节 R2 | 否 | [R2-v117.stl](stl/R2-v117.stl) |
| 关节 R3 | 否 | [R3-v117.stl](stl/R3-v117.stl) |
| 关节 R4 | 否 | [R4-v117.stl](stl/R4-v117.stl) |
| 关节 L1 | 否 | [L1-v117.stl](stl/L1-v117.stl) |
| 关节 L2 | 否 | [L2-v117.stl](stl/L2-v117.stl) |
| 关节 L3 | 否 | [L3-v117.stl](stl/L3-v117.stl) |
| 关节 L4 | 否 | [L4-v117.stl](stl/L4-v117.stl) |
| 内部框架 | 否 | [Internal-Frame-v121.stl](stl/Internal-Frame-v121.stl) |
| 底盖 | 否 | [Bottom-Cover-v121.stl](stl/Bottom-Cover-v121.stl) |
| 顶盖 | 是 | [Top Covers](stl/top-covers/) |

### 顶盖设置（原始样式）

底边：仅外底边
支撑类型：常规（手动）

手动支撑位置：

<img src="assets/topcover-supports2.png" alt="topcover-supports2" width="70%">

切片后的零件效果：

<img src="assets/sliced-topcover.png" alt="sliced-topcover" width="70%">

### 关节推荐打印方向：

使用自动定向工具将自动放置正确的方向。

<img src="assets/joints-orientation.png" alt="sliced-topcover" width="70%">
