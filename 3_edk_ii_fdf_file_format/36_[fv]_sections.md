<!--- @file
  3.6 [FV] Sections

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

## 3.6 [FV] Sections

One or more of theses sections is required for platform images, and not
required for simple Option ROM generation.

#### Summary

This describes the `[FV]` section tag, which is required in all platform FDF
files. This file is created by the platform integrator and is an input to the
one or more parsing utilities.

Note that `FvAttributes`, listed below, may be set via PCD settings. Setting
the attribute via PCD takes precedence over the `FvAttributes` settings in this
FDF file. If the `FvAttribute` is not set by either a PCD or an `FvAttribute`
line in the FDF file, then the default value is `FALSE` - the corresponding bit
in the `EFI_FV_ATTRIBUTE` of the `EFI_FIRMWARE_VOLUME_PROTOCOL.GetVolumeAttributes()`
is set per the `FvAttributesSet` or `FvAttributesClear`; items specified in
`FvAttributesSet` are default "TRUE", while items in `FvAttributesClear` are
default "FALSE".

If FV files are created, they will be created in the
`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV` directory using the values from
the individual instance of the build tools. (Build tools get these values after
parsing DSC, INF, `target.txt`, `tools_def.txt` files and command line options).

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<FV>                ::= "[FV" <FvUiName> "]" <FvStmts>*
<FvStmts>           ::= {<ExtendedFvEntry>} {<FvStatements>} {<GlobalStmts>}
<FvUiName>          ::= "." (a-zA-Z)(a-zA-Z0-9_)*
<ExtendedFvEntry>   ::= <TS> "FV_EXT_ENTRY_TYPE" <MTS> "TYPE" <Eq>
                        <Hex4>
                        "{" [<EOL>]
                        {<TS> "FILE" <Eq> <BinaryFile> [<EOL>]}
                        {<TS> "DATA" <Eq> "{" <DataContent> "}" }
                        "}" <EOL>
<DataContent>       ::= <TS> {<RawData>} {<CFormatGUID>} {<UINT8z>}
                        {<UINT16z>} {<UINT32z>} {<UINT64z>} [<EOL>]
<BinaryFile>        ::= <PATH> <File> [<EOL>]
<FvStatements>      ::= [<BlockStatements>]
                        [<FvAlignment>]
                        [<FvAttributes>]
                        [<FileSystemGuid>]
                        [<FvNameGuid>]
                        [<FvUsedSize>]
                        [<FvNameString>]
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <InfStatements>*
                        <FileStatements>*
<GlobalStmts>       ::= {<MacroDefinition>} {<IncludeStatement>}
<BlockStatements>   ::= <FixedBlocks>
<FixedBlocks>       ::= [<TS> "BlockSize" <Eq> <UINT32> <EOL>]
                        [<TS> "NumBlocks" <Eq> <UINT32> <EOL>]
<SetStatements>     ::= <TS> "SET" <MTS> <PcdName> <Eq> <VALUE> <EOL>
<VALUE>             ::= {<Number>} {<Boolean>} {<GUID>} {<CArray>}
                        {<CString>} {<UnicodeString>} {<Expression>}
<FvAlignment>       ::= [<TS> "FvBaseAddress" <Eq> <UINT64> <EOL>]
                        [<TS> "FvForceRebase" <Eq> <TrueFalse> <EOL>]
                        <TS> "FvAlignment" <Eq>
                        <FvAlignmentValues> <EOL>
