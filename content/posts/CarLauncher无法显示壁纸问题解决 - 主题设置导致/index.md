+++
date = '2025-10-08T22:28:30+08:00'
draft = false
title = 'CarLauncher无法显示壁纸问题解决 - 主题设置导致'
tags = ['Android', 'Wallpaper']
summary= "CarLauncher无法显示壁纸问题解决 - 主题设置导致"

+++

现象：已经设置了壁纸，壁纸服务也有启动，但是仍不可显示壁纸

思考方向：系统各个App页面层级排查

最终解决办法：

1. 在Application的主题中，添加透明背景和允许壁纸显示属性
   
    ```xml
        <style name="Theme.Launcher" parent="Theme.CarUi.NoToolbar">
            <item name="android:windowBackground">@android:color/transparent</item>
            <item name="android:windowShowWallpaper">true</item>
            <item name="textAppearanceGridItem">@android:style/TextAppearance.DeviceDefault.Medium</item>
            <item name="textAppearanceGridItemSecondary">@android:style/TextAppearance.DeviceDefault.Small</item>
        </style>
    ```
    
2. 切记不要在主页Activity进行设置，父类主题可能设置背景，导致背景不透明失效：



