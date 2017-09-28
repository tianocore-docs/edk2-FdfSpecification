<!--- @file
  2.5 [FV] Sections

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

## 2.5 [FV] Sections

The `[FV]` sections are required for platform images, are optional for Capsule
images, and are not required for Option ROM only images. The `[FV]` section
defines what components or modules are placed within a flash device file. These
sections define the order the components and modules are positioned within the
image. The `[FV]` section consists of define statements, set statements and
module statements. A single `[FV]` section is terminated by the start of
another section header or the end of the file. The `[FV]` section has one
required modifier, a user defined section name. The format for `[FV]` section
header is:

`[FV.UiFvName]`

The FV `UiFvName` must be unique for every declared user defined name within
the file. The `UiFvName` is used for specifying the file that will be used in
and `[FD]` section.

Nesting of `[FV]` sections is permitted, making it possible to create a tree
structure containing multiple FV sections within a single `[FV]` section. Nested
sections are specified using either the syntax "FILE FV_IMAGE = Name" or
"SECTION FV_IMAGE = Name" within the top `[FV]` Section.

Use of the `UiFvName` modifier permits the user to include, by `UiFvName`,
previously defined sections within another FV section. This eliminates the need
to re-specify components or modules in multiple places. When the `FvNameString`
entry is present and set to TRUE in an `[FV]` section, the tools will generate
an `FvNameString` entry in FV EXT header using the `UiFvName`.

This section also specifies how to define content for PI FV Extensions which
provides a mapping for a GUID, an OEM file type and FV used size. The size of
`EFI_FIRMWARE_VOLUME_EXT_HEADER` and `EFI_FIRMWARE_VOLUME_EXT_ENTRY` sizes will
be calculated based on content, while the `EFI_FIRMWARE_VOLUME_EXT_ENTRY` type
must be defined by the platform integrator based on the PI specification,
volume 3 The content is limited to the contents of a binary file specified by a
FILE statement or a data array specified by a `DATA` statement.

The `EFI_FIRMWARE_VOLUME_EXT_ENTRY_OEM_TYPE` (using `TYPE=0x0001`) is only
support by including a file or data structure that completes the remainder of
the OEM type entry, where the first entry would have to be a `UINT32`
representing the TypeMask. Additional types defined by the PI spec will be
supported in this manner as well.

The build system permits a recovery feature that allows placing two copies of a
`PEI.fv` in the flash top. If the top one is corrupt, backup one will be
swapped to the top and work.

The backup must be the same as the top _PEI.fv_ although it is placed into
another place. Therefore, the backup FV must be rebased to run at another
address. The `FvBaseAddress` and the optional `FvForceRebase` attributes must be
above `FvAlignment` attribute.

### 2.5.1 DEFINE Statements

`DEFINE` statements are used to define Macro definitions that are scoped to the
individual `[FV]` sections. `DEFINE` statements are processed in order, so a
later `DEFINE` statement for a given `MACRO` over-writes the previous definition
for the remainder of the section or sub-section. The `DEFINE` statements are
typically used for creating short-cut names for directory path names, but may be
used for identifying other items or values that will be used in later statements.

`DEFINE MACRO = VALUE`

The following are examples of the `DEFINE` statement.

```
DEFINE EDKMOD           = $(WORKSPACE)/EdkModulePkg/
DEFINE MDE_MOD_TSPG     = gEfiMdeModulePkgTokenSpaceGuid
DEFINE NV_STOR_VAR_SIZE = PcdFlashNvStorageVariableSize
DEFINE FV_HDR_SIZE      = 0x48
DEFINE VAR_STORE_SIZE   = $(MDE_MOD_TSPG).$(NV_STOR_VAR_SIZE) - $(FV_HDR_SIZE)
```

### 2.5.2 Block Statements

`BLOCK` statements are used to define block size and the number of blocks that
are needed for FV images that do NOT get put into a physical flash part, such
as the recovery image that gets put on a floppy or USB key.

```
BLOCK_SIZE = VALUE
NUM_BLOCKS = VALUE
```

The following is an example of the BLOCK statement:

`BLOCK_SIZE = 512`

### 2.5.3 FV SET Statements

`SET` statements are used to define the values of PCDs. These statements are
positional within the FDF file. `SET` statements set a `PcdName` to a `VALUE`
for this FV.

`SET <PcdName> = VALUE`

The following is an example of the `SET` statement:

`SET gEfiMyTokenSpaceGuid.PcdDisableOnboardVideo = TRUE`

The `VALUE` specified must match the Pcd's datum type and must be the content
data.

