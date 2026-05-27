# README.md — Complete RISC-V Computer in Logisim Evolution


# Complete RISC-V Computer in Logisim Evolution

> Building a complete pipelined computer system from scratch using digital logic.

---

## Overview

This project aims to design and implement a fully functional **32-bit RISC-V computer** inside 
:contentReference[oaicite:0]{index=0} using only digital hardware primitives.

The goal is not just to build a CPU, but to engineer an entire computer system including:

- Pipelined RISC-V CPU
- Memory hierarchy
- System bus
- Interrupt system
- UART console
- DMA engine
- GPU / VGA output
- Storage interfaces
- OS support infrastructure
- Toolchain integration

The system is being built from the ground up with a strong focus on:

- Computer architecture
- Hardware engineering principles
- Clean modular design
- Scalability
- Realistic datapath organization
- Pipeline correctness
- Documentation and architectural reasoning

---

# Project Goals

## Primary Goals

- Build a complete RV32I pipelined processor
- Execute real compiled RISC-V programs
- Run programs written in C
- Create a scalable computer architecture
- Learn low-level hardware engineering deeply
- Simulate realistic processor behavior

---

## Long-Term Goals

- RV32IM support
- Cache hierarchy
- Interrupt handling
- VGA graphics output
- DMA subsystem
- Virtual memory support
- Minimal operating system
- Storage subsystem
- Advanced debugging tools
- Potential multi-core experimentation

---

# ISA Target

| Feature | Value |
|---|---|
| ISA | RV32I |
| Future ISA | RV32IM |
| Word Size | 32-bit |
| Endianness | Little Endian |
| Register Count | 32 |
| Pipeline | 5-stage |
| Clocking | Single synchronous clock |

---

# CPU Architecture

The processor follows a classic 5-stage RISC-V pipeline:

