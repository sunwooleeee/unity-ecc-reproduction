# 2. Hsiao SEC-DED: Hardware-Friendly Parity-Check Design

Source exercise:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/02_72_64_Hsiao_code

## Why this is needed

The baseline configuration in the Unity ECC simulator uses Hsiao SEC-DED as On-Die ECC. Understanding Hsiao therefore directly helps interpret the baseline behavior in the reproduced fault scenarios.

The original exercise asks you to construct a (72,64) Hsiao SEC-DED H-matrix and verify both single-bit correction and double-bit detection.

## What to understand

Hsiao codes are designed as odd-weight-column SEC-DED codes. Compared with a straightforward extended-Hamming-style construction, the H-matrix is chosen to reduce the XOR-tree depth and hardware cost.

The important properties to check are:

- every column is non-zero,
- every column is unique,
- columns have odd weight,
- single-bit errors produce a syndrome that identifies one column,
- double-bit errors remain detectable rather than being silently accepted.

## Practice

1. Open the original Hsiao exercise.
2. Construct `H_Matrix.txt` yourself.
3. Check the column weights and uniqueness.
4. Run:

```bash
python Hsiao_SEC_DED.py
```

5. Confirm the expected behavior:

```text
CE_cnt : 1000
DUE_cnt: 1000
UCE_cnt: 0
```

## Connection to Unity ECC

In the reproduced `CHIPKILL + SE` scenario, the baseline includes a Hsiao On-Die ECC stage before rank-level Chipkill. This layered organization is why the baseline can behave very differently from Unity configurations that disable the separate On-Die ECC stage.

## Checkpoint

You should be able to explain:

- how Hsiao extends the single-bit syndrome idea,
- why odd-weight columns are useful for SEC-DED,
- why Hsiao is considered hardware-friendly,
- and why the presence or absence of an On-Die ECC layer changes the fault pattern seen by rank-level ECC.
