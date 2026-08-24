# H-matrix / G-matrix

원본 실습:
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/03_10_6_Systematic_code
- https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/04_G_H_Matrix_transformation

Hamming과 Hsiao를 보면서 H-matrix는 이해했는데, encoding과 decoding에서 각각 어떤 matrix가 쓰이는지 구분하려고 본 부분이다.

내가 정리한 것은 단순하다.

- G-matrix: data에 redundancy를 붙여 codeword를 만드는 쪽
- H-matrix: 받은 codeword가 parity 조건을 만족하는지 보고 syndrome을 만드는 쪽

즉 encoding과 decoding에서 보는 역할이 다르다.

Systematic code는 data bit가 codeword 안에 그대로 보이고 parity bit가 따로 붙는 형태라서 구조를 따라가기 쉬웠다.

또 H-matrix에서 G-matrix를 만드는 과정은 Gaussian elimination으로 정리되어 있어서, 두 matrix가 서로 독립적인 것이 아니라 같은 code를 다른 관점에서 표현한다는 점을 이해하는 데 도움이 됐다.

Unity ECC를 읽을 때도 redundancy를 어디서 만들고, 실제 correction 판단은 어디에서 하는지를 구분해서 보게 되었다.
