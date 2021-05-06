# Paging

<img src="./images/virtualmemory48.png" />

세그먼트는 코드나 데이터 영역 같이 큰 영역을 한 덩어리로 다루기 때문에 Granularity(세분성)이 크다는 문제가 있다. = 세그먼트의 단위가 너무 크다. 이는 외부 단편화가 발생하게 되는 이유가 된다.

페이징은 세그먼트 매니징 모델을 일반화한 것. 페이징이 가지는 세그먼트와 가장 큰 차이는 페이지의 크기가 고정된다는 점이다. 즉, 세그먼트는 가변적인데 반해 페이징을 고정적인 크기를 가진다. 

Physical Memory를 똑같은 크기의 Physical Page( = Frame) 단위로 나누어서 인덱스를 붙히고 Virtual Memory도 Physical Memory와 똑같은 크기로 Page란 단위로 나눠 준다. 즉, Page 하나 당 Frame 하나가 해당

Virtual Memory는 프로세스마다 하나 씩 존재하므로 Physical Memory의 Frame보다 Virtual Memory의 Page의 수가 훨씬 많다. 

