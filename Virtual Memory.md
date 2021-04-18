#### Physical vs Virtual Memory

<img src="./Images/virtualmemory1.png" /> 

Physical Memory만 사용하면 즉, DRAM만 사용하면 

1. 프로세스 간의 Protection, Isolation이 제공되지 않는다.
2. 포인터의 주소가 고정이 된다
3. 제한된 크기
4. 기타 등

와 같은 문제가 일어날 수 있다.

Virtual Memory를 이용해서 위와 같은 문제들을 해결할 수 있다.

----

#### Protection and Isolation

<img src="./Images/virtualmemory2.png" />

예를 들어 같은 주소 공간에 있다면 악의적인 프로세스가 다른 프로세스에 접근하는 것을 막을 방법이 없다.

또한 커널 메모리에 대해서도 같은 주소 공간에 있으므로 overwrite가 가능하기 때문에 시스템의 안정화에 문제가 있을 수 있고 부팅에 문제가 있을 수 있다. 

----

#### Problem: Pointers in Programs

<img src="./Images/virtualmemory3.png" />

이 문제는 프로세스가 하나일 때는 문제가 없는데 프로세스가 여러 개일 때 문제가 생길 수 있다.

<img src="./Images/virtualmemory6.png" />

포인터의 위치가 고정되기 때문에 Process2에서 foo 함수를 호출하더라도 Process1의 foo 함수에 접근하게 된다. 이는 곧 Protection이 안된다는 뜻

----

<img src="./Images/virtualmemory5.png" />

메모리가 꽉 찬 경우 새로운 프로세스가 도착했을 때 어떤 프로세스가 종료되기 전에는 수행시킬 수가 없게 된다.

----

<img src="./Images/virtualmemory7.png" />

Physical Memory를 사용할 때의 문제는 프로세스가 여러 개일 때 발생한다.

프로세스들의 함수나 데이터는 포인터에 의존하는데 프로세스가 여러 개 동작하게 되면 컴파일 했을 때 위치가 달라진다는 문제가 있다.

----

<img src="./Images/virtualmemory8.png" />

여러 프로세스를 동시에 수행시키려 할 때 어떻게 해결할 수 있을까

1. Address를 고정시켜서 해결하는 방법
2. Load 할 때 Address를 고치는 방법
3. 컴파일된 코드가 먼저 결정된 Address가 아니라 상대적인 Address를 가지게 하는 방법
4. 하드웨어의 서포트를 이용하는 방법

우리가 보고자 하는 것은 4번째 하드웨어의 서포트(가상 메모리)를 이용하는 방법이다

----

<img src="./Images/virtualmemory9.png" />

이 방법은 프로그램(프로세스)가 하나만 있을 때는 문제가 없다. 

동시에 여러 프로세스를 사용하게 하는 방법은 각각의 프로그램을 여러 번 컴파일 하는 것이다. 

저장 공간을 낭비하기 때문에 비효율적인 방법이다.

----

<img src="./Images/virtualmemory10.png" />

Load-Time에 주소를 바꿔주는 방법

첫 번째 주소만 처음에 Load 할 때 바꿔준다는 의미

----

<img src="./Images/virtualmemory11.png" />

정해진 Address를 집어 넣는게 아닌 상대적인 위치를 집어 넣는 것

이 프로그램이 어느 위치로 가던지 그 프로그램이 유효하도록 만들어 주는 것

주소를 숨길 수 있는 장점이 있다.

----

#### Hardware Support

<img src="./Images/virtualmemory12.png" />

여러 개의 프로세스들이 메모리를 공유하는 가장 일반적인 방법

가상 메모리 상의 고정된 위치에서 프로그램을 컴파일

OS와 MMU가 동시에 작동을 해서 Virtual Memory를 Physical Memory로 바꿔서 그 주소를 변형을 하는 형식

----

<img src="./Images/virtualmemory13.png" />

가상 메모리란 프로세스가 논리적으로 가질 수 있는 Address Space를 뜻한다.

실제로 존재하려면 Physical Memory와 Virtual Memory가 Mapping이 되어야 한다.

<img src="./Images/virtualmemory14.png" />

프로세스 1은 자신이 0번지에 있다고 생각하지만 실제로는 0번지가 아니다. 

그렇다면 이 주소를 어떻게 바꿔줄까 하는 문제가 생긴다. 현재는 어떠한 Magical한 방법으로 바꿨다고 생각을 하자. 

