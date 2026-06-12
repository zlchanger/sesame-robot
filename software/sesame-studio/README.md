# Sesame Studio

Sesame Studio 是 Sesame Robot 的动画创作工具。使用它可以设置舵机角度和帧、生成 C++ 代码，并将帧排列成动画序列。

<img src="assets/sesamestudio-preview.png" alt="sesamestudio-preview" width="50%">

## 功能

- **界面**：黑色主题，带有示意图叠加层。
- **视图**：俯视图和侧面角度参考用于定位。
- **颜色编码**：舵机（S0-S7）在输入框和代码中使用颜色编码以便识别。
- **帮助覆盖层**：可通过按钮访问的参考窗口。
- **代码生成**：为 `sesame-firmware.ino` 格式化 `setServoAngle()` 命令。
- **时间控制**：为帧设置延迟时间以创建运动效果。

## 环境要求

- **Python 3.x**：从 [python.org](https://python.org) 下载。
- **Pillow**：用于缩放示意图。
  ```bash
  pip install Pillow
  ```
- **Tkinter**：如果缺失，请安装 `python3-tk`。

## 使用方法

1.  **启动**：
    ```bash
    python sesame_studio.py
    ```

2.  **设置姿态**：
    - 此区域显示带有关节位置框的机器人示意图。
    - 在框中输入角度（0-180）。
    - 使用**帮助（?）**按钮查看参考指南。

3.  **设置时间**：
    - 调整 `Delay (ms)` 字段。
    - 点击 **+ ADD FRAME**。代码将显示在编辑器中。

4.  **导出**：
    - 点击 **Copy to Clipboard** 并粘贴到 Arduino IDE 中。
    - 使用 **Clear Code** 开始新的序列。

## 故障排除

- **"Module not found: tkinter"**：在某些 Linux 发行版上，可能需要单独安装（`sudo apt-get install python3-tk`）。
- **高 DPI 缩放**：如果界面在屏幕上看起来过小或过大，请尝试在 Windows 的 `python.exe` 兼容性设置属性中更改"高 DPI 缩放替代"选项。
