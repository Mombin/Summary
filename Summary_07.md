<h1>Linux System Programming</h1>

1. Linux how to use
2. porting linux on board(porting)
3. device driver programming
4. application progrmming

임베디드에서의 System == System call

1. sudo -s : root(막강한 권한)계정으로 switching account
2. pw : mdsmds로 설정됨
3. ip가 안잡혀있는 eth8번 설정
4. ifconfig eth8 182.168.0.2
5. sudo chwon -R mds:adm exercise_lsp

>네트워크 구성도
![네트워크](./picture/06241124.gif)

>파일 컴파일,링커 개요
![컴파일](./picture/06241600.gif)

### BSS를 DATA와 분리시키고 0으로 초기화하는 과정이 .o파일 이후에 되는 이유

- int git[100];같은 전역변수가 생상됐을 때
  data영역에 생성되면 0이 100개가 생성되기 때문에 .o파일의 용량이 커지게 된다. 그래서 ram으로 복사할 때 0으로 초기화됨

리눅스 구성 3요소
- rootfs(root file system)
- boot loader
- zImage(kernel)

### POSIX
- 애플리케이션 인터페이스 규격
- 함수 이름을 통일한 것

### 라이브러리
- 정적 라이브러리
- 동적 라이브러리
- 공유 라이브러리

