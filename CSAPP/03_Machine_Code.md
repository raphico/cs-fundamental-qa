1. Explain the primary, **systems engineering reason** why a compiler like GCC typically generates an intermediate **assembly code file** before passing it to the assembler, rather than generating the machine code (object file) directly from the C source. What crucial role does this intermediate step play in the overall **software development toolchain** and **debugging process**?

2. Define the **Instruction Set Architecture (ISA)**. Why is the ISA considered the **most crucial hardware/software contract** in a computer system, and what specific details (register files, instruction formats, addressing modes) does it standardize that allow a compiler to generate machine code that will reliably execute on any compliant processor?

3. The x86-64 architecture is characterized as **CISC** (Complex Instruction Set Computer) with **variable-length instructions**. Explain the direct relationship between the CISC philosophy (supporting many complex **addressing modes** and high-level operations) and the **necessity of variable instruction length**. Contrast this with the design simplicity and pipeline efficiency of **RISC** (Reduced Instruction Set Computer) architectures that use fixed-length instructions.

4. The x86-64 architecture explicitly **forbids copying values from one memory location to another in a single `mov` instruction**. Describe the **architectural cost** (in terms of processor pipeline complexity or required memory bus cycles) that necessitated this design restriction, and specify the **minimum sequence of instructions** required to achieve a memory-to-memory copy.

5. The x86-64 ISA omits a dedicated instruction to explicitly **zero-extend** a 4-byte source value to an 8-byte destination (i.e., `movz*lq`). Explain why this instruction is unnecessary, citing the specific **side effect** of the standard **`movl`** instruction when the destination is a 64-bit register.

6. The 16 general-purpose registers in x86-64 can operate on data of different sizes. Explain the **systemic implication** of the rule: "32-bit instructions (e.g., `movl`) set the upper 32 bits of the destination register to **zero**." Why is this convention crucial for maintaining **backward compatibility** with IA32 (32-bit) code and preventing bugs when mixing 32-bit and 64-bit operations?

7. The choice between compiling a program as 32-bit (IA32) or 64-bit (x86-64) on a 64-bit OS involves major system trade-offs.
    * **Memory Addressability:** Quantify the difference in the **maximum addressable heap/stack space** for a 32-bit versus a 64-bit program.
    * **Function Call Efficiency:** Explain how the x86-64 calling convention, which uses **eight new general-purpose registers** (r8-r15) for parameter passing, enhances the **efficiency of function calls** compared to the 32-bit convention, which relied heavily on the **stack**.

8. The `pushq` instruction performs two distinct operations: it **decrements the stack pointer (`%rsp`)** and then **stores the source value**. Explain why this single instruction is often encoded in a **more compact form** (e.g., 1 byte) than the equivalent two separate instructions (`subq 8, %rsp; movq %rbp, (%rsp)`). What efficiency does this compact encoding provide in program execution?

9. The most common x86-64 memory reference form is $\text{Imm}(r_b, r_i, s)$, which calculates the effective address as $\text{Imm} + R[r_b] + R[r_i] \cdot s$. Explain the **primary computational use case** (e.g., array traversal, struct access) for which this full-featured addressing mode was designed, and describe the role of each component: **Index Register ($r_i$)** and **Scale Factor ($s$)**.
