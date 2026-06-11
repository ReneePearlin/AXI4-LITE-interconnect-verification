# Project Journal

## Session 1 (09-06-2026)

### Completed
- Project setup
- GitHub repository creation
- Git configuration
- Documentation structure
- SoC concepts
- Bus architecture
- Master and Slave
- Memory-Mapped I/O
- Address Mapping
- Address Decoding
- Invalid Address Handling
- Handshaking
- VALID and READY
- AMBA overview
- Introduction to AXI4-Lite

### Next Session
- AXI4-Lite Channels (AW, W, B, AR, R)
- Write Transaction Flow
- Read Transaction Flow
- Interconnect Architecture

### Status
Theory phase in progress.
RTL design not started.

Session 2 (11-06-2026)

Completed

AXI4-Lite Channels

* Write Address Channel (AW)
* Write Data Channel (W)
* Write Response Channel (B)
* Read Address Channel (AR)
* Read Data Channel (R)

Write Transaction Flow

* AW handshake
* W handshake
* B handshake
* Conditions for write completion

Read Transaction Flow

* AR handshake
* R handshake
* Conditions for read completion

AXI Handshaking Review

* VALID/READY mechanism on all channels
* Channel directions
* Independent channel operation

Address Decoding Review

* Address range mapping
* Slave selection
* Invalid address detection
* Error response concept

Interconnect Fundamentals

* Role of the interconnect
* Transaction routing
* Write path overview
* Read path overview
* Response routing

State and Storage Concepts

* Need for remembering selected slave
* Register-based state storage
* slave_sel_reg concept
* Sequential vs combinational logic

RTL Design Concepts

* Decoder output encoding
* Register width calculation
* State encoding basics
* Invalid state handling

Key Understanding Achieved

* How AXI-Lite transactions move through an interconnect
* Why address and data channels are separated
* How slave selection is performed
* Why internal state is required
* How selected slave information is preserved between cycles

Next Session

Interconnect Architecture Deep Dive

* Complete internal block diagram
* Address decoder architecture
* Write path architecture
* Read path architecture
* Response multiplexer architecture
* Transaction timing examples
* Internal signal flow
* RTL module breakdown

Status

Theory phase nearly complete.
Next session transitions from architecture design toward RTL implementation planning.
RTL coding expected to begin after architecture review.
