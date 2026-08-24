# ECC Prerequisites for Understanding Unity ECC

This directory collects the ECC exercises that are most directly useful for understanding the Unity ECC reproduction in this repository.

The original exercise implementations are **not duplicated here**. The exercises come from the public `scalable-arch/ECC-exercise` repository and are linked below. This directory is a curated study path that explains why each exercise matters for Unity ECC and what to verify while working through it.

Original exercise repository:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic

## Recommended order

| Step | Exercise | Why it matters for Unity ECC |
|---|---|---|
| 1 | [Hamming SEC](01_hamming_sec.md) | Builds the syndrome/H-matrix model used throughout linear ECC decoding. |
| 2 | [Hsiao SEC-DED](02_hsiao_sec_ded.md) | Directly connects to the Hsiao On-Die ECC used in the baseline configuration. |
| 3 | [H/G Matrix Transformation](03_h_g_matrix.md) | Connects parity-check logic to encoding and systematic-code structure. |
| 4 | [GF(256) + RS Single-Symbol Correction](04_gf256_rs_ssc.md) | Builds the symbol-level view needed to understand Chipkill-style protection and SSC. |
| 5 | [RS Multi-Symbol Correction (optional)](05_rs_multi_symbol_optional.md) | Gives broader context for stronger symbol-level ECC; useful but not required to follow the reproduced Unity scenarios. |

## Scope

The goal is not to complete every exercise in `01_Basic`. CRC and unrelated code constructions are omitted because they are not necessary for the specific Unity ECC experiments reproduced here.

The key conceptual progression is:

`H-matrix / syndrome -> SEC -> SEC-DED -> systematic encoding -> GF(2^8) symbols -> symbol correction -> Chipkill / Unity ECC fault behavior`

## Attribution

The original exercises and implementations were developed by their respective authors in:

https://github.com/scalable-arch/ECC-exercise

This directory contains only my curation, study notes, and Unity-ECC-specific interpretation of those public exercises.
