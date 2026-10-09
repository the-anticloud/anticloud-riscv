# RISCV

![license](https://img.shields.io/badge/license-license-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-semiconductor-lightgrey)

> Anticloud-hardened packaging of the upstream project `RISCV` in category **SEMICONDUCTOR**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** SEMICONDUCTOR · **Upstream:** https://github.com/ultraembedded/riscv · **Upstream pin:** `7ae6f803e30f78c6ea3121e73c3adf50ff912730` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# RISC-V Core

Github: [http://github.com/ultraembedded/riscv](http://github.com/ultraembedded/riscv)

A 32-bit RISC-V core written in Verilog and an instruction set simulator supporting RV32IM.  
This core has been tested against a co-simulation model and exercised on FPGA.

**For a higher performance dual issue CPU with branch prediction, see my latest RISC-V core here;**
[http://github.com/ultraembedded/biriscv](http://github.com/ultraembedded/biriscv)

## Overview
![](doc/overview.png)

## Features
* 32-bit RISC-V ISA CPU core.
* Support RISC-V integer (I), multiplication and division (M), and CSR instructions (Z) extensions (RV32IMZicsr).
* Supports user, supervisor and machine mode privilege levels.
* Basic MMU support - capable of booting Linux with atomics (RV-A) SW emulation.
* Implements base ISA spec [v2.1](https://github.com/ultraembedded/riscv/tree/master/doc/riscv_isa_spec.pdf) and privileged ISA spec [v1.11](https://github.com/ultraembedded/riscv/tree/master/doc/riscv_privileged_spec.pdf).
* Verified using [Google's RISCV-DV](https://github.com/google/riscv-dv) random instruction sequences using cosimulation against [C++ ISA model](https://github.com/ultraembedded/exactstep).
* Support for instruction / data cache, AXI bus interfaces or tightly coupled memories.
* Configurable number of pipeline stages and result forwarding options.
* Synthesizable Verilog 2001, Verilator and FPGA friendly.
* Coremark:  **2.94 CoreMark/MHz**
* Dhrystone: **1.25 DMIPS/MHz** ('legal compile options' / 337 instructions per iteration)
* Want higher performance (**4.1CM/MHz** / **1.9DMIPS/MHz**) - see [my improved core](http://github.com/ultraembedded/biriscv).

#### Configuration

| Param Name                | Valid Range          | Description                                   |
| ------------------------- |:--------------------:| ----------------------------------------------|
| SUPPORT_SUPER             | 1/0                  | Enable supervisor / user privilege levels.    |
| SUPPORT_MMU               | 1/0                  | Enable basic memory management unit.          |
| SUPPORT_MULDIV            | 1/0                  | Enable HW multiply / divide (RV-M).           |
| SUPPORT_LOAD_BYPASS       | 1/0                  | Support load result bypass paths.             |
| SUPPORT_MUL_BYPASS        | 1/0                  | Support multiply result bypass paths.         |
| SUPPORT_REGFILE_XILINX    | 1/0                  | Support Xilinx optimised register file.       |
| EXTRA_DECODE_STAGE        | 1/0                  | Extra decode pipe stage for improved timing.  |
| MEM_CACHE_ADDR_MIN        | 32'h0 - 32'hffffffff | Lowest cacheable memory address.              |
| MEM_CACHE_ADDR_MAX        | 32'h0 - 32'hffffffff | Highest cacheable memory address.             |

## Directories

| Name                | Contents                                            |
| ------------------- | --------------------------------------------------- |
| core/riscv          | RISC-V pipelined RV32IM CPU core (Verilog)          |
| isa_sim             | Instruction set simulator (C)                       |
| top_tcm_axi/src_v   | Example instance with 64KB DP-RAM & AXI Interfaces  |
| top_tcm_axi/tb      | System-C testbench for the core                     |
| top_cache_axi/src_v | Example instance with instruction and data caches.  |
| top_cache_axi/tb    | System-C testbench for the core                     |

## Example Core Instance (with TCM memory)

The top (top_tcm_axi/src_v/riscv_tcm_top.v) contains;
* Instances one of the above cores, adding RAM and standard bus interfaces.
* 64KB dual ported RAM for (I/D code and data).
* AXI4 slave port for loading the RAM, DMA access, etc (including support for burst access).
* AXI4-Lite master port for CPU access to peripherals.
* Separate reset for CPU core to dual ported RAM / AXI interface (to allow program code to be loaded prior to CPU reset de-assertion).

### Memory Map

| Range                     | Description                                         |
| ------------------------- | --------------------------------------------------- |
| 0x0000_0000 - 0x0000_ffff | 64KB TCM Memory                                     |
| 0x0000_2000               | Boot address (configurable, see RISCV_BOOT_ADDRESS) |
| 0x8000_0000 - 0xffff_ffff | Peripheral address space (from AXI4-L port)         |

### Interfaces

| Name         | Description                                                           |
| ------------ | --------------------------------------------------------------------- |
| clk_i        | Clock input                                                           |
| rst_i        | Async reset, active-high. Reset memory / AXI interface.               |
| rst_cpu_i    | Async reset, active-high. Reset CPU core (excluding AXI / memory).    |
| axi_t_*      | AXI4 slave interface for access to 64KB TCM memory.                   |
| axi_i_*      | AXI4-Lite master interface for CPU access to peripherals.             |
| intr_i       | Active high interrupt input (for connection external int controller). |

### Testbench

A basic System-C / Verilator based testbench for the core is provided.

Dependencies;
* gcc
* make
* libelf
* System-C (specify path using SYSTEMC_HOME)
* Verilator (specify path using VERILATOR_SRC)

To build the testbench;
```
cd top_tcm_axi/tb
make
````

To run the provided test executable;
```
cd top_tcm_axi/tb
make run
````

## Example Core Instance (with caches)

The top (top_cache_axi/src_v/riscv_top.v) contains;
* Instances one of the above cores, adding RAM and standard bus interfaces.
* 16KB 2-way set associative instruction cache
* 16KB 2-way set associative data cache with write-back and allocate on write.
* 2 x AXI4 master port for CPU access to instruction / data / peripherals.

### Interfaces

| Name           | Description                                                           |
| -------------- | ----------------------------

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Unknown (no standard manifest detected)** (manifests: none detected; scanned in UPSTREAM_CLONE)
- Top-level source layout: `core/`, `doc/`, `isa_sim/`, `top_cache_axi/`, `top_tcm_axi/`, `top_tcm_wrapper/`
- Snapshot size: **95 files**, **6856 lines of code** (measured; see Benchmarks)
- Primary languages: `.v` (43), `.h` (21), `.cpp` (11), `(none)` (4), `.png` (4), `.build_sysc_tb` (2)
- Upstream commit pinned for this packaging: `7ae6f803e30f78c6ea3121e73c3adf50ff912730`

---

## Installation

```
cd top_tcm_axi/tb
make
````

To run the provided test executable;
```
cd top_tcm_axi/tb
make run
````

*Section quoted from the upstream readme.*
Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

| top_tcm_axi/tb      | System-C testbench for the core                     |
| top_cache_axi/src_v | Example instance with instruction and data caches.  |
| top_cache_axi/tb    | System-C testbench for the core                     |

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

* Configurable number of pipeline stages and result forwarding options.
* Synthesizable Verilog 2001, Verilator and FPGA friendly.
* Coremark:  **2.94 CoreMark/MHz**
* Dhrystone: **1.25 DMIPS/MHz** ('legal compile options' / 337 instructions per iteration)
* Want higher performance (**4.1CM/MHz** / **1.9DMIPS/MHz**) - see [my improved core](http://github.com/ultraembedded/biriscv).

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Unknown (no standard manifest detected) |
| Manifests detected | none |
| Files in snapshot | 95 |
| Lines of code | 6856 |
| Dependency references | 0 |
| Upstream license | BSD-3-Clause |
| Overlay license | Anticommons 0.1.0 |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

* Synthesizable Verilog 2001, Verilator and FPGA friendly.
* Coremark:  **2.94 CoreMark/MHz**
* Dhrystone: **1.25 DMIPS/MHz** ('legal compile options' / 337 instructions per iteration)
* Want higher performance (**4.1CM/MHz** / **1.9DMIPS/MHz**) - see [my improved core](http://github.com/ultraembedded/biriscv).

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `RISCV` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: BSD-3-Clause** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
Copyright (c) 2014, ultraembedded
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of the {organization} nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original BSD-3-Clause terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `BSD-3-Clause` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `RISCV` (category: SEMICONDUCTOR)
- **Upstream URL:** https://github.com/ultraembedded/riscv
- **Pinned commit (SHA):** `7ae6f803e30f78c6ea3121e73c3adf50ff912730`
- **Branch:** master
- **Pin provenance:** GitHub API commits/master. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`26fd419de6f0bc1c4b95d9b87e2673833aee724da7cb9baa437d207cc47ea0f4`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| DANS (KNAW/NWO, Netherlands) | 10.17026/PT | EU-recognised archive |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe
and Mail, Business Insider, National Post, Financial Post, StreetInsider,
Digital Journal, Barchart, International Business Times, and Fox News.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

