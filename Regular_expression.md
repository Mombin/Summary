##손에잡히는정규표현식  

###참고사이트  
>  
> https://regexr.com/
> https://regex101.com/
> http://www.regexper.com/  

###정규표현식이란?
텍스트를 찾고 조작하는데 쓰는 문자열.
☞ 이 책에 나오는 예제들은 모두 직접 실행해 보기를 강력히 권한다.

###특수문자  
>   
> 마침표(.) : **아무** 문자 하나와 일치한다  
> 역슬래시(\\) : 메타 문자들을 이스케이프 하는 데 사용하는 메타 문자[^1]다.  
> 하이픈(-) : 특수 문자가 아니고 일반 문자임. 따라서, 굳이 역슬래시(\\)을 입력하지 않아도 됨.  
> 캐럿(^) : 캐럿(^) 문자는 이 문자 바로 뒤에 있는 문자나 범위 뿐만 아니라 집합 안에 있는 문자나 범위를 모두 제외한다.  
> 대괄호([]) : 집합의 시작과 끝을 나타내는 메타 문자  
>   


~~~
예문
sales1.xls
orders3.xls
sales2.xls
sales3.xls
apac1.xls
europe2.xls
na1.xls
na2.xls
sa1.xls
ca1.xls
sam.xls
~~~
  
~~~
예문
<BODY BGCOLOR="#336633" TEXT="#FFFFFF" MARGINWIDTH="0" TOPMARGIN="0" LEFTMARGIN="0"> 
~~~
  
~~~
예문
"101", "Ben", "Forta"
"102", "Jim", "James"

"103", "Roberta", "Robertson"
"104", "Bob", "Bobson"
~~~
  
###공백 메타 문자
~~~
 [\b] 역스페이스
 \f    페이지 엄김(form feed)
 \n   줄바꿈
 \r    캐리지 리턴
 \t    탭
 \v   수직 탭
~~~

###숫자 메타 문자   
~~~   
 \d    숫자 하나( [0-9] 와 같다 )  
 \D    숫자를 제외한 문자 하나( [^0-9] 와 같다 )  
~~~

~~~
예문  
var Array = new Array();  
...  
if (myArray[0] == 0) {  
    ...  
}  
~~~

###영숫자 메타 문자   
~~~
 \w    대소문자와 밑줄을 포함하는 모든 영숫자( [a-zA-Z0-9_] 와 같다 )  
 \W    영숫자나 밑줄이 아닌 모든 문자( [^a-zA-Z0-9_] 와 같다 )  
~~~

###공백 메타 문자  
~~~
 \s    모든 공백 문자 ([\f\n\r\t\v] 와 같다)  
 \S    공백 문자가 아닌 모든 문자 ([^\f\n\r\t\v] 와 같다)  
~~~

☞ \s \S 에 역스페이스 메타 문자인 [\b] 는 포함되지 않는다  
  
###16진수(HEX) 사용하기
16진수 값은 앞에 \x 을 붙여 표시한다. 즉, \x0A는 줄바꿈 문자가 되며 \n과 같다

