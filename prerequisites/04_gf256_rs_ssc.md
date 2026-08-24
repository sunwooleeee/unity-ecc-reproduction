# 4. GF(256) and Reed-Solomon Single-Symbol Correction

Source exercises:
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/06_Finite_Field
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/07_RS_code_Single_Symbol_Correction

## Why this is needed

Unity ECC discusses rank-level protection for chip-scale faults. That requires moving from bit-level ECC intuition to symbol-level ECC intuition.

The source RS exercise uses a [10,8] Reed-Solomon code over GF(256) and injects one 8-bit symbol error. Its configuration is explicitly connected to DDR5 Chipkill-style protection in the source exercise.

## Practice A: finite-field arithmetic

Before RS decoding, complete the finite-field exercise and understand arithmetic in GF(2^m).

Implement or inspect:

- addition,
- subtraction,
- multiplication,
- division,
- representation using a primitive polynomial.

Run:

```bash
python main.py
```

The important conceptual shift is that one RS symbol is not one bit. In GF(256), one symbol represents 8 bits.

## Practice B: single-symbol correction

Open the original RS SSC exercise and trace:

1. codeword initialization,
2. one-symbol error injection,
3. syndrome generation,
4. error-symbol-location identification,
5. correction,
6. NE / CE / DUE classification.

Build and run:

```bash
g++ RS_code.cpp
./a.out
```

The source exercise expects all injected one-symbol errors to be corrected.

## Connection to Unity ECC

This exercise provides the most direct prerequisite for understanding why a Chipkill-style ECC reasons about **symbols/chips**, while Hamming and Hsiao primarily build intuition at the **bit** level.

It also helps interpret why a fault affecting one entire chip can still be treated as one symbol-level fault under an appropriate organization, while an additional fault in another chip may exceed the assumed correction model.

## Checkpoint

You should be able to explain:

- why GF(256) corresponds naturally to 8-bit symbols,
- the difference between a bit error and a symbol error,
- what SSC means,
- and why rank-level Chipkill behavior cannot be inferred only from bit-level SEC-DED capability.
