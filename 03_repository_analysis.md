# RISC-V Unified Database Repository Analysis

## Repository Overview
- **URL:** https://github.com/riscv-software-src/riscv-unified-db
- **Purpose:** A unified database for RISC-V architectural definitions, configurations, and generation tools.
- **Analysis Date:** January 10, 2026

## Directory Structure

### Key Directories
```
riscv-unified-db/
├── spec/
│   └── schemas/          # JSON Schemas defining the data model (e.g., csr_schema.json)
├── cfgs/
│   └── example_rv64_with_overlay.yaml  # Primary source of architectural parameters
├── backends/
│   ├── generators/       # Scripts to generate code (Go, C, SV)
│   └── profile/          # Tools for profile documentation
└── .github/              # CI/CD workflows
```

### Specification Sources Identified

#### 1. Configuration Files (Primary Source in Context)
| File Path | Purpose | Parameters Defined |
|-----------|---------|-------------------|
| `cfgs/example_rv64_with_overlay.yaml` | Fully specified RV64 system example | ~50+ parameters in `params` section |

#### 2. Schema Definitions
| Schema File | Defines | Relevant to Task? |
|-------------|---------|-------------------|
| `spec/schemas/csr_schema.json` | Structure of CSRs, fields, and access types | Yes, defines meta-parameters like `reset_value` |

#### 3. ISA Manual References
*Note: Actual ISA manual text files (PDF/Markdown) were not present in the provided file context. Analysis relied on the text snippets provided in the prompt and comments within configuration files.*

## Parameter Identification Strategy

### Keyword Search Results (within `example_rv64_with_overlay.yaml`)
- **"must be"**: Found in `NUM_PMP_ENTRIES` ("must be 0, 16, or 64") and `MISALIGNED_LDST` ("must be true when Zicclsm is supported").
- **"can be"**: Found in `M_MODE_ENDIANNESS` ("Can be one of: little, big, dynamic").
- **"implementation"**: Found in `MISALIGNED_LDST` ("whether or not the implementation supports...").

### Sources of Parameters
1.  **Primary:** `cfgs/example_rv64_with_overlay.yaml` (Key-value pairs in `params` block).
2.  **Secondary:** Comments immediately preceding keys in the YAML file.

## Coverage Assessment

### Specification Sections Covered
- ✅ **PMP Configuration:** Covered via `NUM_PMP_ENTRIES` and `PMP_GRANULARITY`.
- ✅ **Endianness:** Covered via `*_MODE_ENDIANNESS` parameters.
- ✅ **Cache:** Partially covered via `CACHE_BLOCK_SIZE`.
- ⚠️ **CSR Address Mapping:** Not covered in config files (likely in schema or implicit).

### Estimated Completeness
Approximately **100%** of the parameters defined in the provided `example_rv64_with_overlay.yaml` file were accessible for extraction.