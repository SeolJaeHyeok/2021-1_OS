# Threads

프로세스는 프로그램이 메모리에 적재되어 동작하고 있는 상태다. 프로세스가 있으면 프로그램을 동작할 수 있고 프로세스들은 서로 통신할 수 있고 대부분의 일들은 프로세스를 가지고 할 수 있다.

하지만 프로세스만 가지고 충분하지 않은 경우가 있다. 이런 경우가 환경이 변했다고 할 수 있는다.

예를 들어 서버가 클라이언트를 많이 가지고 있는 경우 웹서버의 경우를 생각할 수 있다.

웹서버의 경우 외부에서 많은 요청이 들어오게 되는데 그 요청을 처리한 실제로는 쓰레드가 있겠지만 프로세스만 있다고 가정할 때 프로세스를 요청마다 하나씩 만들어줘야된다. 그렇게 되면 시스템 프로세스가 굉장히 많은 상태가 되고 context switching이 자주 일어나게 되고 시스템의 오버헤드가 커지게 된다.

두 번째는 Cpu 코어가 점점 늘어남에 따라 컴퓨팅 파워가 커졌다. 그래서 프로세스를 수행하는걸 가지고는 코어가 늘어난 CPU의 컴퓨팅 파워를 충분히 사용하지 못한다. 다시 말해, CPU 안에 코어가 여러개 있으니 동시에 여러 작업을 할 수 있는데 프로세스는 하나밖에 사용하지 못하니까 즉 한 코어당 프로세스 하나만 사용할 수 있다는 말이기에 CPU가 코어가 늘어나도 컴퓨팅 파워를 충분히 사용하지 못한다는 말이다.

--------------------

#### 문제점 

- 프로세스를 생성할 때 (UNIX 계열에서는) fork()를 사용하는데 이 방식은 parent의 address space를 포함하여 모든 컨텍스트를 전부 다 카피를 해야한다. 즉, 처음에 생성할 때 메모리 액세스를 많이 하기 때문에 굉장히 오래 걸린다.
- 프로세스는 각자의 address space를 독립적으로 가지고 있기 때문에( = 같은 주소라고 해도 프로세스마다 설정된 메모리가 다르다.) IPC(프로세스 간의 통신)가 상당히 불편하다. 

---------

Threads는 프로세스와 비슷하기 때문에 경량 프로세스(Light-Weight-Process)라고 부르기도 한다. 

그렇다면 프로세스와 다른 점은 무엇일까?

- 쓰레드는 Address space를 공유한다 => Address space를 새로 생성하지 않으니까 시간이 훨씬 줄어든다.
- 멀티쓰레드라는 것은 control flow가 두 개 이상 있다는 것을 의미한다. 즉, 동시에 두개 이상의 작업을 수행할 수 있다는 말

--------------

#### 장점

- 자원(Address space) 공유가 가능, IPC가 필요 없다.
- 만드는 속도, context switch가 빠르다. 
- 멀티코어 CPU가 도입이 되면서 코어를 여러 개 이용할 수 있는 능력이 요구되는데 Threads를 이용하면 가능하다.

 #### Single-Threaded Process and Multi-Threaded Process

<img src="./images/Thread1.png" />

Resister와 Stack은 Thread마다 따로 존재해야하고 나머지 Code영역, Global Data영역, File Descripotors 영역들은 공유한다.

-------

#### Thread 구현 방신

1. User threads
2. Kernel threads

- User level threads : 라이브러리 형태로 되어 있기 때문에 컴파일할 때 해당 라이브러리를 같이 링크해서 사용해야 된다. 
- Kernel threads : 커널이 지원하는 Thread. 대부분의 운영체제가 지원하는 Thread

Use level thread는 Kernel thread와 맵핑이 가능하다.

#### Multithreading Models

- Many-to-One
- One-to-One
- Many-to-Many

Many-to-One : 여러 개의 User thread와 kernel thread 하나가 맵핑이 되는 경우 

One-to-One : User thread 하나가 kernel thread 하나와 맵핑이 되는 경우 

Many-to-Many : 여러 개의 User thread와 kernel thread 여러 개가 맵핑이 되는 경우. 단, User-level-thread의 수는 kernel-level-thread보다 많거나 같다. 

------

#### POSIX Pthreads

어렵드아. 교재 가지고 하나하나 이해 해가면서 해야겠다..

