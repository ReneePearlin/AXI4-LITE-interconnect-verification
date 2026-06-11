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

  ---

## AXI4-Lite Channels

AXI4-Lite separates communication into five independent channels. Each channel carries a specific type of information and uses its own VALID/READY handshake mechanism.

### Write Channels

#### AW (Write Address Channel)

Carries the address where data should be written.

Signals:

* AWADDR
* AWVALID
* AWREADY

#### W (Write Data Channel)

Carries the data that will be written to the destination address.

Signals:

* WDATA
* WVALID
* WREADY

#### B (Write Response Channel)

Carries the completion status of a write transaction.

Signals:

* BRESP
* BVALID
* BREADY

### Read Channels

#### AR (Read Address Channel)

Carries the address from which data should be read.

Signals:

* ARADDR
* ARVALID
* ARREADY

#### R (Read Data Channel)

Carries the data returned by the slave.

Signals:

* RDATA
* RRESP
* RVALID
* RREADY

---

## Separation of Address and Data

One of the key architectural features of AXI4-Lite is that addresses and data travel on separate channels.

Advantages:

* Independent operation of address and data paths
* Improved system flexibility
* Reduced communication bottlenecks
* Better scalability for larger systems

Because address and data are independent, an address can be accepted even if the receiver is not yet ready for the data.

---

## Write Transaction

A write transaction transfers data from a master to a slave.

### Step 1

Master sends the destination address using the AW channel.

### Step 2

Slave accepts the address through an AW handshake.

### Step 3

Master sends write data using the W channel.

### Step 4

Slave accepts the data through a W handshake.

### Step 5

Slave generates a write response using the B channel.

### Step 6

Master accepts the response.

A write transaction is complete only after:

* AW Handshake
* W Handshake
* B Handshake

have all successfully occurred.

---

## Read Transaction

A read transaction retrieves data from a slave.

### Step 1

Master sends the desired address using the AR channel.

### Step 2

Slave accepts the address through an AR handshake.

### Step 3

Slave accesses the requested data.

### Step 4

Slave returns the data using the R channel.

### Step 5

Master accepts the returned data.

A read transaction is complete only after:

* AR Handshake
* R Handshake

have successfully occurred.

---

## AXI4-Lite Channel Directions

### Master to Slave

* AW Channel
* W Channel
* AR Channel

### Slave to Master

* B Channel
* R Channel

This separation allows requests and responses to travel in different directions simultaneously.

---

## Interconnect

An interconnect is a hardware block that connects masters and slaves within a system.

Responsibilities of an interconnect include:

* Receiving transactions from masters
* Decoding addresses
* Selecting destination slaves
* Routing transactions
* Returning responses to masters
* Handling invalid addresses

The interconnect acts as the communication manager of the system.

---

## Slave Selection

When a transaction enters the interconnect, the address is examined to determine which slave owns the requested address range.

Example:

Slave0 : 0x0000 – 0x0FFF

Slave1 : 0x1000 – 0x1FFF

Slave2 : 0x2000 – 0x2FFF

Address:

0x1540

Selected Slave:

Slave1

The selected slave receives the transaction while all other slaves ignore it.

---

## Address Decoder

The address decoder is a combinational logic block inside the interconnect.

Purpose:

* Accept an address as input
* Determine the matching slave
* Generate slave selection signals

Example:

0x0540 → Slave0

0x1540 → Slave1

0x2540 → Slave2

Invalid addresses produce an error condition.

---

## Error Responses

If an address does not belong to any valid slave address range, the interconnect generates an error response.

Common AXI4-Lite response:

SLVERR

Error responses prevent transactions from being routed to undefined hardware locations.

---

## Combinational Logic

Combinational logic produces outputs based only on current inputs.

Characteristics:

* No memory
* No stored state
* Immediate response to input changes

Example:

Address Decoder

Address → Decoder → Slave Select

The output depends entirely on the current address value.

---

## Sequential Logic

Sequential logic contains storage elements that preserve information across clock cycles.

Characteristics:

* Contains memory
* Maintains state
* Depends on clocked storage elements

Examples:

* Registers
* Counters
* State Machines

Sequential logic is required whenever information must be remembered for future cycles.

---

## State

State refers to information stored by hardware that persists across clock cycles.

Examples:

* Selected slave information
* Current transaction status
* FSM states

Without state, hardware cannot remember previous events.

---

## Registers

Registers are clocked storage elements used to retain information.

In an interconnect, registers are used to store transaction-related information that must remain available after the original signals have disappeared.

Example:

slave_sel_reg

Purpose:

Store the selected slave associated with an active transaction.

---

## Need for Transaction Storage

Address and data channels are independent.

As a result:

* Address may arrive first
* Data may arrive several cycles later

The interconnect must remember which slave was selected when the address was decoded.

This requires a register to store the selection information until the transaction completes.

---

## State Encoding

State information is commonly represented using binary values.

Example:

00 → Slave0

01 → Slave1

10 → Slave2

11 → Invalid

This encoded value can be stored inside a register and used later for routing decisions.

---

## Register Width Calculation

The number of bits required for a register depends on the number of unique states that must be represented.

Formula:

Required Bits = ceil(log₂(Number of States))

Examples:

2 States → 1 Bit

4 States → 2 Bits

8 States → 3 Bits

9 States → 4 Bits

This concept is widely used when designing decoders, multiplexers, state machines, and interconnect selection logic.


