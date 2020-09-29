# MEISSA_TK1_MB

## 硬件

![board](images/board.png)



| 项目 | 参数 | 备注       |
| ---- | ---- | ---------- |
| 电源 | 12V  | 接口为XT30 |
| CPU  |      |            |
| RAM  |      |            |
| ROM  |      |            |

网口需要定制，接线图如下。这个是按附带的剪线插头的颜色推的，只在原安卓系统下测试过，安卓能显示有线连接图标，但未能联网。（刷系统不需要网线）

> 2020/9/29, 这个线序在小觅的tx1主板上可用，估计tk1也是没有问题的

![ethernet_pinout](images/ethernet_pinout.png)



## 备份系统

> 参考：http://arnon.dk/restoring-a-jetson-tk1-or-how-our-first-jetson-tk1-just-died/

### 准备

- linux系统的PC
- usb线
- meissa tk1主板
- L4T包：https://developer.nvidia.com/embedded/dlc/tk1-driver-package-r218



### 步骤

- 通过usb线将meissa tk1主板连接PC

- meissa tk1主板进入recovery模式

- 主要通过nvflash，这个工具在L4T解压后的bootloader下。

- 获取分区号

  ```
  ./nvflash --getpartitiontable table --bl ardbeg/fastboot.bin --go
  ```

  此时会得到一个table文件，找到里面APP分区的ID号，比如为12

- 读取分区，注意这里的12是上面的分区ID号

  ```
  sudo ./nvflash --read 12 system.img --bl ardbeg/fastboot.bin --go
  ```

  结束后，在当前目录会有一个system.img文件，大小约为14GB





## 安装系统

基本按照官方[Quick Start Guides](https://developer.nvidia.com/embedded/dlc/quick-start-guide-r218)操作。有几个注意点

1. 进入recovery模式的方式为，通电状态下，按住recovery按后点一下reset即可。如果成功，PC端会显示识别到如下usb设备

   ```
   Bus 001 Device 029: ID 0955:7740 NVidia Corp. 
   ```

   识别到这个设备后可以用根据官方方法烧写系统

2. 另外由于这个是非官方的板子，因此需要把jetson-tk1.conf这个配置文件下的BOARDID参数的注释去掉。



## 目前问题

刷入官方系统后无法启动，可能是电源管理的问题。显示如下

![](images/startup_failed.jpeg)