<!--- @file
  3.9 [Rule] Sections

  Copyright (c) 2006-2018, Intel Corporation. All rights reserved.<BR>

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

## 3.9 [Rule] Sections

These are optional sections that describes the `[Rule]` content found in FDF
files.

#### Summary

This section is similar to the `[FV]` section, with the a few exceptions. The
INF statements are not permitted within a rules section.

Rules are used as templates, normally using variable names instead of fully
qualified names, while INF statements are always are fully qualified file names.

The following list specifies the allowed variables that may be used exactly as
typed:

`$(WORKSPACE)`, `$(EDK_SOURCE)`, `$(EFI_SOURCE)`, `$(TARGET)`,
`$(TOOL_CHAIN_TAG)`, `$(ARCH)`, `$(MODULE_NAME)`, `$(OUTPUT_DIRECTORY)`,
`$(BUILD_NUMBER)`, `$(INF_VERSION)`, `$(NAMED_GUID)`, `$(INF_OUTPUT)`

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<Rules>             ::= "[Rule" <RuleArgs> "]" <EOL> <FileStatements>
<RuleArgs>          ::= "." <arch> "." <ModuleType>
                        [<TemplateName>]
<ModuleType>        ::= {<EdkComponentType>} {<Edk2ModuleType>}
<Edk2ModuleType>    ::= {"SEC"} {"PEI_CORE"} {"PEIM"} {"SMM_CORE"}
                        {"DXE_CORE"} {"DXE_DRIVER"}
                        {"DXE_SAL_DRIVER"} {"DXE_SMM_DRIVER"}
                        {"MM_CORE_STANDALONE"} {"MM_STANDALONE"}
                        {"DXE_RUNTIME_DRIVER"} {"UEFI_DRIVER"}
                        {"UEFI_APPLICATION"} {"USER_DEFINED"}
<EdkComponentType>  ::= {"LIBRARY"} {"APPLICATION"} {"AcpiTable"}
                        {"BINARY"} {"BS_DRIVER"} {"LOGO"}
                        {"Legacy16"} {"Microcode"} {"PE32_PEIM"}
                        {"RAWFILE"} {"RT_DRIVER"} {"SAL_RT_DRIVER"}
                        {"SECURITY_CORE"} {"COMBINED_PEIM_DRIVER"}
                        {"PIC_PEIM"} {"RELOCATABLE_PEIM"}
                        {"PEI_CORE"}
<TemplateName>      ::= "." <RuleUiName>
<RuleUiName>        ::= {<Word>} {"BINARY"}
<FileStatements>    ::= if (MODULE_TYPE == "SEC"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "PEIM"
                        || COMPONENT_TYPE == "PEI_CORE"
                        || COMPONENT_TYPE == "PIC_PEIM"
                        || COMPONENT_TYPE == "RELOCATABLE_PEIM"
                        || COMPONENT_TYPE == "SECURITY_CORE"
                        || COMPONENT_TYPE == "PE32_PEIM" ):
                        <TS> "FILE" <MTS> <FvType1> <Eq>
                        <FileStatement1>
                        elif (MODULE_TYPE == "DXE_CORE" 
                        || MODULE_TYPE == "DXE_DRIVER"
                        || MODULE_TYPE == "DXE_SAL_DRIVER"
                        || MODULE_TYPE == "SMM_CORE"
                        || MODULE_TYPE == "MM_CORE_STANDALONE"
                        || MODULE_TYPE == "DXE_SMM_DRIVER"
                        || MODULE_TYPE == "MM_STANDALONE"
                        || MODULE_TYPE == "UEFI_DRIVER"
                        || MODULE_TYPE == "UEFI_APPLICATION"
                        || MODULE_TYPE == "USER_DEFINED"
                        || COMPONENT_TYPE == "BS_DRIVER"
                        || COMPONENT_TYPE == "COMBINED_PEIM_DRIVER"
                        || COMPONENT_TYPE == "APPLICATION"):
                        {<FileStatement2>} {<FileStatement3>}
                        elif (MODULE_TYPE == "FV_IMAGE"):
                        <FileStatement4>
                        else:
                        <TS> "FILE" <MTS> "NON_FFS_FILE" <Eq>
                        [<NamedGuid>] [<Options>] <EOL>
