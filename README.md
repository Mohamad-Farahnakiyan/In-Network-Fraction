# In-Network Fraction Approximate Calculation Using the P4 Programming Language

This repository contains the implementation and materials of the paper:

**"محاسبه تقریبی عملیات تقسیم در داخل شبکه به کمک زبان برنامه‌نویسی پی‌فور"**

Accepted at the **7th National Conference on Informatics of Iran (IPM, Tehran)**, 1404.

---

## Authors

Danial Khorasanizadeh, Mohammad Hossein Farahnakiyan, et al.  
Sharif University of Technology

---

## Abstract

The programmable data plane allows the packet processing pipeline to execute customized logic at line rate to enhance security or monitor traffic. However, the limitations of the P4 programming language and its target hardware make the implementation of certain functions, such as division, challenging. Existing methods to overcome this obstacle rely on lookup tables, hardware-dependent floating-point emulation, or fixed-point approximations with specific precision ranges, which limit accuracy and generalizability.
In this study, a fixed-point division algorithm is presented that does not use storage resources and is independent of hardware architecture, designed for the P4-based programmable data plane. The proposed approach significantly expands the supported value range by normalizing division operations, reduces hardware resource usage through linear approximation of fixed-point computations, and enables a tunable trade-off between accuracy and computational overhead by improving the approximation using the Newton–Raphson method.
The algorithm is implemented in P4 and evaluated using the BMv2 behavioral model. Experimental results show that the proposed method achieves the highest level of accuracy compared to existing approaches, with no memory overhead and only a negligible impact of approximately 1540 kilobits per second on throughput. By presenting a generalizable and efficient division method, this work expands the range of numerical computations that can be performed directly in the data plane.

---

## Method Overview

The division is performed in four steps:

1. **Normalization** of the divisor  
   b = 2^k * n

2. **Linear initial approximation**  
   1/n ≈ 1.5 − 0.5n

3. **Newton–Raphson refinement**  
   x_(t+1) = x_t (2 − n x_t)

4. **Denormalization and final multiplication**

The number of Newton iterations can be adjusted to control the trade-off between computational overhead and numerical accuracy.

---

## Implementation

The algorithm is implemented in **P4** and evaluated on **BMv2 (simple-switch)**.

Reproducibility scripts and experiment execution details are provided in the included submodule. The submodule contains the full experimental setup, scripts, and configuration required to reproduce the results.

---

## Experimental Setup

### System Configuration

- **VM**: Based on p4-guide VM (2025-Oct-01 version)
- **Operating System**: Ubuntu 24.04
- **Memory**: 8 GB
- **CPU**: 6 cores, 13th Gen Intel® Core™ i7-13620H

### Software Environment

- **Python**: Version 3.12.3
- **p4c-bm2-ss**: Version 1.2.5.8 (SHA: 2265f8045, BUILD: Release)
- **simple_switch**: Version 1.15.0-68f4a978
- **scapy**: Version 2.5.0

---

## Running Experiments

To run each experiment, execute the corresponding bash script located in the `test_helpers` directory inside the submodule.

```bash
cd <submodule_directory>/test_helpers
./<experiment_script>.sh
```

## Results (Key Metrics)

With four Newton iterations, the proposed method achieved:

- **Mean absolute error:** 1.34 × 10⁻⁴  
- **Memory usage:** 0 bytes  
- **Average computation time:** 591.093 µs  
- **Throughput:** 630.55 kb/s  

The approach matches the accuracy of lookup-table-based methods while eliminating memory overhead.

---

## Repository Structure

- `codes/` – P4 implementation and scripts  
- `figures/` – Paper figures  
- `paper/` – LaTeX source of the paper  
- `references/` – Bibliography materials  

---

## Submodules

This repository uses a git submodule. Clone with:

```bash
git clone --recurse-submodules https://github.com/Mohamad-Farahnakiyan/In-Network-Fraction.git
```

---

## Citation

If you use this work, please cite:

```bibtex
@inproceedings{khorasanizadeh1404innetwork,
  title     = {محاسبه تقریبی عملیات تقسیم در داخل شبکه به کمک زبان برنامه‌نویسی پی‌فور},
  author    = {Khorasanizadeh, Danial and Farahnakiyan, Mohammad Hossein and Dolati, Mehdi and Jalili, Rasool},
  booktitle = {Proceedings of the 7th National Conference on Informatics of Iran},
  year      = {1404},
  address   = {Tehran, Iran},
  organization = {IPM}
}
```
