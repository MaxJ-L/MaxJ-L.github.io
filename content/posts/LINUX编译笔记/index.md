+++
date = '2025-09-22T02:15:29+08:00'
draft = true
title = 'LINUX编译笔记'
tags = ['LINUX']
summary= "使用WSL Ubuntu编译LINUX并用QEMU加载镜像"
+++

# LINUX编译流程


### 踩坑
+ 问题1：Ubuntu终端无法上网，ping通www.baidu.com
1. ping baidu.com ping 220.181.38.148
2. # 使用Google的DNS服务器，会覆盖原文件内容
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf