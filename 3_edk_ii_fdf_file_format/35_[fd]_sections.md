<!--- @file
  3.5 [FD] Sections

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

## 3.5 [FD] Sections

This is a required section for platform flash parts, and is not required for
simple Option ROM creation.

#### Summary

This describes the `[FD]` section tag, which is required in all FDF files. This
file is created by the platform integrator and, along with the platform DSC
file, is an input to the parsing utilities.

All FD files will be created in the
`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV` directory using the values from
the individual instance of the build tools. (Build tools get these values after
parsing DSC, INF, `target.txt`, `tools_def.txt` files and command line options).

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<FD>               ::= "[FD" [<FdUiName>] "]" <EOL>
                       <TokenStatements>
                       <FdStatements>*
<FdStatements>     ::= {<GlobalStmts>} {<SetStatements>}
                       {<RegionLayout>}
<GlobalStmts>      ::= {<MacroDefinition>} {<IncludeStatement>}
<FdUiName>         ::= "." (a-zA-Z)(a-zA-Z0-9_)*
<TokenStatements>  ::= <TS> "BaseAddress" <Eq> <UINT64> [<SetPcd>]
                       <EOL>
                       <TS> "Size" <Eq> <UINT64> [<SetPcd>] <EOL>
                       <TS> "ErasePolarity" <Eq> {"0"} {"1"} <EOL>
                       <BlockStatements>+
<SetPcd>           ::= <FS> {<PcdName>} {<PcdFieldName>}
<BlockStatements>  ::= <TS> "BlockSize" <Eq> <UINT32> [<SetPcd>]
                       <EOL>
                       [<TS> "NumBlocks" <Eq> <UINT32> <EOL>]
<SetStatements>    ::= <TS> "SET" {<PcdName>} {<PcdFieldName>} <Eq> <VALUE> <EOL>
<VALUE>            ::= {<Number>} {<Boolean>} {<GUID>} {<CArray>}
                       {<CString>} {<UnicodeString>} {<Expression>}
<RegionLayout>     ::= <TS> <Offset> <FS> <Size> <EOL>
                       [<TS> <PcdOffset> [<FS> <PcdSize>] <EOL>]
                       [<RegionType>]
<Offset>           ::= {<HexNumber>} {<Expression>}
<Size>             ::= {<HexNumber>} {<Expression>}
<RegionType>       ::= {<FvType>} {<FileType>} {<CapsuleRegion>}
                       {<DataType>} {<InfRegion>}
<InfRegion>        ::= <TS> "INF" <MTS> [<InfOptions>] <InfFile> <EOL>
<InfOptions>       ::= [<Use>] [<Rule>] [<SetVer>] [<SetUi>]
<Use>              ::= "USE" <Eq> <TargetArch> <MTS>
<TargetArch>       ::= <arch>
<Rule>             ::= "RuleOverride" <Eq> {<Word>} {"BINARY"} <MTS>
<SetVer>           ::= "VERSION" <Eq> <CString> <MTS>
<SetUi>            ::= "UI" <Eq> <CString> <MTS>
<InfFile>          ::= <PATH> <Word> ".inf" [<FS> <RelocFlags>]
<RelocFlags>       ::= {"RELOCS_STRIPPED"} {"RELOCS_RETAINED"}
<CapsuleRegion>    ::= <TS> "CAPSULE" <Eq> UiCapsuleName <EOL>
<PcdOffset>        ::= {<PcdName>} {<PcdFieldName>}
<PcdSize>          ::= {<PcdName>} {<PcdFieldName>}
<FvType>           ::= <TS> "FV" <Eq> <FvNameOrFilename> <EOL>
<FileType>         ::= <TS> "FILE" <Eq> <BinaryFile> <EOL>
<DataType>         ::= <TS> "DATA" <Eq>
                       "{" [<EOL>]
                       [<DataContent>]
                       <TS> "}" <EOL>
