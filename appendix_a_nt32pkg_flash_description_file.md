<!--- @file
  Appendix A Nt32Pkg Flash Description File

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

# Appendix A Nt32Pkg Flash Description File

This section provides a sample FDF using the Nt32Pkg/Nt32Pkg.fdf file.

**********
**Note:** This file must NOT be used as is, as data structures and definitions
do not exist.
**********

```ini
## @file
# This is NT32 FDF file with UEFI HII features enabled
#
# Copyright (c) 2007 - 2010, Intel Corporation. All rights reserved.<BR>
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR
# IMPLIED.
#
########################################################################

########################################################################
#
# FD Section
# The [FD] Section is made up of the definition statements and a
# description of what goes into the Flash Device Image. Each FD section
# defines one flash "device" image. A flash device image may be one of
# the following: Removable media bootable image (like a boot floppy
# image,) an Option ROM image (that would be "flashed" into an add-in
# card,) a System "Flash" image (that would be burned into a system's
# flash) or an Update ("Capsule") image that will be used to update and
# existing system flash.
#
########################################################################
[FD.Nt32]
  # The base address of the FLASH Device.
  BaseAddress = 0x0|gEfiNt32PkgTokenSpaceGuid.PcdWinNtFdBaseAddress
  # The size in bytes of the FLASH Device
  Size      = 0x002a0000 ErasePolarity = 1
  BlockSize = 0x10000
  NumBlocks = 0x2a
  #
  #
  # Following are lists of FD Region layout which correspond to the
  # locations of different images within the flash device.
  #
  # Regions must be defined in ascending order and may not overlap.
  #
  # A Layout Region start with a eight digit hex offset (leading "0x"
  # required) followed by the pipe "|" character, followed by the size of
  # the region, also in hex with the leading "0x" characters. Like:
  # Offset|Size
  # PcdOffsetCName|PcdSizeCName
  # RegionType <FV, DATA, or FILE>
  #
  ########################################################################
  0x00000000|0x00280000
  gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashFvRecoveryBase|gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashFvRecoverySize
  FV = FvRecovery
  0x00280000|0x0000c000
  gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashNvStorageVariableBase|gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableSize
  #NV_VARIABLE_STORE
  DATA = {
  ## This is the EFI_FIRMWARE_VOLUME_HEADER
  # ZeroVector []
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  # FileSystemGuid: gEfiSystemNvDataFvGuid =
  # { 0xFFF12B8D, 0x7696, 0x4C8B,
  # { 0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50 }}
  0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C,
  0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50,
  # FvLength: 0x20000
  0x00, 0x00, 0x02, 0x00, 0x00, 0x00, 0x00, 0x00,
  #Signature "_FVH" #Attributes
  0x5f, 0x46, 0x56, 0x48, 0xff, 0xfe, 0x04, 0x00,
  #HeaderLength #CheckSum #ExtHeaderOffset #Reserved #Revision
  0x48, 0x00, 0x36, 0x09, 0x00, 0x00, 0x00, 0x02,
  #Blockmap[0]: 2 Blocks * 0x10000 Bytes / Block
  0x02, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01, 0x00,
  #Blockmap[1]: End
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  ## This is the VARIABLE_STORE_HEADER`
  #Signature: gEfiVariableGuid =
  # { 0xddcf3616, 0x3275, 0x4164,
  # { 0x98, 0xb6, 0xfe, 0x85, 0x70, 0x7f, 0xfe, 0x7d }}
  0x16, 0x36, 0xcf, 0xdd, 0x75, 0x32, 0x64, 0x41,
  0x98, 0xb6, 0xfe, 0x85, 0x70, 0x7f, 0xfe, 0x7d,
  #Size: 0xc000
  # (gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageVariableSize) -
  # 0x48 (size of EFI_FIRMWARE_VOLUME_HEADER) = 0xBFB8
  # This can speed up the Variable Dispatch a bit.
  0xB8, 0xBF, 0x00, 0x00,
  #FORMATTED: 0x5A #HEALTHY: 0xFE #Reserved: UINT16 #Reserved1: UINT32
  0x5A, 0xFE, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
}
0x0028c000 | 0x00002000
#NV_EVENT_LOG
gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashNvStorageEventLogBase|gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashNvStorageEventLogSize

