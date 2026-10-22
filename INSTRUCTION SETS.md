# ArchS_16

**ArchS_16 (Architecture 16-bit)** is a custom 16-bit CPU architecture designed for a virtual computer built in Scratch as part of the NEXUS project.

The goal is to build a working CPU step by step, starting with registers, basic arithmetic, a display, input, and instruction execution.

## 1. Architecture Overview

ArchS_16 uses fixed-length hexadecimal machine instructions.

| Property | Specification |
|---|---|
| Architecture | 16-bit |
| Instruction length | 16 bits (4 hexadecimal digits) |
| Opcode length | 8 bits (2 hexadecimal digits) |
| Parameter length | 8 bits (2 hexadecimal digits) |
| General-purpose registers | 8 |
| Internal CPU RAM | None |
| Display | Text and numbers |
| Input | Numeric input |
| Stop instruction | `FF` (`HLT`) |

Each instruction consists of four hexadecimal digits:

`OOPP`

- `OO` is the opcode, which identifies the operation.
- `PP` is the parameter, which provides additional information.

For example, `01AB` means opcode `01` with parameter `AB`.

## 2. How the CPU Works

The CPU repeatedly performs these steps:

1. Read the next instruction from the program.
2. Decode its opcode and parameter.
3. Execute the operation.
4. Move to the next instruction unless a jump or stop instruction changes execution.

The program counter (`PC`) tracks which instruction is being executed.

The CPU does not require internal RAM. Registers retain values between instructions. In Scratch, variables may simulate the registers, program counter, and other CPU state.

## 3. Registers

ArchS_16 has eight general-purpose 16-bit registers.

| Register | Purpose |
|---|---|
| `A_REG` | Primary register, used for arithmetic and display output |
| `B_REG` | Secondary register, commonly used as an arithmetic operand |
| `C_REG` | General-purpose storage |
| `D_REG` | General-purpose storage |
| `E_REG` | General-purpose storage |
| `F_REG` | General-purpose storage |
| `G_REG` | General-purpose storage |
| `H_REG` | General-purpose storage |

Each register contains 16 bits, represented by four hexadecimal digits.

For example:

`A_REG = ABCD`

The high byte is `AB`, and the low byte is `CD`.

### Loading a 16-bit Value

A complete 16-bit value can be loaded using two instructions.

```text
01AB
02CD
```

- `01AB` loads `AB` into the high byte of `A_REG`.
- `02CD` loads `CD` into the low byte of `A_REG`.

After both instructions:

`A_REG = ABCD`

## 4. Instruction Set

The following instructions form the initial design. Instructions can be added or revised as the Scratch CPU develops.

### 4.1 Basic Instructions

| Opcode | Mnemonic | Description |
|---|---|---|
| `00` | `NOP` | Do nothing |
| `FF` | `HLT` | Halt CPU execution |

`FF` is the designated halt opcode.

### 4.2 Register Loading

The register-loading instructions place an 8-bit parameter into either the high or low byte of a 16-bit register.

| Opcode | Mnemonic | Description |
|---|---|---|
| `01` | `MAH` | Load the high byte of `A_REG` |
| `02` | `MAL` | Load the low byte of `A_REG` |
| `03` | `MBH` | Load the high byte of `B_REG` |
| `04` | `MBL` | Load the low byte of `B_REG` |
| `05` | `MCH` | Load the high byte of `C_REG` |
| `06` | `MCL` | Load the low byte of `C_REG` |
| `07` | `MDH` | Load the high byte of `D_REG` |
| `08` | `MDL` | Load the low byte of `D_REG` |
| `09` | `MEH` | Load the high byte of `E_REG` |
| `0A` | `MEL` | Load the low byte of `E_REG` |
| `0B` | `MFH` | Load the high byte of `F_REG` |
| `0C` | `MFL` | Load the low byte of `F_REG` |
| `0D` | `MGH` | Load the high byte of `G_REG` |
| `0E` | `MGL` | Load the low byte of `G_REG` |
| `0F` | `MHH` | Load the high byte of `H_REG` |
| `10` | `MHL` | Load the low byte of `H_REG` |

### 4.3 Arithmetic

Arithmetic instructions operate directly on CPU registers.

| Opcode | Mnemonic | Description |
|---|---|---|
| `20` | `ADD` | Add `B_REG` to `A_REG` |
| `21` | `SUB` | Subtract `B_REG` from `A_REG` |
| `22` | `ADDI` | Add the 8-bit parameter to `A_REG` |
| `23` | `SUBI` | Subtract the 8-bit parameter from `A_REG` |
| `24` | `INC` | Increment `A_REG` by 1 |
| `25` | `DEC` | Decrement `A_REG` by 1 |

Results are stored in `A_REG`. Arithmetic results are 16-bit values; overflow and underflow behavior must be defined by the implementation.

### 4.4 Display Output

The display supports both numeric and text output.