<FileStatement1>    ::= <NamedGuid> [<RelocFlags> <MTS>] [<Options>] <EOL>
<FileStatement2>    ::= <TS> "FILE" <MTS> <FvType2> <Eq> <NamedGuid>
                        [<Options>] <EOL>
<FileStatement3>    ::= <TS> "FILE" <MTS> "RAW" <Eq> <NamedGuid>
                        [<Options>] <EOL>
<FileStatement4>    ::= <TS> "FILE" <MTS> "FV_IMAGE" <Eq>
                        <NamedGuid> [<Options>] <EOL>
<NamedGuid>         ::= {"$(NAMED_GUID)"} {<NamedGuidOrPcd>}
<FvType1>           ::= {"SEC"} {"PEI_CORE"} {"PEIM"} {"PEI_DXE_COMBO"}
<FvType2>           ::= {"FREEFORM"} {"DRIVER"} {"DXE_CORE"}
                        {"APPLICATION"} {"SMM_CORE"} {"SMM"}
                        {"MM_CORE_STANDALONE"} {"MM_STANDALONE"}
<RelocFlags>        ::= {"RELOCS_STRIPPED" <MTS>}
                        {"RELOCS_RETAINED" <MTS>}
<Options>           ::= [<UseLocal>] [<FileOpts>] <FileSpec>
<UseLocal>          ::= <KeyString> ["," <KeyString>]
<KeyString>         ::= <Target> "_" <TagName> "_" <ToolArch>
<Target>            ::= {<Word>} {"$(TARGET)"}
<TagName>           ::= {<Word>} {"$(TOOL_CHAIN_TAG)"}
<ToolArch>          ::= {<Arch>} {"$(ARCH)"}
<FileOpts>          ::= ["Fixed" <MTS>] ["Checksum" <MTS>]
                        [<FfsAlignment>]
<FfsAlignment>      ::= "Align" <Eq> <FfsAlignmentValues> <MTS>
<FileSpec>          ::= {<SimpleFile>} {<ComplexFile>} {<SbtGuid>} {<DataFile>}
<DataFile>          ::= "{" <VarFile> "}"
<SimpleFile>        ::= <LeafSecType> [<FileOpts>] <VarFile> <EOL>
<LeafSecType>       ::= {"COMPAT16"} {"PE32"} {"PIC"} {"TE"}
                        {"FV_IMAGE"} {"RAW"} {"DXE_DEPEX"} {"UI"}
                        {"PEI_DEPEX"} {"SMM_DEPEX"} {"VERSION"}
<SbtGuid>           ::= "SUBTYPE_GUID" <MTS> <NamedGuid> <MTS> <FName> <EOL>
<VarFile>           ::= {<FilenameVariable>} {<FName>} {<Ext>}
<FName>             ::= [<PATH>] <Word> "." <Word>
<FilenameVariable>  ::= "$(INF_OUTPUT)/$(MODULE_NAME)" "." <Word>
<ComplexFile>       ::= "{" <EOL>
                        <EncapSection>*
                        <LeafSections>*
                        <TS> "}" <EOL>
<EncapSection>      ::= <TS> {<CompressSection>} {<GuidedSection>}
<CompressSection>   ::= "COMPRESS" <MTS> [<CompType>]
                        "{" [<EOL>]
                        <EncapSec>*
                        <LeafSections> <EOL>*
                        <TS> "}" <EOL>
<CompType>          ::= {"PI_STD"} {"PI_NONE"} <MTS>
<GuidedSection>     ::= "GUIDED" <MTS> <NamedGuid> <MTS> [<GAttr>]
                        "{" <EOL>
                        <EncapSec>*
                        <LeafSections>*
                        <TS> "}" <EOL>
<GAttr>             ::= [<GuidAttrPR>] [<GuidAttrASV>] [<GuidHeaderSize>]
<GuidAttrPR>        ::= "PROCESSING_REQUIRED" <Eq> <BoolType> <MTS>
<GuidAttrASV>       ::= "AUTH_STATUS_VALID" <Eq> <BoolType> <MTS>
<GuidHeaderSize>    ::= "EXTRA_HEADER_SIZE" <Eq> <Number> <MTS>
<LeafSections>      ::= <TS> {<C16Sec>} {<PeSec>} {<PicSec>} {<TeSec>}
                        {<FvSec>} {<RawSec>} {<DxeDepSec>}
                        {<UiSec>} {<VerSec>} {<PeiDepSec>}
                        {<SmmDepSec>} {<SubtypeGuidSec>}
