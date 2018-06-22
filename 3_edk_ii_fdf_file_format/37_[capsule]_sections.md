<!--- @file
  3.7 [Capsule] Sections

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

## 3.7 [Capsule] Sections

These sections are optional.

#### Summary

If capsule files are created, they will be created in the
`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV` directory using the values from
the individual instance of the build tools. (Build tools get these values after
parsing DSC, INF, `target.txt`, `tools_def.txt` files and command line options.)

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<Capsule>           ::= "[Capsule" <UiCapsuleName> "]" <EOL>
                        <UefiTokens>
                        <CapsuleStmts>*
<UiCapsuleName>     ::= "." <Word>
<SetStatements>     ::= <TS> "SET" <MTS> {<PcdName>} {<PcdFieldName>} <Eq> <VALUE> <EOL>
<VALUE>             ::= {<Number>} {<Boolean>} {<GUID>} {<CArray>}
                        {<CString>} {<UnicodeString>} {<Expression>}
<UefiTokens>        ::= <TS> "CAPSULE_GUID" <Eq> <NamedGuidOrPcd> <EOL>
                        [<TS> "CAPSULE_HEADER_SIZE" <Eq> <Bytes> <EOL>] [<TS>
                        "CAPSULE_FLAGS" <Eq> <Flags> <EOL>]
                        [<TS> "CAPSULE_HEADER_INIT_VERSION" <Eq> <Hex2> <EOL>]
<CapsuleStmts>      ::= {<MacroDefinition>} {<SetStatements>}
                        {<CapsuleData>}
<Flags>             ::= <FlagName>
<FlagName>          ::= {"PersistAcrossReset"}
                        {"PersistAcrossReset" "," "InitiateReset"}
                        {"PersistAcrossReset" "," "PopulateSystemTable"}
                        {"PersistAcrossReset" "," "PopulateSystemTable"
                        "," "InitiateReset"}
                        {"PersistAcrossReset" "," InitiateReset"
                        "," "PopulateSystemTable"}
                        {"PopulateSystemTable"}
                        {"PopulateSystemTable" "," "PersistAcrossReset"}
                        {"PopulateSystemTable" "," "PersistAcrossReset"
                        "," "InitiateReset"}
                        {"PopulateSystemTable" "," "InitiateReset" ","
                        "PersistAcrossReset"}
                        {"InitiateReset" "," "PersistAcrossReset"}
                        {"InitiateReset" "," "PersistAcrossReset"
                        "," "PopulateSystemTable"}
                        {"InitiateReset" "," "PopulateSystemTable" ","
                        "PersistAcrossReset"}
<CapsuleData>       ::= <InfStatements>*
                        <FileStatements>*
                        <FvStatements>*
                        <FdStatenents>*
                        <FmpFileStatement>*
                        <FmpPayload>* <Afile>*
<InfStatements>     ::= <TS> "INF" <MTS> [<InfOptions>] <InfFile> <EOL>
<InfOptions>        ::= [<Use>] [<Rule>] [<SetVer>] [<SetUi>]
<Use>               ::= "USE" <Eq> <TargetArch> <MTS>
<TargetArch>        ::= <arch>
<Rule>              ::= "RuleOverride" <Eq> {<RuleUiName>} {"BINARY"} <MTS>
<SetVer>            ::= "VERSION" <Eq> <CString> <MTS>
<SetUi>             ::= "UI" <Eq> <CString> <MTS>
<InfFile>           ::= if (MODULE_TYPE == SEC
                        || MODULE_TYPE == PEI_CORE
                        || MODULE_TYPE == PEIM):
                        <PATH> <Word> ".inf" [<FS> <RelocFlags>] else:
                        <PATH> <Word> ".inf"
<RelocFlags>        ::= {"RELOCS_STRIPPED"} {"RELOCS_RETAINED"}
<KeyString>         ::= <Target> "_" <TagName> "_" <TargetArch>
<Target>            ::= {Target} {"$(TARGET)"}
<TagName>           ::= {TagName} {"$(TOOL_CHAIN_TAG)"}
<FileStatements>    ::= <TS> {<type1>} {<type2>} {<type3>} {<type4>}
<type1>             ::= "FILE" <FvType1> <Eq> <NamedGuidOrPcd> <Options1>
<type2>             ::= "FILE" <FvType2> <Eq> <NamedGuidOrPcd> <Options2>
<type3>             ::= "FILE" "RAW" <Eq> <NamedGuidOrPcd>
                        <Options2>
