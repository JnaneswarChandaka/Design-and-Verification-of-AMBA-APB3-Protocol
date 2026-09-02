# AMBA APB3 Protocol – Verilog Implementation

This project implements the **AMBA APB3 (Advanced Peripheral Bus)** protocol using **Verilog HDL**. APB is a low-cost, low-power interface designed for connecting low-bandwidth peripherals such as UARTs, GPIOs, timers, counters, and control/status registers to a higher-performance system bus.

## Project Overview

The design demonstrates **APB3 master-slave communication** with support for read and write transactions. The APB master generates the required control and address signals, while the APB slave decodes the address and performs register read/write operations.

The implementation follows the standard APB transfer mechanism consisting of **IDLE, SETUP, and ACCESS phases**. The master initiates a transaction by asserting `PSEL`, provides the address and control information during the SETUP phase, and then asserts `PENABLE` during the ACCESS phase.

## APB3 Signals

* `PCLK` – APB clock
* `PRESETn` – Active-low reset
* `PADDR` – Address bus
* `PSEL` – Slave select
* `PENABLE` – Enables the ACCESS phase
* `PWRITE` – Selects read or write operation
* `PWDATA` – Write data
* `PRDATA` – Read data
* `PREADY` – Indicates completion of the transfer
* `PSLVERR` – Indicates an error during the transfer

## Transaction Phases

### 1. IDLE Phase

The bus remains idle when no transfer is taking place. `PSEL` and `PENABLE` are deasserted.

### 2. SETUP Phase

The master asserts `PSEL` and provides the address, write/read control, and write data if required. `PENABLE` remains low.

### 3. ACCESS Phase

The master asserts `PENABLE`. The transfer completes when the selected slave asserts `PREADY`. For a read transaction, the slave provides valid data on `PRDATA`.

## Read and Write Operations

### Write Transaction

1. Master drives the target address on `PADDR`.
2. `PWRITE` is set high.
3. Write data is placed on `PWDATA`.
4. `PSEL` is asserted during SETUP.
5. `PENABLE` is asserted during ACCESS.
6. Slave captures the data when the transfer completes.

### Read Transaction

1. Master drives the target address on `PADDR`.
2. `PWRITE` is set low.
3. `PSEL` is asserted during SETUP.
4. `PENABLE` is asserted during ACCESS.
5. Slave places the requested data on `PRDATA`.
6. The transfer completes when `PREADY` is asserted.

## Design Features

* AMBA APB3-compliant transaction sequence
* Verilog HDL implementation
* Master and slave interface
* Read and write transaction support
* Address decoding
* Peripheral register access
* Synchronous operation using `PCLK`
* Active-low reset
* `PREADY` support for transfer completion
* `PSLVERR` support for error indication
* Simulation and functional verification using a testbench

## Verification

A Verilog testbench is used to verify the APB3 implementation by generating different read and write transactions and checking the corresponding slave responses. The testbench verifies address decoding, register updates, read-back functionality, transaction sequencing, and reset behavior.

## Applications

APB is commonly used for low-bandwidth peripheral interfaces in **SoCs, microcontrollers, ASICs, and FPGA-based systems**. Typical applications include GPIO, UART, timers, watchdogs, interrupt controllers, and configuration/status registers.

## Tools & Technologies

* **Verilog HDL**
* **AMBA APB3**
* **RTL Design**
* **Simulation & Functional Verification**
* **Icarus Verilog / ModelSim**
* **GTKWave**

This project provides practical experience with **AMBA bus protocols, RTL design, finite-state machines, synchronous interfaces, register-mapped peripherals, and hardware verification**.