For a PCD that has a datum type of `VOID`*, the data can be a Unicode string,
as in `L"text"`, a valid C data array (it must be either a C format GUID or a
hex byte array), as in `{0x20, 0x00, 0x20, 0x00, 0x32, 0xFF, 0x00, 0xAA, {0xFF, 0xF0, 0x00, 0x00, 0x00}}.`
Other PCD datum types are either boolean values or a hex value, as in
`0x0000000F`, with a value that is consistent with the PCD's datum type

The value may also be a macro or it may be computed, using arithmetic
operations, arithmetic expressions and or logical expressions. The value
portion of the `SET` statement, when using any of these computations are in-fix
expressions that are evaluated left to right, with items within parenthesis
evaluated before the outer expressions are evaluated. Use of parenthesis is
encouraged to remove ambiguity.

### 2.5.4 APRIORI Scoping

Within some firmware volumes, an `APRIORI` file can be created which is a GUID
named list of modules in the firmware volume. The modules will be invoked or
dispatched in the order they appear in the `APRIORI` file. Within a Firmware
Volume, only one PEI and one DXE Apriori file are permitted. Since nested
Firmware Volumes are permitted, Apriori files are limited to specifying the
files, not FVs that are within the scope of the FV image in which it is
located. (It is permissible for nested FV images to have one PEI and one DXE
Apriori file per FV.) Scoping is accomplished using the curly "{}" braces.

The following example demonstrates an example of multiple APRIORI files.

```ini
[Fv.Root]
  DEFINE NT32     = $(WORKSPACE)/EdkNt32Pkg
  DEFINE BuildDir = $(OUTPUT_DIRECTORY)/$(PLATFORM_NAME)/$(TARGET)_$(TOOL_CHAIN_TAG)
  APRIORI DXE {
    FILE DXE_CORE = B5596C75-37A2-4b69-B40B-72ABD6DD8708 {
      SECTION COMPRESS {
        SECTION PE32 = $(BuildDir)/X/Y/Z/B5596C75-37A2-4b69-B40B-72ABD6DD8708-DxeCore.efi
        SECTION VERSION "1.2.3"
      }
    }
    INF VERSION = "1" ${NT32)/Dxe/WinNtThunk/Cpu/Cpu.inf
  }

  FILE FV_IMAGE = EF41A0E1-40B1-481f-958E-6FB4D9B12E76 {
    SECTION GUIDED 3EA022A4-1439-4ff2-B4E4-A6F65A13A9AB {
      SECTION FV_IMAGE = Dxe {
        APRIORI DXE {
          INF a/a/a.inf
          INF a/c/c.inf
          INF a/b/b.inf
        }
        INF a/d/d.inf
        ...
      }
    }
  }
```

In the example above, There are three FFS files in the `Fv.Root` and one
Encapsulated FV image, with the build tools creating an `APRIORI` file that
will dispatch the `DXE_CORE` first, then the CPU module second. In the FV
image, named Dxe, there will be at least five FFS files, the `APRIORI` file,
listing the GUID names of `a.inf`, `c.inf` and `b.inf`, which will be
dispatched in this order. Once complete, the `d.inf` module will be dispatched.

### 2.5.5 INF Statements

The INF statements point to EDK component and EDK II module INF files. Parsing
tools will scan the INF file to determine the type of component or module. The
component or module type is used to reference the standard rules defined
elsewhere in the FDF file.

The format for INF statements is:

`INF [Options] PathAndInfFileName`

The `PathAndInfFileName` is the `WORKSPACE` (or `PACKAGES_PATH`)relative path
and filename.

Using an INF statement will cause the build tools to implicitly build an FFS
file with the `EFI_FV_FILETYPE` based on the INF module's `MODULE_TYPE` and
content. For example, specifying the following lines in an FV section will
generate an FFS file with an `EFI_FV_FILETYPE_DRIVER` with three sections, the
`EFI_SECTION_PE32`, an `EFI_SECTION_VERSION`, and an `EFI_SECTION_DXE_DEPEX`.
While there is no version file defined in the INF - it has been specified by the
`VERSION` option; and there is a dependency file specified in the INF file's
source file list.

```
DEFINE MMP_U_MT = MdeModulePkg/Universal/MemoryTest
INF VERSION = "1.1" $(MMP_U_MT)/NullMemoryTestDxe/NullMemoryTestDxe.inf
```

Valid options for the INF file line are:

**_RuleOverride = RuleName_**

This permits the platform integrator with a method to override the default
rules built into tools, specified in the _EDK II Build Specification_ which
follows the UEFI and PI specifications for EFI FileType construction. If the
module is a binary module, the default rules are implied by the processor,
module type and `BINARY` rule name. Using the explicit named rule here may
compromise the platform's PI specification compliance. The _RuleName_ is either
the reserved word, `BINARY` (that only applies to INF files that contain only
binary content), or the RuleUiName of a `[Rule]` section in this FDF file.

