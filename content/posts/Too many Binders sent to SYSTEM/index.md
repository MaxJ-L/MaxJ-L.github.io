+++
date = '2025-10-10T19:10:00+08:00'
draft = false
title = 'Too Many Binders Sent to SYSTEM'
tags = ['Android','Crash']
summary= "Too Many Binders Sent to SYSTEM"

+++

### 现象：
+ Launcher崩溃，黑屏重启

### 分析：
1. 直接原因：Launcher崩溃
2. 原因： AMS中判断U10用户过多Binder，导致U10用户的进程被回收。正常Bidner应该不会过多，一般是APP操作不当导致。

```
04-11 01:07:00.934  1292  1386 I ActivityManager: Killing 2575:com.maxj.app.maxjlauncher/u10s1000 (adj 0): Too many Binders sent to SYSTEM
04-11 01:07:00.934  1292  1386 I am_kill : [10,2575,com.maxj.app.maxjlauncher,0,Too many Binders sent to SYSTEM]
```

3. 继续排查日志，发现ConnectivityManager一直在打印
```
	Line  30294: 04-11 01:04:12.969  2915  2915 E ConnectivityManager: NetworkCallback was already registered
	Line  30297: 04-11 01:04:12.976  2915  2915 E NetworkChangeListener: startListening: null
	Line  30298: 04-11 01:04:12.976  2915  2915 E ConnectivityManager: NetworkCallback was already registered
	Line  30301: 04-11 01:04:12.982  2915  2915 E NetworkChangeListener: startListening: null
```
定位PID进程为txzadapter，通知APP解决；
