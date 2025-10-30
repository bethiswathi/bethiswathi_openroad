# Overview from Architecture to Hardware

<img width="900" height="450" alt="image" src="https://github.com/user-attachments/assets/1b763af4-1edf-4112-99d9-1782117384fb" /></br>

| **Component**                     | **Description**                                                                                                 |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Applications (Apps)**           | User-level programs that perform specific tasks or functions.                                                   |
| **System Software**               | Acts as a bridge between hardware and applications, managing system resources and providing essential services. |
| **Operating System (OS)**         | Core system software that controls hardware, memory, files, and processes (e.g., Windows, Linux, Android).      |
| **Compiler**                      | Translates high-level programming code (C, C++, Java) into assembly language.                                   |
| **Assembler**                     | Converts assembly language code into machine (binary) code executable by the processor.                         |
| **RTL (Register Transfer Level)** | Describes digital circuit behavior in terms of registers and data transfers.                                    |
| **Hardware**                      | Physical electronic components that execute instructions and perform computations.                              |

<img width="616" height="532" alt="image" src="https://github.com/user-attachments/assets/9add6537-71e2-4714-a1a9-68786a4a62cd" />

## Components Required for ASIC Development

<img width="502" height="500" alt="image" src="https://github.com/user-attachments/assets/7fa7ac11-e902-4889-ac87-abc74b2478cb" /></br>

### 🧩 What Are RTL IPs?

- **RTL IPs (Register Transfer Level Intellectual Properties)** are **pre-designed and pre-verified digital logic blocks**, described at the **RTL (Register Transfer Level)** using **Verilog** or **VHDL**.
- They can be **reused** in different chip designs to save development time and effort.

### ⚙️ Purpose of RTL IPs

- To **speed up chip design** by reusing verified modules instead of creating everything from scratch.
- To **ensure reliability**, since verified IPs have already been tested.
- To **standardize** digital designs using reusable building blocks.

### 🧠 Benefits of Using RTL IPs
| **Benefit**                | **Explanation**                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------- |
| **Reusability**            | One verified IP can be used in multiple projects.                                     |
| **Reduced Time-to-Market** | Designers can focus on system-level design rather than reimplementing common modules. |
| **Reliability**            | IPs are pre-verified and proven in silicon.                                           |
| **Customization**          | Many IPs are parameterized (configurable width, depth, protocol version, etc.).       |
| **Cost Efficiency**        | Reduces design, testing, and verification costs.                                      |

