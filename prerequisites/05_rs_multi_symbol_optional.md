# 5. Optional: Reed-Solomon Multi-Symbol Correction

Source exercise:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/08_RS_code_Multi_Symbol_Correction_Berlekamp_Massey

## Why this is optional

The reproduced Unity ECC scenarios in this repository can be understood without implementing a full multi-symbol RS decoder. However, this exercise is useful for seeing how correction capability scales beyond SSC and why stronger symbol-level correction requires additional redundancy and more complex decoding.

The source exercise implements a shortened [20,16] Reed-Solomon code over GF(256) and uses the Berlekamp-Massey algorithm for multi-symbol correction.

## Practice

Trace the following decoder stages:

1. syndrome calculation,
2. error-locator polynomial construction,
3. Berlekamp-Massey update process,
4. error-location search,
5. correction and result classification.

Build and run the source exercise as instructed there.

## Connection to Unity ECC

This gives context for the trade-off behind stronger rank-level ECC:

- greater correction capability,
- more redundancy,
- more complex decoding,
- and potentially different latency/implementation costs.

Unity ECC's contribution is not simply "use the strongest possible code." It coordinates protection mechanisms and correction policies around the expected fault patterns and redundancy constraints.

## Checkpoint

You should be able to explain why correcting two symbol errors is qualitatively more complex than correcting one symbol error and why stronger correction capability is not free.
