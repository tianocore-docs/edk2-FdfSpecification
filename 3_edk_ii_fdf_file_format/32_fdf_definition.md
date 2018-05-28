<!--- @file
  3.2 FDF Definition

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

## 3.2 FDF Definition

The FDF definitions define the final properties for a flash image - PCD
settings in this file override any other PCD settings that may have been set in
the platform description (DSC) file. The `[Defines]` section, when specified,
must appear before any other section except the header. (The header, when
specified, is always the first section of an FDF file.) The remaining sections
may be specified in any order within the FDF file.

#### Summary

The EDK II Flash Description (FDF) file has the following format (using the
EBNF).

```c
<EDK_II_FDF> ::= [<Header>]
                 [<Defines>]
                 <FD>*
                 <FV>*
                 <Capsule>*
                 <FmpPayload>*
                 <VTF>*
                 <Rules>*
                 <OptionRom>*
                 <UserExtensions>*
```

**********
**Note:** Assignments set as command-line arguments to the parsing tools take
precedence over all assignments defined in the FDF file. If a variable/value
assignment is specified on the build tool's command-line, that value will
override any variable/value assignment defined in the FDF file.
**********
**Note:** Conditional statements may be used anywhere within the FDF file, with
the ability to group any item within a section as well as entire sections.
**********

### 3.2.1 Common Definitions

#### Summary

The following are common definitions used by multiple section types.

#### Prototype

