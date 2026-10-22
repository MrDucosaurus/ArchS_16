
# ArchS_16

**ArchS_16 (Architecture 16-bit)** is a custom 16-bit CPU architecture designed for a virtual computer built in Scratch as part of the NEXUS project.

## 1. Architecture

- Instruction width: 16 bits (4 hexadecimal digits).
- Opcode width: 8 bits (the first 2 hexadecimal digits).
- Parameter width: 8 bits (the last 2 hexadecimal digits).
- Register width: 16 bits.
- General-purpose registers: `A_REG` through `H_REG`.
- Internal CPU RAM: None.
- Display output: Supported through CPU instructions.
- Instructions execute sequentially unless a control-flow instruction changes the program counter.

Each instruction is encoded as:

`OOPP`

- `OO`: 8-bit opcode.
- `PP`: 8-bit parameter.

Example:

`01AB` means opcode `01` with parameter `AB`.

A 16-bit value is loaded into a register using two instructions, one for the high byte and one for the low byte.

## 2. Registers

| Register | Width | Description |
|---|---:|---|
| `A_REG` | 16-bit | Primary arithmetic and output register |
| `B_REG` | 16-bit | General-purpose register |
| `C_REG` | 16-bit | General-purpose register |
| `D_REG` | 16-bit | General-purpose register |
| `E_REG` | 16-bit | General-purpose register |
| `F_REG` | 16-bit | General-purpose register |
| `G_REG` | 16-bit | General-purpose register |
| `H_REG` | 16-bit | General-purpose register |
| `PC` | 16-bit | Program counter used by the implementation |
| `FLAGS` | Implementation-defined | CPU status flags |

`PC` and `FLAGS` are CPU state, not general-purpose RAM.

Each general-purpose register has a high byte and a low byte. For example, `A_REG = ABCD` means high byte `AB` and low byte `CD`.

## 3. Complete Opcode Table

The following table allocates all 256 possible 8-bit opcodes. Some instructions are provisional and require detailed behavior to be defined before implementation.

### `00`–`0F`: A_REG and B_REG Loading

| Opcode | Mnemonic | Description |
|---|---|---|
| `00` | `NOP` | Do nothing |
| `01` | `MAH` | Load parameter into the high byte of `A_REG` |
| `02` | `MAL` | Load parameter into the low byte of `A_REG` |
| `03` | `MBH` | Load parameter into the high byte of `B_REG` |
| `04` | `MBL` | Load parameter into the low byte of `B_REG` |
| `05` | `MCH` | Load parameter into the high byte of `C_REG` |
| `06` | `MCL` | Load parameter into the low byte of `C_REG` |
| `07` | `MDH` | Load parameter into the high byte of `D_REG` |
| `08` | `MDL` | Load parameter into the low byte of `D_REG` |
| `09` | `MEH` | Load parameter into the high byte of `E_REG` |
| `0A` | `MEL` | Load parameter into the low byte of `E_REG` |
| `0B` | `MFH` | Load parameter into the high byte of `F_REG` |
| `0C` | `MFL` | Load parameter into the low byte of `F_REG` |
| `0D` | `MGH` | Load parameter into the high byte of `G_REG` |
| `0E` | `MGL` | Load parameter into the low byte of `G_REG` |
| `0F` | `MHH` | Load parameter into the high byte of `H_REG` |

### `10`–`1F`: Register Operations

| Opcode | Mnemonic | Description |
|---|---|---|
| `10` | `MHL` | Load parameter into the low byte of `H_REG` |
| `11` | `MOV` | Copy a register value to another register |
| `12` | `XCHG` | Exchange two register values |
| `13` | `CLRA` | Clear `A_REG` |
| `14` | `CLRB` | Clear `B_REG` |
| `15` | `CLRC` | Clear `C_REG` |
| `16` | `CLRD` | Clear `D_REG` |
| `17` | `CLRE` | Clear `E_REG` |
| `18` | `CLRF` | Clear `F_REG` |
| `19` | `CLRG` | Clear `G_REG` |
| `1A` | `CLRH` | Clear `H_REG` |
| `1B` | `SWAPA` | Swap the high and low bytes of `A_REG` |
| `1C` | `SWAPB` | Swap the high and low bytes of `B_REG` |
| `1D` | `PUSHR` | Reserved for future register-stack support |
| `1E` | `POPR` | Reserved for future register-stack support |
| `1F` | `MOVI` | Reserved for a future immediate-load format |

