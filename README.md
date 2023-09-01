# ESP01-Homekit-Switch
# 原生直连Homekit-ESP01/01s继电器固件



## 使用说明
### 下载
    git clone https://github.com/TaylorLottner/esp01-homekit-switch.git

参考：
本项目示例型号为 `esp8266-01s`，如果使用其他型号，需要修改 `/devices/switch/main.c` 文件中的引脚定义
注意：使用之前需要先配置好 [esp-open-sdk](https://github.com/pfalcon/esp-open-sdk) 的编译环境<br>

### 编译固件
可使用docker进行固件编译<br>

启动一个新的容器并指定名称为esp `docker run -it --name esp jedie/esp-open-sdk:latest`<br>
将文件移动到容器中  `docker cp 主机目录 esp:容器目录`<br>
随后进入容器 `docker exec -it esp /bin/bash`<br>
cd移动到放置项目的文件夹中<br>
如：`cd esp01-homekit-switch`<br>
随后`make -C devices/switch all`<br>
编译完成会在 `/devices/switch/firmware` 目录下生成 `switch.bin` 文件<br>




### 刷写固件
#### Windows
1.安装python<br>

2.安装esptool

    pip install esptool
3.把 `/devices/switch/firmware` 目录下的三个文件：`rboot.bin` `blank_config.bin` `switch.bin` 复制到python根目录下<br>

4.清空Flash

    esptool.py -p [端口] earse_flash
    例：esptool.py -p COM4 earse_flash

5.烧录固件

    esptool.py -p [端口] -b 115200 write_flash -fs 1MB -fm dout -ff 40m 0x0 rboot.bin 0x1000 blank_config.bin 0x2000 switch.bin

### 连接HomeKit

#### 扫码连接
![switchcode](https://cdn.jsdelivr.net/gh/TaylorLottner/esp01-homekit-switch@main/qrcode.svg)

#### 手动输入
1.手机wifi搜索并连接名称为 `AshenSwitch-XXXXXXX` 的热点，配置wifi信息<br>

2.打开 `家庭` App<br>

点击右上角+号选择 `添加或扫描配件` 选择 `我没有或无法扫描代码` <br>

选择配件<br>

输入代码 `52101314` 等待连接完成

#### 此项目源于https://github.com/LeeLulin/esp-homekit-direct修改<br>
