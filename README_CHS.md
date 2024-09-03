# 语言

- ENG [English](README.md)
- CHS [简体中文](README_CHS.md)

---

# 原生 Homekit-ESP01/01s 继电器

## 概述

ESP01-Homekit-Switch 是一个开源项目，它允许你将 ESP8266 模块（例如 ESP01 或 ESP8266-01S）变成一个能够接入 Apple Homekit 的家庭自动化开关设备

## 快速开始

### 准备工作

- 确保你已经安装了 Git
- 安装 Python 以及必要的库 `esptool`
- 配置好用于编译的环境，推荐使用 `esp-open-sdk`
- 如果不使用宿主机编译，请使用 `Docker`

### 克隆项目

```bash
git clone https://github.com/TaylorLottner/esp01-homekit-switch.git
```

### 编译固件

#### 宿主机 Make 方式

安装 SDK 环境：
详见：[esp-open-sdk](https://github.com/pfalcon/esp-open-sdk)

```bash
cd esp01-homekit-switch
make -C devices/switch all
```

#### Docker 方式

1. **设置 Docker 环境**  
   使用 Docker 容器来编译可以简化环境配置的过程。

   ```bash
   docker run -itd --name esp larsks/esp-open-sdk:latest /bin/bash
   ```

2. **复制项目到容器**  
   将项目文件复制到 Docker 容器内。

   ```bash
   docker cp esp01-homekit-switch esp:/home/your_username/
   ```

3. **进入容器**  
   进入 Docker 容器内部执行编译命令。

   ```bash
   docker exec -it esp /bin/bash
   ```

4. **编译项目**  
   在容器内导航到项目的根目录并开始编译。
   ```bash
   cd esp01-homekit-switch
   make -C devices/switch all
   ```

编译完成后，在 `/devices/switch/firmware` 目录下将会生成 `switch.bin` 文件。

### 烧录固件

1. **安装 esptool**  
   如果还没有安装 esptool，请通过 pip 安装。

   ```bash
   pip install esptool
   ```

2. **准备固件文件**  
   确认 `/devices/switch/firmware` 目录下包含 `rboot.bin`, `blank_config.bin`, `switch.bin` 文件。

3. **擦除 Flash**  
   使用 esptool 清除设备上的旧固件。

   ```bash
   esptool.py -p [端口] erase_flash
   ```

4. **烧录新固件**  
   使用 esptool 烧录新的固件到设备。
   ```bash
   esptool.py -p [端口] -b 115200 write_flash -fs 1MB -fm dout -ff 40m 0x0 rboot.bin 0x1000 blank_config.bin 0x2000 switch.bin
   ```

### 连接到 HomeKit

- **扫码连接**  
  通过扫描提供的二维码连接设备到 HomeKit。
  ![二维码](qrcode.svg)

- **手动输入**  
  如果无法扫码，可以通过以下步骤手动添加设备：
  1.连接到设备创建的热点 `H.A.N-XXXXXXX`。
  2.打开 iPhone 上的“家庭”App。
  3.点击右上角的+号，选择“添加或扫描配件”。
  4.选择“我没有或无法扫描代码”。
  5.选择相应的配件，并输入配对代码 `52101314`。

特别感谢[LeeLulin/esp-homekit-direct](https://github.com/LeeLulin/esp-homekit-direct)