이걸 도와주는게 바로 CPU에 있는 MMU(가상 메모리를 Physical Memory로 바꿔주는 역할)

<img src="./Images/virtualmemory17.png" />

프로세스는 항상 Virtual Address만 사용 

그렇다면 Physical Address는 어떻게 접근하느냐? MMU가 Virtual Address를 Physical Address로 바꿔서 접근하기 때문에 프로세스는 Physical Address에 접근할 수 없고 커널만 접근 가능하다.

----

<img src="./Images/virtualmemory15.png" />

Address Translation은 MMU의 구현이 되어있지만 MMU만으로는 불가능하고 운영체제가 MMU가 필요로 하는 정보들을 넣어주면 MMU가 Physical Address를 Virtual Address로 바꿔주는 동작을 하게 된다.

----

<img src="./Images/virtualmemory16.png" />

MMU가 Virtual Address를 Physical Address로 바꾸는 방법은 여러 가지가 있는데 (다시 말해 MMU가 어떻게 구현이 되어 있는가)

1. Base and bound registers
2. Segmentation
3. Page tables
4. Multi-level page tables

이제부터 MMU를 이용해서 가상 메모리를 구현하는 방법들을 차례대로 보도록 하자.

----

#### Base and Bounds Registers

<img src="./Images/virtualmemory18.png" />

Base Register와 Bound Register를 사용하는 것이다. 

가상메모리가 연속적으로 있는 하나의 덩어리고 이 연속적으로 있는 가상 주소 덩어리를 연속적으로 있는 Physical  Memory의 덩어리로 Mapping 하는 것

이 프로세스가 적재되는 곳은 BASE 레지스터가 될 것이고 크기는 BOUND 레지스터에 적힌 값

<img src="./Images/virtualmemory19.png" />

<img src="./Images/virtualmemory20.png" />

<img src="./Images/virtualmemory21.png" />

BASE, BOUND Register 값은 처음 load를 했을 때 정해져 있어야 한다. 

이 값을 프로세스가 바꾸면 안된다 바꾸게 되면 Protection이 안된다.

Context에 포함이 되어야 한다.

<img src="./Images/virtualmemory22.png" />

<img src="./Images/virtualmemory23.png" />

<img src="./Images/virtualmemory24.png" />

<img src="./Images/virtualmemory25.png" />

BASE and BOUND Register를 사용하면 

1. 하드웨어를 간단하게 구현할 수 있고
2. 간단한 구현임에도 각 프로세스의 Virtual Memory를 구현할 수 있다.
3. 포인터 위치가 고정되는 문제가 해결된다. 다시 말해, 임의의 위치에 프로세스를 적재할 수 있다.
4. Protection이 가능하다
5. (모든 주소를 변환하지 않고 BASE Register의 값을 변경함으로써) 메모리 상에서 데이터의 위치를 유연하게 할당할 수 있다.

----

#### Limitation

<img src="./Images/virtualmemory26.png" />

1. 프로세스가 자기 자신을 Protect를 못한다.
2. Memory를 공유하기가 어렵다. 같은 코드를 가진 프로세스를 동시에 실행시켰을 때 코드 영역이 같을텐데 두 개의 메모리 공간을 차지하게 된다.
3. 메모리가 동적으로 커지지 못함(Internal Fragmentation 문제 발생)

----

<img src="./Images/virtualmemory27.png" />

<img src="./Images/virtualmemory28.png" />

프로세스가 동적으로 사용하는 메모리(스택, 힙)가 있으므로 메모리가 부족하지 않도록 크게 할당을 해줘야 한다.

이렇게 되면 Internal fragmentation(내부 단편화) 문제가 발생할 수 있다.

Internal fragmentation(내부 단편화) 는 할당된 메모리 중에서 사용하지 않는 메모리가 존재하는 것을 말한다. 이는 효율적이라고 할 수 없고 Space가 낭비 된다고 말할 수 있다.

<img src="./Images/virtualmemory29.png" />

만약 메모리를 충분하게 할당하지 않는다면 위와 같이 스택이 힙 영역을 침범하게 될 수 있다.

<img src="./Images/virtualmemory30.png" />



이 문제는 Stack 영역을 위로 올리지 않는 이상 해결될 수 없고 올린다 하더라도 다시 Internal fragmentation 문제가 발생할 수 있다.