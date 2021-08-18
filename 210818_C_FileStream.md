### C Review

- Compile

- 프로그램은 함수 단위로 이루어짐.
  - 기본은 main함수
  - 반환형, 함수명, 매개변수 형

- 표준 라이브러리
  - 표준화해 만들어 놓은 함수들의 집합
  - stdio.h: 기본 헤더

- return
  - 함수 종료
  - 함수 호출한 영역으로 값 반환
  - void type은 return 생략
  - 둘 이상의 return도 가능

* buffer(=stream) -> 문자열의 흐름
* file/network(둘다 stream의 일종) -> 데이터의 흐름

- Console
  - printf
  - scanf
- Conversion Specifier
  - %c: 단일 문자
  - %s: 문자열
  - %f: 10진 실수
  - %d: 10진 정수
  - %x: 부호 x, 16진 정수
  - %숫자d: 가운데 숫자는 데이터가 입력될 자리를 표시. 
정렬시 유용
    - 가운데 정렬 없음, 함수로 만들어야

- scanf
  - 오류상황 염두에 두지 않음
    - 지정된 입력 서식과 다른 값 입력될 경우 오류
  - 해당 변수의 주소의 값 넣어야 -> &변수명
  - 표준 함수지만, 사용 권장은 x
    - _CRT_SECURE_NO_WARNINGS
    - SDL검사 No
	- 그럼에도 불구하고 공부하는 이유는, printf와 scanf가
많은 입출력 함수의 원형이기 때문

- 변수
  - 변수명
    - 숫자로 시작 X
  - const: 상수처럼 사용
    - 배열, 미리 크기 지정해 둬야 함. 크기 지정에는 상수 필요.

- 상수
  - 데이터 변경 불가

- ASCII code
  - 7bit code, 0~127
  - MSB는 특수한 용도. 
    - 문자 처리 시 2byte code
	- 영문만 있을 때는 txt = binary
	- 한글도 있을 때는 속성 달라짐. Encoding
	- UTF-8, Unicode
	- CRLF: Carriage Return(0d, 13), Line Feed(0a, 10)
	  - Windows 줄바꿈 문자

- 형 변환
  - 자동 형 변환(묵시적 형 변환)
    - 유의
  - 강제 형 변환(명시적 형 변환)
    - Cast

- 반복문
  - while(do~while)
  - for

- while
  - 반복조건이 true일 때, 반복 내용 실행
  - 반복 내용 2줄 이상이면 중괄호
  - 반복조건 1 -> 무한 루프

- break
  - 반복문 탈출

- for
  - 초기문, 조건문, 증감문

- if
  - 분기문, 조건문
  - 실행 조건 만족 시, 내용 실행
  - 1회성

- if~else
  - if문 조건 불만족 시, else문 실행

- switch
  - 조건에 정수. 비교 연산 올 수 없음.
  - 분기가 많다면, if보다 switch
  - case에 break
  - case에 해당하는 것 없다면 default

- goto label
  - 주의
``` C
goto rabbit;   // 이동할 위치 지정
...
rabbit:   // 이동할 위치 표시 위한 label 선언
```

- 삼항 연산자
  - if~else를 한 문장으로

### 함수
- 기본 형태
  - 반환형, 함수명, 매개변수 형
  - 중괄호로 시작해, 중괄호로 끝

- 함수 정의하는 이유
  - 모듈화
    - Low level Function(다른 함수 호출하지 않는 함수)
  - 유지 보수, 확장의 용의성
  - 문제 해결의 용이성: Divide and Conquer

- 함수 선언
  - 호출되기 전에 정의 되어야 함.
  - Prototype 선언: 함수 정의가 뒤에 나올테니 에러 없이 넘어가 줘
    - 헤더파일 선언 이후, main 정의 이전
    - 함수 정의는 main 아래
  > 헤더파일: 프로토타입 선언과 자료구조(구조체...) 정의가 모여 있는 것