```c
<Word>                 ::= (a-zA-Z0-9_)(a-zA-Z0-9_-.)* Alphanumeric characters
                           with optional period ".", dash "-" and/or underscore
                           "_" characters. A period character may not be
                           followed by another period character.
                           No white space characters are permitted.
<SimpleWord>           ::= (a-zA-Z0-9)(a-zA-Z0-9_-)* A word that cannot contain
                           a period character.
<ToolWord>             ::= (A-Z)(a-zA-Z0-9)* Alphanumeric characters. white
                           space characters are not permitted.
<FileSep>              ::= "/"
<Extension>            ::= (a-zA-Z0-9_-)+ One or more alphanumeric characters.
<File>                 ::= <Word> ["." <Extension>]
<PATH>                 ::= [<MACROVAL> <FileSep>] <RelativePath>
<RelativePath>         ::= <DirName> [<FileSep> <DirName>]*
<DirName>              ::= {<Word>} {<MACROVAL>}
<FullFilename>         ::= <PATH> <FileSep> <File>
<Filename>             ::= [<PATH> <FileSep>] <File>
<Chars>                ::= (a-zA-Z0-9_)
<Digit>                ::= (0-9)
<NonDigit>             ::= (a-zA-Z_)
<Identifier>           ::= <NonDigit> <Chars>*
<CName>                ::= <Identifier> # A valid C variable name.
<AsciiChars>           ::= (0x21 - 0x7E)
<CChars>               ::= [{0x21} {(0x23 - 0x5B)} {(0x5D - 0x7E)}
                           {<EscapeSequence>}]*
<DblQuote>             ::= 0x22
<EscapeSequence>       ::= "\" {"n"} {"t"} {"f"} {"r"} {"b"} {"0"}
                           {"\"} {<DblQuote>}
<TabSpace>             ::= {<Tab>} {<Space>}
<TS>                   ::= <TabSpace>*
<MTS>                  ::= <TabSpace>+
<Tab>                  ::= 0x09
<Space>                ::= 0x20
<CR>                   ::= 0x0D
<LF>                   ::= 0x0A
<CRLF>                 ::= <CR> <LF>
<WhiteSpace>           ::= {<TS>} {<CR>} {<LF>} {<CRLF>}
<WS>                   ::= <WhiteSpace>*
<Eq>                   ::= <TS> "=" <TS>
<FieldSeparator>       ::= "|"
<FS>                   ::= {<TS>} <FieldSeparator> <TS>
<Wildcard>             ::= "*"
<CommaSpace>           ::= "," <Space>*
<Cs>                   ::= "," <Space>*
<AsciiString>          ::= [ <TS>* <AsciiChars>* ]*
<EmptyString>          ::= <DblQuote> <DblQuote>
<CFlags>               ::= <AsciiString>
<PrintChars>           ::= {<TS>} {<CChars>}
<QuotedString>         ::= <DblQuote> <PrintChars>* <DblQuote>
<CString>              ::= ["L"] <QuotedString>
<NormalizedString>     ::= <DblQuote> [{<Word>} {<Space>}]+ <DblQuote>
<GlobalComment>        ::= <WS> "#" [<AsciiString>] <EOL>+
<Comment>              ::= "#" <AsciiString> <EOL>+
<UnicodeString>        ::= "L" <QuotedString>
<HexDigit>             ::= (a-fA-F0-9)
<HexByte>              ::= {"0x"} {"0X"} [<HexDigit>] <HexDigit>
<HexNumber>            ::= {"0x"} {"0X"} <HexDigit>+
<HexVersion>           ::= "0x" [0]* <Major> <Minor>
<Major>                ::= <HexDigit>? <HexDigit>? <HexDigit>?
                           <HexDigit>
<Minor>                ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<DecimalVersion>       ::= {"0"} {(1-9) [(0-9)]*} ["." (0-9)+]
<VersionVal>           ::= {<HexVersion>} {(0-9)+ "." (0-99)}
<GUID>                 ::= {<RegistryFormatGUID>} {<CFormatGUID>}
<RegistryFormatGUID>   ::= <RHex8> "-" <RHex4> "-" <RHex4> "-" <RHex4> "-"
                           <RHex12>
<RHex4>                ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<RHex8>                ::= <RHex4> <RHex4>
<RHex12>               ::= <RHex4> <RHex4> <RHex4>
<RawH2>                ::= <HexDigit>? <HexDigit>
<RawH4>                ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>
<OptRawH4>             ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>?
<Hex2>                 ::= {"0x"} {"0X"} <RawH2>
<Hex4>                 ::= {"0x"} {"0X"} <RawH4>
<Hex8>                 ::= {"0x"} {"0X"} <OptRawH4> <RawH4>
<Hex12>                ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <RawH4>
<Hex16>                ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4>
                           <OptRawH4> <RawH4>
<CFormatGUID>          ::= "{" <Hex8> <CommaSpace> <Hex4> <CommaSpace> <Hex4>
                           <CommaSpace> "{"
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                           <Hex2> <CommaSpace> <Hex2> "}" "}"
<CArray>               ::= "{" {<NList>} {<CArray>} "}"
<NList>                ::= <HexByte> [<CommaSpace> <HexByte>]*
<RawData>              ::= <TS> <HexByte>
                           [ <Cs> <HexByte> [<EOL> <TS>] ]*
<Integer>              ::= {(0-9)} {(1-9)(0-9)+}
<Number>               ::= {<Integer>} {<HexNumber>}
<HexNz>                ::= (\x1 - \xFFFFFFFFFFFFFFFF)
<NumNz>                ::= (1-18446744073709551615)
<GZ>                   ::= {<NumNz>} {<HexNz>}
<TRUE>                 ::= {"TRUE"} {"true"} {"True"} {"0x1"}
                           {"0x01"} {"1"}
<FALSE>                ::= {"FALSE"} {"false"} {"False"} {"0x0"} {"0x00"} {"0"}
<BoolType>             ::= {<TRUE>} {<FALSE>}
<MACRO>                ::= (A-Z)(A-Z0-9_)*
<MACROVAL>             ::= "$(" <MACRO> ")"
<PcdName>              ::= <TokenSpaceGuidCName> "." <PcdCName>
<PcdCName>             ::= <CName>
<TokenSpaceGuidCName>  ::= <CName>
<PCDVAL>               ::= "PCD(" <PcdName> ")"
<UINT8>                ::= {"0x"} {"0X"} (\x0 - \xFF)
<UINT16>               ::= "0x"} {"0X"} (\x0 - \xFFFF)
<UINT32>               ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFF)
<UINT64>               ::= {"0x"} {"0X"} (\x0 - \xFFFFFFFFFFFFFFFF)
<UINT8z>               ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
<UINT16z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit>
<UINT32z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit>
<UINT64z>              ::= {"0x"} {"0X"} <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit> <HexDigit> <HexDigit>
                           <HexDigit> <HexDigit>
<ShortNum>             ::= (0-255)
<IntNum>               ::= (0-65535)
<LongNum>              ::= (0-4294967295)
<LongLongNum>          ::= (0-18446744073709551615)
<NumValUint8>          ::= {<ShortNum>} {<UINT8>}
<NumValUint16>         ::= {<IntNum>} {<UINT16>}
<NumValUint32>         ::= {<LongNum>} {<UINT32>}
<NumValUint64>         ::= {<LongLongNum>} {<UINT64>}
<ModuleType>           ::= {"BASE"} {"SEC"} {"PEI_CORE"} {"PEIM"}
                           {"DXE_CORE"} {"DXE_DRIVER"} {"SMM_CORE"}
                           {"DXE_RUNTIME_DRIVER"} {"DXE_SAL_DRIVER"}
                           {"DXE_SMM_DRIVER"} {"UEFI_DRIVER"}
                           {"UEFI_APPLICATION"} {"USER_DEFINED"}
<ModuleTypeList>       ::= <ModuleType> [" " <ModuleType>]*
<IdentifierName>       ::= <TS> {<MACROVAL>} {<PcdName>} <TS>
<MembershipExpression> ::= {<TargetExpress>} {<ArchExpress>} {<ToolExpress>}
<NotOp>                ::= <UnaryOperator> <MTS>
<InOp>                 ::= <MTS> [<NotOp>] {"IN"} {"in"} <MTS>
<TargetExress>         ::= (A-Z) (A-Z0-9)* <InOp> "$(TARGET)"
<ArchExpress>          ::= <DblQuote> <Arch> <DblQuote> <InOp> "$(ARCH)"
<Arch>                 ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {<OA>}
<ToolExpress>          ::= (A-Z) (a-zA-Z0-9)* <InOp> "$(TOOL_CHAIN_TAG)"
<Boolean>              ::= {<BoolType>} {<Expression>}
<EOL>                  ::= <TS> 0x0D 0x0A
<OA>                   ::= (a-zA-Z)(a-zA-Z0-9)*
<arch>                 ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {<OA>}
                           {"common"}
<FvAlignmentValues>    ::= {"1"} {"2"} {"4"} {"8"} {"16"} {"32"}
                           {"64"} {"128"}{"256"} {"512"} {"1K"} {"2K"}
                           {"4K"} {"8K"} {"16K"} {"32K"} {"64K"}
                           {"128K"} {"256K"} {"512K"} {"1M"} {"2M"} {"4M"}
                           {"8M"} {"16M"} {"32M"} {"64M"}
                           {"128M"} {"256M"} {"512M"} {"1G"} {"2G"}
<FfsAlignmentValues>   ::= {"Auto"} {"8"} {"16"} {"32"} {"64"} {"128"}
                           {"512"} {"1K"} {"4K"} {"32K"} {"64K"} {"128K"}
                           {"256K"} {"512K"} {"1M"} {"2M"} {"4M"} {"8M"}
                           {"16M"}
```

