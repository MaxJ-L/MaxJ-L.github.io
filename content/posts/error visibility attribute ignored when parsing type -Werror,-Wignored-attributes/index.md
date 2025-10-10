+++
date = '2025-10-10T21:02:08+08:00'
draft = false
title = 'Error Visibility Attribute Ignored When Parsing Type  Werror, Wignored Attributes'
tags = ['AOSP','编译']
summary= "Error Visibility Attribute Ignored When Parsing Type  Werror, Wignored Attributes"

+++

![Pasted image 20250320101347](./Pasted%20image%2020250320101347.png)

```bash
comm_manager/src/CommunicationManager.o.d -o out/soong/.intermediates/hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/LibCommManager/android_vendor.34_arm64_armv8-a-branchprot_static_cfi/obj/hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/comm_manager/src/CommunicationManager.o hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/comm_manager/src/CommunicationManager.cpp
In file included from hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/comm_manager/src/CommunicationManager.cpp:3:
In file included from hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/comm_soa/include/CommunicationSOAImpl.h:5:
In file included from hardware/interfaces/automotive/vehicle/aidl/impl/maxj/communication/comm_soa/src-gen/v2/commonapi/CanInfoTypes.hpp:21:
In file included from external/capicxx-core-runtime/include/CommonAPI/InputStream.hpp:21:
In file included from external/capicxx-core-runtime/include/CommonAPI/Variant.hpp:23:
external/capicxx-core-runtime/include/CommonAPI/Logger.hpp:31:37: error: 'visibility' attribute ignored when parsing type [-Werror,-Wignored-attributes]
    enum class Level : std::uint8_t COMMONAPI_EXPORT {
                                    ^~~~~~~~~~~~~~~~
external/capicxx-core-runtime/include/CommonAPI/Export.hpp:19:46: note: expanded from macro 'COMMONAPI_EXPORT'
    #define COMMONAPI_EXPORT __attribute__ ((visibility ("default")))
                                             ^~~~~~~~~~~~~~~~~~~~~~
1 error generated.
11:37:46 ninja failed with: exit status 1
```

### Solution

```bash
// bp添加
-Wno-attributes
-Werror
```