### `20`–`2F`: Arithmetic

| Opcode | Mnemonic | Description |
|---|---|---|
| `20` | `ADD` | Add the encoded operand to `A_REG`; store the result in `A_REG` |
| `21` | `SUB` | Subtract the encoded operand from `A_REG`; store the result in `A_REG` |
| `22` | `MUL` | Multiply `A_REG` by the encoded operand |
| `23` | `DIV` | Divide `A_REG` by the encoded operand |
| `24` | `INC` | Increment `A_REG` by 1 |
| `25` | `DEC` | Decrement `A_REG` by 1 |
| `26` | `MOD` | Calculate `A_REG` modulo the encoded operand |
| `27` | `NEG` | Negate `A_REG` using the defined 16-bit arithmetic rules |
| `28` | `ABS` | Set `A_REG` to its absolute value |
| `29` | `ADDB` | Add `B_REG` to `A_REG` |
| `2A` | `SUBB` | Subtract `B_REG` from `A_REG` |
| `2B` | `MULB` | Multiply `A_REG` by `B_REG` |
| `2C` | `DIVB` | Divide `A_REG` by `B_REG` |
| `2D` | `ADC` | Add an operand and the carry flag to `A_REG` |
| `2E` | `SBB` | Subtract an operand and borrow from `A_REG` |
| `2F` | `ARITHX` | Reserved for future arithmetic extensions |

### `30`–`3F`: Bitwise Logic

| Opcode | Mnemonic | Description |
|---|---|---|
| `30` | `AND` | Bitwise AND of `A_REG` and the encoded operand |
| `31` | `OR` | Bitwise OR of `A_REG` and the encoded operand |
| `32` | `XOR` | Bitwise XOR of `A_REG` and the encoded operand |
| `33` | `NOT` | Bitwise NOT of `A_REG` |
| `34` | `NAND` | Bitwise NAND of `A_REG` and the encoded operand |
| `35` | `NOR` | Bitwise NOR of `A_REG` and the encoded operand |
| `36` | `XNOR` | Bitwise XNOR of `A_REG` and the encoded operand |
| `37` | `BITSET` | Set a selected bit in `A_REG` |
| `38` | `BITCLR` | Clear a selected bit in `A_REG` |
| `39` | `BITTST` | Test a selected bit in `A_REG` |
| `3A` | `BITINV` | Invert a selected bit in `A_REG` |
| `3B` | `MASK` | Apply a bit mask to `A_REG` |
| `3C` | `BEXTR` | Reserved for bit-field extraction |
| `3D` | `BINS` | Reserved for bit-field insertion |
| `3E` | `LOGICX` | Reserved for future logic extensions |
| `3F` | `LOGICN` | Reserved for future logic extensions |

### `40`–`4F`: Shifts and Rotations

| Opcode | Mnemonic | Description |
|---|---|---|
| `40` | `SHL` | Shift `A_REG` left |
| `41` | `SHR` | Shift `A_REG` right logically |
| `42` | `SAR` | Shift `A_REG` right arithmetically |
| `43` | `ROL` | Rotate `A_REG` left |
| `44` | `ROR` | Rotate `A_REG` right |
| `45` | `RCL` | Rotate `A_REG` left through carry |
| `46` | `RCR` | Rotate `A_REG` right through carry |
| `47` | `SHLB` | Shift `B_REG` left |
| `48` | `SHRB` | Shift `B_REG` right logically |
| `49` | `ROLA` | Reserved for additional rotate behavior |
| `4A` | `ROTR` | Reserved for additional rotate behavior |
| `4B` | `SHLX` | Reserved for extended shifts |
| `4C` | `SHRX` | Reserved for extended shifts |
| `4D` | `SHFTX` | Reserved for future shift extensions |
| `4E` | `SHFTN` | Reserved for future shift extensions |
| `4F` | `SHIFTX` | Reserved for future shift extensions |

### `50`–`5F`: Status Flags

