+++
date = '2025-09-22T23:56:20+08:00'
draft = true
title = 'Snippets'
tags = []
summary= ""
+++



# Snippets



### 开启WSL2

好的，没问题。

这里为您整理了一份从零开始，在 Windows 上开启 WSL2 并用上 Ubuntu 的完整核心流程。

------



### **Windows 启用 WSL2 并安装 Ubuntu 完整流程**





#### **第 1 步：开启 Windows 核心功能 (一键完成)**



这是最关键的一步，现在微软已经把所有需要的操作都集成到了一条命令里。

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

3. 命令运行完毕后，**重启你的电脑**。



#### **第 2 步：初始化 Ubuntu**



1. 重启电脑后，系统可能会自动启动 Ubuntu 窗口进行最后的安装。
2. 如果没有，就从 **开始菜单** 找到并打开 **Ubuntu**。
3. 第一次启动时，它会提示你 "Installing, this may take a few minutes..."。
4. 安装完成后，会要求你**创建一个 Unix 用户名和密码**。
   - *注意：这个密码是你在 Ubuntu 环境里使用 `sudo` 命令时需要的，输入时屏幕上不会显示，这是正常的。*

创建完成后，你就正式进入了 Ubuntu 的命令行环境！



#### **第 3 步：验证与版本转换 (检查与备用)**



通常经过以上步骤，所有新安装的系统都会是 WSL2。但验证一下总没错，并且如果你有旧的系统需要转换，也可以用下面的命令。

1. 打开 **PowerShell** 或 **CMD** (不是 Ubuntu 窗口)。

2. 输入命令查看所有已安装的 Linux 系统及其 WSL 版本：

   PowerShell

   ```
   wsl -l -v
   ```

3. 你会看到类似输出：

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





# 下载REPO和源代码

```
repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r1 --partial-clone --clone-filter=blob:none
```

1. ``





repo init --partial-clone -b android16-release -u https://android.googlesource.com/platform/manifest --partial-clone --clone-filter=blob:none

repo sync -c --no-tags --prune -j16





---

LINUX

sudo apt-get update
   99  sudo apt-get install build-essential libncurses-dev bison flex libssl-dev libelf-dev dwarves
  100  mkdir -p ~/kernel-dev
  101  cd ~/kernel-dev
  102  wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.16.8.tar.xz
  103  tar -xvf  linux-6.16.8.tar.xz
  104  cd linux-6.16.8/
  105  make -j 32
  106  export ARCH=arm
  107  export CROSS_COMPILE=arm-linux-gnueabihf-
  108  make versatile_defconfig
  109  make -j$(nproc)
  110  make -j 32
  111  sudo apt-get install gcc-arm-linux-gnueabihf
  112  make versatile_defconfig
  113  make -j 32
  114  qemu-system-arm     -M versatilepb     -kernel arch/arm/boot/zImage     -nographic     -append "console=ttyAMA0"
  115  sudo apt install qemu-system-arm

---

~/kernel-dev/linux-6.16.8$ qemu-system-arm \
    -M versatilepb \
    -kernel arch/arm/boot/zImage \
    -nographic \
    -append "console=ttyAMA0"
[W][07412.642735] pw.conf      | [          conf.c: 1031 try_load_conf()] can't load config client.conf: No such file or directory
[E][07412.643060] pw.conf      | [          conf.c: 1060 pw_conf_load_conf_for_context()] can't load config client.conf: No such file or directory
pulseaudio: set_sink_input_volume() failed
pulseaudio: Reason: Invalid argument
pulseaudio: set_sink_input_mute() failed
pulseaudio: Reason: Invalid argument

Error: invalid dtb and unrecognized/unsupported machine ID
  r1=0x00000183, r2=0x00000100
  r2[]=05 00 00 00 01 00 41 54 01 00 00 00 00 10 00 00
Available machine support:

ID (hex)        NAME
ffffffff        Generic DT based system
ffffffff        ARM-Versatile (Device Tree Support)

Please check your kernel config and/or bootloader.



---

