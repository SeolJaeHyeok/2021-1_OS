<img src="./Images/freespace01.png" />

프로세스가 힙 메모리 영역을 다루는 부분 



<img src="./Images/freespace03.png" />

페이징 시스템을 사용하면 페이지 테이블을 이용해서 가상메모리에 필요한 페이지를 그때그때 할당해줄 수 있다.

함수호출이나 로컬변수를 사용하게 되면 필요에 따라 stack이 페이지 단위로 할당된다.

운영체제에서 메모리를 할당할 때는 페이지 단위로 할당을 해야 하기 때문에 무조건 페이지 크기의 배수로 할당이 된다.

<img src="./Images/freespace04.png" />

<img src="./Images/freespace05.png" />

명시적으로 할당하고 반납하는 함수를 호출하게 되어 있는 경우 - C, C++

malloc같은 경우 할당할 수 있는 메모리 크기의 제한이 없다. 이럴 경우 페이지 단위로 할당되지 않는 것을 처리해야 한다. free는 포인터를 받지만 사이즈는 받지 않는다.



<img src="./Images/freespace06.png" />

 Malloc과 free를 구현하려면 먼저 메모리가 사용되고 있는 상태를 유지해야 한다.

각각의 사이즈의 정보를 유지하고 있어야 한다.

임의 사이즈의 메모리를 할당할 수 있게 허용을 하면 external fragmentation(외부 단편화 = 사용공간이 있지만 사용할 수 없는 현상) 문제가 발생 이를 해결해야 함

malloc이나 free가 프로세스의 성능에 영향을 줄 수 있으니 해결할 알고리즘을 생각해야 한다. 

<img src="./Images/freespace07.png" />

연속적이지 않은 메모리를 할당하다보면 전체적인 메모리는 충분히 있지만 따로따로는 메모리를 할당하지 못하는 경우가 생긴다. 

이를 해결해주려면 Compaction(압축)을 해야 한다.  = 메모리에 동작하고 있는 영역들을 한 쪽으로 옮겨줘야 한다. 이 방법은 굉장히 cost가 높은 방법

<img src="./Images/freespace08.png" />

heap영역에서 Fragmentation이 발생하면 충분한 메모리가 있음에도 할당할 수 없는 예

여러 포인터들 중 obj2와 array를 free 해주고 str2를 할당해주려 하니 external fragmentation이 발생

heap에서는 external Fragmentation이 일반적일텐데 구현에 따라서 internal Fragmentation이 발생할 수도 있다.

<img src="./Images/freespace09.png" />

malloc과 free를 구현하기 위해 Heap 영역의 메모리에 대한 Free List를 구현해야 한다. Free List는 Heap 영역에 남아있는 Free한 메모리를 나타내는 자료구조. 즉, Heap 자체를 Free List라는 자료구조로 나타내게 된다는 말

1. Free List는 Linked List가 메모리의 빈 공간들에 대한 정보를 가지고 있는 것이다. 만약 메모리가 할당이 되면 Free List가 쪼개진다(Split). 그 다음 리스트의 순서는 메모리의 address 순서로 유지
2. 각각의 할당된 메모리에는 헤더가 있어서 헤더에는 할당된 메모리의 사이즈를 기록 ex) 100byte를 할당했다면 100byte + 헤더의 크기가 할당된 것. 포인터는 헤더 다음 부분을 사용자에 리턴
3. Free space를 어떻게 할당할 것이냐는 알고리즘 필요

문제는 Linked List dynamic하게 크기를 바꾼다. Dynamic Data Structure는 메모리를 힙에다 할당을 해야 한다. 우리는 힙을 구현하려 하는데 힙을 구현하는 Linked List가 Dynamic Data Structure이기 때문에 힙에 구현이 되어야 한다.즉,  힙을 구현하기 위한 자료구조는 힙에 할당이 되어야 하니까 Free List는 할당되는 리스트하고 같이 힙에 있어야 한다.

<img src="./Images/freespace10.png" />

여기서 포인터는 실제 주소

노드는 Free List의 한 노드, Header는 할당되는 메모리 블록의 헤더

<img src="./Images/freespace11.png" />



<img src="./Images/freespace12.png" />

<img src="./Images/freespace13.png" />

<img src="./Images/freespace14.png" />

First Fit - 요청받은 메모리 크기를 할당할 수 있는 처음 노드에 할당을 한다. 

이 방법은 성능은 좋지만 External Fragmentation이 생기게 된다.

<img src="./Images/freespace15.png" />

