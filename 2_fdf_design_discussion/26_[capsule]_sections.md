<!--- @file
  2.6 [Capsule] Sections

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

## 2.6 [Capsule] Sections

The optional `[Capsule]` sections are used to define the components and modules
that make up formatted, variable-length data structure. Capsules were designed
for and are intended to be the major vehicle for delivering firmware volume
changes to an existing implementation. An update capsule is commonly used to
update the firmware flash image or for an operating system to have information
persist across a system reset. A `[Capsule]` header section requires one
modifier, the `UiCapsuleName` modifier. Zero or more `[Capsule]` sections can
be present in a FDF file.

The following is the format for the `[Capsule]` section header:

`[Capsule.UiCapsuleName]`

The first elements of a `[Capsule]` section are required Token elements, using
the following format.

`Token = VALUE`

### 2.6.1 UEFI Implementation

The UEFI specification defines the `EFI_CAPSULE_HEADER` structure in the
runtime services chapter. The header consists of the following elements. The
following tokens are required in a capsule conforming to the UEFI specification.

**_EFI_CAPSULE_GUID_**

The GUID that defines the contents of a capsule, used by the EFI system table,
which must point to one or more capsules that have the same `EFI_CAPSULE_GUID`
value.

**_EFI_CAPSULE_HEADER_SIZE_**

Size in bytes of the capsule header. If the size specified here is larger than
the size of the `EFI_CAPSULE_HEADER`, then the capsule `GUID` value implies
extended header entries.

**_EFI_CAPSULE_FLAGS_**

Currently, three bit flags have been defined:

```
PersistAcrossReset  = CAPSULE_FLAGS_PERSIST_ACROSS_RESET
InitiateReset       = CAPSULE_FLAGS_INITIATE_RESET and
PopulateSystemTable = CAPSULE_FLAGS_POPULATE_SYSTEM_TABLE
```

The value of the `EFI_CAPSULE_IMAGE_SIZE`, which is the size in bytes of the
capsule, is determined by the tools.

In order to use the `InitiateReset` flag, the `PersistAcrossReset` flag must
also be set.

### 2.6.2 Capsule SET Statements

`SET` statements are used to define the values of PCD statements. These
statements are positional in the FDF file. `SET` statements are set for the
`[Capsule]` section, those set under the `[Capsule]` section header are global
for all sub-sections within the `[Capsule]` section.

`SET PcdName = VALUE`

The following is an example of the `SET` statement.

`SET gEfiMyTokenSpaceGuid.PcdSecStartLocalApicTimer = TRUE`

The `VALUE` specified must match the PCD's datum type and must be the content
data.

For a PCD that has a datum type of `VOID`*, the data can be a Unicode string,
as in `L"text"`, a valid C data array (it must be either a C format GUID or a
hex Byte array), as in `{0x20002000, 0x32FF, 0x00AA, {0xFF, 0xF0, 0x00, 0x00, 0x00, 0xF0, 0x00, 0x00, 0x00, 0xEF, 0x1A, 0x55}}.`
Other PCD datum types are either boolean values or a hex value, as in
`0x0000000F`, with a value that is consistent with the PCD's datum type.

### 2.6.3 Capsule Data

`EFI_CAPSULE_DATA` follows the `EFI_CAPSULE_HEADER` token definitions in the
`[Capsule]` section or sub-sections. The content consists of one or more files,
FD UiName, FV UiName or the following.

#### 2.6.3.1 INF Statements

The INF statement syntax is common to the syntax used for `[FV]` sections.
Refer to section 2.4.4 above.

#### 2.6.3.2 FILE Statements

FILE statement syntax is common to the syntax used for `[FV]` sections. Refer
to Section 2.5.5.
