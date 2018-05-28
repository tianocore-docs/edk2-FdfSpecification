<!--- @file
  2.2 Flash Description File Format

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

## 2.2 Flash Description File Format

The EDK II FDF file describes the layout of UEFI/PI compliant binary images
located within hardware, removable media or update capsules. The binary files
must already exist in order for the build tools to create the final images.
Some content, such as PCD definitions, may be used during the creation of
binary files.

### 2.2.1 Section Entries

To simplify parsing, the EDK II meta-data files continue using the INI format.
This style was introduced for EDK meta-data files, when only the Windows tool
chains were supported. It was decided that for compatibility purposes, that INI
format would continue to be used. EDK II formats differ from the defacto format
in that the semicolon ";" character cannot be used to indicate a comment.

Leading and trailing space/tab characters must be ignored.

It is recommended that duplicate section names be merged by tools.

This description file consists of sections delineated by section names enclosed
within square "[]" brackets. Section names are case-insensitive. The different
sections and their usage are described below. The text of a given section can
be used for multiple section names by separating the section names with a
comma. For example:

`[Rule.IA32.SEC, Rule.X64.SEC]`

The content below each section heading is processed by the parsing utilities in
the order that they occur in the file. The precedence for processing these
architecture section tags is from right to left, with sections defining an
architecture having a higher precedence than a section which uses "common" (or
no architecture extension) as the architecture modifier.

**********
**Note:** Content such as filenames, directory names, MACROs and C variable
names within a section IS case sensitive. IA32, Ia32 and ia32 within a section
in a directory or file name are processed as separate items. (Refer to Naming
Conventions below for more information on directory and/or file naming.)
**********

Sections are terminated by the start of another section or the end of the file.

Comments are not permitted between square brackets of a section specifier.

Duplicate sections (two sections with identical section tags) will be merged by
tools, with the second section appended to the first.

The EDK II Reference build system will ignore [UserExtensions] sections in the
FDF file.

The `[Rules]` and `[VTF]` sections allow the use of architectural modifiers,
however the content must specific to an individual architecture or common to
all architectures.

Therefore, the architectural sections take priority over common section
content. The cannot be combined with a 'common' architecture.

The `[FD]`, `[FV]`, `[Capsule]` and `[OptionRom]` sections cannot specify
architectural modifiers.

### 2.2.2 Comments

The hash "#" character indicates comments in the FDF file. In line comments
terminate the processing of a line. In line comments must be placed at the end
of the line.

Only `BsBaseAddress = 0x0000C1000` in the following example is processed by
tools; the remainder of the line is ignored:

`BsBaseAddress = 0x0000C100 # set boot driver base address`

**********
**Note:** Blank lines and lines that start with the hash # character must be
ignored by tools.
**********

Hash characters appearing within a quoted string are permitted, with the string
being processed as a single entity. The following example must handle the
quoted string as single element by tools.

`UI = " # Copyright 2007, NoSuch, LTD. All rights reserved."`

Comments are terminated by the end of line.

If a hash "#" character is required in a value field, the value field must be
encapsulated by double quotation marks.

### 2.2.3 Valid Entries

Processing of a line is terminated by the end of the line.

Processing of the line is also terminated if a comment is encountered.

Items in quotation marks are treated as a single token and have the highest
precedence. Items encapsulated in parenthesis are also treated as tokens, with
embedded tokens being processed first. All other processing occurs from left to
right.

In the following example, B - C is processed first, then result is added to A
followed by adding 2; finally 3 is added to the result.

`(A + (B - C) + 2) + 3`

In the next example, A + B is processed first, then C + D is processed and
finally the two results are added.

`(A + B) + (C + D)`

Space and tab characters are permitted around field separators.

### 2.2.4 Naming Conventions

The EDK II build infrastructure is supported under Microsoft* Windows*, Linux*
and MAC OS/X operating systems. As a result of multiple environment support,
all directory and file names are case sensitive.

* The use of special characters in directory names and file names is restricted
  to the dash, underscore, and period characters, respectively "-", "_", and
  ".".

