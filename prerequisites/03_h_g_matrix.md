# 3. H-Matrix to G-Matrix and Systematic Encoding

Source exercises:
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/03_10_6_Systematic_code
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/04_G_H_Matrix_transformation

## Why this is needed

Unity ECC is easier to understand if the roles of the parity-check matrix `H` and generator matrix `G` are clearly separated.

`G` defines how redundancy is generated during encoding, while `H` defines the parity constraints checked during decoding.

## Practice A: systematic code

Use the original `(10,6)` systematic-code exercise to identify the data portion and parity portion of a codeword.

Focus on:

- how information bits remain directly visible in a systematic codeword,
- how redundancy is generated,
- how the decoder uses parity constraints rather than comparing against the original data.

Run the exercise as described in the source repository and inspect both `encode` and `decode` paths.

## Practice B: H to G transformation

Use the matrix-transformation exercise and convert an H-matrix into the corresponding G-matrix.

Run:

```bash
python Matrix_transformation.py
```

Trace the Gaussian-elimination steps rather than treating the transformation as a black box.

## Connection to Unity ECC

This exercise helps separate two questions that appear repeatedly in Unity ECC:

1. Where and how is redundancy generated?
2. Which parity constraints are later used to classify or correct a received error pattern?

This distinction is especially useful when reading cross-layer ECC designs where redundancy placement and decoding responsibility are separated across layers.

## Checkpoint

You should be able to explain the difference between `G` and `H`, what "systematic" means, and why a syndrome can be computed without retaining the original dataword.
