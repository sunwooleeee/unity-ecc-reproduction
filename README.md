# Unity ECC 실험 재현 정리

## 시작하게 된 이유

Read Disturbance를 공부하다가 메모리 오류를 실제로 어떻게 막는지 궁금해져서 ECC까지 내려오게 되었다. Unity ECC 논문을 읽기 시작했는데, 처음에는 Hamming, Hsiao, Chipkill, RS code가 각각 어떤 단위의 오류를 보는지 잘 구분이 되지 않았다.

그래서 논문을 계속 읽기 전에 Hamming과 Hsiao부터 다시 보고, 이후 symbol 단위 ECC와 Chipkill까지 연결해서 공부했다. 그 다음 공개된 Unity ECC simulator를 직접 실행하면서 논문에 나온 error scenario 중 일부를 재현해 보았다.

원본 Unity ECC 구현:
https://github.com/scalable-arch/SC_23-Unity-ECC

## 논문을 읽으면서 정리한 ECC 기초

내가 가장 먼저 이해한 것은 H-matrix와 syndrome의 관계였다.

Hamming code에서 각 bit의 H-matrix column은 그 bit가 어떤 parity 검사에 포함되는지를 나타낸다. 한 bit에 오류가 생기면 syndrome이 그 bit의 column과 같아지기 때문에 오류 위치를 찾을 수 있다. 반대로 서로 다른 bit가 같은 column을 가지면 syndrome만 보고 어느 bit가 틀렸는지 구분할 수 없다.

이 개념을 바탕으로 Hsiao SEC-DED를 보았다. Hsiao 역시 bit 단위 ECC이고, Unity ECC simulator의 baseline에서는 On-Die ECC로 사용된다.

여기서 처음에 내가 헷갈렸던 부분이 하나 있었다. `8 data symbols + 2 parity symbols` 같은 구조를 Hsiao와 같은 것으로 생각했는데, 이것은 RS code처럼 symbol 단위로 보는 ECC의 설명이고 Hsiao의 `(72,64)` bit-level SEC-DED와는 다른 구조였다.

관련해서 공부한 실습과 메모는 [`prerequisites/`](prerequisites/README.md)에 따로 정리했다.

원본 실습:
https://github.com/scalable-arch/ECC-exercise/tree/main/01_Basic

## 실험 설정

공개된 Unity ECC simulator의 decoding algorithm은 수정하지 않고 실행 횟수와 mode만 바꿨다.

- `RUN_NUM`: 1,000,000,000 -> 100,000
- Conservative: `CONSERVATIVE_MODE = 1`
- Restrained: `CONSERVATIVE_MODE = 0`

비교한 구성은 다음 세 가지다.

- Baseline: Hsiao SEC-DED On-Die ECC + AMD Chipkill
- Unity Conservative
- Unity Restrained

## 재현한 error scenario

### DBE + DBE

서로 다른 두 chip에 double-bit error가 발생하는 경우를 실행했다.

### CHIPKILL + SE

한 chip에는 chip-wide fault가 발생하고, 다른 chip에는 single-bit error가 추가로 발생하는 경우를 실행했다.

## 결과

| Error scenario | Configuration | CE | DUE | SDC |
|---|---|---:|---:|---:|
| DBE + DBE | Baseline | 8.862% | 89.763% | 1.375% |
| DBE + DBE | Unity Conservative | 12.243% | 87.658% | 0.099% |
| DBE + DBE | Unity Restrained | 98.710% | 1.191% | 0.099% |
| CHIPKILL + SE | Baseline | 100.000% | 0.000% | 0.000% |
| CHIPKILL + SE | Unity Conservative | 0.000% | 93.531% | 6.469% |
| CHIPKILL + SE | Unity Restrained | 3.512% | 89.974% | 6.514% |

전체 raw output은 `results/raw/`, 정리한 값은 `results/summary.csv`에 저장했다.

## 결과를 보고 이해한 부분

### DBE + DBE

Baseline에서는 대부분 DUE가 발생했고 CE는 약 8.9%였다. 반면 Unity Restrained에서는 CE가 약 98.7%까지 올라갔다.

코드를 따라가 보니 Unity의 SSC-DEC 쪽에서는 syndrome을 이용해서 두 chip에 있는 double error를 correction하는 경로가 있었다. Conservative mode에서는 여러 chip을 correction한 경우를 다시 DUE로 판단하는 조건이 있기 때문에 Restrained보다 CE가 낮게 나왔다.

또 하나 눈에 띈 것은 SDC였다. Baseline의 SDC는 1.375%였는데 Unity에서는 약 0.1% 수준으로 줄었다.

### CHIPKILL + SE

이 경우에는 결과가 반대로 나왔다.

Baseline은 이번 simulator 설정에서 100% CE였고, Unity Conservative/Restrained는 대부분 DUE가 나왔다.

내가 이해한 이유는 ECC가 배치된 순서의 차이다. Baseline에는 Hsiao On-Die ECC가 먼저 있기 때문에 single-bit error를 먼저 처리한 뒤 rank-level Chipkill이 chip fault를 처리할 수 있다. 반면 이번 Unity 설정은 별도의 On-Die ECC를 사용하지 않기 때문에 chip-wide fault와 추가 single-bit error가 함께 rank-level ECC로 들어간다.

이 두 실험을 비교하면서, 단순히 "더 강한 ECC가 항상 더 좋다"고 보기 어렵다는 점을 이해했다. 어떤 fault가 발생하는지, bit 단위인지 chip/symbol 단위인지, 그리고 ECC가 어느 계층에 배치되어 있는지가 같이 중요했다.

## 내가 한 작업

- Unity ECC 논문을 읽으면서 필요한 ECC 기초를 정리
- Hamming/Hsiao의 H-matrix와 syndrome 동작 확인
- 공개 Unity ECC simulator build 및 실행
- DBE+DBE, CHIPKILL+SE scenario 재현
- Baseline / Conservative / Restrained 결과 비교
- CE / DUE / SDC raw result 정리
- fault injection과 decoder 관련 코드 경로 확인
- 두 scenario에서 결과가 다르게 나오는 이유 정리

ECC algorithm 자체를 새로 구현하거나 수정한 것은 아니며, 이 저장소는 공개 구현을 이용한 학습, 실험 재현 및 결과 분석을 정리한 것이다.

## 출처

Unity ECC simulator:
https://github.com/scalable-arch/SC_23-Unity-ECC

ECC 기초 실습:
https://github.com/scalable-arch/ECC-exercise