<FvAttributes>      ::= [<TS> "MEMORY_MAPPED" <Eq> <TrueFalse> <EOL>]
                        [<TS> "LOCK_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "LOCK_STATUS" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_LOCK_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_LOCK_STATUS" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_ENABLED_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_DISABLED_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_STATUS" <Eq> <TrueFalse> <EOL>]
                        [<TS> "STICKY_WRITE" <Eq> <TrueFalse> <EOL>]
                        [<TS> "WRITE_POLICY_RELIABLE" <Eq> <TrueFalse> <EOL>]
                        [<TS> "READ_LOCK_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "READ_LOCK_STATUS" <Eq> <TrueFalse> <EOL>]
                        [<TS> "READ_ENABLED_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "READ_DISABLED_CAP" <Eq> <TrueFalse> <EOL>]
                        [<TS> "READ_STATUS" <Eq> <TrueFalse> <EOL>]
                        [<TS> "ERASE_POLARITY" <Eq> {"0"} {"1"} <EOL>]
<FileSystemGuid>    ::= <TS> "FileSystemGuid" <Eq> <NamedGuid> <EOL>
<FvNameGuid>        ::= <TS> "FvNameGuid" <Eq> <NamedGuid> <EOL>
<FvUsedSize>        ::= <TS> "FvUsedSizeEnable" <Eq> <TrueFalse> <EOL>
<FvNameString>      ::= <TS> "FvNameString" <Eq> <TrueFalse> <EOL>
<PeiAprioriSection> ::= <TS> "APRIORI" <MTS> "PEI" <MTS>
                        "{" <EOL>
                        <MacroDefinition>*
                        <InfStatements>*
                        <FileStatements>*
                        <TS> "}" <EOL>
<DxeAprioriSection> ::= <TS> "APRIORI" <MTS> "DXE" <MTS>
                        "{" <EOL>
                        <MacroDefinition>*
                        <InfStatements>*
                        <FileStatements>*
                        <TS> "}" <EOL>
<InfStatements>     ::= <TS> "INF" <MTS> [<InfOptions>] <InfFile>
<InfOptions>        ::= [<Use>] [<Rule>] [<SetVer>] [<SetUi>]
<Use>               ::= "USE" <Eq> <TargetArch> <MTS>
<TargetArch>        ::= <arch>
<Rule>              ::= "RuleOverride" <Eq> {<RuleUiName>} {"BINARY"}
                        <MTS>
<SetVer>            ::= "VERSION" <Eq> <CString> <MTS>
<SetUi>             ::= "UI" <Eq> <CString> <MTS>
<InfFile>           ::= if (MODULE_TYPE == "SEC"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "PEIM"):
                        <PATH> <Word> ".inf" [<FS> <RelocFlags>] <EOL> else:
                        <PATH> <Word> ".inf" <EOL>
<RelocFlags>        ::= {"RELOCS_STRIPPED"} {"RELOCS_RETAINED"}
<FileStatements>    ::= {<type1>} {<type2>} {<type3>} {<type4>}
                        {<type5>} <EOL>
<type1>             ::= <TS> "FILE" <MTS> <FvType1> <Eq> <NamedGuid> <Options1>
<type2>             ::= <TS> "FILE" <MTS> <FvType2> <Eq> <NamedGuid> <Options2>
<type3>             ::= <TS> "FILE" <MTS> "RAW" <Eq> <NamedGuidOrPcd>
                        <Options2>
<type4>             ::= <TS> "FILE" <MTS> "NON_FFS_FILE" <Eq> [<NamedGuid>]
                        <Options2>
<type5>             ::= <TS> "FILE" <MTS> "FV_IMAGE" <Eq>
                        <NamedGuidOrPcd> <Options2>
<FvType1>           ::= {"SEC"} {"PEI_CORE"} {"PEIM"}
<FvType2>           ::= {"FREEFORM"} {"PEI_DXE_COMBO"} {"DRIVER"}
                        {"DXE_CORE"} {"APPLICATION"} {"SMM_CORE"} {"SMM"}
<NamedGuidOrPcd>    ::= {<NamedGuid>} {"PCD(" <PcdName> ")"}
<NamedGuid>         ::= {<RegistryFormatGUID>} {"$(NAMED_GUID)"} {<GuidCName>}
<Options1>          ::= [<Use>] [<FileOpts>] <RelocFlags> <MTS>
                        "{" [<EOL>]
                        {<Filename>} {<SectionData>} <TS> <TS> "}" [<EOL>]
<Options2>          ::= [<Use>] [<FileOpts>] <MTS>
                        "{" [<EOL>]
                        <TS> {<Filename>} {<FileList>+} {<SectionData> <EOL>}
                        "}" <EOL>
<FileList>          ::= <TS> [<FfsAlignment>] <NormalFile> <EOL>
<FileOpts>          ::= ["FIXED" <MTS>] ["CHECKSUM" <MTS>]
                        [<FfsAlignment>]
<FfsAlignment>      ::= "Align" <Eq> <FfsAlignmentValues> <MTS>
<Filename>          ::= <TS> {<FvImage>} {<FdImage>} {<NormalFile>} <EOL>
<FvImage>           ::= <TS> "FV" <Eq> <FvUiName> <EOL>
<FdImage>           ::= <TS> "FD" <Eq> <FdUiName> <EOL>
<FdUiName>          ::= {<Word>} {"common"}
<NormalFile>        ::= <PATH> <Word> "." <Word> <EOL>
<SectionData>       ::= <MacroDefinition>*
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <EncapSec>*
                        <LeafSections>*
<LeafSections>      ::= {<VerSection>} {<UiSec>} {<FvImgSection>}
                        {<DataSection>} {<DepexExpSection>}
<VerSection>        ::= <TS> "SECTION" <MTS> [<VerArgs>] "VERSION" <VerUniArg>
<UiSec>             ::= <TS> "SECTION" <MST> [<FfsAlignment>] "UI" <VerUniArg>
<FvImgSection>      ::= <TS> "SECTION" <MTS> [<FfsAlignment>] "FV_IMAGE"
                        <FvImgArgs>
<VerArgs>           ::= [<FfsAlignment>] [<Build>]
<Build>             ::= "BUILD_NUM" <Eq> <BuildVal> <MTS>
<BuildVal>          ::= {<HexDigit> <HexDigit> <HexDigit> <HexDigit>}
                        {"$(BUILD_NUMBER)"}
<VerUniArg>         ::= <Eq> {<StringData>} {<NormalFile>} <EOL>
<StringData>        ::= {<UnicodeString>} {<QuotedString>}
<FvImgArgs>         ::= <Eq> <FvUiName> <MTS>
                        "{" <EOL>
                        <MacroDefinition>*
                        <ExtendedFvEntry>*
                        [<FvAlignment>]
                        [<FvAttributes>]
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <InfStatements>*
                        <FileStatements>*
                        <TS> "}" <EOL>
<DataSection>       ::= {<KnownSection>} {<SubTypeGuid>}
<KnownSection>      ::= <TS> "SECTION" <MTS> [<FfsAlignment>] <SecData>
<SecData>           ::= <LeafSecType> <MTS> [<ChkReloc> <MTS>]
                        <SectionOrFile>
<LeafSecType>       ::= {"COMPAT16"} {"PE32"} {"PIC"} {"TE"} {"RAW"}
                        {"FV_IMAGE"} {"DXE_DEPEX"} {"SMM_DEPEX"}
                        {"UI"} {"PEI_DEPEX"} {"VERSION"}
<SubTypeGuid>       ::= <TS> "SECTION" <MTS> [<FfsAlignment>] <STG_Data>
<STG_Data>          ::= "SUBTYPE_GUID" <MTS> <GuidValue> <Eq>
                        <NormalFile> <EOL>
<ChkReloc>          ::= if ((LeafSectionType == "PE32"
                        || LeafSectionType == "TE")
                        && (MODULE_TYPE == "SEC"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "PEIM")): [<RelocFlags>]
<SectionOrFile>     ::= {<Eq> <NormalFile> <EOL>} {<EncapSec>}
<EncapSec>          ::= <TS> "SECTION" <MTS> [<FfsAlignment>] <EncapSection>
                        <EOL>
<EncapSection>      ::= {<CompressSection>} {<GuidedSection>}
<CompressSection>   ::= <TS> "COMPRESS" <MTS> [<CompType>]
                        "{" <EOL>
                        <EncapSec>*
                        <LeafSections>*
                        <TS> "}" [<EOL>]
<CompType>          ::= {"PI_STD"} {"PI_NONE"} <MTS>
<GuidedSection>     ::= "GUIDED" <MTS> <NamedGuid> <MTS>
                        [<GuidedOptions>]
                        "{" <EOL>
                        <EncapSec>*
                        <LeafSections>*
                        <TS> "}" [<EOL>]
<GuidedOptions>     ::= [<GuidAttrPr>] [<GuidAttrASV>]
                        [<GuidHeaderSize>]
<GuidAttrPr>        ::= "PROCESSING_REQUIRED" <Eq> <TrueFalse> <MTS>
<GuidAttrASV>       ::= "AUTH_STATUS_VALID" <Eq> <TrueFalse> <MTS>
<GuidHeaderSize>    ::= "EXTRA_HEADER_SIZE" <Eq> <Number> <MTS>
<DepexExpSection>   ::= if ( COMPONENT_TYPE == "LIBRARY"
                        || LIBRARY_CLASS is declared in defines section of the
                        INF
                        || MODULE_TYPE == "USER_DEFINED" ):
                        [<Depex>]
                        else if ( MODULE_TYPE == "PEIM"
                        || MODULE_TYPE == "DXE_DRIVER"
                        || MODULE_TYPE == "DXE_RUNTIME_DRIVER"
                        || MODULE_TYPE == "DXE_SAL_DRIVER" || MODULE_TYPE ==
                        "DXE_SMM_DRIVER" ):
                        <Depex>
                        else if ( MODULE_TYPE == "UEFI_APPLICATION"
                        && MODULE_TYPE == "UEFI_DRIVER"
                        && MODULE_TYPE == "PEI_CORE"
                        && MODULE_TYPE == "DXE_CORE"
                        && MODULE_TYPE == "SMM_CORE"
                        && MODULE_TYPE == "SEC" ):
                        No DEPEX section is permitted
<Depex>             ::= if (MODULE_TYPE == "PEIM"):
                        <PeiDepexExp> else if (MODULE_TYPE ==
                        "DXE_SMM_DRIVER"):
                        <SmmDepexExp> [<DxeDepexExp>] else:
                        <DxeDepexExp>
<PeiDepexExp>       ::= <TS> "SECTION" <MTS> [<FfsAlignment>] "PEI_DEPEX_EXP"
                        <Eq> "{" [<EOL>] <PeiDepex> "}" <EOL>
<PeiDepex>          ::= [<TS> <BoolStmt> {<EOL>} {<MTS>}]
                        [<TS> <DepInstruct> {<EOL>} {<MTS>}]
                        [<TS> "end" {<EOL>} {<MTS>}]
<BoolStmt>          ::= {<Bool>} {<BoolExpress>}
                        {<GuidCName>} {<EOL>} {<MTS>}
<Bool>              ::= {"TRUE"} {"FALSE"} {<GuidCName>}
<GuidCName>         ::= <CName> # A Guid C Name
<BoolExpress>       ::= [<Not>] <GuidCName> [<OP> [<Not>] <GuidCName> ]*
<Not>               ::= "NOT" <MTS>
<OP>                ::= <MTS> {"AND"} {"OR"} <MTS>
<DepInstruct>       ::= "push" <MTS> <Filename>
<DxeDepexExp>       ::= <TS> "SECTION" <MTS> [<FfsAlignment>] "DXE_DEPEX_EXP"
                        <Eq> "{" [<EOL>] <DxeDepex> "}" <EOL>
<DxeDepex>          ::= [<TS> <SorStmt> {<EOL>} {<MTS>}]
                        [<TS> <GuidStmt> {<EOL>} {<MTS>}]
                        [<TS> <BoolStmt> {<EOL>} {<MTS>}]
                        [<TS> <DepInstruct> {<EOL>} {<MTS>}]
                        [<TS> "END" {<EOL>} {<MTS>}]
<SorStmt>           ::= "SOR" <BoolStmt>
<GuidStmt>          ::= {"before"} {"after"} <MTS> <Filename>
<SmmDepexExp>       ::= <TS> "SECTION" <MTS> [<FfsAlignment>] "SMM_DEPEX_EXP"
                        <Eq> "{" [<EOL>] <DxeDepex> "}" <EOL>
```

#### Restrictions

**_Filename_**

For BINARY ONLY content (`UEFI_DRIVER` and `UEFI_APPLICATION` .efi files) the
file names specified in the elements (`FILE` and `SECTION`) of this section
must be relative to the directory identified by the `WORKSPACE` or relative to
a path listed in the `PACKAGES_PATH` system environment variable.

**_TargetArch_**

Only specific architectures are permitted - use of "common" is prohibited.

**_FvBaseAddress_**

The `FvBaseAddress`, if present, must be listed before the `FvAlignment`
element. If present, the `FvForceRebase` must immediately follow the
`FvBaseAddress`.

#### Parameters

**_FvBaseAddress_**

A `UINT64` value that will be used to rebase the code to run at a different
address than the address specified by the location of the FV in the FD section.

**_SUBTYPE_GUID_**

This is short hand notation refering to content that will be placed in a
Section of type: EFI_SECTION_FREEFORM_SUBTYPE_GUID. A single

`EFI_SECTION_FREEFORM_SUBTYPE_GUID` section is permitted in an FFS File of type
`EFI_FV_FILETYPE_FREEFORM`

**_GuidCName_**

A word that is a valid C variable for a GUID.

**_Expression_**

Refer to the EDK II Expression Syntax Specification for more information.

**_COMPRESS_**

Compression sections that use `PI_STD` compression do not have
`PROCESSING_REQUIRED = TRUE` flag, it is only required for `GUIDED` sections.

**_User Interface (UI) entries_**

There are three possible methods for specifying a User Interface string. 1)
Specify the string value in the FDF file, 2) specify a ASCII plain text file
that has an extension of ".ui" or 3) specify a Unicode file with an extension
of ".uni" that contains a single Unicode string.

**_Paths_**

Unless otherwise noted, all file paths are relative to the WORKSPACE directory
or relative to a directory listed in the PACKAGES_PATH. In some cases, the
tools will search well known paths for some files, for example, for FD
filenames, the output will typically be located in the
`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV` directory.

#### Related Definitions

Note that no space characters are permitted on the left side of the expression
(before the equal sign).

**_Target_**

This value must match a target identifier in the EDK II _tools_def.txt_ file -
the first field, where fields are separated by the underscore character.
Wildcard characters are not permitted.

**_TagName_**

This must match a tag name field in the EDK II `tools_def.txt` file - second
field. Wildcard characters are not permitted.

#### Example

```ini
[Fv.Root]
  FvAlignment        = 64
  ERASE_POLARITY     = 1
  MEMORY_MAPPED      = TRUE
  STICKY_WRITE       = TRUE
  LOCK_CAP           = TRUE
  LOCK_STATUS        = TRUE
  WRITE_DISABLED_CAP = TRUE
  WRITE_ENABLED_CAP  = TRUE
  WRITE_STATUS       = TRUE
  WRITE_LOCK_CAP     = TRUE
  WRITE_LOCK_STATUS  = TRUE
  READ_DISABLED_CAP  = TRUE
  READ_ENABLED_CAP   = TRUE
  READ_STATUS        = TRUE
  READ_LOCK_CAP      = TRUE
  READ_LOCK_STATUS   = TRUE

  INF VERSION = "1" $(WORKSPACE)/EdkNt32Pkg/Dxe/WinNtThunk/Cpu/Cpu.inf

  FILE DXE_CORE = B5596C75-37A2-4b69-B40B-72ABD6DD8708 {
    SECTION COMPRESS {
      DEFINE DC    = $(WORKSPACE)/Build/Nt32/DEBUG_IA32
      SECTION PE32 = $(DC)/B5596C75-37A2-4b69-B40B-72ABD6DD8708-DxeCore.efi
    }
  }

  FILE FREEFORM = 85C3EBE1-F58F-4820-8AD3-F2FB62DC3A23 {
    FvAlignment = 512K
    SECTION SUBTYPE_GUID AFC13561-9A65-4754-9C93-E133B3B8767C = $(WORKSPACE)/MyPackage/MyNewType/Binary/newform.bin
  }
  FILE FV_IMAGE = EF41A0E1 - 40B1 - 481f - 958E-6FB4D9B12E76 {
    FvAlignment = 512K
    WRITE_POLICY_RELIABLE = TRUE
    SECTION GUIDED 3EA022A4-1439-4ff2-B4E4-A6F65A13A9AB {
      SECTION FV_IMAGE = Dxe {
        APRIORI DXE {
          INF $(WORKSPACE)/a/a.inf
          INF $(EDK_SOURCE/a/c/c.inf
          INF $(WORKSPACE)/a/b/b.inf
        }
        INF a/d/d.inf
        ...
      }
    }
  }
DEFINE SAMPLE = $(EDK_SOURCE)/Sample

INF $(SAMPLE)/Universal/Network/Ip4/Dxe/Ip4.inf
INF $(SAMPLE)/Universal/Network/Ip4Config/Dxe/Ip4Config.inf
INF $(SAMPLE)/Universal/Network/Udp4/Dxe/Udp4.inf
INF $(SAMPLE)/Universal/Network/Tcp4/Dxe/Tcp4.inf
INF $(SAMPLE)/Universal/Network/Dhcp4/Dxe/Dhcp4.inf
INF $(SAMPLE)/Universal/Network/Mtftp4/Dxe/Mtftp4.inf
INF $(SAMPLE)/Universal/Network/SnpNt32/Dxe/SnpNt32.inf

FILE RAW = 197DB236-F856-4924-90F8-CDF12FB975F3 {
  $(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/$PLATFORM_ARCH)/File.bin
}

FILE RAW = 197DB236-F856-4924-90F8-CDF12FB975F3 {
  Align=16 $(PLATFORM_PACKAGE)/Binaries/File1.pdb
  Align=16 $(PLATFORM_PACKAGE)/Binaries/File2.pdb
  Align=16 $(PLATFORM_PACKAGE)/Binaries/File3.pdb
}
```
