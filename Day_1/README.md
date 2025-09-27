#  Day 1: Introduction to Verilog RTL Design & Synthesis

This covers learning Verilog, open-source simulation with **Icarus Verilog (iverilog)**, and the basics of logic synthesis using **Yosys**. This guide will walk you through practical labs, essential concepts, and insightful explanations to help you build a strong foundation in RTL design.

---
## 1. What is a Simulator, Design, and Testbench?

###  Simulator

A **simulator** is a software tool that mimics the behavior of your hardware design so you can verify its logic without physically implementing it (e.g., ModelSim, Icarus Verilog).

###  Design

The **design** is actual hardware description you wrote (in Verilog/VHDL) that implements the intended circuit or functionality.

###  Testbench

A **testbench** is a separate Verilog/VHDL file that applies inputs and checks outputs of the design in the simulator to verify its correctness.

<div align="center">
  <img src="https://github.com/aneeshbankapur/Introduction-to-tools-for-Verilog-RTL-Design-Synthesis/blob/2457400e2fdac01844ec6d1d27e12d280917e3ea/Day_1/Digital%20Design%20Verification%20Workflow.png" alt="GTKWave Example" width="70%">
</div>

---

## 2. Getting Started with iverilog

**iverilog** is an open-source simulator for Verilog. Here’s the typical simulation flow:

<div align="center">
  <img src="https://github.com/aneeshbankapur/Introduction-to-tools-for-Verilog-RTL-Design-Synthesis/blob/16a9b52654822bcf5b67c9eea68427bdd053b120/Day_1/iverilog%20block%20diagram.png" alt="iverilog Simulation Flow" width="70%">
</div>

- Both the design and testbench are provided as input to iverilog.
- The simulator produces a `.vcd` file for waveform viewing in GTKWave.
  
  ---

## 3. Lab: Simulating a 2-to-1 Multiplexer

Let’s simulate a simple **2-to-1 multiplexer** using iverilog!
###  Step 1: Clone the Workshop Repository

```shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

###  Step 2: Install Required Tools

```shell
sudo apt install iverilog
sudo apt install gtkwave
```

###  Step 3: Simulate the Design

Compile the design and testbench:

```shell
iverilog good_mux.v tb_good_mux.v
```

Run the simulation:

```shell
./a.out
```

View the waveform:

```shell
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img src="https://github.com/aneeshbankapur/Introduction-to-tools-for-Verilog-RTL-Design-Synthesis/blob/16a9b52654822bcf5b67c9eea68427bdd053b120/Day_1/gtkwave_sim.png" alt="GTKWave Example" width="70%">
</div>

---
## 4. Verilog Code Analysis

**The code for the multiplexer (`good_mux.v`):**

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

###  **How It Works**

- **Inputs:** `i0`, `i1` (data), `sel` (select line)
- **Output:** `y` (registered output)
- **Logic:** If `sel` is 1, `y` gets `i1`; if `sel` is 0, `y` gets `i0`.

---

## 5. Introduction to Yosys & Gate Libraries

###  What is Yosys?

**Yosys** is an open-source framework for Verilog Register Transfer Level (RTL) synthesis. It converts digital circuit designs written in hardware description languages into a gate-level netlist.

#### Yosys Features

- **Synthesis:** Converts HDL to a logic circuit
- **Optimization:** Improves speed or area
- **Technology Mapping:** Matches logic to actual hardware cells
- **Verification:** Checks correctness
- **Extensibility:** Supports custom flows

###  Why Do Libraries Have Different Gate "Flavors"?

A `.lib` file contains many versions of each gate (like AND, OR, NOT) with different properties:

- **Performance:** Faster gates for critical paths, slower for power savings
- **Power:** Some gates use less energy
- **Area:** Smaller gates for compact chips
- **Drive Strength:** Stronger gates to drive more load
- **Signal Integrity:** Specialized gates for noise/performance
- **Mapping:** Synthesis tools pick the best flavor for your needs

---

## 6. Synthesis Lab with Yosys

Let’s synthesize the `good_mux` design using Yosys!

###  Step-by-Step Yosys Flow

1. **Start Yosys**
    ```shell
    yosys
    ```

2. **Read the liberty library**
    ```shell
    read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

3. **Read the Verilog code**
    ```shell
    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
    ```

4. **Synthesize the design**
    ```shell
    synth -top good_mux
    ```

5. **Technology mapping**
    ```shell
    abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

6. **Visualize the gate-level netlist**
    ```shell
    show
    ```

<div align="center">
  <img src="https://github.com/aneeshbankapur/Introduction-to-tools-for-Verilog-RTL-Design-Synthesis/blob/16a9b52654822bcf5b67c9eea68427bdd053b120/Day_1/yosys_sim.png" alt="Yosys Gate-level Schematic" width="70%">
</div>

---
## 7. Summary

- You learned about simulators, designs, and testbenches.
- You ran your first Verilog simulation with iverilog and visualized waveforms.
- You analyzed the 2-to-1 mux code.
- You explored Yosys and learned why gate libraries have various flavors.


---
