Paging Sytem에서 Page Table의 크기가 커감으로 인해 메모리를 많이 차지하는 문제와 메모리를 여러 번 Access함으로써 생기는 문제들이 있었다.

----

<img src="./images/virtualmemory57.png" />

페이지 테이블을 사용해서 페이징 시스템을 이용하면 가상메모리를 구현함에 있어 여러 장점이 있었다. 하지만 큰 문제는 페이지 테이블이 커질 수 있다는 것

페이지 테이블이 크기 때문에 CPU에 저장되는 것이 아니라 메모리의 RAM에 저장이 되야하기 때문에 가상메모리를 액세스할 때는 가상메모리의 주소를 물리메모리로 바꿔야 되니까 페이지 테이블 액세스를 함으로써 한 번의 메모리 접근이 반드시 페이지 접근을 포함한 두 번의 접근으로 구현이 되니까 메모리 오버헤드가 두 번이 되는 문제가 있다.

----

## Page Table의 크기 문제 해결(TLB 이용)

<img src="./images/virtualmemory58.png" />

위 문제를 해결하기 위한 답이 바로 Caching

TLB는 페이지 테이블에 대한 Cache를 뜻한다. PTE를 저장하는 Cache

TLB는 CPU안에 있는 Cache이기 때문에 CPU와 같은 속도로 동작하는 굉장히 빠른 Cache

TLB에 캐싱이 되어있는 페이지 테이블을 접근하게 되면 메모리를 두 번 접근하는 효과가 없어지고 한 번만에 접근하는 것과 거의 유사한 속도를 가진다.



<img src="./images/virtualmemory59.png" />

TLB가 Fast Path로 수행하는 비율이 엄청나게 높은 성질을 Locality(지역성)이라고 한다.

Spatial locality(공간적 지역성) : 어떤 메모리의 주소에 접근하게 되면 해당 주소의 근처를 접근할 가능성 높음. 따라서 같은 페이지 안에 있을 확률이 높다.

Temporal locality(시간적 지역성) : 어떤 주소를 접근했다면 같은 주소에 가까운 시간 내에 접근할 가능성이 높다.

----

## Advanced Page Table(Page Table의 크기 문제 해결)

<img src="./images/virtualmemory60.png" />

페이지 테이블이 크기가 커서 램(메모리)에 올라가야 하기 때문에 접근할 때 두 번 접근해야되는 문제를 TLB라는 캐시를 이용해서 스피드 이슈를 해결했는데 테이블 사이즈가 너무 크다는 문제가 남아있다.

예를 들어 4KB 페이지를 가진 32 bit 시스템에는 각각의 페이지 테이블의 크기가 4MB인데 대부분의 엔트리는 invalid 상태이고 4MB 중 아주 조금만 사용하는 문제가 있다. 이 페이지 테이블 크기를 줄이는게 하나의 문제가 된다.

어떻게 이 페이지 테이블의 크기를 줄일 것이냐에는 여러 가지 방법이 있는데 가장 많이 사용되는 방법은 x86에서 사용되는 다단계 페이지 테이블(Multi-layer page table)이라는 방법이다. 그 외의 방법들 또한 존재한다.

----

<img src="./images/virtualmemory61.png" />

가장 먼저 생각할 수 있는 해결책은 페이지의 크기를 키우는 것이다.

이 방법의 문제는 페이지 사이즈가 커지니까 Internal Fragmentation이 커진다. 

그리고 기본적으로 영역이 가장 작은 사이즈가 커지니 힙 스택 코드 또한 해당 사이즈가 된다. 이는 메모리를 낭비하게 되는 문제가 발생할 수 있다.

----

<img src="./images/virtualmemory62.png" />

또 다른 방법은 자료구조를 사용하는 것

페이지 테이블의 크기가 커진 이유는 페이지 테이블이 배열로 구현되어 있기 때문이다. 배열로 쓰기 때문에 index를 다 사용해야 되서 쓸데없는 공간들도 할당을 해줘야 한다. 그렇기 때문에 이러한 자료구조를 리스트나 트리로 만들어주면 중간에 비어있는 공간들을 제거해줄 수 있다. 

----

<img src="./images/virtualmemory63.png" />

또 다른 방법으로 Inverted Page Table이 있다.

기본적인 아이디어는 Page Table 엔트리가 많은 이유는 각 프로세스마다 가상 메모리 주소가 다 있고 가상 메모리 주소를 물리 메모리 주소로 매핑하기 때문인데 Inverted Page Table은 거꾸로 PFN를 가지고 테이블을 만들면 테이블이 시스템에 하나만 있으면 된다는 점을 이용해 크기 문제를 해결한 방법이다.

----

<img src="./images/virtualmemory64.png" />

이 방법 또한 문제가 있다.

테이블이 하나만 있기 때문에 크기 문제는 해결이 되지만 찾는게 복잡하게 된다.  

또한, 가상 메모리를 어떻게 구현할 것인가 하는 문제가 있다.

----

<img src="./images/virtualmemory65.png" />

표준적으로 사용되는 페이지 테이블

페이지 사이즈는 그대로 두고 나머지 20 bit의 페이지 테이블 엔트리 인덱스를 나눠 트리를 두 단계로 유지하는 방법(뭔 말..?)

----

<img src="./images/virtualmemory66.png" />

IA32 Architecture 에서는 Hybrid approach를 사용

다시 말해, 세그멘테이션과 페이징을 동시 사용. 64bit에서는 사용 x

세그멘테이션을 하는데 그것을 페이지 단위로 한다. 즉, 한 세그먼트가 여러 개의 페이지로 구성되어 있다.

----

<img src="./images/virtualmemory67.png" />

IA32 Architecture에서는 Linear Address라는 것을 만들어 낸다.

Linear Address를 만들기 위한 Selector라는 구조가 있다. 

Selector는 13bit, 1bit, 2bit로 구성되어 있는데 g(1bit)는 global table에 들어가는가를 나타내고, p(2bit)는 previlege level을 나타내고 나머지 13bit가 Selector로 사용이 된다.

Selector를 가지고 세그먼트를 지정하고 세그먼트를 지정한 것에서 다시 페이지를 찾도록 되어 있다. 

페이지 사이즈는 4KB or 4MB 선택 가능

----

<img src="./images/virtualmemory68.png" />

위 이미지는 Memory Address Translation 과정을 보여준다.

CPU에서 나오는 것은 가상 주소 -> 이 가상 주소에서 segment unit을 가지고 linear address로 바꾼다.(세그멘테이션) -> linear address를 가지고 페이지 테이블을 참조해서 physical memory로 변환(페이징) -> Physical memory 

----

<img src="./images/virtualmemory69.png" />

셀렉터를 가지고 LDT의 selector의 인덱스를 가지고 segment descriptor(페이지 넘버의 정보)를 가지고 offset과 합치면 linear address가 나온다. 이 linear address를 paging unit으로 넘겨주면

----

<img src="./images/virtualmemory70.png" />

여기서 2단계 페이징을 통해 4KB 페이지를 찾거나 아니면 4MB 페이지 일때는 page directory가 page table이 된다.

----

<img src="./images/virtualmemory71.png" />

TLB를 사용하고 있으므로 miss 됐을 때만 오버헤드가 있고 그렇지 않을 경우 2단계고 4단계고 1단계고 상관없이 TLB hit가 발생하면 그대로 오버헤드 없이 짧은 시간에 Access를 할 수 있다.

다단계 페이징을 하려면 반드시 TLB가 있다는 조건하에서 사용할 수 있다는 것을 잊지 말자.