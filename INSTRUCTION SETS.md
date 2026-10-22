# ArchS_16

**ArchS_16** (Scratch Architecture 16-bit) is a custom 16-bit CPU architecture designed around hexadecimal machine instructions. It is intended to be implemented and simulated in Scratch as part of the **NEXUS** virtual computer project.

> **Status:** Early architecture proposal — opcode assignments and semantics are not yet frozen.

## Core design

- **Word size:** 16 bits.
- **Instruction width:** Exactly 16 bits, written as four hexadecimal digits (`0000`–`FFFF`).
- **Opcode field:** The first two hexadecimal digits select an instruction (`00`–`FF`).
- **Parameter field:** The last two hexadecimal digits provide an 8-bit parameter.
- **Execution model:** Instructions are executed sequentially unless an instruction changes the control flow.
- **Instruction storage:** The architecture does not require CPU RAM to store instructions. Instructions may be supplied by the external program source or simulator.
- **Registers:** CPU registers retain values between instructions. This is separate from having RAM for instruction storage.
- **16-bit constants:** A full 16-bit value can be loaded in two instructions, one for the high byte and one for the low byte.

## Instruction format

Each instruction is exactly four hexadecimal digits:

```text
[ OPCODE ][ PARAMETER ]
   8 bits     8 bits
```

For example, `01AB` consists of opcode `01` and parameter `AB`.

### Loading a 16-bit value

`MAH` means **Move A Register High** and `MAL` means **Move A Register Low**.

```text
01AB    ; MAH AB — set A_REG bits 15–8 to AB
02CD    ; MAL CD — set A_REG bits 7–0 to CD
```

After both instructions, `A_REG = ABCD`.

The comments above are explanatory notation for humans. The CPU executes the four-digit hexadecimal words, not the text names.

## Register model

The initial proposal contains eight general-purpose 16-bit registers:

| Register | High byte | Low byte |
|---|---|---|
| `A_REG` | `AH` | `AL` |
| `B_REG` | `BH` | `BL` |
| `C_REG` | `CH` | `CL` |
| `D_REG` | `DH` | `DL` |
| `E_REG` | `EH` | `EL` |
| `F_REG` | `FH` | `FL` |
| `G_REG` | `GH` | `GL` |
| `H_REG` | `HH` | `HL` |

The high and low byte instructions modify only their respective eight bits. The other half of the register remains unchanged.

## Opcode map

The opcode space contains 256 possible values, from `00` to `FF`. The map below is a **draft allocation**. A name and proposed purpose do not yet guarantee that the instruction's parameter encoding or execution semantics have been fully specified.

`xx` represents any 8-bit parameter (`00`–`FF`), so `01xx` describes the family of words `0100` through `01FF`.

### `00`–`0F` — Immediate byte loads

| Opcode | Name | Proposed operation |
|---|---|---|
| `00xx` | `NOP` | No operation |
| `01xx` | `MAH` | Load parameter into the high byte of `A_REG` |
| `02xx` | `MAL` | Load parameter into the low byte of `A_REG` |
| `03xx` | `MBH` | Load parameter into the high byte of `B_REG` |
| `04xx` | `MBL` | Load parameter into the low byte of `B_REG` |
| `05xx` | `MCH` | Load parameter into the high byte of `C_REG` |
| `06xx` | `MCL` | Load parameter into the low byte of `C_REG` |
| `07xx` | `MDH` | Load parameter into the high byte of `D_REG` |
| `08xx` | `MDL` | Load parameter into the low byte of `D_REG` |
| `09xx` | `MEH` | Load parameter into the high byte of `E_REG` |
| `0Axx` | `MEL` | Load parameter into the low byte of `E_REG` |
| `0Bxx` | `MFH` | Load parameter into the high byte of `F_REG` |
| `0Cxx` | `MFL` | Load parameter into the low byte of `F_REG` |
| `0Dxx` | `MGH` | Load parameter into the high byte of `G_REG` |
| `0Exx` | `MGL` | Load parameter into the low byte of `G_REG` |
| `0Fxx` | `MHH` | Load parameter into the high byte of `H_REG` |

### `10`–`1F` — Register operations