<type4>             ::= "FILE" "NON_FFS_FILE" <Eq> [<NamedGuidOrPcd>] <Options2>
<type5>             ::= "FILE" "FV_IMAGE" <Eq> <NamedGuidOrPcd>
                        <Options2>
<FvType1>           ::= {"SEC"} {"PEI_CORE"} {"PEIM"}
<FvType2>           ::= {"FREEFORM"} {"PEI_DXE_COMBO"} {"DRIVER"}
                        {"DXE_CORE"} {"APPLICATION"} {"SMM_CORE"} {"SMM"}
<Options1>          ::= [<Use>] [<FileOpts>] [<RelocFlags>]
                        "{" [<EOL>]
                        <TS> {<Filename>} {<SectionData>} [<EOL>] <TS> "}"
                        <EOL>
<Options2>          ::= [<Use>] [<FileOpts>]
                        "{" [<EOL>]
                        {<Filename>} {<FileList>+} {<SectionData> <EOL>}
                        <TS> "}" <EOL>
<FileList>          ::= <TS> [<FfsAlignment>] <NormalFile> <EOL>
<FileOpts>          ::= ["FIXED" <MTS>] ["CHECKSUM" <MTS>]
                        [<FfsAlignment>]
<FfsAlignment>      ::= "Align" <Eq> <FfsAlignmentValues>
<FvAlignment>       ::= [<TS> "FvBaseAddress" <Eq> <UINT64> <EOL>]
                        [<TS> "FvForceRebase" <Eq> <TrueFalse> <EOL>]
                        "FvAlignment" <Eq> <FvAlignmentValues> <EOL>
<Filename>          ::= <TS> {<FvImage>} {<FdImage>} {<NormalFile>} <EOL>
<FvImage>           ::= "FV" <Eq> <FvUiName> <EOL>
<FdImage>           ::= "FD" <Eq> <FdUiName> <EOL>
<FdUiName>          ::= {<Word>} {"common"}
<NormalFile>        ::= <PATH> <Word> "." <Word> <EOL>
<SectionData>       ::= <MacroDefinition>*
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <EncapSec>*
                        <LeafSections>*
<PeiAprioriSection> ::= "APRIORI PEI" <MTS>
                        "{" <EOL>
                        [<DefineStatements>]
                        <InfStatements>*
                        <FileStatements>* <TS> "}" <EOL>
<DxeAprioriSection> ::= "APRIORI DXE" <MTS>
                        "{" <EOL>
                        [<DefineStatements>]
                        <InfStatements>*
                        <FileStatements>*
                        <TS> "}" <EOL>
<LeafSections>      ::= {<VerSection>} {<UiSec>} {<FvImgSection>}
                        {<DataSection>} {<DepexExpSec>}
<VerSection>        ::= "SECTION" <MTS> [<VerArgs>] "VERSION" <MTS> <UniArg>
<UiSection>         ::= "SECTION" <MTS> [<FfsAlignment>] "UI" <MTS> <UniArg>
<FvImgSection>      ::= "SECTION" <MTS> [<FfsAlignment>] "FV_IMAGE" <MTS>
                        <FvImgArgs>
<VerArgs>           ::= [<FfsAlignment>] [<Build>]
<Build>             ::= "BUILD_NUM" <Eq> <BuildVal> <MTS>
<BuildVal>          ::= {[a-fA-F0-9]{4}} {"$(BUILD_NUMBER)"}
<UniArg>            ::= <Eq> {<StringData>} {<NormalFile>} <EOL>
<StringData>        ::= {<UnicodeString>} {<QuotedString>}
                        {<NamedMacro>}
<NamedMacro>        ::= if (VerSection): "$(INF_VERSION)" else if (UiSection):
                        {"$(INF_VERSION)"} {"$(MODULE_NAME)"}
