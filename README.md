# SAP-1: 8-Bit Computer

This repository contains the documentation and design files for a hardware implementation of the **SAP-1 (Simple As Possible 1)** computer. This project demonstrates the fundamental "von Neumann" architecture, showing how a CPU fetches, decodes, and executes instructions using 7400-series TTL components.



## Architecture Overview and Documentation

The SAP-1 is a bus-organized computer. All major registers and components communicate via a common **8-bit W-bus**. The machine operates on a 16-byte memory space with a 4-bit address bus and an 8-bit data bus.

### Core Modules
* **Program Counter:** A 4-bit counter that tracks the next instruction address (ranges from `0000` to `1111`).
* **Input/MAR:** The Memory Address Register latches the address from the bus; the input section allows manual programming of the RAM via toggle switches.
* **RAM:** 16 x 8-bit asynchronous static RAM.
* **Instruction Register:** Split into two 4-bit nibbles the upper nibble (opcode) goes to the Controller Sequence, and the lower nibble (operand) can be fed back onto the W-bus.
* **Accumulator (Register A):** An 8-bit register that serves as the primary workspace for all ALU operations.
* **Adder-Subtractor:** An 8-bit asynchronous ALU that uses 74LS283 adders and XOR gates for 2’s complement subtraction.
* **B Register:** A buffer register that holds the second operand for math operations.

---

##  The Control Word 

The power of the SAP-1 lies in its **12-bit control word**. By toggling these bits high or low, the controller dictates exactly how data flows. 

The control signals are defined as:
$$C_P, E_P, \overline{L_M}, \overline{CE}, \overline{L_I}, \overline{E_I}, \overline{L_A}, E_A, S_U, E_U, \overline{L_B}, \overline{L_O}$$

* **Load (L) bits:** Active-low signals that command a register to latch data from the bus.
* **Enable (E) bits:** Active-high signals that command a register to output its data onto the bus.

---

##  Instruction Set Architecture 

The SAP-1 uses a 4-bit opcode. Below is the instruction set implemented in this build:

| Mnemonic | Opcode | Description |
| :--- | :--- | :--- |
| **LDA** | `0000` | Load Accumulator with RAM data |
| **ADD** | `0001` | Add RAM data to Accumulator |
| **SUB** | `0010` | Subtract RAM data from Accumulator |
| **OUT** | `1110` | Load Accumulator data into Output Register |
| **HLT** | `1111` | Stop the clock and halt execution |



---

##  Machine Cycle & Timing

Every instruction follows a 6-state ($T$-state) timing sequence:

1. **Fetch Cycle ($T_1, T_2, T_3$):** - $T_1$: Address from PC is sent to MAR.
   - $T_2$: PC increments.
   - $T_3$: Instruction from RAM is loaded into the IR.
2. **Execution Cycle ($T_4, T_5, T_6$):** - The controller decodes the opcode and activates the necessary $CON$ bits to move data between the Accumulator, ALU, and Registers.

---

##  Build Specifications
* **Logic Family:** 74LS (Low-power) TTL.
* **Clock:** NE555 timer with manual/auto toggle modes.
* **Visuals:** LED arrays for real-time monitoring of the W-bus and all internal registers.

---

## Future of this project
  Sadly, due to hardware limitations this project is coming to a close. Originally, the plan was to create a system that could run a "simple" game such as 'Pong' or 'Tetris'. It's simply not feasible using the barebones design this was created with. Now it's time to move on to project SAP2 with the goal to being able to run Assembler, which would facilitate easier programming.

## Future documentation
Over the upcoming weeks I will be uploading the steps I took to create each part, along with the data sheet. 
