+++
date = '2025-10-10T20:59:54+08:00'
draft = false
title = 'libncurses5编译报错'
tags = ['AOSP','编译']
summary= "libncurses5编译报错"
+++

#### 现象：

编译镜像没有super.img

#### 排查过程：

1. 脚本中generate_super报错，无法找到otatools等工具，对比正常情况，工具在lagvm_qssi下的out/dist/目录下 ---> qssi编译失败；
2. 直接编译qssi系统， ./build.sh dist -j32 --qssi_only， 出现报错：
  ![Pasted image 20250317093711](./Pasted%20image%2020250317093711.png)
3. 出现的so库loading失败，基本so库都是libncurses.so.5，相关工具比较多像是编译工具，如llvm等；

#### 解决办法

重装libncurses5即可；

```bash
sudo apt-get install libncurses5
```
