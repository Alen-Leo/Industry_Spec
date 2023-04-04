<!--
 * @FileName: PBF.md
 * @Author: Alen luojiaming299@163.com
 * @CreateTime: 2022-09-10 08:49:04
 * @LastEditTime: 2022-09-10 20:09:29
 * Copyright (c) 2022 by Alen, All Rights Reserved.
-->

# *Phytium Processor Base Firmware*
> Base on PBF V3.0

## 1. Overview
![SoftwareArchitecture](../../Source/Industry_Phytium_PBF_SoftwareArchitecture.png)
### 1.1 固件分为三层：
+ 飞腾芯片内置可信根（Phytium Boot ROM，PBR）
  + PBR 是飞腾处理器**安全版**芯片内置的可信启动根，负责对 PBF 进行验签。
+ 飞腾处理器基础固件（Processor Base Firmware，PBF）
  + PBF 负责处理器芯片的基本初始化，并提供相关的服务。
  + 同时还负责加载运行在安全态（Secure World）下的 Secure OS。
+ 系统固件（System Firmware，SFW）
  + UEFI & U-Boot.
> 注：PBF、SFW、OS 等都会与带外控制系统（Outband Control System）进行通信。
### 1.2 执行流程
![Process](../../Source/Industry_Phytium_PBF_Process.png)

--------------------------------------------------------------------------------

## 2. 飞腾系统服务
### 2.1 服务总表
PBF 运行在 EL3，上层软件通过 AArch64 系统调用指令 SMC 调用功能服务。
![SystemService01](../../Source/Industry_Phytium_PBF_SystemService01.png)
![SystemService02](../../Source/Industry_Phytium_PBF_SystemService02.png)
![SystemService03](../../Source/Industry_Phytium_PBF_SystemService03.png)
### 2.2 错误码表
![ErrorReturn01](../../Source/Industry_Phytium_PBF_ErrorReturn01.png)
![ErrorReturn02](../../Source/Industry_Phytium_PBF_ErrorReturn02.png)


--------------------------------------------------------------------------------

## Reference
[1] [Phytium Processor Base Firmware V3.0.](https://www.phytium.com.cn/Site/theme/Uploads/20210113/Processor%20Base%20Firmware%E6%8E%A5%E5%8F%A3%E8%A7%84%E8%8C%83.pdf#:~:text=PBF%E6%98%AF%E9%A3%9E%E8%85%BE%E5%A4%84%E7%90%86%20%E5%99%A8%E5%9F%BA%E7%A1%80%E5%9B%BA%E4%BB%B6%EF%BC%8C%E8%B4%9F%E8%B4%A3%E5%A4%84%E7%90%86%E5%99%A8%E8%8A%AF%E7%89%87%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%88%9D%E5%A7%8B%E5%8C%96%EF%BC%8C%E5%B9%B6%E6%8F%90%E4%BE%9B%E7%9B%B8%E5%85%B3%E7%9A%84%E6%9C%8D%E5%8A%A1%E3%80%82%20PBF%E8%BF%98%E8%B4%9F%E8%B4%A3%E5%8A%A0,%E8%BD%BD%E8%BF%90%E8%A1%8C%E5%9C%A8%E5%AE%89%E5%85%A8%E6%80%81%EF%BC%88Secure%20World%EF%BC%89%E4%B8%8B%E7%9A%84Secure%20OS%E3%80%82)