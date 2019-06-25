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


