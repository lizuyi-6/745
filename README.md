![](https://pengzhihui-markdown.oss-cn-shanghai.aliyuncs.com/img/20210123154019.png)

# HoloCubic--多功能透明显示屏桌面站

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/35880ce2c5a74f28b4c647412c3afe48)](https://app.codacy.com/gh/lizuyi-6/745?utm_source=github.com&utm_medium=referral&utm_content=lizuyi-6/745&utm_campaign=Badge_Grade_Settings)

**视频介绍：** https://www.bilibili.com/video/BV1VA411p7MD/

## 0. 关于本项目

这是为了更新视频，缓解本人拖更两个月的尴尬，用一个周末时间赶出来的一个有意思的小玩意。

总的来说功能比较多，显示方式也十分炫酷，大家可以基于我的方案继续扩展实现更多功能。本项目的硬件方案是基于ESP32PICO-D4的，一个很实用的SiP芯片，也因此整板面积能做到一个硬币大小；软件方面主要是基于lvgl-GUI库，移植了ST7789 1.3寸240x240分辨率屏幕的显示驱动，同时将MPU6050作为输入设备，通过感应的方式模拟编码器键值。

## 1. 硬件打样说明

**暂时没发现有啥需要特别注意的。**PCB可以直接拿去打样，两层板很便宜，器件BOM的话也都是比较常用的，整板成本在30以内。

## 2. 固件编译说明

玩过Arduino的基本没有上手难度了，把Firmware/Libraries里面的库安装到Arduino库目录（如果你用的是Arduino IDE的话），然后这里需要修改一个官方库文件才能正常使用：

首先肯定得安装ESP32的Arduino支持包（百度有海量教程），然后在安装的支持包的`esp32\hardware\esp32\1.0.4\libraries\SPI\src\SPI.cpp`文件中，**修改以下代码中的MISO为26**：

    if(sck == -1 && miso == -1 && mosi == -1 && ss == -1) {
        _sck = (_spi_num == VSPI) ? SCK : 14;
        _miso = (_spi_num == VSPI) ? MISO : 12; // 需要改为26
        _mosi = (_spi_num == VSPI) ? MOSI : 13;
        _ss = (_spi_num == VSPI) ? SS : 15;
这是因为，硬件上连接屏幕和SD卡分别是用两个硬件SPI，其中HSPI的默认MISO引脚是12，而12在ESP32中是用于上电时设置flash电平的，上电之前上拉会导致芯片无法启动，因此我们将默认的引脚替换为26。

> 也可以通过设置芯片熔丝的方式解决这个问题，不过那样的操作时一次性的，不建议这么玩。

**另外：**

由于我赶视频制作，代码都是临时写的非常杂乱有很多dirty code，因此仓库中的是所有驱动调通之后的模板代码，可以自己基于这个框架自由开发。

**我后面有空了整理好APP应用代码也会更新出来。**

## 3. 关于分光棱镜

我用的时25.4mm x 25.4mm x 25.4mm的棱镜，淘宝应该可以搜到，单个价格80元左右。

## 其他的后续再补充，有用的话记得点星星~