# Cross Compilation

Scenario where the host (compiler) and target (executable) are different, for when compilation on target is impossible/impractical. E.g. new hardware/os, low capability targets.

Architectures may vary in memory architecture (von Neumann/Harvard), word size (64/8bit), endianness.

# I/O

## Memory Mapped I/O
- Uses same address space to address main memory and I/O devices.
- *Address space* can either be physical RAM, or memory and registers of the I/O device.
- CPU instructions used to access memory can also be used to access devices.
- Reduces usable address space which can be a concern on 8/16bit architectures.

## Port Mapped I/O
- Uses special class of CPU instructions to perform I/O, e.g. `in` and `out` on x86.
- Uses a separate address space, either with an I/O pin on the CPU's physical interface, or a dedicated I/O bus.

# Interrupt Handling

## Interrupt-Driven I/O


# Event Driven Programming

# Real-time Scheduling

## Rate Monotonic Scheduling

## Earliest Deadline First
- Uses a dynamic change of priority, that runs the most urgent task first. More complex to implement as a scheduler, 100% utilisation possible, with and without preemption.

# FAT (File Allocation Table) File Systems



# Serial Communication
Choosing which interface to use mainly depends on *data rate requirements* and *number of devices connected*.

## SPI (Serial Peripheral Interface) - Synchronous
- Devices communicate in full duplex mode using a master-slave architecture, that uses multiple signal lines, to achieve a relatively high data transfer rate.
- Commonly used in high-speed communication scenarios, e.g. sensors, display modules, memory chips.

## UART (Universal Asynchronous Receiver-Transmitter)
- Devices can communicate with simplex, half duplex, and full duplex. Uses two signal lines to achieve point-to-point communication. Low overhead due to its simplicity.
- Commonly used in scenarios that require simple and straightforward serial communication, e.g. connecting microcontrollers to computers, GPS/Bluetooth modules.

## $I^2C$ (Inter-Integrated Circuit) - Synchronous
- Devices communicate via a multi-master/multi-slave architecture, and uses two signal lines (SDA, SCL), that supports moderate data rates.
- Commonly used in scenarios where multiple devices need to communicate on the same bus, e.g. connecting sensors, EEPROMs, real-time clocks.