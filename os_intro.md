## Intro

#### 운영체제의 개념

- 사용자의 프로그램과 하드웨어 사이에서 어떤 행동을 하는 것
- 하드웨어를 추상화시키는 소프트웨어
- 사용자가 하드웨어를 직접 다루게 하지 않게 하는 역할
- 여러 사용자가 함께 사용할 경우 효과적인 사용방법을 제공
- 운영체제는 자원관리자이자 자원 할당자
- 하드웨어를 여러 프로그램이 동시에 접근할 때 발생하는 문제점들을 해결
- 사용자 프로그램의 순서, 방법, 자원의 잘못된 사용 및 에러를 방지해주는 역할

#### OS의 계열

- POSIX 계열
  - Linux, BSDs, Mac, Android, IOS, QNX
  - API들이 같은 모양을 따른다
- Windows 계열

#### 운영체제의 부팅 Procedure

1. 파워를 켜게 되면 BIOS start
2. CMOS에 저장된 setting을 load
3. 그 값을 바탕으로 메모리를 포함한 CPU 외 device 초기화
4. POST(Power-On Self Test) 실행 - 잘못된 부분이 있다면 알려줌
5. BIOS code가 bootstrap(부팅) 실행

##### BIOS

- 일종의 작은 os
- 최초로 부팅을 하는 코드들이 들어있다
- 작은 칩이 있고 그 안에 코드들이 들어있는데 이를 ROM이라고 함. 요즘은 ROM(수정 불가능) 대신 플래시 메모리(erase, programming 가능) 사용
- 칩에 있는 코드들이 RAM에 실려 사용
- 운영체제 코드들은 바로 실행이 안되고 RAM에 들어있어야만 CPU가 가져와서 실행 가능 그 코드들을 RAM으로 옮겨주는 코드가 필요하다. 그러한 코드들이 BIOS 안에 들어있어 자동으로 실행 그 역할을 해주는 BIOS
- BIOS가 시작하는 주소는 정해져있어야 한다. BIOS 코드는 어셈블리로 만듦
- BIOS 코드들이 하드웨어에 따라 맞춤으로 만들어지므로 표준적인 코드가 만들어져야 된다
- 운영체제의 Boot Code도 특정 위치에 있어야만 가져올 수 있다. 그 위치가 MBR(Master Boot Record)
- MBR에 위치한 코드가 RAM에 올라가게 되면 그때부터 운영체제가 동작하는 코드를 수행 
- MBR을 찾아서 코드를 load하는데까지가 BIOS의 역할이다.

 ##### Bootstrapping

- 실제 OS의 코드는 어딘가에 저장, 그것을 찾아서 load를 해야되는데 그게 어딨는지 모르는게 문제
- MBR은 어디에 있다고 지정을 미리 해놓고 BIOS가 그것을 찾으면 RAM에 적재시키면 OS를 load

##### MBR

- 512byte = 약속대로 정해진 코드, 실제 코드는 446byte 나머지는 Partition Entry와 매직넘버
- 446byte로는 OS를 load하기에 너무 작다 . 따라서 이 코드를 가지고 다음 코드를 load하고 그 코드로 또 다음 코드를 Load 하는 chain-loading sequence를 시작한다.

#### Interface to Applications - 어플리케이션과 운영체제가 어떻게 interface하는지

주로 System call을 통해 interface - intterupt 방식으로 구현

- 운영체제의 API라고 할 수 있다. 
- Table로 운영체제의 API 정리하고 시스템 호출 번호를 할당
- 어떤 번호가 들어오면 해당 번호에 해당하는 API를 호출

#### Kernel

<b>Kernel의 특징</b>

- 일종의 프로그램인데, 항상 수행하고 있는 프로그램이다. 운영체제의 핵

- bootloader가 실제로 load하는 코드가 kernel이다
- CPU와 memory 관리
- 프로그램을 메모리에 적재시켜 수행시키는 기능
- 시스템 호출 반드시 제공해야함
- 여러 사용자가 시스템의 자원을 사용함에 있어 다른 프로그램을 방해하면 안되고 사용자의 프로그램으로부터 운영체제를 보호. 결함이 발생했어도 견딜수 있어야함

<b>Architecting Kernels</b>

1. Monolithic kernels
   - 커널이 한 덩어리 => 모든 기능이 하나의 kernel에 존재
   - 모든 코드는 kernel-space에서 구동
   - kernel이 커지는 경향이 있음
   - 장점 : 1. 커널이 개발하는데 코드베이스가 하나이기 때문에 일관성이 있다. 2. 어플리케이션 개발자에게 단일 API를 보여줄 수 있다. 3. Device driver를 찾을 때 복잡한 절차가 필요 없다. 4.  퍼포먼스(속도가 빠르다)
   - 단점 : 1. manage하기가 어렵고 build도 오래 걸림. 2. 만약 작은 device에서 문제가 생긴다면 전체 커널이 다운이 될 수 있다.
2. Microkernels(= Message Packing Architecture)
   - kernel의 아주 핵심적인 부분만 kernel로 만듬
   - 나머지는 kernel 밖 User Space에서
   - 장점 : 1. 코드가 작기때문에 버그를 찾기 쉽다. 2. 보안이 중요한 시스템에는 micro 커널 사용. 
   - 단점 : 1. 성능이 좋지 않다. 2. API가 단일화가 돼있지 않아 사용하기 어렵다. 

3. Hybrid kernel
   - 위 두개를 합친 형태
   - 실제는 1에 가깝다 - 자료 참고