- 변수
  - Local
    - 모든 중괄호 내에 선언되는 변수
  - Global
    - 모든 전역 변수는 Static
    - 가급적 긴 변수명 사용해, 변수의 의미 자세히 표현
  - Static 
    - 함수 내외부 모두 선언 가능
    - Static은 전역 변수로 사용하는 것이 좋음
  - Register

### 배열
- 배열
  - 둘 이상의 변수, 동시 선언의 효과
  - 다량의 데이터 일괄 처리 시 유용
  - Local/Global
  - 배열 자료형, 배열명, 배열길이
    - 배열길이는 상수 Only
	- const
    - 배열명은 포인터
    - 인덱스는 0부터

- 문자열 상수
  - 문자열이면서 상수의 특징
  - printf("Hello World! \n");

- 문자열 변수
  - 문자열이면서 변수의 특징
  - char str1[5] = "Good"; char str2[] = "morning";

- 널문자를 지녀야 하는 이유
  - 문자열의 끝을 표현하기 위해서
  - 쓰레기 값과 실제 문자열의 경계 나타내기 위해
  - printf함수는 널문자를 통해 출력 범위 결정

### 다차원 배열
- 다차원 배열
  - 2차원 이상의 배열
  - 초기화 시, 행 단위로 중괄호 다시
  - 초기화 시, 행은 몰라도 열은 상수를 넣어 배열의 크기 예상하게끔

> [ ] == *
- 1차원 배열명의 포인터 타입 결정
  - 포인터가 가리키는 요소의 자료형
  - 포인터 연산 시 증가하는 byte의 크기
- 1차원 배열 명
  - 배열명이 가리키는 요소의 자료형이 일치한다면, 포인터 연산 시
증가하는 값의 크기도 일치
  - 따라서 1차원 배열명의 경우 가리키는 요소만 참조

### 포인터
- 포인터
  - 메모리의 주소 값 저장하기 위한 변수
  - &: 변수의 주소 값 반환
  - '*': 포인터가 가리키는 메모리 참조

### 구조체
- 구조체: 하나 이상의 기본 자료형을 기반으로 사용자 정의 자료형을 만들
수 있는 문법 요소

- typedef
  - 별명

- union 공용체
  - 하나의 메모리를 둘 이상의 변수가 공유

- enum 열거형
  - week
  - 내부적으로 int

## C 표준함수
- 문자, 문자열 처리 함수
  - 입출력
    - 파일
    - console(특수 파일)
    - 소켓 입출력
  - Stream
    - data 흐름
    - 입력 스트림(stdin, 특수 파일)
    - 출력 스트림(stdout, 특수 파일)

- EOF(End Of File)
  - fgetc, getchar 함수가 파일의 끝에 도달하는 경우 반환
  - -1을 return
  - console에서는 Ctrl-Z가 EOF

- 파일 접근 모드
  - 개방한 파일의 사용 용도를 결정
  - r(read), w(write), a(append, 덧붙임, 요소 추가), r+, w+, a+
    - a+t(의미 x, a만 써도 t되어 있는 것), r+b
  - t(text mode), b(binary mode)
    - t는 CRLF

``` C
#include <stdio.h>
#include <string.h>
// 210813 StreamTest

int main(void)
{
	char buf[1024];
	FILE* f = fopen("C:\\Users\\snows\\aa", "r");  // "r": read()  // 없는 파일이면 NULL 반환
	FILE* fout0 = fopen("C:\\Users\\snows\\aa.o0", "w");  // "w": write
	FILE* fout1 = fopen("C:\\Users\\snows\\aa.o1", "a");  // "r": read()  // 없는 파일이면 NULL 반환
	FILE* fout2 = fopen("C:\\Users\\snows\\aa.o2", "a+b");  // "r": read()  // 없는 파일이면 NULL 반환
	if (f != NULL)
	{
		while (1)										// "w": write()
		{
			if (fgets(buf, 1024, f) == NULL) break;		// 파일 입력 함수
			//if (strlen(buf) < 3) break;  // 엔터도 0 아님. Ctrl+C로 탈출
			//fputs("==== 입력문자열 =====>", stdout);
			fputs(buf, stdout); // 화면 출력
			fputs(buf, fout0);
			fputs(buf, fout1);
			fputs(buf, fout2);
		}
	}
	else printf("입력 파일이 존재하지 않습니다.\n");
	return 0;
}
```

