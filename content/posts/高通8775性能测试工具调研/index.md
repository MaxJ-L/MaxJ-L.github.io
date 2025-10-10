+++
date = '2025-10-10T21:03:37+08:00'
draft = false
title = '高通8775性能测试工具调研'
tags = ['Qualcomm','Performance']
summary= "高通8775性能测试工具调研"

+++

> 本篇文章主要是针对8775官方文档中的System Performance一章里面内容，学习文档中的性能测试工具用法。尽量结合高通文档《80-72626-100 Software Developer Resources for SA7255P, SA8255P, SA8620P, SA8650P, SA8775P HQX & QX Software Products》一起查看。
> 2025-07-24

System Performance的主要内容：
1. 如何挑选合适的Profiler工具（哪些工具能测哪些）；
2. 命令行工具 2 种 （SysProfiler、Sysmon）、 GUI工具两种（Snapdragon Profiler、Qprofiler） 
3. 具体的Profiler工具的使用指南（包括Qprofiler、SysProfiler和两种安卓通用的工具）；

---

## QProfiler
#### 安装：
从QPM上搜索Qualcomm Profiler安装即可；
+ 需要使用管理员打开QPM；
+ 安装有点慢，需要耐心等待；

#### 可以做什么?
+ QProfiler可以连接Android QNX等等系统，Android使用ADB即可；
+ 调研目前只尝试了Android系统，QProfiler可以看到SoC温度、CPU、内存、GPU、IO速度的实时指标，但是无法具体看进程等等细节，作为系统性能工具进行检测，尤其是GPU和IO；

#### 怎么使用？

##### 方案1
1. adb连接好
2. 连接WiFi，电脑也连接这个WiFi； 或者用网线直接连接（公司电脑以太网口好像是被禁用的，无法使用），最终目的是组成可以正常运行的局域网；
3. 运行"C:\Program Files (x86)\Qualcomm\Shared\QualcommProfiler\API\target-la\InstallerLA.exe" （8775 Android是对应target-la，其他的按照官方文档进行选择）
4. 运行QProfiler "C:\Program Files (x86)\Qualcomm\QualcommProfiler\GUI\qcprofiler.exe"
5. 填写车机Android的IP，端口填写62472；
6. 连接上之后选择指标、参数进行检测；

##### 方案2
1. adb连接好；
2. 运行"C:\Program Files (x86)\Qualcomm\Shared\QualcommProfiler\API\target-la\InstallerLA.exe" （8775 Android是对应target-la，其他的按照官方文档进行选择）
3. adb shell 输入以下命令
```bash
export QMONITOR_BACKEND_LIB_PATH=/vendor/qprof/backends 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/vendor/qprof/libs/
qprof --configure --server-ip 127.0.0.1  --port 9999
qprof --start-server
```
4. `adb forward tcp:9999 tcp:9999`
5. 运行QProfiler "C:\Program Files (x86)\Qualcomm\QualcommProfiler\GUI\qcprofiler.exe"
6. 填写  127.0.0.1 ，端口填写 9999；
7. 连接上之后选择指标、参数进行检测；

![Pasted image 20250724145402](./Pasted%20image%2020250724145402.png)

> 更多说明：
> 1. 高通文档 80-54323-2
> 2. 软件ABOUT里面的文档

> 基于gRPC框架实现，实际在机器开启服务端，局域网内工具通过获取机器数据
## Snapdragon Profiler
#### 安装
1. 从QPM下载，安装包下载好之后安装即可；
2. 安装好之后，还不可以直接使用，需要按照"Snapdragon Profiler\README-gtk.txt"下的Installation instructions安装GTK3；
```
----------------------- WINDOWS -----------------------
Windows users should run the Snapdragon Profiler installer (with an internet connection) and follow the prompts to install Gtk3.
Alternate instructions:

  1. Install gtk+3
    Download, unzip, and place in %LOCALAPPDATA%\Gtk\3.24.24: https://github.com/GtkSharp/Dependencies/raw/master/gtk-3.24.24.zip
  2. Go to https://www.nuget.org/downloads and download the recommended version of nuget.exe under "Windows x86 Commandline"
  3. Install gtk#3
    Open a command prompt
    > cd to your downloads directory
    > nuget.exe install gtksourcesharp -Version 3.24.24.38 -OutputDirectory %LOCALAPPDATA%\SDP
  4. If you have installed Snapdragon Profiler to a directory that requires elevated privileges (like Program Files), Profiler cannot automatically
     copy gtk#3 to its root directory for you on startup. You must copy the 8 dlls yourself from 
	 i.e. %LOCALAPPDATA%\SDP\AtkSharp.3.24.24.38\lib\netstandard2.0\AtkSharp.dll => C:\Program Files (x86)\Qualcomm\Snapdragon Profiler\AtkSharp.dll.
```