### 공유 라이브러리
- 공유 라이브러리의 여러 명칭 : shared libraries, shared objects, dynamic shared objects (DSOs), dynamically linked libraries (DLLs - if you're coming from a Windows background)
- LD_LIBRARY_PATH를 사용해 공유 라이브러리 경로 지정
  - export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
- /etc/ld.so.conf 를 사용해 공유 라이브러리 경로 지정
>경로 지정을 하지 않을 경우  
$ gcc -o main main.c -L/usr/local/lib -lmylib  
$ ./main  
$ ./main: error while loading shared libraries: libmylib.so: cannot open shared object file: No such file or directory
```
$ gcc -fPIC -c mylib.c
$ gcc -shared -WL,-soname,libmylib.so.1 -o libmylib.so.1.0.1 mylib.o
$ cp libmylib.so.1.0.1 /usr/local/lib
$ ln -s /usr/local/lib/libmylib.so.1.0.1 /usr/local/lib/libmylib.so
```

공유폴더 만드는 방법
```
[root@localhost ~]$ gcc -fPIC -c *.c
[root@localhost ~]$ gcc --shared -Wl,-soname,libfoo.so -o libfoo.so func1.o func2.o
[root@localhost ~]$ nm -D libfoo.so
[root@localhost ~]$ sudo ldconfig -n .
[root@localhost ~]$ gcc -o program program.o -L. -lfoo
[root@localhost ~]$ ldd program
```
```
[root@localhost ~]$ gcc -fPIC -c *.c
[root@localhost ~]$ gcc -shared -Wl,-soname,libfoo.so.1 -o libfoo.so.1.0 *.o
[root@localhost ~]$ ln -s libfoo.so.1.0 libfoo.so.1
[root@localhost ~]$ ln -s libfoo.so.1 libfoo.so
[root@localhost ~]$ LD_LIBRARY_PATH=`pwd`:$LD_LIBRARY_PATH ; export LD_LIBRARY_PATH
[root@localhost ~]$ su
[root@localhost ~]# cp libfoo.so.1.0 /usr/local/lib
[root@localhost ~]# /sbin/ldconfig
[root@localhost ~]# ( cd /usr/local/lib ; ln -s libfoo.so.1 libfoo.so )
```
- fPIC 옵션은 프로세스의 가상 주소 공간 안의 다른 지역에 로드될 수 있도록 코드를 컴파일 한다. 실행 될 때 그 위치가 정해지기 때문에 가상 공간 안에 고정된 위치를 가지면 안 된다.
- shared 옵션은 공유 라이브러리를 만든다는 옵션이다.
- Wl 옵션은 gcc에게 콤마로 부분된 다음 옵션을 링커에게 알려주기 위한 것이다.
- soname,libfoo.so.1 라이브러리에 대한 soname을 고정시키는 부분이다.
```
LD_LIBRARY_PATH=`pwd`:$LD_LIBRARY_PATH ; export
```
- LD_LIBRARY_PATH: 현재 폴더를 공유 라이브러리 탐색 경로에 넣어준다.
- ldconfig은 /etc/ld.so.conf에 지정된 디렉토리를 찾아다니면서 발견된 각각의 동적 라이브러리들에 대해서 그 라이브러리의 soname에 의해서 불리어지는 심볼릭 링크들을 만들어낸다. 위에 soname에 주어진 이름으로 새로운 심볼릭 링크를 만든다.공유 라이브러리를 이용해서 컴파일을 할 경우 .so를 먼저 경로에서 찾고 없으면 .a를 찾아 주소값을 결정한다.



### 하드링크 심볼릭 링크
- 하드링크 
    $ ln [원본 파일명] [대상 파일명]
  - 하나의 파일에 여러 개의 이름을 부여함,
  - 파일을 없애려면 링크로 생성된 링크 파일을 모두 지워야 함
- 심볼릭 링크
    >$ ln -s [원본 파일명] [대상 파일명]
  - 포인터와 같은 개념(윈도우 바로가기 아이콘)
  - 링크로 생성된 파일에 내용이 존재하지 않고 각각의 i-node를 가진 또 다른 파일이 어디를 가리키고 있는지 알려주는 역할
  - 원본파일명이 바뀌면 사용하지 못함


![](./picture/06251700.gif) 


## System Call

1. system call
2. call <i>library</i> function
3. 함수의 인자값 처리 및 EAX 레지스터에 시스템 콜 등록
4. 0x80(intel) or SWI(arm)
5. Processor가 커널 모드(arm에서는 SVC모드)로 전환
6. 커널이 Interrupt에 대한 system call 루틴을 호출
7. 현재 레지스터 값들을 스택에 저장
8. 넘겨받은 인자값들 확인
9. system call 루틴 실행
10. 스택에 저장된 레지스터값들 복구
11. Processor가 사용자 모드(이전 모드)로 전환
12. system call 함수로부터 return

- Systemcall은 User Level이 아니어도 부를 수 있다. Supervisor모드에서도 호출 가능
- SWI(software interrupt) : User Level에서 Kernel Level로 이동하게(switching mode) 하는 instruction

>리눅스 파일 권한
![](picture/linux_ls.png)

-rwx-|-xr-|-x-  
--|---|---
User|Group|Other

- r: read
- w: write
- x: execute

### File open
```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcnt1.h>
/*open fopen 차이*/
int fd = open(const char *pathname, int flags); // low Level F/O function, type1
int fd = open((const char *pathname, int flags, mode_t mode);// low Level F/O function, type2
FILE *pf = fopen(const char *pathname, int flags, mode_t mode);// high Level F/O function
//flag : O_RDONLY, O_WORLD, O_RDWD, O_CREAT, O_EXCL...
```

### Data & Meta Data
- Data: 파일의 내용
- Meta Data: 파일의 정보

## Process
>Process : 프로그램이 실행되어 돌아가고 있는 상태

>Process State  
![](./picture/process_state.jpg)

<b>프로세스 관련 쉘 명령</b>  
- top : 프로세스 증 제일 cpu가 많이 쓰이는 프로세스
- ps -(e,f,l,L) : 현재 프로세스를 보여줌
- pstree : 프로세스를 트리 형태로 보여줌
- fg/bg: fore ground/ back ground

- execl : 현재 실행되고 있는 프로세스를 덮어 씌워서 실행하는 함수

### fork()  
- 부모와 같은 자식을 낳는 프로세스 
- fork() process는 부모와 자식이 같은 file table, text를 공유해서 사용한다.
- 오직 자식의 pid만 0, 부모는 0>pid

## Scheduler
- CFS(Completely Fairness Scheduler)
- RT(Real Time)
- Deadline
  
>우선순위  
![](picture/priority.png)
CFS에서는 NI값을 이용해 우선순위를 동적으로 바꿀 수 있다.  
- 숫자가 낮을수록 우선순위가 높다.
- realtime priority에서는 숫자가 높을수록 우선순위가 높다.
- -40의 priority는 RT schedule을 따름
- ps -l : PRI(-40~99)는 cfs를 따름
- top : PR(-20~30)는 cfs를 따름
- ps -l의 80 == top의 20

### Realtime에서 우선순위가 같을 때 우선순위 결정 방법
- SCHED_FIFO: 먼저 온 프로세스가 무조건 우선
- SCHED_RR: 적당하게 번갈아가며 우선순위 책정  
  
## Thread
>리눅스의 쓰레드는 `가벼운 프로세스(Light Weight Process) `이다.

- 쓰레드간에는 프로세스끼리와는 달리 Cache의 내용을 공유할 수 있다.

[Thread 참고자료](https://reakwon.tistory.com/56)

## Difference Process & Thread
>Process
![](picture/process.png)  
각 프로세스간 독립적으로 작동한다.

>Thread
![](picture/thread.png)
Code, Data, Heap을 공유하기 때문에 서로 의존적이다.

### 쓰레드의 생성
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void*(*start_routine)(void*), void *arg);
/*
- *thread : id of thread
- attr : attribute thread, NULL if default
- start_routine : 쓰레스 수행을 시작할 위치, 쓰레드 함수 이름
- arg: 생성될 스레드에 전달될 인자들(없으면 NULL)
*/
```
- 쓰레드의 Schedule은 부모의 Schedule policy를 그대로 따른다.  

### pthread_join()   
```c
int pthread_join (pthread_t thread, void *value_ptr);
```
- 지정된 id의 스레드가 종료할 때까지 이 함수를 호출한 스레드를 정지(suspend)시킨다.
- `여러개의 스레드가 한 스레드를 종료시킬 수는 없다.`
- pthread_exit() 으로 종료되면 리턴값을 인자로 받아 리턴한다.

![0627 그림 요약](./picture/06271700.GIF)

- exit()은 process나 thread에서 실행되면 process를 죽이는 함수이다.
- pthread_exit()은 thread만 죽일 수 있는 함수.

- process(system) resource : 파일, RAM에 잡힌 자원 등..
- thread specific resource : 스레드에 있는 FDT(file description table), .stack 등의 자원은 thread가 죽으면 pthread_join()이 해제한다.

- thread가 attached state일 때에는 pthrad_join()에 의존하나 detached state일 때에는 자체적으로 해제한다.

### 세마포어(Semaphore)


- 세마포어 값에 따라 공유된 자원의 접근을 제한하는 기술
- 0,1,2 등으로 사용하나 그 이상의 세마포어 값도 가능하다.
- 세마포어 사용시 Deadlock발생에 주의한다.
  - Deadlock:서로가 상대의 자원을 원하는 경우
  
### MUTEX(`Mut`ually `Ex`clusive Semaphore)

- Critical Section을 가진 쓰레드들의 Runnig Time이 서로 겹치지 않게 각각 단독으로 실행되게 하는 기술
- 키 값이 1일 세마포어와 비슷함
- Task 삭제 보호 : (뮤텍스가 unlock되지 않고)삭제된 뮤텍스를 사용할 수 있게 하는 기능

### MUTEX vs Semaphore
1) Semaphore는 Mutex가 될 수 있지만 Mutex는 Semaphore가 될 수 없습니다.
(Mutex 는 상태가 0, 1 두 개 뿐인 binary Semaphore)

2) Semaphore는 소유할 수 없는 반면, Mutex는 소유가 가능하며 소유주가 이에 대한 책임을 집니다. (Mutex 의 경우 상태가 두개 뿐인 lock 이므로 lock 을 ‘가질’ 수 있습니다.)

3) Mutex의 경우 Mutex를 소유하고 있는 쓰레드가 이 Mutex를 해제할 수 있습니다. 하지만 Semaphore의 경우 이러한 Semaphore를 소유하지 않는 쓰레드가 Semaphore를 해제할 수 있습니다.

4) Semaphore는 시스템 범위에 걸쳐있고 파일시스템상의 파일 형태로 존재합니다. 반면 Mutex는 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 Clean up된다.

>우선순위 역전현상
![](./picture/priority_inversion.png)

1. task3이 공유자원을 액세스 위해 binary semaphore를 가지고 수행
2. Scheduler에 의해 task1이 수행
3. task1은 task3가 먼저 획득한 Semaphore를 얻으려(take)하고, task3가 그 Semaphore를 반환할 때(give)까지 Waiting 상태로 됨
4. Scheduler에 의해 task3 수행
5. Scheduler에 의해 task2가 수행
이 시점에서 task1의 priority가 task2보다 높음에도 불구하고 task2가 먼저 수행 되는 Priority Inversion 문제가 발생
6. task2의 수행이 종료 되면, task3가 재 수행
7. task3가 Semaphore 반환
8. task1 수행

## 우선순위 역전현상 해결 방법

### Priority Inheritance 처리 방법

>Priority Inheritance method
![](./picture/Priority_Inheritance.png)

1. task3이 공유자원을 액세스 위해 binary semaphore를 가지고 수행
2. Scheduler에 의해 task1이 수행
3. task1은 task3가 먼저 획득한 Semaphore를 얻으려(take)하고, task3가 그 Semaphore를 반환할 때(give), Waiting 상태로 됨
4. Scheduler에 의해 task3 수행되는데, task3의 priority를 task1이 waiting 상태인 동안, task1의 레벨로 높임(Priority Inheritance)으로써 task3의 priority가 task2보다 높기 때문에 preemption 없이 수행 
5. Task3이 Semaphore를 반환
6. Task1이 Semaphore를 얻어서 수행됨과 동시에, task3의 priority가 다시 예전의 값으로 복귀
7. Task1 수행
8. Task2 수행



### Priority Ceiling 처리 방법

>Priority Ceiling method
![](./picture/priority_ceiling.png)

- 어떤 쓰레드가 priority ceiling 속성을 가지는 뮤텍스를 Lock설정 하면, 쓰레드의 우선순위는 뮤텍스의 우선순위 올림 값으로 자동으로 상승
- 단, 쓰레드의 우선순위가 뮤텍스의 우선순위 올림 값보다 작은 경우에만 우선순위 변경됨
- 올림 값 보다 더 높은 우선순위 가지는 쓰레드가 잠글 경우 규칙 깨짐 
- 따라서 뮤텍스를 이미 잠근 쓰레드는 뮤텍스를 잠그려고 시도하는 다른 쓰레드 들에 의해 선점되지 않고 자신의 작업을 종료 가능

## IPC(Inter Process Communication)

- 프로세스 간의 통신 정의

### POSIX MESSAGE QUEUE

>Posix Message Queue
![](./picture/msg-queue.png)

### SysV MESSAGE QUEUE
>SysV Message QUEUE
![](./picture/sysv_msgqueue.jpg)


### SHARED MEMORY
- 각 프로세스가 하나의 커널 메모리를 공유하는 기술
- 임의의 프로세스가 공유 메모리 데이터를 쓰면 다른 프로세스가 메모리의 내용을 읽어 각 프로세스간에 통신이 이루어진다.
>Shared Memory
![](./picture/shared_memory.png)

### SIGNAL

```bash
$ kill -9 1234 // $[command] -sigNo PID
```
 Signal Name|Number|Description
---|---|----
 SIGHUP|1|Hangup (POSIX)
 SIGINT|2|Terminal interrupt (ANSI)
 SIGQUI|3|Terminal quit (POSIX)
 SIGILL|4|Illegal instruction (ANSI)
 SIGTRAP|5|Trace trap (POSIX)
 SIGIOT|6|IOT Trap (4.2 BSD)
 SIGBUS|7|BUS error (4.2 BSD)
 SIGFPE|8|Floating point exception (ANSI)
 SIGKILL|9|Kill(can't be caught or ignored) (POSIX)
 SIGUSR1|10|User defined signal 1 (POSIX)
 SIGSEGV|11|Invalid memory segment access (ANSI)
 SIGUSR2|12|User defined signal 2 (POSIX)
 SIGPIP|13|Write on a pipe with no reader, Broken pipe (POSIX)
 SIGALRM|14|Alarm clock (POSIX)
 SIGTERM|15| Termination (ANSI)
 SIGSTKFLT|16|Stack fault
 SIGCHLD|17|Child process has stopped or exited, changed (POSIX)
 SIGCONT|18|Continue executing, if stopped (POSIX)
 SIGSTOP|19|Stop executing(can't be caught or ignored) (POSIX)
 SIGTSTP|20|Terminal stop signal (POSIX)
 SIGTTIN|21|Background process trying to read, from TTY (POSIX)
 SIGTTOU|22|Background process trying to write, to TTY (POSIX)
 SIGURG|23|Urgent condition on socket (4.2 BSD)
 SIGXCPU|24|CPU limit exceeded (4.2 BSD)
 SIGXFSZ|25|File size limit exceeded (4.2 BSD)
 SIGVTALRM|26| Virtual alarm clock (4.2 BSD)
 SIGPROF|27| Profiling alarm clock (4.2 BSD)
 SIGWINCH|28| Window size change (4.3 BSD, Sun)
 SIGIO|29| I/O now possible (4.2 BSD)
 SIGPWR|30| Power failure restart (System V)