**_USE = ARCH_**

The `USE = ARCH` option is used to differentiate if a single INF file is built
different ways, for example a single INF file is called out multiple times in
the DSC file when building the same module for more than one processor
architecture.

**_VERSION = "String"_**

The `VERSION` option is used to create an `EFI_SECTION_VERSION` section with
the FFS file.

**_UI = "String"_**

The UI option is used to create an `EFI_SECTION_USER_INTERFACE` section for an
INF that may not have specified one.

When `RuleOverride` is not specified for binary module INF, **GenFds** tool
will find the FFS rule with `BINARY` rule name. If `RuleOverride` is specified,
**GenFds** tool will still find FFS rule with the specified rule name.

The following are examples of INF statements:

```
DEFINE IFMP = IntelFrameworkModulePkg
INF USE = IA32 $(EDK_SOURCE)/Sample/Universal/Network/Ip4/Dxe/Ip4.inf
INF $(EDK_SOURCE)/Sample/Universal/Network/Ip4Config/Dxe/Ip4Config.inf
INF RULE_OVERRIDE = MyCompress $(IFMP)/Bus/Pci/IdeBusDxe/IdeBusDxe.inf
```

### 2.5.6 FILE Statements

`FILE` statements are provided so that a platform integrator can include
complete EFI FFS files, as well as a method for constructing FFS files using
curly "{}" brace scoping.

FFS file specification syntax is one of the following:

`FILE Type $(NAMED_GUID) [Options] FileName`

OR

```c
FILE Type $(NAMED_GUID) [Options] {
  SECTION SECTION_TYPE = FileName
  SECTION SECTION_TYPE = FileName
}
```

The first statement is commonly used with `EFI_FV_FILETYPE_RAW` files, while
the second type is used for most other file types. The _FileName_ is typically
a binary file, and the consumer of this type of file must have an a priori
knowledge of the format.

The following describes the information that can be specified a File:

**_Type_**

EFI FV File Types - one and only one of the following:

* `RAW` - Binary data

* `FREEFORM` - Sectioned binary data

* `SEC` - Sectioned data consisting of an optional pad section, a terse section
  and an optional raw section.

* `PEI_CORE` - Sectioned data consisting of one PE32, one user interface and
  one version section.

* `DXE_CORE` - Sectioned data containing one or more other sections.

* `PEIM` - Dispatched by PEI Core

* `DRIVER` - Dispatched by DXE core

* `COMBO_PEIM_DRIVER` - Combined PEIM/DXE driver containing PEI and DXE depex
  sections as well as PE32 and version sections.[^1]

* `SMM_CORE` - Sectioned data containing one or more other sections.

* `DXE_SMM_DRIVER` - Dispatched by the SMM Core

* `APPLICATION` - Application, so will not be dispatched

* `FV_IMAGE` - File contains an FV image

* `DISPOSABLE` - This section type is not supported by the EDK II build system

* `0x00 - 0xFF` - Hex values are legal too. See PI specification Volume 3 for
  details

**_NAMED_GUID_**

The `$(NAMED_GUID)` is usually constructed from an INF file's `[Defines]`
section `FILE_GUID` element.

**_Options_**

The `Fixed` and `Checksum` attributes are boolean flags, both default to
`FALSE,` specifying "Fixed" enables the flag to `TRUE`.

The Alignment attribute requires the "= value".

* `Fixed` - File can not be moved, default (not specified) is relocate-able.

* `Alignment` - Data (value is one of: 1, 2, 4, 8, 16, 32, 64, 128, 512, 1K, 2K,
  4K, 8K, 16K, 32K, 64K, 128K, 256K, 512K, 1M, 2M, 4M, 8M, 16M) byte aligned

* `Checksum` - It is recommended that this be controlled on an entire FV basis
  not at the file level, however, we are including this attribute for
  completeness.

UEFI and PI Specifications have rules for file type construction that, by
default, will be used by the tools.

In addition to the arguments on the `FILE` line, for EFI FV File types that are
not `RAW`, additional EFI section information must be specified.

To specify additional section information for a file, the EFI Encapsulation
Sections must be contained within curly "{}" braces that follow the `FILE`
line, while leaf sections are denoted by an `EFI_SECTION` type keyword.
Encapsulation and leaf section types are described below.

**********
**Caution:** If a fully qualified path and filename are specified, the platform
integrator must ensure that all developers using the DSC and FDF file are aware
of the requirements for this path.
**********