~~~
예문
Send personal email to ben@forta.com or ben.forta@forta.com. For questions about a book use support@forta.com. 
If your message is urgent try ben@urgent.forta.com. Feel free to send unsolicited email to spam@forta.com (wouldn't it be nice if it were that simple, huh?).
~~~

###? 의 활용
물음표(?)는 자기 앞에 있는 문자가 없거나 그 문자가 하나만 있는 경우 일치한다. 여기서는 s인데, https?://는 http://나 https://와 일치하지만, 그외에는 일치하지 않는다.

~~~
예문
The URL is http://www.forta.com/, to connect securely use https://www.forta.com/ instead.
~~~

###범위 구간 찾기
~~~
예문  
<BODY BGCOLOR="#336633" TEXT="#FFFFFF" MARGINWIDTH="0" MARGINHEIGHT="0" TOPMARGIN="0" LEFTMARGIN="0">   
~~~

~~~
예문
4/4/03
10-6-2004
2/2/2
01-01-01
~~~


###최소 구간 찾기
~~~
예문  
1001: $496.80
1002: $1290.69  
1003: $26.43  
1004: $613.42  
1005: $7.61  
1006: $414.90  
1007: $25.00  
~~~

###과하게 일치하는 상황 방지하기
~~~
예문
This offer is not available to customers living in <B>AK</B> and <B>HI</B>.
~~~

###탐욕적 수량자와 게으른 수량자  
~~~
탐욕적 수량자    게으른 수량자  
*                 *?
+                +?
{n,}             {n,}?
~~~

그림 1. 탐욕적 수량자 이용의 예  

###위치 찾기(postion matching)  
~~~
예문
The cat scattered his food all over the room.
~~~

~~~
예문
The captain wore this cap and cape proudly as he sat listening to the recap of how his crew saved the men from a capsized vessel.
~~~

~~~
예문
Please enter the nine-digit id as it appears on your color - coded pass-key.
~~~

###단어 경계(boundary) 메타 문자  
~~~
 \b    단어의 시작이나 마지막을 일치시킬 때 사용한다.
 \B    단어 경계와 일치시키고 싶지 않을 때 사용한다.  
~~~
  
###문자열 경계 정의하기  
~~~
예문
This is bad, real bad!
<?xml version="1.0" encoding="UTF-8" ?>
<wsdl:definitions targetNamespace="http://tips.cf"
xmlns:impl="http://tips.cf" xmlns:intf-"http://tips.cf"
xmlns:apachesoap="http://xml.apache.org/xml-soap"
~~~

###다중행 모드 사용하기
~~~
예문
<SCRIPT>
function doSpellCheck(form, field) {
    // Make sure not empty
    if (field.value == ' ') {
        return false;
    }
    // Init
    var windowName='spellWindow';
    var spellCheckURL='spell.cfm?formname=connent&fieldname='+field.name;
    ...
    // Done
    return false;
}
</SCRIPT>
}
~~~

###하위 표현식 사용하기
~~~
예문
Hello, my name is Ben&nbsp;Forta, and I am the author of books on SQL, ColdFusion, WAP, Windows&nbsp;&nbsp;2000, and other subjects.
~~~

~~~
예문
Pinging hog.forta.com [12.159.46.200]
with 32 bytes of data:
~~~

~~~
예문
ID:042
SEX: M
DOB: 1967-08-17
Status: Active
~~~

###OR 연산자의 활용  
  
###역참조로 찾기
~~~
예문
This is a block of of text,
several words here are are repeated, and and they 
should not be.
~~~

~~~
예문
<BODY>
<H1>Welcome to my Homepage</H1>
Content is divided into two sections:<BR>
<H2>ColdFusion</H2>
Information about Macromedia ColdFusion.
<H2>Wireless</H2>
Information about Bluetooth, 802.11, and more.
<H2>This is not valid HTML</H3>
</BODY>
~~~

###치환 작업  
~~~
예문
Hello, ben@forta.com is my email address
~~~

~~~
예문
313-555-1234
232-232-2222
123-211-2432
~~~

###대소문자 변환 메타 문자  
~~~
메타 문자              설명
\E              \L 혹은 \U 변환의 끝을 나타낸다
\l              다음에 오는 글자를 소문자로 변환한다.
\L              \E를 만날 때까지 모든 문자를 소문자로 변환한다.
\u              다음에 오는 글자를 대문자로 변환한다.
\U              \E를 만날 때까지 모든 문자를 대문자로 변환한다.
~~~

~~~
예문
<BODY>
<H1>Welcome to my Homepage</H1>
Content is divided into two sections:<BR>
<H2>ColdFusion</H2>
Information about Macromedia ColdFusion.
<H2>Wireless</H2>
Information about Bluetooth, 802.11, and more.
<H2>This is not valid HTML</H3>
</BODY>
~~~

###전후방 탐색
~~~
예문
http://www.forta.com/
https://mail.forta.com/
ftp://ftp.forta.com/
~~~

~~~
예문
ABC01: $23.45
HGG42: $5.31
CFMX1: $899.00
XTC99: $69.96
Total items found: 4
~~~
  
###한글 처리  
~~~
예문
국어:수
영어:수
수학:수
미술:양
체육:가 
~~~
###정규표현식  
> (?<=[^가-힣])(수|우|미|양|가)(?=[^가-힣])

☞ 이 예제는 유니코드(UTF-8) 환경에서만 제대로 동작한다 euc-kr 등으로 인코딩된 텍스트 경우는 일치하지 않으며, 루비 1.8 버전 같이 정규 표현식에서 유니코드를 지원하지 않는 구현에서도 제대로 동작하지 않는다.  

###유니코드 사용
~~~
예문
Copyright ⓒ 2009 인사이트
~~~

<kbd >CMD+OPTION+M</kbd>
<span class="key">Command</span>

[^1]: 일반적으로 문자 그대로 사용되지 않고 특별한 의미를 지니는 문자.