# Notes for IT5002

## Number System

### Conversion

#### Base-R to Decimal Conversion

$$
1101.101_2=1\times2^3+1\times2^2+1\times2^0+1\times2^{-1}+1\times2^{-3}
$$

$$
2A.8_{16} = 2\times16^1+10\times16^0+8\times16^{-1}
$$

#### Decimal to Base-R Conversion

Repeated Division-by-R Method(For whole numbers)

$$
43_{10}=101011_{2}
$$

$$
\begin{array}{|c|c|c|} 
\hline 
\text{number} & & \text{reminder} \newline
\hline 
43 &  & {} \\
\hline
21 & \mod 2 & 1(LSB) \\
\hline
10 & \mod 2 & 1\\
\hline
5 & \mod 2 & 0\\
\hline
2 & \mod 2 & 1\\
\hline
1 & \mod 2 & 0\\
\hline
0 & \mod 2 & 1(MSB)\\
\hline
\end{array}
$$

Repeated Multiplication-by-R Method(For fractions)

$$
0.3125_{10}=.0101_{2}
$$

$$
\begin{array}{|c|c|c|}
\hline
\text{number} & & \text{reminder}\\
\hline
 0.3125 & \times2 & {} \\
\hline
0.625 & \times2 & 0(MSB)\\
\hline
1.25 & & 1\\
\hline
0.25 & \times 2 & \\
\hline
0.5 & \times2 & 0\\
\hline
1 &  & 1(LSB)\\
\hline
0 &  & \\
\hline
\end{array}
$$

#### Hex Oct Bin

Bin to Hex: **grouping** of binary digits into sets of 4, **Conversion** each group of 4 bits to corresponding hexadecimals for example 1010 to A, **Concatenation** every hex digits.

### ASCII Code

American Standard Code for Information Interchange(ASCII), 7 bits, 1 parity bit(for odd or even parity)

mind that digits( 0 - 9 ) < capital letter ( A - Z ) < lowercase letter ( a - z )

### Negative Number

- Unsigned Numbers: Only non-negative values
- Signed Numbers: Include all values

For **Signed Binary Number**, there are 3 common representations:

- Sign-and-Magnitude
- 1’s Complement
- 2’s Complement

#### Sign-and-Magnitude

the sign is represented by **sign bit**: 0 for +, 1 for -

Example: 1-bit sign and 7-bit magnitude

$$
+52_{10} = + 110100_2=00110100\\
-19_{10} = - 10011_2=10010011
$$

For n-bit Sign-and-Magnitude number:

- Largest Value: $2^{(n-1)}-1$ (n-1 ones)
- Smallest Value: $-2^{n-1}+1$
- Zeros: +0 and -0

#### Complement

**REMIND**: **Positive** number’s complement is itself.

A **Negative** number’s complement is calculated as:

##### 1’s complement

$$
-x=2^n-1-x
$$

For example: -12 1’s complement (8 bit)

