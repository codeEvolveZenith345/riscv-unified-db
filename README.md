# RISC-V Parameter Extraction Assignment

## Assignment Overview
This project documents the AI-assisted extraction of architectural parameters from the RISC-V Unified Database repository. The goal was to identify implementation-defined, optional, and configurable aspects of the architecture by analyzing configuration files and specification snippets.

## Documentation Structure

This submission consists of the following documents:

1. **00_correction_notice.md** - **READ FIRST:** Explanation of methodology correction.
2. **01_llm_configuration.md** - LLM setup and capabilities.
3. **02_prompt_engineering_journey.md** - Prompt development and error analysis.
4. **03_repository_analysis.md** - Analysis of source files (`arch/` vs `cfgs/`).
5. **04_extraction_methodology.md** - Revised technical extraction approach.
6. **05_results_summary.md** - Results from the corrected analysis.
7. **06_detailed_examples.md** - Worked examples from specifications.
8. **parameters.yaml** - Complete extracted parameters (YAML format).

## Quick Start

**To understand the correction:** Read `00_correction_notice.md` first
**To see the process:** Follow documents 1-4 in order
**To see results:** Jump to `parameters.yaml` and `05_results_summary.md`
**To replicate:** Use the prompts in `02_prompt_engineering_journey.md`

## Key Findings
 - **Methodology:** Configuration files (`cfgs/`) are outputs; Specification files (`arch/`) are inputs.
 - **Parameters:** Extracted key constraints on Cache Block Size and CSR Address Conventions from specification text.

## Submission Checklist
- [x] LLM details documented
- [x] Prompt engineering process explained
- [x] Results in YAML format
- [x] Hallucination prevention demonstrated
- [x] Validation methodology described
- [x] Correction of initial approach documented