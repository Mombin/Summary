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

리눅스 실행 요소3
- rootfs(root file system)
- boot loader
- zImage(kernel)