The following is an example for using additional sections:

```c
#Encapsulation - Compress
FILE FOO = 12345678-0000-AAAA-FFFF-0123ABCD12BD {
  SECTION COMPRESS {
    SECTION PE32 = $(WORKSPACE)/EdkModulePkg/Core/Dxe/DxeMain.inf
    SECTION VERSION = "1.2.3"
  }
}

# Encapsulation - GUIDED
FILE FV_IMAGE = 87654321-FFFF-BBBB-2222-9874561230AB {
  SECTION GUIDED gEfiTianoCompressionScheme {
    SECTION PE32 = $(WORKSPACE)/EdkModulePkg/Core/Dxe/DxeMain.inf
  }
}

# LEAF Section
FILE DXE_CORE = B5596C75-37A2-4b69-B40B-72ABD6DD8708 {
  SECTION VERSION $(BUILD_DIR)/$(ARCH)/D6A2CB7F-6A18-4E2F-B43B-9920A733700A-DxeMain.ver
}
```

[^1]: The EDK II build system does not support creation of `COMBO_PEIM_DRIVER` FV type.

#### 2.5.6.1 EFI Encapsulation Sections

There are two types of encapsulation sections, a `COMPRESSION` section and the
`GUIDED` section. The `DISPOSABLE` encapsulation section is not supported by
the EDK II build system.

The `COMPRESS` encapsulation section uses the following format.

```c
SECTION COMPRESS [type] {
  SECTION EFI_SECTION_TYPE = FILENAME
  SECTION EFI_SECTION_TYPE = "string"
}
```

The `[type]` argument is optional, only `EFI_STANDARD_COMPRESSION` is supported
by the PI specification. The current EDK enumerations for compression are a
violation of the PI specification, and `SECTION GUIDED` must be used instead.

The `EFI_SECTION_TYPE` and `FILENAME` are required sub-elements within the
compression encapsulation section. for most sections. However both the
`VERSION` (`EFI_SECTION_VERSION`) and `UI` (`EFI_SECTION_USER_INTEFACE`) may
specify a string, that will be used to create an EFI section.

The `GUIDED` encapsulation section uses one of the following formats.

```c
SECTION GUIDED $(GUID_CNAME) [auth] {
  SECTION EFI_SECTION_TYPE = FILENAME
  SECTION EFI_SECTION_TYPE = "string"
}

SECTION GUIDED $(GUID_CNAME) [auth] FILENAME
```

The required argument is the `GUIDED` name followed by an optional "auth"
flag. If the argument "auth" flag is specified, then the attribute
`EFI_GUIDED_SECTION_AUTH_STATUS_VALID` must be set.

For non-scoped statements (the second `SECTION` statement of the two listed
above,) if filename exists the Attribute
`EFI_GUIDED_SECTION_PROCESSING_REQUIRED` must be set to `TRUE`. The file
pointed to by filename is the data. If filename does not exist
`EFI_GUIDED_SECTION_PROCESSING_REQUIRED` is cleared and normal leaf sections
must be used.

#### 2.5.6.2 EFI Leaf Sections

Leaf sections are identified using the `EFI_SECTION` Type, as specified in the
UEFI specification. Arguments to the `EFI_SECTION` Type include information
that will be used to build a leaf section. Nesting of leaf sections within leaf
sections is not permitted, as a leaf section is defined as UEFI's smallest
entity.

The LEAF section is specified using the following format.

`SECTION LEAF_SECTION [build #] [Align=X] [Unicode String][Filename]`

The following keywords are used for valid `LEAF_SECTION` types.

* `PE32`
* `PIC`
* `TE`
* `DXE_DEPEX`
* `SMM_DEPEX`
* `PEI_DEPEX`
* `VERSION` -- Contains either a 16-bit build number or a Unicode string
* `UI` -- Unicode String
* `COMPAT16`
* `FV_IMAGE`
* `SUBTYPE_GUID` -- A GUID value with content defined by the GUID to be used with
  the section type of `EFI_SECTION_FREEFORM_SUBTYPE_GUID`.

The argument, `build #`, is only valid for `VERSION` leaf section. The number
may be specified in the platform description (DSC) file's `[Defines]` section,
`BUILD_NUMBER` element. EDK INF files may specify a `BUILD_NUMBER` in the
defines section. However, this value is only used if the EDK II DSC file does
not contain a `BUILD_NUMBER` statement.

The **_Filename_** is only optional for `VERSION` and `UI`.

A Unicode string is only valid for `VERSION` or `UI` if the Filename is not
present, and is of the form `L"string"`.

The remaining leaf section types require the **_Filename_** argument. The file
must contain the data for the section.
