<!--- @file
  2.7 [VTF] Sections

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

## 2.7 [VTF] Sections

The optional `[VTF]` sections specify information regarding the IPF Boot Strap
File (BSF) or the IA32 Volume Top File (VTF). Both the `ARCH` and the `UiName`
modifier fields are required. The `[VTF]` section terminates with either the
start of another section, or the end of the file.

The `[VTF]` section modifier usage is shown below.

`[VTF.ARCH.UiName]`

Underneath the `[VTF]` section are specific statements defining information
about the VTF file. EDK Bsf.inf files use two different sections, an `[OPTIONS]`
section and a `[COMPONENTS]` section. For EDK II, the grammar of the `[VTF]`
section statements defines these sections, rather than having separate
sub-sections within the `[VTF]` section.

The format for statements within the section is illustrated below.

`STATEMENT_NAME = Value`

The component version number (`COMP_VER`) values are binary coded decimal
(1 byte for the major number and 1 byte for the minor number). As a result, the
maximum value is "99.99".

### 2.7.1 Options Statement

One and only one options statement, "IA32_RST_BIN", is permitted within any
one `[VTF]` section. This value is a path and name of `IA32_BIOS` reset vector
binary (16 byte) file. If needed, this binary can be put into the VTF file.

### 2.7.2 Component Statements

Within the section, a components sub-section starts with the "COMP_NAME"
statement, and terminates with either the start of another sub-section, major
section or the end of the file. Certain values for component statements are
enumerated values or values that are within a given, specification defined,
range.

Each of the component sections is used to complete a data structure, using the
following sequence.

```c
Name = Region,
       Type,
       Version,
       Checksum Flag,
       Path of Binary File,
       Path of the Symbol File,
       Preferred Size;
```