<C16Sec>            ::= "COMPAT16" <MTS> <C16FileType> [<FileOrExt>] <EOL>
<PeSec>             ::= "PE32" <MTS> <Pe32FileType> [<FileOrExt>] <EOL>
<PicSec>            ::= "PIC" <MTS> <PicFileType> [<FileOrExt>] <EOL>
<TeSec>             ::= "TE" <MTS> <TeFileType> [<FileOrExt>] <EOL>
<RawSec>            ::= "RAW" <MTS> <RawFileType> [<FileOrExtOrPcd>] <EOL>
<DxeDepSec>         ::= "DXE_DEPEX" <MTS> <DdFileType> [<FileOrExt>] <EOL>
<SmmDepSec>         ::= "SMM_DEPEX" <MTS> <DdFileType> [<FileOrExt>] <EOL>
<UiSec>             ::= "UI" <MTS> <UiFileType> [<FileOrExt>] <EOL>
<VerSec>            ::= "VERSION" <MTS> <VerFileType> [<FileOrExt>] <EOL>
<PeiDepSec>         ::= "PEI_DEPEX" <MTS> <PdFileType>
                        [<FileOrExt>] <EOL>
<SubtypeGuidSec>    ::= "SUBTYPE_GUID" <MTS> <NamedGuid> <MTS>
                        <File> <EOL>
<FileOrExt>         ::= {<VarFile>} {<Ext>} <MTS>
<FileOrExtOrPcd>    ::= {<VarFile>} {<Ext>} {"PCD(" <PcdName> ")"}
<Ext>               ::= <FS> "." [a-zA-Z][a-zA-Z0-9]{0,}
<C16FileType>       ::= "COMPAT16" [<FfsAlignment>]
<Pe32FileType>      ::= "PE32" <MTS> [<FfsAlignment>]
                        [<ChkReloc>]
<ChkReloc>          ::= if (MODULE_TYPE == "SEC"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "PEIM"): [<RelocFlags>]
<PicFileType>       ::= "PIC" <MTS> [<FfsAlignment>]
<TeFileType>        ::= "TE" <MTS> [<FfsAlignment>] [<ChkReloc>]
<RawFileType>       ::= {<BinTypes>} {<AcpiFileTypes>} <MTS>
                        [<FfsAlignment>]
<BinTypes>          ::= {"BIN"} {"RAW"}
<AcpiFileTypes>     ::= {"ACPI"} {"ASL"} <MTS> ["Optional" <MTS>]
<DdFileType>        ::= {"DXE_DEPEX"} {"SMM_DEPEX"} [<DpxAlign>]
<DpxAlign>          ::= ["Optional" <MTS>] [<FfsAlignment>]
<UiFileType>        ::= {<UiFile>} {<UiString>}
<UiFile>            ::= "UI" <MTS> [<UiOpts>]
<UiString>          ::= "STRING" <Eq> <StringVal> [<FfsAlignment>]
<StringVal>         ::= {<UnicodeString>} {<QuotedString>}
                        {<NamedMacros>}
