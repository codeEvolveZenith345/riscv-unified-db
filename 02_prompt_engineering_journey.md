# Prompt Engineering Journey

## Initial Strategy

### Understanding the Task
The objective was to extract RISC-V architectural parameters from a unified database repository. The challenge was to distinguish between "hard" architectural rules and "configurable" parameters that implementation choices define.

### Initial Prompt Design (Version 1.0)
The initial approach relied on a comprehensive, multi-phase prompt structure provided in the assignment description.
- **Phase 1:** Map structure.
- **Phase 2:** Keyword scan ("may", "optional").
- **Phase 3:** Deep context analysis of snippets.
- **Phase 4:** Validation.
- **Phase 5:** YAML generation.

**Results:** The prompt successfully guided the analysis of `example_rv64_with_overlay.yaml`.
**Issues Identified:** The prompt assumed the presence of narrative specification text (PDFs/Markdown) which were not in the file context. The strategy had to adapt to extract "specification" data from YAML comments instead.

## Iteration 1: Adaptation to Available Context

### Refined Strategy
Instead of scanning non-existent PDF text, the focus shifted to:
1.  Analyzing `cfgs/example_rv64_with_overlay.yaml` as the "ground truth" for implementation choices.
2.  Treating the comments in that YAML file as the "specification text."
3.  Using the provided snippets in the prompt (e.g., Cache Block Size) to augment the extraction where repo files were silent.

**Changes Made:**
- Targeted the `params` section of the configuration file.
- Extracted constraints from comment blocks (e.g., "must be 0, 16, or 64").

## Final Prompt Strategy

### Key Success Factors
1.  **Structured Output Definition:** The prompt explicitly defined the YAML schema for the output, ensuring consistency.
2.  **Constraint Mapping:** Explicit instructions to look for "ranges" and "dependencies" allowed for the extraction of complex logic (e.g., PMP granularity dependencies).
3.  **Hallucination Checks:** The requirement to provide `exact_quote` forced a verification step where every parameter had to be traced back to a specific line in the source file.

## Hallucination Prevention Techniques

### Techniques Applied
1.  **Explicit Source Grounding:** Every extracted parameter includes an `exact_quote` field. If a quote couldn't be found, the parameter was excluded or marked.
2.  **Constraint Validation:** Constraints were only added if explicitly stated. For example, `NUM_PMP_ENTRIES` has a constraint "0-64", which is explicitly written in the file comments.
3.  **Scope Verification:** The scope (system vs. hart) was inferred from the file structure (global params vs. per-hart config).

### Hallucination Cases Encountered
| Iteration | Hallucination Type | How Detected | How Fixed |
|-----------|-------------------|--------------|-----------|
| 1 | Inferring `CACHE_BLOCK_SIZE` constraints | Comparison with file content | The prompt provided detailed constraints for Cache Block Size in a snippet, but the actual file only said "size of a cache block, in bytes". I restricted the extracted constraints to what was verifiable in the file, while noting the snippet context in the "notes" field. |