<FvImgArgs>         ::= <Eq> <FvUiName>
                        "{" <EOL>
                        <MacroDefinition>*
                        <ExtendedFvEntry>*
                        [<FvAlignment>]
                        <FvAttributes>*
                        [<FileSystemGuid>]
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <InfStatements>*
                        <FileStatements>*
                        <TS> "}" <EOL>
<ExtendedFvEntry>   ::= <TS> "FV_EXT_ENTRY_TYPE" <MTS> <TypeValue>
                        "{" [<EOL>]
                        <TS>
                        {"FILE" <Eq> <BinaryFile>}
                        {"DATA" <Eq> "{" <DataContent> "}"}
                        [<EOL>]
                        <TS> "}" <EOL>
<DataContent>       ::= {<RawData>} {<CFormatGUID>} {<UINT8z>} {<UINT16z>}
                        {<UINT32z>} {<UINT64z>}
<TypeValue>         ::= "TYPE" <Eq> <Hex4> <MTS>
<Afile>             ::= "APPEND" <Eq> <BinaryFile> <EOL>
<BinaryFile>        ::= [<PATH>] <Word> ["." {<bin>} {<dat>}] [<EOL>]
<bin>               ::= {"bin"} {"BIN"} {"Bin}
<dat>               ::= {"dat"} {"DAT"} {"Dat"} {"data"} {"DATA"}
                        {"Data"}
<DataSection>       ::= {<KnownSection>} {<SubTypeGuid>}
<KnownSection>      ::= "SECTION" <MTS> [<FfsAlignment>] <SecData>
<SecData>           ::= <LeafSecType> [<ChkReloc>] <SectionOrFile>
<LeafSecType>       ::= {"COMPAT16"} {"PE32"} {"PIC"} {"TE"} {"RAW"}
                        {"FV_IMAGE"} {"DXE_DEPEX"} {"SMM_DEPEX"}
                        {"UI"} {"PEI_DEPEX"} {"VERSION"}
<SubTypeGuid>       ::= <TS> "SECTION" <MTS> [<FfsAlignment>] <SgData>
<SgData>            ::= "SUBTYPE_GUID" <MTS> <NamedGuidOrPcd> <Eq>
                        <NormalFile> <EOL>
<ChkReloc>          ::= if ((LeafSecType == "PE32"
                        || LeafSecType == "TE")
                        && (MODULE_TYPE == "SEC"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "PEIM")): [<RelocFlags>]
<SectionOrFile>     ::= {<Eq> <NormalFile> <EOL>} {<EncapSec>}
<EncapSec>          ::= "SECTION" <MTS> [<FfsAlignment>] <EncapSection>
<EncapSection>      ::= {<CompressSection>} {<GuidedSection>}
<CompressSection>   ::= "COMPRESS" <MTS> [<CompType>]
                        "{" <EOL>
                        <MacroDefinition>*
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <EncapSec>*
                        <LeafSections>*
                        <TS> "}" <EOL>
<CompType>          ::= {"PI_STD"} {"PI_NONE"} <MTS>
<GuidedSection>     ::= "GUIDED" <NamedGuidOrPcd> [<GuidedOptions>]
                        "{" <EOL>
                        <MacroDefinition>*
                        [<PeiAprioriSection>]
                        [<DxeAprioriSection>]
                        <EncapSec>*
                        <LeafSections>*
                        <TS> "}" <EOL>
<GuidedOptions>     ::= [<GuidAttrPR>] [<GuidAttrASV>] [<GuidHeaderSize>]
<GuidAttrPR>        ::= "PROCESSING_REQUIRED" <Eq> <TrueFalse> <MTS>
<GuidAttrASV>       ::= "AUTH_STATUS_VALID" <Eq> <TrueFalse> <MTS>
<GuidHeaderSize>    ::= "EXTRA_HEADER_SIZE" <Eq> <Number> <MTS>
<FvUiName>          ::= {<Word>} {"common"}
<FvStatements>      ::= "FV" <Eq> <FvNameOrFilename> <EOL>
<FvNameOrFilename>  ::= {<FvUiName>} {<FvFilename>}
<FvFilename>        ::= [<PATH>] <Word> "." "fv"
<FdStatements>      ::= "FD" <Eq> <FdNameOrFilename> <EOL>
<FdNameOrFilename>  ::= {<FdUiName>} {<FdFilename>}
<FdFilename>        ::= [<PATH>] <Word> "." "fd"
<AnyFile>           ::= "FILE" <MTS> "DATA" <Eq> <NormalFile> <EOL>
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
<FileSystemGuid>    ::= "FileSystemGuid" <Eq> <NamedGuidOrPcd>
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
                        elif ( MODULE_TYPE == "UEFI_APPLICATION"
                        || MODULE_TYPE == "UEFI_DRIVER"
                        || MODULE_TYPE == "PEI_CORE"
                        || MODULE_TYPE == "DXE_CORE"
                        || MODULE_TYPE == "SMM_CORE"
                        || MODULE_TYPE == "SEC"):
                        No DEPEX section is permitted
<Depex>             ::= if (MODULE_TYPE == PEIM): <PeiDepexExp> elif
                        (MODULE_TYPE == "DXE_SMM_DRIVER"): <SmmDepexExp>
                        [<DxeDepexExp>] else:
                        <DxeDepexExp>
<PeiDepexExp>       ::= "SECTION" <MTS> [<FfsAlignment>]
                        "PEI_DEPEX_EXP"
                        <Eq> "{" [<EOL>] <PeiDepex> "}" <EOL>
<PeiDepex>          ::= [<BoolStmt> {<EOL>} {<MTS>}]*
                        [<DepInstruct> {<EOL>} {<MTS>}]*
                        ["end"] [<EOL>]
<BoolStmt>          ::= {<Boolean>} {<BoolExpress>}
                        {<GuidCName>} <EOL>
<Boolean>           ::= {"TRUE"} {"FALSE"} {<GuidCName>}
<BoolExpress>       ::= <GuidCName> [<OP> ["NOT"] <GuidCName> ]*
<OP>                ::= <MTS> {"AND"} {"OR"} <MTS>
<DepInstruct>       ::= "push" <Filename>
<DxeDepexExp>       ::= "SECTION" <MTS> [<FfsAlignment>] <DxeExp>
<DxeExp>            ::= "DXE_DEPEX_EXP" <Eq> <MTS>
                        "{" [<EOL>] <DxeDepex>* "}" <EOL>
<DxeDepex>          ::= [<SorStmt> {<EOL>} {<MTS>}]*
                        [<GuidStmt> {<EOL>} {<MTS>}]*
                        [<BoolStmt> {<EOL>} {<MTS>}]*
                        [<DepInstruct> {<EOL>} {<MTS>}]*
                        ["END"] {<EOL>} {<MTS>}]
