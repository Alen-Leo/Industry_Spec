<!--
 * @FileName: PSCI.md
 * @Author: Alen luojiaming299@163.com
 * @CreateTime: 2022-09-11 10:47:52
 * @LastEditTime: 2022-09-11 20:27:04
 * Copyright (c) 2022 by Alen, All Rights Reserved.
-->

# *Arm Power State Coordination Interface*

## 1. Description
PSCI provides a standard interface definition to support the interoperation and integration across the various supervisory systems. This document defines the Power State Coordination Interface, and describes its use for idle, hotplug, shutdown, and reset.
### 1.1 PSCI intended use
+ Provides a generic interface that supervisory software can use to manage power in the following situations:
  + Core idle management.
  + Dynamic addition of cores to and removal of cores from the system, often referred to as hotplug.
  + Secondary core boot.
  + Moving trusted OS context from one core to another.
  + System shutdown and reset.
+ Provides an interface that supervisory software can use in conjunction with Firmware Table (FDT and ACPI) descriptions to support the generalization of power management code.
### 1.2 Softward stacks on Arm-based systems
![PSCI_SoftwareStacks](https://cdn.jsdelivr.net/gh/Alen-Leo/Images/PSCI_SoftwareStacks.png)

--------------------------------------------------------------------------------

## Reference

[1] [Arm Power State Coordination Interface *Platform Design Document*](https://developer.arm.com/documentation/den0022/e/)