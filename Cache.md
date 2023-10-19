# Cache

## Introduce

Main idea of cache is to keep the **frequently and recently** used data in **smaller but faster** memory, only when cannot find data/instruction in cache, refer to main memory. Cache mainly uses **SRAM**, main mem uses **DRAM**.

Principle of Locality: Program accesses only a small portion of the memory address space within a small time interval.

- Temporal Locality: If an item is referenced, it will be referenced **again** soon.
- Spatial Locality: If an item is referenced, **nearby** item will tend to be referenced soon.

Terminology:

- Hit: Data is in cache
  - Hit rate: Fraction of mem accesses that hit.
  - Hit time: Time to access cache.
- Miss: Data not in cache.
  - Miss rate: 1 - Hitrate
  - Miss penalty: Time to replace cache block + hit time.
- Average Access Time:

$$
\begin{aligned}
    \text{Average Access Time} = \text{Hit rate} \times \text{Hit time} + (1-\text{Hit rate})\times \text{Miss penalty}
\end{aligned}
$$

Cache Block/Line: Unit of transfer between memory and cache.
Block size is typically one or more **words**.

## Direct Mapped Cache

Cache Block Size = $2^N$ bytes
Number of cache blocks = $2^M$

Offset = N bits
Index = M bits
Tag = 32 - (N + M) bits

$$
\begin{array}{|c|c|c|}
    \hline
    32 - (N + M) & M & N \\
    \hline
    \text{Tag} & \text{Index} & \text{Offset} \\
    \hline
\end{array}
$$

Cache Structure:

![20231019232356](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/20231019232356.png)

- Tag: Tag of memory block
- Valid bit: indicates whether the data in cache line is valid(when not valid means content may be random bits)

![20231019232612](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/20231019232612.png)

Type of cache miss:

1. Cold/Compulsory miss: On first access to a block, valid bit is zero.
2. Conflict miss: Several block are mapped to the same block. In direct mapped cache or set associative cache.
3. Capacity miss: When block are discarded from cache and cache is **full**.

## Write Policy

1. Write-Through: write data both to cache and main memory. Generally using a **write buffer**.
2. Write-back: only write to cache, write to memory when cache block is replaced. Need to add a **dirty bit**.

## Write Miss Policy

1. Write-Allocate: Load block to cache.
2. Write-Around: Don't load, write directly to main memory only.

## N-Way Set Associative Cache

A block can be placed in N locations in cache.(N > 1, and always be a power of 2)

1. Cache Block Size: $2^N$ bytes.
2. Number of **Cache Set**: $\text{cache capacity} / (\text{n-way} \times \text{block size}) = 2^M$
3. Tag: $32 - (N + M)$ bits

![20231019235718](https://raw.githubusercontent.com/ayhhyhh/IMGbed/master/imgs/20231019235718.png)

## Fully Associative Cache

n-way set associative cache, when n equals to the number of blocks, meaning that a block can be stored in **anywhere** of cache.

**Set Index** is no longer needed. Only Tag and offest need to compute.

## Block Replacement Policy

1. Least Recently Used(LRU): record when cache is accessed, replace least recently used.
2. First In First Out
3. Random Replacement
4. Least Frequently Used
