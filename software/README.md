# 软件

用于充分发挥 Sesame Robot 功能的软件应用。

## 工具

### [Sesame Studio](sesame-studio/)
基于 Python 的可视化动画创作工具。

- **图形界面**：在屏幕上通过参考线布局关节。
- **代码生成器**：输出可直接用于固件的 C++ 代码。

[前往 Sesame Studio 文档 ->](sesame-studio/README.md)

### Sesame Companion App
用于通过本地网络进行高级机器人控制和交互的 Python 应用。

- **语音控制**：集成语音识别实现语音指令。
- **情感映射**：基于情感分析自动设置机器人表情。
- **远程 API 控制**：通过 JSON API 发送指令和表情变化。
- **网络模式**：需要固件启用网络模式。
- **智能家居**：与 Home Assistant 等平台的集成示例。

**注意：** Companion App 是独立仓库，需要刷入最新支持网络模式的固件。

[前往 Sesame Companion App 仓库 ->](https://github.com/dorianborian/sesame-companion-app)


