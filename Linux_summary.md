# Linux

>커널, bash, gcc있으면 어떤 프로그램이든 만들 수 있다.

### bash
>bash 또한 하나의 프로그램이다.

- man bash : bash 명령어 모음
- ctrl+방향키 : word단위 이동
- echo : c언어의 printf와 유사  
  - echo hello world -> hello world # 띄어쓰기 인식 X
  - echo 'hello world' -> hello world # 띄어쓰기 인식 O
- log같은 파일의 뒷부분만 보고 싶을 때
- ctrl+a, home : 커서 맨 앞으로 이동
- ctrl+e, end : 커서 맨 뒤로 이동
- pause(stop) : ctrl+z
- log : 과거 기록
- cat : 파일 맨 앞부터 출력
- head : 파일의 맨 앞만 출력
- tail : 파일의 맨 뒤만 출력
- alias : 명령어 예약(c언어의 macro)
  - alias mnapp="nmap -Pn -A --oss"
  - alias opFile="/Desktop/m2450/..../[file]"
  - 항상 쓰는 alias는 스크립트 파일에 넣어놓아야 함
  - subl ~/.bashrc : script file
  - terminal을 새로 열 때 부터 적용됨
  - 바로 적용시키려면 source ~/.bashrc
- read : 변수 입력
  - read -p "Enter your name: " name<br>
  - Enter tour name: KIM HAN BIN<br>
  - echo $name
- COLORS=purple; echo $COLORS : 변수 저장
  - 띄어쓰기 주의할 것!!!! !!!!
  - 

- 함수 사용법
```
function subl(){ // function은 안써도 됨
if [["$#" -ne 1]]; then
    open -a "Sublime Text"
    return 0
fi
}
```

