+++
date = '2025-10-08T22:28:30+08:00'
draft = false
title = 'Qualcomm8775 QTIVehicle与原生Vehicle STR问题冲突解决'
tags = ['AOSP', 'Qualcomm']
summary= "Qualcomm8775 QTIVehicle与原生Vehicle STR问题冲突解决"

+++


# Android适配说明

## 系统适配
### QTI Vehicle与 自定义 Vehicle冲突
#### 服务冲突
+ Vehicle HAL 是安卓AAOS（Android Automitive OS）的最底层， 虽然AOSP官网默认不支持自行添加Property， 但是国内一贯的做法，都是新增自定义的Property，对应底层QNX、MCU协议或者CAN信号，以此传递APP-车身功能的通信的一环；
+ 按照文档中STR的时序图(STR Sequence Flowsheet)，VHAL需要监听按键输入 /dev/input/event0 power按键， 当power按键按下， 需要向上发送AP_POWER_STATE_REQ属性， SHUTDOWN_PREPARE状态；
+ 上述两点需求，导致了实现合并STR和自定义的Property，方案有多种：
	+ 方案一：自定义VehicleHAL + 监听/dev/input/event0按键节点，收到节点后往上发送AP_POWER_STATE_REQ属性， SHUTDOWN_PREPARE状态；（现使用）
	+ 方案二：两个VHAL共存，因为QTI VHAL占用了default注册，自定义的VHAL需要修改注册名，两者在CarService进行合并，实现过，但是破坏了安卓标准接口，也破坏了CarService原有的设计，代码修改较为生硬；
	+ 方案三：QTI-VHAL也是依赖DefaultVehicleHal和VehicleHardware等AOSP原生模块，可以考虑在这些模块链路中间实现拦截，将自定义的Proeprty进行自定义处理，原生Property留给QTI处理，理论上能尽可能保留两者，但这个也是最近灵机一动想出来的，可行性待验证；

+ 补充：/dev/input/event0只是一个INPUT节点，使用power按键作为QNX通知Android进入STR的通信手段，实际上Android自行发起SUSPEND，或者QNX通过SOA等方式告诉Android需要进入STR，也能够达到同样效果。如果是这样的话，也无需考虑是否适配QTI VHAL的问题了。

+ 方案一修改点：
	+ VHAL中通过EPOLL监听/dev/input/event0节点，收到POWER按键之后，即开始向上发送发送AP_POWER_STATE_REQ属性， SHUTDOWN_PREPARE状态；
	+ 因为涉及event，rc文件的group中添加input用户组，保证能够监听INPUT节点；
	```
	service vendor.vehicle-hal-maxj /vendor/bin/hw/android.hardware.automotive.vehicle@V1-maxj-service
    class early_hal
    user vehicle_network
    group system inet input
	```


### AP_POWER_STATE_REQ配置与参数
#### 配置修改
+ 修改说明：如果需要开启STR，除了处理QTI VHAL和Maxj VHAL冲突以外，还需要设置AP_POWER_STATE_REQ的参数，将对应configArray的参数设置为255（或者7）；
+ 参数说明：
VHAL配置中有个configArray，允许对每个ID进行一个组数值配置，在代码中进行自定义的判断处理；
这里的值定义对应VehicleApPowerStateConfigFlag，代表3个配置，是否支持休眠（Suspend to RAM）、是否支持设置睡眠后定时启动、是否支持睡眠（Suspend to Disk）；
在CarPowerMangerService中，通过PowerHalService进行判断；

```
hardware/automotive/vehicle/aidl/impl/default_config/config/DefaultProperties.json
{
       "property": "VehicleProperty::AP_POWER_STATE_REQ",
       "configArray": [
-        0
+        255
       ],
       "access": "VehiclePropertyAccess::READ",
       "changeMode": "VehiclePropertyChangeMode::ON_CHANGE"
```

```
enum VehicleApPowerStateConfigFlag {
    ENABLE_DEEP_SLEEP_FLAG = 0x1,
    CONFIG_SUPPORT_TIMER_POWER_ON_FLAG = 0x2,
    ENABLE_HIBERNATION_FLAG = 0x4,
}
```

#### 休眠默认值上报
+ 修改说明：需要开启STR，还需要VHAL上报的参数正确，AP_POWER_STATE_REQ这个ID对应的Value是一个int32数组，第一个已确定为 VehicleApPowerStateReq::SHUTDOWN_PREPARE 状态；
第二个需要填写VehicleApPowerStateShutdownParam::CAN_SLEEP，才可以表示休眠，最后进入休眠状态，填0表示不使用，默认为SHUTDOWN关机状态，其他状态相应参考VehicleApPowerStateShutdownParam定义；

## APP/HAL STR规范

> 这部分按照开发经验撰写，本次调试过程中，Android均可休眠。可供后续参考。
#### 资源释放
根据过往经验，APP和HAL服务等如果使用到系统资源，在进入休眠时，尽量将资源进行释放，包括但不限于以下资源：
+ 停止数据传输接口，如SPI、I²C、UART等等；
+ 关闭摄像头；
+ 关闭视频会话；
+ 关闭音频会话；
+ 停止OpenGL渲染；
+ 关闭屏幕；
+ 关闭USB；

#### 状态记忆
+ 音乐/电台等需要记忆上次播放状态、进度、播放曲目、波段、内容等等；

