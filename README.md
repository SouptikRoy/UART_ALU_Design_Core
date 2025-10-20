# UART-ALU System (Verilog)

## 📘 Overview

This project implements a **UART-based Arithmetic Logic Unit (ALU) system** using Verilog HDL. The system receives instructions over UART, processes them in the ALU, and sends the results back via UART. It demonstrates **digital design integration**, **control logic**, **UART communication**, and **modular Verilog design**.

The project serves as an educational platform for understanding:

* Serial communication protocols (UART)
* Instruction decoding and execution
* ALU operation handling
* Integration of multiple modules in FPGA/ASIC design

---

## 🧩 Project Structure

```
UART_ALU/
├── uart_rx.v           # UART receiver module
├── uart_tx.v           # UART transmitter module
├── alu_core.v          # ALU processing module
├── controller.v        # Control unit for instruction decoding and data flow
├── UART_ALU_top.v      # Top-level module integrating UART and ALU
├── UART_ALU_tb.v       # Testbench for waveform and functional simulation
├── utils.v             # Optional utility functions for simulation (baud generators, etc.)
└── README.md           # Project documentation
```

---

## ⚙️ Design Description

### 🧠 Top Module: `UART_ALU_top`

* Integrates **UART RX**, **controller**, **ALU core**, and **UART TX**.
* Handles instruction reception, ALU computation, and transmission of results.
* Interfaces:

  * `rx` : Serial input
  * `tx` : Serial output
  * `clk` : System clock
  * `rst` : Reset signal
* Designed to be modular so that UART or ALU components can be replaced or upgraded independently.

### 🔹 UART Receiver: `uart_rx`

* Receives serial data from external devices.
* Converts serial input into parallel data bytes.
* Generates `data_ready` signal when a byte is fully received.
* Configurable baud rate via parameter.
* Includes start, stop, and parity bit handling.
* Handles edge detection for precise sampling.

### 🔹 Controller: `controller`

* Decodes received instructions from UART into ALU operations.
* Maintains a finite state machine (FSM) for data flow management:

  1. **IDLE**: Wait for incoming instruction
  2. **FETCH**: Capture operands
  3. **EXECUTE**: Trigger ALU computation
  4. **SEND**: Prepare ALU result for UART transmission
* Generates control signals: `alu_op`, `alu_enable`, `tx_data_valid`
* Can be extended to support multiple ALU operations and instruction sets.

### 🔹 ALU Core: `alu_core`

* Performs arithmetic and logic operations on input operands.
* Supported operations: `ADD`, `SUB`, `AND`, `OR`, `XOR`, `NOT`, `SHIFT_LEFT`, `SHIFT_RIGHT`.
* Configurable data width (8-bit, 16-bit, etc.) via parameters.
* Produces status flags such as `zero_flag`, `carry_flag`, and `overflow_flag`.
* Outputs result to controller for transmission.

### 🔹 UART Transmitter: `uart_tx`

* Sends computed ALU result back as serial data.
* Waits for `tx_data_valid` signal from controller.
* Converts parallel ALU output into UART serial stream.
* Supports configurable baud rate and stop/start bits.
* Includes FSM for transmission: **IDLE → SEND → WAIT**

---

## 🧪 Testbench: `UART_ALU_tb.v`

* Simulates sending instructions over UART to the ALU system.
* Monitors ALU result and TX output.
* Verifies proper operation of RX → Controller → ALU → TX data flow.
* Example stimulus:

```verilog
// Send ADD instruction
send_uart_byte(8'h01); // Opcode for ADD
send_uart_byte(8'h05); // Operand A
send_uart_byte(8'h03); // Operand B
```

* Ensures all ALU operations are correctly processed and transmitted.
* Can simulate timing constraints, baud rate mismatches, and edge cases for robust verification.

---

## 📊 Waveform Verification

1. Compile all modules: `uart_rx.v`, `controller.v`, `alu_core.v`, `uart_tx.v`, `UART_ALU_top.v`, `UART_ALU_tb.v`.
2. Run simulation in your simulator (ModelSim, Vivado, eSim).
3. Observe waveforms for:

   * `rx_data` received correctly.
   * `alu_result` matches expected computation.
   * `tx` output correctly serialized.
   * Control signals (`alu_enable`, `tx_data_valid`) toggle correctly.
4. Use `$monitor` or waveform viewer to inspect timing and correctness.

---

## 📝 Notes

* Ensure baud rate settings of UART RX/TX match your testbench or external device.
* Reset (`rst`) must be asserted at the start of simulation.
* Controller FSM can be modified to support additional instructions or complex operations.
* Top module design allows modular testing and easy expansion.
* UART modules are fully parameterized for flexible design reuse.

---

## 🎯 Key Learning Points

* UART communication in FPGA/ASIC design.
* Integration of ALU with control logic.
* Instruction decoding and data flow management.
* Modular and hierarchical Verilog design.
* Testbench creation for serial communication and ALU verification.
* FSM design for control and transmission.

---

## 📂 References

* Verilog HDL Reference Manual
* FPGA UART design tutorials
* ALU design textbooks and FPGA guides
* Digital system design and FSM references
