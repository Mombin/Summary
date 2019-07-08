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
  - COLORS=purple // COLORS에 purple 값을 넣어줌
  - COLORS= purple // 변수를 만들었는데 초기화를
    안했다고 인식(공백때문)
- which 파일명 : 위치 표시
- 변수를 사용(호출)할 때에는 $표시를 붙인다.
- no=0; while [ $no -lt 10 ]; do ((no=no+1)); printf "%02d\n" $no;done
- for name in *; do echo $name; done
- for name in *; //파일명 전체를 적는것과 같음
- for name in *; do echo $name; //파일목록이 화면에 나옴
- for name in *; do mv $name ${name}_01; done //파일명에 _01추가
- for name in *; do mv $name ${name%%\.mp3}.wav; done<br>  파일 전체를 wav로 변경
- 파일명의 글자를 인식해서 변경할수 있음 movie_2018->movie_2019
- ! : !359를 입력하면 history 목록 중 359명령어를 다시 실행함
- !! : 가장 최근의 명령어 수행
- pwd : 현재 디렉토리 보여줌
- ` : PATH=`pwd` -> pwd내용을 PATH에 넣음
- PATH1=$(pwd) : 위와 같은 기능
- $RANDOM : 랜덤 함수 생성, echo $RANDOM
- PATH=$PATH:/home/user/Desktop : 경로 설정 //맨 뒤 
  .bashrc파일에 넣어주어야 설정됨. 
- PATH=/home/user/Desktop:$PATH // 맨앞
- 우선순위는 PATH의 선언 순서에 따라 결정됨.
- 변수 입력
  - GAME="star craft"
  - echo $GAME //좋은 습관
  - echo ${GAME}s //->star crafts, 더 좋은 습관
  - echo $"{GAME}s" // 더더욱 좋은 습관
- echo "This   is    for testing" | tr -s [:space:] ' '
- -->This is for testing
- tr 'a-z' 'sdlfknsdlfknsdf' <<< "hello world"
- /sbin/ifconfig eth0 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}' // ip주소 출력
- rm -rf !(파일명) : 원하는 파일 제외하고 지우기
- 정규표현식(Regular expression) 
- export : 특정 변수의 값을 상속시켜서 다른 프로세스에서도 사용할 수 있게 함.
- xargs : 쉘 고수 되려면 공부해야함
- sleepN : N초만큼 sleep

```
#!/usr/bin/env bash
/* 맨 앞에 스크립트 구문 적는 법
    #!/bin/bash - 불안전
    #!/usr/bin/env bash
*/
# 파일내의 라인수를 카운트 
lines=0
#for i in $(find . -type f); do 
for i in *; do 
rowline=$(wc -l "$i" | awk '{print $1}');
file="$(wc -l "$i" | awk '{print $2}')"; 
lines=$((lines + rowline)); 
echo "Lines["$lines"] " "$file" "has "$rowline" rows.";
done && unset lines
```

##  함수 사용법
```
function subl(){ // function은 안써도 됨
if [["$#" -ne 1]]; then
    open -a "Sublime Text"
    return 0
fi
}
```

## if문 사용법
```
#!/bin/bash

# 도전 QUIZ Regular Expression
# 다음의 출력 결과가 나올 수 있도록 주어진 빈칸(☺︎)을 채우세요
# 정규식(Regular Expression) 비교를 이용하세요

num=$1
re='︎[0-9]'
if ! [[ $num =~ $re ]] ; then # if문 평가
   echo "error: Not a number" >&2; exit 1
else
    echo "is a number"
fi

# 실행결과
# user@ubuntu:~$  ./A05.sh 12
# is a number
# user@ubuntu:~$  ./A05.sh 1A
# error: Not a number
# user@ubuntu:~$
```
