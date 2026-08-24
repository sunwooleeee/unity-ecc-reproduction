# Hamming SEC

원본 실습:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/01_7_4_Hamming_code

Unity ECC를 읽기 전에 H-matrix와 syndrome 관계가 잘 안 잡혀서 먼저 본 실습이다.

내가 이해한 핵심은 다음과 같다.

H-matrix의 각 column은 그 bit가 어떤 parity 검사에 포함되는지를 나타낸다. 한 bit에 error가 생기면 syndrome이 그 bit의 column과 같아진다. 그래서 각 bit의 column이 서로 다르면 syndrome을 보고 어느 bit에서 error가 났는지 찾을 수 있다.

반대로 두 bit가 같은 column을 가지면 같은 syndrome이 나오기 때문에 위치를 구분할 수 없다.

이 실습에서는 `(7,4)` Hamming code의 `H_Matrix.txt`를 구성하고 single-bit error가 모두 correction되는지 확인한다.

실행:

```bash
python Hamming_SEC.py
```

내가 이 실습에서 가져간 부분은 결국 `syndrome -> H-matrix column -> error 위치`라는 구조다. 이후 Hsiao와 Unity ECC decoder를 볼 때도 이 관점으로 코드를 읽었다.
