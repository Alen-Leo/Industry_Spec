<!--
 * @FileName: PI_HOB.md
 * @Author: Alen luojiaming299@163.com
 * @CreateTime: 2022-09-27 21:44:30
 * @LastEditTime: 2022-10-08 09:06:17
 * Copyright (c) 2022 by Alen, All Rights Reserved.
-->

# <center>*Hand-Off Block (HOB)*</center>
## 1. Overview
**Example HOB Producer Phase Memory Map and Usage:**

![PI_HOB_MemoryMapAndUsage](https://cdn.jsdelivr.net/gh/Alen-Leo/Images/PI_HOB_MemoryMapAndUsage.png)

## 2. HOB Construction Rules
The first HOB in the HOB list must be the Phase Handoff Information Table (PHIT) HOB. The last HOB in the HOB list must be the End of HOB List HOB. Only HOB producer phase components are allowed to make additions or changes to HOBs. Once the HOB list is passed into the HOB consumer phase, it is effectively read only.
##### Three required HOBs：
+ The PHIT HOB
+ The memory allocation HOB
+ The resource descriptor HOB
##### HOB construction must obey the following rules:
+ All HOBs must start with a HOB generic header.
+ HOBs may contain boot services data that is available during the HOB producer and consumer phases only until the HOB consumer phase is terminated.
+ HOBs may be relocated in system memory by the HOB consumer phase. HOBs must not contain pointers to other data in the HOB list, including that in other HOBs. The table must be able to be copied without requiring internal pointer adjustment.
+ All HOBs must be multiples of 8bytes in length.
+ The PHIT HOB must always begin on an 8-byte boundary. Due to this requirement and requirement #4 in this list, all HOBs will begin on an 8-byte boundary.
+ HOBs are added to the end of the HOB list. HOBs can only be added to the HOB list during the HOB producer phase, not the HOB consumer phase.
+ HOBs cannot be deleted.

## 3 Adding to the HOB List
To add a HOB to the HOB list, HOB consumer phase software must obtain a pointer to the PHIT HOB (start of the HOB list) and follow these steps:
1. Determine NewHobSize, where NewHobSize is the size in bytes of the HOB to be created.
2. Check free memory to ensure that there is enough free memory to allocate the new HOB. This test is performed by checking that *`NewHobSize`* <= *`PHIT->EfiFreeMemoryTop`* - *`PHIT->EfiFreeMemoryBottom`*.
3. Construct the HOB at *`PHIT->EfiFreeMemoryBottom`*.
4. Set *`PHIT->EfiFreeMemoryBottom`* = *`PHIT->EfiFreeMemoryBottom`* + *`NewHobSize`*.

## 4 HOB Code Definitions
All HOBs consist of a generic header, *`EFI_HOB_GENERIC_HEADER`*, that specifies the type and length of the HOB. Each HOB has additional data beyond the generic header, according to the HOB type.
### 4.1 HOB generic header
Describes the format and size of the data inside the HOB. All HOBs must contain this generic HOB header.
```C
typedef struct _EFI_HOB_GENERIC_HEADER {
  UINT16 HobType;
  UINT16 HobLength;
  UINT32 Reserved;
} EFI_HOB_GENERIC_HEADER;
```
### 4.2 HOB data structure type
```C
//******************************************************
// HobType values
//******************************************************
#define EFI_HOB_TYPE_HANDOFF                  0x0001
#define EFI_HOB_TYPE_MEMORY_ALLOCATION        0x0002
#define EFI_HOB_TYPE_RESOURCE_DESCRIPTOR      0x0003
#define EFI_HOB_TYPE_GUID_EXTENSION           0x0004
#define EFI_HOB_TYPE_FV                       0x0005
#define EFI_HOB_TYPE_CPU                      0x0006
#define EFI_HOB_TYPE_MEMORY_POOL              0x0007
#define EFI_HOB_TYPE_FV2                      0x0009
#define EFI_HOB_TYPE_LOAD_PEIM_UNUSED         0x000A
#define EFI_HOB_TYPE_UEFI_CAPSULE             0x000B
#define EFI_HOB_TYPE_FV3                      0x000C
#define EFI_HOB_TYPE_UNUSED                   0xFFFE
#define EFI_HOB_TYPE_END_OF_HOB_LIST          0xffff
```

## 5 HOB PEI Service
### 5.1 GetHobList()
This function returns the pointer to the list of Hand-Off Blocks (HOBs) in memory.
```C
typedef
EFI_STATUS
(EFIAPI *EFI_PEI_GET_HOB_LIST) (
  IN  CONST EFI_PEI_SERVICES  **PeiServices,
  IN  OUT   VOID              **HobList
  );
```
### 5.2 CreateHob()
This service published by the PEI Foundation abstracts the creation of a Hand-Off Block's(HOB’s) headers.
```C
typedef
EFI_STATUS
(EFIAPI *EFI_PEI_CREATE_HOB) (
  IN  CONST EFI_PEI_SERVICES  **PeiServices,
  IN        UINT16            Type,
  IN        UINT16            Length,
  IN  OUT   VOID              **Hob
  );
```
## Appendix
**Pi Phase Flow:**

![PI_HOB_PiPhaseFlow](https://cdn.jsdelivr.net/gh/Alen-Leo/Images/PI_HOB_PiPhaseFlow.png)

## Reference
[1] [UEFI PI SPEC V1.7.](https://uefi.org/sites/default/files/resources/PI_Spec_1_7_A_final_May1.pdf)