* Period characters may not be followed by another period character. File and
  Directory names must not start with "./", "." or "..".

* Directory names and file names must not contain space or tab characters.

* Directory Names must only contain alphanumeric characters, underscore or dash
  characters and it is recommended that they start with an alpha character.

* Additionally, all EDK II directories that are architecturally dependent must
  use a name with only the first character capitalized. Ia32, Ipf, X64 and Ebc
  are valid architectural directory names. IA32, IPF and EBC are not acceptable
  directory names, and may cause build breaks. From a build tools perspective,
  an IA32 directory name is not equivalent to Ia32 or ia32 An architecture used
  in a directory name must be listed in a section that uses the architecture
  modifier. If a common section contains filenames that have directories with
  architecture modifiers, the file will be processed for all architectures, not
  just the architecture specified in the directory name.

Space Characters in filenames: The build tools must be able to process the tool
definitions file: _tools_def.txt_ (describing the location and flags for
compiler and user defined tools), which may contain space characters in paths
on Windows* systems. The _tools_def.txt_ file is the only file the permits the
use of space characters in the directory name.

The EDK II Coding Style specification covers naming conventions for use within
C Code files, and as well as specifying the rules for directory and file names.
This section is meant to highlight those rules as they apply to the content of
the FDF files.

Architecture keywords (`IA32`, `IPF`, `X64` and `EBC`) are used by build tools
and in metadata files for describing alternate threads for processing of files.
These keywords must not be used for describing directory paths. Additionally,
directory names with architectural names (Ia32, Ipf, X64 and Ebc) do not
automatically cause the build tools or meta-data files to follow these
alternate paths. Directories and Architectural Keywords are similar in name
only.

For clarity, this specification will use all upper case letters when describing
architectural keywords, and the directory names with only the first letter in
upper case.

All directory paths within EDK II FDF files must use the "/" forward slash
character to separate directories as well as directories from filenames.
Example:

`C:/Work/Edk2/edksetup.bat`

File names must also follow the same naming convention required for
directories. No white space characters are permitted. The special characters
permitted in directory names are the only special characters permitted in file
names.

The relative path is relative to the directory the FDF file must be used,
unless otherwise noted. Use of "..", "./" and "../" in the path of the file is
strictly prohibited. All files listed in this section must reside in the
directory this INF file is in or in sub-directories of this directory.

### 2.2.5 !include Statements

The `!include` statement may appear within an EDK II FDF file. The included
file content must match the content type of the current section definition,
contain complete sections, or combination of both.

The argument of this statement is a filename. The file is relative to the
directory that contains this DSC file, and if not found the tool must attempt
to find the file relative to paths listed in the system environment variables,
`$(WORKSPACE)`, `$(PACKAGES_PATH)`, `$(EFI_SOURCE)`, `$(EDK_SOURCE)`, and
`$(ECP_SOURCE)`.  If the file is not found after testing for the possible
combinations, the parsing tools must terminate with an error.

Macros, defined in this FDF file or in the DSC file, are permitted in the
path or file name of the !include statement, as these files are included prior
to processing the file for macros. The system environment variables,
`$(WORKSPACE)`, `$(EDK_SOURCE)`, `$(EFI_SOURCE)`, and `$(ECP_SOURCE)` may also
be used; only these system environment variables are permitted to start the
path of the included file.

Statements in !include files must not break the integrity of the FDF file, the
included file is read in by tools in the exact position of the file, and is
functionally equivalent of copying the contents of the included file and
inserting (paste) the content into the DSC file.

### 2.2.6 Macro Statements

Variables (or macros) used within the FDF file are typically used for path
generation for locating files, used in conditional statements or values for
PCDs.

Token names (reserved words defined in the EDK II meta-data file
specifications) cannot be used as macro names. As an example, using
PLATFORM_NAME as a macro name is not permitted, as it is a token defined in the
DSC file's `[Defines]` section.

MACROS cannot be used to define keywords, statements, nor any other tokens
defined in this spec.

All elements of a macro definition must appear on a single line; the meta-data
file formats do not permit entries to span multiple lines.

