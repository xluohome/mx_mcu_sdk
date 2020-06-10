## 欢迎使用 MCU C-SDK

Ver：1.0.0

> MCU C-SDK仅限**米芯信息**合作伙伴使用。SDK源码及文档需经过米芯信息授权后才可用在商业项目中。SDK源码及文档受中华人民共和国法律之保护，米芯信息拥有全部版权，请合法合规使用。

### 米芯模块

米芯模块是一种产品级接入阿里云AIoT的解决方案。是米芯信息在阿里云AIoT软硬件生态的基础上开发而来。

目前已经适配多个认证模组硬件并已支持阿里云IoT™和天猫精灵™的直连接入。

电控（设备端）开发者只需要将米芯模块与设备控制板MCU的TTL串口电缆连接即可将设备接入阿里云AIoT。


### MCU C-SDK 

基于C语言编写，为方便电控开发者在设备端MCU上集成米芯模块串口编程。C-SDK已实现了基本功能和接口。开发者只需要按照设备功能实现业务逻辑开发即可完成对接。


| 架构 | 型号 | 是否支持 |  
|-|-|-|
|51系列 | 赛元、合泰等8位单片机| YES |
|ARM | STM8 | YES |
|ARM | STM32 | YES |


移植C-SDK到MCU后，MCU需要增加大约1k字节的ROM和200字节的RAM资源消耗 (实际资源消耗依据不同的产品功能会有出入)

### C-SDK使用
1. 将 mx_ 开头的4个文件复制到您的项目目录下；
2. 依据设备端产品对接文档，在mx_mcu_app.h中 添加、修改配置项；
3. 在mx_mcu_app.c中添加、修改功能数据下发、上报部分的代码；
4. SDK中的mx_uart_api.c和mx_uart_api.h 开发者无需修改。
5. 请参考uart.c，uart.h完成项目中的串口部分代码的实现。 


### 代码移植步骤

1. 底层移植：
  - 在串口中断或者串口接收到一帧数据时调用·     ```mx_uart_api.c```文件中的```wifi_uart_rec_data_process(unsigned char dat)```函数；
  - 在mx_mcu_app.c文件中的的 ```wifi_uart_tx_data(unsigned char *ptr, unsigned short len)``` 函数内完成串口发送数据；
  - 串口发送数据时，```mx_uart_api.c```文件中的串口发送标志变量```data_trans_flag```需要置1，发送完成清零。

2. 业务实现：
  - 在```mx_mcu_app.c```文件中的``` mx_func_cmd_handle(const unsigned char value[])```函数中完成产品功能业务逻辑代码；
  - 在MCU的主循环while（1）函数中调用``` mx_uart_cmd_service()```和```mx_mcu_data_update()```及```wifi_status_process()```函数；

3. 重要函数：
  - 重置wifi配网：
  调用```reset_wifi()```函数；
  - MCU数据主动上传：
  ```data_syn_flag```标志位置1；


  
## 免责申明和版权公告 

> 本文中的信息，包括供参考的 URL 地址，如有变更，恕不另行通知。

网站文档“按现状”提供，不负任何担保责任，包括对适销性、适用于特定用途或非侵权性的任何担保，和任何提案、规格或样品在他处提到的任何担保。本站文档不负任何责任，包括使用本站文档内信息产生的侵犯任何专利权行为的责任。本站文档在此未以禁止反言或其他方式授予任何知识产权使⽤许可，不管是明示许可还是暗示许可。 

文中所得测试数据均为实验室测试所得，实际结果可能略有差异。 

> 文中提到的所有商标名称、商标和注册商标均属其各自所有者的财产，特此声明。最终解释权归**绍兴米芯信息技术有限公司**所有。