0x0028e000 | 0x00002000
gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashNvStorageFtwWorkingBase|gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageFtwWorkingSize

#NV_FTW_WORKING
DATA = {
  # EFI_FAULT_TOLERANT_WORKING_BLOCK_HEADER->Signature =
  # gEfiSystemNvDataFvGuid = { 0xFFF12B8D, 0x7696, 0x4C8B,
  # { 0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50 }}
  0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C,
  0xA9, 0x85, 0x27, 0x47, 0x07, 0x5B, 0x4F, 0x50,
  # Crc:UINT32
  # WorkingBlockValid:1, WorkingBlockInvalid:1, Reserved
  #
  0x77, 0x13, 0x9B, 0xD7, 0xFE, 0xFF, 0xFF, 0xFF,
  # WriteQueueSize: UINT64
  0xE0, 0x1F, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
}

0x00290000 | 0x00010000
#NV_FTW_SPARE
gEfiNt32PkgTokenSpaceGuid.PcdWinNtFlashNvStorageFtwSpareBase|gEfiMdeModulePkgTokenSpaceGuid.PcdFlashNvStorageFtwSpareSize

########################################################################
#
# FV Section
#
# [FV] section is used to define what components or modules are placed
# within a flash device file. This section also defines order the
# components and modules are positioned within the image. The [FV] # section consists of define statements, set statements and module # statements.
#
########################################################################
[FV.FvRecovery]
FvBaseAddress = 0x0 # FV Base Address for the Backup copy of FV
FvAlignment   = 16  # FV alignment

#FV attributes setting.
ERASE_POLARITY     = 1
MEMORY_MAPPED      = TRUE
STICKY_WRITE       = TRUE
LOCK_CAP           = TRUE
LOCK_STATUS        = TRUE
WRITE_DISABLED_CAP = TRUE
WRITE_ENABLED_CAP  = TRUE
WRITE_STATUS       = TRUE
WRITE_LOCK_CAP     = TRUE
WRITE_LOCK_STATUS  = TRUE
READ_DISABLED_CAP  = TRUE
READ_ENABLED_CAP   = TRUE
READ_STATUS        = TRUE
READ_LOCK_CAP      = TRUE
READ_LOCK_STATUS   = TRUE
FvNameGuid         = 6D99E806-3D38-42c2-A095-5F4300BFD7DC

########################################################################
#
# The INF statements point to EDK II module INF files,
# which will be placed into this FV image.
# Parsing tools will scan the INF file to determine the type of component
# or module.
# The component or module type is used to reference the standard rules
# defined elsewhere in the FDF file.
#
# The format for INF statements is:
# INF $(PathAndInfFileName)
#
########################################################################
  ##
  # PEI Phase modules
  ##
  # PEI Apriori file example, more PEIM module added later.
  ##

DEFINE MdeModUni = MdeModulePkg/Universal
DEFINE WNOEMHOOK = Nt32Pkg/WinNtOemHookStatusCodeHandlerPei

DEFINE RSCR_P    = ReportStatusCodeRouter/Pei

APRIORI PEI {
  INF $(MdeModUni)/PCD/Pei/Pcd.inf
  INF $(MdeModUni)/$(RSCR_P)/ReportStatusCodeRouterPei.inf
  INF $(MdeModUni)/StatusCodeHandler/Pei/StatusCodeHandlerPei.inf
  INF $(WNOEMHOOK)/WinNtOemHookStatusCodeHandlerPei.inf
  }

APRIORI DXE {
  INF MdeModulePkg/Universal/PCD/Dxe/Pcd.inf
  INF Nt32Pkg/MetronomeDxe/MetronomeDxe.inf
  }

