两个输出引脚控制
将按钮的引脚设置为虚拟引脚，例如 V1 和 V2。
在 Blynk 应用中，打开你刚刚创建的项目。
添加两个按钮（Button）小部件，分别用于控制继电器1和继电器2。
配置每个按钮：
将按钮的引脚设置为虚拟引脚，例如 V1 和 V2。
设置按钮的模式为“Switch”或“Push”。
保存配置。

蓝牙串口调试用数字v1,v2,v3……

1，2，3，4，5，6，7也是可以的

1234567全开
1234567全关
或1236
那就是
1236脚控制开和关


要通过蓝牙控制ESP32上的12个输出引脚，你需要使用蓝牙终端应用程序（如手机上的蓝牙串口助手）发送命令。
这些命令将被ESP32接收并解析，从而控制指定的引脚状态。以下是如何通过蓝牙控制引脚的详细步骤：

1. 连接蓝牙
首先，确保你的手机或其他设备已经安装了蓝牙串口助手应用程序（如“Serial Bluetooth Terminal”或“HC-05 Bluetooth Terminal”）。

2. 配对蓝牙
打开蓝牙串口助手应用程序。
打开手机的蓝牙功能。
搜索附近的蓝牙设备，找到名为“esp32_bluetooth”的设备。
点击配对，可能需要输入配对码（通常是1234或0000）。
3. 发送控制命令
一旦配对成功，你可以在蓝牙串口助手中发送命令来控制ESP32的引脚。命令格式如下：

XH：将第X个引脚设置为高电平（HIGH）。
XL：将第X个引脚设置为低电平（LOW）。
其中，X是一个数字，范围从0到11，表示12个引脚中的某一个。

示例命令
0H：将第0个引脚设置为高电平。
1L：将第1个引脚设置为低电平。
11H：将第11个引脚设置为高电平。
4. 观察OLED显示
发送命令后，OLED显示屏会显示当前的引脚状态。每5秒，显示屏会自动翻页，显示另一组引脚的状态。



wifi控制：
确保 ESP32 已成功连接到与你的设备（如电脑或手机）相同的 WiFi 网络，并获取到 IP 地址。你可以通过串口监视器查看 ESP32 的 IP 地址。
在浏览器地址栏中输入 ESP32 的 IP 地址加上控制路由，例如 “http://esp32_ip_address/control0”（这里假设你要控制第一个引脚，0 是引脚索引，可根据实际情况更改）。
在地址后面添加查询参数来指定状态和持续时间。例如：
“http://esp32_ip_address/control0?state=high&duration=5000” 将第一个引脚设置为高电平持续 5 秒。
“http://esp32_ip_address/control0?state=low” 将第一个引脚设置为低电平。
“http://esp32_ip_address/control0?state=toggle” 切换第一个引脚的状态。
根据传入的不同参数，ESP32 会执行相应的操作，并在浏览器中返回操作结果的文本信息。同时，OLED 显示屏也会根据操作结果进行更新，显示当前的 IP 地址和各个引脚的状态。


以下是几种可以测试手机辅助连接 WiFi 功能是否正常实现的方法：
方法一：模拟初次配置情况
清除已有配置（可选）
如果担心之前有保存的 WiFi 配置影响测试，可以在代码的setup函数开头添加代码来清除之前的设置，像这样：

    // 初始化WiFiManager
    WiFiManager wm;


    // 初始化WiFiManager
    WiFiManager wm;
    wm.resetSettings();//清除保存密码配置

这会确保设备以 “全新” 状态来进行 WiFi 连接测试。
2. 上电操作
给设备上电后，等待片刻，由于清除了配置或者本身就是首次使用，它应该不会自动连接成功（如果之前自动连接是因为有旧配置的话）。此时，使用手机搜索附近的 WiFi 网络，你应该能发现一个名为 “AutoConnectAP”（这是代码中autoConnect函数里设置的默认热点名称）的开放热点。
3. 手机端配置
连接上这个 “AutoConnectAP” 热点后，打开手机浏览器，输入地址 “192.168.4.1”（这是WiFiManager库常见的默认配置页面地址，不同版本可能有差异，若不行可查看其文档查找准确地址），这时会出现一个 WiFi 配置页面，在页面中输入你想要让设备连接的目标 WiFi 网络的 SSID 和密码，然后提交配置信息。
4. 查看连接结果
提交后，设备应该会尝试使用你输入的信息去连接目标 WiFi 网络，如果连接成功，在串口监视器（通过 USB 连接设备到电脑，并打开对应的串口工具，波特率设置为代码里初始化串口时的波特率，比如 115200）中应该能看到类似 “WiFi connected” 以及打印出的 IP 地址等提示信息，表示手机辅助连接 WiFi 功能实现成功了。

方法二：更改目标 WiFi 网络
准备工作
先确保设备当前是正常连接着某个 WiFi 网络（如果是之前自动连接上的情况），记录下当前的 WiFi 名称（SSID）。
更改 WiFi 网络
将你的手机或其他能控制路由器的设备，对当前 WiFi 网络的名称（SSID）或者密码进行修改（比如修改密码），这样原有的 WiFi 配置对于设备来说就失效了。
观察设备反应
设备如果检测到 WiFi 连接断开（可以通过串口监视器查看是否有相关断开提示信息），正常情况下应该会尝试重新连接，由于配置已经失效，它会再次进入配置模式，也就是开启名为 “AutoConnectAP” 的热点，后续按照方法一中手机端配置的步骤，重新输入新的 WiFi 网络信息进行配置，如果最后能成功连接新修改的 WiFi 网络，也说明手机辅助连接功能是正常的。
通过以上两种方式就能比较有效地测试代码中手机辅助连接 WiFi 功能是否真正实现并能正常工作啦。

    // 初始化WiFiManager
    WiFiManager wm;
    wm.resetSettings();//清除保存密码配置

    // 初始化WiFiManager
    WiFiManager wm;
    
