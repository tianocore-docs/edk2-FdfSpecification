<!--- @file
  2.8 [Rule] Sections

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

## 2.8 [Rule] Sections

The optional `[Rule]` sections in the FDF file are used for combining binary
images, not for compiling code. Rules are use with the `[FV]` section's module
INF type to define how an FFS file is created for a given INF file. The EDK II
Build Specification defines the default rules that are implicitly used for
creating FFS files. The implicit rules follow the _PI Specification_ and
_UEFI Specification_.

The `[Rule]` section of the FDF file is used to define custom rules, which may
be applied to a given INF file listed in an `[FV]` section. This section is
also used to define rules for module types that permit the user to define the
content of the FFS file - when an FFS type is not specified by either PI or
UEFI specifications.

The Rules can have multiple modifiers as shown below.

`[Rule.ARCH.MODULE_TYPE.TEMPLATE_NAME]`

If no `TEMPLATE_NAME` is given then the match is based on `ARCH` and
`MODULE_TYPE` modifiers. `BINARY` is a reserved `TEMPLATE_NAME` as a default rule
name for binary modules. The `TEMPLATE_NAME` must be unique to the `ARCH` and
`MODULE_TYPE`. It is permissible to use the same `TEMPLATE_NAME` for two or
more `[Rule]` sections if the `ARCH` or the `MODULE_TYPE` listed are different
for each of the sections.

A `[Rule]` section is terminated by another section header or the end of file.

The content of the `[Rule]` section is based on the `FILE` and section grammar
of the FV section. The difference is the `FILE` referenced in the `[RULE]` is a
`MACRO`. The section grammar is extended to include an optional argument,
`Optional`. The `Optional` argument is used to say a section is optional. That
is to say, if it does not exist, then it is O.K.

**********
**Note:** The `!include` statement is valid for any part of the `[Rule]`
section, including an entire `[Rule]` section.
**********

The generic form of the entries for leaf sections is:

`<SectionType> <FileType> [Options] [{<Filename>} {<Extension>}]`

When processing the FDF file, the following rules apply (in order):

1. If `<SectionType>` not defined or not a legal name, then error
2. If `<FileType>` not defined or not a legal name, then error
3. If `[FilePath/FileName]`, then:
   Add one section to FFS with a section type of `<SectionType>`
4. Else:
   Find all files defined by the INF file whose file type is `<FileType>` and
   add each one to the FFS with a section type of `<SectionType>` in
   alphabetical order.
   Add files defined in `[Sources]` followed by files defined in `[Binaries]`

5. If > 1 `UI` section in final FFS, then error
6. If > 1 `VER` section in final FFS, then error
7. If > 1 `PEI_DEPEX` section in final FFS, then error
8. If > 1 `DXE_DEPEX` section in final FFS, then error
9. If > 1 `SMM_DEPEX` section in final FFS, then error

If a rule specifies a file type, instead of specifying specific file names, the
files that match the extension must be processed in alphabetical order.

#### Example

```ini
[Rule.Common.ACPITABLE]
  FILE FREEFORM = $(NAMED_GUID) {
    RAW ACPI Optional |.acpi
    RAW ASL  Optional |.aml
  }
```

Tools must add the processed .acpi files alphabetically, followed by the .aml
files which must also be added alphabetically.

The file would contain:

`<SOF>a1.acpi, a2.acpi, b1.acpi, b2.acpi, a.aml, b.aml<EOF>`

where, start of file is `<SOF>` and end of file is `<EOF>`

Refer to the _EDK II INF File Specification_ for a description of the
`FileType` for binary files.