Escape character sequences are only permitted within a quoted string. Quoted
strings are treated as literals, escape character sequences within quoted
strings will not be expanded by the tools.

Macros that appear in a double quoted string will not be expanded by parsing
tools. The expectation is that these macros will be expanded by scripting tools
such as make or nmake.

The format and usage for the macro statements is:

`DEFINE MACRO = Path`

Any portion on a path or path and filename can be defined by a macro.

When assigning a string value to a macro, the string must follow the C format
for specifying a string, as shown below:

```
DEFINE MACRO1 = "SETUP"
DEFINE MACRO2 = L"SETUP"
```

When assigning a numeric value to a macro, the number may be a decimal, integer
or hex value, as shown below:

```
DEFINE MACRO1 = 0xFFFFFFFF
DEFINE MACRO2 = 2.3
DEFINE MACRO3 = 10
```

The format for usage of a Macro varies. When used as a value, the Macro name
must be encapsulated by "$(" and ")" as shown below:

`$(MACRO)/filename.foo`

When a macro is tested in a conditional directive statement, determining
whether it has been defined or undefined uses the following format:

`!ifdef MACRO`

**********
**Note:** For backward compatibility, tools may allow $(MACRO) in the !ifdef
and !ifndef statements. This functionality may disappear in future releases,
therefore, it is recommended that platform integrators update their DSC files
if they also alter other content.
**********

When using string comparisons of Macro elements to string literals, the format
of the conditional directive must be:

`!if $(MACRO) == "Literal String"`

**********
**Note:** For backward compatibility, tools may allow testing literal strings
that are not encapsulated by double quotation marks. This functionality may
disappear in future releases, therefore, it is recommended that platform
integrators update their DSC files if they also alter other content.
**********

When testing Macro against another Macro:

`!if $(MACROALPHA) == $(MACROBETA)`

When testing a Macro against a value:

`!if $(MACRONUM) == 2`

or

`!if $(MACROBOOL) == TRUE`

When used in either the `!if` or `!elseif` statements or in an expression used
in a value field, a macro that has not been defined has a value of 0.

Macro Definition statements that appear within a section of the file (other
than the `[Defines]` section) are scoped to the section they are defined in. If
the Macro statement is within the `[Defines]` section, then the Macro is common
to the entire file, with local definitions taking precedence (if the same MACRO
name is redefined in subsequent sections, then that MACRO value is local to
only that section.)

Macros are evaluated where they are used in conditional directives or other
statements, not where they are defined. It is recommended that tools break the
build and report an error if an expression cannot be evaluated.

Any defined MACRO definitions will be expanded by tools when they encounter the
entry in the section except when the macro is within double quotation marks in
build options sections. The expectation is that macros in the quoted values
will be expanded by external build scripting tools, such as nmake or gmake;
they will not be expanded by the build tools. If a macro that is not defined is
used in locations that are not expressions (where the tools would just do macro
expansion as in path names in an INF statement in the `[FV]` section), nothing
will be emitted. If the macro, MACRO1, has not been defined, then:

`INF $(MACRO1)GraphicsDriver.inf`

After macro expansion, the logical result would be equal to:

`INF GraphicsDriver.inf`

It is recommended that tools remove any excess space characters when processing
these types of lines.

Additionally, pre-defined global variables may be used in the body of the FDF
file. The following is an example of using pre-defined variables:


`FILE = $(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/FV/Microcode.bin`

The following table lists the global variables permitted in generating a path
statement as well as variables that can be passed as an argument for a rule.

Macro statements defined the FDF file are local to the file. Macro names used
in values, $(Macro), must be defined in either the DSC file or the FDF file,
and must be defined before they can be used. Macro values specified on the
command-line over ride all definitions of that Macro.

The `EDK_GLOBAL` macros can only be defined in the DSC file, however they are
considered global during the processing of the DSC, FDF and EDK INF files.

Global variables that may be used in this file are listed in the _Well-known
Macro Statements_ table while the format of the System Environment variables
that may be used in EDK II DSC and FDF files are in the next table.

