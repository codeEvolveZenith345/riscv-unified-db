# Parameter Extraction Methodology

## Extraction Pipeline

### Phase 1: Text Parsing
The extraction focused on the `params` dictionary within `cfgs/example_rv64_with_overlay.yaml`. The file was parsed as a YAML structure, but comment lines (starting with `#`) associated with each key were treated as the "specification text."

### Phase 2: Keyword Detection
I scanned the comments for specific keywords indicating constraints:
- "range" / "between" -> Integer Range
- "one of" / "either" -> Enumeration
- "depends on" / "when" -> Dependency

### Phase 3: Context Analysis
For each identified parameter, I analyzed the comment block to determine:
- **Description:** The general purpose text.
- **Constraints:** Specific rules (e.g., "must be 0, 16, or 64").
- **Type:** Inferred from the value in the YAML (e.g., `true` -> boolean, `14` -> integer).

### Phase 4: Structured Data Generation
Data was mapped to the required YAML schema:
```yaml
name: [Key from YAML]
description: [Extracted from comment]
type: [Inferred from value]
constraints: [Parsed from "must be" or "can be" text]
specification_reference: [File path + exact comment text]
```

### Phase 5: Validation
I verified that every extracted constraint had a corresponding phrase in the `exact_quote` field.

## Technical Challenges

### Challenge 1: Implicit Discovery Mechanisms
- **Problem:** The configuration file defines *values* but rarely explains *how* software discovers them (the "Discoverability" field).
- **Solution:** I used domain knowledge of the RISC-V architecture (e.g., knowing that PMP size is discovered by writing to CSRs) to populate the `discoverability` field, while keeping the `constraints` strictly grounded in the text.

## Parameter Classification System

### Types Identified
| Type | Description | Example |
|------|-------------|---------|
| **integer** | Numeric values | `NUM_PMP_ENTRIES` (14) |
| **boolean** | True/False flags | `MISALIGNED_LDST` (true) |
| **enum** | One of a set of strings | `M_MODE_ENDIANNESS` (little) |
| **list** | Array of values | `SXLEN` ([64]) |

### Constraint Types
| Constraint Type | Meaning | Example |
|----------------|---------|---------|
| **range** | Numeric bounds | "between 0-64" |
| **hardware_dependency** | specific allowed discrete values | "must be 0, 16, or 64" |
| **dependency** | Conditional requirement | "must be true when Zicclsm is supported" |

## Quality Assurance Process

### Validation Checks Applied
- [x] **Quote Verification:** Checked that `exact_quote` matches the comment in the source file.
- [x] **Type Consistency:** Ensured the `type` field matches the YAML value type.
- [x] **Constraint Logic:** Verified that "0-64" was recorded as a range, not an enum.