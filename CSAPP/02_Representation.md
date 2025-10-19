1. Justify the necessity of hexadecimal notation over binary or decimal in the context of debugging, memory dumps, and low-level programming. Specifically, what compression of information does hex provide, and what is the direct, fixed relationship it exploits with the underlying bit patterns that makes it superior to decimal for representing memory contents?

2. A single hexadecimal digit maps directly to how many bits? Explain this relationship and why it ensures easy, direct conversion between binary and hex, a property lacking with decimal.

3. State the full range of values a single **byte** can represent when expressed in hexadecimal notation. What constraint on the number of hexadecimal digits per byte does this range impose?

4. Consider the bit pattern `0xCAFE`. What are the **signed and unsigned decimal values** represented by this pattern on a system using a **two's complement 16-bit integer**? Show the calculation for the signed value.

5. Define the **word size ($w$)** of a CPU. Explain how the word size simultaneously dictates three major, interconnected system constraints: the **maximum size of an integer** an arithmetic logic unit (ALU) can process in a single operation, the **size of memory addresses** (and thus the addressable memory limit), and the **size of general-purpose registers**.

6. A processor has a word size of $w$ bits. Formulate the mathematical expression for the **maximum number of unique memory addresses** that can be directly accessed (the addressable memory space). If a system with a 32-bit word size is conceptually limited to $4 \text{ GiB}$ of addressable memory, what is the maximum addressable memory for a 64-bit system, assuming a byte-addressable architecture?

7. Can a $w$-bit processor efficiently and correctly manipulate data structures or perform arithmetic on values that are **larger than $w$ bits** (e.g., a 64-bit integer on a 32-bit machine)? If so, describe the general **mechanism (or sequence of operations)** the compiler and hardware must employ.

8. The C standard does _not_ guarantee an exact number of bytes for all fundamental data types (e.g., `int` vs. `long int`). What **minimum size constraint** _is_ guaranteed for types like `int` and `short`, and why is this **implementation-defined flexibility** a design choice that prioritizes historical compatibility and performance across diverse architectures, even at the cost of strict binary portability?

9. In what **specific system contexts or applications** (e.g., array indexing, memory size calculations, flags/masks) is the use of an **unsigned integer** not just preferred, but **mechanistically necessary** or logically superior to a signed integer? Conversely, when is a signed integer mandatory?

10. The primary difference between executing a 32-bit program and a 64-bit program on a 64-bit operating system lies in the **size of memory pointers** and the **register usage convention**. Explain how the choice of 32-bit or 64-bit compilation for a program (not the OS) impacts both its **maximum addressable heap/stack space** and the **efficiency of function calls** due to the number of general-purpose registers available.

11. Define **Little-Endian** and **Big-Endian** byte ordering in terms of the **address of the most significant byte (MSB)** relative to the least significant byte (LSB) for a multi-byte data word (e.g., a 4-byte integer). If the 4-byte value $0x12345678$ is stored starting at address $0x100$, provide the byte stored at address $0x101$ for both little-endian and big-endian systems.

12. Why is **endianness a critical issue** when transmitting multi-byte binary data (like an integer or floating-point number) between two systems over a network, and what specific **network protocol solution** (name and mechanism) is commonly used to standardize data format and _avoid_ this architectural mismatch failure?

13. Explain why a simple, human-readable text file (like one using ASCII or UTF-8) is considered far more **machine-independent** for data exchange than a file containing raw binary data (e.g., a struct dumped directly to disk). Relate your answer to the concepts of endianness and data type size variability.

14. Differentiate between **bitwise operations ($\&, |, \sim$)** and **logical operations ($\&\&, ||, !$)** in C/C++ based on two criteria:

    - The **type of operands** they operate on and the **value type** they return.
    - The **short-circuit evaluation mechanism**—explain precisely why logical operators exhibit this behavior, while bitwise operators are **required to evaluate all operands**, and why this distinction is critical in system-level programming for performance and correctness.

15. In systems programming, why is the expression **`~0`** a fundamentally **more portable and robust bitmask** for setting all bits to 1 than explicitly hardcoding a constant like `0xFFFFFFFF` or `0xFFFFFFFFFFFFFFFF`? Relate your answer to the **word size ($w$) independence** provided by the bitwise NOT operator.