```text
IF -> ID -> EX -> MEM -> WB
````

---

# Pipeline Stages

## Instruction Fetch (IF)

Responsible for:

* Program Counter management
* Instruction fetching
* Branch target selection
* PC increment logic

### Main Components

* Program Counter
* PC Incrementer
* Branch Target Adder
* Instruction Memory Interface
* IF/ID Pipeline Register

---

## Instruction Decode (ID)

Responsible for:

* Instruction decoding
* Register reads
* Immediate generation
* Control signal generation
* Hazard detection

### Main Components

* Register File
* Immediate Generator
* Main Control Unit
* Hazard Detection Unit
* Instruction Splitter
* ID/EX Pipeline Register

---

## Execute (EX)

Responsible for:

* Arithmetic operations
* Logic operations
* Branch evaluation
* Forwarding
* ALU control

### Main Components

* ALU
* ALU Control Unit
* Forwarding Unit
* Branch Comparator
* Operand Select MUXes
* EX/MEM Pipeline Register

---

## Memory (MEM)

Responsible for:

* Load/store execution
* Memory alignment
* MMIO handling
* Cache integration (future)

### Main Components

* Load/Store Unit
* Data Memory Interface
* Memory Alignment Unit
* MMIO Decoder
* MEM/WB Pipeline Register

---

## Writeback (WB)

Responsible for:

* Writing results back to registers

### Main Components

* Writeback MUX
* Register Write Controller

---

# Comprehensive System Architecture

```text
Complete_Computer_System
│
├── Clock_Reset_System
│   │
│   ├── Global_Clock
│   ├── Clock_Divider
│   ├── Step_Clock_Controller
│   ├── Run_Halt_Controller
│   ├── Reset_Controller
│   └── Debug_Clock_Interface
│
├── CPU_Core
│   │
│   ├── Fetch_Stage
│   │   │
│   │   ├── Program_Counter
│   │   ├── PC_Select_Mux
│   │   ├── PC_Incrementer
│   │   ├── Branch_Target_Adder
│   │   ├── Jump_Target_Generator
│   │   ├── Instruction_Memory_Interface
│   │   └── IF_ID_Register
│   │
│   ├── Decode_Stage
│   │   │
│   │   ├── Instruction_Splitter
│   │   ├── Opcode_Decoder
│   │   ├── Register_File
│   │   ├── Immediate_Generator
│   │   ├── Main_Control_Unit
│   │   ├── Branch_Decoder
│   │   ├── Hazard_Detection_Unit
│   │   ├── CSR_Decode_Unit
│   │   └── ID_EX_Register
│   │
│   ├── Execute_Stage
│   │   │
│   │   ├── ALU
│   │   │   │
│   │   │   ├── Adder
│   │   │   ├── Logic_Unit
│   │   │   ├── Shifter
│   │   │   ├── Comparator
│   │   │   └── Result_Mux
│   │   │
│   │   ├── ALU_Control_Unit
│   │   ├── Operand_Select_Mux_A
│   │   ├── Operand_Select_Mux_B
│   │   ├── Forwarding_Mux_A
│   │   ├── Forwarding_Mux_B
│   │   ├── Forwarding_Unit
│   │   ├── Branch_Comparator
│   │   ├── Branch_Decision_Unit
│   │   ├── Multiply_Divide_Unit
│   │   └── EX_MEM_Register
│   │
│   ├── Memory_Stage
│   │   │
│   │   ├── Load_Store_Unit
│   │   ├── Data_Memory_Interface
│   │   ├── Memory_Alignment_Unit
│   │   ├── Byte_Enable_Generator
│   │   ├── MMIO_Address_Decoder
│   │   ├── Cache_Controller
│   │   ├── Data_Cache
│   │   └── MEM_WB_Register
│   │
│   ├── Writeback_Stage
│   │   │
│   │   ├── Writeback_Mux
│   │   ├── Register_Write_Controller
│   │   └── CSR_Writeback_Path
│   │
│   ├── Pipeline_Control_System
│   │   │
│   │   ├── Stall_Controller
│   │   ├── Flush_Controller
│   │   ├── Bubble_Inserter
│   │   ├── Pipeline_Valid_Manager
│   │   └── Exception_Pipeline_Control
│   │
│   ├── CSR_System
│   │   │
│   │   ├── CSR_Register_File
│   │   ├── Trap_Handler
│   │   ├── Exception_Controller
│   │   ├── Interrupt_Interface
│   │   └── Privilege_Mode_Controller
│   │
│   └── Debug_Unit
│       │
│       ├── Pipeline_State_Viewer
│       ├── Register_Viewer
│       ├── Memory_Viewer
│       ├── Breakpoint_Controller
│       └── Trace_Logger
│
├── Memory_System
│   │
│   ├── Boot_ROM
│   ├── Main_RAM
│   ├── Instruction_Memory
│   ├── Data_Memory
│   ├── Instruction_Cache
│   ├── Data_Cache
│   ├── Cache_Coherency_Controller
│   ├── Memory_Controller
│   ├── DMA_Memory_Interface
│   └── Memory_Arbitration_Unit
│
├── Bus_System
│   │
│   ├── System_Bus
│   ├── Address_Bus
│   ├── Data_Bus
│   ├── Control_Bus
│   ├── Bus_Arbiter
│   ├── Bus_Request_Controller
│   ├── Bus_Grant_Controller
│   ├── Bus_Buffering_System
│   └── MMIO_Bus_Bridge
│
├── IO_System
│   │
│   ├── UART_System
│   │   │
│   │   ├── UART_TX
│   │   ├── UART_RX
│   │   ├── Baud_Rate_Generator
│   │   ├── TX_FIFO
│   │   ├── RX_FIFO
│   │   └── UART_Control_Registers
│   │
│   ├── Timer_System
│   │   │
│   │   ├── System_Timer
│   │   ├── Cycle_Counter
│   │   ├── Compare_Register
│   │   └── Timer_Interrupt_Generator
│   │
│   ├── GPIO_System
│   │   │
│   │   ├── GPIO_Input_Register
│   │   ├── GPIO_Output_Register
│   │   └── GPIO_Control_Register
│   │
│   ├── Keyboard_Controller
│   ├── Mouse_Controller
│   ├── Audio_Controller
│   └── Storage_Controller
│       │
│       ├── Disk_Interface
│       ├── SD_Card_Interface
│       ├── SPI_Controller
│       ├── Block_Device_Buffer
│       └── DMA_Storage_Interface
│
├── Interrupt_System
│   │
│   ├── Interrupt_Controller
│   ├── Interrupt_Vector_Table
│   ├── Priority_Encoder
│   ├── External_Interrupt_Interface
│   ├── Software_Interrupt_Controller
│   └── Timer_Interrupt_Controller
│
├── DMA_System
│   │
│   ├── DMA_Controller
│   ├── DMA_Channel_Manager
│   ├── DMA_Address_Generator
│   ├── DMA_Transfer_Engine
│   ├── DMA_Bus_Interface
│   └── DMA_Interrupt_Generator
│
├── GPU_System
│   │
│   ├── Framebuffer
│   ├── Video_RAM
│   ├── VGA_Controller
│   │   │
│   │   ├── Horizontal_Timing_Generator
│   │   ├── Vertical_Timing_Generator
│   │   ├── Sync_Generator
│   │   └── Pixel_Output_Unit
│   │
│   ├── Sprite_Engine
│   ├── Blitter_Engine
│   ├── Palette_Controller
│   └── GPU_Command_Processor
│
├── Storage_System
│   │
│   ├── Virtual_Disk
│   ├── Filesystem_Controller
│   ├── Block_Cache
│   ├── Directory_Manager
│   └── Disk_Buffer_Cache
│
├── Operating_System_Support
│   │
│   ├── MMU
│   ├── TLB
│   ├── Page_Table_Walker
│   ├── Syscall_Handler
│   ├── Context_Switch_Controller
│   └── Process_Protection_System
│
├── Toolchain_Support
│   │
│   ├── ROM_Image_Loader
│   ├── Program_Loader
│   ├── Debug_Interface
│   ├── Test_Harness
│   └── Trace_Export_System
│
├── Future_Expansion
│   │
│   ├── Branch_Predictor
│   ├── BTB
│   ├── Superscalar_Dispatch
│   ├── Out_of_Order_Engine
│   ├── Vector_Processing_Unit
│   ├── Floating_Point_Unit
│   ├── AI_Accelerator
│   └── Multi_Core_Interface
│
├── Firmware_System
│   │
│   ├── Boot_ROM_Firmware
│   ├── Reset_Vector_Handler
│   ├── Hardware_Initialization
│   ├── Memory_Initialization
│   ├── UART_Console_Init
│   ├── Diagnostic_System
│   ├── Firmware_Monitor_Shell
│   ├── Executable_Loader
│   ├── ELF_Loader
│   ├── Binary_Image_Loader
│   ├── Firmware_Debug_Interface
│   └── Boot_Control_Transfer
│
├── Boot_Process_System
│   │
│   ├── Reset_Sequence
│   ├── ROM_Execution_Path
│   ├── Hardware_Bringup
│   ├── Peripheral_Initialization
│   ├── Memory_Map_Initialization
│   ├── Program_Load_Sequence
│   ├── Kernel_Load_Sequence
│   ├── Userspace_Load_Sequence
│   └── Execution_Entry_Manager
│
├── Operating_System
│   │
│   ├── Kernel_Core
│   │   │
│   │   ├── Scheduler
│   │   ├── Syscall_Interface
│   │   ├── Trap_Handler
│   │   ├── Interrupt_Manager
│   │   ├── Process_Manager
│   │   ├── Context_Switcher
│   │   ├── Memory_Manager
│   │   └── Kernel_Debug_Interface
│   │
│   ├── Filesystem_Layer
│   │   │
│   │   ├── Virtual_Filesystem
│   │   ├── Block_Device_Interface
│   │   ├── File_Descriptor_Manager
│   │   └── Filesystem_Driver
│   │
│   ├── Device_Drivers
│   │   │
│   │   ├── UART_Driver
│   │   ├── Timer_Driver
│   │   ├── VGA_Driver
│   │   ├── Storage_Driver
│   │   └── GPIO_Driver
│   │
│   ├── Userspace_Runtime
│   │   │
│   │   ├── C_Runtime
│   │   ├── Heap_Manager
│   │   ├── Standard_Library_Support
│   │   └── Program_Loader
│   │
│   └── Userspace_Programs
│       │
│       ├── Shell
│       ├── Diagnostics_Utilities
│       ├── File_Utilities
│       ├── Graphics_Demos
│       └── Test_Programs
│
├── Software_Toolchain
│   │
│   ├── GCC_Toolchain
│   │   │
│   │   ├── riscv32_unknown_elf_gcc
│   │   ├── GNU_Assembler
│   │   ├── GNU_Linker
│   │   ├── objcopy
│   │   └── objdump
│   │
│   ├── Build_System
│   │   │
│   │   ├── Makefiles
│   │   ├── Build_Scripts
│   │   ├── Linker_Scripts
│   │   └── Dependency_Manager
│   │
│   ├── Image_Generation_Tools
│   │   │
│   │   ├── ELF_To_Binary_Converter
│   │   ├── Memory_Image_Generator
│   │   ├── ROM_Image_Generator
│   │   └── RAM_Image_Generator
│   │
│   ├── Debugging_Tools
│   │   │
│   │   ├── Disassembly_Tools
│   │   ├── Trace_Analyzers
│   │   ├── Memory_Inspection_Tools
│   │   └── Execution_Debug_Tools
│   │
│   └── Automation_System
│       │
│       ├── Simulation_Runner
│       ├── Automated_Test_Harness
│       ├── Benchmark_Runner
│       └── Continuous_Test_System
│
└── Software_Development_Workflow
    │
    ├── C_Program_Development
    ├── Assembly_Program_Development
    ├── Cross_Compilation
    ├── ELF_Generation
    ├── Binary_Extraction
    ├── Memory_Image_Creation
    ├── ROM_RAM_Loading
    ├── Logisim_Execution
    ├── Runtime_Debugging
    └── Performance_Validation
