<!--- @file
  3.10 [VTF] Section

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

## 3.10 [VTF] Section

This describes the optional `[VTF]` section tag found in FDF files.

#### Summary

If VTF files will be created, they will be created in the
`$(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV` directory using the values from
the individual instance of the build tools. (Build tools get these values after
parsing DSC, INF, `target.txt`, `tools_def.txt` files and command line options.)

The following sequence describes each component:

```
Name = Region,
       Type,
       Version,
       CheckSum_Flag,
       Path_of_Binary_File,
       Path_of_SYM_File,
       Preferred_Size;
```

Where,

**_Name:_**

Name of the component

**_Region:_**

Location in the firmware. Valid locations are:

```
PH - Protected Block region, merged towards the higher address
PL - Protected Block region, merged towards the lower address
H - Flashable region, merged towards the higher address
L - Flashable region, merged towards the lower address
F - First VTF File
N - Not in VTF File
S - Second VTF File
```

**_Type:_**

Component Type. Predefined values are:

```
0x00 : FIT Header entry
0x01 : PAL_B
0x02 - 0x0E : Reserved
0x0F : PAL_A
0x10 - 0x7E : OEM-defined
0x7F : Unused
```

**_Version:_**

Component Version number (XX.YY)

```
- major version number (decimal number, range of 0 to 99)
- minor version number (decimal number, range of 0 to 99)
```

**_Checksum_Flag:_**

Checksum Flag (equivalent to CV bit)

```
0 - Checksum Byte always equals 0, CV=0
1 - calculate Checksum Byte, CV=1
```

**_Checksum:_**

Byte sum of component + Checksum Byte = modulus 0x100

**_Path_of_Binary_File:_**

Path of the Binary file of the component

**_Path_of_SYM_File:_**

Path of the .SYM symbol file of the component

**_Preferred_Size:_**

User preferred component size, overrides actual component file size. Valid is
equal or greater than the actual file size.

#### Prototype

```c
<VTF>                 ::= "[VTF" <Modifiers> "]" <EOL>
                          [<OptionStatement>]
                          <ComponentStatements>*
<Modifiers>           ::= "." <arch> "." <UiName> [<ArchList>]
<ArchList>            ::= "," <Arch>
<Arch>                ::= {"IA32"} {"X64"} {"IPF"}
<UiName>              ::= <Word>
<OptionStatement>     ::= <TS> "IA32_RST_BIN" <Eq> <Filename> <EOL>
<ComponentStatements> ::= <TS> "COMP_NAME" <Eq> <WORD> <EOL>
                          <TS> "COMP_LOC" <Eq> <Location> <EOL>
                          <TS> "COMP_TYPE" <Eq> <CompType> <EOL>
                          <TS> "COMP_VER" <Eq> <Version> <EOL>
                          <TS> "COMP_CS" <Eq> {"1"} {"0"} <EOL>
                          <TS> "COMP_BIN" <Eq> <BinFile> <EOL>
                          <TS> "COMP_SYM" <Eq> <SymFile> <EOL> <TS> "COMP_SIZE"
                          <Eq> <Size> <EOL>
<Location>            ::= {<FvUiName>} {"NONE"} {"None"} {"none"} [<FS>
                          <Region>]
<Region>              ::= {"F"} {"N"} {"S"} {"H"} {"L"} {"PH"} {"PL"}
<CompType>            ::= {"FIT"} {"PAL_B"} {"PAL_A"} {"OEM"} {<Byte>}
<Byte>                ::= "0x" <HexDigit>? <HexDigit>
<Version>             ::= {"-"} {<Major> "." <Minor>} {<BcdHex>}
<Major>               ::= [(0-9)](0-9)
<Minor>               ::= [(0-9)](0-9)
<BcdHex>              ::= "0x" <Major> (0-9) (0-9)
<BinFile>             ::= {"-"} {[<PATH>] <Filename>}
<SymFile>             ::= {"-"} {[<PATH>] <Filename>}
<Size>                ::= {"-"} {<Integer>} {<HexNumber>}
```

#### Restrictions

**_FName_**

All file specified paths are relative to the WORKSPACE directory (or a directory

listed in the PACKAGES_PATH). In some cases, the tools will search well known
paths for some files, for example, for FD filenames, the output will typically
be located in the $(OUTPUT_DIRECTORY)/$(TARGET)_$(TAGNAME)/FV directory.

#### Parameters

**_<BinFile> - Filename_**

If a filename is given, the file must have an extension for a binary type file,
such as ".bin" or ".BIN". Filenames are case sensitive, so the correct case
must be used for all filenames.

**_<SymFile> - Filename_**

If a filename is given, the file must have an extension for a symbol type file,
such as ".sym" or ".SYM". Filenames are case sensitive, so the correct case
must be used for all filenames.

#### Example

```
[VTF.IPF.MyBsf]
IA32_RST_BIN = IA32_RST.BIN

COMP_NAME = PAL_A          # Component Name
COMP_LOC  = FvRecovery | F # In the first VTF file
COMP_TYPE = 0xF            # Component Type (PAL_A=0x0F, defined in SAL Spec.)
COMP_VER  = 7.01           # Version will come from header of PAL_A binary
COMP_CS   = 1              # Checksum_Validity (CV bit)
COMP_BIN  = PAL_A_GEN.BIN  # Path of binary
COMP_SYM  = PAL_A_GEN.SYM  # Path of SYM symbol
COMP_SIZE = -              # Preferred component size in bytes

COMP_NAME = PAL_B     # Component Name
COMP_LOC  = F         # In the first VTF file
COMP_TYPE = 0x01      # Component Type (PAL_A=0x0F, defined in SAL Spec.)
COMP_VER  = -         # Version will come from header of PAL_A binary
COMP_CS   = 1         # Checksum_Validity (CV bit)
COMP_BIN  = PAL_B.BIN # Path of binary
COMP_SYM  = PAL_B.Sym # Path of SYM symbol
COMP_SIZE = -         # Preferred component size in bytes
```
