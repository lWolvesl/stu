# Environment

[回到主页](../../README.md)

- 本次使用的嵌入式开发环境为`Clion + STLink + STM32CubeMX`,使用`STM32CubeMX`构建项目文件
- [参考文档1](https://blog.csdn.net/qq_41072660/article/details/122001734)
- [参考文档2](https://www.bilibili.com/read/cv18659729)

## STM32CubeMX

下载：[百度云盘](https://pan.baidu.com/s/1T6aYL9UTlZ21zHXL4ejBug?pwd=ne8m)

- 在此处选择所用的开发版
  - ![image-20230327200636660](http://cos.wolves.top/img/202303272006674_repeat_1679918796761__531363.png)

- 在`Code Generator`中第二栏的第一个选择如下，生成代码时会自动生成头文件，即可使用函数式编程
  - ![image-20230327200207629](http://cos.wolves.top/img/202303272002645_repeat_1679918527724__535205.png)

## Clion

- `Clion`激活，此教程可以直接从官网下最新版并进行破解
  - [激活教程](https://blog.idejihuo.com/jetbrains/intellij-idea-2022-3-activation-code-to-crack-the-latest-tutorial.html)
  - [文件下载](https://cos.wolves.top/tools/jetbrains.zip)

- 配置从`STM32CubeMX`生成的文件

![image-20230327195738300](http://cos.wolves.top/img/202303271957329_repeat_1679918258493__109247.png)

![image-20230327195845004](http://cos.wolves.top/img/202303271958033_repeat_1679918325093__377506.png)

- 选择`copy`出的文件

  - ```cfg
    # SPDX-License-Identifier: GPL-2.0-or-later
    
    # This is an ST NUCLEO F103RB board with a single STM32F103RBT6 chip.
    # http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/LN1847/PF259875
    
    source [find interface/stlink-v2.cfg]
    
    transport select hla_swd
    
    source [find target/stm32f1x.cfg]
    
    reset_config none separate
    
    ```

  - 将第一个`source`中的`[find interface/stlink-v2.cfg]`改成你所用的`STLink`的类型

## STLink

- OpenOCD中含有`STLink`

- 使用`homebrew`安装`ARM toolchain`和`OpenOCD`
- 安装`homebrew`

```/bin/zsh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"```


- 安装`ARM toolchain`
  ```shell
  brew tap ArmMbed/homebrew-formulae   
  brew install arm-none-eabi-gcc
  //安装完成测试
  arm-none-eabi-gcc -v
  ```

- 安装`OpenOCD`
    ```shell
    brew install open-ocd
    ```

- 安装控制台驱动

  ```shell
  brew install stlink
  ```

  - 测试

    ```shell
    //探针测试
    st-info --probe
    //其他命令
    st-info
    
    st-info --version
    st-info --probe [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    st-info --serial
    st-info --flash  [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    st-info --pagesize  [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    st-info --sram  [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    st-info --chipid  [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    st-info --descr  [--connect-under-reset] [--hot-plug] [--freq=<KHz>]
    ```

    
