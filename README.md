<!--- @file
  README.md for EDK II Flash Description (FDF) File Specification

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

<img src="media/TianocoreTitlePageLogo.jpg" width="300" />

### {{ book.title }}

{% if book.draft %}
** DRAFT FOR REVIEW **
{% else %}
** {{ book.version }} **
{% endif %}

** {{ gitbook.time|date('MM/DD/YYYY hh:mm:ss') }} **

{% if book.udkrelease %}
** {{ book.udkrelease }} **
{% endif %}


### Acknowledgements

Redistribution and use in source (original document form) and 'compiled'
forms (converted to PDF, epub, HTML and other formats) with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code (original document form) must retain the
   above copyright notice, this list of conditions and the following
   disclaimer as the first lines of this file unmodified.

2. Redistributions in compiled form (transformed to other DTDs, converted to
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

Copyright (c) 2006-2017, Intel Corporation. All rights reserved.


### Revision History

| Revision   | Revision History                                                                                                                                                           | Date          |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| 1.0        | Initial release.                                                                                                                                                           | December 2007 |
| 1.1        | Updated based on errata                                                                                                                                                    | August 2008   |
| 1.2        | Updated based on enhancement requests                                                                                                                                      | June 2009     |
| 1.21       | Updated based on enhancement requests and errata                                                                                                                           | March 2010    |
|            | Added support for SMM_CORE                                                                                                                                                 |               |
|            | Added support for CAPSULE_FLAGS_INITIATE_RESET                                                                                                                             |               |
|            | Added Block Statements to all Capsule sections                                                                                                                             |               |
|            | Added "Auto" keyword to FFS alignment                                                                                                                                      |               |
|            | Rule processing for file type lists is alphabetical, i.e., files are added in alphabetical order                                                                           |               |
|            | HSD 203863 Macro Definitions in DSC file are now global to both DSC and FDF files                                                                                          |               |
|            | PCD Values may be constructed using C-style expressions provided the result of the expression matches the datum type of the PCD                                            |               |
|            | FeatureFlagExpression is now defined as a C-style expression using C relational, equality and logical numeric and bitwise operators and/or arithmetic                      |               |
|            |   and bitwise operators that evaluate to a value that matches the Datum Type of the PCD. Precedence and associativity follow C standards                                   |               |
| 1.22       | Grammatical and formatting editing                                                                                                                                         | May 2010      |
| 1.22 w/    | Updated to match the implementation at the time of the UDK2010 SR1 release:                                                                                                | December 2011 |
| Errata A   | Updated to support UEFI version 2.3.1 and updated spec release dates in Introduction                                                                                       |               |
|            | Clarify UEFI's PI Distribution Package Specification                                                                                                                       |               |
|            | Standardize Common data definitions for all specifications                                                                                                                 |               |
|            | Added NOTE in 3.2 saying the sections must appear in the order listed above.                                                                                               |               |
|            | Spelling and punctuation fixes                                                                                                                                             |               |
|            | Do not require the FDF_SPECIFICATION to be updated if that is the only thing that changes.                                                                                 |               |
|            | Removed duplicate content and added the scoping rules for Macros, clarified MACRO Summary                                                                                  |               |
|            | Added statement to allow specifying an FD in a Capsule as well as to allow any file in a Capsule                                                                           |               |
|            | Removed references to system environment variables in the Macros section                                                                                                   |               |
|            | Clarify that C data arrays must be byte arrays for PCD value fields; allowing C format GUID structures as well.                                                            |               |
|            | Updated conditional rules in 2.2.8 for how to use macro and PCDs; updated PCD usage in conditional statements                                                              |               |
|            | Add clarification to require a comparison operator for string tokens (no atomic string) in expressions for conditional directives                                          |               |
|            | Updated expression descriptions                                                                                                                                            |               |
|            | Allow a PCD's value to be used in other sections, not just the section it was defined in                                                                                   |               |
|            | Added table of valid environment variables that can be used in this file; put in table of System Environment variables and the EDK II Global macro values.                 |               |
|            | Updated how macros can be used in 2.2.6; updated where macros are evaluated                                                                                                |               |
|            | Provide rules for how macros can be used in different BuildOptions sections                                                                                                |               |
|            | Make sure that macros are not restricted to only directories                                                                                                               |               |
|            | Prohibit macros in the filename specified in !include statements                                                                                                           |               |
|            | Updated 2.3.3 and 2.4.3 to allow macro and expression for value in a set statement                                                                                         |               |
|            | Added `<Filename>` to Macro values.                                                                                                                                        |               |
|            | Clarify that only a limited number of system environment variables, not macros can be used in the !include statement                                                       |               |
|            | Clarify the rules for finding the !include files                                                                                                                           |               |
|            | Clarify how macros can be shared between sections                                                                                                                          |               |
|            | Add statement about not expanding macros that are in a double quoted string.                                                                                               |               |
|            | Update macro usages in conditional directive statements                                                                                                                    |               |
|            | Put in rules for combining sections and where macros can be inherited                                                                                                      |               |
| 1.22 w/    | Updates:                                                                                                                                                                   | December 2011 |
| Errata A   | Updated OptionROM section for COMPRESS to PCI_COMPRESS, matching the INF spec.                                                                                             |               |
| (Cont.)    | Removed CREATE_FILE from all FDF sections that had it                                                                                                                      |               |
|            | Add FvBaseAddress attribute in [FV] section                                                                                                                                |               |
|            | Added BCD Hex choice for COMP_VER in VTF section                                                                                                                           |               |
|            | Changed #elif to #elseif in 3.2.3 to match implementation                                                                                                                  |               |
|            | Removed duplicate definition of true in 3.2.3                                                                                                                              |               |
|            | Added EBNF for `<Extension>`                                                                                                                                               |               |
|            | Allow DATA section to contain a GUID value                                                                                                                                 |               |
|            | Add reserved keyword, BINARY as Rule name, updated rule override information for binary modules                                                                            |               |
|            | Clarify that the VTF COMP_VER value is stored in BCD using 1 byte for major and one byte for the minor number, yielding a maximum value 99.99                              |               |
|            | Removed support for variable block size in the [FD] section - not currently supported by build tools                                                                       |               |
|            | Removed paragraph about build options in section 2.2.6                                                                                                                     |               |
|            | Updated priority list; described precedence of the SET statements overriding previous definitions; removing set scoping statements                                         |               |
|            | Update EBNF for conditionals and removed                                                                                                                                   |               |
|            | `<SectionStatements>` in EBNF                                                                                                                                              |               |
|            | Remove the word "should" and replace it with other text that means "recommended, but not required", replaced remaining instances of should with either must or will        |               |
|            | Updated 2.2.8 to include text to describe examples                                                                                                                         |               |
|            | Added 2.2.9 section which includes expression table and text                                                                                                               |               |
|            | Updated 2.2.10 to state that macros are evaluated when used, not when entered                                                                                              |               |
|            | Added the "IN" operator as an Equality Operator - added description and restriction of it's usage using                                                                    |               |
|            | `<MemberExpression>`                                                                                                                                                       |               |
|            | Define how EDK_GLOBAL values can be used, but not defined in the FDF file                                                                                                  |               |
|            | Add text regarding the FvForceRebase flag                                                                                                                                  |               |
|            | Removed PART_NAME from [Defines] section, as it is not supported by current tools and none of the FDF files have the [Defines] section                                     |               |
|            | Removed the GuidCName from `<NamedGuid>` as this is not supported by tools at this time                                                                                    |               |
| 1.22 w/    | Updates:                                                                                                                                                                   | June 2012     |
| Errata B   | Section 1.3, Updated UEFI and PI specifications to reflect Errata                                                                                                          |               |
|            | Section 2.2.6, Provide clarification on MACRO values                                                                                                                       |               |
|            | Section 3.8, Remove undocumented tags that begin with "SEC_" that are used internally by tools                                                                             |               |
|            | Sections 2.4.6.2, 3.6, 3.7 and 3.8, Provide explanation of SUBTYPE_GUID in Parameters section                                                                              |               |
|            | Sections 3.6, 3.7 and 3.8, Modify the EBNF entries for section SUBTYPE_GUID to allow specifying the GUID value                                                             |               |
|            | 3.6, 3.7 and 3.8, Clarify User Interface entries in parameters section                                                                                                     |               |
|            | Section 3.2.1, Remove invalid reference to ExtendedLine                                                                                                                    |               |
|            | Section 3.2.1, Fix the DOS EOL character sequence                                                                                                                          |               |
|            | The FDF Specification Version will not change in the `[Defines]` section                                                                                                   |               |
| 1.22 w/    | Updates                                                                                                                                                                    | August 2013   |
| Errata C   | Section 1.3, Updated UEFI and PI specifications with Errata                                                                                                                |               |
|            | Section 2.3,4.3, 2.4.5, 3.5, 3.6, 3.7, 3.8 and 3.10 Allow binary files to be located outside of the WORKSPACE                                                              |               |
|            | Section 2.4.6 and 2.4.6.1, added type DISPOSABLE to encapsulation file types                                                                                               |               |
|            | Sections 3.6, 3.7 and 3.8 Clarify that the plain text file for the User Interface is plain ASCII text                                                                      |               |
|            | Section 2.1 Updated the figure to remove top-level Makefile                                                                                                                |               |
|            | Section 2.1 Update location of the FDF file                                                                                                                                |               |
|            | Section 2.2.8,and 3.2.3 Clarify PCD usage in conditional directive statements                                                                                              |               |
|            | Included new section PCD RULES                                                                                                                                             |               |
|            | Added reference to the EDK II Build Specification for PCD Processing Rules                                                                                                 |               |
|            | Added `<Afile>` Definition to the Capsule section EBNF for appending the contents of a binary file to a section; appends are listed in order first in list first append,   |               |
|            |   second in list is appended after the first. These are strictly data files which can only be used by a driver that has a prior knowledge of the content                   |               |
| 1.24       | Updates:                                                                                                                                                                   | December 2014 |
|            | Changed specification version to 1.24                                                                                                                                      |               |
|            | Updated FDF_SPECIFICATION to 0x00010018 and updated EBNF to specify this value as 1.24                                                                                     |               |
|            | Updated UEFI specification and EDK II meta data specifications in section 1.2; added the EDK II UNI Unicode File Specification and EDK II Expression Syntax Specification  |               |
| 1.24 w/    | Updates:                                                                                                                                                                   | March 2015    |
| Errata A   | Update link to the EDK II Specifications, fixed the name of the Multi-String .UNI File Format Specification                                                                |               |
|            | When FvNameGuid is present in an FV section, the EDK II build tools will generate an FvName string in the FV EXT Header                                                    |               |
|            | Removed [UserExtensions] sections 2.9 and 3.11 as these sections should be used in DSC files, not the FDF file.                                                            |               |
| 1.25       | Updates:                                                                                                                                                                   | June 2015     |
|            | Updated to support UEFI 2.5 and PI 1.4                                                                                                                                     |               |
|            | Added clarificaiton regarding use of .. and . in path names                                                                                                                |               |
|            | Add support to generate an FMP Capsule in section 3.7 and added new section for FMP Payload data.                                                                          |               |
|            | Since the above adds a new feature, update minor revision of FDF SPECIFICATION to 0x00010019 Only FDF files that use new feature need to use 0x0010019,                    |               |
|            | older FDF files that do not use the functionality do not need to modify the FDF_SPECIFICATION values                                                                       |               |
|            | Added clarification on when the FDF_SPECIFICATION value must be updated to 0x00010019                                                                                      |               |
| 1.24 w/    | Updates:                                                                                                                                                                   | July 2015     |
| Errata A   | - Require a boolean flag, FvNameString to an FV section in order for the tools to generate the the FvNameString entry in an extended FV header.                            |               |
| 1.26       | Updates:                                                                                                                                                                   | January 2016  |
|            | Specification revision to 1.26                                                                                                                                             |               |
|            | Revised WORKSPACE wording for updated build system that can handle packages located outside of the                                                                         |               |
|            | WORKSPACE directory tree (refer to the TianoCore.org/ EDKII website for additional instructions on setting up a development environment).                                  |               |
|            | Added new system environment variables,                                                                                                                                    |               |
|            | PACKAGES_PATH and EDK_TOOLS_BIN, used by the build system.                                                                                                                 |               |
|            | Allow INF statements in FD regions.                                                                                                                                        |               |
|            | Clarified [UserExtensions] content in chapter 2 (to match implementation)                                                                                                  |               |
| 1.28       | Convert to GitBooks                                                                                                                                                        | March 2017    |
|            | [#426](https://bugzilla.tianocore.org/show_bug.cgi?id=426) IMAGE_TYPE_ID must be provided with value, FDF should mark it as required section                               |               |
