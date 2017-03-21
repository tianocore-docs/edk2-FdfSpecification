<!--- @file
  1.1 Overview

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

## 1.1 Overview

EDK II tools use INI-style text-based files to describe components, platforms
and firmware volumes. The EDK II Build Infrastructure supports generation of
binary images compliant with Unified EFI Forum (UEFI) specifications UEFI and
Platform Initialization (PI).

The EDK II build processes, defined in the EDK II Build Specification, use
separate steps to create EFI images. EDK Build Tools are included as part of
the EDK II compatibility package. In order to use EDK II Modules or the EDK II
Build Tools, EDK II DSC and FDF files must be used.

The EDK II FDF file is used in conjunction with an EDK II DSC file to generate
bootable images, option ROM images, and update capsules for bootable images
that comply with the UEFI specifications listed above.

The FDF file describes the content and layout of binary images that are either
a boot image or PCI Option ROMs.

This document describes the format of EDK II FDF files that are required for
building binary images for an EDK II platform. The goals are:

* **Compatibility** - No compatibility with EDK FDF files exists in format
  or tools.

* **Simplified platform build and configuration** - The FDF files simplify the
  process of adding EDK components and EDK II modules to a firmware volume on
  any given platform.

The EDK build tools are provided as part of the EdkCompatibilityPkg which is
included in EDK II.

Table 1 shows the FDF compatibility between platform, module and component
builds.

###### Table 1 EDK Build Infrastructure Support Matrix

|                    | EDK FDF | EDK II FDF | EDK DSC | EDK II DSC |
| ------------------ |:-------:|:----------:|:-------:|:----------:|
| EDK Build Tools    | YES     | NO         | YES     | NO         |
| EDK II Build Tools | NO      | YES        | NO      | YES        |
