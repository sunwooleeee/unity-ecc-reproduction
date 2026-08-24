# Unity ECC 읽으면서 정리한 ECC 기초

Unity ECC 논문을 읽다가 막혔던 개념들을 따로 정리한 폴더다.

`ECC-exercise/01_Basic` 전체를 옮긴 것은 아니고, 실제로 Unity ECC를 이해할 때 필요했던 부분 위주로 골랐다. 원본 코드는 그대로 두고 여기에는 내가 이해한 내용과 Unity ECC와의 연결만 적었다.

원본 실습:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic

## 내가 공부한 순서

1. [Hamming SEC](01_hamming_sec.md)
   - H-matrix의 각 column이 무엇을 뜻하는지
   - syndrome으로 error bit 위치를 찾는 방식

2. [Hsiao SEC-DED](02_hsiao_sec_ded.md)
   - Hamming과 공통되는 syndrome 방식
   - single-bit correction / double-bit detection
   - Unity ECC baseline의 On-Die ECC와 연결

3. [H-matrix / G-matrix](03_h_g_matrix.md)
   - encoding과 decoding을 구분하기 위해 봄
   - redundancy를 만드는 쪽과 syndrome을 계산하는 쪽의 역할 차이

4. [GF(256) / RS Single-Symbol Correction](04_gf256_rs_ssc.md)
   - bit 단위 ECC와 symbol 단위 ECC의 차이
   - 8-bit symbol과 Chipkill을 이해하기 위해 봄

처음에는 Hsiao의 `(72,64)`와 RS의 `8 data symbols + 2 parity symbols` 같은 설명을 비슷하게 생각했는데, 공부하면서 둘은 보는 단위부터 다르다는 것을 정리했다. Hsiao는 bit-level SEC-DED이고, RS는 여러 bit를 하나의 symbol로 묶어서 다룬다.

## 출처

원본 실습과 코드는 다음 저장소의 저자들에게 있다.

https://github.com/scalable-arch/ECC-exercise

이 폴더에는 원본 코드를 복사하지 않고, 내가 공부하면서 정리한 내용만 적었다.
