# GF(256) / RS Single-Symbol Correction

원본 실습:
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/06_Finite_Field
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/07_RS_code_Single_Symbol_Correction

Hsiao까지는 bit 단위 ECC로 이해했는데, Chipkill과 RS code를 보면서 symbol이라는 단위가 새로 나와서 이 부분을 따로 봤다.

내가 정리한 핵심은 `1 symbol = 1 bit`가 아니라는 점이다. 이 실습의 RS code는 GF(256)을 사용하므로 한 symbol이 8 bit다.

그래서 bit error와 symbol error는 다르게 봐야 한다. 예를 들어 한 chip에서 여러 bit가 망가져도 ECC 구조에 따라서는 하나의 symbol error로 볼 수 있다.

Finite Field 실습은 RS code에서 쓰는 GF 연산이 어떤 식으로 돌아가는지 보기 위한 단계였고, 그 다음 RS Single-Symbol Correction 실습에서는 8-bit symbol 하나가 잘못된 경우를 correction한다.

RS 실습의 구조는 `[10,8]`이고, 8개의 data symbol과 2개의 parity symbol을 사용하는 식이다. 예전에 이 구조를 Hsiao `(72,64)`와 비슷하게 생각했는데, 실제로는 Hsiao는 bit-level SEC-DED이고 RS는 symbol-level code라서 출발점이 다르다.

Unity ECC를 볼 때 이 차이를 알고 나서야 Chipkill이 왜 chip/symbol 단위 fault를 다룬다고 설명되는지 조금 더 명확해졌다.