#### 可以做什么？
![Pasted image 20250724150144](./Pasted%20image%2020250724150144.png)
1. Trace采集；
2. 实时性能分析；
3. CPU采样；
4. GPU帧捕获；
相关的性能指标比QProfiler会多，从Android性能检测的指标来看，可以按照捕获具体进程的指标，进程指标可以捕获CPU和内存，系统指标可以捕获CPU、GPU、内存、网络（主要为WiFi）

#### 使用
+ Android
1. adb连接正常
2. 按照文档，adb shell `setprop vendor.egl.profiler 1` 配置可以检测； 不同的系统有不同的配置方法，具体见软件的文档参考；
3. Profiler File-Connect自动检测到，点击Connect连接即可；
4. 按照页面自行捕获数据即可；

> 其他系统尚无尝试过，页面需要填入HOST IP地址，应该也是需要组成局域网才可以；

> 参考文档：
> 1. 软件Help中的文档；
> 2. 高通文档 80-PG469-15
> 3. 高通文档 80-71528-1
> 4. 高通文档 80-72626-100 《Software Developer Resources for SA7255P, SA8255P, SA8620P, SA8650P, SA8775P HQX & QX Software Products》 SystemPerformace一章


## SysProfiler命令行工具
#### 安装
+ QNX镜像内置的sysprofiler_app，只可以测QNX
+ Linux Android（LA/LV Mental、 LA/LV GVM）的指标， 需要提CASE向高通获取；
![Pasted image 20250724151838](./Pasted%20image%2020250724151838.png)

#### 可以做什么？
图中的指标都可以测试，有选项可以生成Profetto能看的json文件，也可以生成CSV等等；
![Pasted image 20250724151925](./Pasted%20image%2020250724151925.png)

#### 如何使用
+ 直接命令行使用
+ 具体参数说明，见高通文档 80-72626-100 《Software Developer Resources for SA7255P, SA8255P, SA8620P, SA8650P, SA8775P HQX & QX Software Products》 SystemPerformace -- Usage guide for profling tools -- SysProfiler


## Sysmon命令行工具
#### 安装
+ 无需安装，QNX系统内置 sysMonAppQNX

#### 可以做什么？
+ 没有详细的user guide文档，以下摘自 80-72626-100 一段翻译：
```
valeosysMonApp 是一个 QNX 可执行文件，它通过 FastRPC 与 aDSP、cDSP、mDSP、sDSP 或 NPU 子系统上的 Q6 进行交互，并为用户提供分析 Q6 工作负载、获取时钟信息、设置或删除核心和总线时钟、获取软件线程信息以及获取软件线程级配置文件统计信息等功能。

支持的作系统包括 LA GVM、LA metal、QNX 和 HGY。

指标：Sysmon 详细指标
```

#### 使用
+ QNX串口直接运行即可，没有专门文档说明，只能看--help参考；
+ 从描述来看Android应该用不上，有关联的部分只有上面 支持的工作系统包括LA GVM；
+ 查阅后Q6主要是DSP相关性能，教程可查看高通 VD80-50431-501 ；


## 参考文档
1. 80-72626-100 《Software Developer Resources for SA7255P, SA8255P, SA8620P, SA8650P, SA8775P HQX & QX Software Products》
2. KBA-240602194851《How to use sysmonapp on HQX》
3. VD80-50431-501 How to use sysmonapp on HQX视频
4. 80-54323-2 Qualcomm Profiler User Guide
5. KBA-220422011853 How to capture and parse sensor‘s sysMonapp log correctly
6. Snapdragon Profiler User Guide - Automotive Platform.pdf 软件内文档
7. Snapdragon Profiler User Guide.pdf 软件内文档
8. QCP User Guide.pdf 软件内文档