> cmd창에서 aa*-> aa로 시작하는 모든 것을 보여줌(와일드카드)
- dir 눌러서 나오는 시간은 파일 생성 시간
  - aa.o2만 크기 조금 다름
  - type aa로 봤을 때 출력은 다 같음
  - 메모장으로 봤을 때
    - aa.o2만 CRLF가 아니라 Unix(LF)임
    - CR 사라짐
	- binary 모드 출력 시 LF만 남아있음

## File Stream 이용한 성적표
### file 안의 data 이용한 처리
- fprintf(file*, "", a), fscanf(file*, "", &a)

- write: 새 결과 나오면 새 파일로
- append: 새 결과는 기존 결과 밑에 덧붙임

- 메모장 Encoding이 UTF-8인 경우 ANSI로 바꿔줘야 한글이 나옴

> table1(표 형식)으로 못하고, 1열로 바꾼 table2 이용

``` C
// 210818
// File Stream 이용한 성적표
// file 안의 data 이용한 처리

#include <stdio.h>
#include <string.h>
typedef struct student {
	int kor;
	int eng;
	char name[10];
} STU;


void sortEx(double* arr, int n);
void swap(int* a, int* b);
void swapEx(double* a, double* b);
void swapEx1(char* a, char* b);
void swapEx2(const char** a, const char** b);
void SWAP(void* a, void* b, int op);

const int nArr = 10;

STU student[nArr];

int main(void)
{
	double f_kor = 0.3, f_eng = 0.7;
	double tot[nArr];
	int i, j, k;

	int m, n;
	char buf[1024];
	FILE* fin = fopen("C:\\Users\\snows\\table3.txt", "r");  // "r": read()  // table2의 data 가져올 수 있게	
	FILE* fout = fopen("C:\\Users\\snows\\table4.txt", "w+b");  // table4이라는 파일 생성해서 저장할 수 있도록

	for (i = 0; i < nArr; i++)
	{
		fscanf(fin, "%s %d %d", buf, &m, &n);
		student[i].kor = m;
		student[i].eng = n;
		strcpy(student[i].name, buf);
		tot[i] = student[i].kor * f_kor + student[i].eng * f_eng;
		//tot[i] = (student[i].kor = kor[i]) * f_kor + (student[i].eng = eng[i]) * f_eng; // 위 세 주석 한 줄로 합침.
	}											// 점수를 구조체의 멤버변수에 대입하고, 가중치 곱함. 가중치 곱해진 두 점수를 더해 tot에 대입.
	printf("Original :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", tot[i]); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");
	//printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	//printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	//printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	//printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", tot[i]); printf("\n\n");
	printf("이름	국어	영어	합계	\n");
	fprintf(fout, "이름	국어	영어	합계	\n");  // table4에 써질 내용
	for (int i = 0; i < nArr; i++)
	{
		printf("%7s%7d%7d%7.2lf\n", student[i].name, student[i].kor, student[i].eng, tot[i]);
		fprintf(fout, "%7s%7d%7d%7.2lf\n", student[i].name, student[i].kor, student[i].eng, tot[i]);   // table4에 써질 내용
	}

	return 0;
}
void sortEx(double* arr, int nArr)
{
	int i, j, temp;
	for (i = 0; i < nArr; i++)
	{
		for (j = i; j < nArr; j++)
		{
			if (arr[i] < arr[j])                              // swap 함수 사용해도 됨.
			{
				/*temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;

				temp = kor[i];
				kor[i] = kor[j];
				kor[j] = temp;

				temp = eng[i];
				eng[i] = eng[j];
				eng[j] = temp;

				temp = nam[i];
				nam[i] = nam[j];
				nam[j] = temp;*/

				//swapEx(arr + i, arr + j);    // = swap(&a[i], &a[j]); // tot: double
				//swap(kor + i, kor + j);      // kor: int
				//swap(eng + i, eng + j);      // eng: int
				//swapEx1(nam + i, nam + j);
				//swapEx2(name + i, name + j);
				SWAP(arr + i, arr + j, 8);
				SWAP(student + i, student + j, 18);  //
				//SWAP(kor + i, kor + j, 4);
				//SWAP(eng + i, eng + j, 4);  
				//SWAP(name + i, name + j, 4);  // 위와 의미적으로 동일, name이 문자열의 배열 == 포인터(주소 값)
			}
		}
	}
}
void swap(int* a, int* b)
{
	int c = *a;
	*a = *b;
	*b = c;
}
void swapEx(double* a, double* b)
{
	double c = *a;
	*a = *b;
	*b = c;
}
void swapEx1(char* a, char* b)
{
	char c = *a;
	*a = *b;
	*b = c;
}
void swapEx2(const char** a, const char** b)   // Data type 맞춰주는 것이 우선. // parameter: string*(4byte 변수), 주소가 전달된 것.
{											// 내용 바뀐 것 없음, 값이 바뀌려면 * 필요    // const char* a[], const char* b[]
	const char* c = *a;		// c* = a[]
	*a = *b;				// a[0] = b[0]
	*b = c;					// b[0] = c
}
void SWAP(void* a, void* b, int op)  // 포인터 받아주는 쪽에서 void 하면, 함수 호출시 void포인터에 넣을 필요 없음. 포인터만 던지면 됨.
{										//	op는 자료형의 크기
	if (op == 1)			//char
	{
		char c = *(char*)a;
		*(char*)a = *(char*)b;
		*(char*)b = c;
	}
	else if (op == 4)		//int, float,
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
	else if (op == 18)			//double
	{
		STU c = *(STU*)a;
		*(STU*)a = *(STU*)b;
		*(STU*)b = c;
	}
}
```

