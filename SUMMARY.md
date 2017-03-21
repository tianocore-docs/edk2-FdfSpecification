<!--- @file
  Summary

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

# Summary

* [EDK II Flash Description (FDF) File Specification](README.md#edk-ii-flash-description-fdf-file-specification)
* [1 Introduction](1_introduction/README.md#1-introduction)
  * [1.1 Overview](1_introduction/11_overview.md#11-overview)
  * [1.2 Terms](1_introduction/12_terms.md#12-terms)
  * [1.3 Related Information](1_introduction/13_related_information.md#13-related-information)
  * [1.4 Target Audience](1_introduction/14_target_audience.md#14-target-audience)
  * [1.5 Conventions Used in this Document](1_introduction/15_conventions_used_in_this_document.md#15-conventions-used-in-this-document)
* [2 FDF Design Discussion](2_fdf_design_discussion/README.md#2-fdf-design-discussion)
  * [2.1 Processing Overview](2_fdf_design_discussion/21_processing_overview.md#21-processing-overview)
  * [2.2 Flash Description File Format](2_fdf_design_discussion/22_flash_description_file_format.md#22-flash-description-file-format)
  * [2.3 [Defines] Section](2_fdf_design_discussion/23_[defines]_section.md#23-defines-section)
  * [2.4 [FD] Sections](2_fdf_design_discussion/24_[fd]_sections.md#24-fd-sections)
  * [2.5 [FV] Sections](2_fdf_design_discussion/25_[fv]_sections.md#25-fv-sections)
  * [2.6 [Capsule] Sections](2_fdf_design_discussion/26_[capsule]_sections.md#26-capsule-sections)
  * [2.7 [VTF] Sections](2_fdf_design_discussion/27_[vtf]_sections.md#27-vtf-sections)
  * [2.8 [Rule] Sections](2_fdf_design_discussion/28_[rule]_sections.md#28-rule-sections)
  * [2.9 [OptionRom] Sections](2_fdf_design_discussion/29_[optionrom]_sections.md#29-optionrom-sections)
* [3 EDK II FDF File Format](3_edk_ii_fdf_file_format/README.md#3-edk-ii-fdf-file-format)
  * [3.1 General Rules](3_edk_ii_fdf_file_format/31_general_rules.md#31-general-rules)
  * [3.2 FDF Definition](3_edk_ii_fdf_file_format/32_fdf_definition.md#32-fdf-definition)
  * [3.3 Header Section](3_edk_ii_fdf_file_format/33_header_section.md#33-header-section)
  * [3.4 [Defines] Section](3_edk_ii_fdf_file_format/34_[defines]_section.md#34-defines-section)
  * [3.5 [FD] Sections](3_edk_ii_fdf_file_format/35_[fd]_sections.md#35-fd-sections)
  * [3.6 [FV] Sections](3_edk_ii_fdf_file_format/36_[fv]_sections.md#36-fv-sections)
  * [3.7 [Capsule] Sections](3_edk_ii_fdf_file_format/37_[capsule]_sections.md#37-capsule-sections)
  * [3.8 [FmpPayload] Sections](3_edk_ii_fdf_file_format/38_[fmppayload]_sections.md#38-fmppayload-sections)
  * [3.9 [Rule] Sections](3_edk_ii_fdf_file_format/39_[rule]_sections.md#39-rule-sections)
  * [3.10 [VTF] Section](3_edk_ii_fdf_file_format/310_[vtf]_section.md#310-vtf-section)
  * [3.11 PCI OptionRom Section](3_edk_ii_fdf_file_format/311_pci_optionrom_section.md#311-pci-optionrom-section)
* [Appendix A Nt32Pkg Flash Description File](appendix_a_nt32pkg_flash_description_file.md#appendix-a-nt32pkg-flash-description-file)
* [Appendix B Common Error Messages](appendix_b_common_error_messages.md#appendix-b-common-error-messages)
  * [B.1 [FD] Syntax Errors:](appendix_b_common_error_messages.md#b1-fd-syntax-errors)
  * [B.2 [FV] Syntax Errors:](appendix_b_common_error_messages.md#b2-fv-syntax-errors)
  * [B.3 [CAPSULE] Syntax Errors:](appendix_b_common_error_messages.md#b3-capsule-syntax-errors)
  * [B.4 [Rule] Syntax Errors:](appendix_b_common_error_messages.md#b4-rule-syntax-errors)
* [Appendix C Reports](appendix_c_reports.md#appendix-c-reports)
---
* Tables
  * [Table 1 EDK Build Infrastructure Support Matrix](1_introduction/11_overview.md#table-1-edk-build-infrastructure-support-matrix)
  * [Table 2 Well-known Macro Statements](2_fdf_design_discussion/22_flash_description_file_format.md#table-2-well-known-macro-statements)
  * [Table 3 Using System Environment Variable](2_fdf_design_discussion/22_flash_description_file_format.md#table-3-using-system-environment-variable)
  * [Table 4 Reserved [Rule] Section Macro Strings](2_fdf_design_discussion/22_flash_description_file_format.md#table-4-reserved-rule-section-macro-strings)
  * [Table 5 Operator Precedence and Supported Operands](2_fdf_design_discussion/22_flash_description_file_format.md#table-5-operator-precedence-and-supported-operands)
---
* Figures
  * [Figure 1 EDK II Build Data Flow](2_fdf_design_discussion/21_processing_overview.md#figure-1-edk-ii-build-data-flow)
  * [Figure 2 EDK II Create Image Flow](2_fdf_design_discussion/21_processing_overview.md#figure-2-edk-ii-create-image-flow)
---
* Examples
  * [Example (EDK II FDF)](3_edk_ii_fdf_file_format/32_fdf_definition.md#example-edk-ii-fdf)