#### Android系统重启
Android系统长时间运行，内存碎片会导致系统卡顿，需要定期自动重启，建议以下条件满足时，可以进行Android系统重启
+ 系统运行时长累计运行超过60小时或者休眠次数达20h；
+ 档位处于P档；
+ 车速为0；
+ 时间尽量选择凌晨时间；
+ 用车情况：重启前1小时未被唤醒；

>以上为Android重启的建议需求，建议安排合理需求，定时对整机进行冷启动重启；

#### 监听电源状态代码
+ Java示例代码

```java
car = Car.createCar(mContext);  // 获取CAR
carManager = (CarPowerManager) car.getCarManager(Car.POWER_SERVICE);   // 获取CarPowerManager
carManager.setListener(mContext.getMainExecutor(), new CarPowerManager.CarPowerStateListener() {  // 监听CarPowerState
    @Override  
    public void onStateChanged(int i) {  
        @CarPowerManager.CarPowerState  int state = i;  
        Log.i(TAG, "onStateChanged: " + i);  
		// do somthing ...
    }  
});
```

+ C++示例代码
```C++
// 获取AIDL Vehicle
// 参考 packages/services/Car/cpp/vhal/client/src/AidlVhalClient.cpp
// hardware/interfaces/automotive/vehicle/vts/src/VtsHalAutomotiveVehicle_TargetTest.cpp

参数是 AP_POWER_STATE_REQ 、 areaId为0 、 采样率为 0 （OnChange类型，此参数无用）

class VehicleCallback : public IVehicleCallback::BnVehicleCallback {
public:
    ndk::ScopedAStatus onPropertyEvent(
            const std::vector<VehiclePropValue>& values) override {
        for (const auto& value : values) {
            if (value.prop == static_cast<int32_t>(VehicleProperty::AP_POWER_STATE_REQ)) {
                if (!value.value.int32Values.empty()) {
                    LOG(INFO) << "AP_POWER_STATE_REQ changed: " 
                              << value.value.int32Values[0];
                    // 处理状态变化：
                    // 0 = WAIT_FOR_VHAL
                    // 1 = DEEP_SLEEP_EXIT
                    // 2 = SHUTDOWN_PREPARE
                    // 3 = CANCEL_SHUTDOWN
                }
            }
        }
        return ndk::ScopedAStatus::ok();
    }

    ndk::ScopedAStatus onPropertySet(const VehiclePropValue& value) override {
        // 属性设置事件（此处不需要处理）
        return ndk::ScopedAStatus::ok();
    }

    ndk::ScopedAStatus onPropertySetError(
            const std::vector<VehiclePropError>& errors) override {
        for (const auto& error : errors) {
            LOG(ERROR) << "Property set error: " << error.propId 
                       << " in area " << error.areaId;
        }
        return ndk::ScopedAStatus::ok();
    }
};

const std::string instance = std::string(IVehicle::descriptor) + "/default";
// 获取IVehicle服务实例
std::shared_ptr<IVehicle> vehicle = IVehicle::fromBinder(
	ndk::SpAIBinder(AServiceManager_getService(instance.c_str())));

if (!vehicle) {
	LOG(ERROR) << "Failed to get IVehicle service";
	return 1;
}

// 创建我们的回调对象
auto callback = ndk::SharedRefBase::make<VehicleCallback>();

// 设置订阅选项
SubscribeOptions options = {
	.propId = static_cast<int32_t>(VehicleProperty::AP_POWER_STATE_REQ),
	.areaIds = {0},  // 全局区域
	.sampleRate = 0.0f,  // 事件触发模式（非周期性）
};

// 执行订阅
ndk::ScopedAStatus status = vehicle->subscribe(callback, {options});
if (!status.isOk()) {
	LOG(ERROR) << "Subscribe failed: " << status.getMessage();
	return 1;
}

LOG(INFO) << "Successfully subscribed to AP_POWER_STATE_REQ";

```



## APP/HAL STR规范


```java
car = Car.createCar(mContext);  // 获取CAR
carManager = (CarPowerManager) car.getCarManager(Car.POWER_SERVICE);   // 获取CarPowerManager
carManager.setListener(mContext.getMainExecutor(), new CarPowerManager.CarPowerStateListener() {  // 监听CarPowerState
    @Override  
    public void onStateChanged(int i) {  
        @CarPowerManager.CarPowerState  int state = i;  
        Log.i(TAG, "onStateChanged: " + i);  
		// do somthing ...
    }  
});
```



# 调试说明

+ 调试步骤按照《80-73603-1_REV_AA_Gen3_and_Gen4_STR_Debug_User_Guide》文档调试即可；
+ 步骤：
```
1. 关闭蓝牙、WiFi、USB
USB关闭命令为
#echo none > /sys/bus/platform/devices/a800000.ssusb/mode // pass through usb devices
#echo none > /sys/bus/platform/devices/a600000.ssusb/mode

2. 从QNX侧模拟发送Power按键到安卓
#echo power >/dev/qvm/la/powerkey

3. QNX侧查看QVM状态
cat /dev/qvm/la/power_status // 实测发现，power_status不存在，无法判别，因此通过adb是否能够连上安卓来进行判别
// 如果安卓完全关机，并非进入休眠，qvm/la文件夹不存在
```

