<!--- @file
  3.3 Header Section

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

## 3.3 Header Section

This is an optional section.

#### Summary

This section contains Copyright and License notices for the INF file in
comments that start the file.

This section is optional using a format of:

```ini
## @file Nt32.fdf
# Abstract
#
# Description
#
# Copyright
#
# License
#
##
```

### Prototype

```ini
<Header>      :: = <Comment>*
                   "##" <Space> [<Space>] @file" <EOL>
                   [<Abstract>]
                   [<Description>]
                   <Copyright>+
                   "#" <EOL>
                   <License>+
                   "##" <EOL>
<Filename>    :: = <Word> "." <Extension>
<Abstract>    :: = "#" <MTS> <AsciiString> <EOL>
                   ["#" <EOL>]
<Description> :: = ["#" <MTS> <AsciiString> <EOL>]+
                   ["#" <EOL>]
<Copyright>   ::= "#" <MTS> <CopyName> <Date> "," <CompInfo> <EOL>
<CopyName>    ::= ["Portions" <MTS>] "Copyright (c)" <MTS>
<Date>        ::= <Year> [<TS> {<DateList>} {<DateRange>}]
<Year>        ::= "2" (0-9)(0-9)(0-9)
<DateList>    ::= <CommaSpace> <Year> [<CommaSpace> <Year>]*
<DateRange>   ::= "-" <TS> <Year>
<CompInfo>    ::= (0x20 - 0x7e)* <MTS> "All rights reserved."
                  [<TS> "<BR>"]
<License>     ::= ["#" <MTS> <AsciiString> <EOL>]+
                  ["#" <EOL>]
```

#### Example

```ini
## @file
# Emulation Platform Pseudo Flash Part
#
# The Emulation Platform can be used to debug individual modules,
# prior to creating a real platform. This also provides an example for
# how to create an FDF file.
#
# Copyright (c) 2006 - 2008, NoSuch Corporation. All rights reserved.
#
# This program and the accompanying materials are licensed and made
# available under the terms and conditions of the BSD License which
# accompanies this distribution. The full text of the license may be
# found at:
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS
# OR IMPLIED.
#
##
```
