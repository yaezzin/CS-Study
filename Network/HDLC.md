# HDLC (High-level Data Link Control)

HDLC는 데이터링크 계층에서 널리 사용되는 표준으로 비트 전송을 기본으로 하며, 고속 데이터 전송에 적합한 제어 방식이다.
* 일대일 혹은 일대다로 연결된 환경에 데이터의 송수신 기능을 제공
* 역사적으로 SDLC 프로토콜을 ISO에서 발전시켜 HDLC로 발표하였고 이후 LAP, LAPB로 향상

## HDLC 프레임 구조 및 종류

<img width="539" alt="스크린샷 2022-10-10 오후 10 18 30" src="https://user-images.githubusercontent.com/97823928/194875570-209d70bf-712b-499c-895c-12a22cff5f77.png">

HDLC프로토콜을 사용하는 프레임 구조는 비트 프레임 방식을 사용한다. 프레임의 경우 다음과 같이 당야한 필드로 나누어져 구성된다.
* ```Flag (01111110)``` : 프레임의 시작과 끝을 알려주는 동기 기능을 수행
* ```Address``` : 명령의 경우 수신 측 주소, 응답의 경우 송신 측 주소 기록에 사용됨
* ```Control``` : 프레임 종류를 구분하는 용도로 사용
  * 정보 프레임 : 사용자 데이터 및 일부 제어 정보 전달을 위해 쓰이며, 제어부가 0으로 시작
  * 감독 프레임 : 오직 제어 정보만 실음. 오류 제어와 흐름 제어를 위해 사용되며 10으로 시작
  * 비번호 프레임 : 링크의 동작 모드 설정과 관리를 위해 사용되며 11로 시작 
* ```Data``` : 정보 메세지, 제어정보, 링크관리 정보 등 데이터가 삽입되는 곳
* ```CheckSum``` : CRC 기능을 통해 에러를 검출, HDLC프레임이 정확히 전송되었는가를 확인하기 위한 16비트 코드

### 프레임 종류

컨트롤 필드 값의 경우 정보, 감독, 비번호 프레임으로 구분된다. 프레임들의 필드를 살펴보자!

* ```Seq``` : 정보 프레임의 송신용 순서번호로 사용
* ```Next``` : 피기배킹을 이용한 응답기능으로 사용된다. 수신된 프레임 번호가 아닌 다음에 수신을 기대하는 프레임 번호가 제시됨
* ```Type``` : 감독 프레임과 비번호 프레임에 존재, 이 값을 통해 SABM, UA, DM 등등올 타입을 나누어준다.
* ```P/F``` : Poll/Final의 약자로 일반적으로 주국의 명령프레임에서 사용될때는 Poll, 종국의 응답 프레임에서 사용될 떄엔느 Final이라 함

```
* 주국 : 명령을 전송하는 호스트
* 종국 : 명령에 대한 응답을 회신하는 호스트
* 혼합국 : 주국과 종국 기능을 모두 지닌 호스트
```

#### 1. 정보 프레임

* Seq, Next, P/F 필드를 가짐
* 맨 처음 비트를 0으로 가져 정보 프레임인 것을 나타냄
* Seq는 송신용 순서번호를, Next는 응답용 순서번호를 가짐
* P/F는 P가 1로 설정된 경우 주국에서 종국에 데이터 전송을 허용하는 것을 의미하고 F가 1로 설정된 경우 종국에서 주국으로 데이터 전송을 하는 것을 의미

#### 2. 감독 프레임

* 맨 앞의 필드가 1로 되어 있어 정보 프레임이 아니라는 것을 나타내고 다음 비트가 0이 나와 이것이 감독 프레임인 것을 알려줌
* 감독프레임은 데이터를 응답하는 기능만 수행하기 떄문에 Seq에 대한 값은 필요 없으며 Next, Type만 존재
* Type의 경우 2비트로 구성되어 4가지 종류로 나누어짐
  * 00 : 긍정
  * 01 : 부정
  * 10 : 흐름제어의 용도로 정상적으로 작동하고 있으나 현재 데이터를 받을 수 없는 상태
  * 11 : 선택적 재전송에서의 부정 응답기능을 지원

### 3. 비번호 프레임

* 말 그대로 순서번호가 없는 프레임
* 비번호 프레임의 경우 Type의 2비트와 Modifier 3비트를 합쳐 5비트를 통해 종류를 나눔
  * SAMA : 비동기 균형 모드의 연결 설정을 요구
  * SNRM : 정규 응답 모드의 연결 설정을 요구
  * SARM : 비동기 응답 모드의 연결 설정을 요구
  * DISC : 연결 설정 해제를 요구
  * REST : 리셋 기능 수행
  * FRMR : 프레임 수신을 거부
  * UA : 비번호 프레임에 대한 응답 기능을 수행

#### 비번호 프레임의 연결 설정 모드

<img width="646" alt="스크린샷 2022-10-10 오후 10 40 28" src="https://user-images.githubusercontent.com/97823928/194879632-bf0400c7-aa04-48b2-ab0d-c4227d905f66.png">

```정규 응답 모드 (NRM)```
* 불균형 모드를 의미
* 호스트 하나는 주국이고 다른 하나는 종국인 형태
* 종국에서 데이터를 전송하려면 주국의 허락이 필요

```비동기 균형 모드 (ABM)```
* 두 호스트 모두 혼합국으로 동작
* 양쪽에서 명령과 응답을 전송할 수 있음

```비동기 응답 모드```
* 불균형 모드로 주국의 허락없이 종국에서 데이터를 전송할 수 있음

## LAP, LAPB 프로토콜

### LAP
* Link Access Protocol
* 비동기 응답 모드로 동작
* 연결 설정으로 주국에서 SARM 명령전송을 하고 종국에서 SARM 응답전송을 한다.

### LAPB
* 양쪽 호스트가 혼합국으로 동작하는 비동기 균형 모드로 설정되어 SABM 전송으로 연결이 설정됨

## 참고자료
* https://copycode.tistory.com/79
