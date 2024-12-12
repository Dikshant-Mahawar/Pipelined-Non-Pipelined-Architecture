# Non-Pipelined and Pipelined Architecture Simulation

This project simulates both non-pipelined and pipelined processor architectures. It implements various stages of instruction processing, such as fetching, decoding, execution, memory access, and writing back. The project also includes optimizations for handling pipeline dependencies and adding stalls when necessary.

1. Non-Pipelining

1.1 Fetch Instruction
This function takes the instruction memory and program counter (PC) as parameters and returns the instruction located at the current program counter (PC) in the instruction memory.

1.2 Binary Neg Decimal Function
This function converts a binary value to a decimal value. It handles negative cases by checking the most significant bit (MSB) and converting accordingly.

1.3 Decode Instruction
This function decodes an instruction based on its opcode. It supports various instruction types such as:

R-type: Register-type instructions.
lw (load word): Loads a word from memory into a register.
sw (store word): Stores a word from a register into memory.
addi (add immediate): Adds an immediate value to a register.
bne (branch not equal): Branches if two registers are not equal.
beq (branch equal): Branches if two registers are equal.
mul (multiply): Multiplies two registers.
li (load immediate): Loads an immediate value into a register.
jal (jump and link): Jumps to a subroutine and stores the return address.
j (jump): Unconditional jump to an address.
1.4 Execute Instruction
This function executes the decoded instruction. It determines the control lines for write-back (wb), write memory (wm), and read memory (rm). It also calculates the result for R-type instructions (rd1) and I-type instructions (rt1).

1.5 Memory Access Function
This function handles memory access based on the control lines for write memory (wm) and read memory (rm). It either reads from or writes to data memory as needed.

1.6 Write Back Function
This function updates the register memory (reg memory) based on the result (rd1) and the destination register (rd).

2. Pipelining

2.1 Dependency Function
The dependency function checks if the current instruction has any dependencies on the previous two instructions. To find dependencies, it compares the destination registers of the previous two instructions with any of the source registers of the current instruction. A dependency array is created, where "true" indicates a dependency and "false" indicates no dependency.

2.2 Adding Stalls
To handle dependencies, stalls are added to the pipeline. Multiple variables act as indices for each stage of the pipeline: fetch, decode, execute, memory, and write-back. If a dependency is found, two stalls are added, and the affected instructions are delayed by two cycles. For example:

In the decode stage, if a dependency is detected, the instruction will not proceed to the execute stage for two more cycles.
The fetch stage is delayed by two cycles until the dependent instruction moves to the execute stage.
These delays are managed by adjusting the values of the fetch-idx, dec-idx, exe-idx, mem-idx, and wb-idx variables.

2.3 Handling BEQ and BNE
Branch instructions such as BEQ (branch equal) and BNE (branch not equal) can be checked in any stage. In this project, we check for these conditions in the fetch stage. If a jump instruction is encountered (such as BEQ or BNE), the value of fetch-idx is updated to the jump address. Additionally, instructions after the jump instruction are not executed until the jump is performed, ensuring correct program flow.

3. Features

Non-pipelined Architecture: Implements a simple single-cycle instruction processing architecture.
Pipelined Architecture: Implements a pipeline with stall handling and dependency management for improved performance.
Branch Handling: Efficient handling of branch instructions like BEQ and BNE in the fetch stage to minimize delays.
Support for Multiple Instruction Types: The project supports various instruction types including R-type, load, store, and jump instructions.