Best Fit - 가장 알맞은 크기를 가진 노드에 할당을 하기 때문에 External Fragmentation이 가장 적다. 

이 방법은 최악의 경우 O(n)의 시간복잡도를 가진다. 즉, 요청받은 메모리 크기와 일치하는 노드가 없다면 모든 리스트를 다 찾아야 하기 때문에 대부분의 경우 O(n)의 시간 복잡도를 가진다.

<img src="./Images/freespace16.png" />

<img src="./Images/freespace17.png" />

기본적인 Free List 구현의 성능을 어떻게 높힐것인가

<img src="./Images/freespace18.png" />

1. 할당을 할 때 사이즈에 맞는 크기의 노드를 찾느냐 - Best Fit 
   - External Fragmentation이 적다
   - 속도가 느리다. 보통 O(n)
2. 가장 처음 노드에 할당을 하느냐 - First Fit
   - External Fragmentation이 크다
   - 속도가 빠르다. 보통 O(n)보다 빠름

3. 위 두 개의 방법의 중간 정도되는 방법 - Next Fit
   - 약간의 자료구조 변경이 필요
   - Free를 할 때 free 메모리가 연속해 있으면 합쳐줘야 하는데  free가 속도가 느린것을 개선하기 위해(O(n) -> O(1))
   - Bining - External Fragmentation을 줄이면서 할당을 빠르게 하는 방법, 구현이 복잡, Internal Fragmentation이 발생

<img src="./Images/freespace19.png" />

자료구조를 Circular List로 변경.

First Fit을 하지만 Split이 일어나게 되면 헤드의 위치(원래는 주소의 위치가 가장 큰 곳에 고정)를 다음 위치로 변경. 찾는 순서의 시작점이 변경

<img src="./Images/freespace20.png" />

<img src="./Images/freespace21.png" />

<img src="./Images/freespace22.png" />

<img src="./Images/freespace23.png" />

<img src="./Images/freespace24.png" />

Malloc의 속도를 향상시킬 수 있는 방법

1. 2의 제곱으로만 할당을 해주는 것 - External Fragmentation을 줄이지만 Internal Fragmentation이 늘어나고 속도면에서는 큰 이점이 없음
2. Binning

<img src="./Images/freespace25.png" />

<img src="./Images/freespace26.png" />

Binning - Free List를 같은 사이즈의 Bin으로 미리 나누어 놓는 것. Ex) 32byte 몇 개, 64yte 몇 개, 제일 크게는 페이지로 나누어 놓는 것

각 크기마다 미리미리 만들어 놓기 때문에 찾기가 쉬워지기 때문에 O(1)에 찾을 수가 있다.

만약 해당하는 크기의 Bin이 모두 할당이 되어서 없다면 더 큰 Bin을 나누어서 만들어주기 때문에 약간의 시간이 더 소요된다. 

<img src="./Images/freespace27.png" />

다음으로는 멀티 쓰레드가 Malloc을 할 경우에 대해서 생각해봐야 한다. 

멀티 쓰레드에서 Malloc을 쓰게 되면 Lock을 사용하기 때문에 성능이 떨어진다.

<img src="./Images/freespace28.png" />

그렇기 때문에 Lock을 사용하지 않고 Arena라는 구조를 사용한다.

Arena -  각 쓰레드마다 힙을 나눠서 주는 것. 여러 개의 Arena를 만들어 놓고 각 쓰레드가 Malloc을 위해서 사용하는 공간. 각각의 Arena에는 Free List가 따로따로 존재. 쓰레드가 생기면 사용할 Arena를 배정하므로 Lock을 사용할 필요 x

물론 Arena의 수보다 쓰레드가 많아지게 되면 문제가 생길 수 있다. 그렇기에 적절한 수를 유지해야 하는 필요성이 있다.

<img src="./Images/freespace29.png" />

<img src="./Images/freespace30.png" />

<img src="./Images/freespace31.png" />

메모리 누수가 생기는 경우

free를 안할 때 메모리 누수가 발생

<img src="./Images/freespace32.png" />

- Dangling pointer - Malloc을 하고 free까지 했는데 해당 포인터를 나중에 그냥 써버리는 경우 
  - 이렇게 한다면 어떻게 될 지 모르게 된다.

- Double free - free를 두 번 하는 경우
  - 일반적으로 Free list가 연산을 하게되므로 잘못되는 경우가 생길 수 있는데 마찬가지로 정확히 어떻게 될 지 모른다.



