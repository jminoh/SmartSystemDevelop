### C Review

- 프로그래밍 언어 구동방식
  - 인터프리터: 파이썬, BASIC
  - 컴파일러: 기계어 번역, 컴파일링 후 실행파일 단독으로 실행 가능

- S/W 개발 방법론
  - Agile Process

- 함수
  - 인수, 인자, argument, factor

+ 세미콜론  
  - 연산을 수행하는 문장

+ 표준 라이브러리에 대한 이해
  - 헤더 파일 포함해야 사용 가능(ex. stdio.h)
    - 헤더 파일의 포함을 알리는 선언은 가장 먼저 등장해야.

- return
  - 함수 종료(빠져 나옴)(void, return 0;)
  - 호출 영역으로 값 반환
  - 둘 이상의 return문 존재하는 것 가능

- console: 사용자에 의해 입출력되는 장비 
  - 입력: 키보드 / 출력: 모니터

- 서식문자(Conversion Specifier)
  - 출력 대상의 출력 형태 지정
  - %s, %c, %d, %lf, %f, %u(10진 정수 부호x), %e(지수)
  - %e %E / %x %X는 대소문자 차이
  - %%: % 출력

- \n: 개행, Line Feed, LF
- \r: 캐리지 리턴, Carriage return, CR
- \t: 수평 탭
- \\: 백슬래시(\), 경로 표시에 사용

- 자료형
    > 문자열 표현을 위해 char*(문자열 포인터) 사용
  - 정수형: char, int, long
  - 실수형: float, double

- 변수: 주소에 붙이는 이름 
  - 한글로 만들 수 없음
  - 대소문자 구분
  - 숫자로 시작할 수 없음.
  - keyword 사용 불가
  - 공백 불가

- 연산자
  - 대입 연산자와 산술 연산자: =, +, -, *, /, %
  - 기타 대입 연산자: 대입 연산자 + 산술 연산자
  - 증감 연산자: ++, --
  - 관계 연산자: < > == !- <= >=
  - 논리 연산자: && || !
  - 비트단위 연산자: ~ & ^(Exclusive OR, XOR) | << >> 
  - ','연산자
  - 우선 순위 존재
    - ()로 표현해 주는 것이 좋음

- C keyword
  - extern: 외부 소스 파일의 전역 변수 사용할 때 사용
    - 컴파일 시 외부소스 연결
    - 실무적으로는 extern할 것들은 헤더파일에 선언해 둠
  - inline: C++, assembly 등 속도, 효율 중시할 때 가끔 사용

- 데이터 표현 단위
  - 비트
  - 바이트: 1byte = 8bit
  - 데이터 저장 단위는 byte(Address Bus)
  - nibble = 4bit -> 16진수 digit

- 정수 표현
  - MSB(Most Significant Bit): 가장 왼쪽 비트, 부호 표현
    - 4byte = 4G개 표현, -2G ~ 2G
    - unsigned라면 부호비트까지 숫자표현에 사용

- Shift 연산
  - shift left: *2, 우측은 0으로 채움
  - shift right: /2, 좌측은 0으로 채움

- ASCII code
  - 1byte => -128~127인데, 음수 빼고 0~127(16진수 0x0~0x7f)
  - char형
  - 숫자 누르면 숫자 기호가 들어가는 것. 실제 숫자는 ASCII code값이
들어감

- 심볼릭 상수
  - 이름을 지니는 상수
  - const를 통한 변수의 상수화

- 자료형 변환
  - 자동 형 변환
    - 오류 발생 소지 높아짐
  - 강제 형 변환
    - 프로그래머가 명시적으로 하는 것.
    - casting

- 반복문
  - while
    - 반복조건 만족 시 반복내용을 수행
    - 반복내용이 한 줄이면 중괄호 생략 가능
	- while(i<10) printf("Hello World! \n"), i++;
	- ','까지 사용해서 한 줄로 구성
    - 반복 조건으로 true오면 무한루프 가능
	- while(1) ; => 완벽한 무한루프
	- ;사용함으로 문장 완성
  - do~while
  - for
    - 초기문, 조건문, 증감문 하나의 문장으로 구성

- 분기문
  - if
    - 초기문, 조건문, 증감문 하나의 문장으로 구성
    - 실행 조건 만족 시 내용 실행
    - if - else if - else
    - 조건문 정수로 처리할 수 있다면, switch문이 낫다.
  - switch~case
    - 조건문(정수)에 해당하는 case 실행
    - break: 반복문에서 사용하는 것이 원칙이나, switch~case문에서도 사용
    - 조건문에 연산 불가

- GOTO label문
  - goto abc;
  - abc:

- 함수
  - 반환형 함수명 입력의 형태
  > 사용 전에 선언되어 있어야!(함수, 변수, 자료형)
  - 인자와 반환값 있을 수도, 없을 수도
  - 재귀 함수

- 변수
  - 전역 변수: 함수 내에 선언되지 X
    - Static, 전역으로 사용
  - 지역 변수: 중괄호 내에서 사용

- 배열
  - index 0에서 시작
  - for문과 결합 

- 문자열 변수
  - char* = char[]
  - 문자열이면서 변수의 특징을 가짐

- 문자열 vs char형 배열
  - char* vs char[]
  - char[]은 '\0'없을 수도

- 다차원 배열
  - 초기화 시, 행별로 중괄호 다시

## 포인터
- 2차원 배열
  - String(char*)의 배열

  > * == [ ]

- 포인터: 주소 값을 담고 있는 변수
  - & 연산자: 변수의 주소 값 반환
  - '*' 연산자: 포인터가 가리키는 메모리 참조

