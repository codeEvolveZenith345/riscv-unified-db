# Parameter Extraction Methodology

## Extraction Pipeline

### Phase 1: Source Selection
The extraction strategy pivoted from configuration files to specification sources:
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
Parameters were cross-referenced against:
- **Configuration Files:** Checked `cfgs/` to see how these parameters are concretely instantiated (used as validation, not source).
- **Specification References:** Ensured every parameter links back to a specific section or quote.


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