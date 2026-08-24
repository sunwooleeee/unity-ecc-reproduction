# RS Multi-Symbol Correction (참고)

원본 실습:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic/08_RS_code_Multi_Symbol_Correction_Berlekamp_Massey

이 부분은 현재 Unity ECC reproduction을 진행하면서 직접 필요한 수준까지는 아니어서 참고용으로만 남겼다.

Single-Symbol Correction보다 더 많은 symbol error를 고치려면 redundancy도 더 필요하고 decoding도 복잡해진다는 정도를 보기 위해 확인했다.

원본 실습은 GF(256) 기반 shortened RS code에서 여러 symbol error를 correction하고, Berlekamp-Massey algorithm을 사용한다.

지금 정리한 Unity ECC 실험에서는 이 알고리즘 자체를 구현하거나 수정하지 않았다. 다만 ECC correction capability를 강하게 만들수록 parity/redundancy와 decoding 복잡도가 같이 늘어난다는 맥락을 이해하는 데 참고했다.
