1. Explain the primary, **systems engineering reason** why a compiler like GCC typically generates an intermediate **assembly code file** before passing it to the assembler, rather than generating the machine code (object file) directly from the C source. What crucial role does this intermediate step play in the overall **software development toolchain** and **debugging process**?

2. Define the **Instruction Set Architecture (ISA)**. Why is the ISA considered the **most crucial hardware/software contract** in a computer system, and what specific details (register files, instruction formats, addressing modes) does it standardize that allow a compiler to generate machine code that will reliably execute on any compliant processor?

3. The x86-64 architecture explicitly **forbids copying values from one memory location to another in a single `mov` instruction**. Describe the **architectural cost** (in terms of processor pipeline complexity or required memory bus cycles) that necessitated this design restriction, and specify the **minimum sequence of instructions** required to achieve a memory-to-memory copy.

4. The x86-64 ISA omits a dedicated instruction to explicitly **zero-extend** a 4-byte source value to an 8-byte destination (i.e., `movz*lq`). Explain why this instruction is unnecessary, citing the specific **side effect** of the standard **`movl`** instruction when the destination is a 64-bit register.

5. The 16 general-purpose registers in x86-64 can operate on data of different sizes. Explain the **systemic implication** of the rule: "32-bit instructions (e.g., `movl`) set the upper 32 bits of the destination register to **zero**." Why is this convention crucial for maintaining **backward compatibility** with IA32 (32-bit) code and preventing bugs when mixing 32-bit and 64-bit operations?

6. The choice between compiling a program as 32-bit (IA32) or 64-bit (x86-64) on a 64-bit OS involves major system trade-offs.

   - **Memory Addressability:** Quantify the difference in the **maximum addressable heap/stack space** for a 32-bit versus a 64-bit program.
   - **Function Call Efficiency:** Explain how the x86-64 calling convention, which uses **eight new general-purpose registers** (r8-r15) for parameter passing, enhances the **efficiency of function calls** compared to the 32-bit convention, which relied heavily on the **stack**.

7. The `pushq` instruction performs two distinct operations: it **decrements the stack pointer (`%rsp`)** and then **stores the source value**. Explain why this single instruction is often encoded in a **more compact form** (e.g., 1 byte) than the equivalent two separate instructions (`subq 8, %rsp; movq %rbp, (%rsp)`). What efficiency does this compact encoding provide in program execution?

8. The most common x86-64 memory reference form is $\text{Imm}(r_b, r_i, s)$, which calculates the effective address as $\text{Imm} + R[r_b] + R[r_i] \cdot s$. Explain the **primary computational use case** (e.g., array traversal, struct access) for which this full-featured addressing mode was designed, and describe the role of each component: **Index Register ($r_i$)** and **Scale Factor ($s$)**.

9. The `leaq` instruction has only one variant (`leaq`), operating exclusively on **quad words (64 bits)**. Explain the fundamental reason for this design constraint by relating the instruction's primary function—computing an **effective address**—to the architecture's **fixed address size**. Why would a `leal` (32-bit) or `leaw` (16-bit) variant be redundant or nonsensical in a 64-bit address space?

10. The `leaq` instruction's true power lies in its use as an **arithmetic optimizer**. Given the expression $t = 7x + 15$ where $x$ is in register `%rdi`, write the single `leaq` instruction that computes $t$ and stores the result in `%rax`. Justify why a compiler would prefer this single instruction over a combination of `addq` and `imulq` instructions for simple, fixed-multiplier arithmetic.

11. The actual shift amount used by a shift instruction (`sal`, `sar`, `shr`) is controlled by the immediate value $k$ or the 8-bit register `%cl`. Explain how the **width of the operand ($w$ bits)** dictates the **effective shift amount** (the modulo operation applied to the count). Specifically, state the modulo operation for a 64-bit operand ($w=64$) and describe the practical consequence of this modulo arithmetic for large shift counts (e.g., shifting by 64).