<NamedMacros>       ::= {"$(INF_VERSION)"} {"$(MODULE_NAME))"
<UiOpts>            ::= ["Optional" <MTS>] [<FfsAlignment>]
<VerFileType>       ::= {<VerFile>} {<VerString>}
<VerFile>           ::= "VERSION" <MTS> [<VerOpts>]
<VerOpts>           ::= ["Optional" <MTS>] [<BuildAlign>]
<VerString>         ::= "STRING" <Eq> <VerStringVal> <MTS> [<BuildAlign>]
<VerStringVal>      ::= {<UnicodeString>} {<QuotedString>}
                        {"$(INF_VERSION)"}
<BuildAlign>        ::= ["Optional" <MTS>] [<BuildArg>]
                        [<FfsAlignment>]
<BuildArg>          ::= "BUILD_NUM = $(BUILD_NUMBER)" <MTS>
<PdFileType>        ::= "PEI_DEPEX" <MTS> [<DpxAlign>]
<FvSec>             ::= "FV_IMAGE" <MTS> {<FvBin>} {<FvImageSection>}
<FvBin>             ::= "FV" <MTS> [<FfsAlignment>] [<FileOrExt>] <EOL>
<FvImgSection>      ::= "FV_IMAGE" <MTS> <FvImgArgs>
<FvImgArgs>         ::= "{" <EOL>
                        <MacroDefinition>*
                        [<FvAlignment>]
                        <FvAttributes>*
                        [<AprioriSection>]
                        <FileStatements>*
                        <TS> "}" <EOL>
<FvAlignment>       ::= [<TS> "FvBaseAddress" <Eq> <UINT64> <EOL>]
                        <TS> "FvAlignment" <Eq> <FvAlignmentValues> <EOL>
<FvAttributes>      ::= [<TS> "MEMORY_MAPPED" <Eq> <BoolType> <EOL>]
                        [<TS> "LOCK_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "LOCK_STATUS" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_LOCK_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_LOCK_STATUS" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_ENABLED_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_DISABLED_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_STATUS" <Eq> <BoolType> <EOL>]
                        [<TS> "STICKY_WRITE" <Eq> <BoolType> <EOL>]
                        [<TS> "WRITE_POLICY_RELIABLE" <Eq> <BoolType> <EOL>]
                        [<TS> "READ_LOCK_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "READ_LOCK_STATUS" <Eq> <BoolType> <EOL>]
                        [<TS> "READ_ENABLED_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "READ_DISABLED_CAP" <Eq> <BoolType> <EOL>]
                        [<TS> "READ_STATUS" <Eq> <BoolType> <EOL>]
<PeiAprioriSection> ::= "APRIORI PEI" <MTS>
                        "{" <EOL>
                        <MacroDefinition>*
                        <FileStatements>* <TS> "}" <EOL>
<DxeAprioriSection> ::= "APRIORI DXE" <MTS>
                        "{" <EOL>
                        <MacroDefinition>*
                        <FileStatements>*
                        <TS> "}" <EOL>
```

#### Restrictions

**_FName_**

For BINARY ONLY content (`UEFI_DRIVER` and `UEFI_APPLICATION` .efi files) the
file names specified in the `SECTION` element of this section must be relative
to the directory identified by the `WORKSPACE` system environment variable (or
relative to a directory listed in the `PACKAGES_PATH`).

#### Parameters

**_$(INF_VERSION)_**

This value refers to the `VERSION_STRING` element in the INF file's `[Defines]`
section, not the `INF_VERSION` element, which is assigned based on the INF
specification revision.

**_SUBTYPE_GUID_**

This is short hand notation refering to content that will be placed in a
Section of type: `EFI_SECTION_FREEFORM_SUBTYPE_GUID`. A single

`EFI_SECTION_FREEFORM_SUBTYPE_GUID` section is permitted in an FFS File of type
`EFI_FV_FILETYPE_FREEFORM`

**_RuleUiName_**

A unique single word identifier. The word `"BINARY"` is reserved; it is
recommended that it be used for the rules that process INF modules that only
contain binary content.

**_COMPRESS_**

Compression sections that use `PI_STD` compression do not have
`PROCESSING_REQUIRED = TRUE` flag, it is only required for GUIDED sections.

**_User Interface (UI) entries_**

There are three possible methods for specifying a User Interface string. 1)
Specify the string value in the FDF file, 2) specify a plain ASCII text file
that has an extension of ".ui" or 3) specify a Unicode file with an extension
of ".uni" that contains a single Unicode string.

#### Related Definitions

**_Target_**

Must match a target identifier in the EDK II _tools_def.txt_ file - the first
field, where fields are separated by the underscore character. Wildcard
characters are not permitted.

**_TagName_**

Must match a tag name field in the EDK II _tools_def.txt_ file - second field.
Wildcard characters are not permitted

#### Example

```ini
[Rule.IA32.SEC]
  FILE SEC = $(NAMED_GUID) Fixed Align=32 |.efi

[Rule.Common.PEIM]
  FILE PEIM = $(NAMED_GUID) {
    TE TE |.te
    PEI_DEPEX PEI_DEPEX Optional |.Depex
    VERSION STRING    = "$(INF_VERSION)" Optional BUILD_NUM = $(BUILD_NUM)
    UI UNI_UI Optional | .uni
  }

[Rule.Common.PEIM.PE32]
  FILE PEIM = $(NAMED_GUID) {
    PEI_DEPEX PEI_DEPEX Optional | .dxs
    COMPRESS {
      PE32 PE32 |.efi
      VERSION UNI_VER Optional BUILD_NUM = $(BUILD_NUM) | .ver
      UI UI Optional | .ui
    }
  }
```
