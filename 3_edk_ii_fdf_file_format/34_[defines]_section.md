<!--- @file
  3.4 [Defines] Section

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

## 3.4 [Defines] Section

This is an optional section. This section, if present, must be the first
section following comment blocks at the beginning of the file.

#### Summary

This section describes the defines section content in the FDF files. This file
can be created by a developer and is an input to the EDK II build tool parsing
utilities. Elements may appear in any order within this section.

The code for this version of the FDF specification is "0x0001001A" and new
versions of this specification must increment the minor (001A) portion of the
specification code for backward compatible changes, and increment the major
number for non-backward compatible specification changes.

This revision of the specification adds FMP Capsule support. Any FDF file that
uses this feature must use the 0x0001001A FDF_SPECIFICATION value. Older FDF
files that do not use this feature do not need to update the value.

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<Defines>    ::= "[Defines]" <EOL>
                 [<TS> "FDF_SPECIFICATION" <Eq> <SpecVer> <EOL>]
                 [<TS> "FDF_VERSION" <Eq> <DecimalVersion> <EOL>] <DefStmts>*
<DefStmts>   ::= {<MacroDefinition>} {<SetStmts>} {<IncludeStatement>}
<UiNameType> ::= <AsciiString>
<SpecVer>    ::= {<HexVersion>} {(0-9)+ "." (0-9)+}
<SetStmts>   ::= <TS> "SET" <MTS> <PcdName> <Eq> [<VALUE>] <EOL>
<VALUE>      ::= {<Number>} {<Boolean>} {<GUID>} {<CArray>}
                 {<CString>} {<UnicodeString>} {<Expression>}
```

#### Parameters

**_Expression_**

Refer to the EDK II Expression Syntax Specification for more information.

**_FDF_VERSION_**

The version number for this flash definition; the value is not used by build
tools, but the version element is provided for user tracking capabilities that
may be used by other user interface tools.

**_FDF_SPECIFICATION_**

For this specification, the version value is 0x0001001A. Tools that process
this version of the FDF file can successfully process earlier versions of the
FDF files (this is a backward compatible update). If an FDF file with an
earlier version of the `FDF_SPECIFICATION` is modified to use the FMP Payload
section and FMP Capsule definitions, the version value should be updated to
0x0001001A. There is no requirement to change existing entries if no other
content changes. This value may also be specified as decimal value, such as
1.26.

**_PcdNames_**

PCDs defined in this section take precedence over PCD values specified in other
meta-data files. Note that PCDs defined via the SET statements in sub-sections
of the document can override the values set here as well as in other EDK II
meta-data files.

#### Example

```ini
[Defines]
  FDF_SPECIFICATION                          = 0x0001001A
  DEFINE BIG_STUFF                           = False
  SET gEfiMyPlatformTokenSpaceGuid.MyUsbFlag = True
```