- 포인터 오류 조심
  - 초기화 하기
  - 상수로 초기화 하지 않기
    - Device Driver, SDK, H/W 장비
    - DMA 
    - PCI

- 문자열 표현 방식의 이해
  - 배열 기반의 문자열 변수
    - char str1[5] = "abcd";   // 어디엔가 존재하는 a, b, c, d, \0을 
복사해 와서 str1에 저장
  - 포인터 기반의 문자열 상수
    - char *str2 = "ABCD";    // str2에는 값을 복사해 오는 것이 아니라
어딘가 존재하는 ABCD\0의 **주소**가 복사되어 저장됨

- 포인터 배열
  - 문자열 배열에 사용 
    - char* arr[3] = {"abc", "def", "ghi"};
    - char* arr[3] = {0x1000, 0x2000, 0x3000};

- 포인터에 의한 인자의 전달
  - Call by Reference

- 배열의 함수 인자 전달
  - 배열명, 배열 주소, 포인터에 의한 전달

- const: 변수의 상수화

- void 포인터
  - 자료형에 대한 정보 제외
  - 포인터 연산, 메모리 참조와 관련된 일에 사용하려면 casting해야

- 구조체
  - 하나 이상의 기본 자료형을 기반으로 사용자 정의 자료형을 만들 수 
있음.
  - 구조체 멤버, 사용 시 '.'

- typedef
  - 새로운 변수 타입 선언 가능
    - 만들어져 있는 변수 타입에 별명 지어줌

- union 공용체
  - 메모리 공간을 둘 이상의 변수가 공유

- enum 열거형
  - week

## 7강 C 표준함수

- 이식성(porting)
- strlen, strcpy 등...
- 플랫폼
  - H/W: PC, MAC, SUN, RASPBERRY
  - OS: MacOS, Windows, Linux

### 24장. 문자와 문자열 처리 함수
- 입출력
  - 파일, 콘솔, 소켓 입출력 
    - Console: 키보드(입력), 모니터(출력)
    - ***파일 > 정확히는 모든 입출력은 파일 입출력, 콘솔 입출력도 이것의 특수 형태
	- Text File
	- Binary File
    - 소켓: 네트워크(특히 Ethernet) 통신을 위한 라이브러리로 이루어져
있는 device

- 스트림에 대한 이해(C#)
  - 데이터를 송수신하기 위한 일종의 다리

- 표준 입출력 스트림
  - 프로그램 실행 시 자동으로 생성 및 소멸
  - 모니터와 키보드를 그 대상으로 함
    - stdin: 표준 입력 스트림, 키보드
    - stdout: 표준 출력 스트림, 모니터
    - stderr: 표준 에러 스트림, 모니터
	- 셋을 묶어 Console

- 문자 출력 함수
  - printf, fprintf(파일출력), int putchar(int c): 문자 하나, 
int fputc(int c, FILE* stream): 파일로 출력

- 문자 입력 함수
  - scanf, fscanf, getchar, getch, fgetc(FILE* stream)

- EOF(End Of File): file을 읽을 때, 끝에 도달했다는 read함수의 
return 값
  - fgetc, getchar 함수가 파일의 끝에 도달하는 경우 반환
  - 파일의 끝을 표현하기 위한 상수(-1 값 가짐)
  - console의 경우 Ctrl-Z가 파일의 EOF를 의미

- 문자 단위 입출력 함수의 필요성
  - 용도에 맞는 적절한 함수를 제공함으로써 성능 향상을 도모

  > cmd(command) 창
    - C:\Users\snows>: 프롬프트
    - copy con aa + 타자 치고 Ctrl+Z: 1개 파일이 복사되었습니다.
	- dir 다시 하면 aa 파일 생성
	- console로부터 파일 aa로 입력한 내용을 복사한 것
	- 카메라로 찍어 보낸 것 같은.(파일로 만들어 복사한 것 x)

    - copy aa bb
	- aa를 bb로 복사한 것.

    - con <- console

    - type aa 
	  - 해당 파일 읽어서 모니터에 출력해 줌.

    - type bb >> cc     // C에서는 shift right
	  - cmd에서는 redirection
	  - cc라는 파일 생성

    - typed bb >> con 
	  - con이 생성되는 것이 아니라 출력이 됨
	  - C++ console

- 문자열 출력 함수
  - puts, fputs
- 문자열 입력 함수
  - gets, fgets

```C
#include <stdio.h>
#include <string.h>
// 210813 StreamTest

int main(void)
{
	char buf[1024];
	while (1)
	{
		fgets(buf, 1024, stdin);		// 파일 입력 함수
		if (strlen(buf) < 3) break;  // 엔터도 0 아님. Ctrl+C로 탈출
		fputs("==== 입력문자열 =====>", stdout);
		fputs(buf, stdout);
	}
	return 0;
}
```
> stdin, stdout은 이미 존재하는 파일 스트림
  - 언제라도 사용 가능
    - 나머지는 언제라도 사용 가능한 것이 아님
    - 나머지는 아직 존재 X or 사용할 준비 X
	- File Stream!!
	- FILE *f = fopen(파일 전체 경로(\\해당파일이름, 백슬래시 
2개씩으로 바꿔줌), 용도(입력용인지 출력용인지 명시)); 
	  - fopen함수 이용해 초기화
	  - 용도
		- "r": read()
		- "w": write(overwrite) // 기존 파일 날리고 or 
기존 없으면 새로 
		- "a": append  // 추가, 기존 파일 뒤에
 
	










