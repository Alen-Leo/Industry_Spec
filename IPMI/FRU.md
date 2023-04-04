# <center>*IPMI FRU Information Storage Definition*</center>

## FRU Information Device Format
+ **Common Header (necessary)**
+ **Internal Use Area (optional)**
+ **Chassis Info Area (optional)**
+ **Board Info Area (optional)**
+ **Product Info Area (optional)**
+ **MultiRecord Info Area (optional)**

> The Chassis Info, Board Info, and Product Info areas each contain a number of variable length fields. Each of these fields is preceded by a type/length byte that indicates the length of the field and the type of encoding that is used for the field. The leading fields in each area serve predefined functions. These fields can be followed by ‘custom’ fields that are defined by manufacturing or by the OEM. The same variable length field format is used within records in the MultiRecord Area.
>
> The application traverses all fields by walking individual fields until the ‘end-of-fields’ type/length byte (value C1h) is encountered. And only pre-defined fields are allowed to be fixed length without type/length bytes. All custom fields are specified as variable length fields with type/length bytes.

----------------------------------------------------------------------------

## Suggested Storage Organization For 2Kb EEPROM
![SuggestedStorageOrganization](../../Source/Industry_IPMI_FRU_SuggestedStorageOrganization.png)

----------------------------------------------------------------------------

## Different Area Format
### 1. Common Header Format
![CommonHeaderFormat](../../Source/Industry_IPMI_FRU_CommonHeaderFormat.png)

### 2. Internal Use Area Format
![InternalUseAreaFormat](../../Source/Industry_IPMI_FRU_InternalUseAreaFormat.png)

### 3. Chassis Info Area Format
![ChassisInfoAreaFormat](../../Source/Industry_IPMI_FRU_ChassisInfoAreaFormat.png)
> **Chassis Type** The enumeration values for System Enclosure and Chassis Types is defined in the [SMBIOS] specification, Table 16 – System Enclosure or Chassis Types.

### 4. Board Info Area Format
![BoardInfoAreaFormat](../../Source/Industry_IPMI_FRU_BoardInfoAreaFormat.png)

### 5. Product Info Area Format
![ProductInfoAreaFormat](../../Source/Industry_IPMI_FRU_ProductInfoAreaFormat.png)

### 6. MultiRecord Info Area Format
#### a. Record Header
![MultiRecordHeader](../../Source/Industry_IPMI_FRU_MultiRecordHeader.png)
**Record Type**
![MultiRecordHeader_RecordType](../../Source/Industry_IPMI_FRU_MultiRecordHeader_RecordType.png)
**End of list**
This bit indicates if this record is the last record in the MultiRecord area. If this bit is zero, it indicates that one or more records follow.
#### b. Record Field Definitions
**Record Type 0x00 -> Power Supply Information**
![MultiRecordField_RecordType0x00](../../Source/Industry_IPMI_FRU_MultiRecordField_RecordType0x00.png)

--------------------------------------------------------------------------------

## Appendix A - FRU Information Layout Example
![FruInformationLayoutExample](../../../../Source/Industry_IPMI_FRU_FruInformationLayoutExample.png)

--------------------------------------------------------------------------------

## Appendix B - TYPE/LENGTH BYTE FORMAT
#### a. Specification
![TypeLengthSpec](../../../../Source/Industry_IPMI_FRU_TypeLengthSpec.png)
#### b. BCD PLUS definition
![BcdPlusDefine](../../Source/Industry_IPMI_FRU_TypeLengthSpec.png)
#### c. 6-bit ASCII definition
![6bitAsciiDefinition01](../../Source/Industry_IPMI_FRU_6bitAsciiDefinition01.png)
![6bitAsciiDefinition02](../../Source/Industry_IPMI_FRU_6bitAsciiDefinition02.png)
#### d. Language Codes
Any language code other than English indicates that the string data is encoded as Unicode when bits 7:6 of the Type/Length code = 11b.

--------------------------------------------------------------------------------

## Reference
[1] [Platform Management FRU Information Storage Definition V1.0.](https://www.intel.com/content/dam/www/public/us/en/documents/specification-updates/ipmi-platform-mgt-fru-info-storage-def-v1-0-rev-1-3-spec-update.pdf)