###### Table 2 Well-known Macro Statements

| Exact Notation        | Derivation                                                                                                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `$(WORKSPACE)`        | System Environment Variable.                                                                                                                                                                                                         |
| `PACKAGES_PATH`       | System Environment Variable that cannot be used in EDK II meta-data Files. The build system will automatically detect if this variable is present and use directories listed in this variable as if they were listed in $(WORKSPACE) |
| `$(EDK_SOURCE)`       | System Environment Variable.                                                                                                                                                                                                         |
| `$(EFI_SOURCE)`       | System Environment Variable.                                                                                                                                                                                                         |
| `$(EDK_TOOLS_PATH)`   | System Environment Variable                                                                                                                                                                                                          |
| `EDK_TOOLS_BIN`       | System Environment Variable that cannot be used in EDK II meta-data Files.                                                                                                                                                           |
| `$(ECP_SOURCE)`       | System Environment Variable                                                                                                                                                                                                          |
| `$(OUTPUT_DIRECTORY)` | Tool parsing from either the DSC file or via a command line option. This is typically the Build/Platform name directory created by the build system in the EDK II WORKSPACE                                                          |
| `$(BUILD_NUMBER)`     | Tool parsing from either an EDK INF file or the EDK II DSC file's `BUILD_NUMBER` statement. The EDK II DSC file's `BUILD_NUMBER` takes precedence over an EDK INF file's                                                             |
|                       | `BUILD_NUMBER` if and only if the EDK II DSC specifies a                                                                                                                                                                             |
|                       | `BUILD_NUMBER`.                                                                                                                                                                                                                      |
|                       | Future implementation may allow for setting the                                                                                                                                                                                      |
|                       | `BUILD_NUMBER` variable on the build tool's command line.                                                                                                                                                                            |
| `$(NAMED_GUID)`       | Tool parsing `FILE_GUID` statement in the INF file.                                                                                                                                                                                  |
| `$(MODULE_NAME)`      | Tool parsing the `BASE_NAME` statement in the INF file.                                                                                                                                                                              |
| `$(INF_VERSION)`      | Tool parsing the `VERSION_STRING` statement in the INF file.                                                                                                                                                                         |
| `$(INF_OUTPUT)`       | The OUTPUT directory created by the build system for each EDK II module.                                                                                                                                                             |
| `$(TARGET)`           | Valid values are derived from INF, DSC, _target.txt_ and _tools_def.txt_. FDF parsing tools may obtain these values from command-line options.                                                                                       |
| `$(TOOL_CHAIN_TAG)`   | Valid values are derived from INF, DSC, _target.txt_ and _tools_def.txt_. FDF parsing tools may obtain these values from command-line options.                                                                                       |
| `$(ARCH)`             | Valid values are derived from INF, DSC, _target.txt_ and _tools_def.txt_. FDF parsing tools may obtain these values from command-line options.                                                                                       |

**********
**Note:** System environment variables may be referenced, however their values
must not be altered.
**********

###### Table 3 Using System Environment Variable

| Macro Style Used in Meta-Data files | Windows Environment Variable | Linux & OS/X Environment Variable |
| ------------------- | ------------------ | ----------------- |
| `$(WORKSPACE)`      | `%WORKSPACE%`      | `$WORKSPACE`      |
| `$(EFI_SOURCE)`     | `%EFI_SOURCE%`     | `$EFI_SOURCE`     |
| `$(EDK_SOURCE)`     | `%EDK_SOURCE%`     | `$EDK_SOURCE`     |
| `$(EDK_TOOLS_PATH)` | `%EDK_TOOLS_PATH%` | `$EDK_TOOLS_PATH` |
| `$(ECP_SOURCE)`     | `%ECP_SOURCE%`     | `$ECP_SOURCE`     |

The system environment variables, `PACKAGES_PATH` and `EDK_TOOLS_BIN`, are not
permitted in EDK II meta-data files.

Macros defined in the FDF file are local to the FDF file. They are also
positional in nature, with later definitions overriding previous definitions
for the remainder of the file.

