# Detailed Parameter Extraction Examples

## Example 1: Cache Block Size (Specification Snippet)

### Source Text
> "The capacity and organization of a cache and the size of a cache block are both implementation-specific." (Privileged Spec 19.3.1)

### Extraction Process

#### Step 1: Keyword Identification
- Triggered by: "implementation-specific"

#### Step 2: Parameter Identification
- **Name:** `cache_block_size`
- **Type:** Integer (implied by "size")

#### Step 3: Constraint Extraction
- **Constraint 1:** Alignment
  - **Text Basis:** "contiguous, naturally aligned power-of-two (or NAPOT)"
  - **Value:** "power_of_two_or_napot"

#### Step 4: YAML Generation
```yaml
  - name: "cache_block_size"
    source: "specification_snippet"
    description: "Size of a cache block..."
    type: "integer"
    implementation_freedom: "implementation_specific"
    specification_reference:
      document: "RISC-V Privileged Specification"
      section: "19.3.1"
```

---

## Example 2: Misaligned Load/Store (Dependency)

### Source Text
```yaml
  # whether or not the implementation supports misaligned loads and stores in main memory (not including atomics)
  # must be true when Zicclsm is supported
  MISALIGNED_LDST: true
```

### Extraction Process

#### Step 1: Keyword Identification
- Triggered by: "whether or not", "must be true when"

#### Step 2: Parameter Identification
- **Name:** `MISALIGNED_LDST`
- **Type:** Boolean

#### Step 3: Constraint Extraction
- **Constraint 1:** Dependency
  - **Text Basis:** "must be true when Zicclsm is supported"
  - **Value:** "Zicclsm"

#### Step 4: YAML Generation
```yaml
  - name: "MISALIGNED_LDST"
    description: "Indicates whether the implementation supports misaligned loads and stores."
    type: "boolean"
    constraints:
      - type: "dependency"
        value: "Zicclsm"
        description: "Must be true when the Zicclsm extension is supported."
```

---

## Example 3: WLRL Field Behavior (Multiple Parameters from One Section)

### Source Text
```text
Some read/write CSR fields specify behavior for only a subset of possible 
bit encodings, with other bit encodings reserved. Software should not write 
anything other than legal values to such a field...

Implementations are permitted but not required to raise an illegal-instruction 
exception if an instruction attempts to write a non-supported value to a WLRL field.

Implementations can return arbitrary bit patterns on the read of a WLRL field 
when the last write was of an illegal value, but the value returned should 
deterministically depend on the illegal written value and the value of the 
field prior to the write.
```

### Extraction Process

#### Step 1: Keyword Identification
- Trigger 1: "permitted but not required" → **OPTIONAL** behavior
- Trigger 2: "can return arbitrary" + "should deterministically" → **IMPLEMENTATION_SPECIFIC** choice

#### Step 2: Multiple Parameters from Same Text
This paragraph yields **TWO DISTINCT** parameters:

**Parameter 1: Exception Behavior**
```yaml
- name: "wlrl_illegal_value_handling"
  type: "exception_behavior"
  implementation_freedom: "optional"
  exact_quote: "Implementations are permitted but not required to raise an 
                illegal-instruction exception..."
```

**Parameter 2: Read-Back Behavior**
```yaml
- name: "wlrl_read_after_illegal_write"
  type: "bitfield_behavior"
  implementation_freedom: "implementation_specific"
  constraints:
    - type: "determinism"
      value: "deterministic_but_arbitrary"
  exact_quote: "Implementations can return arbitrary bit patterns on the read of a 
                WLRL field when the last write was of an illegal value, but the 
                value returned should deterministically depend..."
```

#### Step 3: Validation
✅ Both parameters cite the same specification section (2.3.2)
✅ Each has distinct `exact_quote` from different sentences
✅ Different types: `exception_behavior` vs `bitfield_behavior`
✅ Different implementation_freedom levels: `optional` vs `implementation_specific`

### Lesson Learned
A single specification paragraph can define **multiple independent** implementation choices. Each choice is a separate parameter.

---

## Anti-Pattern Examples

### What NOT to Do: Hallucinated Discovery Mechanism
```yaml
# WRONG - Inventing a specific CSR bit that wasn't mentioned
discoverability:
  method: "csr_read"
  mechanism: "Read bit 3 of mstatus register"  # ❌ This specific mechanism is not in the text!
```

### What NOT to Do: Ignoring Context
```yaml
# WRONG - Ignoring the "not including atomics" qualification
description: "Supports all misaligned memory accesses." # ❌ Text explicitly excludes atomics
```