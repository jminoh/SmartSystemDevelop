### 복습

- 성적표 실습에서 swap 함수 다시 보기

5강 포인터

- 포인터: 주소를 data로 하는 변수
  - 포인터의 크기: 4byte

13장

- 포인터와 포인터 변수

- 컴퓨터의 주소 체계에 따라 크기가 결정
  - 32비트 시스템 기반: 4byte
    - 인텔 CPU: page 단위로 관리

- 포인터 타입과 선언
  - 포인터 선언 시 사용되는 연산자: '*'
  - A형 포인터(A*): A형 변수의 주소 값을 저장

- 주소 관련 연산자
  - & 연산자: 변수의 주소 값 반환
  - * 연산자: 포인터가 가리키는 메모리 참조
    - 직접 접근: printf("%d", a);
    - 간접 접근: printf("%d", *pa);

- 오류
  - 포인터 초기화를 하지 않은 경우
  - 포인터 초기화를 임의의 숫자로 하는 경우
  - 포인터 사용 시 정확한 위치를 지정해야

## 14장 포인터와 배열

- 배열명: 첫 번째 배열요소의 주소 값
  - *a(== *(a+0)) == a[0]

- 배열명과 포인터 비교
  - 포인터는 변수, 배열명은 상수
``` C
int main(void)
{
	int a[5] = {0,1,2,3,4};
	int b = 10;
	a = &b;     // a는 상수이므로 오류, a가 변수면 O
}
```

- 배열명의 타입
  - 배열명도 포인터이므로 타입 존재

- 포인터 연산을 통한 배열 요소의 접근
  - a[0] == *a == *(a + 0)

- 문자열 표현 방식의 이해
  - char 배열
  - 배열 기반의 문자열 변수
  - 포인터 기반의 문자열 상수
    - char str1[5] = "abcd";
	- 문자열을 이용해서 배열을 초기화
    > char *str2 = "ABCD"; 가장 일반적인 형태의 사용
	- 포인터 str2를 선언하고, 문자열의 주소로 초기화
``` C
#include <stdio.h>
int main(void)
{
	char str1[5]="abcd";
	char *str2="ABCD";

	printf("%s \n", str1);
	printf("%s \n", str2);

	str1[0] = 'x';  // 
	// str2[0] = 'x' ; // Error, 상수니까
	
	printf("%s \n", str1);
	printf("%s \n", str2);

	return 0;
}
```

- 포인터 배열
  - 배열의 요소로 포인터를 지니는 배열
    - int* arr1[10];
    - double* arr2[20];
    - char* arr3[30];
  - 2차원 배열과 같다
	>  *[] == ** == [][]  // == string* == string[]

	> char* == string

	> char** == string array

  - 포인터 배열의 예
``` C
#include <stdio.h>
int main(void)
{
	char* arr[3] = {
		"Fervent-lecture",
		"TCP/IP",
		"Socket Programming"
	};
	printf("%s \n", arr[0]);
	printf("%s \n", arr[1]);
	printf("%s \n", arr[2]);

	return 0;
}
```
- 성적표 실습에서 이름(nam[])을 포인트 배열로 바꾸면?
  - const char* name[10] = { "김씨", "이씨", "박씨", "최씨", "안씨", "정씨", "소씨", "조씨", "허씨", "심씨" };
    > String Array
    - const를 이용해서 문자열(성)들을 Array(name[])에 초기화
    - **  ---  "abc"
    - **a ---  'a'
    - ** = *의 주소

###오늘

## 15장 포인터와 함수에 대한 이해
- 기본적인 인자의 전달 방식
  - 인자 = 인수, factor, argument  // 함수 호출시 넣는 값
  - 값의 복사에 의한 전달(Call by value)
    - Stack(LIFO)을 통해 인수 전달, 결과 전달 등을 수행

- 배열의 함수 인자 전달 방식
  - 배열명(배열 주소, 포인터, 참조)에 의한 전달(Call by reference)
    - Stack에 들어가는 것은 Value가 저장된 공간의 주소

- 배열 이름, 포인터의 sizeof 연산
  - 배열명: 배열 전체 크기를 바이트 단위로 반환
  - 포인터: 포인터 변수의 크기(4byte)

- '*' == [ ]

- scanf 함수 호출 시 & 붙이는 이유
  - &: 변수의 주소
  - 해당 주소에 저장하게 됨
    - cf. 배열명은 이미 주소이기 때문에 & 필요 X
    - 포인터도 변수의 주소를 저장하고 있기 때문에 필요 X
	- 포인터 변수 자체의 주소를 원하는 경우 & 사용