| Opcode | Mnemonic | Description |
|---|---|---|
| `50` | `CLF` | Clear all status flags |
| `51` | `STF` | Set status flags according to the parameter |
| `52` | `CLZ` | Clear the zero flag |
| `53` | `STZ` | Set the zero flag |
| `54` | `CLC` | Clear the carry flag |
| `55` | `STC` | Set the carry flag |
| `56` | `CLN` | Clear the negative flag |
| `57` | `STN` | Set the negative flag |
| `58` | `CLV` | Clear the overflow flag |
| `59` | `STV` | Set the overflow flag |
| `5A` | `GETF` | Copy status flags into a defined register format |
| `5B` | `PUTF` | Load status flags from a defined register format |
| `5C` | `FLAGT` | Toggle a selected flag |
| `5D` | `FLAGQ` | Query a selected flag |
| `5E` | `FLAGX` | Reserved for future flag operations |
| `5F` | `FLAGN` | Reserved for future flag operations |

### `60`–`6F`: Jumps and Control Flow

| Opcode | Mnemonic | Description |
|---|---|---|
| `60` | `JMP` | Jump to a program location |
| `61` | `JZ` | Jump if the zero flag is set |
| `62` | `JNZ` | Jump if the zero flag is clear |
| `63` | `JC` | Jump if the carry flag is set |
| `64` | `JNC` | Jump if the carry flag is clear |
| `65` | `JN` | Jump if the negative flag is set |
| `66` | `JNN` | Jump if the negative flag is clear |
| `67` | `JV` | Jump if the overflow flag is set |
| `68` | `JNV` | Jump if the overflow flag is clear |
| `69` | `JGT` | Reserved for signed greater-than branching |
| `6A` | `JLT` | Reserved for signed less-than branching |
| `6B` | `JGE` | Reserved for signed greater-or-equal branching |
| `6C` | `JLE` | Reserved for signed less-or-equal branching |
| `6D` | `CALL` | Reserved for subroutine calls |
| `6E` | `RET` | Reserved for subroutine returns |
| `6F` | `JMPX` | Reserved for extended jumps |

### `70`–`7F`: Execution Control

| Opcode | Mnemonic | Description |
|---|---|---|
| `70` | `HALT` | Stop CPU execution |
| `71` | `WAIT` | Wait for an event or defined number of cycles |
| `72` | `STEP` | Reserved for single-step debugging |
| `73` | `RESET` | Reset CPU state |
| `74` | `BREAK` | Trigger a debugging break |
| `75` | `NOPX` | Reserved no-operation extension |
| `76` | `SLEEP` | Pause execution until a defined event |
| `77` | `WAKE` | Resume from a supported sleep state |
| `78` | `TRAP` | Trigger a software-defined trap |
| `79` | `YIELD` | Yield execution to the virtual computer |
| `7A` | `EXECX` | Reserved for execution extensions |
| `7B` | `EXECA` | Reserved for execution extensions |
| `7C` | `EXECB` | Reserved for execution extensions |
| `7D` | `EXECC` | Reserved for execution extensions |
| `7E` | `EXECN` | Reserved for execution extensions |
| `7F` | `EXECF` | Reserved for execution extensions |

### `80`–`8F`: External Memory and Data Sources

These instructions are reserved for possible interaction with external memory or data sources. They do not imply internal CPU RAM.

| Opcode | Mnemonic | Description |
|---|---|---|
| `80` | `LDX` | Reserved for loading from an external source |
| `81` | `STX` | Reserved for storing to an external source |
| `82` | `LDBX` | Reserved for loading a byte externally |
| `83` | `STBX` | Reserved for storing a byte externally |
| `84` | `LDRX` | Reserved for loading a register externally |
| `85` | `STRX` | Reserved for storing a register externally |
| `86` | `LEAX` | Reserved for external address calculation |
| `87` | `ADDRX` | Reserved for external address operations |
| `88` | `SEGSET` | Reserved for selecting a data segment |
| `89` | `SEGGET` | Reserved for reading the selected segment |
| `8A` | `DATRD` | Reserved for external data reads |
| `8B` | `DATWR` | Reserved for external data writes |
| `8C` | `EXTIO` | Reserved for external I/O |
| `8D` | `EXTQ` | Reserved for external-source queries |
| `8E` | `EXTX` | Reserved for future external-source extensions |
| `8F` | `EXTN` | Reserved for future external-source extensions |

### `90`–`9F`: Display and Input/Output