<DataContent>      ::= <TS> {<RawData>} {<CFormatGUID>} {<UINT8z>} [<EOL>]
<BinaryFile>       ::= [<Location>] <File>
<Location>         ::= {<BuildId>} {<PATH>}
<BuildId>          ::= <VarLoc> {"FV"} {<arch>} "/"
<VarLoc>           ::= "$(OUTPUT_DIRECTORY)/$(TARGET)_($TOOL_CHAIN_TAG)/"
<FvNameOrFilename> ::= {<FvUiName>} {<FvFilename>}
<FvUiName>         ::= {<Word>} {"common"}
<FvFilename>       ::= [<PATH>] <Word> "." {"fv"} {"Fv"} {"FV"}
```

#### Restrictions

For the `FvFilename`, the PATH is relative to the EDK II environment variable
`$(WORKSPACE)`or relative to a path listed in the `PACKAGES_PATH` environment
variable. If the path is not specified, the `PATH` defaults to the following
location, where `$(OUTPUT_DIRECTORY)` is specified in the EDK II Platform DSC
file's `[Defines]` section. If a path is not present, or the "_.fv_" file
extensions do not appear in the value, the build system will use a filename
based on the UiFvFilename specified in the FDF file:

`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/FV`

For the Binary File, the `PATH` must be relative to the EDK II environment
variable:`$(WORKSPACE)`or relative to a path listed in the `PACKAGES_PATH`
environment variable. If not specified, the `PATH` defaults to the directory
location of the EDK II Platform DSC file. If not found, the tools must test the
directory location of this FDF file, if different from the directory containing
the Platform's DSC file.

If a GUID is used (either the C format or Registry Format) in the data content,
tools will be required to process the GUID into a byte array.

Raw Data arrays in FDF files are always listed as byte arrays, using
little-endian format. Numbers listed in the DataContent section must be zero
filled, in order to determine the size of the element. For example, a `UINT16`
value of 1 must be specified as `0x0001`.

#### Parameters

**_FdUiName_**

This name is used by the _`GenFds`_ tool to generate FD image files. If not
present, only one FD section is allowed and the _`GenFds`_ tool will use the
name of the active platform as the name of the FD image.

**_UiCapsuleName_**

The `UiCapsuleName` must be specified in a `[Capsule]` section header defined
in this the file.

**_FvUiName_**

The `FvUiName` must be specified in a `[FV]` section header defined in this the
file.

**_PcdValue_**

The PCD Value may be a specific numeric value, an array of numeric bytes, a
GUID, a quoted string, an L quoted string (representing a unicode string), an
arithmetic expression, a logic expression or a macro from a previously defined
macro statement.

**_Expression_**

Refer to the EDK II Expression Syntax Specification for more information.

**_Location_**

For BINARY ONLY files, the location specified in the FILE element of this
section must be relative to the directory identified by the WORKSPACE or
relative to a path listed in the PACKAGES_PATH system environment variable.

#### Example

```ini
[FD.FdMain]
BaseAddress   = 0xFFF00000 |gEfiMyPlatformTokenSpaceGuid.PcdFlashAreaBaseAddress
Size          = 0x102000
ErasePolarity = 1
BlockSize     = 0x10000
NumBlocks     = 16
BlockSize     = 0x1000
NumBlocks     = 2

# Offset:Size
0x000000|0x0C0000
gEfiMyPlatformTokenSpaceGuid.PcdFlashFvMainBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashFvMainSize
FV   = FvMain

0x0C0000|0x00A000
gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageSize
Data = { # Variable Store
0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C,
0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50,
0x00, 0x00, 0x02, 0x00, 0x00, 0x00, 0x00, 0x00,
0x5F, 0x46, 0x56, 0x48, 0xFF, 0x8E, 0xFF, 0xFF,
0x5A, 0xFE, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
}

0x0CA000 | 0x002000
gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchAddress|gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchSize
FILE = FV/Microcode.bin

0x0CC000 | 0x002000 # Event Log
gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageEventLogBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageEventLogSize

0x0CE000 | 0x002000 # FTW Working Area
gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageFtwWorkingBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageFtwWorkingSize
Data = {
0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C,
0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50,
0x85, 0xAE, 0x2D, 0xBF, 0xFE, 0xFF, 0xFF, 0xFF,
0xE4, 0x1F, 0x00, 0x00, 0xFF, 0xFF, 0xFF, 0xFF
}

0x0D0000 | 0x010000 # FTW Spare Block
gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageFtwSpareBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashNvStorageFtwSpareSize

0x0E0000 | 0x020000
gEfiMyPlatformTokenSpaceGuid.PcdFlashFvRecoveryBase|gEfiMyPlatformTokenSpaceGuid.PcdFlashFvRecoverySize
FV = FV/FvRecovery.fv
```
