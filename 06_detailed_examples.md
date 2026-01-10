# Detailed Parameter Extraction Examples

## Example 1: PMP Entries (Complex Constraint)

### Source Text
```yaml
  # number of implemented PMP entries. Can be any value between 0-64, inclusive.
  #
  # the number of implemented PMP registers must be 0, 16, or 64.
  # ...
  NUM_PMP_ENTRIES: 14
```

### Extraction Process

#### Step 1: Keyword Identification
- Triggered by: "Can be any value", "must be"
- Context: Comments preceding `NUM_PMP_ENTRIES`

#### Step 2: Parameter Identification
- **Name:** `NUM_PMP_ENTRIES`
- **Type:** Integer (value is 14)

#### Step 3: Constraint Extraction
- **Constraint 1:** Range
  - **Text Basis:** "Can be any value between 0-64, inclusive."
  - **Value:** "0-64"
- **Constraint 2:** Hardware Dependency
  - **Text Basis:** "the number of implemented PMP registers must be 0, 16, or 64."
  - **Value:** "0, 16, 64"

#### Step 4: YAML Generation
```yaml
  - name: "NUM_PMP_ENTRIES"
    description: |
      Defines the number of implemented Physical Memory Protection (PMP) entries.
    type: "integer"
    constraints:
      - type: "range"
        value: "0-64"
        description: "Can be any value between 0 and 64, inclusive."
      - type: "hardware_dependency"
        value: "0, 16, 64"
        description: "The number of implemented PMP registers must be 0, 16, or 64."
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