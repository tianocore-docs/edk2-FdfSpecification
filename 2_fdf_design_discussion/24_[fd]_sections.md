<!--- @file
  2.4 [FD] Sections

  Copyright (c) 2006-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 2.4 [FD] Sections

The `[FD]` sections are made up of the definition statements and a description
of what goes into the Flash Device Image. Each FD section defines one flash
"device" image. A flash device image may be one of the following: Removable
media bootable image (like a boot floppy image,) a System "Flash" image (that
would be burned into a system's flash) or an Update ("Capsule") image that will
be used to update and existing system flash.

Multiple FD sections can be defined in a FDF file.

The section header format is `[FD.FdUiName]` where the `FdUiName` can be any
value defined by the user. If only a single FD is constructed for a platform
then `FdUiName` is optional, and the processing tools will use the DSC file
`[Defines]` section's `PLATFORM_NAME` value for creating the FD file.

An FD section is terminated by any other section header section or the end of
the file.

This section is required for platform images, and not required for OptionROM
images.

### 2.4.1 FD TOKEN Statements

The Token statements are used to define the physical part. These include the
base address of the image, the size of the image, the erase polarity and block
information. Only one of each of the valid token names can be defined in any
one FD section, except as noted below.

`Token = VALUE [| PcdName]`

Only one token statement can appear on a single line, and each token statement
must be on a single line. Multi-line token statements are not permitted.

There are five valid Token names defined by this specification.

**_BaseAddress_**

The base address of the FLASH Device.

**_Size_**

The size in bytes of the FLASH Device

**_ErasePolarity_**

Either 0 or 1, depending on the erase polarity of the Flash Device.

**_BlockSize_**

One or More - Size of a block, optionally followed by number of blocks.
Multiple `BlockSize` statements are legal, and the first statement represents
block 0 (the first block) and subsequent `BlockSize` statements represent
blocks 1 - N.

**_NumBlocks_**

Zero or one - The number of continuous blocks of size, `BlockSize`. If
`NumBlocks` is not present, the number of blocks defaults to 1.

An optional `PcdName` may follow the Token statement and is separated from the
statement using a pipe "|" character. The `PcdName` is assigned `$(VALUE)`.
Only one `PcdName` can be assigned a Token's Value.

### 2.4.2 FD DEFINE statements

`DEFINE` statements are used to define `MACRO` definitions that are scoped to
the individual FD sections. `DEFINE` statements are processed in order, so a
later `DEFINE` statement for a given `MACRO` over-writes the previous definition.
The `DEFINE` statements are typically used for creating short-cut names for
directory path names but may be used for identifying other items or values that
will be used in later statements.

`DEFINE MACRO = PATH`

The following are examples of the DEFINE statement.

```
DEFINE FV_DIR           = $(OUT_DIR)/$(TARGET)_$(TOOL_CHAIN_TAG)/$(ARCH)
DEFINE MDE_MOD_TSPG     = gEfiMdeModulePkgTokenSpaceGuid
DEFINE NV_STOR_VAR_SIZE = PcdFlashNvStorageVariableSize
DEFINE FV_HDR_SIZE      = 0x48
DEFINE VAR_STORE_SIZE   = $(MDE_MOD_TSPG).$(NV_STOR_VAR_SIZE) - $(FV_HDR_SIZE)
```

The `$(MACRO)` can be used to create a shorthand notation that can be used
elsewhere within the FDF file. Macro values may be scoped to subsections of the
FDF file. Macros are also positional, with later values replacing values for
macros at the same level. When tools process this file, the `$(MACRO)` name
will be expanded in commands or files emitted from the tools. In the following
example, `$(OUTPUT_DIRECTORY)` is a variable, whose value is found in the
platform's DSC file, and this file assigns `OUT_DIR` as the variable name to
use, with the same value as `$(OUTPUT_DIRECTORY)`:

```
DEFINE OUT_DIR = $(OUTPUT_DIRECTORY)
DEFINE FV_DIR = $(OUT_DIR)/$(TARGET)_$(TOOL_CHAIN_TAG)/$(ARCH)/FV
```

If the DSC file declares `OUTPUT_DIRECTORY = $(WORKSPACE)/Build/Nt32`,
`TARGET = DEBUG`, target.txt uses `MYTOOLS` for the tool chain, and the platform
is `IA32`, then a statement later in the section that references `$(FV_DIR)` is
interpreted by the tools as being:

`$(WORKSPACE)/Build/Nt32/DEBUG_MYTOOLS/IA32/FV`

### 2.4.3 FD SET statements

`SET` statements are used to define the values of PCD statements. The current
PCD maps for regions include extra PCD entries that define properties of the
region, so the `SET` statement can occur anywhere within an FD section.

`SET` statements are positional within the FDF file.

`SET PcdName = VALUE`

Additionally, a PCD Name is made up of two parts or three parts, separated 
by a period "." character. The format for a `PcdName` is:

`PcdTokenSpaceGuidCName.PcdCName`
or `PcdTokenSpaceGuidCName.PcdCName.PcdFieldName`

The following is an example of the `SET` statement:

`SET gFlashDevicePkgTokenSpaceGuid.PcdEfiMemoryMapped = TRUE`

The `VALUE` specified must match the PCD's datum type and must be the content
data.

For a PCD that has a datum type of `VOID`*, the data can be a Unicode string,
as in `L"text"`, a valid C data array (it must be either a C format GUID or a
hex byte array), as in `{0x20, 0x01, 0x50, 0x00, 0x32, 0xFF, 0x00, 0xAA, {0xFF, 0xF0, 0x00, 0x00, 0x00}}.`
For other PCD datum types, the value may be a boolean or a hex value, as in
`0x0000000F,` with a value that is consistent with the PCD's datum type.

The value may also be a macro or `$(PCD)` or it may be computed, using arithmetic
operations, arithmetic expressions and or logical expressions. The value
portion of the `SET` statement, when using any of these computations are in-fix
expressions that are evaluated left to right, with items within parenthesis
evaluated before the outer expressions are evaluated. Use of parenthesis is
encouraged to remove ambiguity.

### 2.4.4 FD Region Layout

Following the FD defines section are lists of `Regions` which correspond to the
locations of different images within the flash device. Currently most flash
devices have a variable number of blocks, all of identical size. When "burning"
an image into one of these devices, only whole blocks can be burned into the
device at any one time. This puts a constraint that all layout regions of the
FD image must start on a block boundary. To accommodate future flash parts that
have variable block sizes, the layout is described by the offset from the
`BaseAddress` and the size of the section that is being described. Since
completely filling a block is not probable, part of the last block of a region
can be left empty. To ensure that no extraneous information is left in a
partial block, it is recommended that the block be erased prior to burning it
into the device.

Regions must be defined in ascending order and may not overlap.

A layout region start with an eight digit hex offset (leading "0x" required)
followed by the pipe "|" character, followed by the size of the region, also in
hex with the leading "0x" characters.

The typical layout region is terminated by the start of another region or an FV
Section header.

The format for an FD Layout Region is:

```ini
Offset|Size
[TokenSpaceGuidCName.PcdOffsetCName | TokenSpaceGuidCName.PcdSizeCName] ?
  [RegionType] ?
```

Setting the optional PCD names in this fashion is a shortcut. The two regions
listed below are identical, with the first example using the shortcut, and the
second using the long method:

```
0x000000|0x0C0000
gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress | gEfiMyTokenSpaceGuid.PcdFlashFvMainSize
FV = FvMain

0x000000|0x0C0000
SET gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress = 0x000000 + $(BaseAddress)
SET gEfiMyTokenSpaceGuid.PcdFlashFvMainSize        = 0x0C0000
FV = FvMain
```

The shortcut method is preferred, as the user does not need to maintain the
values in two different locations.

The `RegionType`, if specified, must be one of the following `FV`, `DATA`,
`FILE`, `INF` or `CAPSULE`. Not specifying the `RegionType` implies that the
region starting at the "Offset", of length "Size" must not be touched (unless
the region will be used for VPD data). This type of region is typically used for
event logs that are persistent between system resets, and modified via some
other mechanism (and Each FD region has a `UiName` modifier, then the output
image files uses the `UiName` modifier for the file name.

**********
**Note:** Although sub-regions existed in EDK FDF files, EDK II FDF does not
use the concept of subregions.
**********

#### 2.4.4.1 FV RegionType

The `FV RegionType` is a pointer to either one of the unique FV names that are
defined in the `[FV]` section. Both of these are files that contains a binary FV
as defined by the PI 1.0 specification. The format for the `FV RegionType` is
the following:

`FV = UiFvName`

The following is an example of FV region type:

```
0x000000|0x0C0000
gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress | gEfiMyTokenSpaceGuid.PcdFlashFvMainSize
FV = FvMain
```

#### 2.4.4.2 DATA RegionType

The `DATA RegionType` is a region that contains is a hex byte value or an array
of hex byte values. This data that will be loaded into the flash device,
starting at the first location pointed to by the Offset value. The format of the
`DATA RegionType` is:

DATA = { <Hex Byte Data Structure> }

The following is an example of a DATA region type.

```c
0x0CA000 | 0x002000
gEfiMyTokenSpaceGuid.PcdFlashNvStorageBase | gEfiMyTokenSpaceGuid.PcdFlashNvStorageSize
DATA = {
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C
}
```

The `!include` statement is valid for hex array portion of the `DATA RegionType`.
The following is a valid usage for the `!include` statement.

```c
0x0CA000 | 0x002000
gEfiMyTokenSpaceGuid.PcdFlashNvStorageBase | gEfiMyTokenSpaceGuid.PcdFlashNvStorageSize
DATA = {
  !include NvStoreInit.txt
}
```

#### 2.4.4.3 FILE RegionType

The `FILE RegionType` is a pointer to a binary file that will be loaded into the
flash device, starting at the first location pointed to by the `Offset` value.
It should be noted that a file can be fully qualified path and filename that is
outside of the current WORKSPACE (or the directories listed in PACKAGES_PATH
system environment variable). The file must be a binary (.efi) or a raw binary
file. The format of the `FILE RegionType` is:

`FILE = $(FILE_DIR)/Filename.bin`

**********
**Caution:** If a fully qualified path and filename is specified, the platform
integrator must ensure that all developers using the DSC and FDF file are aware
of the requirements for this path.
**********

The following is an example of the `FILE RegionType`.

```ini
0x0CC000|0x002000
gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchAddress | gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchSize
FILE = $(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/X64/Microcode.bin

# VPD Data Region
0x0026D000|0x00001000
gEfiMdeModulePkgTokenSpaceGuid.PcdVpdBaseAddress

FILE = $(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/FV/8C3D856A-9BE6468E-850A-24F7A8D38E08.bin
```

#### 2.4.4.4 Capsule RegionType

The `CAPSULE RegionType` is a pointer to a capsule section `UiName` that will be
loaded into the flash device, starting at the first location pointed to by the
`Offset` value. The format of the `FILE RegionType` is:

`CAPSULE = UiCapsuleName`

The following is an example of the `CAPSULE RegionType`.

```
0x0CC000|0x002000
gEfiTokenSpaceGuid.PcdCapsuleOffset | gEfiTokenSpaceGuid.PcdCapsuleSize
CAPSULE = MyCapsule
```

#### 2.4.4.5 INF Region Type

The INF statements point to EDK component and EDK II module INF files. Parsing
tools will scan the INF file to determine the type of component or module. The
component or module type is used to reference the standard rules defined
elsewhere in the FDF file.

The format for INF statements is:

`INF [Options] PathAndInfFileName`

The PathAndInfFileName is the WORKSPACE (or PACKAGES_PATH) relative path and
filename.

#### 2.4.4.6 No RegionType Specified

It is permissible to define a region with no data pre-loaded. For example,
event logging needs a data region to store events. This region is filled with
data that matches the `ErasePolarity` bit during the initial installation of
the firmware or through UEFI or operating system commands and services.

An example of no region type specified is:

```
0x0CE000|0x002000
gEfiMyTokenSpaceGuid.PcdFlashNvStorageEventLogBase | gEfiMyTokenSpaceGuid.PcdFlashNvStorageEventLogBase
```