16. Describe the **mechanism of an arithmetic right shift ($\gg$)** for a signed integer, paying close attention to how the **Most Significant Bit (MSB)**, often the sign bit, is handled. Explain _why_ the hardware is designed to perform this sign-extension—what mathematical property of two's complement representation is preserved by this action?

17. The C standard leaves the behavior of the **right shift operator ($\gg$)** on signed integers **implementation-defined** (allowing for logical or arithmetic shift) or **undefined** (if shifting a negative number). Given this ambiguity, why is it considered a **systems best practice** to explicitly cast an integer to an **unsigned type** _before_ performing a right shift operation if a **logical shift** is required, ensuring maximum portability and predictability?

18. From a hardware architecture perspective, do modern CPUs execute bitwise operations ($\&, |, \wedge$) on a 64-bit integer **simultaneously across all 64 bit positions** or **consecutively**? Briefly explain the design principle (e.g., parallelism within the ALU) that dictates this performance choice.

19. Does the C standard mandate that signed integers must be represented using the Two's Complement scheme? If not, identify the three permissible options under the C standard (C99/C11), and explain why the standard maintains this flexibility

20. For a $w$-bit binary representation using Two's Complement, state the minimum and maximum representable integer values in terms of $w$. Explain the asymmetry in this range—why is there one negative number with no positive counterpart—and identify this specific value (e.g., $T_{\text{min}}$).

21. Describe the precise consequence and failure mode that occurs in C when a programmer attempts to calculate the absolute value of the minimum two's complement integer, $T_{\text{min}}$ (e.g., abs(TMin)). What mathematical property of two's complement is violated, and what value is incorrectly produced?

22. Identify the most common computer architecture representation for signed integers. Why has this particular scheme (Two's Complement) become dominant over alternatives like One's Complement or Signed Magnitude, specifically concerning the simplification of arithmetic hardware (e.g., addition and subtraction) and the handling of the number zero?

23. When a signed integer variable is explicitly cast to its corresponding unsigned type (e.g., (unsigned int)x), what fundamental characteristic of the value is preserved, and what characteristic is fundamentally altered? Specifically, explain how the underlying bit pattern relates to the numeric value interpretation across this conversion.

24. When converting a value from a smaller integer type to a larger integer type (e.g., short to long), describe the two distinct bit-extension mechanisms employed . Under what precise conditions (based on the original type's signedness) is each mechanism selected, and why is this choice critical for preserving the numeric value of the original integer?

25. Consider the expression (unsigned int)s, where s is a negative signed short. At the binary level, does the system first perform a type promotion (e.g., to signed int) and then the unsigned reinterpretation (casting), or vice-versa? Explain the sequence required to correctly yield the final unsigned int value.

26. Define truncation in the context of integer conversion. Describe the specific, low-level binary operation that implements truncation (e.g., for converting a 32-bit int to an 8-bit char), and how this process inherently leads to loss of the Most Significant Bits (MSBs) and potential data corruption

27. Explain why unsigned integers are the preferred and most logical choice for low-level tasks that require treating data as a collection of raw bits (e.g., creating bitmasks, implementing bitsets, or working with hardware registers)

28. In C, are integer constants (e.g., 123, 0xAF) treated as signed or unsigned by default? Explain the rule C uses to determine the type of an un-suffixed integer constant, considering the value and the sizes of the standard integer types.

29. The C standard deliberately avoids specifying the exact binary mechanism for conversions between signed and unsigned integers (it only specifies the final mathematical result). Why does the standard prioritize the mathematical outcome over the binary mechanism, and how does this relate to the standard's need to support architectures that use signed representations other than Two's Complement?

30. Explain the integer promotion rule regarding signed and unsigned types in C expressions (e.g., signed_var < unsigned_var). Describe the subtle bug that results from the implicit conversion of the signed operand to an unsigned type, leading to a mathematically incorrect comparison. Provide a small code snippet demonstrating an instance where this behavior could lead to a buffer overflow or other system vulnerability.
