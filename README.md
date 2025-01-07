# **Port_FreeRTOS_RV_PCORE**

## **Objective**

The **Port_FreeRTOS_RV_PCORE** project aims to provide a working setup of the FreeRTOS operating system on RISC-V cores, leveraging the power of FreeRTOS for real-time tasks and demonstrating efficient execution on RISC-V hardware using a simple UART communication example.

---

## **Prerequisites**

To build and run the project, ensure that you have the following tools and software installed:

- **RISC-V Toolchain**: The project requires the **RISC-V cross-toolchain** to compile the code for RISC-V architecture. Instructions for installation are included below.
- **Make**: A build system for automating the compilation process.
- **Simulation/Emulation Tools**: Required for running the project on simulators, specifically **sim-verilate**.

---

## **Installation of RISC-V Toolchain**

Follow the steps below to install the **RISC-V cross-toolchain** on your system:

1. **Clone the RISC-V toolchain repository**:
   ```bash
   git clone https://github.com/riscv/riscv-gnu-toolchain.git
   cd riscv-gnu-toolchain
   ```

2. **Install required dependencies** (for Ubuntu-based systems):
   ```bash
   sudo apt-get install autoconf automake bison flex gawk build-essential libgmp3-dev libmpc-dev libmpfr-dev libisl-dev
   ```

3. **Build the toolchain**:
   ```bash
   ./configure --prefix=/path/to/install/directory
   make
   ```

4. **Update environment variables**:
   ```bash
   export PATH=$PATH:/path/to/install/directory/bin
   ```

---

## **Building and Running the Project**

Once the prerequisites are installed, follow these steps to compile and run the project:

1. **Clone the project repository**:
   ```bash
   git clone https://your_project_repository_url.git
   cd Port_FreeRTOS_RV_PCORE
   ```

2. **Build the project using `make`**:
   ```bash
   make
   ```

3. **Output**: The output will be a binary `.afx` file, which contains the compiled application for the RISC-V core.

---

## **Converting `.afx` to `.hex`**

After building the project, you can convert the `.afx` binary file to a `.hex` file for further use:

1. **Convert** `.afx` to `.hex`:
   ```bash
   riscv32-unknown-elf-objcopy -O ihex build/main.afx build/main.hex
   ```

2. **Rename** the `.hex` file to `hello.hex`:
   ```bash
   mv build/main.hex hello.hex
   ```

3. **Copy** the `hello.hex` file to the required directory:
   ```bash
   cp hello.hex /path/to/uetrvpcore/sdk/example-uart/src/
   ```

---

## **Running the Simulation**

To simulate the project, you need to run the simulation tools:

1. **Run the simulation with UART**:
   ```bash
   sim-verilate uart
   ```

2. **Enable VCD generation**:
   To generate a VCD (Value Change Dump) file for waveform analysis, use the following command:
   ```bash
   sim-verilate uart vcd=1
   ```

3. **Cycle Count Configuration**:
   If you wish to modify the number of cycles during simulation, edit the `Makefile` or pass the desired value with the `max_cycles` flag:
   ```bash
   make max_cycles=<desired_number_of_cycles>
   ```

---

## **Viewing the Output**

After running the simulation, the output will be available in the `uartlog` file. You can check the content of this log file to see the required UART output.

For a successful demo, you should expect output similar to the following in the `uartlog`:


## **Notes for Modifications**

- To change the behavior of the tasks (such as altering the FreeRTOS tasks or their interactions), modify the `main.c` or `blinky.c` file.
- After making changes, rebuild the project using the `make` command to regenerate the `.afx` file.


