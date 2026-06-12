# Sesame Robot 项目
___
![License](https://img.shields.io/badge/License-APACHE2.0-yellow)
![Microcontroller](https://img.shields.io/badge/Microcontroller-ESP32-blue)
![Firmware](https://img.shields.io/badge/Firmware-C%2B%2B-blue?logo=c%2B%2B)
![IDE](https://img.shields.io/badge/IDE-Arduino-00979D?logo=arduino&logoColor=white)
![GitHub stars](https://img.shields.io/github/stars/dorianborian/sesame-robot?style=social)
![GitHub forks](https://img.shields.io/github/forks/dorianborian/sesame-robot?style=social)

<img width="100%" height="728" alt="sesame-cover" src="https://github.com/user-attachments/assets/f0cc6ad0-135b-4515-8750-900f224ed7ae" />

<p align="center">
  <a href="https://www.youtube.com/watch?v=NIgoQVQF_Ng">
    <img src="https://github.com/user-attachments/assets/1663e022-0680-4053-97b4-53e669a6f07d" width="49%" alt="tutorial-button">
  </a>
  <a href="https://discord.gg/XDXkhQd8bC">
    <img src="https://github.com/user-attachments/assets/378fcb48-5b12-4b46-9dcb-452432d49913" width="49%" alt="discord-button">
  </a>
</p>

___

**你好，来自你的新朋友。**

Sesame 是一个基于 ESP32 微控制器系统的开源机器人项目，强调表情和运动能力。
本项目面向各种技能水平的创客和工程师！Sesame 提供了一个动感十足的入门平台，让您开始接触行走机器人。
要构建一个 Sesame 机器人，您需要具备基本的焊接技能、约 50-60 美元的硬件组件、一台 3D 打印机以及对 Arduino IDE 的基本了解。

本仓库包含 CAD 设计文件、STL 文件、构建和接线指南，以及适用于 ESP32 控制器的基础/扩展固件。
还包含一些调试固件，可能有助于让您的 Sesame 顺利运行起来。

## 特性

*   **四足设计：** 使用 8 个舵机（每条腿 2 个），实现约 8 个总自由度。
*   **表情显示屏：** 配备 128x64 OLED 屏幕作为反应式面部表情，与运动同步。
*   **完全可打印：** 完全为 PLA 材料 3D 打印设计，只需最少的支撑。
*   **网络连接：** 连接到您的 WiFi 网络，实现远程控制和 API 访问。
*   **JSON API：** RESTful API，支持从 Python、JavaScript 等语言进行程序化控制。
*   **对话表情：** 丰富的表情库，包含适用于语音助手项目的对话变体。
*   **Sesame Studio：** 全新的动画创作软件，轻松创建自定义动作。
*   **Sesame Companion App：** Python 应用程序，用于语音控制和高级交互。
*   **串行 CLI：** 通过串行命令行界面或 Web UI 控制机器人并触发动画。
*   **预编程动作：** 包含行走、挥手、跳舞、指向、休息等动画。


## 观看 YouTube 发布视频

<a href="https://www.youtube.com/watch?v=1UDsWkcQZhc"><img src="https://github.com/user-attachments/assets/710cb5a6-163e-47e7-a294-5e2d2ab07627" width="70%" alt="thumb-youtube"></a>

___

## 快速开始

按照以下步骤构建您自己的 Sesame Robot：

### 1. 收集零件
查看**[物料清单（BOM）](hardware/bom/README.md)**，获取所需电子元件和硬件的完整列表。
*   微控制器：Lolin S2 Mini（推荐 DIY 构建使用）、Sesame Distro Board V3（当前版本，已预刷固件，支持 Bambu Lab 电池）、V2（旧版，仅 USB 供电）或 ESP32-DevKitC-32E 搭配 Distro Board V1（旧版）
*   执行器：8× MG90 舵机
*   电源：5V 3A 电源（S2 Mini 和 V2 Distro Board 使用 USB-C PD，或电池 + 降压转换器；详见 BOM 中关于 Bambu Lab 14500 7.4V 800mAh 锂离子电池的选项）

### 2. 打印零件
下载 STL 文件并遵循**[打印指南](hardware/printing/README.md)**。
*   设计用于 PLA 材料
*   仅需最少支撑

### 3. 构建与接线
遵循**[构建指南](docs/build-guide/README.md)**和**[接线指南](docs/wiring-guide/README.md)**来组装框架并连接电子元件。

### 4. 刷写固件
从**[固件目录](firmware/README.md)**上传代码。
*   需要 Arduino IDE
*   配置 WiFi AP 设置

### 5. 创建动画
使用**[Sesame Studio](software/sesame-studio/README.md)**以可视化方式为您的机器人设计姿态和动作序列。

<img width="100%" height="728" alt="sesame-wakeup-gif" src="https://github.com/user-attachments/assets/a4951195-4253-40a4-a87d-d14fad57ff5f" />

---

## 软件与固件

### Sesame Studio
Sesame Studio 是一个独立的桌面应用程序，位于 `software/sesame-studio/` 目录下。它允许您：
*   使用示意图界面可视化地摆动机器人姿态。
*   自动生成舵机角度的 C++ 代码。
*   将帧序列组合成复杂的动画。

[**> 前往 Sesame Studio**](software/sesame-studio/README.md)


### Sesame Simulator
Sesame Simulator 由 Jay Li 创建，是一个基于 Rust 的 3D 仿真环境，用于在虚拟空间中测试 Sesame 的运动和运动学。其特性包括：
*   **基于物理的仿真：** 无需硬件即可测试行走和平衡。
*   **Web 界面：** 直接在浏览器中运行仿真器。
*   **URDF 集成：** 精确建模 Sesame 的物理属性。

[**> 前往 Sesame Simulator**](https://one-for-all.github.io/sesame-robot-sim/)

### Sesame Companion App
Sesame Companion App 是一个基于 Python 的应用程序，可通过本地网络对机器人进行高级控制和交互。它利用新的 JSON API 和网络模式功能提供：
*   **语音助手集成：** 使用语音命令控制 Sesame，并查看实时情感表情。
*   **远程控制：** 从本地网络的任何位置指挥您的机器人。
*   **面部表情控制：** 根据对话或上下文动态更改表情。
*   **API 示例：** 用于构建您自己的集成的参考实现。

Companion App 适用于运行最新固件并启用网络模式的机器人。

[**> 前往 Sesame Companion App 仓库**](https://github.com/dorianborian/sesame-companion-app)

### 固件
ESP32 固件（`sesame-firmware-main.ino`）处理运动学、面部显示和 WiFi 控制接口。
*   **Web UI：** 通过内置接入点从手机控制机器人。
*   **自定义表情：** 添加您自己的位图（指南详见固件文档）。

[**> 前往固件文档**](firmware/README.md)


---

## 贡献

这个机器人是一个用于构建新功能、外观、工具和创意的平台。由于当前固件是基础实现，非常欢迎针对以下方面的 Pull Request：
*   运动学改进
*   新动画
*   改进的 Web UI/UX
*   传感器集成（超声波、陀螺仪等）

我也非常期待看到基于本项目的硬件、软件、表情等方面的分支作品。如果您成功构建了一个，请务必给我发消息，我可能会在我的网站或频道上展示您的作品！

---

*作者：[Dorian Todd](https://www.doriantodd.com/)。您的 Sesame Robot 需要帮助？在 Discord 上给我发消息，我的用户名是 "starphee"*
