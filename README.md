# 基于QT的嵌入式Linux车载中控系统

## 项目简介
`TopCarBoard_M`是一个基于 Qt 的车载中控项目，包含 GPS 数据处理、IMU 数据处理、地图显示、天气信息等功能。项目通过串口读取 GPS 和 IMU 数据，并在LCD显示屏上实时显示。


## 环境配置

- 交叉编译环境：Ubuntu 16.04
- Qt版本： 5.7.0
- 文件系统：讯为QTE5.7镜像
- Linux内核：参考iTop4412_Kernel_3.0内核进行修改
- 功能模块：
  - IMU传感器：亚博智能
  - GPS传感器：亚博智能
  - USB摄像头：海康威视
  - USB拓展坞：绿联


---

## 项目结构

```shell
TopCarBoard_M/
├── main.cpp
├── mainwidget.cpp
├── mainwidget.h
├── TopCarBoard_M.pro
├── bin/
│   └── TopCarBoard_M
├── Dashboard/
│   ├── res.qrc
│   ├── widget.cpp
│   ├── widget.h
│   └── widget.ui
├── GPSProcess/
│   ├── GPSProcess.cpp
│   └── ...
├── IMUProcess/
│   ├── IMUProcess.cpp
│   └── ...
├── QMapDemo/
│   ├── src/
│   ├── multidemo.cpp
│   └── ...
├── UVC/
|   ├── IMUProcess.cpp
│   └── ...
├── Weather/
│   └── ...
└── 修改后的内核镜像文件/
    └──zImage
```

### 1.根目录
- `main.cpp`: 程序入口文件，初始化应用程序。
- `mainwidget.cpp / mainwidget.h`: 主窗口的实现文件，负责应用程序的主要界面逻辑。
- `TopCarBoard_M.pro`: Qt 项目的配置文件，定义了项目的构建规则和依赖。

---

### 2.Dashboard/
仪表盘相关的资源和界面文件：
- `camera.png, dashboard.png, ...`: 仪表盘界面使用的图标和图片资源。
- `res.qrc`: Qt 资源文件，定义了项目中使用的图片资源。
- `widget.cpp / widget.h / widget.ui`: 仪表盘界面的实现文件，定义了仪表盘的布局和功能。

---

### 3.GPSProcess/
GPS 数据处理模块：
- `GPSProcess.cpp / GPSProcess.h`: 
  - 通过串口读取 GPS 数据（如 `$GNGGA` 和 `$GNVTG` NMEA 语句）。
  - 解析 GPS 数据，提取时间、经纬度、卫星数量、速度等信息。
  - 使用 `QSocketNotifier` 实现串口数据的异步读取。

---

### 4.IMUProcess/
IMU 数据处理模块：
- `IMUProcess.cpp / IMUProcess.h`: 
  - 通过串口读取 IMU 数据。
  - 解析 IMU 数据，提取姿态、加速度等信息。

---

### 5.MapDemo/
地图显示模块：

- `src/`目录: 
  - 存放开源项目QMapControl所需的库文件

- `multidemo.cpp/multidemo.h`:
  - 地图显示程序，通过瓦片地图服务器，在线加载地图。


---

### 6.UVC/
UVC 摄像头模块：

- `processImage.cpp/processImage.h`: 
  - 处理 UVC 摄像头的视频流。
- `videodevice.cpp/videodevice.h`:
  - USB摄像头设备初始化，使用Video4Linux2（V4L2）API来控制摄像头设备。


---

### 7.Weather/
天气信息模块：
- `mainwindow.cpp/mainwindow.h`: 
  - 获取并显示实时天气信息。
  - 提供温度、湿度、天气图标等数据。

---

### 8.修改后的内核镜像文件/
- 存放与项目定制化内核镜像文件zImage。

---

### 9.bin/

- 存放交叉编译后生成的可执行文件。

---

## 使用说明

1. **内核编译:**

   - 进入iTop4412_Kernel_3.0内核文件目录，终端输入`make menuconfig`命令，导航至以下选项，启用CP210x、CH341驱动。

     - ```shell
       Device Drivers --->
       	[*] USB support --->
       		USB Serial Converter support ---> 
       			<*> USB CP210x family of UART Bridge Controllers
       ```

     - ```shell
       Device Drivers --->
       	[*] USB support ---> 
       		USB Serial Converter support ---> 
       			<*> USB CH341 support
       ```
   
   - 终端输入命令`make zImage`生成内核镜像，使用`fastboot`将内核镜像烧录至开发板。
   
2. **构建项目**:
   - 使用 Qt Creator 打开 `TopCarBoard_M.pro` 文件，浏览工程项目。
   - 将项目工程导入Ubuntu系统中，进入工程目录，使用`qmake`命令编译生成`Makefile`，再使用`make`命令生成可执行文件，可执行文件名为`TopCarBoard_M`，项目文件夹中已将生成的可执行文件放至`/bin`目录下。
   
3. **运行程序**:

   - iTop4412开发板正确烧录内核镜像及文件系统镜像后，连接开发板调试串口至电脑，按下电源按钮，启动开发板。
   - 将包含可执行文件的工程文件夹拷贝至U盘，将U盘插入iTop4412开发板，U盘正确挂载后，将工程项目拷贝至开发板。
   - 开发板通过USB拓展坞连接到USB摄像头、GPS、IMU模块、鼠标。
   - 将开发板以太网口与电脑相连并配置IP地址、默认网关等网络信息，确保开发板能够与电脑ping通，并可访问互联网，如`ping www.baidu.com`可以ping通。
   - 进入U盘工程目录，终端输入`./TopCarBoard_M`运行可执行文件。

4. **功能测试**:

   - 仪表面板：检查仪表板显示是否正常，候选框能否控制LED，旋转滑动变阻器速度是否发生变化。
   - GPS 数据：检查是否正确经纬度、速度等信息。
   - 摄像头采集：检查摄像头是否能够正常调用并采集实时画面。
   - IMU 数据：检查是否正确显示姿态和加速度信息。
   - 地图显示：检查地图是否正确加载。
   - 天气信息：检查天气数据能否正确加载。

---

## 注意事项
- 在代码中将IMU传感器设备文件设为`/dev/ttyUSB0`，将GPS传感器设备文件设为`/dev/ttyUSB1`。具体配置需根据USB实际挂载情况进行设置。先插入的USB设备挂载为`ttyUSB0`，按插入顺序递增USB编号。
- 主机需共享WLAN网络至以太网，并配置主机以太网IP地址与开发板以太网IP地址处于同一个网段内，如配置主机IP地址为`192.168.137.1`，开发板IP地址为`192.168.137.2`，并配置开发板默认网关为`192.168.137.1`。