``` C
#if 0
// 210818 성적표 이전 테스트
#include <stdio.h>
void StreamTest(void);
int main()
{
	StreamTest();
}
void StreamTest(void)
{
	int m, n, i = 0;
	char buf[1024];
	FILE* fin = fopen("C:\\Users\\snows\\table1.txt", "r");  // "r": read()  // 없는 파일이면 NULL 반환	
	FILE* fout = fopen("C:\\Users\\snows\\table2.txt", "w+b");  // 
	if (fin != NULL)
	{
		while (i < 10)
		{
			fscanf(fin, "%s %d%* %d%*", buf, &m, &n);
			fprintf(fout, "이름: %s\n 국어 점수: %d%*\n 영어 점수: %d%*\n", buf, m, n);
			printf("이름: %s\n 국어 점수: %d%*\n 영어 점수: %d%*\n", buf, m, n);
			i++;
		}
	}
	else printf("입력 파일이 존재하지 않습니다.\n");
}
```

### 강사님
- White Space(tab, space, CRLF)
  - 따로 처리 안 하면, 문자열에 포함되어 버림
- 문자열 내에서 \는 매우 특별한 기호
  - fopen에서 경로를 위해 \를 사용할 때, 2개씩 사용
- "%-7s": -표시는 왼쪽정렬

