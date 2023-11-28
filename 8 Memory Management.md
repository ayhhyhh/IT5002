
Physical Memory 
- Physical Memory is the actual matrix of capacitors(DRAM) or flip-flops(SRAM) that stores data and instructions. Memory addresses serve as **byte indices**

Word
- Physical memory is organized in bytes, but CPUs often transfer data in units of (>1) bytes. The unit is known as "word".

Alignment Issue
- Data may or may not be fetched across word boundaries, depending on the design of circuitry.
- Mis-aligned fetch is inefficient.

Endianness
- Big Endian: higher order bytes at lower addresses
- Little Endian: lower order bytes at lower addresses
- x86: little endian

Problems in multiple program system:
- Conflicting addresses
- Access violations
Solution: Logical Addressing.
- Base Register: Contains the starting address for the program.
	- Physical Add = Base Register + Logical Add
- Limit Register: Contains the length of the memory segment.
	- If a program tries to access memory outside the range \[base, base + limit), raises a "segmentation fault"
- Base and limit registers allow us to **partition** memory for each running process.

Fragmentation:
- Internal Fragmentation: 
	- Partition is larger than is needed.
	- Cannot be used by other processes.(Because it is inside a process.)
- External Fragmentation:
	- Free memory is broken into small chunks.
	- In TOTAL, free memory is sufficient, but individual chunks is insufficient to fulfill requests.

For individual programs, memory structure:
- Instructions.
- Global variables.
- Stack: local variables and local addresses
- Heap: Dynamic variables.
For linux program memory structure:
![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311281059620.png)

## Manage Free Memory

Normally, there are two approaches managing free memory:
- Bit maps
- Free/Allocated List
Memory is divided up into fixed sized chunks called **allocation unit**s.

![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311281101864.png)

For Free/Allocated List, after releasing memory chunk, remember to coalesce freed blocks together.

## Allocation Policy

- First Fit
	- Scan through the list/bit map and find the **first** block of free units that can fit the requested size.
	- Fast. Easy to implement.
- Best Fit
	- Scan through the list/bit map and find the **smallest** block of free units that can fit the requested size.
	- Minimize waste.
	- Result in many tiny useless holes.(External Fragmentation)
- Worst Fit
	- Scan through the list/bit map and find the **Largest** block of free units that can fit the requested size.
	- Reduce the number of tiny useless holes.

Tips: Can use sort to make it easy to find smallest or largest block, but would be harder to merge when free.

### Buddy Allocation

Aka, Quick Fit.

![image.png](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/202311281111405.png)

Allocation Process: Allocate 128bytes for example.
- Split 512 byte block into 2 blocks of 256 bytes.
- Split one 256 byte block into 2 128-byte block.
- Allocate one 128-byte block
Free Process:
- Free(0), coalesces with its buddy block at 128@128, form a 256@0
- Coalesces with 256@256, form a 512@0