- 포인터가 가리키는 변수의 상수화
    - const char* name[]
	- 상수 string 
```C
int a = 10;
const int *p = &a;   // p에 담기는 주소가 상수의 주소값
*p = 30     // Error  // p를 상수로 선언 -> 초기화 후 변경 불가
a = 30      // OK
```

- 포인터 상수화
``` C
int a = 10;
int b = 20;
int *const p = &a;
p = &b	// Error
*p = 30	// OK
```

- const키워드를 사용하는 이유
  - 컴파일 시 잘못된 연산에 대한 에러 메시지
  - 프로그램을 안정적으로 구성

## 16장 포인터의 포인터
  - = 더블 포인터
  - 싱글 포인터의 주소 값을 저장하는 용도의 포인터
  - string array에서 많이 사용(문자열 배열)
  - call by reference

- 구현사례1, 구현사례2 살펴보기

- 포인터 배열과 포인터 타입
  - 1차원 배열의 경우 배열명이 가리키는 대상을 통해 타입 결정됨
  - 포인터 배열도 마찬가지

## 17장 함수 포인터와 void 포인터
  - void: 없음(반환값, 입력값...)
  - void 포인터: data type이 없는 포인터

- 함수 포인터의 이해
  - pointer: 주소를 가리키는 변수

- 함수 이름의 포인터 타입을 결정짓는 요소
  - return type + parameter type
```
int fct1 (int a)
{	
	a++;		==>  int (*fPtr1)(int);  //  (변수명)(인수타입)
 	return a;		==>  -> fPtr1 = fct1
}				-> fPtr1(10)
```
		
	- 이렇게 쓰는 이유
		- 장비업체에서 SDK 제공, 사용 X
		- 사전 규약

- void 포인터란 무엇인가?
  - 자료형에 대한 정보가 제외된, 주소 정보를 담을 수 있는 형태의 변수
  - 포인터 연산, 메모리 참조와 관련된 일에 활용할 수 없다.
	- *(p+3)(int면 주소 3개만큼 지나서) 같은 연산 불가능.
	- void포인터는 타입 정해지지 않았기 때문
  - 임시 용도
  - 실사용 시 casting(강제 형변환)한 후 사용
  - 어떤 타입의 주소라도 받을 수 있음
	- 받을 때 에러 없이 받을 수 있음
	- cf. void라는 변수형은 없음
	- 포인터 변수(주소)의 크기는 4byte로 다른 타입의 주소로
바꿀 수 있음
  - 어떤 자료형의 주소를 받았든, 포인터로 그 변수에 값을 써넣을 수 없음
  - 포인터 연산 불가능(몇 바이트씩 증감인지 모름)
  - cast연산 필수

  - printf(" %d %f %s", 10, 3.14, "홍");  // 10진 정수, 실수, 문자열
	- 정수값, 실수값, 문자열(서식문자의 주소)가 Stack에 담김.

### 실습
``` C
// Void Test
#include <stdio.h>

void VoidPrint(void* p, int i)
{
	if (i == 1)
	{
		char* cp = (char*)p;
		printf("%c\n", *cp);
	}
	if (i == 2) printf("%d\n", *(int*)p);
	if (i == 3) printf("%f\n", *(double*)p);


}
int main(void)
{
	char c = 'z';
	int n = 10;
	double a = 1.414;

	void* vp;
	VoidPrint(vp = &c, 1);
	VoidPrint(vp = &n, 2);
	VoidPrint(vp = &a, 3);
}
```
    > swap 함수에서 맨 윗줄에 변수 반환형만 빼면 다 같음
	> void 포인터 가능함

### 실습
- void 포인터 이용해 swap함수 재정의

``` C
//void 포인터로 swap함수 재정의
#include <stdio.h>
void SWAP(void *p1, void *p2, int op);   // op는 자료형 크기
int main(void)
{

	int n[] = { 1,2,3,4,5 };
	SWAP(n+1, n+2, 4);

	for (int i = 0; i < sizeof(n) / sizeof(n[0]); i++)
		printf("%d  ", n[i]);
}

void SWAP(void* a, void* b, int op)  // 포인터 받아주는 쪽에서 void 하면, 함수 호출시 void포인터에 넣을 필요 없음. 포인터만 던지면 됨.
{
	if (op == 1)			//char
	{
		char c = *(char*)a;
		*(char*)a = *(char*)b;
		*(char*)b = c;
	}
	else if (op == 4)		//int, float, char*
	{
		int c = *(int*)a;
		*(int*)a = *(int*)b;
		*(int*)b = c;
	}
	else if (op == 8)			//double
	{
		double c = *(double*)a;
		*(double*)a = *(double*)b;
		*(double*)b = c;
	}
}
```
	> 메모리 구조에 대한 이해
		> op = 4로 int, float, char* 다 커버

