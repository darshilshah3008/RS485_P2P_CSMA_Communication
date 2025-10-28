# RS-485 Peer-to-Peer Communication with CSMA

This project implements a **multi-node peer-to-peer (P2P) communication network** using a **2-wire RS-485 bus**, running a **carrier-sense multiple access (CSMA) protocol**. Each node can transmit and receive application-level messages, control devices, and send acknowledgments, while intelligently handling bus collisions using randomized exponential back-off.

---

## ✅ Features

- Two-wire RS-485 shared bus
- CSMA (carrier-sense multiple access) protocol
- Peer-to-peer message transmission
- Automatic packet retries using exponential back-off
- Message ID-based duplicate filtering
- UART text interface for PC command control
- Node addressing (0–254 + broadcast 255)
- Supports control & data commands
- Acknowledgment & address management
- Real-time status + event reporting
- Device control on multiple channels

> The system acknowledges received packets and resends them when acknowledgment is missing.  


---

## 🛠 Hardware Overview

| Component | Function |
|----------|----------|
| **TM4C123GH6PM (ARM Cortex-M4F)** | Primary microcontroller |
| **SN75HVD12 RS-485 transceiver** | Converts TTL ↔ RS-485 differential signals  |
| Power LED, TX LED, RX LED | Activity indication |
| Push button | Input device |
| LEDs / Servos / RGB LED | Node output devices |
| UART to PC terminal | CLI control |

> Only one node drives the bus at a time—DE is enabled only for transmit.  


---

## 🔗 RS-485 Communication Details

- Differential 2-wire link
- Baud rate: **38400**
- Up to **32 nodes** on shared bus
- 120-ohm termination at each end  


Each data unit = 11 bits  
(start bit + data byte + addr/data type + stop bit)  


A message ID + checksum ensures uniqueness and integrity.

---

## 📦 Packet Structure

DST_ADDRESS
SRC_ADDRESS
SEQ_ID
COMMAND
CHANNEL
SIZE
DATA[SIZE]
CHECKSUM



Broadcast address: **255**

---

## 📟 Supported Commands (Examples)

| Cmd | Function |
|-----|----------|
| Set | Update channel value |
| Pulse | Timed waveform output |
| Sawtooth / Triangle | Pattern output |
| Data Request | Request sensor / device read |
| Data Report | Report value |
| LCD Text | Display |
| RGB control | Lighting output |
| UART forwarding | Data bridging |
| Set Address | Change node address |
| Poll Request/Response | Node info |
| Reset | Reboot node |


---

## ⚙️ CSMA + Back-off

To prevent collisions:
1. Node senses bus before sending  
2. If busy → wait
3. On missing ACK → exponential back-off retry
4. Optional random delay for collision spreading  


Retransmissions are capped; failure is reported to PC.

---

## 🖥 PC Text Interface

Via UART terminal (115200-8-N-1), user can:

reset A
cs ON/OFF
random ON/OFF
set A,C,V
get A,C
poll
sa A,Anew
ack ON/OFF

yaml
Copy code


Node responds with confirmations such as:
- `Ready`
- `Queuing Msg N`
- `Transmitting Msg N, Attempt M`
- `Error Sending Msg N`



---

## 🔧 Node Behavior

- Controls ≥3 device channels
- Sends async messages on input change
- Stores new address in non-volatile memory  

- TX LED blinks on transmission
- RX LED blinks on receive  

Status changes automatically generate Data Reports.

---

## 🧪 Testing

Nodes are tested on a shared RS-485 bus alongside other nodes and controlled via PC. 

---

## ✅ Summary

This project demonstrates a robust RS-485 communication network with:
- Peer-to-peer message passing
- Bus arbitration via CSMA
- Auto-retry with exponential back-off
- PC-controlled UART CLI
- Routing of control + data commands
- Multi-device support on each node
