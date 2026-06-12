# Sesame Robot 固件

本文档提供 Sesame Robot 所用固件的架构、控制逻辑和硬件抽象层的技术信息。

> [!NOTE]
> 固件现已组织为模块化结构，包含一个主入口点和用于位图、运动和 Web 资源的专用头文件。这使得自定义更加容易，代码库更加整洁。

## 目录

- [如何刷写固件](#如何刷写固件)
- [网络配置](#网络配置与连接)
- [API 参考](#api-参考)
  - [旧版 Web 端点](#旧版-web-端点)
  - [JSON API](#json-api推荐网络客户端使用)
  - [Python 示例](#python-api-示例)
  - [JavaScript 示例](#nodejsjavascript-示例)
- [高级集成示例](#高级集成示例)
  - [语音助手集成](#语音助手集成)
  - [智能家居集成](#智能家居集成)
- [空闲动画系统](#空闲动画系统)
- [固件架构](#固件架构)
- [技术实现概览](#技术实现概览)
- [资源管线与表情自定义](#资源管线与表情自定义)
- [硬件抽象层](#硬件抽象层-hal)

## 如何刷写固件

> [!NOTE]
> **Sesame 构建套件用户：** 您的 Distro Board V3（及早期 V2）套件已预刷最新固件。您仅在需要自定义或更新到更新版本时才需要刷写固件。

### 环境准备

1. **Arduino IDE**（推荐 2.0 或更高版本）
2. **ESP32 开发板支持**：通过 Arduino IDE 的开发板管理器安装
   - 打开 Arduino IDE
   - 进入**文件 → 首选项**
   - 在"附加开发板管理器 URL"中添加：`https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
   - 进入**工具 → 开发板 → 开发板管理器**
   - 搜索"ESP32"并安装"ESP32 by Espressif Systems"（v2.0.0 或更高版本）
3. **所需库**（通过库管理器安装）：

- `ESP32Servo` **v3.0.9**（推荐）
- `Adafruit SSD1306`
- `Adafruit GFX Library`

> [!IMPORTANT]
> 本项目请使用 **ESP32Servo v3.0.9**。较新版本目前存在已知问题：向一个舵机写入数据可能影响多个通道（[madhephaestus/ESP32Servo#103](https://github.com/madhephaestus/ESP32Servo/issues/103)）。

### 刷写步骤

1. **连接开发板**通过 USB 连接到电脑
2. **打开固件**：

   - 在 Arduino IDE 中打开 [sesame-firmware-main.ino](sesame-firmware-main.ino)
   - 确保它位于一个同名文件夹中。
   - 同时包含所有 .h 头文件。
3. **选择开发板**：

   - 进入**工具 → 开发板**
   - Lolin S2 Mini：选择"LOLIN S2 Mini"
   - Sesame Distro Board V1：选择"ESP32 Dev Module"
   - Sesame Distro Board V2 或 V3：选择"ESP32S3 Dev Module"
4. **配置开发板设置**：

   - **Lolin S2 Mini**：
     - **上传速度**：921600
     - **USB CDC On Boot**："Enabled"
     - **分区方案**："Default 4MB with spiffs"
   - **Distro Board V2 或 V3（ESP32-S3）**：
     - **USB CDC On Boot**："Enabled"（串口监视器必需）
     - **Flash Mode**："QIO 80MHz"
     - **分区方案**："Default 4MB with spiffs"
5. **选择正确的端口**：

   - 进入**工具 → 端口**并选择您的 ESP32 的 COM 端口
6. **在代码中选择您的开发板配置**：

   - 打开 [sesame-firmware-main.ino](sesame-firmware-main.ino)
   - 找到引脚配置部分（约第 55-65 行附近）
   - **如果您使用 Lolin S2 Mini 构建：** 取消 S2 Mini 的 `servoPins` 数组和 `I2C_SDA`/`I2C_SCL` 定义的注释。将 Distro Board 部分注释掉。
   - **如果您使用 Distro Board V3 构建：** 取消 V3 的 `servoPins` 数组和 `I2C_SDA`/`I2C_SCL` 定义的注释。将其他部分注释掉。
   - **如果您使用 Distro Board V2 构建：** 取消 V2 的 `servoPins` 数组和 `I2C_SDA`/`I2C_SCL` 定义的注释。将其他部分注释掉。
   - **如果您使用 Distro Board V1 构建：** 取消 V1 的 `servoPins` 数组和 `I2C_SDA`/`I2C_SCL` 定义的注释。将其他部分注释掉。
7. **（可选）配置网络模式**：

   - 如果您想让机器人连接到 WiFi 网络（用于 API 访问和远程控制），编辑网络配置部分（约第 17-22 行附近）：

   ```cpp
   #define NETWORK_SSID "YourNetworkName"  // 您的 WiFi 网络名称
   #define NETWORK_PASS "YourPassword"     // 您的 WiFi 密码
   #define ENABLE_NETWORK_MODE true        // 设为 true 启用
   ```

   - 将 `ENABLE_NETWORK_MODE` 保持为 `false` 则仅使用接入点模式
8. **上传固件**：

   - 点击 Arduino IDE 中的**上传**按钮（→）
   - 等待编译和上传完成
9. **测试连接**：

   - 打开串口监视器（**工具 → 串口监视器**，设置波特率为 115200）
   - 复位开发板 - 您应该看到启动信息和网络信息
   - 串口监视器提供单个电机控制功能，用于测试和故障排查
   - **如果仅使用 AP 模式**：连接到"Sesame-Controller-BETA"WiFi 网络（密码：`12345678`），然后访问任意网站
   - **如果使用网络模式**：在串口监视器中查看"Connected to network!"消息，然后通过 `http://sesame-robot.local` 或显示的 IP 地址访问

### 故障排除

- **Linux USB 权限**：如果端口没有显示或收到"Permission Denied"错误，您可能需要将用户添加到 `dialout` 组：`sudo usermod -a -G dialout $USER`。注销并重新登录使更改生效。
- **上传失败**：尝试在上传时按住 BOOT 按钮，或尝试另一根 USB 线。对于基于 S3 的开发板，"USB CDC On Boot"设置在复位后查找端口时至关重要。
- **端口未找到**：安装相应的 USB 驱动程序（S2 Mini 使用 CP210x，部分 ESP32 板使用 CH340）。Windows 10/11 通常包含这些驱动。
- **机器人不动作**：检查电源供应和舵机连接；如果发生电压骤降，在 Web 设置中增加 `motorCurrentDelay`
- **无法连接到网络**：
  - 验证 `ENABLE_NETWORK_MODE` 已设为 `true`，且 SSID/密码正确
  - 检查串口监视器中的连接状态 - 查找"Connected to network!"或错误消息
  - 确保您的 WiFi 网络是 2.4GHz（ESP32 不支持 5GHz）
  - 尝试在代码中增加连接超时（当前为 10 秒 / 20 次尝试）
- **mDNS 主机名无法解析**：
  - 在 Windows 上，安装 [Bonjour Print Services](https://support.apple.com/kb/DL999)
  - 在 Linux 上，确保 `avahi-daemon` 已安装并运行
  - 尝试通过 IP 地址访问（在串口监视器中查看分配的 IP）
  - 部分路由器会阻止 mDNS 流量 - 检查路由器设置或使用 IP 地址
- **API 命令无效**：
  - 确保您发送的是 POST 请求到 `/api/command`（不是 GET）
  - 验证已设置 `Content-Type: application/json` 头
  - 检查串口监视器中的错误消息和解析后的命令输出
  - 先用简单的 cURL 命令测试连通性

## 固件架构

固件拆分为几个关键文件，以保持逻辑有序和资源易于管理：

- **[sesame-firmware-main.ino](sesame-firmware-main.ino)**：主入口点，包含 `setup()`、`loop()`、核心系统逻辑、网络配置和 API 端点。
- **[face-bitmaps.h](face-bitmaps.h)**：OLED 面部表情宏和原始位图数据的专用头文件。现在包含丰富的对话表情用于语音助手集成。
- **[movement-sequences.h](movement-sequences.h)**：所有程序化运动和姿态动画的定义。
- **[captive-portal.h](captive-portal.h)**：包含基于 Web 的远程控制界面的 HTML、CSS 和 JS。

## 技术实现概览

固件基于 Arduino-ESP32 框架构建。当前固件运行在单核事件循环上，使用基于硬件的 PWM 定时器实现精确的电机控制。

### PWM 与舵机运动学

- **定时器分配**：固件使用 `ESP32PWM::allocateTimer(n)` 预留硬件定时器 0-3。这可以防止与其他外设冲突，并确保高分辨率 PWM 信号（50Hz 频率）。由于定时器数量有限，在向固件添加额外设备或调用时可能会出现网络错误。例如，在机器人的修改版本中，我尝试添加了两个电调及其舵机控制代码，CPU 因内部定时器耗尽而导致强制门户停止工作。如果您自定义固件时遇到网络错误，请检查定时器分配。
- **脉宽映射**：腿部舵机的角度（0-180）映射到微秒（732us 到 2929us）。此范围设定为大多数具有 180 度限制的业余舵机的最大行程，但可以在 `servos[i].attach()` 调用中编辑。如果您使用具有更大运动范围的电机（如 270 度舵机），需要将 PWM 映射设置为不同的微秒长度。对于 270 度舵机，我发现（833us 到 2167us）可以正常工作。
- **错开激活**：为防止同步感性负载导致的 VCC 轨坍缩（电压骤降），`setServoAngle` 辅助函数在顺序脉冲之间引入了一个强制的 `motorCurrentDelay`（默认 20ms）。此延迟应根据您的电源设置进行调整。如果您有强大的专用电源，可以尝试将其设为零。也可以在运行中通过 AP 控制器设置菜单进行更改。

### 通信与网络栈

- **双模 WiFi**：ESP32 可以同时工作在接入点模式（用于直接连接）和客户端模式（连接到现有网络），使用 `WiFi.mode(WIFI_AP_STA)`。
- **SoftAP 与强制门户**：ESP32 使用 `WiFi.softAP()` 初始化一个接入点。`DNSServer` 监听 UDP 端口 53，使用通配符"*"重定向将所有 DNS 查询映射到内部网关（`192.168.4.1`）。
- **mDNS 服务发现**：固件通过 `ESPmDNS` 库使用组播 DNS（mDNS）广播 `sesame-robot.local`，无需硬编码 IP 地址即可进行网络发现。
- **RESTful API 接口**：`WebServer` 处理旧版 URL 参数端点和现代基于 JSON 的 API：
  - `/cmd?go=[dir]`：旧版运动控制
  - `/cmd?pose=[name]`：旧版姿态触发
  - `/api/status`：JSON 状态端点（GET）
  - `/api/command`：JSON 命令端点（POST）- 支持仅面部更新和组合的面部+运动指令
  - `/getSettings` / `/setSettings`：参数配置
- **仅面部命令支持**：`/api/command` 端点智能检测仅面部请求（无 `command` 字段），并在不触发动机动画的情况下更新显示。
- **非阻塞控制流**：固件使用自定义的 `pressingCheck(String cmd, int ms)` 函数代替 `delay()`。此函数在动画帧期间轮询 `server.handleClient()` 和 `dnsServer.processNextRequest()`，实现实时可中断性（例如，松开按钮立即停止）。此 pressingCheck 协议可用于行走等运动命令，仅在按住按钮时执行每个运动。

### 显示与图形子系统

- **I2C 总线硬件**：利用 ESP32 的硬件 I2C 控制器以 400kHz（快速模式）运行，在向 SSD1306 显示屏推送全帧缓冲区时实现最低延迟。
- **内存管理（`PROGMEM`）**：大型 128x64 位图数组（每帧 1024 字节）使用 `PROGMEM` 属性存储在 Flash 中。
- **基于宏的资源管理**：固件使用 [face-bitmaps.h](face-bitmaps.h) 中的 `FACE_LIST` 宏自动注册和处理新的面部表情，减少添加动画时所需的样板代码。
- **渲染管线**：`updateAnimatedFace()` 函数在主运动逻辑之外管理帧率和序列循环，确保即使在复杂运动期间也能呈现流畅的视觉反馈。
- **动态 WiFi 信息覆盖**：`updateWifiInfoScroll()` 函数在运行的前 30 秒内（首次输入之前）将滚动连接信息合成到面部表情位图之上，以面部表情为背景，叠加黑底白字以提高可读性。
- **空闲动画系统**：实现逼真的空闲行为，包括随机眨眼（含双击眨眼）和来回式面部动画，在未检测到输入时自动触发。

## 开发环境要求

- **开发板支持**：推荐 ESP32 by Espressif Systems（v2.0.0+）。Lolin S2 Mini 和 ESP32-WROOM32 DevKitC 支持最佳。
- **库**：
  - `ESP32Servo` **v3.0.9**：底层 PWM 定时器管理。（因较新版本存在[已知的多舵机指令泄漏问题](https://github.com/madhephaestus/ESP32Servo/issues/103)而固定版本）
  - `Adafruit_SSD1306` & `Adafruit_GFX`：基于缓冲区的 OLED 渲染。
  - `ESPmDNS`：mDNS 服务发现（包含在 ESP32 开发板支持中）。
  - `DNSServer`：强制门户 DNS 重定向（包含在 ESP32 开发板支持中）。
  - `WebServer`：HTTP 服务器实现（包含在 ESP32 开发板支持中）。
- **工具**：推荐 Arduino IDE 2.0+。

## 网络配置与连接

固件现在支持**双模 WiFi 操作**，允许机器人同时充当接入点（用于直接连接）并连接到您现有的 WiFi 网络（用于与其他设备集成和远程控制）。

### 接入点模式（默认）

默认情况下，机器人创建自己的 WiFi 网络：

- **SSID**：`Sesame-Controller-BETA`
- **密码**：`12345678`（如果默认密码无法连接，部分设备可能需要您更改此密码）
- **IP 地址**：`192.168.4.1`

连接到该网络，然后访问任意网站即可进入强制门户控制界面。如果连接困难，尝试在代码的 `#define AP_PASS` 部分将密码更改为自定义密码。

### 网络模式（可选）

要将机器人连接到您的家庭或办公室 WiFi 网络：

1. **启用网络模式**在 [sesame-firmware-main.ino](sesame-firmware-main.ino) 中：

   ```cpp
   #define NETWORK_SSID "YourNetworkName"  // 您的 WiFi 网络名称
   #define NETWORK_PASS "YourPassword"     // 您的 WiFi 密码
   #define ENABLE_NETWORK_MODE true        // 设为 true 启用
   ```
2. **将更新的固件刷入**您的机器人。
3. **通过多种方式访问**：

   - **mDNS 主机名**：`http://sesame-robot.local`（适用于大多数设备）
   - **网络 IP**：在 115200 波特率下查看串口监视器获取分配的 IP 地址
   - **仍可通过 AP 访问**：机器人即使在连接到您的网络后仍保持其接入点

### mDNS 发现

固件包含一个 mDNS 响应器，在本地网络上广播主机名 `sesame-robot.local`。这使您无需知道 IP 地址即可访问机器人：

```bash
# 从浏览器访问
http://sesame-robot.local

# Ping 测试
ping sesame-robot.local
```

**注意**：mDNS 原生支持：

- macOS 和 iOS 设备
- 安装了 Avahi 的 Linux
- Windows 10+（可能需要 Bonjour 服务）

### 安全注意事项

固件专为本地网络使用而设计，默认不包含身份验证。启用网络模式时：

- **仅限本地网络**：机器人响应来自网络上任何设备的请求。请勿在未采取适当安全措施的情况下将其暴露到互联网。
- **无 HTTPS**：通信是未加密的 HTTP。避免通过机器人的 API 传输敏感数据。
- **接入点密码**：默认 AP 密码是 `12345678`。用于生产环境时，将固件中的 `AP_PASS` 更改为更强的密码。
- **可信网络**：仅将机器人连接到可信的 WiFi 网络。

增强安全性：

1. 将默认 AP 密码更改为强而独特的密码
2. 使用网络分段（IoT VLAN）将机器人与关键设备隔离
3. 实施防火墙规则以限制对特定 IP 地址的访问
4. 考虑在生产部署的自定义固件分支中添加身份验证头

### WiFi 信息显示

机器人的 OLED 屏幕现在具有智能 WiFi 信息系统：

- **前 30 秒**：如果未收到输入，WiFi 连接信息在显示屏顶部滚动显示
- **首次输入之后**：WiFi 信息消失，仅显示面部表情
- **双模信息**：当同时连接到 AP 和网络时，显示两种连接的详细信息

滚动文本包括：

- 接入点 SSID 和 IP
- 网络名称和 IP（如果已连接）
- mDNS 主机名
- 强制门户使用说明

## API 参考

固件公开了旧版 Web 端点和现代基于 JSON 的 API 用于程序化控制。

### 旧版 Web 端点

这些端点使用 URL 参数，主要由 Web 界面使用：

#### 运动控制

```http
GET /cmd?go=forward
GET /cmd?go=backward
GET /cmd?go=left
GET /cmd?go=right
GET /cmd?stop
```

#### 姿态控制

```http
GET /cmd?pose=wave
GET /cmd?pose=dance
GET /cmd?pose=rest
GET /cmd?pose=stand
# ... (所有可用姿态请参见 movement-sequences.h)
```

#### 单个电机控制

```http
GET /cmd?motor=1&value=90
# motor: 1-8（电机编号）
# value: 0-180（角度度数）
```

#### 设置管理

```http
GET /getSettings
# 返回：{"frameDelay":100,"walkCycles":10,"motorCurrentDelay":20,"faceFps":8}

GET /setSettings?frameDelay=120&walkCycles=15&motorCurrentDelay=25&faceFps=10
```

### JSON API（推荐网络客户端使用）

JSON API 专为外部设备、Python 脚本和 IoT 集成的程序化控制而设计。

#### 获取机器人状态

```http
GET /api/status
```

**响应：**

```json
{
  "currentCommand": "forward",
  "currentFace": "walk",
  "networkConnected": true,
  "apIP": "192.168.4.1",
  "networkIP": "192.168.1.100"
}
```

#### 发送命令

```http
POST /api/command
Content-Type: application/json

{
  "command": "forward",
  "face": "walk"
}
```

**响应：**

```json
{
  "status": "ok",
  "message": "Command executed"
}
```

#### 仅更新面部表情

发送面部表情更改而不触发运动：

```http
POST /api/command
Content-Type: application/json

{
  "face": "happy"
}
```

**响应：**

```json
{
  "status": "ok",
  "message": "Face updated"
}
```

#### 停止命令

```http
POST /api/command
Content-Type: application/json

{
  "command": "stop"
}
```

### 可用命令

**运动命令：**

- `forward`、`backward`、`left`、`right` - 连续运动（循环直到停止）
- `stop` - 立即停止当前运动

**姿态命令（一次性动画）：**

- `rest`、`stand`、`wave`、`dance`、`swim`、`point`
- `pushup`、`bow`、`cute`、`freaky`、`worm`、`shake`
- `shrug`、`dead`、`crab`

**可用面部表情：**

- 运动表情：`walk`、`rest`、`stand`、`dance`、`wave` 等
- 对话表情：`happy`、`sad`、`angry`、`surprised`、`sleepy`、`love`、`excited`、`confused`、`thinking`
- 说话变体：`talk_happy`、`talk_sad`、`talk_angry` 等
- 特殊表情：`idle`、`idle_blink`、`default`

### Python API 示例

```python
import requests
import time

# 机器人 IP 或主机名
robot_url = "http://sesame-robot.local"

# 获取状态
response = requests.get(f"{robot_url}/api/status")
status = response.json()
print(f"当前表情：{status['currentFace']}")

# 让机器人挥手并显示开心的表情
requests.post(f"{robot_url}/api/command", json={
    "command": "wave",
    "face": "happy"
})

time.sleep(3)

# 仅更改表情而不移动
requests.post(f"{robot_url}/api/command", json={
    "face": "excited"
})

# 停止运动
requests.post(f"{robot_url}/api/command", json={
    "command": "stop"
})
```

### cURL 示例

```bash
# 获取机器人状态
curl http://sesame-robot.local/api/status

# 让机器人跳舞
curl -X POST http://sesame-robot.local/api/command \
  -H "Content-Type: application/json" \
  -d '{"command":"dance","face":"dance"}'

# 仅更改面部表情
curl -X POST http://sesame-robot.local/api/command \
  -H "Content-Type: application/json" \
  -d '{"face":"happy"}'

# 停止运动
curl -X POST http://sesame-robot.local/api/command \
  -H "Content-Type: application/json" \
  -d '{"command":"stop"}'
```

### Node.js/JavaScript 示例

```javascript
const robotURL = 'http://sesame-robot.local';

// 获取状态
fetch(`${robotURL}/api/status`)
  .then(res => res.json())
  .then(data => console.log('机器人状态：', data));

// 发送命令
fetch(`${robotURL}/api/command`, {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    command: 'wave',
    face: 'happy'
  })
})
.then(res => res.json())
.then(data => console.log('响应：', data));

// 仅更新面部表情
fetch(`${robotURL}/api/command`, {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({face: 'surprised'})
})
.then(res => res.json())
.then(data => console.log('响应：', data));
```

## 高级集成示例

### 语音助手集成

对话表情、JSON API 和网络模式的组合使 Sesame Robot 非常适合语音助手项目。以下是一个使用 Python 配合语音识别的示例：

```python
import requests
import speech_recognition as sr
from textblob import TextBlob
import time

robot_url = "http://sesame-robot.local"

def analyze_sentiment(text):
    """分析情感并返回适当的面部表情"""
    blob = TextBlob(text)
    polarity = blob.sentiment.polarity
  
    if polarity > 0.5:
        return "excited"
    elif polarity > 0.2:
        return "happy"
    elif polarity < -0.5:
        return "angry"
    elif polarity < -0.2:
        return "sad"
    else:
        return "thinking"

def set_robot_face(face, talking=False):
    """更新机器人的面部表情"""
    if talking:
        face = f"talk_{face}"
  
    requests.post(f"{robot_url}/api/command", json={"face": face})

# 初始化语音识别
recognizer = sr.Recognizer()

while True:
    with sr.Microphone() as source:
        print("正在聆听...")
        set_robot_face("idle")
    
        try:
            audio = recognizer.listen(source, timeout=5)
            set_robot_face("thinking")
        
            text = recognizer.recognize_google(audio)
            print(f"您说了：{text}")
        
            # 确定情感并显示说话表情
            emotion = analyze_sentiment(text)
            set_robot_face(emotion, talking=True)
        
            # 处理命令...
            time.sleep(2)
        
            # 恢复到中性表情
            set_robot_face(emotion, talking=False)
        
        except sr.WaitTimeoutError:
            set_robot_face("sleepy")
        except sr.UnknownValueError:
            set_robot_face("confused")
```

### 智能家居集成

与 Home Assistant 或其他智能家居平台集成：

```python
# Home Assistant 自动化示例
import requests

def robot_notification(message_type):
    """根据通知类型显示机器人表情"""
    robot_url = "http://sesame-robot.local"
  
    emotion_map = {
        "doorbell": "surprised",
        "alarm": "angry",
        "reminder": "thinking",
        "success": "happy",
        "error": "sad"
    }
  
    face = emotion_map.get(message_type, "default")
    requests.post(f"{robot_url}/api/command", json={"face": face})
  
    # 可选：添加运动
    if message_type == "doorbell":
        requests.post(f"{robot_url}/api/command", json={
            "command": "wave",
            "face": "happy"
        })
```

### WebSocket 流式传输（高级）

对于具有最低延迟的实时控制，您可以轮询状态端点或实现简单的状态机：

```javascript
// React/Vue.js 实时机器人控制
class RobotController {
  constructor(robotURL) {
    this.url = robotURL;
    this.currentFace = 'default';
  }
  
  async updateEmotion(emotion, isSpeaking) {
    const face = isSpeaking ? `talk_${emotion}` : emotion;
  
    if (face !== this.currentFace) {
      await fetch(`${this.url}/api/command`, {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({face})
      });
  
      this.currentFace = face;
    }
  }
  
  async getStatus() {
    const res = await fetch(`${this.url}/api/status`);
    return await res.json();
  }
  
  async performAction(action, emotion = null) {
    const payload = {command: action};
    if (emotion) payload.face = emotion;
  
    await fetch(`${this.url}/api/command`, {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify(payload)
    });
  }
}

// 使用示例
const robot = new RobotController('http://sesame-robot.local');

// 设置表情而不移动
await robot.updateEmotion('happy', false);

// 设置说话表情
await robot.updateEmotion('excited', true);

// 执行动作并配合表情
await robot.performAction('dance', 'excited');

// 检查状态
const status = await robot.getStatus();
console.log(`机器人表情：${status.currentFace}，正在：${status.currentCommand}`);
```

## 空闲动画系统

固件包含一个智能空闲动画系统，自动激活：

### 行为

- **激活**：当机器人在一段时间内未收到命令时，进入空闲模式
- **空闲表情**：使用 `FACE_ANIM_BOOMERANG` 模式下的 `idle` 面部表情显示柔和的"呼吸"动画
- **眨眼**：随机触发眨眼动画（3-7 秒间隔）
- **双击眨眼**：30% 的概率出现双击眨眼以实现逼真行为
- **退出**：任何运动命令或输入立即退出空闲模式

### 技术实现

空闲系统使用：

- `enterIdle()` - 使用来回动画激活空闲表情
- `exitIdle()` - 返回正常运行
- `updateIdleBlink()` - 管理随机眨眼时序和双击眨眼逻辑
- `scheduleNextIdleBlink()` - 随机化眨眼间隔以实现自然的外观

要自定义空闲行为，修改 [sesame-firmware-main.ino](sesame-firmware-main.ino) 中的时序值：

```cpp
scheduleNextIdleBlink(3000, 7000);  // 眨眼间隔的最小和最大毫秒数
```

## 资源管线与表情自定义

固件包含一个资源管线，简化了自定义面部表情的添加过程。

### 表情库

固件包含丰富的表情库，分为三个类别：

#### 运动表情

与物理姿态和动画同步：

- `walk`、`rest`、`stand`、`swim`、`dance`、`wave`、`point`
- `pushup`、`bow`、`cute`、`freaky`、`worm`、`shake`、`shrug`
- `dead`、`crab`、`idle`、`idle_blink`

#### 对话表情

专为表达性沟通和语音助手集成而设计：

- **基础情绪**：`happy`、`sad`、`angry`、`surprised`、`sleepy`、`love`、`excited`、`confused`、`thinking`
- **说话变体**：`talk_happy`、`talk_sad`、`talk_angry`、`talk_surprised`、`talk_sleepy`、`talk_love`、`talk_excited`、`talk_confused`、`talk_thinking`

"talk_"变体具有张嘴效果，用于对口型和动画语音。这些表情非常适合：

- 语音助手项目（Alexa、Google Assistant、自定义 TTS）
- 通过 JSON API 控制的聊天机器人界面
- 互动故事讲述和教育应用
- 远程控制表演

#### 特殊表情

- `default` - 启动/回退表情
- `idle` - 空闲状态的柔和呼吸动画
- `idle_blink` - 空闲时随机触发的眨眼动画

### 添加自定义表情

### 工作流程：

1. **图像创建**：使用颜文字或 [Emojicombos](https://emojicombos.com/kaomoji) 寻找表情。在 [jsPaint](https://jspaint.app/) 等工具中创建 `128x64` 的图像。
2. **位图转换**：使用 [image2cpp](https://javl.github.io/image2cpp/)，选择 `Horizontal` 缩放、`128x64` 分辨率、`Arduino Code` 输出。
3. **注册**：
   - 将您的表情名称添加到 [face-bitmaps.h](face-bitmaps.h) 的 `FACE_LIST` 宏中。
   - 将生成的 C 数组粘贴到 [face-bitmaps.h](face-bitmaps.h) 中，放在列表中最后一个位图之后。
   - （可选）对于动画，添加编号后缀（例如 `_1`、`_2`）并在 [sesame-firmware-main.ino](sesame-firmware-main.ino) 的 `faceFpsEntries` 中注册 FPS。

### 表情动画

要使动画被 `MAKE_FACE_FRAMES` 宏识别，您在 [face-bitmaps.h](face-bitmaps.h) 中的数组名称必须遵循严格的命名约定：

- **根帧**：`epd_bitmap_myface`（这是必需的，作为第 0 帧）。
- **后续帧**：`epd_bitmap_myface_1`、`epd_bitmap_myface_2` 等。
- **限制**：默认系统每个表情最多支持 6 帧（根帧 + 5 个编号帧）。
- **动画模式**：动画可以配置为 `LOOP`（循环播放，从第 0 帧重新开始）、`ONCE`（在最后一帧停止）或 `BOOMERANG`（向前播放然后反向播放）。这些模式通常在 [movement-sequences.h](movement-sequences.h) 中触发姿态时定义。

### 宏系统

`FACE_LIST` 宏使用 X-Macros 自动生成变量声明和注册对象：

```cpp
#define FACE_LIST \
    X(walk) \
    X(rest) \
    X(my_new_face) // 只需添加这一行！
```

这消除了在添加新资源时手动更新多个 switch 语句或数组的需要。

## 硬件抽象层（HAL）

固件通过 `servoPins` 数组抽象引脚定义。默认配置针对 **Sesame Distro Board V1、V2 或 V3** 和 **Lolin S2 Mini** 进行了优化，但可以轻松移植到任何具有 WiFi 能力的 ESP32（例如 S3、C3 或 DevKit V1）。

### 引脚配置表

#### Lolin S2 Mini（ESP32-S2）

| 电机/组件 | 数组索引 | GPIO 引脚 | 备注 |
| ----------------- | ----------- | ------------ | ---------------------------- |
| 电机 0 | 0 | 1 | R1 |
| 电机 1 | 1 | 2 | R2 |
| 电机 2 | 2 | 4 | L1 |
| 电机 3 | 3 | 6 | L2 |
| 电机 4 | 4 | 8 | R4 |
| 电机 5 | 5 | 10 | R3 |
| 电机 6 | 6 | 13 | L3 |
| 电机 7 | 7 | 14 | L4 |
| **I2C SDA** | - | **33** | SSD1306 数据线（硬件 I2C） |
| **I2C SCL** | - | **35** | SSD1306 时钟线（硬件 I2C） |

#### Sesame Distro Board V1（ESP32-WROOM32）

| 电机/组件 | 数组索引 | GPIO 引脚 | 备注 |
| ----------------- | ----------- | ------------ | ---------------------------- |
| 电机 0 | 0 | 15 | R1 |
| 电机 1 | 1 | 2 | R2 |
| 电机 2 | 2 | 23 | L1 |
| 电机 3 | 3 | 19 | L2 |
| 电机 4 | 4 | 4 | R4 |
| 电机 5 | 5 | 16 | R3 |
| 电机 6 | 6 | 17 | L3 |
| 电机 7 | 7 | 18 | L4 |
| **I2C SDA** | - | **21** | SSD1306 数据线（硬件 I2C） |
| **I2C SCL** | - | **22** | SSD1306 时钟线（硬件 I2C） |

#### Sesame Distro Board V3（ESP32-S3）

| 电机/组件 | 数组索引 | GPIO 引脚 | 备注 |
| ----------------- | ----------- | ----------- | ---------------------------- |
| 电机 0 | 0 | 4 | R1 |
| 电机 1 | 1 | 5 | R2 |
| 电机 2 | 2 | 6 | L1 |
| 电机 3 | 3 | 7 | L2 |
| 电机 4 | 4 | 10 | R4 |
| 电机 5 | 5 | 11 | R3 |
| 电机 6 | 6 | 12 | L3 |
| 电机 7 | 7 | 13 | L4 |
| **I2C SDA** | - | **8** | SSD1306 数据线（硬件 I2C） |
| **I2C SCL** | - | **9** | SSD1306 时钟线（硬件 I2C） |

#### Sesame Distro Board V2（ESP32-S3）

| 电机/组件 | 数组索引 | GPIO 引脚 | 备注 |
| ----------------- | ----------- | ----------- | ---------------------------- |
| 电机 0 | 0 | 4 | R1 |
| 电机 1 | 1 | 5 | R2 |
| 电机 2 | 2 | 6 | L1 |
| 电机 3 | 3 | 7 | L2 |
| 电机 4 | 4 | 15 | R4 |
| 电机 5 | 5 | 16 | R3 |
| 电机 6 | 6 | 17 | L3 |
| 电机 7 | 7 | 18 | L4 |
| **I2C SDA** | - | **8** | SSD1306 数据线（硬件 I2C） |
| **I2C SCL** | - | **9** | SSD1306 时钟线（硬件 I2C） |

### 移植到其他 ESP32 变体

要将其移植到不同的 ESP32 变体，修改 [sesame-firmware-main.ino](sesame-firmware-main.ino) 头部中的 `servoPins` 和 `I2C_` 定义。确保所选引脚具有 PWM 能力且不是"仅输入"引脚。

## 执行与部署

1. **工具链**：将 IDE 配置为 `ESP32 Dev Module` 或 `Lolin S2 Mini`。
2. **校准**：使用串口监视器（115200）发送手动步进命令（例如 `rn wf`）。
3. **电源管理**：如果机器人在运动过程中出现电压骤降，在 Web 设置中增加 `motorCurrentDelay` 以进一步错开舵机脉冲。