| Opcode | Name | Proposed operation |
|---|---|---|
| `10xx` | `MHL` | Load parameter into the low byte of `H_REG` |
| `11xx` | `RCPY` | Copy one register to another |
| `12xx` | `RSWP` | Swap two registers |
| `13xx` | `RCLR` | Clear a selected register |
| `14xx` | `RINC` | Increment a selected register |
| `15xx` | `RDEC` | Decrement a selected register |
| `16xx` | `RNEG` | Negate a selected value |
| `17xx` | `RNOT` | Invert every bit in a selected register |
| `18xx` | `RTEST` | Test a register value |
| `19xx` | `RSEL` | Select a register |
| `1Axx` | `RGET` | Read through a selected-register interface |
| `1Bxx` | `RPUT` | Write through a selected-register interface |
| `1Cxx` | `RPAIR` | Select a register pair |
| `1Dxx` | `RJOIN` | Combine two 8-bit register parts |
| `1Exx` | `RSPLIT` | Split a 16-bit value into two bytes |
| `1Fxx` | `RINFO` | Read register information |

### `20`–`2F` — Arithmetic

| Opcode | Name | Proposed operation |
|---|---|---|
| `20xx` | `ADD` | Add selected values |
| `21xx` | `SUB` | Subtract selected values |
| `22xx` | `MUL` | Multiply values |
| `23xx` | `DIV` | Divide values |
| `24xx` | `MOD` | Calculate a remainder |
| `25xx` | `ADC` | Add with carry |
| `26xx` | `SBC` | Subtract with borrow |
| `27xx` | `ABS` | Calculate absolute value |
| `28xx` | `MIN` | Select the smaller value |
| `29xx` | `MAX` | Select the larger value |
| `2Axx` | `SQRT` | Calculate integer square root |
| `2Bxx` | `POW2` | Calculate a power of two |
| `2Cxx` | `SIGN` | Determine a value's sign |
| `2Dxx` | `SATADD` | Saturating addition |
| `2Exx` | `SATSUB` | Saturating subtraction |
| `2Fxx` | `RAND` | Generate a pseudorandom value |

### `30`–`3F` — Bitwise logic

| Opcode | Name | Proposed operation |
|---|---|---|
| `30xx` | `AND` | Bitwise AND |
| `31xx` | `OR` | Bitwise OR |
| `32xx` | `XOR` | Bitwise XOR |
| `33xx` | `NOT` | Bitwise NOT |
| `34xx` | `NAND` | Bitwise NAND |
| `35xx` | `NOR` | Bitwise NOR |
| `36xx` | `XNOR` | Bitwise XNOR |
| `37xx` | `BSET` | Set a selected bit |
| `38xx` | `BCLR` | Clear a selected bit |
| `39xx` | `BFLIP` | Toggle a selected bit |
| `3Axx` | `BTST` | Test a selected bit |
| `3Bxx` | `BCOUNT` | Count set bits |
| `3Cxx` | `CLZ` | Count leading zero bits |
| `3Dxx` | `CTZ` | Count trailing zero bits |
| `3Exx` | `BEXT` | Extract a bit field |
| `3Fxx` | `BINS` | Insert a bit field |

### `40`–`4F` — Shifts and bit manipulation

| Opcode | Name | Proposed operation |
|---|---|---|
| `40xx` | `SHL` | Shift bits left |
| `41xx` | `SHR` | Shift bits right logically |
| `42xx` | `SAR` | Shift bits right arithmetically |
| `43xx` | `ROL` | Rotate bits left |
| `44xx` | `ROR` | Rotate bits right |
| `45xx` | `RCL` | Rotate left through carry |
| `46xx` | `RCR` | Rotate right through carry |
| `47xx` | `SWAPB` | Swap the two bytes |
| `48xx` | `REV8` | Reverse bit order |
| `49xx` | `ZEXT` | Extend a value with zeroes |
| `4Axx` | `SEXT` | Sign-extend a value |
| `4Bxx` | `MASK` | Apply a bit mask |
| `4Cxx` | `EXMASK` | Extract a masked value |
| `4Dxx` | `BITREV` | Reverse bit order |
| `4Exx` | `BYTEGET` | Select a byte |
| `4Fxx` | `BYTEPUT` | Replace a byte |

### `50`–`5F` — Comparisons and flags

