# Swapping

<img src="./images/virtualmemory72.png" />

페이징 시스템 가정 - 디스크로 페이지로 프레임을 나눴을 경우,

물리 메모리가 프레임으로 나눠져있을 때 사용하지 않는 프레임이 있다면 디스크에 저장하고 디스크에 있는 페이지를 메모리에 올리는 것

만약 프로세스가 메모리에 접근할 때 접근하려는 페이지가 DRAM에 없는 경우(Page fault)를 처리해줘야 한다. 이럴 경우 스와핑이 일어나게 된다. 즉, 디스크에 있는 내가 원하는 페이지를 가져온 다음 다시 접근해야 한다.

스와핑은 CPU가 자동으로 할 수 없고 운영체제가 구현을 해줘야 한다. 그리고 CPU는 운영체제가 구현하는 부분을 지원할 수 있는 하드웨어를 지원해야 한다.

----

<img src="./images/virtualmemory73.png" />

----

<img src="./images/virtualmemory74.png" />

1. 스와핑을 위한 자료구조 - 메모리에서 어떤 페이지가 Swap out 했는지 또 어떤 페이지가 디스크에 있는지 등 여러 정보들을 알아야 하기 때문
2. 메모리 페이지에 대한 메타데이터가 필요
3. Page fault 핸들러를 운영체제가 구현할 수 있어야 함

----

<img src="./images/virtualmemory75.png" />

----

<img src="./images/virtualmemory76.png" />



----

<img src="./images/virtualmemory77.png" />

----

<img src="./images/virtualmemory78.png" />

운영체제의 page falut 핸들러는 가장 중요하게 해야하는 일이 현재 메모리 상에 있는 어떤 페이지를 쫓아내고(swap out) 어떤 페이지를 swap in 할지 결정하는 일을 한다. 

1. On-demand approach - 요구가 있을 때 수행하는 것
   - 페이지가 요청이 됐는데 들어갈 곳이 없다면 그때 스와핑을 하겠다
2. Proactive - 선제적으로 대응
   - On-demand의 문제점은 메모리가 꽉 찰 때까지 기다렸다 수행되기 때문에 이후에는 디스크와 계속해서 스와핑이 일어나서 디스크와 스와핑하는 시간이 길어지는 문제점이 있다. 이는 성저하로 이어진다.
   - Proactive는 꽉 찰때 까지 기다리지 않고 꽉 차기 전에 어느정도를 남겨놓고 그 기준에 도달하면 미리 스와핑을 하는 것

----

<img src="./images/virtualmemory79.png" />

어떤 페이지를 쫓아낼 것인가? == page replacement policy

수학적으로 optimal한 방법이 존재 

- 미래의 접근할 페이지들을 알고있다고 가정하면 최적의 방법은 가장 나중의 접근할 프로세스를 쫓아내는 것
- 그러나 미래를 알 수 없기에 실제로는 이렇게 구현할 수 없다.
- 하지만 Optimal 값과 비교해서 얼마나 근접하게 구현할 수 있는가에 대한 비교는 가능

----

<img src="./images/virtualmemory80.png" />

Optimal과 LRU의 차이

3개의 페이지만 가질 수 있다고 가정하고 나머지는 디스크에 존재

Optimal은 미래를 보는 것이기 때문에 0,1,2는 처음 접근하는 것이기 때문에 무조건 Miss가 나는데 이 Miss를 Cold Miss라고 한다. 이렇게 세 개가 Miss가 나면 메모리에 0,1,2 가 들어가게 된다. 만약 3을 액세스 하려 하면 Miss가 발생하고 Optimal은 가장 나중에 수행되는 2를 쫓아내고 3을 메모리에 추가하게 된다. 

LRU는 미래를 모르기 때문에(과거를 본다. 다시 말해, 지나온 프로세스들을 검사(?) 한다.) Cold Miss까지는 똑같고 3을 액세스하려 하면 Miss가 발생하고 가장 최근에 접근하지 않은 2를 쫓아낸다.

시험문제 - Miss가 몇 번 일어나는가? (Cold Miss 포함 유무)

최악의 경우에도 LRU가 Optimal의 두 배를 넘지 않는다는 것이 수학적으로 증명이 되어 있음

----

<img src="./images/virtualmemory81.png" />

Cache 사이즈가 늘어날수록 Hit가 늘어난다.

Locality가 없다면 어떤 방법을 쓰던 큰 영향이 없고 Cache 사이즈에 비례해서 늘어난다.

----

<img src="./images/virtualmemory82.png" />

Locality가 있을 경우(80%의 메모리 액세스가 20%의 페이지 안에서 이루어 진다.)

FIFO나 RAND는 Locality가 적용이 되면 성능이 약간 개선되고 LRU가 조금 더 Optimal에 가까워 진다.

----

<img src="./images/virtualmemory83.png" />



----

<img src="./images/virtualmemory84.png" />

LRU를 어떻게 구현할 것인가?

LRU는 가장 최근에 접근하지 않은 액세스를 쫓아내야 하므로 접근한 시간 또는 접근한 순서를 저장해야 한다. 

1. 각각의 액세스를 페이지 테이블에 기록한다
   - 이 방법은 페이지 테이블을 찾을 때 추가적인 오버헤드가 일어난다.
2. 'LRU도 approximating을 사용해서 구현해보자' 

----

<img src="./images/virtualmemory85.png" />



----

<img src="./images/virtualmemory86.png" />

LRU를 어떻게 Approximating하는가?

먼저 페이지 테이블 엔트리에 액세스 비트를 가지고 LRU를 할 건데 운영체제가 일정한 시간마다 이 비트를 체크를 해서 setting이 되어 있으면 접근을 했다고 보는 것이다. 이렇게 하려면 운영체제가 이 비트를 0으로 먼저 만들어줘야 한다.

정확하게는 알 수 없지만 대략적인 시간은 알 수 있다.

----

<img src="./images/virtualmemory87.png" />

----

<img src="./images/virtualmemory88.png" />



----

<img src="./images/virtualmemory89.png" />

----