```
qemu-system-aarch64 \ 还是不行


cd linux-6.16.8/
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ ls
COPYING        Kbuild    MAINTAINERS     README      block       crypto   include   ipc     mm                       modules.order  samples   sound  virt       vmlinux.o
CREDITS        Kconfig   Makefile        System.map  built-in.a  drivers  init      kernel  modules.builtin          net            scripts   tools  vmlinux
Documentation  LICENSES  Module.symvers  arch        certs       fs       io_uring  lib     modules.builtin.modinfo  rust           security  usr    vmlinux.a
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ export ARCH=arm64
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ export CROSS_COMPILE=aarch64-linux-gnu-
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ make defconfig
*** Default configuration is based on 'defconfig'
#
# No change to .config
#
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ make -j$(nproc)
  CALL    scripts/checksyscalls.sh
maxj@DESKTOP-NKC3K33:~/kernel-dev/linux-6.16.8$ qemu-system-aarch64 \
    -M virt \
    -cpu cortex-a57 \
    -kernel arch/arm64/boot/Image \
    -nographic \
    -append "console=ttyAMA0"
[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x411fd070]
[    0.000000] Linux version 6.16.8 (maxj@DESKTOP-NKC3K33) (aarch64-linux-gnu-gcc (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0, GNU ld (GNU Binutils for Ubuntu) 2.42) #2 SMP PREEMPT Tue Sep 23 02:36:26 CST 2025
[    0.000000] KASLR enabled
[    0.000000] random: crng init done
[    0.000000] Machine model: linux,dummy-virt
[    0.000000] efi: UEFI not found.
[    0.000000] OF: reserved mem: Reserved memory: No reserved-memory node in the DT
[    0.000000] NUMA: Faking a node at [mem 0x0000000040000000-0x0000000047ffffff]
[    0.000000] NODE_DATA(0) allocated [mem 0x47fbcb40-0x47fbf1bf]
[    0.000000] Zone ranges:
[    0.000000]   DMA      [mem 0x0000000040000000-0x0000000047ffffff]
[    0.000000]   DMA32    empty
[    0.000000]   Normal   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000040000000-0x0000000047ffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000040000000-0x0000000047ffffff]
[    0.000000] cma: Reserved 32 MiB at 0x0000000045c00000
[    0.000000] psci: probing for conduit method from DT.
[    0.000000] psci: PSCIv1.1 detected in firmware.
[    0.000000] psci: Using standard PSCI v0.2 function IDs
[    0.000000] psci: Trusted OS migration not required
[    0.000000] psci: SMC Calling Convention v1.0
[    0.000000] percpu: Embedded 25 pages/cpu s62232 r8192 d31976 u102400
[    0.000000] Detected PIPT I-cache on CPU0
[    0.000000] CPU features: detected: Spectre-v2
[    0.000000] CPU features: detected: Spectre-v3a
[    0.000000] CPU features: detected: Spectre-v4
[    0.000000] CPU features: detected: Spectre-BHB
[    0.000000] CPU features: kernel page table isolation forced ON by KASLR
[    0.000000] CPU features: detected: Kernel page table isolation (KPTI)
[    0.000000] CPU features: detected: ARM erratum 1742098
[    0.000000] CPU features: detected: ARM erratum 832075
[    0.000000] CPU features: detected: ARM errata 1165522, 1319367, or 1530923
[    0.000000] alternatives: applying boot alternatives
[    0.000000] Kernel command line: console=ttyAMA0
[    0.000000] printk: log buffer data + meta data: 131072 + 458752 = 589824 bytes
[    0.000000] Dentry cache hash table entries: 16384 (order: 5, 131072 bytes, linear)
[    0.000000] Inode-cache hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    0.000000] software IO TLB: SWIOTLB bounce buffer size adjusted to 0MB
[    0.000000] software IO TLB: area num 1.
[    0.000000] software IO TLB: mapped [mem 0x0000000047f1d000-0x0000000047f5d000] (0MB)
[    0.000000] Fallback order for Node 0: 0
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 32768
[    0.000000] Policy zone: DMA
[    0.000000] mem auto-init: stack:all(zero), heap alloc:off, heap free:off
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] rcu: Preemptible hierarchical RCU implementation.
[    0.000000] rcu:     RCU event tracing is enabled.
[    0.000000] rcu:     RCU restricting CPUs from NR_CPUS=512 to nr_cpu_ids=1.
[    0.000000]  Trampoline variant of Tasks RCU enabled.
[    0.000000]  Tracing variant of Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=1
[    0.000000] RCU Tasks: Setting shift to 0 and lim to 1 rcu_task_cb_adjust=1 rcu_task_cpu_ids=1.
[    0.000000] RCU Tasks Trace: Setting shift to 0 and lim to 1 rcu_task_cb_adjust=1 rcu_task_cpu_ids=1.
[    0.000000] NR_IRQS: 64, nr_irqs: 64, preallocated irqs: 0
[    0.000000] Root IRQ handler: gic_handle_irq
[    0.000000] GICv2m: range[mem 0x08020000-0x08020fff], SPI[80:143]
[    0.000000] rcu: srcu_init: Setting srcu_struct sizes based on contention.
[    0.000000] arch_timer: cp15 timer(s) running at 62.50MHz (virt).
[    0.000000] clocksource: arch_sys_counter: mask: 0x1ffffffffffffff max_cycles: 0x1cd42e208c, max_idle_ns: 881590405314 ns
[    0.000061] sched_clock: 57 bits at 63MHz, resolution 16ns, wraps every 4398046511096ns
[    0.006930] Console: colour dummy device 80x25
[    0.008903] Calibrating delay loop (skipped), value calculated using timer frequency.. 125.00 BogoMIPS (lpj=250000)
[    0.009269] pid_max: default: 32768 minimum: 301
[    0.011045] LSM: initializing lsm=capability
[    0.012816] Mount-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.012852] Mountpoint-cache hash table entries: 512 (order: 0, 4096 bytes, linear)
[    0.037917] cacheinfo: Unable to detect cache hierarchy for CPU 0
[    0.048050] rcu: Hierarchical SRCU implementation.
[    0.048089] rcu:     Max phase no-delay instances is 1000.
[    0.052648] EFI services will not be available.
[    0.053045] smp: Bringing up secondary CPUs ...
[    0.054176] smp: Brought up 1 node, 1 CPU
[    0.054197] SMP: Total of 1 processors activated.
[    0.054219] CPU: All CPU(s) started at EL1
[    0.054408] CPU features: detected: 32-bit EL0 Support
[    0.054417] CPU features: detected: 32-bit EL1 Support
[    0.054464] CPU features: detected: CRC32 instructions
[    0.054612] CPU features: detected: PMUv3
[    0.063747] alternatives: applying system-wide alternatives
[    0.081619] Memory: 45196K/131072K available (17856K kernel code, 5330K rwdata, 12584K rodata, 11392K init, 749K bss, 52040K reserved, 32768K cma-reserved)
[    0.106232] devtmpfs: initialized
[    0.125950] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    0.126492] posixtimers hash table entries: 512 (order: 1, 8192 bytes, linear)
[    0.126727] futex hash table entries: 256 (16384 bytes on 1 NUMA nodes, total 16 KiB, linear).
[    0.128709] 2G module region forced by RANDOMIZE_MODULE_REGION_FULL
[    0.128777] 0 pages in range for non-PLT usage
[    0.128809] 512256 pages in range for PLT usage
[    0.131854] pinctrl core: initialized pinctrl subsystem
[    0.140043] DMI not present or invalid.
[    0.153172] NET: Registered PF_NETLINK/PF_ROUTE protocol family
[    0.161808] DMA: preallocated 128 KiB GFP_KERNEL pool for atomic allocations
[    0.162197] DMA: preallocated 128 KiB GFP_KERNEL|GFP_DMA pool for atomic allocations
[    0.162377] DMA: preallocated 128 KiB GFP_KERNEL|GFP_DMA32 pool for atomic allocations
[    0.162556] audit: initializing netlink subsys (disabled)
[    0.164331] audit: type=2000 audit(0.144:1): state=initialized audit_enabled=0 res=1
[    0.168506] thermal_sys: Registered thermal governor 'step_wise'
[    0.168536] thermal_sys: Registered thermal governor 'power_allocator'
[    0.168831] cpuidle: using governor menu
[    0.169808] hw-breakpoint: found 6 breakpoint and 4 watchpoint registers.
[    0.170136] ASID allocator initialised with 32768 entries
[    0.174293] Serial: AMBA PL011 UART driver
[    0.213024] 9000000.pl011: ttyAMA0 at MMIO 0x9000000 (irq = 13, base_baud = 0) is a PL011 rev1
[    0.214945] printk: console [ttyAMA0] enabled
[    0.260458] HugeTLB: allocation took 0ms with hugepage_allocation_threads=1
[    0.261688] HugeTLB: allocation took 0ms with hugepage_allocation_threads=1
[    0.262566] HugeTLB: registered 1.00 GiB page size, pre-allocated 0 pages
[    0.263111] HugeTLB: 0 KiB vmemmap can be freed for a 1.00 GiB page
[    0.263472] HugeTLB: registered 32.0 MiB page size, pre-allocated 0 pages
[    0.263890] HugeTLB: 0 KiB vmemmap can be freed for a 32.0 MiB page
[    0.264217] HugeTLB: registered 2.00 MiB page size, pre-allocated 0 pages
[    0.264840] HugeTLB: 0 KiB vmemmap can be freed for a 2.00 MiB page
[    0.265287] HugeTLB: registered 64.0 KiB page size, pre-allocated 0 pages
[    0.265804] HugeTLB: 0 KiB vmemmap can be freed for a 64.0 KiB page
[    0.277002] ACPI: Interpreter disabled.
[    0.281969] iommu: Default domain type: Translated
[    0.282381] iommu: DMA domain TLB invalidation policy: strict mode
[    0.283912] SCSI subsystem initialized
[    0.286248] usbcore: registered new interface driver usbfs
[    0.286820] usbcore: registered new interface driver hub
[    0.287377] usbcore: registered new device driver usb
[    0.289416] pps_core: LinuxPPS API ver. 1 registered
[    0.289843] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.290458] PTP clock support registered
[    0.291198] EDAC MC: Ver: 3.0.0
[    0.292591] scmi_core: SCMI protocol bus registered
[    0.294799] FPGA manager framework
[    0.295711] Advanced Linux Sound Architecture Driver Initialized.
[    0.305922] vgaarb: loaded
[    0.308062] clocksource: Switched to clocksource arch_sys_counter
[    0.309618] VFS: Disk quotas dquot_6.6.0
[    0.309958] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
[    0.311821] pnp: PnP ACPI: disabled
[    0.330046] NET: Registered PF_INET protocol family
[    0.337213] IP idents hash table entries: 2048 (order: 2, 16384 bytes, linear)
[    0.343435] tcp_listen_portaddr_hash hash table entries: 256 (order: 0, 4096 bytes, linear)
[    0.344655] Table-perturb hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    0.345367] TCP established hash table entries: 1024 (order: 1, 8192 bytes, linear)
[    0.346334] TCP bind hash table entries: 1024 (order: 3, 32768 bytes, linear)
[    0.347393] TCP: Hash tables configured (established 1024 bind 1024)
[    0.350158] UDP hash table entries: 256 (order: 2, 16384 bytes, linear)
[    0.352321] UDP-Lite hash table entries: 256 (order: 2, 16384 bytes, linear)
[    0.354083] NET: Registered PF_UNIX/PF_LOCAL protocol family
[    0.357014] RPC: Registered named UNIX socket transport module.
[    0.357402] RPC: Registered udp transport module.
[    0.357668] RPC: Registered tcp transport module.
[    0.357944] RPC: Registered tcp-with-tls transport module.
[    0.358243] RPC: Registered tcp NFSv4.1 backchannel transport module.
[    0.358595] PCI: CLS 0 bytes, default 64
[    0.362672] kvm [1]: HYP mode not available
[    0.366618] Initialise system trusted keyrings
[    0.369726] workingset: timestamp_bits=42 max_order=15 bucket_order=0
[    0.372331] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.373959] NFS: Registering the id_resolver key type
[    0.374702] Key type id_resolver registered
[    0.375099] Key type id_legacy registered
[    0.375633] nfs4filelayout_init: NFSv4 File Layout Driver Registering...
[    0.376243] nfs4flexfilelayout_init: NFSv4 Flexfile Layout Driver Registering...
[    0.377240] 9p: Installing v9fs 9p2000 file system support
[    0.414430] Key type asymmetric registered
[    0.415138] Asymmetric key parser 'x509' registered
[    0.416234] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 245)
[    0.416916] io scheduler mq-deadline registered
[    0.417268] io scheduler kyber registered
[    0.417844] io scheduler bfq registered
[    0.438545] pl061_gpio 9030000.pl061: PL061 GPIO chip registered
[    0.442067] ledtrig-cpu: registered to indicate activity on CPUs
[    0.446165] pci-host-generic 4010000000.pcie: host bridge /pcie@10000000 ranges:
[    0.447142] pci-host-generic 4010000000.pcie:       IO 0x003eff0000..0x003effffff -> 0x0000000000
[    0.448827] pci-host-generic 4010000000.pcie:      MEM 0x0010000000..0x003efeffff -> 0x0010000000
[    0.449838] pci-host-generic 4010000000.pcie:      MEM 0x8000000000..0xffffffffff -> 0x8000000000
[    0.451359] pci-host-generic 4010000000.pcie: Memory resource size exceeds max for 32 bits
[    0.453104] pci-host-generic 4010000000.pcie: ECAM at [mem 0x4010000000-0x401fffffff] for [bus 00-ff]
[    0.455519] pci-host-generic 4010000000.pcie: PCI host bridge to bus 0000:00
[    0.456635] pci_bus 0000:00: root bus resource [bus 00-ff]
[    0.457352] pci_bus 0000:00: root bus resource [io  0x0000-0xffff]
[    0.457871] pci_bus 0000:00: root bus resource [mem 0x10000000-0x3efeffff]
[    0.458284] pci_bus 0000:00: root bus resource [mem 0x8000000000-0xffffffffff]
[    0.460499] pci 0000:00:00.0: [1b36:0008] type 00 class 0x060000 conventional PCI endpoint
[    0.464395] pci 0000:00:01.0: [1af4:1000] type 00 class 0x020000 conventional PCI endpoint
[    0.465161] pci 0000:00:01.0: BAR 0 [io  0x0000-0x001f]
[    0.465738] pci 0000:00:01.0: BAR 1 [mem 0x00000000-0x00000fff]
[    0.466249] pci 0000:00:01.0: BAR 4 [mem 0x00000000-0x00003fff 64bit pref]
[    0.467031] pci 0000:00:01.0: ROM [mem 0x00000000-0x0007ffff pref]
[    0.469931] pci 0000:00:01.0: ROM [mem 0x10000000-0x1007ffff pref]: assigned
[    0.470744] pci 0000:00:01.0: BAR 4 [mem 0x8000000000-0x8000003fff 64bit pref]: assigned
[    0.471676] pci 0000:00:01.0: BAR 1 [mem 0x10080000-0x10080fff]: assigned
[    0.472486] pci 0000:00:01.0: BAR 0 [io  0x1000-0x101f]: assigned
[    0.473140] pci_bus 0000:00: resource 4 [io  0x0000-0xffff]
[    0.473564] pci_bus 0000:00: resource 5 [mem 0x10000000-0x3efeffff]
[    0.473933] pci_bus 0000:00: resource 6 [mem 0x8000000000-0xffffffffff]
[    0.529145] virtio-pci 0000:00:01.0: enabling device (0000 -> 0003)
[    0.543333] Serial: 8250/16550 driver, 4 ports, IRQ sharing enabled
[    0.549985] msm_serial: driver initialized
[    0.551337] SuperH (H)SCI(F) driver initialized
[    0.552205] STM32 USART driver initialized
[    0.569607] loop: module loaded
[    0.572252] megasas: 07.734.00.00-rc1
[    0.576893] physmap-flash 0.flash: physmap platform flash device: [mem 0x00000000-0x03ffffff]
[    0.579080] 0.flash: Found 2 x16 devices at 0x0 in 32-bit bank. Manufacturer ID 0x000000 Chip ID 0x000000
[    0.580546] Intel/Sharp Extended Query Table at 0x0031
[    0.581997] Using buffer write method
[    0.583964] physmap-flash 0.flash: physmap platform flash device: [mem 0x04000000-0x07ffffff]
[    0.584987] 0.flash: Found 2 x16 devices at 0x0 in 32-bit bank. Manufacturer ID 0x000000 Chip ID 0x000000
[    0.586338] Intel/Sharp Extended Query Table at 0x0031
[    0.588235] Using buffer write method
[    0.588969] Concatenating MTD devices:
[    0.589333] (0): "0.flash"
[    0.589698] (1): "0.flash"
[    0.590110] into device "0.flash"
[    0.620300] tun: Universal TUN/TAP device driver, 1.6
[    0.639539] thunder_xcv, ver 1.0
[    0.640463] thunder_bgx, ver 1.0
[    0.641002] nicpf, ver 1.0
[    0.644554] hns3: Hisilicon Ethernet Network Driver for Hip08 Family - version
[    0.645321] hns3: Copyright (c) 2017 Huawei Corporation.
[    0.646347] hclge is initializing
[    0.646860] e1000: Intel(R) PRO/1000 Network Driver
[    0.647369] e1000: Copyright (c) 1999-2006 Intel Corporation.
[    0.648101] e1000e: Intel(R) PRO/1000 Network Driver
[    0.648607] e1000e: Copyright(c) 1999 - 2015 Intel Corporation.
[    0.649240] igb: Intel(R) Gigabit Ethernet Network Driver
[    0.649851] igb: Copyright (c) 2007-2014 Intel Corporation.
[    0.650733] igbvf: Intel(R) Gigabit Virtual Function Network Driver
[    0.651419] igbvf: Copyright (c) 2009 - 2012 Intel Corporation.
[    0.652913] sky2: driver version 1.30
[    0.656212] VFIO - User Level meta-driver version: 0.3
[    0.663187] usbcore: registered new interface driver usb-storage
[    0.672561] rtc-pl031 9010000.pl031: registered as rtc0
[    0.673648] rtc-pl031 9010000.pl031: setting system clock to 2025-09-23T14:54:59 UTC (1758639299)
[    0.676430] i2c_dev: i2c /dev entries driver
[    0.688855] sdhci: Secure Digital Host Controller Interface driver
[    0.689437] sdhci: Copyright(c) Pierre Ossman
[    0.691077] Synopsys Designware Multimedia Card Interface Driver
[    0.693577] sdhci-pltfm: SDHCI platform and OF driver helper
[    0.704681] usbcore: registered new interface driver usbhid
[    0.705486] usbhid: USB HID core driver
[    0.716276] hw perfevents: enabled with armv8_pmuv3 PMU driver, 7 (0,8000003f) counters available
[    0.729551] NET: Registered PF_PACKET protocol family
[    0.731297] 9pnet: Installing 9P2000 support
[    0.732149] Key type dns_resolver registered
[    0.764878] registered taskstats version 1
[    0.766663] Loading compiled-in X.509 certificates
[    0.788803] Demotion targets for Node 0: null
[    0.796999] input: gpio-keys as /devices/platform/gpio-keys/input/input0
[    0.803396] clk: Disabling unused clocks
[    0.804274] PM: genpd: Disabling unused power domains
[    0.805134] ALSA device list:
[    0.805438]   No soundcards found.
[    0.811430] /dev/root: Can't open blockdev
[    0.812317] VFS: Cannot open root device "" or unknown-block(0,0): error -6
[    0.812717] Please append a correct "root=" boot option; here are the available partitions:
[    0.813286] 1f00          131072 mtdblock0
[    0.813447]  (driver?)
[    0.813954] List of all bdev filesystems:
[    0.814219]  ext3
[    0.814264]  ext2
[    0.814497]  ext4
[    0.814600]  squashfs
[    0.814693]  vfat
[    0.814929]
[    0.815337] Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(0,0)
[    0.816839] CPU: 0 UID: 0 PID: 1 Comm: swapper/0 Not tainted 6.16.8 #2 PREEMPT
[    0.817828] Hardware name: linux,dummy-virt (DT)
[    0.818475] Call trace:
[    0.819051]  show_stack+0x18/0x24 (C)
[    0.819838]  dump_stack_lvl+0x34/0x8c
[    0.820089]  dump_stack+0x18/0x24
[    0.820730]  panic+0x388/0x3e8
[    0.821250]  mount_root_generic+0x274/0x354
[    0.822007]  mount_root+0x170/0x334
[    0.822150]  prepare_namespace+0x6c/0x2a4
[    0.822332]  kernel_init_freeable+0x28c/0x2cc
[    0.822705]  kernel_init+0x20/0x1d8
[    0.822912]  ret_from_fork+0x10/0x20
[    0.823490] Kernel Offset: 0x562e34400000 from 0xffff800080000000
[    0.823824] PHYS_OFFSET: 0xfff1000040000000
[    0.824317] CPU features: 0x1040,000804f0,02000800,0400421b
[    0.824700] Memory Limit: none
[    0.825368] ---[ end Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(0,0) ]---
```





