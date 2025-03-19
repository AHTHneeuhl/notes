### Addressing Modes in a CPU

Addressing modes define how the CPU interprets the operand address of an instruction. Here are the common types:

1. **Immediate Addressing**: The operand is a constant value embedded directly in the instruction.
   - Example: `MOV R1, #10` (Load the value 10 into register R1).

2. **Register Addressing**: The operand is stored in a CPU register.
   - Example: `ADD R1, R2, R3` (Add the contents of R2 and R3, store the result in R1).

3. **Direct Addressing**: The operand address is specified directly in the instruction.
   - Example: `LOAD R1, 2000` (Load the contents of memory address 2000 into R1).

4. **Indirect Addressing**: The instruction specifies a register or memory location that contains the address of the operand.
   - Example: `LOAD R1, (R2)` (Load the contents of the memory address stored in R2 into R1).

5. **Indexed Addressing**: The operand address is calculated by adding an offset to a base address stored in a register.
   - Example: `LOAD R1, 100(R2)` (Load the contents of memory address 100 + value in R2 into R1).

6. **Base-Register Addressing**: Similar to indexed addressing, but the base address is fixed, and the register provides an offset.
   - Example: `LOAD R1, (R2 + 100)` (Load the contents of memory address R2 + 100 into R1).

7. **Relative Addressing**: The operand address is calculated relative to the program counter (PC).
   - Example: `JUMP +50` (Jump to the address 50 bytes ahead of the current PC).

8. **Stack Addressing**: Operands are implicitly accessed from the stack (e.g., push/pop operations).
   - Example: `PUSH R1` (Push the contents of R1 onto the stack).

---

### Pipelining and Performance Improvement

**Pipelining** is a technique where multiple instructions are overlapped in execution, similar to an assembly line. It improves performance by allowing the CPU to work on multiple instructions simultaneously, increasing throughput.

#### Types of Hazards in Pipelining:
1. **Structural Hazards**: Occur when hardware resources are insufficient to support overlapping execution (e.g., two instructions needing the same memory unit).
2. **Data Hazards**: Occur when an instruction depends on the result of a previous instruction that hasn’t completed yet.
   - Example: `ADD R1, R2, R3` followed by `SUB R4, R1, R5` (R1 is not yet updated).
3. **Control Hazards**: Occur due to branch instructions, where the next instruction to execute is unknown until the branch is resolved.
   - Example: `JUMP 100` (The CPU doesn’t know the next instruction until the jump is evaluated).

---

### RISC vs. CISC Architectures

- **RISC (Reduced Instruction Set Computer)**:
  - Simpler, fixed-length instructions.
  - Fewer addressing modes.
  - Emphasizes pipelining and high clock speeds.
  - Example: ARM, RISC-V.

- **CISC (Complex Instruction Set Computer)**:
  - Complex, variable-length instructions.
  - Many addressing modes.
  - Single instructions can perform multiple operations.
  - Example: x86.

**Modern Preference**: RISC is preferred in modern processors (e.g., ARM in mobile devices) due to its simplicity, energy efficiency, and suitability for pipelining. However, CISC architectures like x86 are still widely used in desktops and servers due to backward compatibility and high performance for complex tasks.

---

### CPI, MIPS, and IPC

- **CPI (Cycles Per Instruction)**: Average number of clock cycles required to execute an instruction.
- **MIPS (Million Instructions Per Second)**: Number of instructions executed per second (in millions).
- **IPC (Instructions Per Cycle)**: Average number of instructions executed per clock cycle.

**Relationship**:
- $\text{IPC} = \frac{1}{\text{CPI}}$
- $\text{MIPS} = \frac{\text{Clock Frequency (in MHz)}}{\text{CPI}}$

---

### Calculating MIPS Rating

Given:
- Clock cycle time = 2 ns (0.5 GHz or 500 MHz).
- IPC = 1.5.

Steps:
1. Calculate CPI: $\text{CPI} = \frac{1}{\text{IPC}} = \frac{1}{1.5} = 0.67$.
2. Calculate MIPS: $\text{MIPS} = \frac{\text{Clock Frequency (in MHz)}}{\text{CPI}} = \frac{500}{0.67} \approx 746.27$.

**MIPS Rating**: Approximately 746.27 MIPS.