<!--- @file
  2.9 [OptionRom] Sections

  Copyright (c) 2006-2019, Intel Corporation. All rights reserved.<BR>

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

## 2.8 [OptionRom] Sections

The EDK II `[OptionRom]` sections allow for extending the FDF for processing of
standalone legacy PCI Option ROM images or stand-alone UEFI PCI Option ROM
images. A required modifier, DriverName, will specify where in the build's FV
directory, the OptionROM file will be placed.

If the user is only creating PCI Option ROM images, then the `[FV]` and `[FD]`
sections are not required. If an FD and FV sections exist, then the tools will
create the FD image as well as the Option ROM image. If they are not in the FDF
file, then they will only generate the Option ROM image.

Each `[OptionRom]` section terminates with either the start of another section,
or the end of the file. The `[OptionRom]` section header usage is shown below.

`[OptionRom.ROMName]`

If more than architecture (for example, IA32 and EBC) for the driver is to be
bundled in an option rom file, then more than one INF entry (specified by the
USE option) can be used to include the other architecture.

Having different sections for the same option rom driver for different
architectures is not permitted.

This is an optional section for platform images.