```



---

# Top-Level Design Philosophy

This project follows several important engineering principles:

## Modular Design

Every subsystem is implemented as an isolated reusable hardware module.

---

## Bottom-Up Engineering

Each component is defined with:

* Exact inputs/outputs
* Signal widths
* Timing behavior
* Control logic
* Internal datapaths

No ambiguous behavior is allowed.

---

## Scalable Architecture

The design intentionally leaves room for:

* caches
* DMA
* GPU
* MMU
* privilege modes
* advanced pipeline features

without requiring a complete redesign later.

---

## Realistic Hardware Constraints

The implementation attempts to respect real-world computer architecture concepts:

* pipeline hazards
* forwarding
* stalls
* branch penalties
* bus arbitration
* memory timing
* alignment behavior

---

# Planned Features

## CPU Features

* [x] RV32I base architecture
* [ ] RV32IM extension
* [ ] CSR support
* [ ] Exceptions and traps
* [ ] Privilege modes
* [ ] Branch prediction

---

## Memory Features

* [x] Boot ROM
* [x] Main RAM
* [ ] Instruction cache
* [ ] Data cache
* [ ] MMU
* [ ] Virtual memory

---

## IO Features

* [ ] UART
* [ ] Timers
* [ ] GPIO
* [ ] Keyboard input
* [ ] Mouse input
* [ ] Audio controller

---

## Graphics Features

* [ ] VGA controller
* [ ] Framebuffer
* [ ] Sprite engine
* [ ] 2D acceleration

---

## Storage Features

* [ ] SD card interface
* [ ] Virtual disk
* [ ] Filesystem layer

---

# Current Development Status

## In Progress

* CPU datapath planning
* Pipeline organization
* Immediate generator
* Register file
* Architectural decomposition

---

# Documentation Philosophy

This repository treats architecture documentation as a first-class engineering artifact.

The project maintains:

* architecture notes
* implementation decisions
* debugging reports
* timing analysis
* future expansion plans
* performance analysis

The goal is to document not only *what* was built, but also:

* why it was built that way
* what alternatives were considered
* what problems occurred during implementation

---

# Tools & Technologies

| Tool                 | Purpose                |
| -------------------- | ---------------------- |
| Logisim Evolution    | Hardware simulation    |
| RISC-V GCC Toolchain | Program compilation    |
| RISC-V ISA           | Processor architecture |
| Markdown             | Documentation          |
| Git/GitHub           | Version control        |

---

# Firmware Direction

## Firmware & Boot Process

The computer boots through a custom firmware environment stored inside Boot ROM.

The firmware is responsible for initializing the machine, preparing hardware subsystems,
loading executable programs, and transferring execution control to the operating environment.

---

## Boot Sequence


1. Reset vector enters Boot ROM
2. Firmware initializes hardware
3. UART console initialized
4. Executable loaded
5. Kernel/application launched

---

## Firmware Responsibilities

The firmware layer provides:

* Hardware initialization
* Memory setup
* Peripheral configuration
* UART console support
* Diagnostics and self-tests
* Executable loading
* Monitor/debug shell
* Low-level debugging support

---

## Firmware Design Goals

The firmware is intended to behave similarly to early-stage boot firmware found in
real computer systems.

The implementation aims to remain:

* lightweight
* understandable
* debuggable
* hardware-oriented

while still supporting progressively more advanced software environments.

---
# Operating System Direction

The long-term target environment is a lightweight Unix-like operating system
designed specifically for the custom RISC-V platform.

The operating system will initially focus on:
- simplicity
- educational value
- hardware transparency
- incremental capability growth

---

## Planned Features

Planned operating system functionality includes:

- Minimal task scheduler
- System call interface
- Filesystem support
- Process abstraction
- Userspace applications
- C runtime support
- UART terminal interaction
- Executable loading

---

## Not Initially Planned

The early operating system intentionally avoids extremely complex features such as:

- Full virtual memory
- Linux compatibility
- Dynamic linking
- Advanced userspace isolation
- Complex driver frameworks

These may be explored later as future architectural expansions.


---

# Software Toolchain

The project uses a standard RISC-V embedded software toolchain
to compile and prepare programs for execution on the simulated hardware.



## Compiler

```text
riscv32-unknown-elf-gcc
````