| Opcode | Mnemonic | Description |
|---|---|---|
| `90` | `AOUT` | Output `A_REG` to the display |
| `91` | `HALT` | Stop CPU execution |
| `92` | `BOUT` | Output `B_REG` to the display |
| `93` | `COUT` | Output `C_REG` to the display |
| `94` | `DOUT` | Output `D_REG` to the display |
| `95` | `EOUT` | Output `E_REG` to the display |
| `96` | `FOUT` | Output `F_REG` to the display |
| `97` | `GOUT` | Output `G_REG` to the display |
| `98` | `HOUT` | Output `H_REG` to the display |
| `99` | `AIN` | Reserved for input into `A_REG` |
| `9A` | `BIN` | Reserved for input into `B_REG` |
| `9B` | `CIN` | Reserved for input into `C_REG` |
| `9C` | `DIN` | Reserved for input into `D_REG` |
| `9D` | `EIN` | Reserved for input into `E_REG` |
| `9E` | `FIN` | Reserved for input into `F_REG` |
| `9F` | `IOX` | Reserved for future I/O extensions |

### `A0`–`AF`: Constants and Data Conversion

| Opcode | Mnemonic | Description |
|---|---|---|
| `A0` | `SETC` | Reserved for loading a constant |
| `A1` | `GETC` | Reserved for retrieving a constant |
| `A2` | `HEX2D` | Reserved for hexadecimal-to-decimal conversion |
| `A3` | `DEC2H` | Reserved for decimal-to-hexadecimal conversion |
| `A4` | `ASCII` | Reserved for character conversion |
| `A5` | `CHR` | Reserved for output character conversion |
| `A6` | `STR` | Reserved for string-related operations |
| `A7` | `NUM` | Reserved for numeric conversion |
| `A8` | `BCD` | Reserved for binary-coded decimal conversion |
| `A9` | `UNBCD` | Reserved for BCD decoding |
| `AA` | `CONSTX` | Reserved for constant extensions |
| `AB` | `DATAX` | Reserved for data conversion extensions |
| `AC` | `CVT` | Reserved for conversion operations |
| `AD` | `CVTX` | Reserved for conversion extensions |
| `AE` | `CONSTN` | Reserved for future constants |
| `AF` | `DATAN` | Reserved for future data operations |

### `B0`–`BF`: Advanced Arithmetic

| Opcode | Mnemonic | Description |
|---|---|---|
| `B0` | `UMUL` | Reserved for unsigned multiplication |
| `B1` | `SMUL` | Reserved for signed multiplication |
| `B2` | `UDIV` | Reserved for unsigned division |
| `B3` | `SDIV` | Reserved for signed division |
| `B4` | `UCOMP` | Reserved for unsigned comparison |
| `B5` | `SCOMP` | Reserved for signed comparison |
| `B6` | `MIN` | Reserved for minimum selection |
| `B7` | `MAX` | Reserved for maximum selection |
| `B8` | `POW` | Reserved for exponentiation |
| `B9` | `SQRT` | Reserved for square root |
| `BA` | `GCD` | Reserved for greatest common divisor |
| `BB` | `LCM` | Reserved for least common multiple |
| `BC` | `ARX` | Reserved for arithmetic extensions |
| `BD` | `ARY` | Reserved for arithmetic extensions |
| `BE` | `ARZN` | Reserved for arithmetic extensions |
| `BF` | `ARFN` | Reserved for arithmetic extensions |

### `C0`–`CF`: Bit Fields and Validation

| Opcode | Mnemonic | Description |
|---|---|---|
| `C0` | `BITLEN` | Reserved for bit-length calculation |
| `C1` | `BITCNT` | Reserved for counting set bits |
| `C2` | `CLZCNT` | Reserved for counting leading zeros |
| `C3` | `CTZCNT` | Reserved for counting trailing zeros |
| `C4` | `PARITY` | Reserved for parity calculation |
| `C5` | `REV` | Reserved for bit reversal |
| `C6` | `BEXT` | Reserved for bit-field extraction |
| `C7` | `BDEP` | Reserved for bit-field deposit |
| `C8` | `RANGE` | Reserved for range checking |
| `C9` | `VALID` | Reserved for value validation |
| `CA` | `CHK` | Reserved for a generic check operation |
| `CB` | `TEST` | Reserved for bit testing |
| `CC` | `BITX` | Reserved for bit-operation extensions |
| `CD` | `BITY` | Reserved for bit-operation extensions |
| `CE` | `BITZ` | Reserved for bit-operation extensions |
| `CF` | `BITN` | Reserved for bit-operation extensions |

### `D0`–`DF`: Interrupts and Exceptions

