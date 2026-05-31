# Execute Stage Architecture Decisions and Tradeoff Analysis

## Design Objective

The Execute stage was designed under a waterfall methodology.

The objective was to establish a stable, long-term architecture before implementation, minimizing future interface changes and subsystem redesign.

---

# ALU Control Architecture

A unified 4-bit ALU_Control bus was adopted as the primary operation selector for the Execute stage.

Inputs:

* ALUOp
* funct3
* funct7

Output:

* ALU_Control[3:0]

This bus controls all Execute-stage functional units.

---

# Alternatives Considered

## Alternative A — Direct Opcode Execution

Instruction decoder directly generates every ALU operation.

Example:

```text
opcode
   ->
ADD
SUB
AND
OR
...
```

### Advantages

* Simple conceptually

### Disadvantages

* Large control logic
* Tight coupling between decode and execute
* Poor scalability
* Difficult to extend

### Verdict

Rejected.

Not suitable for a modular pipelined processor.

---

## Alternative B — Hierarchical Decode (Selected)

```text
opcode
   ->
ALUOp
   +
funct3
   +
funct7
   ->
ALU_Control
```

### Advantages

* Smaller decode logic
* Modular architecture
* Matches RISC-V encoding philosophy
* Easy future expansion

### Disadvantages

* Additional ALU Control Unit

### Verdict

Selected.

Provides the cleanest long-term architecture.

---

# Branch Handling Architecture

Branch instructions required special consideration.

---

## Alternative A — Separate ALU Control Code Per Branch

```text
BEQ
BNE
BLT
BGE
BLTU
BGEU
```

all receive unique ALU_Control encodings.

Example:

```text
1010 = BEQ
1011 = BNE
1100 = BLT
...
```

### Advantages

* Branch Comparator becomes simpler

### Disadvantages

* Branch type decoded twice
* Wastes ALU_Control encoding space
* Couples branch logic to ALU Control

### Verdict

Rejected.

Branch semantics already exist inside funct3.

Duplicating them inside ALU_Control provides little benefit.

---

## Alternative B — Single Branch Operation (Selected)

ALU_Control contains only:

```text
BRANCH
```

for all branch instructions.

Branch Comparator receives:

```text
Operand A
Operand B
funct3
```

and determines:

```text
BEQ
BNE
BLT
BGE
BLTU
BGEU
```

internally.

### Advantages

* No duplicate decoding
* Compact ALU_Control space
* Cleaner branch logic
* Maintains Execute-stage uniformity

### Disadvantages

* Branch Comparator contains a small decode table

### Verdict

Selected.

Provides the cleanest balance between modularity and simplicity.

---

# Compare Unit Architecture

RV32I requires:

```text
SLT
SLTU
```

---

## Alternative A — CompareSel Signal

Comparator computes one comparison type at a time.

```text
CompareSel
   ->
Signed
Unsigned
```

### Advantages

* Slightly fewer active circuits

### Disadvantages

* Additional control signal
* Additional multiplexing
* Extra architectural complexity

### Verdict

Rejected.

---

## Alternative B — Parallel Comparators (Selected)

Compute:

```text
signed_less
unsigned_less
```

continuously.

ALU_Control selects the desired result.

### Advantages

* Simpler interface
* No CompareSel signal
* Consistent with ALU philosophy

### Verdict

Selected.

---

# ALU Structural Organization

---

## Alternative A — Instruction-Oriented ALU

```text
ADD Unit
SUB Unit
AND Unit
OR Unit
...
```

### Advantages

* Easy to understand initially

### Disadvantages

* Hardware duplication
* Poor scalability
* Does not reflect actual hardware organization

### Verdict

Rejected.

---

## Alternative B — Functional Unit Organization (Selected)

```text
ALU
├── Arithmetic Unit
│   ├── ADD
│   └── SUB
│
├── Logic Unit
│   ├── AND
│   ├── OR
│   └── XOR
│
├── Shift Unit
│   ├── SLL
│   ├── SRL
│   └── SRA
│
├── Compare Unit
│   ├── SLT
│   └── SLTU
│
├── Branch Comparator
│
└── Result Multiplexer
```

### Advantages

* Mirrors real datapath organization
* Modular
* Easier debugging
* Easier future enhancement

### Verdict

Selected.

---

# Execution Strategy

---

## Alternative A — Sequential Activation

Only the required unit executes.

```text
ADD -> Arithmetic Unit active
AND -> Logic Unit active
```

### Advantages

* Lower hardware activity
* Better power efficiency

### Disadvantages

* Additional enable logic
* Increased control complexity

### Used In

Many commercial processors.

---

## Alternative B — Parallel Evaluation (Selected)

All Execute-stage units compute continuously.

```text
Arithmetic Unit
Logic Unit
Shift Unit
Compare Unit
Branch Comparator
```

operate in parallel.

A final multiplexer selects the required result.

### Advantages

* Very simple control logic
* Easier debugging
* Excellent fit for Logisim Evolution
* Consistent with educational CPU design

### Disadvantages

* Additional simulated hardware activity

### Verdict

Selected.

The simplicity benefit significantly outweighs the simulated hardware cost.

---

# Comparison with Real Processors

Modern processors often use specialized execution units:

```text
Issue Logic
│
├── Integer ALU
├── Branch Unit
├── Load/Store Unit
├── Multiply Unit
├── Floating Point Unit
```

Only the required unit is scheduled for execution.

This improves:

* power efficiency
* throughput
* resource utilization

However, it requires:

* issue logic
* resource arbitration
* scheduling
* hazard tracking

Such complexity is unnecessary for the goals of this project.

The selected architecture intentionally prioritizes:

* architectural clarity
* correctness
* modularity
* educational value
* maintainability

over hardware optimization.