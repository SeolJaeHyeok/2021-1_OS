# Threads

프로세스는 프로그램이 메모리에 적재되어 동작하고 있는 상태다. 프로세스가 있으면 프로그램을 동작할 수 있고 프로세스들은 서로 통신할 수 있고 대부분의 일들은 프로세스를 가지고 할 수 있다.

하지만 프로세스만 가지고 충분하지 않은 경우가 있다. 이런 경우가 환경이 변했다고 할 수 있다.

예를 들어 서버가 클라이언트를 많이 가지고 있는 경우 웹서버의 경우를 생각할 수 있다.

웹서버의 경우 외부에서 많은 요청이 들어오게 되는데 그 요청을 처리한 실제로는 쓰레드가 있겠지만 프로세스만 있다고 가정할 때 프로세스를 요청마다 하나씩 만들어줘야된다. 그렇게 되면 시스템 프로세스가 굉장히 많은 상태가 되고 context switching이 자주 일어나게 되고 이는 시스템의 오버헤드가 커지게 되는 결과로 이어진다.

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

- 자원(Address space) 공유가 가능, IPC가 필요 없다. 물론 동기화는 따로 처리해줘야 한다.
- 만드는 속도, context switch가 빠르다. 
- 멀티코어 CPU가 도입이 되면서 코어를 여러 개 이용할 수 있는 능력이 요구되는데 Threads를 이용하면 가능하다.

----

 #### Single-Threaded Process and Multi-Threaded Process

<img src="./images/thread1.png" />

Resister와 Stack은 Thread마다 따로 존재해야하고 나머지 Code영역, Global Data영역, File Descripotors 영역들은 공유한다.

-------

#### Thread 구현 방법

1. User threads
2. Kernel threads

- User level threads : 라이브러리 형태로 되어 있기 때문에 컴파일할 때 해당 라이브러리를 같이 링크해서 컴파일해야 된다. 
- Kernel threads : 커널이 지원하는 Thread. 대부분의 운영체제가 지원하는 Thread

Use level thread는 Kernel thread와 맵핑이 가능하다.

#### Multithreading Models

- Many-to-One
- One-to-One
- Many-to-Many
- Two-level

Many-to-One : 여러 개의 User thread와 kernel thread 하나가 맵핑이 되는 경우,  ex) Solaris Green Threads, GNU Portable Threads

One-to-One : User thread 하나가 kernel thread 하나와 맵핑이 되는 경우 

Many-to-Many : 여러 개의 User thread와 kernel thread 여러 개가 맵핑이 되는 경우. 단, User-level-thread의 수는 kernel-level-thread보다 많거나 같다. 

Two-level Model: M:M와 O:O을 섞어서 사용하는 경우

------

#### POSIX Pthreads

POSIX의 표준 API, 구현이 아닌 명세

기본적으로 Pthread는 User-level-thread인데 어떻게 구현되는지는 운영체제마다 다르다. Ex) 리눅스의 경우 pthread(user-level-thread) 가 kernel-thread와 자동으로 매핑이 되는데 이는 pthread가 커널 스레드라는 뜻이 아니라 구현할 때 자동으로 pthread와 kernel-thread를 매핑하도록 만들어 줬기 때문이다

**Pthread API**

프로세스의 main 함수 안에서 호출 

pthread_attr_init() : Attribute를 초기화 하는 함수

pthread_create() : 새로운 thread를 만드는 함수

pthread_exit(): 현재 thread를 종료하는 함수

pthread__join(): 다른 thread가 종료되기를 기다리는 함수

위 함수들은 프로세스를 만들 때 사용하는 fork(), exit(), wait()와 비슷한 것을 확인하지만 조금 다른 것을 확인할 수 있다.

<img src="./images/thread2.png" />

----

#### Linux Threads

clone()이라는 API를 통해 새로운 threads를 만드는데, 리눅스에서는 thread라고 부르지 않고 task라고 부른다.

Fork()와 다르게 커널에 필요한 코드, 스택을 할당하는 작업을 하는데 메모리를 카피하지 않는다. 

**Thread Oddities**

<img src="./images/thread3.png" />

----

#### Advanced Threading

Thread를 미리 만들어 놓는다는 뜻으로 Thread Pool을 만들어 Thread Pool 안에서 thread를 가져와 사용하는 것을 의미한다.

thread_create를 했을 때 thread를 새로 생성할 필요 없이 기존에 존재하던 thread 중에서 빈 것을 가져와 사용할 수 있으니 생성에 필요한 시간이 절약되는 장점이 있다.

또한 Thread를 너무 많이 만들게 되면 Context switching이 많이 일어나 overhead가 일어나게 되고 CPU 수보다 thread가 많아지게 되면 성능 상 문제가 있을 수 있기 때문에 미리 Thread를 만들어놔 그것을 방지할 수 있게 된다.

----

#### Thread Local Storage

Thread들은 address space를 공유하기 때문에 Code, Global Data, File Descriptors 등을 공유한다. 하지만 자신만의 global data를 사용하고 싶은 경우가 있을 수 있다. 또한 자신의 local 변수를 만들어 한번 수행하면 없어지기 때문에 TLS를 사용해서 해결할 수 있다.

즉, TLS는 한 쓰레드 내에서만 글로벌 변수로 사용되는 것이다. 이는 static 변수랑 비슷하다고 할 수 있다.

----

#### OpenMP

----

#### Processes vs Threads

**Thread 장점**

프로세스보다 멀티 코어를 활용할 수 있고, 빠르게 생성할 수 있고 Context Switching도 적게 일어나기 때문에 Overhead도 적다는 점이 Process보다 나은 점이다.

**Thread의 단점은 곧 Process의 장점**

**Processes 장점**

만약 하나의 Process에서 오류가 발생해서 그 Process가 종료된다 한들 다른 Process에는 아무런 영향이 없다. 즉, Protection이 보장된다. 

<img src="./images/thread4.png" />