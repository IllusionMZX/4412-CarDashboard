# Qt-based Embedded Linux Automotive Dashboard System
# 基于QT的嵌入式Linux车载中控系统

<div align="center">

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Qt](https://img.shields.io/badge/Qt-5.7.0-green.svg)
![Platform](https://img.shields.io/badge/platform-Linux%20ARM-orange.svg)
![Board](https://img.shields.io/badge/board-iTop4412-red.svg)

</div>

## Language / 语言

- [English](#english-documentation)
- [中文](#中文文档)

---

## English Documentation

### 🚗 Project Overview

`TopCarBoard_M` is a comprehensive Qt-based automotive dashboard system designed for embedded Linux platforms. The system integrates multiple functionalities including GPS navigation, IMU sensor monitoring, camera feed processing, map visualization, and weather information display.

### 🎯 Key Features

- **Real-time Dashboard** - Interactive instrument panel with speed, fuel, and temperature gauges
- **GPS Navigation** - NMEA protocol parsing with coordinate and speed display
- **IMU Monitoring** - Real-time accelerometer, gyroscope, and angle data visualization
- **Camera Integration** - USB camera feed with YUV to RGB conversion
- **Map Display** - Interactive map with tile-based rendering using QMapControl
- **Weather Information** - Real-time weather data with graphical interface
- **Modular Architecture** - Clean separation of concerns with dedicated modules

### 🏗️ System Architecture

```mermaid
graph TB
    subgraph "Hardware Layer"
        A[iTop4412 Development Board]
        B[GPS Module via USB-Serial]
        C[IMU Sensor via USB-Serial]
        D[USB Camera]
        E[LCD Display 480x272]
        F[USB Hub]
    end
    
    subgraph "Kernel Layer"
        G[Modified Linux Kernel 3.0]
        H[CP210x Driver]
        I[CH341 Driver]
        J[V4L2 Driver]
        K[USB Core]
    end
    
    subgraph "Application Layer"
        L[MainWidget - Central Control]
        
        subgraph "Functional Modules"
            M[Dashboard Widget]
            N[GPS Process]
            O[IMU Process]
            P[UVC Camera]
            Q[Map Demo]
            R[Weather Module]
        end
        
        subgraph "Qt Framework"
            S[Qt Core 5.7.0]
            T[Qt GUI]
            U[Qt Network]
            V[Qt Widgets]
        end
    end
    
    subgraph "Data Flow"
        W[Serial Data Stream]
        X[Network Data]
        Y[Video Stream]
        Z[User Interaction]
    end
    
    A --> G
    B --> W
    C --> W
    D --> Y
    
    W --> N
    W --> O
    Y --> P
    X --> R
    X --> Q
    Z --> L
    
    L --> M
    L --> N
    L --> O
    L --> P
    L --> Q
    L --> R
    
    M --> E
    N --> E
    O --> E
    P --> E
    Q --> E
    R --> E
```

### 🛠️ Development Environment

| Component | Version/Model |
|-----------|---------------|
| **Cross-compilation** | Ubuntu 16.04 |
| **Qt Framework** | 5.7.0 |
| **Filesystem** | QTE5.7 Image |
| **Linux Kernel** | Modified iTop4412_Kernel_3.0 |
| **Target Board** | iTop4412 Development Board |
| **Display** | 480×272 LCD |

### 📦 Hardware Components

| Module | Model | Interface |
|--------|-------|-----------|
| **IMU Sensor** | YaBo Intelligence | USB-Serial (CP210x/CH341) |
| **GPS Module** | YaBo Intelligence | USB-Serial (CP210x/CH341) |
| **USB Camera** | HikVision | USB 2.0 |
| **USB Hub** | UGREEN | USB 2.0 |


### 📁 Project Structure

```
TopCarBoard_M/
├── 📁 Root Directory
│   ├── main.cpp                    # Application entry point
│   ├── mainwidget.cpp/.h          # Main menu interface
│   └── TopCarBoard_M.pro          # Qt project configuration
│
├── 📊 Dashboard/                   # Instrument panel module
│   ├── widget.cpp/.h/.ui          # Dashboard UI implementation
│   ├── res.qrc                    # Resource file
│   └── 🖼️ icons/                  # Dashboard icons and images
│
├── 🗺️ QMapDemo/                   # Map display module
│   ├── multidemo.cpp/.h           # Map widget implementation
│   ├── gps_modul.cpp/.h           # GPS integration
│   └── src/QMapControl/           # Open-source QMapControl library
│
├── 📡 GPSProcess/                  # GPS data processing
│   ├── GPSProcess.cpp/.h          # NMEA protocol parsing
│   └── Serial communication with GPS module
│
├── 📱 IMUProcess/                  # IMU sensor processing
│   ├── IMUProcess.cpp/.h          # Accelerometer, gyroscope data
│   └── Motion detection and alerts
│
├── 📹 UVC/                        # USB camera module
│   ├── processImage.cpp/.h       # Image processing
│   ├── videodevice.cpp/.h         # V4L2 camera interface
│   └── YUV to RGB conversion
│
├── 🌤️ Weather/                    # Weather information
│   ├── mainwindow.cpp/.h/.ui      # Weather UI
│   ├── weatherdata.h              # Data structures
│   └── Network-based weather API
│
└── 🔧 bin/                        # Compiled executables
    └── TopCarBoard_M
```

### 🔧 Module Descriptions

#### 🏠 MainWidget (Central Control)
- **Purpose**: Main menu interface with 6 functional modules
- **Features**: 
  - Grid-based icon layout (2×3)
  - Module navigation and window management
  - Resource management with automatic cleanup

#### 📊 Dashboard Module
- **Purpose**: Real-time instrument panel display
- **Features**:
  - Analog speedometer with needle animation
  - Fuel level and temperature gauges
  - LED indicator controls (turn signals, hazard lights)
  - ADC-based speed input from potentiometer

#### 📡 GPS Processing Module
- **Purpose**: Real-time GPS data acquisition and parsing
- **Protocol**: NMEA 0183 ($GNGGA, $GNVTG sentences)
- **Features**:
  - Coordinate parsing (latitude/longitude)
  - Speed calculation (knots/km/h)
  - Satellite count and signal quality
  - Asynchronous serial communication

#### 📱 IMU Processing Module
- **Purpose**: Inertial measurement and motion detection
- **Features**:
  - 3-axis accelerometer data
  - 3-axis gyroscope data
  - Calculated angle information
  - Collision detection with buzzer alerts

#### 📹 Camera Module
- **Purpose**: USB camera video stream processing
- **Technology**: Video4Linux2 (V4L2) API
- **Features**:
  - Real-time video capture
  - YUV422 to RGB24 color space conversion
  - Frame rate optimization
  - Error handling and device management

#### 🗺️ Map Display Module
- **Purpose**: Interactive map visualization
- **Technology**: QMapControl library
- **Features**:
  - Tile-based map rendering
  - Online map data fetching
  - GPS position overlay
  - Zoom and pan functionality

#### 🌤️ Weather Module
- **Purpose**: Real-time weather information display
- **Features**:
  - Network-based weather API integration
  - Temperature and humidity display
  - Weather icon visualization
  - JSON data parsing

### 🚀 Installation and Setup

#### 1. Kernel Configuration
Navigate to the iTop4412_Kernel_3.0 directory and configure the kernel:

```bash
make menuconfig
```

Enable the following drivers:

**CP210x USB-to-Serial Driver:**
```
Device Drivers --->
    [*] USB support --->
        USB Serial Converter support ---> 
            <*> USB CP210x family of UART Bridge Controllers
```

**CH341 USB-to-Serial Driver:**
```
Device Drivers --->
    [*] USB support ---> 
        USB Serial Converter support ---> 
            <*> USB CH341 support
```

Compile the kernel:
```bash
make zImage
```

Flash the kernel using fastboot:
```bash
fastboot flash kernel zImage
```

#### 2. Cross-compilation
In Ubuntu 16.04 environment:

```bash
# Navigate to project directory
cd TopCarBoard_M/

# Generate Makefile
qmake

# Compile the project
make

# The executable will be generated in bin/
```

#### 3. Deployment

1. **Prepare the target board:**
   - Flash the modified kernel and QTE5.7 filesystem
   - Connect debug serial port to PC
   - Power on the development board

2. **Network setup:**
   - Configure Ethernet connection between PC and board
   - Set PC IP: `192.168.137.1`
   - Set board IP: `192.168.137.2`
   - Set board gateway: `192.168.137.1`

3. **Hardware connections:**
   - Connect USB hub to iTop4412
   - Connect GPS module, IMU sensor, USB camera, and mouse to hub
   - Ensure proper device enumeration (`/dev/ttyUSB0`, `/dev/ttyUSB1`)

4. **File transfer and execution:**
   ```bash
   # Copy project to USB drive, then to board
   # On the target board:
   chmod +x TopCarBoard_M
   ./TopCarBoard_M
   ```

### 🧪 Testing and Validation

| Module | Test Criteria |
|--------|---------------|
| **Dashboard** | LED control, speedometer response to ADC input |
| **GPS** | Coordinate accuracy, speed calculation |
| **Camera** | Real-time video feed, color conversion |
| **IMU** | Motion detection, collision alerts |
| **Map** | Tile loading, GPS position overlay |
| **Weather** | Network connectivity, data parsing |

### ⚙️ Configuration Notes

- **Device mapping**: IMU → `/dev/ttyUSB0`, GPS → `/dev/ttyUSB1`
- **Baud rates**: GPS (9600), IMU (115200)
- **Display resolution**: 480×272 pixels
- **Network proxy**: Configured for HTTP proxy at `192.168.137.1:7890`

### 🎬 System Demonstration

The following images showcase the real-world functionality of the TopCarBoard_M system:

#### 📱 Main Interface
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image1.jpg" alt="Main Interface" width="600">
<p><em>Main menu interface with 6 functional modules arranged in a 2×3 grid layout</em></p>
</div>

#### 📊 Dashboard Panel
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image2.png" alt="Dashboard Panel" width="600">
<p><em>Interactive dashboard with LED controls - checkboxes synchronize with turn signals and hazard lights. Speed updates by rotating the potentiometer to adjust ADC voltage values</em></p>
</div>

#### 📹 USB Camera Live Feed
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image3.png" alt="USB Camera" width="600">
<p><em>Real-time video capture from USB camera with YUV to RGB conversion</em></p>
</div>

#### 🗺️ Interactive Map Display
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image4.png" alt="Map Display" width="600">
<p><em>Real-time map loading with mouse and scroll wheel navigation. Tile-based rendering for smooth interaction</em></p>
</div>

#### 📱 IMU Sensor Data Visualization
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image5.png" alt="IMU Data" width="600">
<p><em>Three-axis accelerometer, gyroscope, and attitude angle data display from IMU sensor</em></p>
</div>

#### ⚠️ Motion Detection Alert
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image6.png" alt="Motion Alert" width="600">
<p><em>IMU motion detection in action - buzzer alerts and warning messages appear when rapid acceleration changes are detected (simulated by shaking the IMU sensor)</em></p>
</div>

#### 📡 GPS Positioning System
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image7.png" alt="GPS Positioning" width="600">
<p><em>GPS location data display showing current coordinates. Accuracy verified against mobile phone GPS readings</em></p>
</div>

#### 🌤️ Weather & Time Information
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image8.png" alt="Weather Display" width="600">
<p><em>Network-based real-time weather and time information display</em></p>
</div>

### 🔧 Troubleshooting

**Common Issues:**

1. **Serial device not found**
   - Check USB device enumeration: `lsusb`
   - Verify driver loading: `lsmod | grep usbserial`

2. **Camera initialization failed**
   - Check V4L2 device: `ls /dev/video*`
   - Verify camera compatibility: `v4l2-ctl --list-devices`

3. **Network connectivity issues**
   - Test basic connectivity: `ping 8.8.8.8`
   - Check proxy configuration in main.cpp

---

## 中文文档

### 🚗 项目概述

`TopCarBoard_M` 是一个基于 Qt 的综合性车载仪表盘系统，专为嵌入式 Linux 平台设计。系统集成了多种功能，包括 GPS 导航、IMU 传感器监控、摄像头图像处理、地图可视化和天气信息显示。
### 🎯 核心功能

- **实时仪表盘** - 交互式仪表板，包含速度、燃油和温度表
- **GPS 导航** - NMEA 协议解析，显示坐标和速度信息
- **IMU 监控** - 实时加速度计、陀螺仪和角度数据可视化
- **摄像头集成** - USB 摄像头图像流，YUV 到 RGB 转换
- **地图显示** - 基于瓦片渲染的交互式地图，使用 QMapControl
- **天气信息** - 实时天气数据与图形化界面
- **模块化架构** - 清晰的关注点分离，专用功能模块

### 🏗️ 系统架构图

上方的系统架构图展示了整个系统的层次结构：

**硬件层**：包括 iTop4412 开发板、GPS 模块、IMU 传感器、USB 摄像头等硬件组件

**内核层**：经过修改的 Linux 3.0 内核，包含 CP210x、CH341 驱动程序和 V4L2 驱动

**应用层**：基于 Qt 5.7.0 框架的应用程序，包含主控制器和各功能模块

### 🛠️ 开发环境

| 组件 | 版本/型号 |
|------|-----------|
| **交叉编译环境** | Ubuntu 16.04 |
| **Qt 框架** | 5.7.0 |
| **文件系统** | 讯为 QTE5.7 镜像 |
| **Linux 内核** | 修改版 iTop4412_Kernel_3.0 |
| **目标板** | iTop4412 开发板 |
| **显示屏** | 480×272 LCD |

### 📦 硬件组件

| 模块 | 型号 | 接口 |
|------|------|------|
| **IMU 传感器** | 亚博智能 | USB-串口 (CP210x/CH341) |
| **GPS 模块** | 亚博智能 | USB-串口 (CP210x/CH341) |
| **USB 摄像头** | 海康威视 | USB 2.0 |
| **USB 扩展坞** | 绿联 | USB 2.0 |

### 📁 项目结构

```
TopCarBoard_M/
├── 📁 根目录
│   ├── main.cpp                    # 应用程序入口
│   ├── mainwidget.cpp/.h          # 主菜单界面
│   └── TopCarBoard_M.pro          # Qt 项目配置文件
│
├── 📊 Dashboard/                   # 仪表盘模块
│   ├── widget.cpp/.h/.ui          # 仪表盘界面实现
│   ├── res.qrc                    # 资源文件
│   └── 🖼️ icons/                  # 仪表盘图标和图片
│
├── 🗺️ QMapDemo/                   # 地图显示模块
│   ├── multidemo.cpp/.h           # 地图组件实现
│   ├── gps_modul.cpp/.h           # GPS 集成
│   └── src/QMapControl/           # 开源 QMapControl 库
│
├── 📡 GPSProcess/                  # GPS 数据处理
│   ├── GPSProcess.cpp/.h          # NMEA 协议解析
│   └── 与 GPS 模块的串口通信
│
├── 📱 IMUProcess/                  # IMU 传感器处理
│   ├── IMUProcess.cpp/.h          # 加速度计、陀螺仪数据
│   └── 运动检测和报警
│
├── 📹 UVC/                        # USB 摄像头模块
│   ├── processImage.cpp/.h       # 图像处理
│   ├── videodevice.cpp/.h         # V4L2 摄像头接口
│   └── YUV 到 RGB 转换
│
├── 🌤️ Weather/                    # 天气信息
│   ├── mainwindow.cpp/.h/.ui      # 天气界面
│   ├── weatherdata.h              # 数据结构
│   └── 基于网络的天气 API
│
└── 🔧 bin/                        # 编译后的可执行文件
    └── TopCarBoard_M
```

### 🔧 模块详细说明

#### 🏠 MainWidget（中央控制器）
- **功能**：主菜单界面，包含 6 个功能模块
- **特性**：
  - 网格式图标布局（2×3）
  - 模块导航和窗口管理
  - 自动清理的资源管理

#### 📊 仪表盘模块
- **功能**：实时仪表盘显示
- **特性**：
  - 指针动画的模拟速度表
  - 燃油液位和温度表
  - LED 指示灯控制（转向信号灯、危险警告灯）
  - 基于 ADC 的电位器速度输入

#### 📡 GPS 处理模块
- **功能**：实时 GPS 数据采集和解析
- **协议**：NMEA 0183（$GNGGA、$GNVTG 语句）
- **特性**：
  - 坐标解析（经度/纬度）
  - 速度计算（节/公里/小时）
  - 卫星数量和信号质量
  - 异步串口通信

#### 📱 IMU 处理模块
- **功能**：惯性测量和运动检测
- **特性**：
  - 3 轴加速度计数据
  - 3 轴陀螺仪数据
  - 计算角度信息
  - 碰撞检测与蜂鸣器报警

#### 📹 摄像头模块
- **功能**：USB 摄像头视频流处理
- **技术**：Video4Linux2 (V4L2) API
- **特性**：
  - 实时视频捕获
  - YUV422 到 RGB24 色彩空间转换
  - 帧率优化
  - 错误处理和设备管理

#### 🗺️ 地图显示模块
- **功能**：交互式地图可视化
- **技术**：QMapControl 库
- **特性**：
  - 基于瓦片的地图渲染
  - 在线地图数据获取
  - GPS 位置叠加
  - 缩放和平移功能

#### 🌤️ 天气模块
- **功能**：实时天气信息显示
- **特性**：
  - 基于网络的天气 API 集成
  - 温度和湿度显示
  - 天气图标可视化
  - JSON 数据解析

### 🚀 安装和配置

#### 1. 内核配置
进入 iTop4412_Kernel_3.0 目录并配置内核：

```bash
make menuconfig
```

启用以下驱动程序：

**CP210x USB 转串口驱动：**
```
Device Drivers --->
    [*] USB support --->
        USB Serial Converter support ---> 
            <*> USB CP210x family of UART Bridge Controllers
```

**CH341 USB 转串口驱动：**
```
Device Drivers --->
    [*] USB support ---> 
        USB Serial Converter support ---> 
            <*> USB CH341 support
```

编译内核：
```bash
make zImage
```

使用 fastboot 烧录内核：
```bash
fastboot flash kernel zImage
```

#### 2. 交叉编译
在 Ubuntu 16.04 环境中：

```bash
# 进入项目目录
cd TopCarBoard_M/

# 生成 Makefile
qmake

# 编译项目
make

# 可执行文件将在 bin/ 目录中生成
```

#### 3. 部署

1. **准备目标板：**
   - 烧录修改后的内核和 QTE5.7 文件系统
   - 连接调试串口到 PC
   - 开启开发板电源

2. **网络设置：**
   - 配置 PC 和开发板之间的以太网连接
   - 设置 PC IP：`192.168.137.1`
   - 设置开发板 IP：`192.168.137.2`
   - 设置开发板网关：`192.168.137.1`

3. **硬件连接：**
   - 将 USB 扩展坞连接到 iTop4412
   - 将 GPS 模块、IMU 传感器、USB 摄像头和鼠标连接到扩展坞
   - 确保设备正确枚举（`/dev/ttyUSB0`、`/dev/ttyUSB1`）

4. **文件传输和执行：**
   ```bash
   # 将项目复制到 U 盘，然后复制到开发板
   # 在目标板上：
   chmod +x TopCarBoard_M
   ./TopCarBoard_M
   ```

### 🧪 测试和验证

| 模块 | 测试标准 |
|------|----------|
| **仪表盘** | LED 控制、速度表对 ADC 输入的响应 |
| **GPS** | 坐标精度、速度计算 |
| **摄像头** | 实时视频流、颜色转换 |
| **IMU** | 运动检测、碰撞报警 |
| **地图** | 瓦片加载、GPS 位置叠加 |
| **天气** | 网络连接、数据解析 |

### ⚙️ 配置说明

- **设备映射**：IMU → `/dev/ttyUSB0`，GPS → `/dev/ttyUSB1`
- **波特率**：GPS (9600)，IMU (115200)
- **显示分辨率**：480×272 像素
- **网络代理**：配置为 HTTP 代理 `192.168.137.1:7890`

### 🎬 系统演示效果

以下图片展示了 TopCarBoard_M 系统的实际功能演示：

#### 📱 主界面显示
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image1.jpg" alt="主界面" width="600">
<p><em>主菜单界面，包含 6 个功能模块，采用 2×3 网格布局</em></p>
</div>

#### 📊 仪表盘面板
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image2.png" alt="仪表盘面板" width="600">
<p><em>交互式仪表盘，点击复选框可控制 LED，与仪表板转向灯、双闪灯同步闪烁。旋转滑动变阻器调节 ADC 采集电压值，速度实时更新</em></p>
</div>

#### 📹 USB 摄像头实时画面
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image3.png" alt="USB摄像头" width="600">
<p><em>USB 摄像头实时视频采集，YUV 到 RGB 颜色空间转换</em></p>
</div>

#### 🗺️ 交互式地图显示
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image4.png" alt="地图显示" width="600">
<p><em>实时地图加载，通过鼠标和鼠标滚轮调整地图画面。基于瓦片的地图渲染，交互流畅</em></p>
</div>

#### 📱 IMU 传感器数据可视化
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image5.png" alt="IMU数据" width="600">
<p><em>IMU 传感器三轴加速度、角速度、姿态角度数据显示</em></p>
</div>

#### ⚠️ 运动检测报警
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image6.png" alt="运动检测" width="600">
<p><em>IMU 运动检测功能演示 - 挥动 IMU 传感器模拟加速度剧烈变化，蜂鸣器鸣叫，显示屏显示警告信息</em></p>
</div>

#### 📡 GPS 定位系统
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image7.png" alt="GPS定位" width="600">
<p><em>GPS 定位数据显示，可采集当前经纬度等信息。与手机 GPS 对比，定位准确</em></p>
</div>

#### 🌤️ 天气时间信息
<div align="center">
<img src="https://github.com/IllusionMZX/4412-CarDashborad/blob/main/image/image8.png" alt="天气显示" width="600">
<p><em>通过网络获取实时天气和时间信息显示</em></p>
</div>

### 🔧 故障排除

**常见问题：**

1. **串口设备未找到**
   - 检查 USB 设备枚举：`lsusb`
   - 验证驱动加载：`lsmod | grep usbserial`

2. **摄像头初始化失败**
   - 检查 V4L2 设备：`ls /dev/video*`
   - 验证摄像头兼容性：`v4l2-ctl --list-devices`

3. **网络连接问题**
   - 测试基本连接：`ping 8.8.8.8`
   - 检查 main.cpp 中的代理配置

### 📄 许可证

本项目采用 MIT 许可证。详细信息请参阅 [LICENSE](LICENSE) 文件。

### 🤝 贡献

欢迎提交问题报告和功能请求。如需贡献代码，请：

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

### 📞 联系方式

如有任何问题或建议，请通过以下方式联系：

- 项目仓库：[GitHub](https://github.com/IllusionMZX/4412-CarDashborad)
- 问题报告：[Issues](https://github.com/IllusionMZX/4412-CarDashborad/issues)
