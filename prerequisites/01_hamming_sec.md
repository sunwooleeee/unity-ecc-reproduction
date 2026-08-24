# 1. Hamming SEC: H-Matrix and Syndrome

Source exercise:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/01_7_4_Hamming_code

## Why this is needed

Unity ECC uses syndrome-based decoding. Before looking at Hsiao, SSC, DEC, or Chipkill behavior, it is necessary to understand the basic relation

`error pattern -> syndrome -> error location`.

The original exercise asks you to construct the H-matrix for a (7,4) Hamming SEC code and verify that every injected single-bit error can be corrected.

## What to understand

For a linear block code,

`syndrome = H * received_word^T` over GF(2).

For a single-bit error at position `i`, the syndrome is the `i`-th column of `H`. Therefore, every bit position must have a unique non-zero H-matrix column if all single-bit errors are to be located uniquely.

## Practice

1. Open the original Hamming exercise.
2. Construct `H_Matrix.txt` yourself before checking the solution.
3. Verify that all 7 columns are non-zero and unique.
4. Run:

```bash
python Hamming_SEC.py
```

5. Confirm the expected behavior:

```text
CE_cnt : 1000
UCE_cnt: 0
```

## Connection to Unity ECC

This is the conceptual base for reading code such as syndrome generation and syndrome-table lookup in the Unity ECC simulator. The same logic scales from identifying one erroneous bit to recognizing more complex error patterns.

## Checkpoint

You should be able to explain why, for a single-bit error, the syndrome becomes exactly one H-matrix column and why duplicate columns would make correction ambiguous.
