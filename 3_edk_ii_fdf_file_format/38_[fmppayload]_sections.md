<!--- @file
  3.8 [FmpPayload] Sections

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

## 3.8 [FmpPayload] Sections

These are optional sections that describes the FMP payload content for FMP
Capsule files.

There must be at least one and at most two `<FmpFileData>` statements.  The
`<FmpFileData>` statements start with `FILE DATA`.  The first <FmpFileData>
statement provides the information for UpdateImage in an
`EFI_FIRMWARE_MANAGEMENT_CAPSULE_IMAGE_HEADER`.  The second <FmpFileData>
statement, if present, provides the information for VendorCode in an
`EFI_FIRMWARE_MANAGEMENT_CAPSULE_IMAGE_HEADER`.

#### Prototype

```c
<FmpPayload>  ::= "[FmpPayload" "." <UiFmpName> "]" <EOL>
                  <FmpTokens>
                  <FmpFileData>{1,2}
<UiFmpName>   ::= <Word>
<FmpTokens>   ::= [<TS> "IMAGE_HEADER_INIT_VERSION" <Eq> <Hex2> <EOL>]
                  <TS> "IMAGE_TYPE_ID" <Eq> <RegistryFormatGUID> <EOL>
                  [<TS> "IMAGE_INDEX" <Eq> <Hex2> <EOL>]
                  [<TS> "HARDWARE_INSTANCE" <Eq> <Hex2> <EOL>]
<FmpFileData> ::= <TS> "FILE" <Space> "DATA" <Eq> <Filename> <EOL>
```

#### Example

```ini
[FmpPayload.Payload1]
  # FMP payload header
  IMAGE_HEADER_INIT_VERSION = 0x02
  # FMP payload header
  IMAGE_TYPE_ID     = 938A6F2E-9711-49CE-90D5-7ED68AC96501
  IMAGE_INDEX       = 0x1 # FMP payload header
  HARDWARE_INSTANCE = 0x0 # FMP payload header

  FILE DATA = UpdateImage.bin
  FILE DATA = VendorCodeBytes.bin # optional
```
