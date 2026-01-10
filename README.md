# RISC-V Parameter Extraction Assignment

## Assignment Overview
This project documents the AI-assisted extraction of architectural parameters from the RISC-V Unified Database repository. The goal was to identify implementation-defined, optional, and configurable aspects of the architecture by analyzing configuration files and specification snippets.

## Documentation Structure

This submission consists of the following documents:

1. **01_llm_configuration.md** - LLM setup and capabilities
2. **02_prompt_engineering_journey.md** - Prompt development process
3. **03_repository_analysis.md** - Repository structure analysis
4. **04_extraction_methodology.md** - Technical extraction approach
5. **05_results_summary.md** - Results and statistics
6. **06_detailed_examples.md** - Worked examples
7. **parameters.yaml** - Complete extracted parameters (YAML format)

## Quick Start

**To understand the process:** Read documents 1-4 in order.
**To see results:** Jump to documents 5-6 and `parameters.yaml`.
**To replicate:** Follow the methodology in document 4 using the `example_rv64_with_overlay.yaml` file.

## Key Findings
- Extracted critical parameters regarding Memory Protection (PMP) and Endianness.
- Identified complex constraints involving hardware register implementation vs. logical entry counts.
- Validated parameters against source comments to ensure accuracy.

## Submission Checklist
- [x] LLM details documented
- [x] Prompt engineering process explained
- [x] Results in YAML format
- [x] Hallucination prevention demonstrated
- [x] Validation methodology described