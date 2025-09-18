# Ns-3-simulations

### Language | Idioma
**English** | [Português](README.md)

### Tasks
[Click here to see the tasks](Tasks)

## Network simulator
[Ns-3](https://www.nsnam.org/) is a discrete-event network simulator for Internet systems, targeted primarily for research and educational use. ns-3 is free, open-source software, licensed under the GNU GPLv2 license, and maintained by a worldwide community.

## mmWave: ns-3 Module
- This is an ns-3 module for simulation of 5G cellular networks operating in mmWaves.
- Support for a wide range of channel models, including the 3GPP TR 38.901-based model for frequencies between 0.5 and 100 GHz. Ray tracing and measured traces can also be used.
- Custom PHY and MAC classes that support 3GPP NR frame structure and numerologies.
- Custom schedulers for support of dynamic TDD formats
- Carrier aggregation at the MAC layer
- RLC layer enhancements with packet resegmentation for retransmissions
- Dual connectivity with LTE base stations, with fast secondary cell handover and channel tracking
- Core network element simulation (also with MME as a real node)

For more information about the ns3 module, visit [mmWave-Ns3](https://github.com/nyuwireless-unipd/ns3-mmwave)

## RxPacketTrace
### RxPacketTrace is responsible for monitoring packets transmitted in the simulation. Each column in the file contains specific information about transmitted packets (DL-Downlink) and received packets (UL-Uplink)

- **DL/UL** - Indicates the packet state, whether it's in Downlink or Uplink. Downlink is packet transmission by the base station and Uplink is packet transmission by the user to the base station
- **Time**: The timestamp (in seconds) when the event (packet transmission or reception) occurred.
- **Frame**: The frame number in which transmission or reception occurred. Frames are time units in the LTE/5G temporal structure.
- **subF**: Subframe in which transmission or reception occurred. A frame is divided into subframes.
- **slot**: Slot in the subframe where transmission or reception occurred. Each subframe is divided into slots.
- **1stSym**: First symbol of transmission or reception in the slot.
- **symbol#**: Number of symbols involved in transmission or reception.
- **cellId**: ID of the cell to which the transmission or reception is associated. In LTE/5G networks, each cell has a unique identifier.
- **rnti**: Radio Network Temporary Identifier (RNTI), which is a temporary identifier used to identify user equipment (UE) in the network.
- **ccId**: Component Carrier ID, used in carrier aggregation systems where multiple frequency carriers can be used simultaneously.
- **tbSize**: Transport Block Size, which is the size of the transport block in bits. This value indicates the amount of data transmitted or received.
- **mcs**: Modulation and Coding Scheme, which indicates the modulation and coding scheme used for transmission. MCS is a parameter that influences data transmission rate and robustness against errors.
- **rv**: Redundancy Version, used to identify different redundant versions of the transport block in HARQ (Hybrid Automatic Repeat Request) for error correction.
- **SINR(dB)**: Signal to Interference plus Noise Ratio in decibels. It's a measure of signal quality received by user nodes.
- **corrupt**: Binary indicator (0 or 1) showing whether the packet was corrupted (1) or not (0) during transmission.
- **TBler**: Transport Block Error Rate, which is the transport block error rate. Indicates the proportion of transport blocks that were received with errors.

![image](https://github.com/Hiarleyy/Ns-3-simulations/assets/111695591/2177e459-496d-4d2d-a01d-f03cea067d53)

## DlRLcStats
### The DlRlcStats file is used to analyze the performance of the RLC (Radio Link Control) layer in the downlink (DL - Downlink) in a network simulation performed with ns-3. It contains detailed metrics that allow evaluating and understanding data transmission behavior in the network.

- **% start**: Start of the time interval (in seconds) for recorded statistics. **end**: End of the time interval (in seconds) for recorded statistics.
- **CellId**: Identification of the cell where data was recorded.
- **IMSI**: International Mobile Subscriber Identity of the user.
- **RNTI**: Radio Network Temporary Identifier of the user.
- **LCID**: Logical Channel Identifier, used to differentiate different data flows.
- **nTxPDUs**: Number of transmitted protocol data units (PDUs).
- **TxBytes**: Number of transmitted bytes.
- **nRxPDUs**: Number of received PDUs.
- **RxBytes**: Number of received bytes.
- **delay**: Average delay (in seconds) of PDUs.
- **stdDev**: Standard deviation of PDU delay.
- **min**: Smallest delay value (in seconds) recorded.
- **max**: Largest delay value (in seconds) recorded.
- **PduSize**: Average PDU size (in bytes).
- **stdDev.1**: Standard deviation of PDU size.
- **min.1**: Smallest PDU size recorded (in bytes).
- **max.1**: Largest PDU size recorded (in bytes).

![image](https://github.com/Hiarleyy/Ns-3-simulations/assets/111695591/cd2436ee-cf0b-4368-8434-176ba3ec8eb5)

## Documentation
#### [Batman Algorithm](Documentação/bat_EN.md) || [NS3 C++ Simulation](Documentação/packet5G_EN.md) || [10 Nodes Results](Documentação/10node_results_EN.md) || [Docker NS3-MmWave](Documentação/Docker-MmWave_EN.md)


## Contact

### Marcos Hiarley
<div>
<a href ="mailto:marcoshiarley.silva@gmail.com"><img src ="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href ="https://www.instagram.com/hiarley._/"><img src ="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"></a>
<a href ="https://www.linkedin.com/in/marcos-hiarley/"><img src ="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" href="www.linkedin.com/in/marcos-hiarley-1853a7226"/></a>
</div>


### Robert Gabriel
<div>
<a href ="mailto:robertgabriel@disroot.org"><img src ="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href ="https://www.instagram.com/robertdsgabriel/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"></a>
</div>