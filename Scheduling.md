#### Setting the Stage

N개의 CPU가 있고, P개의 프로세스/쓰레드가 있다고 가정

그렇다면 우리는 

- 프로세스를 어떤 순서로 스케줄링해야 되는가?
- 각 프로세스를 실행할 CPU를 무엇인가?

를 결정해야 한다.

----

#### Factors influencing Scheduling

- 프로세스의 특성
  - CPU 중심 작업인가 I/O 중심 작업인가?
  - 프로세스가 제약조건을 가지고 있는가?
  - 프로세스가 예측가능한가?
- 머신(하드웨어와 운영체제)의 특성
  - CPU의 개수가 몇개인가?
  - 운영체제가 선점을 허용하는가?
  - CPU에 의해 메모리가 공유가 어떻게 이루어지는가?
- 사용자의 특성
  - 상호작용이 일어나는 프로세스인가?
  - 프로세스가 백그라운드 작업인가 포어그라운드 작업인가?

----

<img src="./Images/Scheduling1.png" />

스케줄러는 크게 스케줄러와 디스패쳐로 생각할 수 있다. 

스케줄러는 어떤 프로세스를 선택할 것인가 하는 정책(policy)가 있는데 그 policy를 말하는게 스케줄러다. 즉, 레디 큐의 어떤 프로세스를 선택하는가를 정하는 것이 스케줄러고 선택된 프로세스를 CPU에 올려 수행을 해주는 것을 디스패쳐라고 한다.

----

<img src="./Images/Scheduling2.png" />

디스패처는 수행된 정책에 따라 프로세스를 CPU에 올려주는 일을 한다. 디스패처의 가장 중요한 작업은 Context Switching을 해주는 것이다.

Context Switching : 사용자의 Context를 저장 => 유저모드에서 커널모드로 변환 =>  다음 수행할 프로세스의 Context를 복원 => 커널모드에서 유저모드로 변환

Context Switching overhead가 많으면 성능 상의 문제가 있을 수 있다. 그렇기에 이를 줄이게 된다면 퍼포먼스가 높은 운영체제라고 할 수 있다. 

----

<img src="./Images/Scheduling3.png" />

----

<img src="./Images/Scheduling4.png" />

CPU Burst가 일어나는 이유 : 어떤 프로세스가 CPU를 간헐적으로 사용하다가 폭발적으로 사용하게 되면 발생

----

#### Scheduling Optimization Creteria

<img src="./Images/Scheduling5.png" />

----

#### Classic Scheduler(교재 4)

#### Turnaround Time 중시 => Completion(완료 시간) - arrival(도착 시간)

- First Come, First Serve(FCFS) 
  - 먼저 온 것을 먼저 서비스 해준다.
  - FIFO 큐에 프로세스가 저장된다.

<img src="./Images/Scheduling6.png" />

<img src="./Images/Scheduling7.png" />

정책이 바뀐 것이 아닌 단지 도착 순서만 바뀌었을 뿐인데 turnaround time이 눈에 띄게 짧아진 것을 확인할 수 있다. 이는 좋은 정책이라고 말할 수 없다.

- Shortest Job First(SJF)
  - 작업 길이가 짧은 것을 우선적으로 처리하는 정책

<img src="./Images/Scheduling8.png" />

하지만 Arrival time이 다를 경우 Preempt를 시키지 않으면 FCFS와 같은 결과를 낳게 된다.

그래서 나온 것이 

- Shortest Time-To-Completion First(STCF)
  - 최소 잔여시간 우선 정책 == 선점형 최단 작업 우선(PSJF)

<img src="./Images/Scheduling9.png" />

----

#### Response Time 중시 => First run time(처음 스케쥴된 시간) - arrival time(도착 시간) 

<img src="./Images/Scheduling10.png" />

- Round Robin(RR)
  - Time slice(a.k.a. scheduling quantum)에 따라 시간을 쪼개 돌아가면서 프로세스를 수행하는 것  => 공정성 높음, 응답시간 빠름
  - 기본적으로 CPU가 타이머를 제공해야 하고 타이머 인터럽트가 발생할 때마다 스케줄러가 동작해야 한다.

<img src="./Images/Scheduling11.png" />

----

#### RR vs STCF

<img src="./Images/Scheduling12.png" />

<img src="./Images/Scheduling13.png" />

----

#### Time Slice를 어떻게 설정해야 되는가

- Time slice를 얼마로 설정하느냐에 따라 응답 시간이 달라 지고 Context Switching도 달라진다.
- Time slice가 짧을수록 응답시간 기준 RR의 성능은 좋아지지만, Context Switching이 너무 많이 일어나게 돼 전체 성능에 영향을 줄 수 있다.
- 컴퓨터나 CPU의 성능이 좋아지면 좋아질수록 Time Slice가 빨라지는 경향이 있다. 

<img src="./Images/Scheduling14.png" />