**********
**Note:** When using the characters "|" or "||" in an expression, the
expression must be encapsulated in open "(" and close ")" parenthesis.
**********
**Note:** Comments may appear anywhere within a FDF file, provided they follow
the rules that a comment may not be enclosed within Section headers, and that
in line comments must appear at the end of a statement.
**********

### Parameter Definitions

**_Expression_**

Refer to the EDK II Expression Syntax Specification for more information.

**""UnicodeString**

When the `<UnicodeString>` element (these characters are string literals as
defined by the C99 specification: L"string", not actual Unicode characters) is
included in a value, the build tools may be required to expand the ASCII string
between the quotation marks into a valid UCS-2LE encoded string. The build
tools parser must treat all content between the field separators (excluding
white space characters around the field separators) as ASCII literal content
when generating the AutoGen.c and AutoGen.h files.

**_Comments_**

Strings that appear in comments may be ignored by the build tools. An ASCII
string matching the format of the ASCII string defined by `<UnicodeString>`
(L"Foo" for example,) that appears in a comment must never be expanded by any
tool.

**_CFlags_**

CFlags refers to a string of valid arguments appended to the command line of
any third party or provided tool. It is not limited to just a compiler
executable tool. MACRO values that appear in quoted strings in CFlags content
must not be expanded by parsing tools.

