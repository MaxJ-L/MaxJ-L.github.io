+++
date = '2025-09-28T22:28:18+08:00'
draft = false
title = 'WLS2编译AOSP笔记'
tags = ['AOSP', 'WSL2', '编译']
summary= "WSL2编译AOSP的笔记"
+++



# WSL2编译AOSP笔记

> 学了这么久，虽然是已经非常熟练的操作，但还是需要记录一下...
>
> 为什么选择WSL2，不选择VMWare，因为VMWare占用的资源太大了，WSL2对比下来，发现轻松很多。而且也方便用AS等软件直接打开源码。



## 相关概念

1. WSL2，Windows Subsystem for Linux，Windows操作系统上的一个功能，用来允许在Windows运行Linux环境；
2. AOSP，额看这篇笔记的应该都懂，Android Open Source Project， 主要指Android开源代码；



## 步骤

### WSL2安装和初始化

#### 安装

1. 以 **管理员身份** 打开 **PowerShell**。

   - *方法：在开始菜单搜索 "PowerShell"，右键点击，选择“以管理员身份运行”。*

2. 在 PowerShell 窗口中，输入以下命令并回车：

   PowerShell

   ```
   wsl --install
   ```

   这条命令会自动帮你完成以下所有事情：

   - **开启** “虚拟机平台” 功能。
   - **开启** “适用于 Linux 的 Windows 子系统” 功能。
   - **下载并安装** 最新版的 Linux 内核。
   - **设置 WSL2** 为默认版本。
   - **从微软商店下载并安装** 最新版的 Ubuntu 发行版。

3. 命令运行完毕后，**重启电脑**。



#### Ubuntu初始化

1. 搜索打开Ubuntu，或者在Terminal选项中打开；
2. 第一次启动会提示："Installing, this may take a few minutes..."；
3. 安装完成后，创建**初始账号和密码**；



#### 验证WSL版本

1. 打开**PowerShell**，输入以下

```
wsl -l -v
```

2. 你会看到类似输出：

```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

- 如果 `VERSION` 列显示为 **`2`**，说明一切正常，你已经成功用上了 WSL2。

- **【转换操作】** 如果 `VERSION` 列显示为 **`1`**，说明这是一个 WSL1 的旧实例，你需要手动将其**转换**为 WSL2。执行以下命令即可：

  PowerShell

  ```
  # 把 "Ubuntu" 替换成你列表中显示的名字
  wsl --set-version Ubuntu 2
  ```

  等待转换完成后，你的 Ubuntu 就成功升级到 WSL2 了。



---



### AOSP下载和编译

> 参考官网：[Try Android development  | Android Open Source Project](https://source.android.com/docs/setup/start)

#### 安装所需软件

```bash
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```

> 注意一下，需要每个都安装成功



#### 安装repo

安装repo有2种方式，一种是直接`sudo apt install repo`，但是一般这样安装，repo的版本都比较低，我一般通过源码进行安装；

```bash
export REPO=$(mktemp /tmp/repo.XXXXXXXXX)
curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo
gpg --recv-keys 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65
curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} && install -m 755 ${REPO} ~/bin/repo
```

查看repo版本

```bash
repo version
```



#### 下载源码

```bash
repo init --partial-clone -b android-latest-release -u https://android.googlesource.com/platform/manifest
# 这里是android新出的manifest名称，用android-latest-release替代main，来代表最新的释放版本；
# 也可以通过AOSP官网查看其他manifest分支

# --partial-clone 这个选项是Android用来替代--depth=1选项的，这个会智能地下载历史记录，但不会立马下载一些非必要的文件，只有在真正下载用到的时候才会进行下载源码

repo sync -c --no-tags --prune -j16
```



#### 编译源码

```bash
source build/envsetup.sh
lunch <COMBO>
# COMBO一般我编译sdk_car_x86_64-aosp_current-eng
m -j32
# -j是线程数，看你自己的CPU来确定，如果线程数太高，可能会编译失败，甚至ninja由于线程数过高直接被kill掉，这种情况连报错都不会显示
```



#### 启动

emulator即可。需要注意，如果只是输入emulator，它会根据环境变量去找对应的镜像，需要先source envsetup.sh和lunch；

```bash
emulator
```



