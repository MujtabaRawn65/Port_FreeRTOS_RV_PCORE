# **Port_FreeRTOS_RV_PCORE**

## **Objective**

The Port_FreeRTOS_RV_PCORE project is designed to set up and run the FreeRTOS operating system on RISC-V cores, specifically demonstrated on the UET RV PCORE platform. The project showcases the capability of FreeRTOS in managing real-time tasks on RISC-V architecture, highlighting the efficiency of the system through a simple UART communication example.

---

## **Prerequisites**

To build and run the project, ensure that you have the following tools and software installed:

- **GNU RISC-V Toolchain** (tested on Crosstool-NG)
- **Verilator**: A tool for compiling Verilog code into C++ models, used for simulation.
  - To install Verilator, follow the instructions from [Verilator Installation Guide](https://verilator.org/).
  
- **GTKWave**: A waveform viewer used to visualize simulation outputs, such as VCD files.
  - Install GTKWave with the following command (for Ubuntu-based systems):
    ```bash
    sudo apt-get install gtkwave
    ```
---

## **How to Build Toolchain**

Follow the steps below to build the **GNU RISC-V Toolchain** using Crosstool-NG:

1. **Clone the Crosstool-NG repository**:
   ```bash
   git clone https://github.com/crosstool-ng/crosstool-ng
   cd crosstool-ng
   ```

2. **Bootstrap and configure Crosstool-NG**:
   ```bash
   ./bootstrap
   ./configure --enable-local
   make
   ```

3. **Generate Toolchain Configuration**:
   Create a `defconfig` file (exactly the same name, with no file extension) with the following content based on your build architecture:

   **For RV32 builds**:
   ```text
   CT_EXPERIMENTAL=y
   CT_ARCH_RISCV=y
   CT_ARCH_64=n
   CT_ARCH_ARCH="rv32ima"
   CT_ARCH_ABI="ilp32"
   CT_TARGET_CFLAGS="-mcmodel=medany"
   CT_TARGET_LDFLAGS="-mcmodel=medany"
   CT_MULTILIB=y
   CT_DEBUG_GDB=y
   ```

   **For RV64 builds**:
   ```text
   CT_EXPERIMENTAL=y
   CT_ARCH_RISCV=y
   CT_ARCH_64=y
   CT_ARCH_ARCH="rv64ima"
   CT_ARCH_ABI="lp64"
   CT_TARGET_CFLAGS="-mcmodel=medany"
   CT_TARGET_LDFLAGS="-mcmodel=medany"
   CT_MULTILIB=y
   CT_DEBUG_GDB=y
   ```

4. **Run the command to save configurations**:
   ```bash
   ./ct-ng defconfig
   ```

5. **Build the GNU Toolchain**:
   ```bash
   ./ct-ng build
   ```

   This will install the toolchain to `~/x-tools/riscv32-unknown-elf` or `~/x-tools/riscv64-unknown-elf` depending on your build.

---

## **Building the Project**

1. **Add the path to your RISC-V toolchain**:
   ```bash
   export PATH=~/x-tools/{YOUR_TOOLCHAIN}/bin:$PATH
   ```

2. **Build the project using `make`**:
   Simply run:
   ```bash
   make
   ```

   If you want to build in debug mode, pass `DEBUG=1`. For an RV64 build, use `XLEN=64`:
   ```bash
   make DEBUG=1
   make XLEN=64
   ```

3. **Resulting Executable**:
   The resulting executable file will be located at `./build/RTOSDemo.axf`.
   
4. **Converting `.afx` to `.hex`**:

    After building the project, you can convert the `.afx` binary file to a `.hex` file for further use:
   
      ```bash
      riscv32-unknown-elf-objcopy -O binary RTOSDemo.axf RTOSDemo.bin
      hexdump -ve '1/4 "%08x\n"' RTOSDemo.bin > RTOSDemo.hex
      ```
   ---

## **Running FreeRTOS Demo on RISC-V Core**

Once the binary is generated, follow these steps to run the project on UETRV-Pcore:

**Copy** the contents of `Demo/build/RTOSDemo.hex` to the required directory as `hello.hex`:
```bash
cp Demo/build/RTOSDemo.hex UETRV-Pcore/sdk/example-uart/build/hello.hex
```

---

## **Running the Simulation**

To simulate the project, you need to run the simulation tools:

1. **Run the simulation with UART**:
   ```bash
   cd UETRV-Pcore
   sim-verilate uart
   ```

2. **Enable VCD generation**:
   To generate a VCD file for waveform analysis, use the following command:
   ```bash
   sim-verilate uart vcd=1
   ```
   The output will be generated as `trace.vcd` file in main folder.
   
4. **Cycle Count Configuration**:
   If you wish to modify the number of cycles during simulation, edit the `Makefile` or pass the desired value with the `max_cycles` flag:
   ```bash
   make max_cycles=<desired_number_of_cycles>
   ```

---

## **Viewing the Output**

After running the simulation, the output will be available in the `uartlog` file in the same folder. You can check the content of this log file to see the required UART output.

For a successful demo, you should expect output similar to the following in the `uartlog`:

---

## **Notes for Modifications**

- To change the behavior of the tasks (such as altering the FreeRTOS tasks or their interactions), modify the `main.c` or `blinky.c` file.
- After making changes, rebuild the project using the `make` command to regenerate the `.afx` file.