### 🧰 Open-Source RTL IP Examples
| **IP Name**                      | **Description**               | **Source**                                                         |
| -------------------------------- | ----------------------------- | ------------------------------------------------------------------ |
| **PicoRV32**                     | Compact RISC-V CPU core       | [Clifford Wolf – GitHub](https://github.com/cliffordwolf/picorv32) |
| **Ibex**                         | Small 32-bit RISC-V core      | [LowRISC](https://github.com/lowRISC/ibex)                         |
| **Wishbone Bus**                 | Open hardware bus interface   | OpenCores                                                          |
| **UART / SPI / I2C Controllers** | Common communication blocks   | OpenCores, LibreCores                                              |
| **OpenTitan IPs**                | Security, crypto, and SoC IPs | [OpenTitan Project](https://opentitan.org/)                        |

### 🧩 What Is PDK Data?

- **PDK (Process Design Kit) data** refers to all the **information, files, and models** provided by a **semiconductor foundry** that describe **how to design and fabricate ICs (chips)** using their specific process technology (like 130 nm, 65 nm, etc.).
- It acts as a bridge between the design team (who make the chip layout) and the foundry (who manufacture the chip).


### ⚙️ Purpose of PDK Data

- To ensure your design **follows the physical, electrical, and timing rules** of a given process.
- To let **EDA tools** (like Magic, OpenROAD, Yosys) accurately **simulate, verify, and layout** the chip.
- To provide **models** for transistors, interconnects, and devices used in circuit design.

### 🧠 Why PDK Data Is Important
| **Reason**            | **Explanation**                                               |
| --------------------- | ------------------------------------------------------------- |
| **Accuracy**          | Ensures simulations reflect real silicon behavior.            |
| **Manufacturability** | Guarantees your layout follows all physical rules.            |
| **Interoperability**  | Enables use of standard EDA tools and libraries.              |
| **Consistency**       | Keeps all designers working with the same foundry parameters. |

### 🧮 Example: SkyWater 130nm PDK Data
| **File/Folder**                      | **Purpose**                                    |
| ------------------------------------ | ---------------------------------------------- |
| `sky130_fd_sc_hd/`                   | High-density standard cell library             |
| `libs.tech/magic/sky130.tech`        | Magic technology file (defines layers & rules) |
| `libs.tech/ngspice/sky130.lib.spice` | Device models for SPICE simulation             |
| `libs.tech/klayout/sky130.lyp`       | Layer map for KLayout viewer                   |
| `libs.tech/openlane/`                | Configuration files for OpenLane flow          |
| `libs.ref/`                          | References for transistor and cell parameters  |
| `docs/`                              | Documentation and design guidelines            |



## 🧩 What Are EDA Tools?

- **EDA (Electronic Design Automation)** tools are **software programs** used by engineers to **design, verify, and simulate** electronic systems such as **ICs (Integrated Circuits), ASICs**, and **FPGAs**.
- They automate complex design steps that would be impossible or very time-consuming to do manually.

## ⚙️ Purpose of EDA Tools

- To design **digital and analog** circuits efficiently.
- To simulate and verify **functionality, timing, and layout**.
- To prepare the final **GDSII** file for fabrication.

### 🧠 Why EDA Tools Are Important
| **Reason**       | **Explanation**                                     |
| ---------------- | --------------------------------------------------- |
| **Automation**   | Handle millions of transistors efficiently.         |
| **Accuracy**     | Provide realistic simulation and layout validation. |
| **Optimization** | Improve area, power, and performance.               |
| **Verification** | Ensure the chip works correctly before fabrication. |
| **Productivity** | Greatly reduce design time and human error.         |

### 🧰 Popular Open-Source EDA Toolchain Example

The OpenLane flow integrates several open-source tools:
```
Yosys  →  OpenROAD  →  Magic  →  Netgen  →  KLayout
```

Used with SkyWater 130nm PDK, it enables fully open-source ASIC design.


## Overview of RTL to GDS Flow

<img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/4a9c744d-402d-455e-90c3-e26290bffb93" /></br>


| **Step**   | **Stage**                                       | **Description**                                                                                                                                                                                                                     |
| ---------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1️⃣**    | **Architectural Design**                        | Define system specifications and microarchitecture based on physical and performance constraints.                                                                                                                                   |
| **2️⃣**    | **RTL Design / Behavioral Modeling**            | Describe circuit behavior using HDL (Verilog/VHDL). Includes: <br>• **RTL Design:** Uses registers, combinational logic, and modules (IPs). <br>• **Behavioral Modeling:** Describes system behavior at a higher abstraction level. |
| **3️⃣**    | **RTL Verification**                            | Functionally verify that the RTL meets design specifications using simulation and testbenches.                                                                                                                                      |
| **4️⃣**    | **DFT (Design for Test) Insertion**             | Insert test structures (like scan chains) to allow post-fabrication testing.                                                                                                                                                        |
| **5️⃣**    | **Logic Synthesis**                             | Convert RTL to a gate-level netlist using EDA tools. <br>• **GTECH Mapping:** Map HDL to generic gates and optimize logic. <br>• **Technology Mapping:** Map optimized netlist to standard cells from the PDK.                      |
| **6️⃣**    | **Standard Cells**                              | Pre-designed logic blocks (NAND, NOR, FFs, etc.) with defined physical and timing characteristics.                                                                                                                                  |
| **7️⃣**    | **Post-Synthesis STA (Static Timing Analysis)** | Verify timing (setup and hold) to ensure correct operation at the desired frequency.                                                                                                                                                |
| **8️⃣**    | **Floorplanning**                               | Define chip layout, macro placement, and power distribution network (PDN). Create power rings and straps to reduce IR drop and EM issues.                                                                                           |
| **9️⃣**    | **Placement**                                   | Position standard cells on the floorplan. <br>• **Global Placement:** Optimizes overall position. <br>• **Detailed Placement:** Legalizes positions (no overlaps).                                                                  |
| **🔟**     | **CTS (Clock Tree Synthesis)**                  | Build a clock network (often H-tree) to deliver low-skew clock signals to all sequential elements.                                                                                                                                  |
| **1️⃣1️⃣** | **Routing**                                     | Connect all cells, macros, and pins using metal layers while ensuring DRC compliance.                                                                                                                                               |


