INF MdeModulePkg/Core/Pei/PeiMain.inf
INF MdeModulePkg/Universal/PCD/Pei/Pcd.inf
INF $(MdeModUni)/$(RSCR_P)/ReportStatusCodeRouterPei.inf
INF $(MdeModUni)/StatusCodeHandler/Pei/StatusCodeHandlerPei.inf
INF $(WINOEMHOOK)/WinNtOemHookStatusCodeHandlerPei.inf
INF Nt32Pkg/BootModePei/BootModePei.inf
INF Nt32Pkg/StallPei/StallPei.inf
INF Nt32Pkg/WinNtFlashMapPei/WinNtFlashMapPei.inf
INF Nt32Pkg/WinNtAutoScanPei/WinNtAutoScanPei.inf
INF Nt32Pkg/WinNtFirmwareVolumePei/WinNtFirmwareVolumePei.inf
INF MdeModulePkg/Universal/Variable/Pei/VariablePei.inf
INF Nt32Pkg/WinNtThunkPPIToProtocolPei/WinNtThunkPPIToProtocolPei.inf
INF MdeModulePkg/Core/DxeIplPeim/DxeIpl.inf

  ##
  # DXE Phase modules
  ##
INF MdeModulePkg/Core/Dxe/DxeMain.inf
INF MdeModulePkg/Universal/PCD/Dxe/Pcd.inf
INF Nt32Pkg/MetronomeDxe/MetronomeDxe.inf
INF Nt32Pkg/RealTimeClockRuntimeDxe/RealTimeClockRuntimeDxe.inf
INF Nt32Pkg/ResetRuntimeDxe/ResetRuntimeDxe.inf
INF MdeModulePkg/Core/RuntimeDxe/RuntimeDxe.inf
INF Nt32Pkg/FvbServicesRuntimeDxe/FvbServicesRuntimeDxe.inf
INF MdeModulePkg/Universal/SecurityStubDxe/SecurityStubDxe.inf
INF MdeModulePkg/Universal/SmbiosDxe/SmbiosDxe.inf
INF MdeModulePkg/Universal/EbcDxe/EbcDxe.inf
INF $(MdeModUni)/MemoryTest/NullMemoryTestDxe/NullMemoryTestDxe.inf
INF MdeModulePkg/Universal/HiiDatabaseDxe/HiiDatabaseDxe.inf
INF Nt32Pkg/WinNtThunkDxe/WinNtThunkDxe.inf
INF Nt32Pkg/CpuRuntimeDxe/CpuRuntimeDxe.inf
INF IntelFrameworkModulePkg/Universal/BdsDxe/BdsDxe.inf
INF $(MdeModUni)/FaultTolerantWriteDxe/FaultTolerantWriteDxe.inf
INF Nt32Pkg/MiscSubClassPlatformDxe/MiscSubClassPlatformDxe.inf
INF Nt32Pkg/TimerDxe/TimerDxe.inf

DEFINE RSCR_RD = ReportStatusCodeRouter/RuntimeDxe
INF $(MdeModUni)/$(RSCR_RD)/ReportStatusCodeRouterRuntimeDxe.inf

DEFINE SCH_RD = StatusCodeHandler/RuntimeDxe
INF $(MdeModUni)/$(SCH_RD)/StatusCodeHandlerRuntimeDxe.inf

DEFINE NTWINNTOEMHOOK = Nt32Pkg/WinNtOemHookStatusCodeHandlerDxe
INF $(NTWINNTOEMHOOK)/WinNtOemHookStatusCodeHandlerDxe.inf

INF MdeModulePkg/Universal/Variable/RuntimeDxe/VariableRuntimeDxe.inf
INF MdeModulePkg/Universal/WatchdogTimerDxe/WatchdogTimer.inf

DEFINE MCRD = MonotonicCounterRuntimeDxe
INF $(MdeModUni)/$(MCRD)/MonotonicCounterRuntimeDxe.inf