<SorStmt>           ::= "SOR" <MTS> <BoolStmt>
<GuidStmt>          ::= {"before"} {"after"} <MTS> <Filename>
<SmmDepexExp>       ::= "SECTION" <MTS> [<FfsAlignment>] "SMM_DEPEX_EXP"
                        <Eq> "{" [<EOL>] <DxeDepex> "}" <EOL>
<FmpPayload>        ::= <TS> "FMP_PAYLOAD" <Eq> <UiFmpName> <EOL>
<UiFmpName>         ::= <Word>
<FmpFileStatement>  ::= <TS> "FILE" <Space> "DATA" <Eq> <Filename> <EOL>
```

#### Restrictions

**_Filename_**

For BINARY ONLY content (`UEFI_DRIVER` and `UEFI_APPLICATION` .efi files) the
file names specified in the elements (`FILE` and `SECTION`) of this section
must be relative to the directory identified by the `WORKSPACE` system
environment variable or relative to a path listed in the `PACKAGES_PATH` system
environment variable.

**_TargetArch_**

Only specific architectures are permitted - use of "common" is prohibited.

**_NamedGuidOrPcd_**

When specifying the CAPSULE_GUID value for an FMP Capsule, the GUID value must
be same as 6dcbd5ed-e82d-4c44-bda1-7194199ad92a.

#### Parameters

**_UiCapsuleName_**

Filename that will be used to create an FV file.

**_CreateFile_**

Filename to create instead of using the `UiCapsuleName`.

**_FvBaseAddress_**

The `FvBaseAddress`, if present, must be listed before the `FvAlignment`
element.

The `FvForceRebase` flag, if present, must immediately follow the
`FvBaseAddress`.

**_SUBTYPE_GUID_**

This is short hand notation refering to content that will be placed in a
Section of type: `EFI_SECTION_FREEFORM_SUBTYPE_GUID`. A single

`EFI_SECTION_FREEFORM_SUBTYPE_GUID` section is permitted in an FFS File of type
`EFI_FV_FILETYPE_FREEFORM`

**_Depex_**

Depex sections are prohibited for modules with a `MODULE_TYPE` of
`UEFI_DRIVER`, `UEFI_APPLICATION`, `PEI_CORE`, `DXE_CORE` or `SEC`. modules
with `MODULE_TYPE` of `USER_DEFINED` and all Library instances may or may not
have a `DEPEX` section.

Modules that use `DXE_RUNTIME_DRIVER` as the `MODULE_TYPE` require a `DEPEX`
section if and only if they are pure DXE Runtime drivers - UEFI Runtime Drivers
that use the `DXE_RUNTIME_DRIVER` `MODULE_TYPE` must not have a `DEPEX` section.

If a library instance is required by a module that prohibits depex sections,
the libraries' depex section is ignored. For modules that do require a depex
section, the depex section of all dependent libraries is AND'ed with the depex
section of the module.

**_Expression_**

Refer to the EDK II Expression Syntax Specification for more information.

**_Paths_**

Unless otherwise specified, all file specified paths are relative to the
`WORKSPACE` directory or relative to a directory listed in the `PACKAGES_PATH`.
In some cases, the tools will search well known paths for some files, for
example, for FD filenames, the output will typically be located in the
`$(OUTPUT_DIRECTORY)/ $(TARGET)_$(TAGNAME)/FV` directory.

**_COMPRESS_**

Compression sections that use `PI_STD` compression do not have
`PROCESSING_REQUIRED = TRUE` flag, it is only required for GUIDED sections.

**_User Interface (UI) entries_**

There are three possible methods for specifying a User Interface string. 1)
Specify the string value in the FDF file, 2) specify a plain ASCII text file
that has an extension of ".ui" or 3) specify a Unicode file with an extension
of ".uni" that contains a single Unicode string.

**_Append_**

The APPEND element is used to specify a workspace relative path (or relative to
a directory listed in the PACKAGES_PATH) and file name for a raw binary file.
The order that files will be appended is the order in which they are listed in
the section. Any driver that needs to access these files must have a prior
knowledge of the content - for example, a new payload image - as these files
are not processed by the EDK II tools. They have no section types or any other
kind of identifier that has been defined by UEFI/PI specifications.

#### Related Definitions

**_Target_**

Must match a target identifier in the EDK II _tools_def.txt_ file - the first
field, where fields are separated by the underscore character. Wildcard
characters are not permitted.

**_TagName_**

Must match a tag name field in the EDK II `tools_def.txt` file - second field.
Wildcard characters are not permitted

#### Example

```ini
[Capsule.Fob]
CAPSULE_GUID        = 42857F0A-13F2-4B21-8A23-53D3F714B840
CAPSULE_HEADER_SIZE = 32

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

[Capsule.FmpCapsuleImage]
  # normal header for FMP capsule content
  # special Guid
  CAPSULE_GUID = 6dcbd5ed-e82d-4c44-bda1-7194199ad92a
  # normal header
  CAPSULE_FLAGS = PersistAcrossReset, InitiateReset
  # normal header
  CAPSULE_HEADER_SIZE = 0x20
  # The following identifies this as an FMP capsule header
  CAPSULE_HEADER_INIT_VERSION = 0x1

  FILE DATA   = Driver1.efi
  FILE DATA   = Driver2.efi  # zero or more
  FMP_PAYLOAD = Payload1     # zero or more
```