---

```
wsl --set-version Ubuntu-24.04 2
```





echo 'export http_proxy="http://172.22.176.1:7890"' >> ~/.bashrc
echo 'export https_proxy="http://172.22.176.1:7890"' >> ~/.bashrc
echo 'export all_proxy="socks5://172.22.176.1:7890"' >> ~/.bashrc





---



### UBUNTU

sudo nano /etc/wsl.conf

[network]
generateResolvConf = false



```
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```



```
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev libc6-dev-i386 x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```



```
export REPO=$(mktemp /tmp/repo.XXXXXXXXX)
curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo
gpg --recv-keys 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65
curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} && install -m 755 ${REPO} ~/bin/repo
```

```
echo 'export PATH=~/bin:$PATH' >> ~/.bashrc
```

```
repo init --partial-clone -b android16-release -u https://android.googlesource.com/platform/manifest --partial-clone --clone-filter=blob:none

repo sync -c --no-tags --prune -j16
```

sdk_car_x86_64-ap1a-eng

trunk_staging

sdk_car_x86_64-aosp_current-eng

```
lunch aosp_cf_x86_64_only_phone-aosp_current-eng
```

sdk_phone_x86_64-aosp_current-eng

1. `repo init --partial-clone -b android-latest-release -u https://android.googlesource.com/platform/manifest`
2. 

