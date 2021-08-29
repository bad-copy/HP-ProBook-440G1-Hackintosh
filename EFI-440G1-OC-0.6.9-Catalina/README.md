HP ProBook 440G1 EFI制作说明：
基于OpenCore 0.6.9
HM87芯片组
CPU：4200m
禁用独显，UEFI(SCM)模式
机械硬盘
Catalina 10.15.7

添加驱动：
1、Lilu.kext
2、VirtualSMC.kext
3、WhateverGreen.kext（显卡驱动，需要修改设备属性）
4、SMCProcessor.kext（温度传感器）
5、USBPorts.kext（定制USB端口，解除电源限制）
6、VoodooPS2Controller.kext
7、VoodooPS2Keyboard.kext（键盘）
8、VoodooPS2Mouse.kext（鼠标）
9、RealtekRTL8111.kext（有线网卡）
10、VoodooHDA.kext（声卡，AppleALC不能正常驱动）
11、SMCBatteryManager.kext（电量显示）

ACPI补丁：
1、将7000700001082200017900改为7000700001022200007900，防bios重置；
2、USB控制器改名EHC2->EH02，EHC1->EH01；
3、核显设备改名GFX0->IGPU；
4、SAT0->SATA；
5、EC0->EC；
6、PNLF->XNLF（亮度调节）；
7、电池电量相关函数改名BTIF->BTI0，GBTI->BTI1，BTST->BTS1，GBTC->BTC0，SBTC->BTC1，GPMC->PMC0。

ACPI添加：
1、SSDT-PNLF-Haswell_Broadwell.aml，结合PNLF改名，实现屏幕亮度控制；
2、SSDT-PLUG-_PR.CPU0.aml，原生电源管理；
3、SSDT-HP440G1-BATTERY.aml，电池电量补丁（ECEnabler.kext也可以实现，但会提示很多错误）。

设备属性：
1、核显：PciRoot(0x0)/Pci(0x2,0x0)，添加以下属性：
  AAPL,ig-platform-id -> 0600260A
  device-id -> 12040000

其它设置项：
1、ScanPolicy设置为0；
2、UnblockFsConnect设置为true（解决部分分区无法显示）；
3、AppleCpuPmCfgLock设置为true；
4、AppleXcpmCfgLock设置为true；
5、DisableRtcChecksum设置为true（防睡眠造成的bios重置）；
6、机型设置为MacBookPro11,4；
7、使用Hackintool注入EDID到overrides，解决显示器偏色问题（设备属性中注入无效）。

已知问题：
1、内置无线网卡未驱动（MT7630E）；
2、内置蓝牙未驱动（Mediatek，M76）；
3、内置指纹未驱动（Synaptics）；
4、内置读卡器未驱动（RTS5227）；
4、触摸板只能当鼠标用（VoodooTrackPad不能驱动触摸板）；
5、重启黑屏，只能关机；
6、重启后亮度会比之前变亮一些（仿冒光线传感器无法解决，白果也一样）；
7、电池电量比Windows上显示的高4%左右，HWMonitor显示正常。