| Opcode | Mnemonic | Description |
|---|---|---|
| `90` | `AOUT` | Output `A_REG` to the display |
| `91` | `BOUT` | Output `B_REG` to the display |
| `92` | `CLS` | Clear the display |

For `AOUT` and `BOUT`, the parameter selects the output mode:

| Parameter | Mode | Description |
|---|---|---|
| `00` | Numeric | Display the register value as a decimal number |
| `01` | Character | Interpret the low byte as a character code and display that character |

For example, `A_REG = 0041` represents decimal `65`. In character mode, the low byte `41` is hexadecimal for decimal `65`, which corresponds to `A` in ASCII.

The character mode initially uses ASCII for the basic character set. Extended text handling can be defined later.

`CLS` clears the display regardless of its previous contents.

### 4.5 Input

| Opcode | Mnemonic | Description |
|---|---|---|
| `A0` | `AIN` | Read numeric input into `A_REG` |
| `A1` | `BIN` | Read numeric input into `B_REG` |

The input operation pauses execution until a value is supplied, or follows another clearly defined input policy in the Scratch implementation.

## 5. Display Design

The virtual computer's display should be able to show:

- Decimal numbers, such as `1234`.
- Individual characters, such as `A`.
- Text built from character output.
- A cleared screen when `CLS` executes.

For the initial Scratch version, display output can be implemented using a variable such as `DisplayText`.

The CPU updates this variable when an output instruction executes. A display sprite reads the variable and renders the result.

## 6. Example Programs

### 6.1 Display a Number

This program loads the hexadecimal value `1234` into `A_REG` and displays it as a decimal number.

```text
0112
0234
9000
FF00
```

Instruction-by-instruction:

| Machine code | Meaning |
|---|---|
| `0112` | Set the high byte of `A_REG` to `12` |
| `0234` | Set the low byte of `A_REG` to `34` |
| `9000` | Display `A_REG` numerically |
| `FF00` | Halt the CPU |

The resulting register value is `1234` hexadecimal, which is `4660` decimal. Therefore, the numeric display should show `4660`.

### 6.2 Display a Character

This program loads the ASCII code for `A` and displays the character.

```text
0100
0241
9001
FF00
```

Instruction-by-instruction:

| Machine code | Meaning |
|---|---|
| `0100` | Set the high byte of `A_REG` to `00` |
| `0241` | Set the low byte of `A_REG` to `41` |
| `9001` | Display the low byte as a character |
| `FF00` | Halt the CPU |

Expected display:

`A`

### 6.3 Add Two Values

This program loads two values into `A_REG` and `B_REG`, adds them, and displays the result.

```text
0100
020A
0300
0405
2000
9000
FF00
```

Instruction-by-instruction:

| Machine code | Meaning |
|---|---|
| `0100` | Set the high byte of `A_REG` to `00` |
| `020A` | Set the low byte of `A_REG` to `0A` (10) |
| `0300` | Set the high byte of `B_REG` to `00` |
| `0405` | Set the low byte of `B_REG` to `05` (5) |
| `2000` | Add `B_REG` to `A_REG` |
| `9000` | Display the result numerically |
| `FF00` | Halt the CPU |

Expected result:

`15`

## 7. Program Representation

Instructions are stored as a sequence supplied to the virtual computer. The Scratch implementation may represent the program as a list containing one four-digit hexadecimal instruction per item.

For example:

```text
01AB
02CD
9000
FF00
```

The CPU reads one list item at a time. The program counter identifies the current instruction.

This program representation is a property of the Scratch implementation; it does not require internal CPU RAM in the architecture.

## 8. Implementation Rules

1. Every instruction contains exactly four hexadecimal digits.
2. The first byte is the opcode.
3. The second byte is the parameter.
4. Registers retain their values between instructions.
5. Each general-purpose register is 16 bits wide.
6. Full 16-bit constants are loaded using two byte-loading instructions.
7. `ADD` and `SUB` use `A_REG` and `B_REG`.
8. `ADDI` and `SUBI` use the 8-bit immediate parameter.
9. `AOUT` and `BOUT` support numeric and character output through their parameters.
10. `CLS` clears the display.
11. `FF` (`HLT`) stops CPU execution.
12. Scratch variables may simulate CPU state without introducing architectural internal RAM.

## 9. Future Development

Potential future additions include:

- Register-to-register copying.
- Bitwise operations such as `AND`, `OR`, and `XOR`.
- Shifts and rotations.
- Status flags.
- Conditional and unconditional jumps.
- More input and output instructions.
- Additional character and text handling.

These features should be added only after their behavior and parameter formats have been defined.

## 10. Project Status

ArchS_16 is an early-stage CPU architecture being developed for a Scratch-based virtual computer.

The immediate goal is a working prototype that can load register values, perform simple arithmetic, display numbers and characters, accept numeric input, and stop execution using `FF` (`HLT`).

The instruction set will evolve as the architecture is implemented and tested.