| Opcode | Name | Proposed operation |
|---|---|---|
| `50xx` | `CMP` | Compare two values |
| `51xx` | `CMPEQ` | Test equality |
| `52xx` | `CMPNE` | Test inequality |
| `53xx` | `CMPLT` | Test less-than |
| `54xx` | `CMPGT` | Test greater-than |
| `55xx` | `CMPLE` | Test less-than or equal |
| `56xx` | `CMPGE` | Test greater-than or equal |
| `57xx` | `FGET` | Read status flags |
| `58xx` | `FSET` | Set status flags |
| `59xx` | `FCLR` | Clear status flags |
| `5Axx` | `FNOT` | Invert selected flags |
| `5Bxx` | `FCOPY` | Copy status flags |
| `5Cxx` | `FCMP` | Compare status flags |
| `5Dxx` | `CGET` | Read carry flag |
| `5Exx` | `OGET` | Read overflow flag |
| `5Fxx` | `ZGET` | Read zero flag |

### `60`–`6F` — Jumps and control flow

| Opcode | Name | Proposed operation |
|---|---|---|
| `60xx` | `JMP` | Jump to a target address |
| `61xx` | `JZ` | Jump if the zero flag is set |
| `62xx` | `JNZ` | Jump if the zero flag is clear |
| `63xx` | `JC` | Jump if carry is set |
| `64xx` | `JNC` | Jump if carry is clear |
| `65xx` | `JO` | Jump if overflow is set |
| `66xx` | `JNO` | Jump if overflow is clear |
| `67xx` | `JN` | Jump if the result is negative |
| `68xx` | `JP` | Jump if the result is positive |
| `69xx` | `JLT` | Jump if less than |
| `6Axx` | `JGT` | Jump if greater than |
| `6Bxx` | `JLE` | Jump if less than or equal |
| `6Cxx` | `JGE` | Jump if greater than or equal |
| `6Dxx` | `CALL` | Call a subroutine |
| `6Exx` | `RET` | Return from a subroutine |
| `6Fxx` | `JREL` | Relative jump |

### `70`–`7F` — Execution control

| Opcode | Name | Proposed operation |
|---|---|---|
| `70xx` | `HALT` | Stop CPU execution |
| `71xx` | `WAIT` | Wait for an event |
| `72xx` | `STEP` | Execute one instruction step |
| `73xx` | `BREAK` | Pause for debugging |
| `74xx` | `RESUME` | Resume execution |
| `75xx` | `RESET` | Reset CPU state |
| `76xx` | `SYNC` | Synchronize execution |
| `77xx` | `YIELD` | Temporarily yield execution |
| `78xx` | `LOOP` | Repeat an instruction block |
| `79xx` | `DJNZ` | Decrement a counter and jump if nonzero |
| `7Axx` | `LOOPEND` | End a loop |
| `7Bxx` | `LOOPSET` | Configure a loop parameter |
| `7Cxx` | `SLEEP` | Put the CPU into a sleep state |
| `7Dxx` | `WAKE` | Wake the CPU |
| `7Exx` | `FENCE` | Wait for earlier operations to complete |
| `7Fxx` | `CHECK` | Check execution status |

### `80`–`8F` — Memory and segments

These operations may access **external memory**. They do not imply that the CPU has internal RAM for instruction storage.

| Opcode | Name | Proposed operation |
|---|---|---|
| `80xx` | `MEMR` | Read from memory |
| `81xx` | `MEMW` | Write to memory |
| `82xx` | `MEMCPY` | Copy a memory range |
| `83xx` | `MEMFILL` | Fill a memory range |
| `84xx` | `MEMCMP` | Compare memory |
| `85xx` | `MEMTEST` | Test memory access |
| `86xx` | `SEGSEL` | Select a segment |
| `87xx` | `SEGGET` | Read the selected segment |
| `88xx` | `SEGCHK` | Validate a segment |
| `89xx` | `SEGJMP` | Jump to a segmented address |
| `8Axx` | `SEGREAD` | Read from a segment |
| `8Bxx` | `SEGWRITE` | Write to a segment |
| `8Cxx` | `ADRGET` | Read an address register |
| `8Dxx` | `ADRSET` | Set an address register |
| `8Exx` | `ADRADD` | Increment an address |
| `8Fxx` | `ADRSUB` | Decrement an address |

### `90`–`9F` — Input and output

