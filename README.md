# AI Vector Accelerator Organisation README

This README contains an introduction to the project, a description of each of the repositories, and a guide to getting TinyMLPerf running on the final hardware.

## Introduction
This organisation presents a collection of RTL and software which, when combined, forms an integrated TinyML platform.
The CV32E40P, an open-source RISC-V core produced by the OpenHW Group, has been augmented with support for a subset of the RISC-V Vector (RVV) ISA via a dedicated accelerator.
A port of TensorFlow Lite for Microcontrollers (TFL Micro) with support for the added RVV instructions has been produced.
A port of TinyMLPerf has shown that the accelerated core produces a 5.4x speedup in modern ML applications over the standard CV32E40P.

This project was completed over 10 weeks by 6 Fourth Year Electronic Engineering students from the University of Southampton for Embecosm Ltd.
We hope it serves an an inspiration to other small teams looking to produce dedicated embedded hardware for machine learning applications.

**Disclaimer:** This organisation was created as part of a University project.
The owners make no guarantees that it will be supported in the future or that any issues or pull requests submitted will be resolved as the project has now finished.

## Repositories

Below is a brief description of each repository.

### riscv-toolchain-setup-tests

Contains scripts to automatically build and configure the GNU RISC-V toolchain for both the standard and accelerated versions of the CV32E40P.

Also contains simple C/C++ programs to verify the correct configuration of the toolchain, Spike, and the CV32E40P Verilator model.

### NN_software

Contains implementations of the NN operations most prevalent in TensorFlow networks in C/C++ and RVV assembly.
Used to generate the object files needed to give TFL Micro support for the RVV instructions.

### cv32e40p

A fork of the OpenHW Group CV32E40P with modifications allowing more flexible integration with an accelerator/co-processor through the APU interface.

### ava-core

A lightweight core designed to integrate to the CV32E40P through the APU interface.
Implements a subsection of the RVV ISA with 32 32-bit vector registers and 4 processing elements.

### core-v-verif-ava

A fork of the OpenHW Group core-v-verif repository supporting the verification of the ava-core.
Can be used to construct a verilator model of the integrated CV32E40P and AVA Core which can be used to run arbitrary software.

### tensorflow

A fork of Google's TensorFlow repository.
Contains a port of the reference implementation of TFL Micro for the CV32E40P, and an optimised port supporting the RVV instructions implemented in the AVA Core.

### tinymlperf

A fork of the TinyMLPerf suite of Benchmarks for embedded systems under development by the MLCommons group.
It is configured to build TFL Micro from the repository above, allowing the performance of both the standard and accelerated CV32E40P to be measured.

## Getting Started

The steps below should guide you to the point of being able to run TinyMLPerf on the accelerated CV32E40P:

1. Follow instructions in the repo riscv-toolchain-setup-tests repository to install the GNU RISC-V toolchain, Spike, and Verilator.
2. Use the core-v-verif-ava repo to build the verilator model for the accelerated CV32E40P.
3. Ensure that you can run the test programs found in the riscv-toolchain-setup-tests repo both on Spike and on the verilator model.
4. Follow the instructions in the tinymlperf repo to build TinyMLPerf for the standard and accelerated versions of Spike and the CV32E40P.
5. Run the binaries on Spike, then on the verilator model and note the cycle counts reported in the printouts.

