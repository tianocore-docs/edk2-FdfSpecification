<!--- @file
  3.11 PCI OptionRom Section

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

## 3.11 PCI OptionRom Section

This is an optional section.

#### Summary

This section is used to specify the content of a PCI Option ROM container. A
PCI Option ROM image may contain zero or more PCI ROM image files - binary
only, and zero or more UEFI driver images, specified by either binary or INF
files, that are to be packaged into a single Option ROM image. Additionally,
support for a single EFI driver with both native (IA32, X64, IPF, etc). and EBC
images in the same PCI Option ROM container is provided.

Conditional statements may be used anywhere within this section.

#### Prototype

```c
<OptionRom>    ::= "[OptionRom" "." <DriverName> "]" <EOL> <Components>*
<DriverName>   ::= (a-zA-Z)(a-zA-Z0-9)*
<Components>   ::= {<InfComponent>} {<Binary>}
<InfComponent> ::= <TS> "INF" <MTS> <UseArch> <InfFile>
                   [<Overrides>] <EOL>
<UseArch>      ::= "USE" <Eq> <TargetArch> <MTS>
<TargetArch>   ::= <arch>
<InfFile>      ::= [<PATH>] <Word> ".inf"
<Overrides>    ::= <MTS> "{" <EOL>
                   [<TS> "PCI_VENDOR_ID" <Eq> <UINT16> <EOL>]
                   [<TS> "PCI_CLASS_CODE" <Eq> <UINT8> <EOL>]
                   [<TS> "PCI_DEVICE_ID" <Eq> <UINT16> <EOL>]
                   [<TS> "PCI_REVISION" <Eq> <UINT8> <EOL>]
                   [<TS> "PCI_COMPRESS" <Eq> <TrueFalse> <EOL>]
                   <TS> "}" <EOL>
<Binary>       ::= {<EfiBinary>} {<OtherBinary>}
<EfiBinary>    ::= <TS> "FILE" <MTS> "EFI" <EfiFileName>
                   [<Overrides>] <EOL>
<EfiFileName>  ::= <MTS> [<PATH>] <Word> {".efi"} {".EFI"} {".Efi"}
<OtherBinary>  ::= <TS> "FILE" <MTS> "BIN" <Filename> <EOL>
```

#### Restrictions

**_TargetArch_**

Only specific architectures are permitted - use of "common" or the wildcard
character is prohibited.

**_Paths_**

For BINARY ONLY content (`UEFI_DRIVER` and `UEFI_APPLICATION` .efi files) the
file names specified in `<EfiFileName>` of this section must be relative to the
directory identified by the `WORKSPACE` system environment variable (or
relative to a directory listed in the PACKAGES_PATH system environment
variable). In some cases, the tools will search well known paths for some
files, for example, for FD filenames, the output will typically be located in
the `$(OUTPUT_DIRECTORY)/ $(TARGET)_$(TAGNAME)/FV` directory.

#### Related Definitions

**_DriverName_**

Specifies the name of the created PCI Option ROM image that will be placed in
the build's FV directory.

**_USE_**

Specifies the architecture to use to create a PCI Option ROM.

**_Filename_**

Filenames must match the actual case of the file; three variations are shown
for the .efi extension in the ENBF above.

#### Example

```c
[OptionRom.AtapiPassThru]
  INF USE = IA32 OptionRomPkg/AtapiPassThruDxe/AtapiPassThruDxe.inf {
    PCI_REVISION = 0x0020
  }
  INF USE = EBC OptionRomPkg/AtapiPassThruDxe/AtapiPassThruDxe.inf
```