repo init --partial-clone -b android16-release -u https://android.googlesource.com/platform/manifest



---

```
emulator
INFO         | Android emulator version 35.3.8.0 (build_id 12560773) (CL:N/A)
INFO         | Graphics backend: gfxstream
INFO         | Changing default hw.initialOrientation to landscape

INFO         | Checking system compatibility:
INFO         |   Checking: hasSufficientDiskSpace
INFO         |      Ok: Disk space requirements to run avd: `<build>` are met.
INFO         |   Checking: hasSufficientHwGpu
INFO         |      Ok: Hardware GPU requirements to run avd: `<build>` are passed.
INFO         |   Checking: hasSufficientSystem
INFO         |      Ok: System requirements to run avd: `<build>` are met.
WARNING      | Failed to open /home/maxj/.android/adbkey
WARNING      | adbkey generation failed
ProbeKVM: This user doesn't have permissions to use KVM (/dev/kvm).
The KVM line in /etc/group is: [kvm:x:993:]

If the current user has KVM permissions,
the KVM line in /etc/group should end with ":" followed by your username.

If we see LINE_NOT_FOUND, the kvm group may need to be created along with permissions:
    sudo groupadd -r kvm
    # Then ensure /lib/udev/rules.d/50-udev-default.rules contains something like:
    # KERNEL=="kvm", GROUP="kvm", MODE="0660"
    # and then run:
    sudo gpasswd -a $USER kvm

If we see kvm:... but no username at the end, running the following command may allow KVM access:
    sudo gpasswd -a $USER kvm

You may need to log out and back in for changes to take effect.

ERROR        | x86_64 emulation currently requires hardware acceleration!
CPU acceleration status: This user doesn't have permissions to use KVM (/dev/kvm).
The KVM line in /etc/group is: [kvm:x:993:]

If the current user has KVM permissions,
the KVM line in /etc/group should end with ":" followed by your username.

If we see LINE_NOT_FOUND, the kvm gr
More info on configuring VM acceleration on Linux:
https://developer.android.com/studio/run/emulator-acceleration#vm-linux
General information on acceleration: https://developer.android.com/studio/run/emulator-acceleration.
```