$$
\begin{aligned}
-12=&2^8-1-12\\
=&243\\
=&1111\ 0011_{1's}
\end{aligned}
$$

**TECHNIQUE**: invert all bits.

Largest Value: $2^{n-1}-1=0111\ 1111=127$

Smallest Value: $-2^{n-1}-1=1000\ 0000=-127$

Zeros: $+0=0000\ 0000; \ \ -0=1111\ 1111$

MSB can still represent sign.

##### 2’s complement

$$
-x=2^n-x
$$

For example: -12 2’s complement (8 bit)

$$
\begin{aligned}
-12_{10}=& 2^8-12 \\
=&244\\
=&11110100_{2's}
\end{aligned}
$$

**TECHNIQUE**: Convert to binary(0000 1100), **invert** all bits(1111 0011), **add 1**(1111 0100)

Largest Value: $2^{n-1}-1|_{n=8}=0111\ 1111=127$

Smallest Value: $-2^{n-1}|_{n=8} = 1000\ 0000 = 128$

Zero: $0=0000\ 0000$

MSB can still represent sign

##### Fraction

The main idea of complement is the same. For negative number, 1’s complement to invert all bits, 2’s complement to invert and add 1. For positive number, complements are itself.

#### Base-R complement

For a number in Base-R, there are two kinds of complement: **r’s complement** and **r-1’s complement**. Positive numbers are themselves. **Negative** number with **n-bit integer** part and **m-bit fraction** part, the complement is computed as:

- r’s complement

$$
\begin{align}
-x=r^{n}-x
\end{align}
$$

To obtain the essence, the r’s complement of a negative n-bit number is the number that, when added to its corresponding positive counterpart, yields $r^n$(0000 0000).(MSB 1 is overflowed)

- r-1’s complement

$$
-x=r^{n}-r^{m}-x
$$

Similarly, r-1’s complement of a negative n-bit integer and m-bit fraction add its positive counterpart yield $r^n-r^m$(which is all ones, 1111.1111).

### Addition and Subtraction

Signed numbers are of a **fixed** range, so if go beyond this range, **overflow** occurs.

Check Overflow:

- positive add positive -> negative
- negative add negative -> positive

For 2’s complement, if there is a carry out of MSB, ignore.

For 1’s complement, if there is a carry out of MSB, **add 1** to the result

$$
\begin{aligned}
-2+-5 = &1101+1010\\
= & 1\ 0111(\text{carry out}) + 1\\
=& 1000=-7
\end{aligned}
$$

Subtraction is add the second operand’s complement.

### Excess Notation

Excess Notation swifts the binary number.

For example, 000 in BIN is corresponding to 0 in DEC. In Excess-4 representation, 000 in BIN is corresponding to -4 in DEC, other number are swift like this.

$$
\begin{array}{|c|c|}
\hline
\text{Excess-4}&\text{DEC value} \\
\hline
000&-4\\
\hline
001&-3\\
\hline
010&-2\\
\hline
011&-1\\
\hline
100&0\\
\hline
101&1\\
\hline
110&2\\
\hline
111&3\\
\hline
\end{array}
$$

### Real Number

#### Fixed-Point Representation

Range is limited.

#### Floating Point Representation

3 component: sign, exponent, mantissa(fraction)

$$
\begin{array}{|c|c|c|}
\hline
\text{sign} & \text{exponent} & \text{mantissa}\\
\hline
\end{array}
$$

The base (radix) is assumed to be 2.

Two formats (IEEE 754):

- [**Single-precision**]([Single-precision floating-point format - Wikipedia](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)) (32 bits): 1-bit sign, 8-bit, exponent with bias 127 (excess-127), 23-bit mantissa
- Double-precision (64 bits): 1-bit sign, 11-bit, exponent with bias 1023 (excess-1023), 52-bit mantissa

**IMPORTANT**

$$
\begin{array}{|c|c|c|c|}
\hline
\text{exponent}&\text{fraction} = 0 & \text{fraction}\neq0 & \text{Equation}\\
\hline
00_\text{H} = 0000\ 0000_2 & \pm 0 & \text{denomalize number} & (-1)^{\text{sign}}\times 2^{-126}\times 0.\text{fraction}\\
\hline 
01_\text{H},..., \text{FE}_ \text{H} & \text{normal} & \text{normal} & (-1)^{\text{sign}}\times 2^{\text{exponent}-127}\times 1.\text{fraction} \\
\hline
\text{FF}_\text{H} & \pm \infty & \text{NaN} & \\
\hline
\end{array}
$$


## MIPS

### Brief

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

### Instruction

MIPS is a Load-Store register architecture with **32** registers, each 32-bit long. Each **word** contain **32** bits. Memory addresses are **32-bit** long.

Why? Answer: They can fit into registers.

MIPS uses **byte address**, consecutive words differ by **4**

#### Memory Instruction

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

#### Decision

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

##### IF statement

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

##### Loop Statement

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

##### Inequalities

branch if less than, use ``slt`` (set on less than) or ``slti``.
```mipsasm
slt $t0, $s1, $s2 # $t0 set to 1 if [$s1] < [$s2]
```