Used for:

* C compilation
* low-level runtime generation
* executable generation

---

## Assembler

```text
GNU binutils
```

Used for:

* Assembly compilation
* object generation
* linking support

---

## Binary Utilities

Utilities used during development:

```text
objcopy
objdump
```

These tools assist with:

* ELF inspection
* disassembly
* binary extraction
* memory image generation

---

## Languages

Primary development languages:

* C
* RISC-V Assembly
* Python (tooling and automation)

---

## Automation Pipeline

Planned automation tooling includes:

* ELF to memory image conversion
* ROM/RAM image generation
* automated build scripts
* executable packaging
* simulation test harnesses


# Development Workflow

The software and hardware development process follows a complete
cross-compilation and simulation workflow.

---

## Workflow


1. Write C/Assembly program
2. Compile using RISC-V GCC
3. Link with custom linker script
4. Convert ELF to flat binary
5. Generate Logisim memory image
6. Load image into simulated RAM/ROM
7. Execute on custom computer


---

## Workflow Goals

This workflow enables:

* rapid hardware validation
* realistic software testing
* executable-level debugging
* operating system experimentation
* compiler compatibility testing

The long-term objective is to create a complete hardware/software co-design environment.


---

# Future Scope

Potential future expansions include:

* Superscalar execution
* Out-of-order execution
* Floating Point Unit
* Vector extensions
* AI accelerator experiments
* Multi-core architecture

---

# Inspirations

This project draws inspiration from:

* classic RISC architectures
* educational CPU projects
* modern processor pipelines
* operating system development
* hardware engineering workflows

---

# Why This Project Exists

Most CPU tutorials stop at:

* tiny toy processors
* single-cycle CPUs
* incomplete datapaths

This project attempts to go much further:

> Build a real computer architecture progressively,
> understand every signal path,
> and document the entire engineering journey.

---

# License

MIT License (planned)

---

# Project Status

🚧🚧🚧 Active Development 🚧🚧🚧

This project is evolving continuously as the architecture matures.

---

# Author

Built as a long-term computer engineering and architecture project.

~ AtharvM2411

