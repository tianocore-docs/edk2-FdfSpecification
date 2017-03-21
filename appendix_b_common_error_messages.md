<!--- @file
  Appendix B Common Error Messages

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

# Appendix B Common Error Messages

Standard build tools must throw error messages, halting the build. Warning
messages may be emitted from build tools, unless a quiet flag has been set on
the command-line.

## B.1 [FD] Syntax Errors:

Missing required Token statements - report line number and file name.

Size of the FV (%s) is larger than the Region Size 0x%X specified.

The named FV (%s) is not listed in the FDF file.

Size of the DATA is larger than the region size specified.

Size of the FILE (%s) is larger than the region size specified.

The region at Offset 0x%X cannot fit into Block array with BlockSize %X.

Region: %s is not in the FD address scope.

## B.2 [FV] Syntax Errors:

Missing required Token statements - report line number and file name.

Incorrect alignment in the FV.

## B.3 [CAPSULE] Syntax Errors:

No capsule specific errors, as all capsule errors are captured under other
parser errors.

## B.4 [Rule] Syntax Errors:

Unable to find rule for INF %s.

Module built under different architectures, unable to determine which module to
use.
