# MIPS

## Brief

- **Complier**: translates code into assembly language statement.
- **Assembler**: translates assembly language into machine language instructions.

- **Instruction Set Architecture** (ISA): An abstraction on interface between the hardware and the low-level software. (**Independent **from hardware)

Summary:

- Instruction and data are stored in memory **together**.

- Load-Store Model: Limit memory operation and rely on registers for storage during execution.(Only Load and Store Instruction access memory)

- Instruction Types: Memory, Calculation, Control Flow.

- General Purpose Register:

  - Faster processing

  - Limited in number: 16 / 32 (MIPS) 个，**complier **associate variables with registers

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