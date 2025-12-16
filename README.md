# Industrial Cybersecurity: DoS Attack Examples

This repository contains educational examples of Denial of Service (DoS) attacks and related security demonstrations targeting industrial control systems (ICS), specifically focusing on Siemens S7 PLCs communicating via Ethernet protocols.

## Overview

The project demonstrates two main attack vectors for disrupting HMI-to-PLC communications:

1. **Session Hijacking Attack** ([DoS_HijackSession.py](./DoS_HijackSession.py)) - Intercepts and exploits an existing valid session between an HMI and PLC
2. **New Session Establishment** ([DoS_NewSession.py](./DoS_NewSession.py)) - Establishes a malicious session between devices to disrupt operations
3. **PLC Input Processing Unit (IPU) Attacks** - Direct attacks on PLC memory areas using Snap7 library

## Project Structure

### Attack Scripts
- `DoS_HijackSession.py` - Hijacks active HMI-PLC sessions
- `DoS_NewSession.py` - Establishes unauthorized sessions
- `DoS_IPU.py` - Writes random data to PLC input memory areas to cause denial of service
- `read_IPU.py` - Reads PLC input processing unit memory for analysis

### Protocol Implementations
- `s7.py` - Siemens S7 Communication Protocol implementation
- `cotp.py` - Connection-Oriented Transport Protocol (ISO 8073)
- `tpkt.py` - TPKT Protocol (ISO 1006)
- `clnp.py` - ConnectionLess Network Protocol
- `ether.py` - Ethernet layer handling
- `s01fd.py` - Protocol-specific utilities

### Configuration & Data
- `addresses.json` - Network and MAC address configuration (required)

## Prerequisites

### Required Libraries
- **scapy** - Packet manipulation and network engineering
- **snap7** - Siemens S7 PLC communication library

### Installation

```bash
pip install scapy snap7
```

### Network Requirements
- Direct network access to target PLC
- Root/Administrator privileges (for raw packet operations)
- Knowledge of target network topology

## Configuration

Before running any attack script, you must create an `addresses.json` configuration file with the network details:

```json
{
    "srcEth": "00:11:22:33:44:55",
    "dstEth": "aa:bb:cc:dd:ee:ff",
    "srcIP": "192.168.1.100",
    "dstIP": "192.168.1.101"
}
```

**Fields:**
- `srcEth` - Source MAC address (attacker HMI)
- `dstEth` - Destination MAC address (target PLC)
- `srcIP` - Source IP address (attacker HMI)
- `dstIP` - Destination IP address (target PLC)

## Usage Examples

### Session Hijacking Attack
```bash
sudo python3 DoS_HijackSession.py
```
Requires an active, sniffable session between HMI and PLC.

### New Session DoS
```bash
sudo python3 DoS_NewSession.py
```
Establishes a new malicious session without requiring an existing connection.

### PLC Memory Disruption
```bash
python3 DoS_IPU.py <target_plc_ip>
python3 read_IPU.py <target_plc_ip>
```

## Technical Details

### Protocol Stack
The attacks leverage the industrial S7 communication protocol stack:
- **Layer 1:** Ethernet (Physical)
- **Layer 2:** IP (Network)
- **Layer 3:** TCP (Transport)
- **Layer 4:** TPKT (ISO 1006 Protocol)
- **Layer 5:** COTP (ISO 8073 - Connection-Oriented Transport)
- **Layer 6:** S7COMM (Siemens proprietary application protocol)

### Attack Mechanisms
1. **TCP Sequence Hijacking** - Monitors and synchronizes TCP sequence/acknowledgment numbers to inject packets into active sessions
2. **Session Replay** - Captures and replays S7COMM protocol packets to maintain connection state
3. **Input Manipulation** - Directly writes garbage data to PLC input areas, preventing legitimate data updates

## Credits

- **Protocol implementations** derived from [s7scan](https://github.com/klsecservices/s7scan)
- **Author:** Jaime Mohedano
- **License:** MIT (see [LICENSE](./LICENSE) file)

## Disclaimer

⚠️ **EDUCATIONAL PURPOSE ONLY** ⚠️

This code is provided strictly for educational and authorized security research purposes. Unauthorized access to computer systems is illegal. Users assume all responsibility for compliance with applicable laws and regulations. The authors are not liable for misuse or damage resulting from the use of this code.

## License

MIT License © 2023 Jaime Mohedano
