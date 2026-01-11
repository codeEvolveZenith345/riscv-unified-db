## Critical Error Identified

> **The Story:** Writing prompts for AI is like explaining a task to a very literal colleague who follows instructions exactly—but doesn't understand context. This document shows how I went from vague instructions ("find parameters") to precise ones ("extract phrases with 'implementation-specific' and cite the exact sentence"). Along the way, I learned to prevent the AI from inventing information by requiring it to quote sources.

### Issue: Wrong Source Files
**Initial Approach:** Analyzed `cfgs/*.yaml` files
**Problem:** These are OUTPUTS of the database, not specification INPUTS
**Correction:** Pivoted to analyze `arch/**/*.yaml` files containing 
               specification descriptions

### Lesson Learned
Configuration files (cfgs/) are for users to set parameter VALUES.
Specification files (arch/) contain the DEFINITIONS with implementation 
freedom indicators.

## If I Had the Full Repository Context

### Refined Prompt for arch/ Files:
Analyze all files in arch/csr/, arch/inst/, arch/ext/ directories.
For each file:

Extract the description: field text
Search for implementation-freedom indicators:

"implementation-defined" / "implementation-specific"
Field types: WARL, WLRL, WPRI
"may" / "might" / "should" / "optional"


Extract constraints from surrounding text
Generate YAML parameter for each freedom point found

Example:
yaml# In arch/csr/mstatus.yaml
description: |
  The MBE bit is WARL and may be read-only 0 or 1 depending on 
  whether big-endian is supported.
Extract as:
yaml- name: "mstatus_mbe_endianness_support"
  type: "bitfield"
  implementation_freedom: "optional"
  exact_quote: "may be read-only 0 or 1 depending on whether big-endian is supported"