# MIPS

## Brief

- **Complier**: translates code into assembly language statement.
- **Assembler**: translates assembly language into machine language instructions.

- **Instruction Set Architecture** (ISA): An abstraction on interface between the hardware and the low-level software. (**Independent** from hardware)

Summary:

- Instruction and data are stored in memory **together**.

- Load-Store Model: Limit memory operation and rely on registers for storage during execution.(Only Load and Store Instruction access memory)

- Instruction Types: Memory, Calculation, Control Flow.

- General Purpose Register:

  - Faster processing

  - Limited in number: 16 / 32 (MIPS) 个，**complier** associate variables with registers

      | **Name**  | **Register  number** | **Usage**|
      | --------- | -------------------- | -------- |
      |\$zero     | 0                    | Constant  value 0 |
      |\$at       | 1                    | Reserved for the assembler |
      |\$v0-\$v1  | 2-3                  | Values  for results and expression evaluation |
      |\$a0-\$a3  | 4-7                  | Arguments|
      |\$t0-\$t7  | 8-15                 | Temporaries|
      |\$s0-\$s7  | 16-23                | Program  variables|
      |\$t8-\$t9  | 24-25                | More  temporaries |
      |\$k0-\$k1  | 26-27                | Reserved for OS |
      |\$gp       | 28                   | Global  pointer   |
      |\$sp       | 29                   | Stack  pointer    |
      |\$fp       | 30                   | Frame  pointer    |
      |\$ra       | 31                   | Return  address   |

  - **No Data Type**

## Instruction

MIPS is a Load-Store register architecture with **32** registers, each 32-bit long. Each **word** contain **32** bits. Memory addresses are **32-bit** long.

Why? Answer: They can fit into registers.

MIPS uses **byte address**, consecutive words differ by **4**

### Memory Instruction

**Only** ``Load`` and ``Store`` instructions can access data in memory.

```mipsasm
lw $t0, 40($3) # Load Word MEM[[$3] + 40] to $t0
sw $t0, 28($3) # Store Word [$t0] to MEM[[$3] + 28]
lb # Load byte
sb # Store byte
ulw # unaligned load word
lh # load half word
lwl, lwr # load word left/right
```

**Important**: For ``lw`` and ``sw``, sum of base address and offset need to be multiple of 4.

### Decision

Alter the control flow of program.
Change the next instruction to be executed.

- Conditional branch

```mipsasm
bne $t0, $t1, label # branch if not equal
beq $t0, $t1, label # branch if equal
```

- Unconditional jump

```mipsasm
j label # jump to
```

Label is an "anchor" in the assembly code in indicate point of interest, usually as **branch target**, _NOT_ instructions.

#### IF statement

```C
if (i == j){
  a = b + c;
}
else{
  a = b - c;
}
```

convert to mips assembly.

```mipsasm
  bne $s1, $s2, ELSE
  add $s0, $s3, $s4
ELSE:
  sub $s0, $s3, $s4
EXIT:
```

#### Loop Statement

```C
while(i != j){
    i = i + 1;
}
```

convert to mips assembly.

```mipsasm
Loop:
  beq $s1, $s2, Exit
  addi $s1, $s1, 1
  j Loop
Exit:
# Another form
```

#### Inequalities

branch if less than, use ``slt`` (set on less than) or ``slti``.

```mipsasm
slt $t0, $s1, $s2 # $t0 set to 1 if [$s1] < [$s2]
```

### R-Format Instruction

Example: ``add``, ``sub``, ``and``, ``or``, ``nor``, ``slt``...
Special cases: ``srl``, ``sll``

$$
\begin{array}{|c|c|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{rd} & \text{shamt} & \text{func} \\
\hline
6 & 5 & 5 & 5 & 5 & 6 \\
\hline
\end{array}
$$

- opcode: For R-format instructions, **opcode = 0**.
- rs: Source Register, First Operand.
- rt: Target Register, Second Operand.
- rd: Destination Register, which receives result of computation.
- shamt: Amount a **shift** instructions. 5-bit(0 - 31). For non-shift instructions, shamt = 0.
- funct: specifies R-Format instructions.

Example:

```asm
add $8, $9, $10
```

$$
\begin{array}{|c|c|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{rd} & \text{shamt} & \text{func} \\
\hline
0 & 9 & 10 & 8 & 0 & 32 \\
\hline
000000 & 01001 & 01010 & 01000 & 00000 & 100000 \\
\hline
\end{array}
$$

```asm
sll $8, $9, 4 # swift left logic
```

$$
\begin{array}{|c|c|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{rd} & \text{shamt} & \text{func} \\
\hline
0 & 0 & 9 & 8 & 4 & 0 \\
\hline
000000 & 00000 & 01001 & 01000 & 00100 & 000000 \\
\hline
\end{array}
$$

### I-Format Instruction

Example: ``addi``, ``andi``, ``ori``, ``slti``, ``lw``, ``sw``, ``beq``, ``bne``...

$$
\begin{array}{|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{immediate} \\
\hline
6 & 5 & 5 & 16\\
\hline
\end{array}
$$

- opcode : Specify an instruction
- rs: Source operand (if any)
- rt: Note the difference from R-Format instructions.
- immediate: **Signed integer**, 16-bit($-2^{15}\sim 2^{15}-1$)

For example:

```asm
addi $21, $22, -50
```

$$
\begin{array}{|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{immediate} \\
\hline
8 & 22 & 21 & -50\\
\hline
001000 & 10110 & 10101 & 1111\ 1111\ 1100\ 1110\\
\hline
\end{array}
$$

```asm
lw $9, 12($8)
```

$$
\begin{array}{|c|c|c|c|}
\hline
\text{opcode} & \text{rs} & \text{rt} & \text{immediate} \\
\hline
35 & 8 & 9 & 12\\
\hline
100011 & 01000 & 01001 & 0000\ 0000\ 0000\ 1100\\
\hline
\end{array}
$$

**Important**

``beq`` and ``bne``: Different from other I-Format instuctions.(In datapath)
They are relative jump, the address needs to calculate with PC.

- NOT taken: $ \text{PC} = \text{PC} + 4 $
- Taken: $ \text{PC} = (\text{PC} + 4) + (\text{immediate} \times 4 ) $

Mind times 4 !!!
So branch range is $ \pm 2^{17} $ bytes or $\pm 2^{15}$ words

### J-Format

Unconditional Jump: It is a **absolute** jump, non-relative with PC.

$$
\begin{array}{|c|c|}
\hline
\text{opcode} & \text{target address}\\
\hline
6 & 26 \\
\hline
\end{array}
$$

- Unsigned number.
- Last 2 bits: Like branches, only jump to **word-aligned** addresses, so last two bits are 00 implicitly.
- First 4 bits: From $\text{PC+4}$.

So jump range is 256 MB. $2^{28}$ bytes.

### Address Mode

- Register addressing: (``add``) oprand is a register
- Immediate addressing: (``addi``) oprand is a constant.
- Base addressing: (``lw``) oprand is at the MEM whose address is sum of a register and a constant.
- PC-relative addressing: (``beq``) address is sum of PC and a constant.
- Pseduo-direct addressing: (``j``) upper 4-bit of PC + 26-bit constant + 00.

