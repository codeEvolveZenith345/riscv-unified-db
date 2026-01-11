# Parameter Extraction Results Summary

## Statistics

### Total Parameters Extracted: 12

### By Source
| Source Type | Count |
|-------------|-------|
| Specification Snippets | 11 |
| Repository Arch Files | 1 |

### By Type
| Parameter Type | Count | Percentage |
|----------------|-------|------------|
| bitfield_behavior | 3 | 25% |
| access_control | 2 | 17% |
| bitfield | 2 | 17% |
| integer | 1 | 8% |
| unspecified | 1 | 8% |
| exception_behavior | 1 | 8% |
| address_range | 1 | 8% |
| csr_aliasing | 1 | 8% |

### By Implementation Freedom
| Category | Count | Examples |
|----------|-------|----------|
| implementation_specific | 5 | `cache_block_size`, `wpri_field_reset_value` |
| convention_based | 3 | `csr_read_write_convention`, `high_half_csr_implementation` |
| optional | 2 | `wlrl_illegal_value_handling`, `privileged_csr_access_trapping` |
| recommended | 1 | `debug_csr_visibility` |
| reserved_for_custom | 1 | `custom_csr_address_allocation` |

## Key Findings

### Critical Parameters
1.  **`cache_block_size`**: Fundamental to memory hierarchy. Defined as implementation-specific in the spec, but must be power-of-two/NAPOT.
2.  **CSR Address Conventions**: The spec enforces strict encoding rules (bits 11:10 for RW/RO) even though the specific CSRs present are implementation-defined.
3.  **Field Behaviors (WARL/WLRL)**: The spec defines complex "deterministic but arbitrary" constraints for illegal values, which are critical for verification.

### Methodology Shift
The analysis successfully pivoted from reading *outputs* (configuration values) to reading *inputs* (specification definitions), resulting in a more accurate model of the RISC-V design space.

## Validation Status

### Fully Verified: 12 parameters
All extracted parameters include direct quotes from the RISC-V Privileged Specification or architecture files.

## Recommendations for Future Work
1.  **Full Spec Ingestion:** Ingest the full text of the RISC-V Unprivileged and Privileged specs to capture all "implementation-defined" behaviors.
2.  **Automated Arch Scanning:** Write scripts to grep `arch/**/*.yaml` for "implementation-specific" keywords.