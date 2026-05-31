# ALU Subsystem

## Overview

The Arithmetic Logic Unit (ALU) is the primary computational component of the Execute Stage.

Its responsibility is to perform arithmetic, logical, shift, and comparison operations required by the RV32I instruction set.

The ALU receives operands from the Decode Stage through the ID/EX pipeline register and produces results that are forwarded to the Memory Stage through the EX/MEM pipeline register.

---

# Design Goals

The ALU subsystem was designed with the following objectives:

* Modular architecture
* Clean separation of control and execution
* Easy extensibility for future ISA support
* Compatibility with a pipelined datapath
* Simplicity of implementation in Logisim Evolution
* Long-term maintainability

The design follows a waterfall methodology, prioritizing architectural stability before implementation.

---

# ALU Control Architecture

The Execute Stage uses a unified ALU control interface.

Inputs:

```text
ALUOp
funct3
funct7
```

Output:

```text
ALU_Control[3:0]
```

The ALU Control Unit translates instruction semantics into a compact operation code used by the ALU subsystem.

Control flow:

```text
Instruction
    ↓
Main Control Unit
    ↓
ALUOp
    ↓
ALU Control Unit
    ↓
ALU_Control[3:0]
    ↓
ALU Subsystem
```

---

# Supported RV32I Operations

## Arithmetic Operations

```text
ADD
SUB
```

Used by:

```text
ADD
ADDI
SUB
Address Calculations
Load/Store Address Generation
```

---

## Logical Operations

```text
AND
OR
XOR
```

Used by:

```text
AND
ANDI
OR
ORI
XOR
XORI
```

---

## Shift Operations

```text
SLL
SRL
SRA
```

Used by:

```text
SLL
SLLI
SRL
SRLI
SRA
SRAI
```

---

## Comparison Operations

```text
SLT
SLTU
```

Used by:

```text
SLT
SLTI
SLTU
SLTIU
```

---

# ALU Structural Organization

The ALU is organized into functional units rather than instruction-specific units.

```text
ALU
│
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

This organization mirrors real datapath design and simplifies future expansion.

---

# Arithmetic Unit

Inputs:

```text
Operand_A[31:0]
Operand_B[31:0]
```

Outputs:

```text
Add_Result[31:0]
Sub_Result[31:0]
```

Responsibilities:

* Integer addition
* Integer subtraction
* Address generation

The Arithmetic Unit is shared by both arithmetic instructions and memory address calculations.

---

# Logic Unit

Inputs:

```text
Operand_A[31:0]
Operand_B[31:0]
```

Outputs:

```text
AND_Result
OR_Result
XOR_Result
```

Responsibilities:

* Bitwise logical operations

---

# Shift Unit

Inputs:

```text
Operand_A[31:0]
Shift_Amount[4:0]
```

Outputs:

```text
SLL_Result
SRL_Result
SRA_Result
```

Responsibilities:

* Logical left shift
* Logical right shift
* Arithmetic right shift

---

# Compare Unit

The Compare Unit supports RV32I comparison instructions.

Implemented comparisons:

```text
SLT
SLTU
```

Architecture:

```text
Signed Comparator
Unsigned Comparator
```

Both comparators operate continuously.

The ALU_Control signal selects the required result.

This removes the need for additional comparison-select control signals.

---

# Branch Comparator

The Branch Comparator is a dedicated execution unit.

Inputs:

```text
Operand_A
Operand_B
funct3
```

Supported branch types:

```text
BEQ
BNE
BLT
BGE
BLTU
BGEU
```

Output:

```text
Branch_Taken
```

The branch type remains encoded in funct3 and is not duplicated within ALU_Control.

This minimizes redundant decoding and reduces control complexity.

---

# Result Multiplexer

All functional units operate in parallel.

Generated results:

```text
Arithmetic Result
Logic Result
Shift Result
Compare Result
```

The Result Multiplexer selects the final ALU output using:

```text
ALU_Control[3:0]
```

Output:

```text
ALU_Result[31:0]
```

---

# Execution Strategy

The ALU subsystem uses parallel evaluation.

```text
Arithmetic Unit
Logic Unit
Shift Unit
Compare Unit
Branch Comparator
```

all compute continuously.

The final output is selected through multiplexing.

Advantages:

* Simple control logic
* Easier debugging
* Clean datapath organization
* Excellent fit for Logisim Evolution

This approach prioritizes clarity and maintainability over power optimization.

---

# Interface Summary

## Inputs

```text
Operand_A[31:0]
Operand_B[31:0]
ALU_Control[3:0]
funct3[2:0]
```

## Outputs

```text
ALU_Result[31:0]
Branch_Taken
```

---

# Future Expansion

Planned future additions include:

```text
RV32M Multiply Unit
RV32M Divide Unit
CSR Operations
Advanced Branch Handling
Forwarding Integration
Performance Monitoring
```

The current architecture was intentionally designed to accommodate these additions without major structural redesign.

---

# Architectural Status

The ALU subsystem serves as the computational core of the Execute Stage and forms the foundation for all arithmetic, logical, comparison, and branch operations within the processor. The design emphasizes modularity, scalability, and architectural clarity while maintaining compatibility with future ISA extensions.
