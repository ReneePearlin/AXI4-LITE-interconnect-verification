# Concepts and Theory

## System-on-Chip (SoC)

A System-on-Chip (SoC) integrates multiple functional blocks such as CPUs, memory controllers, communication peripherals, timers, and other hardware modules onto a single chip.

Examples:
- CPU
- UART
- GPIO
- Timer
- SPI Controller
- Memory Controller

---

## Why Buses Are Needed

Directly connecting every hardware block to every other hardware block leads to:

- Excessive wiring
- Increased routing complexity
- Larger chip area
- Difficult verification
- Poor scalability

A shared communication bus provides a scalable architecture.

---

## Master and Slave

### Master

A master initiates communication transactions.

Example:
- CPU

### Slave

A slave responds to communication requests.

Examples:
- UART
- GPIO
- Timer

---

## Memory-Mapped I/O

Peripherals are assigned address ranges and appear as memory locations from the CPU's perspective.

Example:

UART  : 0x1000_0000

GPIO  : 0x2000_0000

TIMER : 0x3000_0000

The CPU accesses peripherals by reading and writing to these addresses.

---

## Address Decoding

Address decoding determines which peripheral should receive a transaction based on the address range.

Example:

Address = 0x2000_0000

Selected Peripheral = GPIO

---

## Invalid Address Handling

If an address does not belong to any peripheral, the interconnect should generate an error response instead of routing the transaction.

---

## Handshaking

Reliable communication requires acknowledgment between sender and receiver.

Transfer occurs only when:

VALID = 1

READY = 1

---

## VALID and READY

VALID indicates that the sender has valid information available.

READY indicates that the receiver can accept the information.

A successful transfer occurs only when both signals are asserted simultaneously.

---

## AMBA Overview

AMBA (Advanced Microcontroller Bus Architecture) is a family of on-chip communication protocols developed by Arm.

AMBA includes:

- APB
- AHB
- AXI
- AXI4-Lite

---

## AXI4-Lite

AXI4-Lite is a simplified version of AXI designed for control and configuration interfaces.

Characteristics:

- Simple architecture
- Memory-mapped communication
- Industry-standard protocol