Macros may be used in other macros or in conditional directive statements.
Macros can be defined or used in the `[Defines]`, `[FD]`, `[FV]`, `[Capsule]`
and `[OptionROM]` sections.

Macros defined in common sections may be used in the architecturally modified
sections of the same section type. Macros defined in architectural sections
cannot be used in other architectural sections, nor can they be used in the
common section. Section modifiers in addition to the architectural modifier
follow the same rules as architectural modifiers. Macros must be defined before
they can be used.

Macro evaluation is done at the time the macro is used in an expression,
conditional directive or value field, not when a macro is defined. Macros in
quoted strings will not be expanded by parsing tools; all other macro values
will be expanded, without evaluation, as other elements of the build system
will perform any needed tests.

#### Example

```ini
[FV.common]
  FILE FV_IMAGE = EF41A0E1-40B1-481f-958E-6FB4D9B12E76 {
    FvAlignment           = 512K
    WRITE_POLICY_RELIABLE = TRUE
    SECTION GUIDED 3EA022A4-1439-4ff2-B4E4-A6F65A13A9AB {
      SECTION FV_IMAGE = Dxe {
        APRIORI DXE {
          INF $(WORKSPACE)/a/a.inf
          INF $(EDK_SOURCE/a/c/c.inf
          INF $(WORKSPACE)/a/b/b.inf
        }
        INF a/d/d.inf
        ...
      }
    }
  }
```

The `[Rule]` section of the FDF file allows for using macros that are also
defined for the EDK II _build_rule.txt_ file. The following table provides the
list of these pre-defined macro statements. These macros should never be
expanded during the initial parsing phase, as other tools use these macros to
generate the UEFI and PI compliant images. Additionally, the macro names should
never be set by the user, as these values are filled in by the build tools
based other file and base names.

###### Table 4 Reserved `[Rule]` Section Macro Strings

| Variable String | Description                                                                      |
| --------------- | -------------------------------------------------------------------------------- |
| "${src}"        | Source file(s) to be built (full path)                                           |
| "${s_path}"     | Source file directory (absolute path)                                            |
| "${s_dir}"      | Source file relative directory within a module.                                  |
|                 | Note: ${s_dir} is always equals to "." if source file is given in absolute path. |
| "${s_name}"     | Source file name without path.                                                   |
| "${s_base}"     | Source file name without extension and path.                                     |
| "${s_ext}"      | Source file extension.                                                           |
| "${dst}"        | Destination file(s) built from ${src} (full path)                                |
| "${d_path}"     | Destination file directory (absolute path)                                       |
| "${d_name}"     | Destination file name without path.                                              |
| "${d_base}"     | Destination file name without extension and path                                 |
| "${d_ext}"      | Destination file extension                                                       |

The `SET` and `DEFINE` statements are not permitted in the `[Rule]` section.

### 2.2.7 PCD Names

Unique PCDs are identified using the format to identify the named PCD:

`PcdTokenSpaceGuidCName.PcdCName`

The PCD's Name (`PcdName`) is defined as PCD Token Space Guid C name and the
PCD C name - separated by a period "." character. PCD C names are used in C
code and must follow the C variable name rules.

A PCD's values are positional with in the FDF file, and may be set by either
the automatic setting grammar defined in this specification, or through `SET`
statements. Once the PCD's value has been defined, it may be used anywhere
within the FDF file; they are not limited to sections that they are defined in.
PCD values may be absolute, values defined by macros, or expressions.

Refer to the _EDK II Build Specification_, Pre-Build AutoGen Stage chapter for
PCD processing rules.

### 2.2.8 Conditional Statements (!if...)

Conditional statements are used by the build tools preprocessor function to
include or exclude statements in the FDF file.

Most section definitions in the EDK II meta-data files have architecture
modifiers in the section tags. Use of architectural modifiers in the section
tag is the recommended method for specifying architectural differences. Some
sections do not have architectural modifiers and there are some unique cases
where having a method for specifying architectural specific items would be
valuable, hence the ability to use these values.