| Opcode | Name | Proposed operation |
|---|---|---|
| `90xx` | `IN` | Read input |
| `91xx` | `OUT` | Write output |
| `92xx` | `IN8` | Read 8-bit input |
| `93xx` | `OUT8` | Write 8-bit output |
| `94xx` | `IN16` | Read 16-bit input |
| `95xx` | `OUT16` | Write 16-bit output |
| `96xx` | `INCHR` | Read a character |
| `97xx` | `OUTCHR` | Write a character |
| `98xx` | `OUTHEX` | Display a hexadecimal value |
| `99xx` | `OUTDEC` | Display a decimal value |
| `9Axx` | `INBUF` | Read input into a buffer |
| `9Bxx` | `OUTSTR` | Display a string |
| `9Cxx` | `IOGET` | Read an I/O port |
| `9Dxx` | `IOSET` | Write to an I/O port |
| `9Exx` | `IOCHECK` | Check I/O status |
| `9Fxx` | `IOWAIT` | Wait for an I/O device |

### `A0`–`AF` — Constants and data operations

| Opcode | Name | Proposed operation |
|---|---|---|
| `A0xx` | `LIT` | Process an immediate constant |
| `A1xx` | `CONST` | Select a constant |
| `A2xx` | `SIGNEXT` | Sign-extend a value |
| `A3xx` | `ZEROEXT` | Zero-extend a value |
| `A4xx` | `PACK` | Pack data |
| `A5xx` | `UNPACK` | Unpack data |
| `A6xx` | `ENDIAN` | Change byte order |
| `A7xx` | `DATACOPY` | Copy data |
| `A8xx` | `DATASWAP` | Swap data |
| `A9xx` | `DATACHECK` | Validate data |
| `AAxx` | `ENCODE` | Encode data |
| `ABxx` | `DECODE` | Decode data |
| `ACxx` | `CRC` | Calculate a CRC checksum |
| `ADxx` | `HASH` | Calculate a hash |
| `AExx` | `DATACMP` | Compare data |
| `AFxx` | `DATASEL` | Select a data source |

### `B0`–`BF` — Advanced arithmetic

| Opcode | Name | Proposed operation |
|---|---|---|
| `B0xx` | `UMUL` | Unsigned multiplication |
| `B1xx` | `SMUL` | Signed multiplication |
| `B2xx` | `UDIV` | Unsigned division |
| `B3xx` | `SDIV` | Signed division |
| `B4xx` | `UMOD` | Unsigned remainder |
| `B5xx` | `SMOD` | Signed remainder |
| `B6xx` | `MAC` | Multiply and accumulate |
| `B7xx` | `FIXMUL` | Fixed-point multiplication |
| `B8xx` | `FIXDIV` | Fixed-point division |
| `B9xx` | `CLAMP` | Clamp a value to a range |
| `BAxx` | `AVG` | Calculate an average |
| `BBxx` | `DIST` | Calculate a difference between values |
| `BCxx` | `MINMAX` | Calculate minimum and maximum |
| `BDxx` | `POW` | Raise a value to a power |
| `BExx` | `LOG2` | Calculate base-2 logarithm |
| `BFxx` | `MATHX` | Extended arithmetic operation |

### `C0`–`CF` — Bit fields and validation

| Opcode | Name | Proposed operation |
|---|---|---|
| `C0xx` | `FIELDGET` | Read a bit field |
| `C1xx` | `FIELDSET` | Write a bit field |
| `C2xx` | `FIELDCLR` | Clear a bit field |
| `C3xx` | `FIELDCPY` | Copy a bit field |
| `C4xx` | `FIELDTEST` | Test a bit field |
| `C5xx` | `FIELDAND` | Apply AND to a bit field |
| `C6xx` | `FIELDOR` | Apply OR to a bit field |
| `C7xx` | `FIELDXOR` | Apply XOR to a bit field |
| `C8xx` | `POPCNT` | Count set bits |
| `C9xx` | `PARITY` | Calculate parity |
| `CAxx` | `CRCSTEP` | Perform one CRC step |
| `CBxx` | `CRCINIT` | Initialize CRC state |
| `CCxx` | `CRCFINAL` | Finalize CRC calculation |
| `CDxx` | `CHECKSUM` | Calculate a checksum |
| `CExx` | `VERIFY` | Verify data |
| `CFxx` | `VALIDATE` | Validate a value or structure |

### `D0`–`DF` — Interrupts and exceptions

