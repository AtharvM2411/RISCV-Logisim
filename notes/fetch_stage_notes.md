# Fetch Stage Documentation

## Complete RISC-V Computer in Logisim Evolution

---

# Overview

The Fetch Stage is the first fully implemented pipeline stage of the processor.

Its responsibility is to:

* maintain program flow
* fetch instructions from instruction memory
* update the Program Counter
* propagate fetched instructions into the decode stage

Implemented pipeline flow:

```text id="a3d4nq"
PC
 ↓
Instruction Memory
 ↓
IF/ID Register
 ↓
Decode Stage
```

---

# Implemented Components

## 1. Program Counter (PC)

### Purpose

Stores the address of the current instruction.

### Inputs

| Signal  | Width | Description       |
| ------- | ----- | ----------------- |
| `CLK`   | 1     | Global clock      |
| `reset` | 1     | Resets PC         |
| `stall` | 1     | Freezes PC update |
| `PC_in` | 32    | Next PC value     |

### Outputs

| Signal   | Width |
| -------- | ----- |
| `PC_out` | 32    |

---

## Behavior

Normal operation:

PC_{next}=PC_{in}

Sequential execution:

PC_{next}=PC+4

---

## Stall Handling

When:

```text id="c2l4fp"
stall = 1
```

the PC register write-enable is disabled.

Effect:

```text id="y8t5vc"
PC value is preserved
```

This prevents the pipeline from advancing during hazards.

---

# 2. PC Increment Logic

## Purpose

Generates:

```text id="v4r8tm"
PC + 4
```

for normal sequential instruction execution.

---

## Behavior

PC_{next}=PC+4

---

# 3. Variable PC Update Path

## Purpose

Supports future:

* branches
* jumps
* exceptions
* trap vectors

Current implementation already allows dynamic PC source selection.

---

## Design Benefit

Avoids future fetch-stage redesign.

---

# 4. Instruction Memory Subsystem

## Purpose

Fetches instructions using current PC address.

---

## Implemented Features

* 32-bit instruction width
* address-driven fetch
* ROM-based instruction storage
* selectable instruction sources
* modular memory subsystem

---

## Current Configuration

| Property    | Value          |
| ----------- | -------------- |
| Width       | 32-bit         |
| Addressing  | Word Addressed |
| Memory Type | ROM            |
| Capacity    | 64K x 32       |

---

# 5. IF/ID Pipeline Register

## Purpose

Transfers fetch-stage outputs into decode stage.

---

## Stored Values

| Value         | Width |
| ------------- | ----- |
| `PC`          | 32    |
| `instruction` | 32    |
| `valid_bit`   | 1     |

---

# Stall Support

Pipeline register write-enable:

```text id="t9z4nm"
WE = !stall
```

Effect:

* pipeline state freezes during stalls
* prevents invalid advancement

---

# Reset Behavior

On reset:

```text id="m3x8qp"
pipeline registers cleared
```

---

# 6. Valid Bit Register

## Purpose

Tracks whether pipeline data is valid.

---

## Why It Exists

Required later for:

* pipeline bubbles
* flush handling
* branch recovery
* exception handling
* hazard management

---

## Current Behavior

| Value | Meaning           |
| ----- | ----------------- |
| `1`   | Valid instruction |
| `0`   | Invalid / bubble  |

---

# 7. Instruction Splitter

## Purpose

Extracts instruction fields for decode stage.

---

## Extracted Fields

| Field  | Bits    |
| ------ | ------- |
| opcode | [6:0]   |
| rd     | [11:7]  |
| funct3 | [14:12] |
| rs1    | [19:15] |
| rs2    | [24:20] |
| funct7 | [31:25] |

---

# Design Notes

The splitter is implemented as an isolated reusable subcircuit.

Benefits:

* cleaner decode stage
* simpler routing
* reusable datapath extraction
* easier debugging

---

# Fetch Stage Pipeline Flow

## Sequential Flow

```text id="d7w2kt"
1. PC outputs current address
2. Instruction memory fetches instruction
3. Instruction enters IF/ID register
4. PC+4 generated
5. Next PC selected
6. PC updated on next clock
```

---

# Current Fetch Architecture

```text id="z5r0mf"
Program Counter
      ↓
PC Select Logic
      ↓
Instruction Memory
      ↓
IF/ID Register
      ↓
Instruction Splitter
      ↓
Decode Stage
```

---

# Implemented Pipeline Features

## Completed

* [x] Clocked PC register
* [x] PC stall support
* [x] PC+4 logic
* [x] Variable PC update path
* [x] Instruction memory subsystem
* [x] IF/ID pipeline register
* [x] Valid bit tracking
* [x] Instruction field extraction

---

# Future Planned Additions

## Fetch Stage Enhancements

Planned later:

* branch prediction
* instruction cache
* prefetch queue
* compressed instruction support
* branch target buffer
* exception vector routing

---

# Engineering Decisions

## Modular Memory Subsystem

Instruction memory implemented as isolated subsystem.

Reason:

* scalability
* easier cache integration
* cleaner hierarchy

---

## Separate Valid Bit Register

Chosen early to simplify:

* future hazard handling
* flush logic
* exception recovery

---

## Stall-Based Write Enable

Pipeline freezing implemented through register write-enable control.

Reason:

* simpler hardware
* deterministic behavior
* realistic pipeline design

---