```
sudo gpasswd -a $USER kvm
wsl --shutdown

```



---



```
emulator
INFO         | Android emulator version 35.3.8.0 (build_id 12560773) (CL:N/A)
INFO         | Graphics backend: gfxstream
INFO         | Changing default hw.initialOrientation to landscape

INFO         | Checking system compatibility:
INFO         |   Checking: hasSufficientDiskSpace
INFO         |      Ok: Disk space requirements to run avd: `<build>` are met.
INFO         |   Checking: hasSufficientHwGpu
INFO         |      Ok: Hardware GPU requirements to run avd: `<build>` are passed.
INFO         |   Checking: hasSufficientSystem
INFO         |      Ok: System requirements to run avd: `<build>` are met.
INFO         | Warning: Could not find the Qt platform plugin "wayland" in "/home/maxj/android/aosp1/prebuilts/android-emulator/linux-x86_64/lib64/qt/plugins" ((null):0, (null))

INFO         | Warning: From 6.5.0, xcb-cursor0 or libxcb-cursor0 is needed to load the Qt xcb platform plugin. ((null):0, (null))

INFO         | Info: Could not load the Qt platform plugin "xcb" in "/home/maxj/android/aosp1/prebuilts/android-emulator/linux-x86_64/lib64/qt/plugins" even though it was found. ((null):0, (null))

Fatal: This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

Available platform plugins are: offscreen, linuxfb, minimal, vnc, xcb.
 ((null):0, (null))
INFO         | Fatal: This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.  

Available platform plugins are: offscreen, linuxfb, minimal, vnc, xcb.
 ((null):0, (null))

[1392:1392:20250924,221107.897914:ERROR file_io_posix.cc:144] open /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq: No such file or directory (2)        
[1392:1392:20250924,221107.897964:ERROR file_io_posix.cc:144] open /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq: No such file or directory (2)        
Aborted (core dumped)
```