**_OA_**

Other Architecture - One or more user defined target architectures, such as ARM
or PPC. The architectures listed here must have a corresponding entry in the
EDK II meta-data file, _Conf/tools_def.txt_. Only `IA32`, `X64`, `IPF` and
`EBC` are routinely validated.

**_FileSep_**

FileSep refers to either the back slash "\" or forward slash "/" characters
that are used to separate directory names. All EDK II FDF files must use the
"/" forward slash character when specifying the directory portion of a
filename. Microsoft operating systems, that normally use a back slash character
for separating directory names, will interpret the forward slash character
correctly.

**_TokenSpaceGuidCName_**

A word that is a valid C variable that specifies the name space for a
particular PCD.

**_PcdCName_**

A word that is a valid C variable that specifies the name of the token number
which a member of the name space specified by the TokenSpaceGuidCName.

**_CArray_**

All C data arrays used in PCD value fields must be byte arrays. The C format
GUID style is a special case that is permitted in some fields that use the
`<CArray>` nomenclature.

**_EOL_**

The DOS End Of Line: "0x0D 0x0A" character sequence must be used for all EDK
II meta-data files. All *Nix based tools can properly process the DOS EOL
characters. Microsoft based tools cannot process the *Nix style EOL characters.

### 3.2.2 MACRO Statements

Use of MACRO statements is optional.

#### Summary

Macro statements are characterize by a `DEFINE` line or may be defined on a
command line of a parsing tool.

Define statements are processed according to the following precedence.

Highest Priority

1. Command-line option -D MACRO=Value
2. Most Recent in file
3. Macros defined in the FDF file's `[Defines]` section
4. Macros defined in the DSC file's `[Defines]` section Lowest Priority

If the Macro statement is within the `[Defines]` section, then the Macro is
common to the entire file, with local definitions taking precedence (if the
same MACRO name is used in subsequent sections, then the MACRO value is local
to only that section.) Macro statements may not be referenced before they are
defined.

Macros may be inherited from the DSC file specifying this FDF file.

All content for a macro statement must appear on a single line.

If the tools encounter a `MacroVal`, as in $(MACRO), that is not defined, the
build tools must break.

#### Prototype

```c
<MacroDefinition> ::= <TS> "DEFINE" <MTS> <MACRO> [<Eq> [<VALUE>]] <EOL>
<VALUE>           ::= {<Number>} {<BoolType>} {<CFormatGUID>} {<PATH>}
                      {<CString>} {<UnicodeString>} {<CArray>}
                      {<Expression>} {<CFlags>} {<Filename>}
```

#### Restrictions

**_System Environment Variables_**

System environment variable may not be re-defined in this file. System
environment variables cannot be overridden by the build system tools.

#### Parameters

**_Expression_**

C-style expression using C relational, equality and logical numeric and bitwise
operators and/or arithmetic and bitwise operators that evaluate to a value (for
PCDs, the value must match the Datum Type of the PCD). Precedence and
associativity follow C standards. Along with absolute values, macro names and
PCDs may be used within an expression. For both macro names and PCDs, the
element must be previously defined before it can be used.

**_VALUE_**

The `<EQ> <VALUE>` is optional, and if not included, uses a default of TRUE.

**********
**Note:** Some MACRO and PCD values may be defined in the Platform DSC file.
**********

