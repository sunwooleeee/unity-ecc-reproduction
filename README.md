# Unity ECC Error-Scenario Reproduction

## Overview

This repository contains my reproduction and analysis of selected fault scenarios from the public Unity ECC simulator.

Original implementation:  
https://github.com/scalable-arch/SC_23-Unity-ECC

The goal of this work was to understand how different ECC organizations behave under different memory fault patterns by reproducing representative error scenarios and tracing the relevant fault-injection and decoding paths.

## Motivation

While studying read disturbance and memory reliability, I became interested in how physical memory faults are detected and corrected by ECC mechanisms.

After reading the Unity ECC work, I studied basic ECC mechanisms such as Hamming and Hsiao codes and then used the public Unity ECC simulator to reproduce selected fault scenarios.

The main question explored in this reproduction is:

> How does the effectiveness of an ECC architecture change depending on the type and granularity of memory faults?

## Experimental Setup

The original Unity ECC simulator was used with the following configurations.

### Baseline

- Hsiao SEC-DED On-Die ECC
- AMD Chipkill Rank-Level ECC

### Unity ECC Conservative

- No On-Die ECC
- Unity SSC-DEC Rank-Level ECC
- Conservative correction policy

### Unity ECC Restrained

- No On-Die ECC
- Unity SSC-DEC Rank-Level ECC
- Restrained correction policy

### Reproduction Configuration

For faster reproduction:

- `RUN_NUM`: 1,000,000,000 -> 100,000
- Conservative mode: `CONSERVATIVE_MODE = 1`
- Restrained mode: `CONSERVATIVE_MODE = 0`

No changes were made to the ECC decoding algorithms.

Each configuration was evaluated using 100,000 Monte Carlo iterations.

## Fault Scenarios

Two representative fault scenarios were reproduced.

### 1. DBE + DBE

Double-bit errors are injected into two different chips.

### 2. CHIPKILL + SE

A chip-wide fault is injected into one chip and a single-bit error is injected into another chip.

## Results

| Fault Scenario | Configuration | CE | DUE | SDC |
|---|---|---:|---:|---:|
| DBE + DBE | Baseline | 8.862% | 89.763% | 1.375% |
| DBE + DBE | Unity Conservative | 12.243% | 87.658% | 0.099% |
| DBE + DBE | Unity Restrained | 98.710% | 1.191% | 0.099% |
| CHIPKILL + SE | Baseline | 100.000% | 0.000% | 0.000% |
| CHIPKILL + SE | Unity Conservative | 0.000% | 93.531% | 6.469% |
| CHIPKILL + SE | Unity Restrained | 3.512% | 89.974% | 6.514% |

The raw simulator outputs are available in:

`results/raw/`

A machine-readable summary is available in:

`results/summary.csv`

## Analysis

### DBE + DBE

For the DBE+DBE scenario, the baseline configuration corrects only a small fraction of the injected faults.

Unity ECC reduces the SDC rate, while the restrained policy dramatically increases the CE rate.

The Unity SSC-DEC decoder contains a syndrome table for double-error correction. When a syndrome matches an entry in this table, two erroneous bits located in different chips can be corrected.

In conservative mode, corrections involving multiple chip positions can later be classified as DUE.

Restrained mode disables this additional conservative restriction, allowing substantially more successful corrections to be classified as CE.

### CHIPKILL + SE

The behavior is reversed for the CHIPKILL+SE scenario.

The baseline configuration achieves 100% CE in this experiment.

With the baseline architecture, Hsiao On-Die ECC first corrects the single-bit error. The Rank-Level Chipkill mechanism can then handle the remaining chip-wide fault.

Unity ECC does not use a separate On-Die ECC stage. Therefore, the chip-wide fault and the additional single-bit error reach the Rank-Level ECC together, frequently exceeding the correction capability of the SSC-DEC configuration.

## Key Observation

The results show that ECC effectiveness depends strongly on the fault pattern.

Unity ECC Restrained performs substantially better for the DBE+DBE scenario, while the conventional Hsiao On-Die ECC + Chipkill organization performs better for CHIPKILL+SE.

This suggests that reliability cannot be evaluated only by the nominal correction strength of an ECC code.

The interaction between:

- fault granularity,
- fault combinations,
- ECC correction capability,
- and the organization of ECC across memory-system layers

is an important design consideration.

## My Reproduction Work

My contribution in this repository is the reproduction and analysis of the public Unity ECC implementation.

Specifically, I:

- built and executed the public Unity ECC simulator,
- configured 100,000-run Monte Carlo experiments,
- reproduced DBE+DBE and CHIPKILL+SE fault scenarios,
- compared Baseline, Unity Conservative, and Unity Restrained configurations,
- collected and organized CE, DUE, and SDC results,
- traced the relevant fault-injection and decoder paths,
- and analyzed why the configurations behave differently across fault patterns.

## Attribution

The Unity ECC simulator and ECC implementations were developed by the original authors.

Original repository:  
https://github.com/scalable-arch/SC_23-Unity-ECC

This repository contains my experimental reproduction, result organization, and analysis based on that public implementation.
