+++
date = '2025-07-08T22:28:30+08:00'
draft = false
title = 'Android-15.0.0_r1 lunch无法显示menu.md'
tags = ['Android','编译']
summary= "Android-15.0.0_r1 lunch无法显示menu.md"

+++

#### Issue

source build/envsetup.sh和lunch`后，无法显示menu；

```sh
You're building on Linux

Warning: Cannot display lunch menu.

Note: You can invoke lunch with an explicit target:

  usage: lunch [target]

Which would you like? [aosp_cf_x86_64_phone-trunk_staging-eng]
Pick from common choices above (e.g. 13) or specify your own (e.g. aosp_barbet-trunk_staging-eng):
```

#### Fixed

疑似因为部分环境变量没有提前设置好，因此导致了此错误。

大部分情况根据这个步骤，可以解决

```sh
. ./build/envsetup.sh
export TARGET_RELEASE=trunk_staging
OR
export TARGET_RELEASE=ap1a # (as shown in your quoted error)
build_build_var_cache
lunch
```

#### Preferences

1. [lunch failed | Page 2 | XDA Forums](https://xdaforums.com/t/lunch-failed.4665348/page-2)