#### Examples:

```
DEFINE SECCORE    = MyPlatform/SecCore
DEFINE GEN_SKU    = MyPlatform/GenPei
DEFINE SKU1       = MyPlatform/Sku1/Pei
DEFINE FLASH_SIZE = 0x00280000
DEFINE MY_MACRO
```

#### EDK_GLOBAL

The macro names defined using the `EDK_GLOBAL` statement in the DSC file may be
used in paths, value fields and conditional statements in this file. The
`EDK_GLOBAL` statement itself, cannot be specified in this file.

### 3.2.3 Conditional Directive Blocks

Use of conditional directive blocks is optional.

#### Summary

Conditional statements may appear anywhere within the file. Conditional
directive blocks can be nested. Conditional directive processing must emulate a
C pre-processor.

* All conditional directives can use MACRO, FixedAtBuild or FeatureFlag PCD
  values, which must evaluate to either '`True`' or '`False`.'

* Directives must be the only statement on a line.

* String evaluations are permitted, and are case sensitive; the two string
  values must be an exact match to evaluate to '`True`'.

* The expression immediately following the '`!if`' statement controls whether
  the content after the line is processed or not. `TRUE` is any non-zero and/or
  non-NULL value other than zero.

* Each '`!if`' within the source code must be matched with a closing '`!endif`'.

* Zero or more `!elseif` statements are permitted; only one `!else` statement
  is permitted.

* Conditional directive blocks may be nested.

* Directives can be used to encapsulate entire sections or elements within a
  single section, such that they do not break the integrity of the section
  definitions.

* Directives are in-fix expressions that are evaluated left to right; content
  within parenthesis is evaluated before the outer statements are evaluated.
  Use of parenthesis is recommended to remove ambiguity.

* The values of the FixedAtBuild and FeatureFlag PCDs used in the conditional
  statements must be set in the `[PcdsFixedAtBuild]` or `[PcdsFeatureFlag]`
  section(s) of the DSC file or included in `SET` statements.

* Default values from the DEC file are not permitted. Values used in directive
  statement in the FDF files must be define in either the DSC file or the FDF
  file.

Conditional directives may appear before a Macro, FixedAtBuild or FeatureFlag
PCD has been defined. Therefore, the reference build tools may be required to
perform two passes on this file to resolve all directive statements:

1. Obtain the values of the Macros, FixedAtBuild or FeatureFlag PCDs used for
   theconditional directives

2. Evaluate the conditional statements for inclusion in the build.

If the value of a FixedAtBuild or FeatureFlag PCD cannot be determined, the
build will break.

If the value of a FixedAtBuild or FeatureFlag PCD used in a conditional
directive cannot be determined during the first pass, the build should break.
Macros, FixedAtBuild and FeatureFlag PCDs used in conditional statements in the
first pass must not be located within conditional directives. It is permissible
to have a Macro that is undefined after the first pass. It is permissible to
have macros that are undefined used in !ifdef and `!ifndef` statements.
FixedAtBuild or FeatureFlag PCDs in the first pass must not be located within a
conditional directive.

Macro and PCD values may be inherited from the DSC file.

**********
**Note:** PCDs, used in conditional directives, must be defined and the value
set in either the FDF or DSC file in order to be used in a conditional
statement; values from INF or DEC files are not permitted.
**********

#### Prototype

```c
<Conditional>         ::= <IfStatement>
                          <ElseIfConditional>*
                          [<ElseConditional>]
                          <TS> "!endif" <EOL>
<IfStatement>         ::= {<TS> "!if" <MTS> <ExpressionStatement>}
                          {<TS> "!ifdef" <MTS> <MACRO> <EOL>}
                          {<TS> "!ifndef" <MTS> <MACRO> <EOL>}
                          <Statements>*
<Statements>          ::= {<Statements>} {<Conditional>}
<ElseIfConditional>   ::= <TS> "!elseif" <MTS> <ExpressionStatement>
                          <EOL>
                          <Statements>*
<ElseConditional>     ::= <TS> "!else" <EOL> <Statements>*
<ExpressionStatement> ::= <Expression> <EOL>
```

