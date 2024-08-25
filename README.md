# RAM and ROM Design Using Verilog

## Overview

This project involves the design and implementation of RAM (Random Access Memory) and ROM (Read-Only Memory) modules using Verilog. The project includes the following modules:
- **Dual Port RAM**: A RAM module with two separate ports allowing simultaneous read and write operations.
- **ROM**: A simple ROM module with predefined memory content.

## Project Structure

- **`dual_port_ram.v`**: Verilog module for Dual Port RAM.
- **`rom.v`**: Verilog module for ROM.

## Dual Port RAM Design

### Description

The Dual Port RAM module supports simultaneous read and write operations on two separate ports (Port A and Port B). This allows for flexible data access and storage, making it useful for systems requiring parallel data processing.

### Module Definition

```verilog
module dual_port_ram(
  input [7:0] data_a, data_b,       // Input data for Port A and Port B
  input [5:0] addr_a, addr_b,       // Addresses for Port A and Port B
  input we_a, we_b,                 // Write enable signals for Port A and Port B
  input clk,                        // Clock signal
  output reg [7:0] q_a, q_b         // Output data from Port A and Port B
);

  reg [7:0] ram [63:0];             // 8x64-bit RAM storage

  always @ (posedge clk) begin
    if (we_a)
      ram[addr_a] <= data_a;        // Write to Port A
    else
      q_a <= ram[addr_a];           // Read from Port A
  end

  always @ (posedge clk) begin
    if (we_b)
      ram[addr_b] <= data_b;        // Write to Port B
    else
      q_b <= ram[addr_b];           // Read from Port B
  end

endmodule
```

### Features
- **Data Width**: 8 bits.
- **Address Width**: 6 bits, allowing for 64 memory locations.
- **Dual Ports**: Supports simultaneous operations on two ports (Port A and Port B).
- **Synchronous Operation**: All operations are synchronized with the clock signal.

### Simulation and Testing

To test the Dual Port RAM module, a testbench should be created to verify the following scenarios:
- Write and read operations on both ports simultaneously.
- Different address and data inputs for each port.
- Verification of data integrity during simultaneous operations.

## ROM Design

### Description

The ROM module stores predefined data that can be accessed using a 4-bit address. The data is initialized in the `initial` block and is read during operation.

### Module Definition

```verilog
module rom (
  input clk,                        // Clock signal
  input en,                         // Enable signal
  input [3:0] addr,                 // Address input
  output reg [3:0] data             // Output data
);

  reg [3:0] mem [15:0];             // 4x16-bit ROM storage

  always @ (posedge clk) begin
    if (en)
      data <= mem[addr];            // Read from ROM
    else
      data <= 4'bxxxx;              // High-impedance state when disabled
  end

  initial begin                     // ROM content initialization
    mem[0] = 4'b0010;
    mem[1] = 4'b0010;
    mem[2] = 4'b1110;
    mem[3] = 4'b0010;
    mem[4] = 4'b0100;
    mem[5] = 4'b1010;
    mem[6] = 4'b1100;
    mem[7] = 4'b0000;
    mem[8] = 4'b1010;
    mem[9] = 4'b0010;
    mem[10] = 4'b1110;
    mem[11] = 4'b0010;
    mem[12] = 4'b0100;
    mem[13] = 4'b1010;
    mem[14] = 4'b1100;
    mem[15] = 4'b0000;
  end

endmodule
```

### Features
- **Data Width**: 4 bits.
- **Address Width**: 4 bits, allowing for 16 memory locations.
- **Synchronous Operation**: All read operations are synchronized with the clock signal.
- **Predefined Content**: The memory is initialized with predefined values in the `initial` block.

### Simulation and Testing

To test the ROM module, a testbench should verify:
- Correct data retrieval based on the input address.
- The behavior of the module when the enable signal (`en`) is low.

## How to Run

1. **Synthesize** the Verilog files using a synthesis tool like Xilinx Vivado, Synopsys Design Compiler, or any other compatible tool.
2. **Simulate** the design using ModelSim, Vivado Simulator, or another Verilog simulation tool. Create testbenches to verify the functionality of both the Dual Port RAM and ROM modules.
3. **Verify** that all operations (read/write for RAM, read for ROM) perform as expected.
