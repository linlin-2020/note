## 如何关闭华为手机或平板系统更新

1. 进入开发者选项，打开“手机打开USB调试”，“仅充电模式下允许ADB调试”，手机上确认允许电脑usb调试。  
1. 手机上断开wifi和移动网络，应用管理里找到系统更新-存储-删除数据，再退出进入系统更新，目的是消除已有的设置红点角标，还没出现设置红点的直接跳过这步。  
1. 手机连接电脑把adb文件夹放在电脑C盘根目录下
1. 开始-运行-CMD，打开后运行命令cd c:\adb
1. 运行命令adb shell pm disable-user com.huawei.android.hwouc 关闭系统更新提示，关闭后，手机设置-系统-系统更新 是打不开了，以后再也没有更细提示了
1. 输入命令adb shell pm enable com.huawei.android.hwouc 这是重新打开系统更新。
1. over
