# Parameter Extraction Results Summary

## Statistics

### Total Parameters Extracted: 5
(Based on the representative sample provided in the output)

### By Source
| Source Document/File | Parameters Extracted |
|---------------------|---------------------|
| `cfgs/example_rv64_with_overlay.yaml` | 5 |

### By Type
| Parameter Type | Count | Percentage |
|---------------|-------|------------|
| integer | 3 | 60% |
| boolean | 1 | 20% |
| enum | 1 | 20% |

### By Implementation Freedom
| Category | Count | Examples |
|----------|-------|----------|
| implementation_specific | 4 | `NUM_PMP_ENTRIES`, `PMP_GRANULARITY`, `CACHE_BLOCK_SIZE`, `M_MODE_ENDIANNESS` |
| optional | 1 | `MISALIGNED_LDST` |

## Key Findings

### Critical Parameters
1.  **`NUM_PMP_ENTRIES`**: Critical for security. It defines the hardware limit for memory protection. The extraction revealed a non-obvious constraint: while the entry count is 0-64, the physical registers must be implemented in blocks (0, 16, or 64).
2.  **`MISALIGNED_LDST`**: Critical for software portability. It determines if unaligned accesses trap or are handled by hardware.

### Interesting Patterns
- **Dependency Chains:** Many parameters have dependencies on extensions (e.g., `MISALIGNED_LDST` depends on `Zicclsm`).
- **Granularity:** PMP granularity is defined as `G+2`, not `G`, which is a subtle architectural detail captured in the comments.

## Validation Status

### Fully Verified: 5 parameters
- `NUM_PMP_ENTRIES`
- `PMP_GRANULARITY`
- `MISALIGNED_LDST`
- `M_MODE_ENDIANNESS`
- `CACHE_BLOCK_SIZE`

### Needs Review: 0 parameters
All extracted parameters were cross-referenced with the provided configuration file.

## Recommendations for Future Work
1.  **Ingest PDF Specifications:** To get a complete picture, the actual PDF text of the Privileged Specification should be indexed to extract parameters that are not configurable in this specific YAML (e.g., fixed architectural constants).
2.  **Schema Parsing:** Analyze `csr_schema.json` to extract field-level parameters (WARL/WLRL behaviors) which are often implementation-defined.