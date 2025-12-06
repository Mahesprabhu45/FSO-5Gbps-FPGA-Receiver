# FPGA-Based 5 Gbps Free-Space Optical Communication Receiver

This repository documents the design and FPGA implementation of a high-speed receiver architecture for a **5 Gbps free-space optical (FSO) communication link**. The system is engineered to operate under realistic atmospheric conditions including **scintillation, fading, and burst error statistics**, ensuring reliable data recovery in both laboratory and field environments.

The design integrates high-speed sampling, robust digital synchronization, adaptive decision logic, and forward error correction to deliver a **final post-FEC throughput of 4.686 Gbps with BER < 10⁻¹²**.

---

## System Summary

| Parameter | Specification |
|----------|--------------|
| Modulation Format | NRZ On-Off Keying (OOK) |
| Optical Wavelength | 1550 nm |
| Gross Line Rate | 5 Gbps |
| Net Payload Rate | 4.686 Gbps (after RS(255,239) FEC) |
| ADC Sampling | 20 GS/s, 8-bit resolution |
| FPGA Platform | Xilinx Kintex UltraScale+ |
| Target BER | < 10⁻¹² (post-FEC) |
| Fade Tolerance | 10–50 ms atmospheric fade duration |

---

## Architecture Overview

The signal chain is partitioned into five major functional blocks:

1. **High-Speed Data Acquisition**
   - InGaAs APD + TIA analog front-end
   - 20 GS/s ADC (JESD204C interface)

2. **Clock and Data Recovery (CDR)**
   - Dual-loop timing recovery
   - Fade-aware gated phase detector and holdover state machine

3. **Adaptive Threshold Detection**
   - Decision-directed dual-IIR algorithm
   - 1 kHz threshold update aligned to atmospheric coherence time

4. **Error Protection and Framing**
   - Convolutional interleaver for burst decorrelation (5 ms turbulence model)
   - Reed-Solomon RS(255,239) decoding via Xilinx LogiCORE IP

5. **Output Processing**
   - Frame synchronization, CRC validation, and buffered high-rate data output

---

## Block Diagram
Optical Signal → APD/TIA → ADC (20 GS/s)
│
▼
Clock/Data Recovery → Adaptive Threshold → Bit Decisions
│
▼
Framing → Interleaver → RS Decoder → CRC → Output Payload (4.686 Gbps)

---

## FPGA Resource Summary

(Representative synthesis on XCKU5P device)

| Component | Estimated Utilization |
|----------|------------------------|
| LUTs | ~11k (≈3%) |
| Flip-Flops | ~9k |
| DSP48E2 Slices | 20–24 |
| BRAM (36 Kb) | 5–8 |
| End-to-End Latency | < 10 µs (excluding interleaver depth) |

---

## Features

- Designed for **high-throughput optical communication systems**
- Resilient to **atmospheric turbulence and beam power fluctuations**
- Low-latency architecture compatible with **real-time applications**
- Modular RTL implementation suitable for FPGA/ASIC migration

---

## Repository Contents

## References

- ITU-T G.709 Optical Transport Network Standard  
- CCSDS Coding and Synchronization Recommendations  
- NASA LLCD and DLR Optical Downlink Demonstrations  
- Andrews, L.C., *Laser Beam Propagation Through Random Media*


