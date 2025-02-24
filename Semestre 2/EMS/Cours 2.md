
# EMS - ES Platforms 

---

## Digital Computer Building Blocks
- **Basic Components**:
  - **Combinational Logic**: AND, NAND, OR, NOR, NOT, XOR, XNOR gates.
  - **Memory Elements**: SRAM/DRAM cells, bit/word lines, transistor-capacitor storage.

---

## Application-Specific Integrated Circuits (ASICs)
### Pros & Cons
| **Pros**                   | **Cons**                     |
|----------------------------|------------------------------|
| High performance/power     | Design cost > $2M            |
| Low unit cost (mass production) | Long development cycles (months) |
| Extreme integration         | Requires specialized skills  |

### Design Flow
1. **System Specification**
2. **Logic/Circuit Design**
3. **Physical Design & Verification**
4. **Fabrication & Testing**
- **Example**: Bitcoin miners (SHA256 acceleration).

### Building Blocks
- **Standard Cells**: Predefined logic primitives (AND, OR, flip-flops).
- **Process Development Kit (PDK)**: Includes design rules, simulation models (e.g., SKY130).

---

## Field-Programmable Gate Arrays (FPGAs)
### Architecture
- **Configurable Logic Blocks (CLBs)**: Built from LUTs (Look-Up Tables) and flip-flops.
- **Interconnects**: Reconfigurable routing between blocks.
- **Example Devices**:
  - Small: ICE40LP (1280 LUTs).
  - Large: Xilinx XCVU13P (3.78M LUTs).

### Design Flow
- **Hardware Description Languages (HDLs)**: Verilog/VHDL.
  ```verilog
  module upcount (input clk, clear, output [1:0] Q);
    reg [1:0] _Q;
    always @(posedge clk) begin
      if (clear) _Q <= 2'b00;
      else _Q <= _Q + 1;
    end
    assign Q = _Q;
  endmodule
  ```

### Additional Resources
- DSP blocks, BRAM (Block RAM), clock managers, specialized IP cores.

---

## System-on-Chips (SoCs)
### Key Features
- **Integration**: CPU, GPU, memory, accelerators, I/O on one chip.
- **Examples**:
  - **Qualcomm Snapdragon**: ARM Cortex CPUs, cellular/Wi-Fi, mixed-signal design.
  - **TI AM355x**: Low-cost industrial SoC (A8 CPU, LCD controllers).

### Design Challenges
- **Customization**: Software-driven differentiation (e.g., embedded Linux).
- **Multicore Architectures**:
  - **Heterogeneous**: ARM big.LITTLE (high/low-power cores).
  - **FPGA Integration**: Xilinx Zynq (ARM + FPGA fabric).

---

## Microcontrollers (MCUs)
### Characteristics
- **Simplified Cores**: 8/16/32-bit (e.g., 8051, ARM Cortex-M).
- **Low Power**: MSP430 (0.1µA standby, 220µA/MIPS active).
- **Use Cases**: Safety-critical systems (automotive, aerospace).

### Example MCUs
| **MCU**      | Bits | Flash/RAM | Price (10k units) | Features                |
|--------------|------|-----------|-------------------|-------------------------|
| STM8 (ST)    | 8    | 8-64KB    | $0.2-$1          | USART, SPI, 12-bit ADC  |
| MSP430 (TI)  | 16   | 2-256KB   | $0.25-$8         | Ultra-low power, RTC    |

### Programming
- **Bare-metal** or **RTOS** (FreeRTOS, Zephyr).
- No MMU/FPU; predictable timing for real-time tasks.

---

## Trade-Off Summary
| **Platform** | Design Cost | Unit Cost | Flexibility | Power Efficiency |
|--------------|-------------|-----------|-------------|-------------------|
| ASIC         | Very High   | Very Low  | None        | High              |
| FPGA         | Medium      | Medium    | High        | Medium            |
| SoC          | Medium      | Medium    | Medium      | Medium-High       |
| MCU          | Low         | Low       | Low         | Very High         |