## 6강 구조체와 사용자 정의 자료형

- 구조체(struct)

### 22장. 구조체와 사용자 정의 자료형 1

- 구조체의 정의
  - 하나 이상의 기본 자료형을 기반으로 사용자 정의 자료형을 만들
수 있는 문법 요소
  - Data type이 된다.

``` C
struct point         // struct는 keyword
{		// point라는 이름의 구조체 선언
	int x;	// 구조체 멤버 int x 
	int y;	// 구조체 멤버 int y
};
	- x좌표, y좌표를 멤버로 갖고 있는 구조체 point

- 구조체 변수의 선언 case 

``` C
struct point {
	int x;
	int y;
} p1, p2, p3;
```

    - 구조체 변수로 p1, p2, p3 선언
    - 구조체 선언이후로는 struct point p3, p4, p5; 로 구조체 변수
만들어 사용

- 구조체 변수의 접근(Access)
	- '.'으로
	> .있으면 class멤버 or 구조체 멤버 변수
``` C
struct point {
    int x;
    int y;
}
int main(void)
{
    sturuct point p1;
    p1.x = 10;	// p1의 멤버 x에 10을 대입
    p1.y = 20;	// p1의 멤버 y에 20을 대입

    return 0;
}
```
	

- 구조체 변수의 초기화
  - 배열 초기화 문법과 일치

``` C
struct person {
    char name[20];
    char phone[20];
    int age;
};
int main(void)
{
    struct person p = {"Free Lec", "02-3142-6702", 20};
    ...
    return 0;
}
```
	- 배열은 모두 동일한 data type 집합
	- 구조체는 다른 data type 집합

- 구조체 배열의 선언
``` C
struct person {
    char name[20];
    char phone[20];
    int age;
};
int main(void)
{
    struct person pArray[10];
    ...
    return 0;
}
```
	- pArray는 10개의 배열요소를 가지며, 각 배열요소는 
구조체로 name, phone, age를 멤버 변수를 갖는다.

- 구조체 배열 요소의 접근
  - pArray[1].age = 10;  // 두 번째 요소의 age에 접근
  - strcpy(pArray[1].name, "홍길동");  // 두 번째 요소의 name에 접근
	- name은 문자 배열, 초기화 과정에서 넣지 않는다면
strcpy 이용해야 함.
  - strcpy(pArray[1].phone, "333-3333"); // 두 번째 요소의 phone에 접근

> string.h 함수 (문자열 관련 함수)
  - strlen(string length): 문자열 길이 반환
  - strcpy(string copy): 문자열 복사
    - strcpy(char* destination, char* source)

- 저수준 함수 = Low level Function
  - 함수 내에 다른 함수를 호출한 것이 없이 모두 처리.

- 구조체 배열의 초기화
``` C
struct person {
    char name[20];
    char phone[20];
    int age;
};
int main(void)
{
    struct person pArray[3] ={
	{"Lee", "333"},
	{"Kim", "555"},
	{"SES", "777"}
     };
    ...
    return 0;
}
```

- 구조체와 포인터
  1. **구조체 포인터** 를 선언하여 구조체 변수를 가리키는 경우
  2. 구조체의 멤버로 포인터 변수가 선언되는 경우 -> 당연

- 구조체와 배열 그리고 포인터 예시

``` C
struct person {
    char name[20];
    char phone[20];
};

int main(void)
{
    struct person man = {"Thomas", "354-00xx"};
    struct peerson *pMan;
    pMan = &man;

    // 구조체 변수를 이용한 출력
    printf("name : %s\n", man.name);
    printf("phone : %s\n", man.phone);

    // 구조체 포인터를 이용한 출력1.
    printf("name : %s\n", (*pMan).name);
    printf("phone : %s\n", (*pMan).phone);

    // 구조체 포인터를 이용한 출력2.
    printf("name : %s\n", pMan->name);	// 포인터로 멤버변수 가리킬 때
    printf("phone : %s\n", pMan->phone);
    return 0;
```

- 구조체 변수와 주소 값의 관계
  - double: 8byte
  - int: 4byte
  - char: 1byte
  - Pointer: 4byte
  - struct: 모른다. 
> 구조체는 멤버의 종류에 따라 다르게 구성이 됨.
	> 구조체의 byte 수는 선언마다 다름.
	> 멤버 구성 다 더하면 됨.

### 내일: 23장 구조체와 사용자 정의 자료형 2




