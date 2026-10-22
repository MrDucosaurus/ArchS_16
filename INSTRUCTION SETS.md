
# ArchS_16

**ArchS_16 (Architecture 16-bit)** is a custom 16-bit CPU architecture designed for a virtual computer built in Scratch as part of the NEXUS project.

## Overview

ArchS_16 uses fixed-length machine instructions, registers, and hexadecimal opcodes.

The initial goal is to build a simple working CPU in Scratch with a display. The architecture can be expanded as the virtual computer develops.

## Instruction Format

Every instruction is exactly **4 hexadecimal digits (16 bits)**.

```text
OOOO PPPP
```

- `OO` — 8-bit opcode identifying the instruction.
- `PP` — 8-bit parameter used by the instruction.

For example:

```text
0100
```

This means opcode `01` with parameter `00`.

A program containing four instructions can be written as:

```text
0100024190009100
```

This contains the following instructions:

| Machine code | Opcode | Parameter | Meaning |
|---|---|---|---|
| `0100` | `01` | `00` | Set the high byte of `A_REG` to `00` |
| `0241` | `02` | `41` | Set the low byte of `A_REG` to `41` |
| `9000` | `90` | `00` | Output `A_REG` to the display |
| `9100` | `91` | `00` | Stop the CPU |

The display instructions above are initial test assignments and may change as the instruction set is developed.

## Registers

ArchS_16 uses eight general-purpose 16-bit registers:

- `A_REG`
- `B_REG`
- `C_REG`
- `D_REG`
- `E_REG`
- `F_REG`
- `G_REG`
- `H_REG`

Each register contains 16 bits and can be loaded one byte at a time.

For example, `A_REG` contains a high byte and a low byte:

```text
A_REG = ABCD
```

- High byte: `AB`
- Low byte: `CD`

### Loading a Register

The following instructions load the value `ABCD` into `A_REG`:

```text
01AB
02CD
```

After both instructions execute:

```text
A_REG = ABCD
```

This works because each instruction contains an 8-bit parameter.

## No Internal CPU RAM

ArchS_16 does not use internal CPU RAM for storing instructions or general-purpose data.

- Instructions are supplied by the virtual computer's program.
- Registers retain their values between instructions.
- Register operations work directly with CPU registers.
- External memory may be introduced later if required, without making it internal CPU RAM.

The Scratch implementation may use Scratch variables to simulate CPU state. These variables represent registers and other CPU components rather than requiring physical RAM inside the architecture.

## Initial Opcode Assignments

The following opcodes are initial proposals. Not all instructions are implemented yet.

### Register Loading

| Opcode | Instruction | Description |
|---|---|---|
| `00` | `NOP` | Do nothing |
| `01` | `MAH` | Move immediate byte into A_REG high byte |
| `02` | `MAL` | Move immediate byte into A_REG low byte |
| `03` | `MBH` | Move immediate byte into B_REG high byte |
| `04` | `MBL` | Move immediate byte into B_REG low byte |
| `05` | `MCH` | Move immediate byte into C_REG high byte |
| `06` | `MCL` | Move immediate byte into C_REG low byte |
| `07` | `MDH` | Move immediate byte into D_REG high byte |
| `08` | `MDL` | Move immediate byte into D_REG low byte |
| `09` | `MEH` | Move immediate byte into E_REG high byte |
| `0A` | `MEL` | Move immediate byte into E_REG low byte |
| `0B` | `MFH` | Move immediate byte into F_REG high byte |
| `0C` | `MFL` | Move immediate byte into F_REG low byte |
| `0D` | `MGH` | Move immediate byte into G_REG high byte |
| `0E` | `MGL` | Move immediate byte into G_REG low byte |
| `0F` | `MHH` | Move immediate byte into H_REG high byte |
| `10` | `MHL` | Move immediate byte into H_REG low byte |

### Arithmetic

| Opcode | Instruction | Description |
|---|---|---|
| `20` | `ADD` | Add the encoded operand to A_REG and store the result in A_REG |
| `21` | `SUB` | Subtract the encoded operand from A_REG and store the result in A_REG |
| `22` | `MUL` | Multiply A_REG by the encoded operand |
| `23` | `DIV` | Divide A_REG by the encoded operand |
| `24` | `INC` | Increment A_REG |
| `25` | `DEC` | Decrement A_REG |

For `ADD`, the operand encoding must specify whether the parameter represents an immediate value or a register. Full 16-bit operands can be loaded into a register using two byte-loading instructions before performing arithmetic.

### Status Flags

| Opcode | Instruction | Description |
|---|---|---|
| `50` | `CLF` | Clear status flags |
| `51` | `STF` | Set status flags according to the defined operation |

The exact status flags and their behavior remain to be defined.

### Control Flow

| Opcode | Instruction | Description |
|---|---|---|
| `60` | `JMP` | Jump to a program location |
| `61` | `JZ` | Jump if the zero flag is set |
| `62` | `JNZ` | Jump if the zero flag is not set |
| `63` | `JC` | Jump if the carry flag is set |
| `64` | `JNC` | Jump if the carry flag is not set |

The program-location encoding must be defined for the Scratch implementation.

### Display and CPU Control

| Opcode | Instruction | Description |
|---|---|---|
| `90` | `OUT` | Output a value to the display |
| `91` | `HALT` | Stop CPU execution |

These are provisional assignments for the first working prototype.

## First Test Program

The first test program loads the ASCII code for the letter `A` into `A_REG`, outputs it, and stops.

```text
0100
0241
9000
9100
```

Combined into a single hexadecimal string:

```text
0100024190009100
```

Expected register value:

```text
A_REG = 0041
```

`0041` is hexadecimal for decimal `65`, the ASCII code for the character `A`.

Expected display:

```text
A
```

## Scratch Implementation

The initial Scratch CPU should contain:

- Eight 16-bit general-purpose registers.
- A program counter (`PC`).
- An instruction decoder.
- A display.
- A mechanism for executing instructions sequentially.
- A halt state to stop execution.

Scratch variables may be used to represent registers, the program counter, the current instruction, and display output.

## Development Goals

1. Implement the registers.
2. Implement the instruction decoder.
3. Load and execute hexadecimal instructions.
4. Display characters or values.
5. Implement arithmetic operations.
6. Add conditional jumps and status flags.
7. Expand the instruction set as needed.

## Project Status

ArchS_16 is an early-stage CPU architecture under development.

The opcode map is a proposal, not a complete specification. Before implementing the full instruction set, define parameter encoding, arithmetic behavior, program addressing, display output, and the execution model precisely.