| Opcode | Name | Proposed operation |
|---|---|---|
| `D0xx` | `INT` | Trigger an interrupt |
| `D1xx` | `IRET` | Return from an interrupt |
| `D2xx` | `INTEN` | Enable interrupts |
| `D3xx` | `INTDIS` | Disable interrupts |
| `D4xx` | `INTGET` | Read interrupt status |
| `D5xx` | `INTSET` | Set interrupt status |
| `D6xx` | `TRAP` | Trigger a software trap |
| `D7xx` | `EXC` | Raise an exception |
| `D8xx` | `ERRGET` | Read error status |
| `D9xx` | `ERRCLR` | Clear error status |
| `DAxx` | `ERRSET` | Set error status |
| `DBxx` | `ASSERT` | Check a condition |
| `DCxx` | `PANIC` | Stop after a fatal error |
| `DDxx` | `RECOVER` | Begin error recovery |
| `DExx` | `VECGET` | Read an interrupt vector |
| `DFxx` | `VECSET` | Set an interrupt vector |

### `E0`–`EF` — Debugging and system information

| Opcode | Name | Proposed operation |
|---|---|---|
| `E0xx` | `CPUINFO` | Read CPU information |
| `E1xx` | `CPUID` | Read CPU identification |
| `E2xx` | `CPUMODE` | Change CPU mode |
| `E3xx` | `REGDUMP` | Display register contents |
| `E4xx` | `TRACE` | Trace instruction execution |
| `E5xx` | `PROFILE` | Measure execution |
| `E6xx` | `BREAKPT` | Set a breakpoint |
| `E7xx` | `WATCHPT` | Set a watchpoint |
| `E8xx` | `DBGGET` | Read debug status |
| `E9xx` | `DBGSET` | Change debug status |
| `EAxx` | `SYSINFO` | Read system information |
| `EBxx` | `TIMERGET` | Read a timer |
| `ECxx` | `TIMERSET` | Set a timer |
| `EDxx` | `CLOCKGET` | Read a clock |
| `EExx` | `VERSION` | Read architecture version |
| `EFxx` | `FEATURE` | Query CPU features |

### `F0`–`FF` — CPU control and extensions

| Opcode | Name | Proposed operation |
|---|---|---|
| `F0xx` | `SYS` | Execute a system function |
| `F1xx` | `SYSCALL` | Call a system service |
| `F2xx` | `SYSRET` | Return from a system service |
| `F3xx` | `PRIVGET` | Read privilege level |
| `F4xx` | `PRIVSET` | Change privilege level |
| `F5xx` | `MICRO` | Execute a micro-operation |
| `F6xx` | `MICROEND` | End a micro-operation sequence |
| `F7xx` | `CPUCFG` | Configure the CPU |
| `F8xx` | `CPUSTAT` | Read CPU status |
| `F9xx` | `CPUCTRL` | Change CPU control state |
| `FAxx` | `EXT0` | Extension operation 0 |
| `FBxx` | `EXT1` | Extension operation 1 |
| `FCxx` | `EXT2` | Extension operation 2 |
| `FDxx` | `EXT3` | Extension operation 3 |
| `FExx` | `RESERVED` | Reserved for future use |
| `FFxx` | `CPUEND` | End CPU execution |

## Important implementation notes

This table is a broad opcode proposal, not yet a complete executable specification. Before implementing the CPU, each instruction needs a precise definition of:

1. **Parameter encoding:** how the 8-bit parameter selects registers, flags, bit positions, ports, or other operands.
2. **Side effects:** which registers and flags are read or modified.
3. **Control flow:** how jumps and calls identify their targets without exceeding the 16-bit instruction width.
4. **External devices:** which instructions require an external memory or I/O interface.
5. **Unsupported instructions:** whether an undefined opcode halts, raises an exception, or is ignored.

A few proposed instructions overlap in purpose and may be consolidated as the architecture develops. Opcodes should be considered provisional until their behavior is specified and tested.

## Example program

This example loads `ABCD` into `A_REG`:

```text
01AB
02CD
```

After execution:

```text
A_REG = ABCD
```

## Project goals

- Define a consistent 16-bit instruction set.
- Build a CPU simulator in Scratch.
- Specify registers, flags, control flow, and parameter encoding.
- Add external memory and I/O only where needed.
- Test instruction behavior with small hexadecimal programs.

---

**ArchS_16 is a work in progress.** The hexadecimal opcode map is the starting point for a more precise CPU specification.
