# Hsiao SEC-DED

원본 실습:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/02_72_64_Hsiao_code

Hamming을 본 다음 Unity ECC baseline에 나오는 Hsiao On-Die ECC를 이해하려고 본 실습이다.

Hsiao도 H-matrix와 syndrome을 이용한다는 점에서는 Hamming과 비슷하게 이해했다. 차이는 `(72,64)` 구조에서 single-bit error를 correction하면서 double-bit error는 detect하도록 만든 SEC-DED code라는 점이다.

내가 처음에 헷갈렸던 부분은 Hsiao를 symbol 단위 ECC처럼 생각했던 것이다. 하지만 Hsiao `(72,64)`는 64 data bit + 8 redundancy bit로 보는 bit-level code이고, 뒤에서 나오는 RS code처럼 8-bit symbol 여러 개를 묶어서 보는 방식과는 다르다.

이 실습에서는 `H_Matrix.txt`를 구성하고 다음 동작을 확인한다.

- single-bit error -> correction
- double-bit error -> detection

실행:

```bash
python Hsiao_SEC_DED.py
```

Unity ECC reproduction에서는 baseline에 Hsiao On-Die ECC가 먼저 들어가기 때문에, 뒤의 `CHIPKILL + SE` 결과를 이해할 때 이 부분이 중요했다.