#### Restrictions

**_MACRO and PCD Values_**

When a MACRO is used in conditional directives `!if` or `!elseif`, the
`<MACROVAL>` - $(MACRO) - format is used. When a PCD is used in a conditional
directive (or in an expression) the `<PCDVAL>` - $(PcdName) - format is used.

**_Number values_**

For Numeric expressions, numbers must not be encapsulated by double quotation
marks

**_Strings_**

Strings in `<PCDVAL>` elements must be `NULL` terminated. The `NULL` character
is not part of the string that is tested. All other string comparisons do not
include the double quotation marks encapsulating the string. If the string is
"myapple", the only characters that would be tested are myapple. When using
strings in the expression statements, there must be a comparison operator.

#### Parameters

**_Statements_**

Any valid section, section statement or set of statements defined in this
specification may be within the scope of the conditional statements. The
encapsulated statements must not break the integrity of the file. All
statements within the encapsulation must end with `<EOL>`.

**_MACRO Usage in Expression Statements_**

If a MACRO is used in expression statements, the MACRO must be encapsulated
between "$(" and ")" character sets (matching C format). String comparisons
are case sensitive and must exactly equal, character for character - leading
and trailing white space characters included.

**_PcdFeatureFlag_**

The FeatureFlag PCD is a boolean PCD that is set to either `True` (1) or
`False` (0). The PCD datum type for a FeatureFlag PCD is always `BOOLEAN`. It
may be used in a logical expression.

**_FixedPcdName_**

A FixedAtBuild PCD will have a set value at build time, and that cannot be
modified in the binary image, nor modified at runtime. For directives, the PCD
datum type is limited to `UINT8`, `UINT16`, `UINT32`, `UINT64`, `UINTN` and
`BOOLEAN`. Using a FixedAtBuild PCD that has a datum type of `VOID`* is limited
to text-based comparisons in directives. Using a PCD that has a value of a byte
array is not permitted. FixedAtBuild PCDs may be used in a logical expression.

**_Numeric Expression_**

This is a test of numbers, using relational or equality operators, that
evaluates to `TRUE` or `FALSE`

**_Logical Expression_**

This is a test where the expression, MACRO value or PCD value (include
`<MACROVAL>` or `<PCDVAL>` used in an expression) must evaluate to either
`TRUE` (1) or `FALSE` (0), no operators are required, however logical
operators, as well full expressions can be used. (expressions that do not
evaluate to `TRUE` or `FALSE` must break the build).

**_String Expressions_**

The strings must be exactly identical in order to match. Literal strings must
be encapsulated by double quotation marks. There must be a comparison operator
between two strings (using a string without an operator is not permitted). Also
permitted are the membership expressions, for architectures, targets and tool
chain tag names.

**_All Expression_**

C-style expression using C relational, equality and logical numeric and bitwise
operators that evaluate to either `TRUE` (1) or `FALSE` (0). Values other than
zero or one are invalid. Precedence and associativity follow C standards. Along
with absolute values, macro names and PCDs may be used within an expression.
For both macro names and PCDs, the element must be previously defined before it
can be used. A new operator, "in" is also permitted for testing membership of
an item in a list of one or more items.

#### Example