INF MdeModulePkg/Universal/CapsuleRuntimeDxe/CapsuleRuntimeDxe.inf
INF MdeModulePkg/Universal/Console/ConPlatformDxe/ConPlatformDxe.inf
INF MdeModulePkg/Universal/Console/ConSplitterDxe/ConSplitterDxe.inf
INF $(MdeModUni)/Console/GraphicsConsoleDxe/GraphicsConsoleDxe.inf
INF MdeModulePkg/Universal/Console/TerminalDxe/TerminalDxe.inf
INF MdeModulePkg/Universal/DevicePathDxe/DevicePathDxe.inf
INF MdeModulePkg/Universal/Disk/DiskIoDxe/DiskIoDxe.inf
INF MdeModulePkg/Universal/Disk/PartitionDxe/PartitionDxe.inf
INF MdeModulePkg/Universal/SetupBrowserDxe/SetupBrowserDxe.inf
INF MdeModulePkg/Universal/PrintDxe/PrintDxe.inf

DEFINE DUC = Disk/UnicodeCollation
INF RuleOverride = TIANOCOMPRESSED $(MdeModUni)/$(DUC)/EnglishDxe/EnglishDxe.inf

INF MdeModulePkg/Bus/Pci/PciBusDxe/PciBusDxe.inf
INF MdeModulePkg/Bus/Scsi/ScsiBusDxe/ScsiBusDxe.inf
INF MdeModulePkg/Bus/Scsi/ScsiDiskDxe/ScsiDiskDxe.inf
INF IntelFrameworkModulePkg/Bus/Pci/IdeBusDxe/IdeBusDxe.inf
INF Nt32Pkg/WinNtBusDriverDxe/WinNtBusDriverDxe.inf
INF Nt32Pkg/WinNtBlockIoDxe/WinNtBlockIoDxe.inf
INF Nt32Pkg/WinNtSerialIoDxe/WinNtSerialIoDxe.inf
INF Nt32Pkg/WinNtGopDxe/WinNtGopDxe.inf
INF Nt32Pkg/WinNtSimpleFileSystemDxe/WinNtSimpleFileSystemDxe.inf
INF $(MdeModUni)/PlatformDriOverrideDxe/PlatformDriOverrideDxe.inf
INF MdeModulePkg/Universal/DriverSampleDxe/DriverSampleDxe.inf

INF MdeModulePkg/Universal/Network/DpcDxe/DpcDxe.inf
INF MdeModulePkg/Universal/Network/ArpDxe/ArpDxe.inf
INF MdeModulePkg/Universal/Network/Dhcp4Dxe/Dhcp4Dxe.inf
INF MdeModulePkg/Universal/Network/Ip4ConfigDxe/Ip4ConfigDxe.inf
INF MdeModulePkg/Universal/Network/Ip4Dxe/Ip4Dxe.inf
INF MdeModulePkg/Universal/Network/MnpDxe/MnpDxe.inf
INF MdeModulePkg/Universal/Network/VlanConfigDxe/VlanConfigDxe.inf
INF MdeModulePkg/Universal/Network/Mtftp4Dxe/Mtftp4Dxe.inf
INF MdeModulePkg/Universal/Network/Tcp4Dxe/Tcp4Dxe.inf
INF MdeModulePkg/Universal/Network/Udp4Dxe/Udp4Dxe.inf
INF Nt32Pkg/SnpNt32Dxe/SnpNt32Dxe.inf
INF MdeModulePkg/Universal/Network/UefiPxeBcDxe/UefiPxeBcDxe.inf
INF MdeModulePkg/Universal/Network/IScsiDxe/IScsiDxe.inf

########################################################################
#
# FILE statements are provided so that a platform integrator can include
# complete EFI FFS files, as well as a method for constructing FFS files
# using curly "{}" brace scoping. The following three FILEs are
# for binary shell, binary fat and logo module.
#
########################################################################
FILE APPLICATION = PCD(gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdShellFile) {
  SECTION PE32 = EdkShellBinPkg/FullShell/Ia32/Shell_Full.efi
  }
FILE DRIVER = 961578FE-B6B7-44c3-AF35-6BC705CD2B1F {
  SECTION PE32 = FatBinPkg/EnhancedFatDxe/Ia32/Fat.efi
  }
FILE FREEFORM = PCD(gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdLogoFile) {
  SECTION RAW = MdeModulePkg/Logo/Logo.bmp
  }