``` C
// 210818
// File Stream 이용한 성적표
// file 안의 data 이용한 처리
// 강사님, table1 이용, White space
// avg 추가

#include <stdio.h>
#include <string.h>
typedef struct student {	// tot에 sort함수를 사용하면서, student도 sort하기 위해
	char name[10];
	int kor;
	int eng;
	double tot;
	double avg;
} STU;


void sortEx(double* arr, int n);
void swap(int* a, int* b);
void swapEx(double* a, double* b);
void swapEx1(char* a, char* b);
void swapEx2(const char** a, const char** b);
void SWAP(void* a, void* b, int op);
void sortSTU(STU* a, int n);

const int nArr = 10;
STU student[nArr];

int main(void)
{
	double f_kor = 0.3, f_eng = 0.7;
	int i, j, k;

	FILE* fin = fopen("C:\\Users\\snows\\table1.txt", "r");  // "r": read()  // table2의 data 가져올 수 있게	
	FILE* fout = fopen("C:\\Users\\snows\\table2.txt", "w+b");  // table2이라는 파일 생성해서 저장할 수 있도록

	for (i = 0; i < nArr; i++) fscanf(fin, "%s", student[i].name);
	for (i = 0; i < nArr; i++) fscanf(fin, "%d", &student[i].kor);
	for (i = 0; i < nArr; i++) fscanf(fin, "%d", &student[i].eng);
	for (i = 0; i < nArr; i++)
	{
		student[i].tot = student[i].kor + student[i].eng;
		student[i].avg = student[i].tot / 2;
	}

	printf("Original :\n"); 
	printf("%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	for (int i = 0; i < nArr; i++)
	{
		printf("%7s %7d %7d %7.2f %7.2f\n", student[i].name, student[i].kor, student[i].eng,
			student[i].tot, student[i].avg);
	}

	sortSTU(student, nArr);

	printf("Sorted :\n");
	printf("%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	fprintf(fout, "%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	for (int i = 0; i < nArr; i++)
	{
		printf("%7s %7d %7d %7.2f %7.2f\n", student[i].name, student[i].kor, student[i].eng,
			student[i].tot, student[i].avg); 
		fprintf(fout, "%7s %7d %7d %7.2f %7.2f\n", student[i].name, student[i].kor, student[i].eng,
			student[i].tot, student[i].avg); 
	}
	
	return 0;
}
void sortSTU(STU* a, int n)
{
	int i, j, k;
	for (i = 0; i < n; i++)
	{
		for (j = i; j < n; j++)
		{
			if ((a+i)->avg < (a+j)->avg)  // 구조체 포인터의 값 =>a[i].avg < a[j].avg, a+i: i번째 구조체, ->avg:의 avg 멤버
			{								
				SWAP(a + i, a + j, sizeof(STU));  // 구조체의 size => sizeof(구조체), 포인터 두 개 받아서 교환
			}
		}
	}
}

void sortEx(double* arr, int nArr)
{
	int i, j, temp;
	for (i = 0; i < nArr; i++)
	{
		for (j = i; j < nArr; j++)
		{
			if (arr[i] < arr[j])                              // swap 함수 사용해도 됨.
			{
				
				SWAP(arr + i, arr + j, 8);
				SWAP(student + i, student + j, 18); 
				
			}
		}
	}
}
void swap(int* a, int* b)
{
	int c = *a;
	*a = *b;
	*b = c;
}
void swapEx(double* a, double* b)
{
	double c = *a;
	*a = *b;
	*b = c;
}
void swapEx1(char* a, char* b)
{
	char c = *a;
	*a = *b;
	*b = c;
}
void swapEx2(const char** a, const char** b)   // Data type 맞춰주는 것이 우선. // parameter: string*(4byte 변수), 주소가 전달된 것.
{											// 내용 바뀐 것 없음, 값이 바뀌려면 * 필요    // const char* a[], const char* b[]
	const char* c = *a;		// c* = a[]
	*a = *b;				// a[0] = b[0]
	*b = c;					// b[0] = c
}
void SWAP(void* a, void* b, int op)  // 포인터 받아주는 쪽에서 void 하면, 함수 호출시 void포인터에 넣을 필요 없음. 포인터만 던지면 됨.
{										//	op는 자료형의 크기
	if (op == 1)			//char
	{
		char c = *(char*)a;
		*(char*)a = *(char*)b;
		*(char*)b = c;
	}
	else if (op == 4)		//int, float,
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
	else if (op == sizeof(STU))			//double
	{
		STU c = *(STU*)a;
		*(STU*)a = *(STU*)b;
		*(STU*)b = c;
	}

}
```



