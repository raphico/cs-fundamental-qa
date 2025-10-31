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

10. The `leaq` instruction has only one variant (`leaq`), operating exclusively on **quad words (64 bits)**. Explain the fundamental reason for this design constraint by relating the instruction's primary function—computing an **effective address**—to the architecture's **fixed address size**. Why would a `leal` (32-bit) or `leaw` (16-bit) variant be redundant or nonsensical in a 64-bit address space?

11. The `leaq` instruction's true power lies in its use as an **arithmetic optimizer**. Given the expression $t = 7x + 15$ where $x$ is in register `%rdi`, write the single `leaq` instruction that computes $t$ and stores the result in `%rax`. Justify why a compiler would prefer this single instruction over a combination of `addq` and `imulq` instructions for simple, fixed-multiplier arithmetic.

12. The actual shift amount used by a shift instruction (`sal`, `sar`, `shr`) is controlled by the immediate value $k$ or the 8-bit register `%cl`. Explain how the **width of the operand ($w$ bits)** dictates the **effective shift amount** (the modulo operation applied to the count). Specifically, state the modulo operation for a 64-bit operand ($w=64$) and describe the practical consequence of this modulo arithmetic for large shift counts (e.g., shifting by 64).

13. The x86-64 architecture provides two distinct instructions for **full 128-bit multiplication** of two 64-bit values: `mulq` (unsigned) and `imulq` (signed, one-operand form). At the **bit-level**, the process of generating the 128-bit product is identical for both. Explain *how* the assembler/processor uses the instruction name (`mulq` vs. `imulq`) to correctly distinguish between a signed and unsigned multiplication context, and specify the **fixed register pair** used to hold the 128-bit result.

14. When implementing 64-bit **signed division** using the `idivq` instruction, the dividend must be prepared as a 128-bit quantity in the register pair `%rdx:%rax`. If the C source is dividing a standard `long x`, explain the **mechanism and purpose** of the **`cqto` instruction** in preparing the high-order 64 bits (`%rdx`). Why is this sign-extension critical for preserving the correct value and sign of the dividend before division?

15. In contrast to signed division, when implementing 64-bit **unsigned division** using `divq`, the high-order register (`%rdx`) must be manually set to zero. Provide the **most efficient x86-64 instruction** to achieve this zeroing of `%rdx` and explain *why* this manual zero-clearing step is necessary, given that there is no specialized instruction like `cqto` for zero extension in this context.

16. In compiled assembly code, one frequently encounters the instruction **`xorq %rcx, %rcx`** even when no logical exclusive-OR operation was present in the C source.
    * What is the **precise effect** of this instruction on the register's value?
    * **Why** do compilers favor this instruction over the seemingly more straightforward `movq $0, %rcx` or `subq %rcx, %rcx`? (Hint: Consider the instruction's size and dependencies.)