| Opcode | Mnemonic | Description |
|---|---|---|
| `D0` | `INT` | Reserved for software interrupts |
| `D1` | `IRET` | Reserved for interrupt return |
| `D2` | `INTEN` | Reserved for enabling interrupts |
| `D3` | `INTDIS` | Reserved for disabling interrupts |
| `D4` | `EXC` | Reserved for exception handling |
| `D5` | `EXRET` | Reserved for exception return |
| `D6` | `TRAPX` | Reserved for trap extensions |
| `D7` | `FAULT` | Reserved for fault handling |
| `D8` | `ERRGET` | Reserved for retrieving error state |
| `D9` | `ERRCLR` | Reserved for clearing error state |
| `DA` | `INTQ` | Reserved for interrupt queries |
| `DB` | `INTX` | Reserved for interrupt extensions |
| `DC` | `EXCX` | Reserved for exception extensions |
| `DD` | `FAULTX` | Reserved for fault extensions |
| `DE` | `IRQX` | Reserved for IRQ extensions |
| `DF` | `IRQN` | Reserved for future interrupt operations |

### `E0`–`EF`: Debugging and System Information

| Opcode | Mnemonic | Description |
|---|---|---|
| `E0` | `DBG` | Reserved for debug output |
| `E1` | `DBGBRK` | Reserved for a debug breakpoint |
| `E2` | `REGDMP` | Reserved for register dumping |
| `E3` | `PCGET` | Reserved for reading the program counter |
| `E4` | `PCSET` | Reserved for setting the program counter |
| `E5` | `CPUINFO` | Reserved for CPU information |
| `E6` | `VER` | Reserved for architecture version information |
| `E7` | `TRACE` | Reserved for instruction tracing |
| `E8` | `ASSERT` | Reserved for assertion checks |
| `E9` | `LOG` | Reserved for logging |
| `EA` | `DBGX` | Reserved for debug extensions |
| `EB` | `SYSDAT` | Reserved for system data |
| `EC` | `SYSQ` | Reserved for system queries |
| `ED` | `SYSX` | Reserved for system extensions |
| `EE` | `DBGX2` | Reserved for debug extensions |
| `EF` | `DBGN` | Reserved for future debug operations |

### `F0`–`FF`: CPU Control and Extensions

| Opcode | Mnemonic | Description |
|---|---|---|
| `F0` | `CPUID` | Reserved for CPU identification |
| `F1` | `CPUVER` | Reserved for CPU version |
| `F2` | `CPUMODE` | Reserved for CPU mode selection |
| `F3` | `CPUSTAT` | Reserved for CPU status |
| `F4` | `CPURST` | Reserved for CPU reset control |
| `F5` | `CPUSHL` | Reserved for CPU shutdown |
| `F6` | `CPUSLP` | Reserved for CPU sleep |
| `F7` | `CPUWAK` | Reserved for CPU wake-up |
| `F8` | `EXT` | Reserved for instruction extensions |
| `F9` | `EXTA` | Reserved for extension group A |
| `FA` | `EXTB` | Reserved for extension group B |
| `FB` | `EXTC` | Reserved for extension group C |
| `FC` | `EXTD` | Reserved for extension group D |
| `FD` | `EXTX` | Reserved for extension group X |
| `FE` | `EXTN` | Reserved for future extensions |
| `FF` | `CUSTOM` | Reserved for custom instructions |

## 4. Example Program

The following program loads the ASCII code for `A` into `A_REG`, outputs it to the display, and halts.

```text
0100
0241
9000
9100
```

Combined machine code:

`0100024190009100`

Expected register value:

`A_REG = 0041`

Expected display output:

`A`

`0041` is hexadecimal for decimal `65`, the ASCII code for the character `A`.

## 5. Implementation Rules

1. Instructions are always 16 bits long.
2. The opcode is the first byte; the parameter is the second byte.
3. Registers retain their values between instructions.
4. Loading a full 16-bit value requires two byte-loading instructions.
5. `ADD` operates on `A_REG` and a defined operand.
6. `AOUT` outputs `A_REG` to the display.
7. `HALT` stops instruction execution.
8. Instructions marked reserved are not implemented until their behavior is defined.
9. The architecture does not require internal CPU RAM.
10. Scratch variables may simulate CPU state in the virtual implementation.

## 6. Development Status

ArchS_16 is an early-stage architecture. This table allocates all 256 opcode values, but it is not a claim that all instructions are implemented or fully specified.

Before expanding the CPU, define the operand encoding for arithmetic and register operations, program addressing, display behavior, status flags, and execution control.
