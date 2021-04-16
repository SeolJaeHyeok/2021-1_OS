#### Goals of Our simple OS

- APIs for device access
  - Read from the keyboard
  - Read and write to a simple disk
  - Display text to the screen
- Ability to run a simple user program
- A basic command line for running programs
- 32-bit x86
  - IA32

----

#### x86 Registers(32bit)

- General purpose registers
  - EAX, EBX,ECX, EDX
- Stack register
  - ESP: Points to the top of the stack
    - The stack grows down, so this is the lowest address
  - EBP: Points to the bottom of the current stack frame
    - Not strictly necessary, may be used as a general purpose register
- Other registers
  - EIP: Points to the currently executing instruction
  - EFLAGS: Bit vector of flags
    - Stores things like carry (after addition), equals zero (after a comparison), etc.

----

<img src="./Images/makingos1.png" />

----

<img src="./Images/makingos2.png" />

%eax 와 같은 식으로 쓰게 되면 AT&T 포맷(유닉스에서 사용)이다. 이 포맷은 source-destination 방식으로 어셈블리어를 작성한다.(자세한 사항은 구글링 ㄱㄱ..^^)

----

<img src="./Images/makingos3.png" />

----

<img src="./Images/makingos4.png" />

우리는 Free Memory에 운영체제를 구현해야 하는 것

----

<img src="./Images/makingos5.png" />

하나의 메모리 주소는 한 바이트를 의미하는데 모든 CPU가 그런것은 아니고 인텔 아키텍쳐는 위와 같은 형식으로 구성

일반적으로 대부분의 경우 인텔 아키텍쳐와 호환이 되기 때문에 위와 같은 형식이 일반적이라고 생각해도 무방하다

[](브라켓)이 있으면 그 안에 들어가는 내용은 주소

----

<img src="./Images/makingos6.png" />

 멀티 바이트의 순서를 어떻게 정할 것인가를 정의 => 정해진 것이 아닌 우리가 정하면 된다.

----

<img src="./Images/makingos7.png" />

----

<img src="./Images/makingos8.png" />

frame은 하나의 화면을 뜻하고 buffer는 화면에 직접 뿌리는 것이 아닌 buffer에 입력하면 한번에 나타내주는 것을 말함. 

따라서 Console frame buffer는 모니터에 나타나는 한 화면을 나타낼 수 있는 메모리

----

<img src="./Images/makingos9.png" />

<img src="./Images/makingos10.png" />

Hello World를 화면에 출력한다는 것은 사실 frame buffer에 쓰기만 하는 것이고 frame buffer에서 화면에 출력하는 것은 비디오 카드가 해주는 것이다. 따라서 우리는 frame buffer에 써주기만 하면 된다.

----

<img src="./Images/makingos11.png" />

call : return address를 계산해서 스택에 push하고 해당 주소에 해당하는 함수로 jump

ret: 스택에서 return address를 가져와서 jump

----

<img src="./Images/makingos12.png" />

<img src="./Images/makingos13.png" />

스택은 push한 순서와 반대로 pop을 하게된다. 그렇기 때문에 argument를 스택에 push 할 때 뒤부터 넣어주어야지 pop을 할 때 정상적으로 가져올 수 있는 것이다. 

관행적으로, Return value는 EAX에 위치

----

**API화 해야하는 이유**

<img src="./Images/makingos14.png" />

----

<img src="./Images/makingos15.png" />

----