```ini
!if $(MyPlatformTspGuid.IPF_VERSION_1) && NOT $(MyPlatformTspGuid.IPF_VERSION_2)
  [VTF.IPF.MyBsf]
    !ifdef IA32RESET
      # IPF_VERSION is 1 and IA32RESET defined
      IA32_RST_BIN           = IA32_RST.BIN
    !endif
    COMP_NAME = PAL_A
    COMP_LOC  = MyVtfVF | F
    COMP_TYPE = 0xF
    COMP_VER  = 7.01
    COMP_CS   = 1
    !if ($(PROCESSOR_NAME) == "M1")
      COMP_BIN = M1PalCode/PAL_A_M1.BIN
      COMP_SYM = M1PalCode/PAL_A_M1.SYM
    !elseif ($(PROCESSOR_NAME) == "M2")
      COMP_BIN = M2PalCode/PAL_A_M2.BIN
      COMP_SYM = M2PalCode/PAL_A_M2.SYM
    !else
      COMP_BIN = GenPal/PAL_A_GEN.bin
      COMP_SYM = GenPal/PAL_A_GEN.sym
    !endif
    COMP_SIZE = -
!elseif $(MyPlatformTspGuid.IPF_VERSION_2)
  [VTF.IPF.MyBsf]
    !ifdef IA32RESET
      IA32_RST_BIN = IA32_RST.BIN
    !endif
    COMP_NAME = PAL_A
    COMP_LOC  = MyVtfFv | F
    COMP_TYPE = 0xF
    COMP_VER  = 7.01
    COMP_CS   = 1
    COMP_BIN  = GenPal/PAL_A_GEN.bin
    COMP_SYM  = GenPal/PAL_A_GEN.sym
    COMP_SIZE = -
    COMP_NAME = PAL_B
    COMP_LOC  = MyVtfFv | S
    COMP_TYPE = 0x01
    COMP_VER  = -
    COMP_CS   = 1
    COMP_BIN  = GenPal/PAL_B_GEN.bin
    COMP_SYM  = GenPal/PAL_B_GEN.sym
    COMP_SIZE = -
!else
  [VTF.X64.MyVtf]
      IA32_RST_BIN = IA32_RST.BIN
!endif
!ifndef MY_MACRO
DEFINE MY_MACRO
!endif
```

### 3.2.4 !include Statements

Use of this statement is optional.

#### Summary

Defines the `!include` statement in FDF files. This statement is used to
include, at the statement's line, a file which is processed at that point, as
though the text of the included file was actually in the FDF file. Statements
in the `!include` file(s) are additions to the FDF file, and do not replace
information in the FDF file. The included file's content must match the content
of the section that the `!include` statement resides, or it may contain
completely new sections of the same section type. If the included file contains
new sections, then the section being processed in the Platform FDF file is
considered to have been terminated.

If the `<Filename>` contains "$" characters, then macros defined in the DSC
file, FDF file, and the system environment variables, `$(WORKSPACE)`,
`$(EDK_SOURCE)`, `$(EFI_SOURCE)`, and `$(ECP_SOURCE)` are substituted into
`<Filename>`.

The tools look for `<Filename>` relative to the directory the FDF file resides.
If the file is not found, and the directory containing this FDF file is not the
same directory as the directory containing the DSC file, the tools must attempt
to locate the file relaitive to the directory that contains the DSC file.

If none of these methods find the file, and a directory separator is in
`<Filename>`, the tools attempt to find the file in a WORKSPACE (or a directory
listed in the PACKAGES_PATH) relative path. If the file cannot be found, the
build system must exit with an appropriate error message.

#### Prototype

`<IncludeStatement> ::= <TS> "!include" <MTS> <Filename> <EOL>`

###### Example (EDK II FDF)

```
!include myPlatform/NvRamDefs.txt
!include myFeatures.mak
!include $(WORKSPACE)/PackageDir/Features.dsc
!include $(MACRO1)/AnotherDir/$(MACRO2)/Features.dsc
```

### 3.2.5 !error Statements

Use of this statement is optional.

#### Summary
This section defines the `!error` statement in EDK II FDF files.
This statement is used to cause build tool to stop at the location where the
statement is encountered and error message following the `!error` statement
is output as a message.

#### Prototype

`<ErrorStatement> ::= <TS> "!error" <MTS> <ErrorMessage> <EOL>`
`<ErrorMessage>   ::= <AsciiString>`

#### Parameters

**_ErrorMessage_**

It should in the same line with `!error` statement, and it can consist of several
words not necessarily in quotes.

#### Example (EDK II FDF)

```
!error "unsupported feature!"
```