########################################################################
#
# Rules are use with the [FV] section's module INF type to define
# how an FFS file is created for a given INF file. The following Rule are
# the default rules for the different module type. User can add the
# customized rules to define the content of the FFS file.
#
########################################################################

########################################################################
# Example of a DXE_DRIVER FFS file with a Checksum encapsulation section
#
#
#
#[Rule.Common.DXE_DRIVER]
#   FILE DRIVER = $(NAMED_GUID) {
#     DXE_DEPEX DXE_DEPEX Optional $(INF_OUTPUT)/$(MODULE_NAME).depex
#     COMPRESS PI_STD {
#       GUIDED {
#         PE32    PE32                    $(INF_OUTPUT)/$(MODULE_NAME).efi
#         UI      STRING="$(MODULE_NAME)" Optional
#         VERSION STRING="$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
#       }
#     }
#   }
#
########################################################################

[Rule.Common.PEI_CORE]
  FILE PEI_CORE = $(NAMED_GUID) {
    PE32    PE32 Align=4K             $(INF_OUTPUT)/$(MODULE_NAME).efi
    UI      STRING = "$(MODULE_NAME)" Optional
    VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
  }

[Rule.Common.PEIM]
  FILE PEIM = $(NAMED_GUID){
    PEI_DEPEX PEI_DEPEX                 Optional $(INF_OUTPUT)/$(MODULE_NAME).depex
    PE32      PE32 Align=4K             $(INF_OUTPUT)/$(MODULE_NAME).efi
    UI        STRING = "$(MODULE_NAME)" Optional
    VERSION   STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
  }

[Rule.Common.DXE_CORE]
  FILE DXE_CORE = $(NAMED_GUID) {
    COMPRESS PI_STD {
      PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
      UI      STRING = "$(MODULE_NAME)" Optional
      VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
    }
  }

[Rule.Common.UEFI_DRIVER]
  FILE DRIVER = $(NAMED_GUID) {
    DXE_DEPEX DXE_DEPEX Optional $(INF_OUTPUT)/$(MODULE_NAME).depex
    COMPRESS PI_STD {
      GUIDED {
        PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
        UI      STRING = "$(MODULE_NAME)" Optional
        VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
      }
    }
  }

[Rule.Common.UEFI_DRIVER.TIANOCOMPRESSED]
  FILE DRIVER = $(NAMED_GUID) {
    DXE_DEPEX DXE_DEPEX Optional $(INF_OUTPUT)/$(MODULE_NAME).depex
    GUIDED A31280AD-481E-41B6-95E8-127F4C984779 PROCESSING_REQUIRED = TRUE {
      PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
      UI      STRING = "$(MODULE_NAME)" Optional
      VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
    }
  }

[Rule.Common.DXE_DRIVER]
  FILE DRIVER = $(NAMED_GUID) {
    DXE_DEPEX   DXE_DEPEX Optional        $(INF_OUTPUT)/$(MODULE_NAME).depex
    COMPRESS PI_STD {
      GUIDED {
        PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
        UI      STRING = "$(MODULE_NAME)" Optional
        VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
      }
    }
  }

[Rule.Common.DXE_RUNTIME_DRIVER]
  FILE DRIVER = $(NAMED_GUID) {
    DXE_DEPEX   DXE_DEPEX Optional        $(INF_OUTPUT)/$(MODULE_NAME).depex
    COMPRESS PI_STD {
      GUIDED {
        PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
        UI      STRING = "$(MODULE_NAME)" Optional
        VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
      }
    }
  }

[Rule.Common.UEFI_APPLICATION]
  FILE APPLICATION = $(NAMED_GUID) {
    COMPRESS PI_STD {
      GUIDED {
        PE32    PE32                      $(INF_OUTPUT)/$(MODULE_NAME).efi
        UI      STRING = "$(MODULE_NAME)" Optional
        VERSION STRING = "$(INF_VERSION)" Optional BUILD_NUM=$(BUILD_NUMBER)
      }
    }
  }
```