12. The x86-64 architecture provides two distinct instructions for **full 128-bit multiplication** of two 64-bit values: `mulq` (unsigned) and `imulq` (signed, one-operand form). At the **bit-level**, the process of generating the 128-bit product is identical for both. Explain _how_ the assembler/processor uses the instruction name (`mulq` vs. `imulq`) to correctly distinguish between a signed and unsigned multiplication context, and specify the **fixed register pair** used to hold the 128-bit result.

13. When implementing 64-bit **signed division** using the `idivq` instruction, the dividend must be prepared as a 128-bit quantity in the register pair `%rdx:%rax`. If the C source is dividing a standard `long x`, explain the **mechanism and purpose** of the **`cqto` instruction** in preparing the high-order 64 bits (`%rdx`). Why is this sign-extension critical for preserving the correct value and sign of the dividend before division?

14. In contrast to signed division, when implementing 64-bit **unsigned division** using `divq`, the high-order register (`%rdx`) must be manually set to zero. Provide the **most efficient x86-64 instruction** to achieve this zeroing of `%rdx` and explain _why_ this manual zero-clearing step is necessary, given that there is no specialized instruction like `cqto` for zero extension in this context.

15. In compiled assembly code, one frequently encounters the instruction **`xorq %rcx, %rcx`** even when no logical exclusive-OR operation was present in the C source.

    - What is the **precise effect** of this instruction on the register's value?
    - **Why** do compilers favor this instruction over the seemingly more straightforward `movq $0, %rcx` or `subq %rcx, %rcx`? (Hint: Consider the instruction's size and dependencies.)

16. Identify the four primary **single-bit condition code registers** in the x86-64 CPU and state the precise event (e.g., overflow, sign change, zero result) that each one tracks. Then, explain the **difference in overflow detection**—how is the **Carry Flag (CF)** used to detect overflow for **unsigned** operations, while the **Overflow Flag (OF)** is used for **two's complement (signed)** operations?

17. The `leaq` instruction is unique among arithmetic/logical operations because it **does not set any condition codes**. Justify this design choice by explaining the instruction's primary intended use in **address computation**. Why is setting flags unnecessary or even undesirable for its core purpose?

18. Compare and contrast the purpose and mechanism of the **`cmp`** instruction class versus the **`test`** instruction class. Explain why these instructions are essential for control flow, specifically describing their shared characteristic of **setting condition codes without altering any general-purpose registers**.

19. The **`cmp`** (compare) and **`test`** instructions set all four primary condition codes ($\text{CF}, \text{ZF}, \text{SF}, \text{OF}$) based on the result of an arithmetic or logical operation. Describe the precise sequence of events when executing **`cmpq %rsi, %rdi`** where the contents of `%rdi` and `%rsi` are identical. Why is it a critical error to state that flags other than $\text{ZF}$ are "cleared" in this case?

20. The C standard's definition of **signed less than** ($a < b$) is implemented in assembly using the **$\text{V}$-flag**, formally defined as $\text{V} = \mathbf{SF} \oplus \mathbf{OF}$ (Sign Flag XOR Overflow Flag).

    - Explain the intuitive meaning of the $\text{V}$-flag. Why does this specific logical combination correctly determine the signed ordering, even when a **two's complement overflow** occurs during the subtraction $b - a$?
    - Which assembly instruction relies on the $\text{V}$-flag alone to set a destination byte to 0 or 1?

21. Using only the $\text{CF}$ (Carry Flag) and $\text{ZF}$ (Zero Flag), provide the precise logical condition (e.g., $\mathbf{CF} \land \mathbf{ZF}$, $\mathbf{CF} \lor \mathbf{ZF}$, $\sim\mathbf{CF}$) that must be true for the following two unsigned comparisons to hold after a **`cmp`** instruction:

    - $a < b$ (Unsigned, _set below_)
    - $a \le b$ (Unsigned, _set below or equal_)

22. The `inc` and `dec` instructions set the **Overflow (OF)** and **Zero (ZF)** flags but **leave the Carry Flag (CF) unchanged**. Explain the systems engineering implication of this specific design choice. (Hint: Consider the common use of the CF in multi-precision arithmetic.)

23. Differentiate between **conditional control transfer (jumps)** and **conditional data transfer (conditional moves, `cmov`)** as mechanisms for implementing C's conditional logic. Why is the conditional move strategy often **more efficient on modern pipelined processors**, and what crucial processor feature (related to branches) does it effectively bypass to achieve this performance gain?

24. Define **branch misprediction** in the context of a pipelined processor. Explain the severe performance penalty (e.g., 15-30 clock cycles) incurred by a mispredicted jump. How does this penalty influence a compiler's decision to use a slower-looking **conditional move**, even if it requires computing both the `then` and `else` expressions?

25. Not all conditional C expressions can be safely compiled using a conditional move. Give a specific example (like the `cread` function dereferencing a pointer) where using a conditional move would result in **invalid program behavior** (e.g., a fatal error) due to **side effects**. Explain why the traditional **conditional jump** mechanism is mandatory in such cases.

26. Jump targets in x86-64 are typically encoded using a **PC-relative address**. Explain what PC-relative addressing means, and justify _why_ this encoding method is superior to absolute addressing for generating **compact and relocatable object code**. Illustrate how the program counter (PC) is used in the calculation of the target address during execution.

27. Consider a jump instruction encoded using a 4-byte PC-relative offset. If the jump instruction starts at address $A$, and the target offset is $O$ (in two's complement), write the formula for the final jump target address $T$. Explain why $T$ is calculated relative to the address _following_ the jump instruction, $A + \text{InstructionLength}$.

28. The conditional move (`cmov`) instruction class is subject to a strict architectural restriction: **The destination operand cannot be a memory location.**

    - Explain the likely **architectural reason** for this constraint, relating it to the desire to keep the instruction simple, fast, and free of the memory complexity associated with a full Read-Modify-Write cycle.
    - What practical limitation does this impose on a compiler when translating conditional assignments in C? (e.g., When is a CMOV not an option for a conditional assignment?)

29. The `do-while` loop is the most straightforward C loop to translate into machine code.

    - Explain the **architectural reason** why a `do-while` loop requires only **one conditional jump instruction** to implement its control flow.
    - Describe the assembly structure, specifying where the conditional test and the jump back to the loop body are placed relative to the main body code.

30. When reversing the assembly code for a loop, a register often holds a program variable (e.g., `%rax` holds `result` in factorial).

    - If a register is used to hold a loop counter (`n`), what two specific instructions (one for updating, one for testing) would you expect to see inside the loop body to implement the C expression `n = n - 1` followed by the loop check?

31. One common translation for a `while` loop is the **Jump-to-Middle** strategy.

    - Explain the **necessity** of the **unconditional jump** (`jmp`) instruction placed immediately before the loop body. Why can't the compiler simply place the loop test at the beginning of the code block?
    - How does this strategy structurally resemble the assembly code for a `do-while` loop, despite implementing the initial test required by the `while` structure?

32. **Guarded-Do Strategy and Optimization Trade-offs:** The compiler often uses the **Guarded-Do** strategy (a preliminary conditional jump followed by a `do-while` structure) when higher optimization levels are enabled.

    - Explain the primary goal of the preliminary conditional jump (`jle .L_done`).
    - In what specific scenario, concerning the initial value of the test expression, does the **Guarded-Do** strategy execute _fewer_ instructions compared to the **Jump-to-Middle** strategy?

33. The C standard defines the behavior of a `for` loop by equating it to a structure using a `while` loop.

    - Provide the C structure (using a `while` loop, initialization, and update expressions) that precisely describes the behavior of the general `for (init-expr; test-expr; update-expr) body-statement;`.

34. The presence of a `continue` statement inside a `for` loop breaks the simple equivalence rule from the previous question.

    - Explain what execution step (initialization, body, test, or update) is **incorrectly bypassed** if one naively translates a `for` loop containing a `continue` statement into the equivalent `while` loop template.
    - To correctly implement a `continue` in assembly or goto code, where must the program jump (which part of the loop structure) to ensure the loop continues its intended sequence?

35. In a C `switch` statement where the case values range from a minimum of $L$ to a maximum of $H$ (e.g., $100$ to $106$), the compiler must first compute a normalized index, $i$.

    - Formulate the mathematical expression used to compute the normalized index $i$ from the switch variable $x$.
    - Why is this normalization step absolutely mandatory before accessing the jump table?

36. The assembly code must perform a **bounds check** on the normalized index $i$ before accessing the jump table.

    - What is the maximum valid value for the index $i$? (Express this in terms of $L$ and $H$).
    - Describe the **precise assembly sequence** (using comparison and jump instructions) that ensures execution correctly branches to the **default case** if the index is outside the valid, normalized range $[0, H-L]$.

37. Once the normalized index $i$ is validated, the execution transfers control using a two-step process: table lookup and indirect jump.

    - Describe the **memory reference calculation** used to retrieve the target address from the jump table. Why must a scale factor of **8** be used in this calculation?
    - Write the **general assembly format** for the final indirect jump instruction that uses the retrieved address to transfer control.

38. Contrast the worst-case time complexity of using a **jump table** to execute a multiway branch with the time complexity of implementing the same logic using a long chain of **`if-else if` statements**. Explain _why_ the jump table achieves its constant-time (O(1)) performance regardless of the number of cases.

39. The compiler uses a **heuristic** (based on density and number of cases) to decide whether to implement a `switch` statement using a jump table or a sequence of `cmp`/`jmp` instructions.

    - Identify the two primary scenarios (based on the number and range of cases) that would cause a compiler to **reject** the jump table approach and instead generate an equivalent sequence of sequential conditional tests.
    - Justify the compiler's choice in the scenario where the case values are **very sparse** across a large range (e.g., cases 0, 100, and 1000).

40. The C language allows **case fall-through** (omitting a `break` statement). Describe how the assembly code handles a fall-through scenario for a block of code (e.g., Case 102 falling into Case 103). What specific instruction is **omitted** at the end of the first case's code block to implement the fall-through?

41. Explain how a jump table is structured to efficiently handle **missing case values** (e.g., case 101) and **multiple labels** corresponding to the same block of code (e.g., cases 104 and 106).

42. Describe the precise low-level effects of the **`call Q`** instruction and its counterpart **`ret`** on the **Program Counter (PC)** and the **Stack Pointer ($\% \mathbf{rsp}$)**. Why does the **Last-In, First-Out (LIFO)** discipline of the run-time stack perfectly match the control flow requirements for general procedure calls (including recursion)?

43. A procedure must allocate storage on the stack if a local variable needs to have its address taken (e.g., `&arg1`) or if it's an array/structure. Explain the compiler's two-step strategy for managing such local storage:

    - **Allocation:** What single instruction is used at the start of the function, and what calculation determines its immediate operand?
    - **Access:** How are these local variables accessed during the function's execution, in terms of the stack pointer ($\% \mathbf{rsp}$)?

44. State the registers used for the first six integral arguments according to the **x86-64 ABI**. If a function is called with a 32-bit integer as the first argument, explain which register is used to pass the value and how the full 64-bit register is affected to ensure compatibility.

45. When a procedure has more than six integral arguments, the excess arguments are passed on the stack.

    - In which procedure's stack frame (the **caller's** or the **callee's**) do the 7th and subsequent arguments ultimately reside?
    - What is the **offset** (in bytes) relative to the callee's $\% \mathbf{rsp}$ where the callee can first access the 7th argument? Justify this offset based on the content pushed by the `call` instruction.

46. According to the ABI, where is the return value of a function (e.g., a `long` or a pointer) stored? What happens if the function returns a large data type, such as a **struct** that exceeds 64 bits? (Hint: Consider the compiler's strategy of passing a hidden pointer.)

47. Registers $\% \mathbf{rbx}, \% \mathbf{rbp}$, and $\% \mathbf{r12}$ through $\% \mathbf{r15}$ are designated as **callee-saved**.

    - If a function $\text{P}$ (the caller) relies on a value in $\% \mathbf{rbx}$, and calls function $\text{Q}$, describe the **exact assembly sequence** $\text{Q}$ must execute at its **entry** and **exit** to ensure $\% \mathbf{rbx}$'s value is preserved and restored.

48. Explain the scenario where the **caller** ($\text{P}$) is forced to save a register, even though the register is designated as **caller-saved**. Why does this register convention (caller-saved) lead to more efficient code overall, despite the potential overhead?

49. The use of the stack and the register-saving conventions are sufficient to implement **recursive procedures**. Explain how the stack naturally isolates the state of one recursive call (e.g., its local variables and return address) from all other **simultaneously active** calls of the same function.

50. The x86-64 ABI mandates a crucial performance rule regarding the stack: **The stack pointer ($\% \mathbf{rsp}$) must be aligned to a 16-byte boundary just before a `call` instruction executes.**

    - Given that the `call` instruction pushes an 8-byte return address, what is the guaranteed alignment (relative to a 16-byte boundary) of the **stack pointer** immediately upon entry into the callee function?
    - Why is this 16-byte alignment rule critical for system performance, especially concerning certain **floating-point or vector instructions**?

51. If a procedure (the callee) requires allocating a stack frame for local variables, the total size allocated (via `subq` on $\% \mathbf{rsp}$) must ensure that the 16-byte alignment is preserved for any _further_ functions it calls. If the callee needs $X$ bytes for local variables and saved registers, the stack frame size must be adjusted to the nearest multiple of 16. Formulate the mathematical expression for the minimum required allocation size, $A$, in terms of $X$, that maintains the 16-byte alignment before any subsequent inner calls.

52. The register $\% \mathbf{rbp}$ historically serves as the **Frame Pointer**.

    - What fixed reference point does the Frame Pointer provide, and why is this particularly valuable for **debugging** or in unoptimized code (e.g., when compiled with `-O0`)?
    - In highly optimized code, the compiler often **repurposes** $\% \mathbf{rbp}$ as a general-purpose callee-saved register. Explain the architectural trade-off here: what performance benefit is gained, and what debugging feature is sacrificed?

53. Describe the low-level, two-step operation performed by the assembly instruction **`pushq S`** in terms of the stack pointer (`%rsp`) and memory access $\mathbf{M}_8$:

    - Step 1: Stack Pointer update ($\mathbf{R}[\% \mathbf{rsp}] \leftarrow \dots$)
    - Step 2: Memory Write ($\mathbf{M}_8[\mathbf{R}[\% \mathbf{rsp}]] \leftarrow \dots$)
      Justify why the stack pointer is updated _before_ the memory write.

54. For a declaration `T A[N];` where $T$ has a size of $L$ bytes, state the formula for the memory address of element $A[i]$. Then, explain the architectural significance of the x86-64 instruction set allowing scaled addressing factors of $1, 2, 4,$ and $8$. How do these fixed factors simplify the translation of common C array and pointer arithmetic?

55. The C expression `A[i]` is identical to the pointer expression `*(A + i)`.

    - Explain the **scaling operation** that the machine code must perform on the index $i$ before it is added to the base address $A$ to implement this pointer arithmetic correctly.
    - Provide the single x86-64 instruction that computes the data address $A + L \cdot i$ and loads the value into a register, assuming the base address $A$ is in $\% \mathbf{rdx}$, the index $i$ is in $\% \mathbf{rcx}$, and the element size $L=4$.

56. C stores multi-dimensional arrays (e.g., `T D[R][C];`) in memory using **row-major order**.

    - Provide the general formula for the memory address of element $D[i][j]$, where $L$ is the size of type $T$. Explain how the number of columns ($C$) serves as the effective **stride** or scale factor when calculating the offset for row $i$.

57. In the assembly code for accessing a nested array $P[i][j]$, you observe the following address computation sequence:

    1. `leaq 0(,%rdi,8), %rdx`
    2. `subq %rdi, %rdx`
    3. `addq %rsi, %rdx`
       Assuming $\% \mathbf{rdi}$ holds the row index $i$ and the element size is 8 bytes (`long`), reverse engineer the C expression being computed in $\% \mathbf{rdx}$. Then, determine the **fixed column dimension $N$** of the array, $P[M][N]$, that results in this assembly code optimization.

58. For fixed-size arrays (like matrix multiplication), compilers often optimize array access by replacing index-based looping with **pointer arithmetic**.

    - When computing the inner product of row $i$ of matrix $A$ and column $k$ of matrix $B$ (where $A[i][j]$ and $B[j][k]$ are accessed inside the loop), describe the two distinct memory offsets that must be added to the pointers of $A$ and $B$ in each iteration to traverse the row and column, respectively.

59. Explain why compilers can’t optimize variable-size array indexing as well as fixed-size array indexing.

60. Explain the critical architectural difference in memory layout and access between a **Nested Array** (`int D[R][C]`) and an **Array of Pointers** (`int *D[R]`).

    - Which structure guarantees a **single, contiguous block** of memory, enabling the fast, single-step address calculation $x_D + L \cdot (C \cdot i + j)$?
    - Describe the minimum **number of memory lookups** (dereferences) required at the assembly level to access an element $D[i][j]$ in the **Array of Pointers** structure.

61. C arrays use **Row-Major Order**. Define this term explicitly. Why is this specific memory ordering choice—as opposed to column-major order—a crucial factor for optimizing program performance and exploiting **data caching** in loops that iterate through rows?

62. The scale factor $s$ in the x86-64 addressing mode can only be $1, 2, 4,$ or $8$.

    - If an array consists of elements (e.g., a custom `struct`) with a size $L=12$ bytes, what **multi-instruction sequence** must the compiler generate to compute the offset $L \cdot i$, and what specific assembly instruction replaces the single-step addressing mode in this scenario? Justify why this represents a performance cost.

63. For a fixed-size array $D[R][C]$, the address calculation $x_D + L \cdot (C \cdot i + j)$ uses known constants. For a **Variable-Sized Array (VLA)** defined as `T D[N][N]` where $N$ is only known at runtime, the compiler must perform a dynamic calculation.

    - Identify the **specific arithmetic operation** in the address calculation that requires a runtime execution of the **`imulq`** instruction, which could otherwise be handled by a simpler shift or `leaq` for a fixed-size array.
    - Quantify the **performance penalty** incurred by the VLA implementation due to this runtime calculation versus the compiled-in constant calculation for a fixed-size array.

64. Structures store all components in a **contiguous region of memory**. Explain the compiler's low-level mechanism for accessing a specific field (e.g., `r->j`) when given the base address of the structure (`r`). Why is this access performed entirely at **compile time**, and what specific information must the compiler know about the structure?

65. Assume a pointer `r` to a structure `struct rec` is in register $\% \mathbf{rdi}$, and an index $i$ is in $\% \mathbf{rsi}$. Write the single **`leaq`** instruction that computes the pointer value for the structure element **`&(r->a[i])`** and stores it in $\% \mathbf{rax}$. Explain how the instruction's components map to the structure's base address, array offset, and element size.

66. The C notation `rp->width` is syntactic sugar for `(*rp).width`. Describe the exact, minimal sequence of assembly instructions required to compute the value of `rp->width`, assuming the structure pointer `rp` is in register $\% \mathbf{rdi}$ and the `width` field has an offset of $O$ bytes and size $L$ bytes.

67. Define the memory allocation strategy of a C **`union`** (e.g., `union U3`). How does this strategy differ fundamentally from a **`struct`**, and how is the overall size of the union determined?

68. A common, non-portable systems application of a union is to bypass the type system to inspect the raw bit pattern of a data type.

    - Given a union containing a `double d` and an `unsigned long u`, explain the mechanism by which storing a value in `d` and then reading it from `u` allows a programmer to retrieve the **IEEE 754 bit pattern** without affecting the raw bits.
    - Why does the numeric value of `u` generally bear **no relation** to the numeric value of `d` after this operation?

69. The x86-64 architecture, while flexible, recommends that primitive objects of size $K$ bytes be aligned to addresses that are multiples of $K$.

    - Why does maintaining this alignment (e.g., 8-byte alignment for a `double` or `long`) simplify the hardware design of the memory interface and improve system performance?

70. Consider a structure `struct S1 { int i; char c; int j; }`. If the fields were minimally packed, `j` would start at an offset of 5 bytes.

    - Explain why the compiler must insert **internal padding** bytes between `c` and `j`.
    - Calculate the minimum number of padding bytes required between `c` and `j` to ensure that `j` satisfies its 4-byte alignment requirement, assuming the structure starts at a 4-byte aligned address.

71. If a structure must be used in an array (e.g., `struct S2 d[4];`), the compiler may add **padding to the end** of the structure.

    - Explain why this end padding is necessary. What specific alignment property of the **entire structure's size** must be satisfied to guarantee that every element $d[i]$ in the array also satisfies its maximum alignment requirement?

72. In C, every pointer has an associated type (e.g., `int *`, `char **`). Why are these **pointer types not a part of the machine code** (the binary executable)? What is the primary role of pointer typing that the C compiler maintains to assist the programmer?

73. If a pointer `p` of type `char *` has the address value $X$, explain the numerical difference in the resulting memory address when calculating the C expressions:

    - `(long *) p + 1`
    - `(long *) (p + 1)`
      Relate your answer to the **scaling factor** used in pointer arithmetic and the **precedence** of the cast operation.

74. Function pointers provide a powerful capability for dynamic control flow.

    - If `fp` is a function pointer, what value does `fp` store?
    - When the function is invoked via `fp(...)`, what x86-64 instruction is executed to transfer control, and how does this differ from a standard direct `call` instruction?

75. Explain the precise mechanism by which a **buffer overflow** (writing past the end of a local stack array like `char buf[8]`) can lead to a program crashing or executing malicious code. What critical piece of program state, stored immediately above the buffer, is typically corrupted to redirect control flow?

76. Describe the **stack protector mechanism** (using a **canary** value) implemented by modern compilers like GCC.

    - Where is the canary value typically stored relative to the local buffer and the return address?
    - What instruction sequence is executed immediately before a function returns to detect corruption, and how does the use of a **randomized** canary value thwart an attacker who cannot inspect the running system?

77. What is the security goal of **ASLR**, and how does it attempt to thwart buffer overflow attacks? Briefly describe the **"NOP sled"** technique attackers use to attempt to bypass ASLR, and why this technique is less effective in 64-bit systems compared to 32-bit systems.

78. Name the three generations of media registers that evolved with SIMD extensions (MMX, SSE, AVX). What is the primary purpose of **SIMD** (Single Instruction, Multiple Data)?

79. When x86-64 operates on **scalar floating-point data** (individual `float` or `double`), which portion of the 256-bit **YMM register** is primarily used? By what name does the assembly code typically refer to this lower, 128-bit portion?

80. How many **YMM/XMM registers** are available in the AVX architecture for storing floating-point data?

81. What instruction class (`vmovss`, `vmovsd`, etc.) is generally used to move a scalar floating-point value between a memory location and an XMM register?

82. How many floating-point arguments (up to a maximum) can be passed to a function using the **XMM registers** under the x86-64 convention? What XMM register is used for returning a floating-point value?

83. Are the XMM registers designated as **caller-saved** or **callee-saved**? What is the practical implication of this convention for a function ($\text{P}$) that calls another function ($\text{Q}$)?

84. Unlike integer arithmetic, AVX floating-point instructions **cannot use immediate values** as operands. How must the compiler define and access floating-point constants (like $1.8$) for use in assembly arithmetic instructions?

85. The floating-point comparison instructions (`ucomisd`, etc.) set the standard $\text{ZF}$ (Zero Flag) and $\text{CF}$ (Carry Flag), but also introduce the $\text{PF}$ (Parity Flag). What specific condition does the $\text{PF}$ flag indicate, and why is this condition important in C floating-point logic (e.g., $x == x$)?
