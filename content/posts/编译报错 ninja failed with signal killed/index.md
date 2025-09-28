+++
date = '2025-09-28T22:28:30+08:00'
draft = false
title = '编译报错 Ninja Failed With Signal Killed'
tags = ['AOSP', 'WSL2', '编译']
summary= "AOSP编译错误：Ninja Failed With Signal Killed"

+++

# AOSP编译错误：Ninja Failed With Signal Killed

![Pasted image 20250320101451](./Pasted%20image%2020250320101451.png)

#### 原因和解决办法：线程太多导致

网络上大部分的原因基本上都是Android服务器环境内存不足，需要检查内存或者通过检查内存交换区来进行解决；然而在这里不起作用。

在ninja killed的时候，部分时候Ubuntu的确是卡顿或者卡死的， VMWare分配了32GB内存，且基本上没有开太多的任务，因此不是内存的原因。

我是通过写了个简单的脚本进行编译的，这里-j线程改成了16，后面改成8就OK了，估计是线程太多导致Linux卡顿，系统杀死卡顿进程导致。

```bash
source build/envsetup.sh
lunch aosp_arm64-trunk_staging-eng
make -j16 2>&1 |tee build2.log
```

#### 网上其他原因以及解决办法

网络上大部分是因为内存问题导致，通过新建内存交换区解决。

```bash
#!/bin/bash
# 建立swap文件

# 查看目前swap
free -m

# 建立swap文件,大小4G
dd if=/dev/zero of=/var/swapfile bs=1024 count=4000000

# 启用虚拟内存,将swap文件设置为swap分区文件
mkswap /var/swapfile

#注意：insecure permissions 0644, 0600 suggested.
chmod 600 /var/swapfile

# 激活swap,启用分区交换文件
swapon /var/swapfile

# 停用虚拟内存
#swapoff /var/swapfile

# 查看内存和虚拟内存
free -m

# 开机启用swap

echo '/var/swapfile swap swap defaults 0 0' >> /etc/fstab

# 查看目前swap
free -m
```



```bash
FAILED: out/soong/.intermediates/external/crosvm/devices/libdevices/android_arm64_armv8-a_rlib_rlib-std_apex10000/7911d933735aaf6db37b7bd96172f083/libdevices.rlib
OUT_DIR=out ANDROID_RUST_VERSION=1.78.0 CARGO_CRATE_NAME=devices CARGO_PKG_NAME=devices CARGO_PKG_VERSION=0.1.0 CARGO_PKG_VERSION_MAJOR=0 CARGO_PKG_VERSION_MINOR=1 CARGO_PKG_VERSION_PATCH=0 prebuilts/rust/linux-x86/1.78.0/bin/rustc -C linker=prebuilts/clang/host/linux-x86/clang-r522817/bin/clang++ -C link-args=" -Wl,--as-needed -target aarch64-linux-android -
# ...
out/soong/.intermediates/external/crosvm/net_util/libnet_util/android_arm64_armv8-a_rlib_rlib-std_apex10000/582736703e6423fcea154bd3703d3915/ -Z stack-protector=strong -Z remap-cwd-prefix=. -C debuginfo=2 -C opt-level=3 -C relocation-model=pic -C overflow-checks=on -C force-unwind-tables=yes -C symbol-mangling-version=v0 --color=always -Z dylib-lto -Z link-native-libraries=no --cfg soong -C force-frame-pointers=y  -C panic=abort -Z debug-info-for-profiling -Z tls-model=global-dynamic --cap-lints allow --edition=2021 -C metadata=libdevices --cfg 'feature="android_display"' --cfg 'feature="android_display_stub"' --cfg 'feature="audio"' --cfg 'feature="audio_aaudio"' --cfg 'feature="balloon"' --cfg 'feature="geniezone"' --cfg 'feature="gfxstream"' --cfg 'feature="gpu"' --cfg 'feature="gpu_display"' --cfg 'feature="gunyah"' --cfg 'feature="net"' --cfg 'feature="usb"' --cfg 'feature="virgl_renderer"' --crate-type=rlib --crate-name=devices --target=aarch64-linux-android --sysroot=/dev/null -C codegen-units=1 && grep ^out/soong/.intermediates/external/crosvm/devices/libdevices/android_arm64_armv8-a_rlib_rlib-std_apex10000/7911d933735aaf6db37b7bd96172f083/libdevices.rlib: out/soong/.intermediates/external/crosvm/devices/libdevices/android_arm64_armv8-a_rlib_rlib-std_apex10000/7911d933735aaf6db37b7bd96172f083/libdevices.rlib.d.raw > out/soong/.intermediates/external/crosvm/devices/libdevices/android_arm64_armv8-a_rlib_rlib-std_apex10000/7911d933735aaf6db37b7bd96172f083/libdevices.rlib.d
error: failed to build archive: No such file or directory

error: aborting due to 1 previous error

...

ninja: build stopped: subcommand failed.
03:49:38 ninja failed with: exit status 1

#### failed to build some targets (12:25 (mm:ss)) ####

```
