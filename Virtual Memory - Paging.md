# Paging (Homework #3 공부)

<img src="./images/virtualmemory48.png" />

세그먼트는 코드나 데이터 영역 같이 큰 영역을 한 덩어리로 다루기 때문에 Granularity(세분성)이 크다는 문제가 있다. = 세그먼트의 단위가 너무 크다. 이는 외부 단편화가 발생하게 되는 이유가 된다.

페이징은 세그먼트 매니징 모델을 일반화한 것. 페이징이 가지는 세그먼트와 가장 큰 차이는 페이지의 크기가 고정된다는 점이다. 즉, 세그먼트는 가변적인데 반해 페이징을 고정적인 크기를 가진다. 

Physical Memory를 똑같은 크기의 Physical Page( = Frame) 단위로 나누어서 인덱스를 붙히고 Virtual Memory도 Physical Memory와 똑같은 크기로 Page란 단위로 나눠 준다. 즉, Page 하나 당 Frame 하나가 해당

Virtual Memory는 프로세스마다 하나 씩 존재하므로 Physical Memory의 Frame보다 Virtual Memory의 Page의 수가 훨씬 많다. 

----

<img src="./images/virtualmemory49.png" />

----

<img src="./images/virtualmemory50.png" />

여기서는 virtual memory가 physical memory보다 작은걸로 나와있는데 일반적으로는 virtual memory가 더 크다.

Segment table에는 Base와 Bound 모두 있었지만 Paging table에서는 크기가 모두 같기 때문에 Bound는 필요 없다.

----

<img src="./images/virtualmemory50.png" />

----

<img src="./images/virtualmemory51.png" />

일반적으로 페이지 테이블은 메모리에 저장이 되는데 사용자의 메모리 스페이스에 저장이 되면 안된다. 즉, 커널만 접근할 수 있는 메모리에 저장

----

<img src="./images/virtualmemory52.png" />

----

<img src="./images/virtualmemory53.png" />

Copy on Write를 가지고 fork()를 훨씬 빠르게 할 수 있다. 

Copy on Write란 Write를 하면 Copy를 하는 것을 말한다.

----

<img src="./images/virtualmemory54.png" />

----

<img src="./images/virtualmemory55.png" />

Paging을 사용하면 Segmentation의 이점을 가지면서 동시에 Granularity(세분성)를 조금 더 작게할 수 있으므로  Address Space를 조금 더 성기게 쓸 수 있다. 다시 말해, 아주 잘게 나눠 쓸 수 있다는 뜻

Internal Fragmentation을 페이지 크기보다 작게 제한할 수 있다.

페이지 크기가 모두 같고 페이지가 빈 곳을 못 들어가는 경우가 없으므로 Segmentation에서 나오는 External Fragmentation은 없다. 

----

<img src="./images/virtualmemory56.png" />

한 프로세스의 Page Table이 너무 거대해질 수 있다는 문제가 있다. 그러므로 대부분의 엔트리는 empty거나 invalid 상태일 것이다.

이렇게 된다면 Page Table이 전체 프로세스의 성능을 떨어트리게 될 것이다.