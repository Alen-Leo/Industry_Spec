<!--
 * @FileName: SMBIOS.md
 * @Author: Alen luojiaming299@163.com
 * @CreateTime: 2022-10-01 12:48:41
 * @LastEditTime: 2022-10-20 22:17:02
 * Copyright (c) 2022 by Alen, All Rights Reserved.
-->

# <center>*System Management BIOS (SMBIOS)*</center>

## 1. SMBIOS EPS
![SmBiosEPS](../../Source/Industry_SMBIOS_EPS.png)
The only access method defined for the SMBIOS structures is a table-based method. It provides the SMBIOS structures as a packed list of data referenced by a table entry point.

**SMBIOS 访问流程：**
1. 通过关键字或者 GUID 找到 Entry Point structure.
2. 通过 EPS 读取 SMBIOS Structure 的表数量和表起始地址.

## 2. SMBIOS Structure
Each SMBIOS structure has a formatted section and an optional unformed section. The formatted section of each structure begins with a 4-byte header. Remaining data in the formatted section is determined by the structure type, as is the overall length of the formatted section.
![SmBiosStructure](../../Source/Industry_SMBIOS_SmBiosStructure.png)
### 2.1 Structure header format
![StructureHeaderFormat](../../Source/Industry_SMBIOS_StructureHeaderFormat.png)
### 2.2 Text Strings
Text strings associated with a given SMBIOS structure are appended directly after the formatted portion of the structure. Each string is terminated with a null (00h) BYTE and the set of strings is terminated with an additional null (00h) BYTE.If a string field references no string, a null (0) is placed in that string field. If the formatted portion of the structure contains string-reference fields and all the string fields are set to 0 (no string references), the formatted section of the structure is followed by two null (00h) BYTES.
## 3. EFI_SMBIOS_PROTOCOL
### 3.1 Protocol Interface Structure
Allows consumers to log SMBIOS data records, and enables the producer to create the SMBIOS tables for a platform.
```C
typedef struct _EFI_SMBIOS_PROTOCOL {
  EFI_SMBIOS_ADD Add;
  EFI_SMBIOS_UPDATE_STRINGUpdateString;
  EFI_SMBIOS_REMOVE Remove;
  EFI_SMBIOS_GET_NEXT GetNext;
  UINT8 MajorVersion;
  UINT8 MinorVersion;
} EFI_SMBIOS_PROTOCOL;
```
### 3.2 Member Description
| Function | Description |
| :---: | --- |
| EFI_SMBIOS_PROTOCOL.Add() | Add an SMBIOS record including the formatted area and the optional strings that follow the formatted area.|
| EFI_SMBIOS_PROTOCOL.UpdateString() | Update the string associated with an existing SMBIOS record. |
| EFI_SMBIOS_PROTOCOL.Remove() | Remove an SMBIOS record. |
| EFI_SMBIOS_PROTOCOL.GetNext() | Allow the caller to discover all or some of the SMBIOS records. |
> **Note**
> 1. EDK 2 reference code: SmbiosTable.c, SmbiosLib.c, SmbiosDxe.c, SmbiosTable.c and so on.
> 2. Add()->利用gBS写入System table，GetNext()->则遍历获取指定type指定index的Structure.

## Appendix
内存上的连续性，使用PI Protocol实现SMBIOS的创造更新等。

## Reference
[1] [System Management BIOS (SMBIOS) Reference Specification](https://www.dmtf.org/standards/smbios)



