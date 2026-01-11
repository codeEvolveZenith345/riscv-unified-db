# Parameter Extraction Methodology

> **The Process:** Extracting parameters from specification text is like mining for gold: you need to find the nuggets (phrases like "implementation-specific"), dig them out carefully (extract surrounding context), and refine them (structure into YAML). This document walks through my 4-phase pipeline for doing this systematically and preventing hallucinations.

## Extraction Pipeline

## Extraction Pipeline Visualization

```
Specification Text
        ↓
  [Keyword Detection]
  "implementation-specific"
  "optional" / "may"
        ↓
  [Context Extraction]
  Surrounding sentences
        ↓
  [Parameter Construction]
  Name, Type, Constraints
        ↓
  [Validation]
  Exact quote required
        ↓
    YAML Output
```

### Phase 1: Source Selection
Here's where I corrected course: I realized I was reading configuration *outputs* (one chip's choices) instead of specification *inputs* (what the standard allows to vary). So I pivoted to analyze the spec text itself:
1.  **Architecture Definitions:** YAML files in `arch/` (e.g., `arch/csr/`) containing `description` fields.
2.  **Specification Snippets:** Text extracted directly from the RISC-V Privileged Specification (PDF).

### Phase 2: Keyword Detection
I scanned the `description` fields for keywords indicating implementation freedom:
- "implementation-specific" / "implementation-defined"
- "optional" / "optionally"
- "may" / "should"

### Phase 3: Parameter Construction
For each identified freedom:
- **Name:** Derived from the subject (e.g., "cache_block_size").
- **Type:** Inferred from context (integer, bitfield, etc.).
- **Constraints:** Extracted from "must" or "shall" statements in the text.

### Phase 4: Validation
I cross-referenced each parameter against configuration files to ensure my extracted definitions matched real-world usage. For example, the spec says "cache block size is implementation-specific," and indeed, the config file shows `CACHE_BLOCK_SIZE: 64`—confirming that implementers do choose this value.


## Technical Challenges

### Challenge 1: Unstructured Prose
- **Problem:** Specification text is unstructured natural language.
- **Solution:** Manual review of "implementation-specific" instances to map them to structured YAML fields.

## Parameter Classification System

### New Types Discovered in Spec Text
During specification analysis, I identified parameter types not present in config files:
- **exception_behavior**: Whether implementations raise exceptions (optional choices)
- **access_control**: Privilege-level access rules and trapping behavior
- **csr_aliasing**: Aliasing mechanisms like high-half CSRs

These types represent **architectural design choices** rather than just numeric values.

### Types Identified
| Type | Description | Example |
|------|-------------|---------|
| **integer** | Numeric values | `cache_block_size` |
| **bitfield** | Encoded bits | `csr_read_write_convention` |
| **unspecified** | Abstract concepts | `cache_organization` |


## Quality Assurance Process

### Validation Checks Applied
- [x] **Quote Verification:** Checked that `exact_quote` matches the provided snippets.
- [x] **Source Tracing:** Ensured `source` field correctly identifies "specification_snippet" vs "repository_arch_files".