Statements are prefixed by the exclamation "!" character. Conditional
statements may appear anywhere within the FDF file.

**********
**Note:** A limited number of statements are supported. This specification does
not support every conditional statement that C programmers are familiar with.
**********

Supported statements are:

`!ifdef, !ifndef, !if, !elseif, !else and !endif`

Refer to the Macro Statement section for information on using Macros in
conditional directives.

When using the `!ifdef` or `!ifndef`, the macro name must be used; the macro
name must not be encapsulated between `$(` and `)`. (For backward
compatibility, macro names encapsulated between `$(` and `)` are allowed in FDF
files that have `FDF_SPECIFICATION` versions less that `0x00010016`.)

When using a marco in the `!if` or `!elseif` conditionals, the macro name must
be encapsulated between `$(` and `)`.

A macro that is not defined has a default value of 0 (`FALSE`) when used in a
conditional comparison statement.

It is recommended you not use PCDs in the `!ifdef` or `!ifndef` statements.
Using a PCD in an `!ifdef` or `!ifndef` statement will cause the build to break
with an error message.

When using a PCD in the `!if` or `!elseif` conditionals, the PCD name
`(TokenSpaceGuidCName.PcdCname)` must be used; the PCD name must not be
encapsulated between "$(" and ")". Do not encapsulate the PCD name in the
"$(" and ")" required for macro values or in the "PCD(" and ")" used in `[FV]`
or `[Capsule]` sections as shown in the example below.

```
!if ( gTokenSpaceGuid.PcdCname == 1 ) AND ( $(MY_MACRO) == TRUE )
DEFINE FOO=TRUE
!endif
```

If the PCD is a string, only the string needs to be encapsulated by double
quotation marks, while a Unicode string can have the double quoted string
prefixed by "L", as in the following example:

```
!if gTokenSpaceGuid.PcdCname == L"Setup"
DEFINE FOO=TRUE
!endif
```

When used in `!if` and `!elseif` conditional comparison statements, it is the
value of the Macro or the PCD that is used for testing, not the name of the
macro or PCD.

Strings can only be compared to strings of a like type (testing an ASCII string
against a Unicode format string must fail), numbers can only be compared
against numbers and boolean objects can only evaluate to `TRUE` or `FALSE`. See
the Operator Precedence table, in the Expressions section below for a list of
restrictions on comparisons.

Using macros in conditional directives that contain flags for use in the
`[BuildOptions]` sections of DSC files is not recommended.

If a PCD is used in a conditional statement, the value must first come from the
FDF file, then from the DSC file. If the value cannot be determined from these
two locations, the build system should break with an error message.

**********
**Note:** PCDs, used in conditional directives, must be defined and the value
set in either the FDF or DSC file in order to be used in a conditional
statement; values from INF or DEC files are not permitted.
**********

The following is an example of conditional statements.

```ini
!if ("MSFT" in $(FAMILY)) or ("INTEL" in $(FAMILY))
... statements
!elseif $(FAMILY) == "GCC"
... statements
!endif

!ifdef FOO
  !ifndef BAR
    # FOO defined, BAR not defined
  !else
    # FOO defined, BAR is defined
  !endif
!elseif $(BARFOO)
  # FOO is not defined, BARFOO is defined as TRUE
!elseif $(BARFOO) == "FOOBAR"
  # FOO is not defined, BARFOO is defined as FOOBAR
!else
  # FOO is not defined while BARFOO is either NOT defined or does not
  # equal "FOOBAR"
!endif
```

### 2.2.9 !error Statement

The `!error` statement may appear within any section of EDK II FDF file. The
argument of this statement is an error message, it causes build tool to stop
at the location where the statement is encountered and error message following
the `!error` statement is output as a message.

The following example show the valid usage of the `!error` statement.

```ini
!if $(FEATURE_ENABLE) == TRUE
  !error "unsupported feature!"
!endif
```

### 2.2.10 Expressions

Expressions can be used in conditional directive comparison statements and in
value fields for Macros and PCDs in the DSC and FDF files.