```
emulator
INFO         | Android emulator version 35.3.8.0 (build_id 12560773) (CL:N/A)
INFO         | Graphics backend: gfxstream
INFO         | Changing default hw.initialOrientation to landscape

INFO         | Checking system compatibility:
INFO         |   Checking: hasSufficientDiskSpace
INFO         |      Ok: Disk space requirements to run avd: `<build>` are met.
INFO         |   Checking: hasSufficientHwGpu
INFO         |      Ok: Hardware GPU requirements to run avd: `<build>` are passed.
INFO         |   Checking: hasSufficientSystem
INFO         |      Ok: System requirements to run avd: `<build>` are met.
INFO         | Warning: Could not find the Qt platform plugin "wayland" in "/home/maxj/android/aosp1/prebuilts/android-emulator/linux-x86_64/lib64/qt/plugins" ((null):0, (null))

INFO         | Warning: From 6.5.0, xcb-cursor0 or libxcb-cursor0 is needed to load the Qt xcb platform plugin. ((null):0, (null))

INFO         | Info: Could not load the Qt platform plugin "xcb" in "/home/maxj/android/aosp1/prebuilts/android-emulator/linux-x86_64/lib64/qt/plugins" even though it was found. ((null):0, (null))

Fatal: This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

Available platform plugins are: offscreen, linuxfb, minimal, vnc, xcb.
 ((null):0, (null))
INFO         | Fatal: This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.  

Available platform plugins are: offscreen, linuxfb, minimal, vnc, xcb.
 ((null):0, (null))

[1534:1534:20250924,221127.202242:ERROR file_io_posix.cc:144] open /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq: No such file or directory (2)        
[1534:1534:20250924,221127.202291:ERROR file_io_posix.cc:144] open /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq: No such file or directory (2)        
Aborted (core dumped)
```

---





#### Clash Linux使用

[Clash for linux 教程](https://v2free.net/doc/#/linux/clash)

[Clash for linux 教程](https://v2free.net/doc/#/linux/clashweb.html)