Expressions follow C relation, equality, logical and bitwise precedence and
associativity. Not all C operators are supported, only operators in the
following list can be used.

**********
**Note:** Due to the flexibility of the build system, a new operator, "IN"
has been added that can be used to test whether an element is in a list. The
format for this is `<Value> IN <MACRO_LIST>`, where `MACRO_LIST` can only be
one of `$(ARCH)`, `$(FAMILY)`, `$(TOOL_CHAIN_TAG)` and `$(TARGET)`.
**********

Use of parenthesis is encouraged to remove ambiguity.

When comparing a string to a number or boolean value, a warning message will be
emitted. In this case, the tools will always evaluate the expression using the
"==" and "EQ" operators to `FALSE`, using the "!=" and "NE" operators
to `TRUE`; other operator comparisons are not supported, and will cause the
build system to terminate with an error message. Comparing a number to a
boolean value (no warning message will be emitted) will be evaluated normally,
however, only the numeric value of 1 will be considered a match to the "=="
and "EQ" operators against a boolean value of `TRUE`.

Additional scripting style operators may be used in place of C operators as
shown in the table below.

###### Table 5 Operator Precedence and Supported Operands

| Operator                                     | Use with Data Types | Notes                                                                                                                                                                                                                         | Priority |
| -------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `or`, `OR`, <code>&#124;&#124;</code>        | Number, Boolean     |                                                                                                                                                                                                                               | Lowest   |
| `and`, `AND`, `&&`                           | Number, Boolean     |                                                                                                                                                                                                                               |          |
| <code>&#124;</code>                          | Number, Boolean     |                                                                                                                                                                                                                               |          |
| `^`, `xor`, `XOR`                            | Number, Boolean     | Exclusive OR                                                                                                                                                                                                                  |          |
| `&`                                          | Number, Boolean     | Bitwise AND                                                                                                                                                                                                                   |          |
| `==`, `!=`, `EQ`, `NE`, `IN`                 | All types           | The IN operator can only be used to test a unary object for membership in a list                                                                                                                                              |          |
|                                              |                     | Space characters must be used before and after the letter operators Strings compared to boolean or numeric values using "==" or "EQ" will always return FALSE, while using the "!=" or "NE" operators will always return TRUE |          |
| `<=`, `>=`, `<`, `>`, `LE`, `GE`, `LT`, `GT` | All                 | Space characters must be used before and after the letter operators                                                                                                                                                           |          |
| `+`, `-`                                     | Number, Boolean     | Cannot be used with strings - the system does not automatically do concatenation. Tools should report a warning message if these operators are used with both a boolean and number value                                      |          |
| `!`, `not`, `NOT`                            | Number, Boolean     |                                                                                                                                                                                                                               | Highest  |

The `IN` operator can only be used to test a literal string against elements in
the following global variables:

**_$(FAMILY)_**

`$(FAMILY)` is considered a list of families that different `TOOL_CHAIN_TAG`
values belong to. The `TOOL_CHAIN_TAG` is defined in the `Conf/target.txt` or
on the command-line. The `FAMILY` is associated with the `TOOL_CHAIN_TAG` in
the `Conf/ tools_def.txt` file (or the `TOOLS_DEF_CONF` file specified in the
`Conf/target.txt` file) file. While different family names can be defined,
`ARMGCC`, `GCC`, `INTEL`, `MSFT`, `RVCT`, `RVCTCYGWIN` and `XCODE` have been
predefined in the `tools_def.txt` file.

**_$(ARCH)_**

`$(ARCH)` is considered the list of architectures that are to be built, that
were specified on the command line or come from the `Conf/target.txt` file.

**_$(TOOL_CHAIN_TAG)_**

`$(TOOL_CHAIN_TAG)` is considered the list of tool chain tag names specified on
the command line

**_$(TARGET)_**

`$(TARGET)` is considered the list of target (such as `DEBUG`, `RELEASE` and
`NOOPT`) names specified on the command line or come from the `Conf/target.txt`
file.

For logical expressions, any non-zero value must be considered `TRUE`.

Invalid expressions must cause a build break with an appropriate error message.
