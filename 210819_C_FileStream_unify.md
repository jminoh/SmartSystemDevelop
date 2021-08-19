File* fin = fopen("경로, \는 2개씩", "r a w");
- File 입력함수
  - fgets: 한 줄(문자열)씩 처리
  - fgetc: 한 문자씩 처리 
  - fscanf: 전형화된 입력
    - scanf와 사용법 동일, 첫 argu로 File pointer 들어감

- File 출력함수
  - fputs: 한 줄(문자열)씩 처리
  - fputc: 한 문자씩 처리
  - fprintf
    - 첫 argu로 File pointer 들어감

- 문자열의 길이 반환하는 strlen
  - size_t strlen(const char* s)

- 문자열을 복사하는 strcpy
  - 문자열 상수 대입연산 불가
  - char* strcpy(char* dest, const char* src);
    - '\0'까지 복사
  - char* strncpy(char* dest, const char* src, size_t n);
    - 맨 뒤의 size는 복사할 문자 수
    - size_t는 typedef 자료형(unsigned int)
    - '\0' 복사 X

- 문자열 추가하는 함수 strcat -> 문자열 결합
  - char* strcat(char* dest, const char* src);
  - char* strncat(char* dest, const char* src, size_t n);

- 문자열을 비교하는 함수 strcmp(Compare)
  - 조건문에서 사용
  - int strcmp(const char* s1, const char* s2);
  - int strncmp(const char* s1, const char* s2, size_t n);
  - s1 > s2, return 양수
  - s1 = s2, return 0
  - s1 < s2, return 음수
  - s1이 abc, s2가 abcd일 경우, 4번째에 s1은 널, s2는 d로 s2가 더 큼

- 문자열을 숫자로 변환하는 함수
  - #include <stdlib.h>
  - int atoi(char *ptr): 문자열을 int형 데이터로 변환(a(ascii) to i(integer))
  - long atol(char *ptr):  문자열을 long형 데이터로 변환
  - double atof(char *ptr):  문자열을 double형 데이터로 변환

- 대소문자 변환을 처리하는 함수
  - #include <ctypes.h>
  - int toupper(int c);
  - int tolower(intc);

### 25장 파일 입출력

- 파일의 OPEN
  - 데이터를 주고 받을 수 있는 스트림의 생성
``` C
#include <stdio.h>
  FILE* fopen(const char* filename, const char* mode)
```
  - const char* filename  ->  full path(전체 경로)
    - 해당 파일의 위치가 프로그램 실행 위치(Current directory)라면 
전체경로x, 파일명만 가능
      - 프로그램 실행 위치는 repository의 Debug 폴더 내.
      - 실행 프로그램은 Debug 폴더 내의 응용프로그램
	- 응용프로그램만 타 폴더로 옮겨 가면, 다른 곳에서 수행 가능.

  - 현재 프로그램이 실행되는 위치(Current Directory)
    - cmd창에서 뜨는 위치
      > type 파일명으로 파일 볼 수 있음
      > cd ..에서 ..은 상위 direc, cd는 change directory
	> . 은 현재 dir, /은 root dir
      > 

  - 성공 시 해당 파일의 파일 포인터, 실패 시 NULL 포인터 리턴

- 파일 OPEN mode
  - 파일 접근 모드 + 데이터 입출력 모드
- 파일 접근 모드   
  - r(read), w(write), a(append)
- 데이터 입출력 모드
  - t(text mode, 기본)
  - b(binary mode): 있는 그대로 저장.
    - t와 b의 차이는 LF처리
    - t는 CRLF
    - b는 LF
  - "a + b" 추천 (/r/n 써야)

- CR & LF
  - CR(Carriage Return): \r
  - LF(Line Feed): \n
  - C(Windows): LF만 써도 CRLF로 됨 vs Unix LF쓰면 그대로 LF만

- FILE 구조체의 포인터
  - FILE 구조체: stdio.h에 저장되어 있음
  - fopen함수의 return type
  - 파일 포인터
  - 개방한 파일에 대한 여러 정보를 지니는 변수를 가리키는 포인터
  - 용도
    - 데이터 입출력 함수의 호출
    - 위치 정보의 참조
    - 파일의 끝 확인

- 파일의 종결(CLOSE)
  - 스트림의 종결
  ``` C
  #include <stdio.h>
  int fclose(FILE * stream)
  ```
  - 종료가 오류 없이 제대로 이뤄지면 0 리턴
  - 모든 파일 입력 끝난 후, fclose(포인터)
  - 프로그램 정상 종료 시 OS가 닫아주지만, 비정상 종료나 여타의 상황이
벌어질 수 있으므로 닫아주는 것이 좋음
  - 여러 파일을 한번에 닫아주려면
    > fcloseall(); : 현재 열린 모든 파일 닫는 함수

**Windows Programming에서 file입출력 함수는 사용하지만, console
입출력 함수는 사용하지 않음**

- 파일 위치 지시자
  - FILE 구조체 변수의 멤버로서 존재
  - READ & WRITE에 대한 위치 정보가 된다.
  - 입출력 함수의 호출에 의해 이동
  - 순차적인 입력 및 출력이 가능한 이유
  - Random Access

- Random Access
  - 특정 위치 임의 접근 방식의 입출력
  - DB에서 빈번히 사용
``` C
#include <stdio.h>
int fseek(FILE * stream, long offset, int wherefrom)
```
  - 성공시 0, 실패 시 0이 아닌 값을 리턴

|만약에 wherefrom이|파일 위치 지시자를 offset만큼 이동하기 전에|
|-----------------------|---------------------------------------------------|
|SEEK_SET(0)이라면|파일의 맨 앞으로 이동|
|SEEK_CUR(1)이라면|이동하지 않는다.|
|SEEK_END(2)이라면|파일의 끝으로 이동한다.|

### 26장. 메모리 관리와 동적 할당
- 스택, 힙, 데이터 영역
  - 프로그램의 실행 위해 기본적으로 할당하는 메모리 공간
  - 컴파일 타임에 함수에서 요구하는 스택의 크기가 결정 되어야 함
  - S/W적으로 나뉨, 가변적(위치, 파라미터...)

- 메모리 동적 할당
  - 런타임에 메모리 공간의 크기를 결정지어서 할당(힙 영역에 할당)
  - #include <stdlib.h>
  - void* malloc(size_t size)
    - memory allocate(메모리 할당)
    - falloc, calloc, valloc도 존재(메모리 할당 함수)
    - void*: 포인터 연산, value 참조 불가
	- cast해야
  - 성공 시 할당된 메모리의 첫 번째 주소 리턴, 실패 시 NULL 포인터 리턴

- 동적 할당된 메모리 공간의 소멸
  - #include <stdlib.h>
  - void free(void* ptr)

- malloc 함수 활용
  - int* i = (int*)malloc(sizeof(int));
    - int* i = (int*)malloc(4);
	- 4바이트 할당, 대입
	> int i[ x ];와 동일(sizeof(int)를 x라 할 때)

### 27장. 매크로와 전처리기
- 전처리기(pre-processor)에 의한 전처리
  - 전처리기: 컴파일러 지시자
  - ex: #include, #define

- #define으로 시작하는 전처리기 지시자
  - <>: System Header, 상대 경로에 의해
  - " ":  만든 Header, 절대 경로 full path
  - const int num = 10; == #define NUM 10;
  - define을 Header에 넣어 버리는 경우도 빈번

- 전처리기에 의한 매크로 처리(치환)
``` C
/* Preproc.c */
#include <stdio.h>

#define string "C++ Compatible C"
#define cal (3*4)+(12/4)

#define ONE 1
#define TWO ONE+ONE
#define THREE TWO+ONE

int main(void)
{
	printf("string: %s \n", string);
	printf("cal: %d \n", cal);
	printf("ONE=%d, TWO=%d, THREE=%d\n", ONE, TWO, THREE);

	return 0;
}
```

- 매크로 함수
  - 매크로를 기반으로 정의되는 함수
  - 함수X, 매크로! 다만 함수의 특성이 있을 뿐
  - #define SQUARE(x) x*x
    - 변수 독립적
  - 장점
    - 자료형에 독립적
    - 실행 속도 향상
  - 단점
    - 구현 어려움
    - 디버깅 어려움
  - 매크로 함수의 조건
    - 함수 크기 작아야
    - 그렇지 않으면 실행 파일 크기 증가

### 28장. 모듈화 프로그래밍
- 모듈
  - 프로그램 구성요소(함수, 기능(루프), 단위 S/W....)
  - 관련 데이터와 함수 묶여서 모듈 형성
  - 파일 단위로 나뉘는 게 보통

- 모듈화 프로그래밍
  - 기능별로 파일을 나눠가며 프로그래밍하는 것
    - 작업을 나눠서 할 수 있다.
	- 여러 사람
	- 시차(우선순위)
  - 유지 보수성 좋아짐

- 파일 분할, 컴파일
  - 파일 분할하더라도 완전한 독립 X
  - 파일 분할하더라도 상호 참조 발생 가능, 이는 전역 변수와 전역 함수로 
제한됨.
    - extern, 외부(내 소스x, 밖에서 컴파일하렴)의 변수, 함수를 
참조하겠다는 명시적 선언

- 외부 접근 금지
  - static 키워드에 의한 접근의 제한

- 링크
  - 선언된 함수의 정의를 찾아서 연결

- 헤더 파일의 포함
  - 전처리기에 의해 하나의 파일을 다른 하나의 파일에 포함시키는 작업

- 헤더 파일 포함 방법
``` C
#include <abc.h> // 표준 디렉토리에서 abc.h를 찾아서 포함
#include "c:/include/abc.h" // c:/include에서 abc.h를 찾아서 포함
```

- #if, #elif, #else, #endif 기반 조건부 컴파일 // elif, else, endif모두
최상단 if에 종속(중간에 if가 있다면 가까운 if)
	> if... else if... else 에서 마지막 else는 바로 위의 if문에 종속
```
#if CONDITION1
	expression1

#elif CONDITION2  
	expression2

#else 
	expression3

#endif
```

- 헤더 파일 포함 관계에서 발생하는 문제
  - 하나의 헤더 파일 두 번 이상 포함
    - 중복해서 함수 정의 되거나 변수가 선언되는 문제
    - 조건부 컴파일로 해결

### malloc
- 활용하기
``` C
// 210819
// 저장공간 크기, file에서 받기
// malloc 이용해 구조체 메모리 확보, 포인터로 공간 받아서 배열처럼 사용
// 포인터로 멤버변수 접근 시 화살표 사용

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
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
STU student[nArr];                       // 상수 nArr, STU student 막음


int main(void)
{
	double f_kor = 0.3, f_eng = 0.7;
	int i, j, k;
	//****************************************************************************
	//****************************************************************************
	//****************************************************************************
	int num;  //file에서 read
	STU* students;           // arr로 잡았던 것을 포인터로/ malloc으로 메모리 확보
	//****************************************************************************
	//****************************************************************************
	//****************************************************************************

	FILE* fin = fopen("C:\\Users\\snows\\table1.txt", "r");  // "r": read()  // table2의 data 가져올 수 있게	
	FILE* fout = fopen("C:\\Users\\snows\\table2.txt", "w+b");  // table2이라는 파일 생성해서 저장할 수 있도록
	fscanf(fin, "%d", &num);		// file에서 num 받아옴
	students = (STU*)malloc(sizeof(STU) * num);  // STU타입의 students 포인터에 malloc으로 저장공간 할당
												// sizeof(STU),  / 배열처럼 사용 가능

	for (i = 0; i < num; i++) fscanf(fin, "%s", students[i].name);   // 배열처럼 . 연산, 포인터로 하려면 화살표
	for (i = 0; i < num; i++) fscanf(fin, "%d", &students[i].kor);
	for (i = 0; i < num; i++) fscanf(fin, "%d", &students[i].eng);
	for (i = 0; i < num; i++)
	{
		students[i].tot = students[i].kor + students[i].eng;
		students[i].avg = students[i].tot / 2;
	}

	printf("Original :\n");
	printf("%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	for (int i = 0; i < num; i++)
	{
		printf("%7s %7d %7d %7.2f %7.2f\n", students[i].name, students[i].kor, students[i].eng,
			students[i].tot, students[i].avg);
	}

	sortSTU(students, num);

	printf("Sorted :\n");
	printf("%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	fprintf(fout, "%-7s %-7s %-7s %-7s %-7s\n\n", "이름", "국어", "영어", "총점", "평균");
	for (int i = 0; i < num; i++)
	{
		printf("%7s %7d %7d %7.2f %7.2f\n", (students + i)->name, (students + i)->kor, (students + i)->eng,
			(students + i)->tot, (students + i)->avg);    // 포인터로 멤버 변수 접근 시, 화살표 사용
		fprintf(fout, "%7s %7d %7d %7.2f %7.2f\n", (students + i)->name, (students + i)->kor, (students + i)->eng,
			(students + i)->tot, (students + i)->avg);    // fout -> stdout: printf와 같은 효과
	}
	fclose(fout);
	fclose(fin);

	return 0;
}
void sortSTU(STU* a, int n)
{
	int i, j, k;
	for (i = 0; i < n; i++)
	{
		for (j = i; j < n; j++)
		{
			if ((a + i)->avg < (a + j)->avg)  // 구조체 포인터의 값 =>a[i].avg < a[j].avg, a+i: i번째 구조체, ->avg:의 avg 멤버
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

### 여러 Source Program을 단일 System으로 묶어 주기
1. Menu 구성
  - 프로젝트 별
  - 1. DataType, 2. FuncTest, 3. gugudan, 4. Hello, 5. ArrayTest
    - ArrayTest 하에서는 또 소분류
	- 1. score();\n
	- 2. Good();\n
	- 3. PointerTest();\n
	- 4. solution1();\n
	- 5. SwapTest();\n
	- 6. sortTestNew();\n
	- 7. VoidTest();\n
	- 8. StreamTest();\n
	- 9. sortTestEx();\n
	- 0. Exit\n\n
	- ==================
	- Select Menu\n

``` C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <math.h>
#include <conio.h>
#include <time.h>

const int students = 20;
int score()
{
	int i, j, k;
	double kortotal = 0, engtotal = 0, mattotal = 0, koravg = 0, engavg = 0, matavg = 0, tot = 0, avg = 0;
	double kor[students], eng[students], mat[students];

	srand(time(NULL));
	printf("   KOSTA 학생들 국어, 영어, 수학 점수 현황\n================================================\n\n\n");
	for (i = 0; i < students; i++)
	{
		kor[i] = ((rand() % 1000) + 1) / 10.0;  // rand: 난수 발생기 (정수) 0 ~ 100.0   %1000하면 0~999, 1 더해주고 /10
		eng[i] = ((rand() % 1000) + 1) / 10.0;												// 0.1 ~ 100
		mat[i] = ((rand() % 1000) + 1) / 10.0;
		kortotal += kor[i];
		engtotal += eng[i];
		mattotal += mat[i];
		printf("%2d번째 학생 국어점수: %4.1lf  영어점수: %4.1lf  수학점수: %4.1lf  ", i + 1, kor[i], eng[i], mat[i]);
		printf("총점: %5.1lf  평균: %4.1lf\n", kor[i] + eng[i] + mat[i], (kor[i] + eng[i] + mat[i]) / 3);
	}
	koravg = kortotal / students;
	engavg = engtotal / students;
	matavg = mattotal / students;
	tot = kortotal + engtotal + mattotal;
	avg = tot / (students * 3);
	printf("국어 총점: %6.1lf, 평균: %4.1lf\n", kortotal, koravg);
	printf("영어 총점: %6.1lf, 평균: %4.1lf\n", engtotal, engavg);
	printf("수학 총점: %6.1lf, 평균: %4.1lf\n", mattotal, matavg);
	printf("전체 총점: %6.1lf, 전체 평균: %4.1lf\n", tot, avg);

	return 0;
}
int Good()
{
	char good[5] = "Good";
	char mon[] = "morning"; // [8]
	char noon[] = "afternoon"; // [10]
	char even[] = "evening"; // [8]
	int t;


	printf("지금 시각은? 24시간 기준으로 입력해 주세요.");
	scanf("%d", &t);

	if ((t >= 6) && (t < 12))
	{
		printf("%s %s\n", good, mon);
	}
	else if (t < 20)  // (t >= 12) && (t < 20)
	{
		printf("%s %s\n", good, noon);
	}
	else if (((t >= 0) && (t < 6)) || (t <= 24))  // ((t >= 20)&&(t <= 24))
	{
		printf("%s %s\n", good, even);
	}
	else
	{
		printf("잘못 입력하셨습니다.\n");
	}

	return 0;
}
int PointerTest()
{
	int a[3][2] = { 1,2,3,4,5,6 };
	int b[3] = { 1,2,3 };
	int(*pa)[2] = a;

	printf("a[0] : %d \n", a[0]);
	printf("a[0] : %d \n", a[0]);
	printf("a[1] : %d \n", a[1]);
	printf("a[2] : %d \n", a[2]);
	printf("a    : %d \n\n\n\n", a);

	printf("a   : %d \n", a);
	printf("a+1 : %d \n", a + 1);
	printf("a+2 : %d \n\n\n\n", a + 2);
	printf("%d", a + 1);
	return 0;
}

// function define 
//     Prototype  :  int str_len(char *str)
// 문자열 str을 받아서 해당 문자열의 길이를 되돌려 줌.
int str_len(char* str)  // parameter로 배열이 아닌 포인터로 받음, [] == *, str[]도 가능
{						// 전제: 마지막에 '\0'이 붙어있다.
	int i, res = 0;
	// while (*(str + res++)); return res; // 아래 함수 한 줄로. 아무것도 안 하고 루프만 돌게
	while (1)
	{
		if (str[res] != 0) res++;   // '0' -> 문자열의 마지막에 존재하는 NULL
		else break;
	}
	return res;
}

int solution1()
{
	char s[80];				// 배열명은 포인터
	int i, funcres;
	printf("문자열을 입력하시오.");
	scanf("%s", s);

	funcres = str_len(s);
	printf("입력문자열 [%s]의 길이는 %d 입니다 \n", s, funcres);

	for (i = 0; i < funcres; i++)     // 비교문 내에 함수 있으면 별로 안 좋음
	{									// loop돌 때마다 함수 계산, 속도 느려짐
		if (s[i] != 0)
			printf("%c ", s[i]);
		else
			break;
	}
	return 0;
}
// function define 
//     Prototype  :  void swap(int a, int b)
// 정수 변수 a와 b의 값을 교환.
void swap(int* a, int* b)	// a,b를 포인터로 선언하고 전달된 매개변수 값으로 설정 (초기화)
{							// 포인터 사용방법 : 포인터가 가리키는 주소의 값 : *p
							//					 주소 자체 : p
	int c = *a;
	//	printf("	Input  > a(%08x) : %d, b(%08x) : %d\n", a, *a, b, *b);
	*a = *b;
	*b = c;
	//	printf("	Result > a(%08x) : %d, b(%08x) : %d\n", a, *a, b, *b);
}

int SwapTest()
{
	int a = 50, b = 60;
	printf("Original> a(%08x) : %d,  b(%08x) : %d\n", &a, a, &b, b);

	swap(&a, &b);

	printf("After swap> a(%08x) : %d,  b(%08x) : %d\n", &a, a, &b, b);

	return 0;
}

void  sort(int* a, int n)
{
	int i, j, k;

	for (i = 0; i < n; i++)
	{
		for (j = i; j < n; j++)
		{
			if (a[i] < a[j]) swap(a + i, a + j); // =swap(&a[i], &a[j]); 
		}
	}
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

void swapEx2(const char** a, const char** b)
{
	const char* c = *a;
	*a = *b;
	*b = c;
}

//struct student {
//	int kor;
//	int eng;
//	char name[10];
//};
//typedef struct student STU;
typedef struct {
	char name[10];
	int kor;
	int eng;
	double tot;
	double avg;
} STU;

void SWAP(void* a, void* b, int op)
{
	if (op == 1)	// char
	{
		char c = *(char*)a;
		*(char*)a = *(char*)b;
		*(char*)b = c;
	}
	if (op == 4)	// int, flo
	{
		int c = *(int*)a;
		*(int*)a = *(int*)b;
		*(int*)b = c;
	}
	if (op == 8)	// double
	{
		double c = *(double*)a;
		*(double*)a = *(double*)b;
		*(double*)b = c;
	}
	if (op == sizeof(STU))	// STU
	{
		STU c = *(STU*)a;
		*(STU*)a = *(STU*)b;
		*(STU*)b = c;
	}
}
// 전역변수 : 이하의 함수에서 사용 가능
const int nArr = 10;
int kor[] = { 67, 70, 77, 65, 68, 72, 79, 55, 85, 61 };
int eng[] = { 70, 75, 80, 60, 65, 55, 80, 95, 67, 84 };
char nam[] = "ABCDEFGHIJK";//문자열 포인터로 변경 : "홍길동" "홍길이" "홍길삼" "홍길사" "홍길오" "길육" 길칠 길팔 길구 
const char* name[] = { "홍길동", "홍길이", "홍길삼", "홍길사", "홍길오", "맹일", "맹이", "맹삼", "맹사", "맹오" };
STU student[nArr];

void  sortEx(double* a, int n)
{
	int i, j, k;

	for (i = 0; i < n; i++)
	{
		for (j = i; j < n; j++)
		{
			if (a[i] < a[j])
			{
				//swapEx(a + i, a + j); // =swap(&a[i], &a[j]);  // tot : double
				//swap(kor + i, kor + j);
				//swap(eng + i, eng + j);
				//swapEx2(name + i, name + j);
				SWAP(a + i, a + j, 8);
				SWAP(student + i, student + j, 18);
				//SWAP(kor + i, kor + j, 4);
				//SWAP(eng + i, eng + j, 4);
				//SWAP(name + i, name + j, 4);
			}
		}
	}
}
void  sortSTU(STU* a, int n)
{
	int i, j, k;

	for (i = 0; i < n; i++)
	{
		for (j = i; j < n; j++)
		{
			if ((a + i)->avg < (a + j)->avg)	// 구조체 포인터의 값  ===> a[i].avg < a[j].avg
			{
				SWAP(a + i, a + j, sizeof(STU));
			}
		}
	}
}

void sortTest()	//  배열을 이용한 성적처리
{
	double f_kor = 0.3, f_eng = 0.7;
	double tot[nArr];
	int i, j, k;

	for (i = 0; i < nArr; i++)
	{
		tot[i] = kor[i] * f_kor + eng[i] * f_eng;
	}
	printf("Original :\n");
	printf("이름 : "); for (int i = 0; i < nArr; i++) printf("%7s ", name[i]); printf("\n\n");
	printf("국어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", kor[i]); printf("\n\n");
	printf("영어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", eng[i]); printf("\n\n");
	printf("합계 : "); for (int i = 0; i < nArr; i++) printf("%7.2f ", tot[i]); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");
	printf("이름 : "); for (int i = 0; i < nArr; i++) printf("%7s ", name[i]); printf("\n\n");
	printf("국어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", kor[i]); printf("\n\n");
	printf("영어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", eng[i]); printf("\n\n");
	printf("합계 : "); for (int i = 0; i < nArr; i++) printf("%7.2f ", tot[i]); printf("\n\n");
}

void sortTestNew()		// 구조체를 이용한 성적처리
{
	double f_kor = 0.3, f_eng = 0.7;
	double tot[nArr];
	int i, j, k;

	for (i = 0; i < nArr; i++)
	{
		student[i].kor = kor[i];
		student[i].eng = eng[i];
		strcpy(student[i].name, name[i]);

		tot[i] = student[i].kor * f_kor + student[i].eng * f_eng;
	}
	printf("Original :\n");
	printf("이름 : "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계 : "); for (int i = 0; i < nArr; i++) printf("%7.2f ", tot[i]); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");
	printf("이름 : "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어 : "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계 : "); for (int i = 0; i < nArr; i++) printf("%7.2f ", tot[i]); printf("\n\n");
}

void sortTestEx()		// 구조체를 이용한 성적처리 - 파일 입출력
{
	double f_kor = 0.3, f_eng = 0.7;
	double tot[nArr];
	int i, j, k;

	int m, n;
	char buf[1024];
	FILE* fin = fopen("C:\\Users\\snows\\table3.txt", "r");  // "r": read()  // table2의 data 가져올 수 있게	
	FILE* fout = fopen("C:\\Users\\snows\\table4.txt", "w+b");  // table4이라는 파일 생성해서 저장할 수 있도록

	for (i = 0; i < nArr; i++) fscanf(fin, "%s", student[i].name);
	for (i = 0; i < nArr; i++) fscanf(fin, "%d", &student[i].kor);
	for (i = 0; i < nArr; i++) fscanf(fin, "%d", &student[i].eng);
	for (i = 0; i < nArr; i++)
	{
		student[i].tot = student[i].kor + student[i].eng;
		student[i].avg = student[i].tot / 2;
	}
	printf("Original :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", student[i].tot); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");

	printf("이름	국어	영어	합계	\n");
	fprintf(fout, "이름	국어	영어	합계	\n");  // table4에 써질 내용
	for (int i = 0; i < nArr; i++)
	{
		printf("%7s%7d%7d%7.2lf\n", student[i].name, student[i].kor, student[i].eng, student[i].tot);
		fprintf(fout, "%7s%7d%7d%7.2lf\n", student[i].name, student[i].kor, student[i].eng, student[i].tot);   // table4에 써질 내용
	}
}

void VoidPrint(void* p, int i)
{
	if (i == 1)
	{
		char* cp = (char*)p;
		printf("%c\n", *cp);
	}
	if (i == 2)	printf("%d\n", *(int*)p);
	if (i == 3)	printf("%f\n", *(double*)p);
}

void VoidTest()
{
	char c = 'z';
	int  n = 10;
	double a = 1.414;

	void* vp;
	VoidPrint(&c, 1);
	VoidPrint(&n, 2);
	VoidPrint(&a, 3);
}

void StreamTest()
{
	char buf[1024];
	FILE* f = fopen("C:\\Users\\snows\\aa", "r");  // "r": read()  // 없는 파일이면 NULL 반환
	FILE* fout0 = fopen("C:\\Users\\snows\\aa.o0", "w");  // "w": write,  fout0은 file pointer, write는 새 결과로 갈아치움 !=append
	FILE* fout1 = fopen("C:\\Users\\snows\\aa.o1", "a");  // 
	FILE* fout2 = fopen("C:\\Users\\snows\\aa.o2", "a+b");  // append: 새 결과는 기존 결과 밑에 덧붙임
	if (f != NULL)
	{
		while (1)										// "w": write()
		{
			if (fgets(buf, 1024, f) == NULL) break;		// 파일 입력 함수  , 한 줄 단위로 읽어옴
			//if (strlen(buf) < 3) break;  // 엔터도 0 아님. Ctrl+C로 탈출
			//fputs("==== 입력문자열 =====>", stdout);
			fputs(buf, stdout); // 화면 출력
			fputs(buf, fout0);
			fputs(buf, fout1);
			fputs(buf, fout2);
		}
	}
	else printf("입력 파일이 존재하지 않습니다.\n");
}


int main()
{
	int pick = 0, stop = 1;

	while (stop)
	{
		printf("\n\n***** 프로그램 목록 *****\n\n");
		printf("	1. score();\n");
		printf("	2. Good();\n");
		printf("	3. PointerTest();\n");
		printf("	4. solution1();\n");
		printf("	5. SwapTest();\n");
		printf("	6. sortTestNew();\n");
		printf("	7. VoidTest();\n");
		printf("	8. StreamTest();\n");
		printf("	9. sortTestEx();\n");
		printf("	0. Exit\n");
		printf("=========================\n");
		printf("	Select Menu\n\n");

		scanf("%d", &pick);
		switch (pick)
		{
		case 1: score(); break;
		case 2: Good(); break;
		case 3: PointerTest(); break;
		case 4: solution1(); break;
		case 5: SwapTest(); break;
		case 6: sortTestNew(); break;
		case 7: VoidTest(); break;
		case 8: StreamTest(); break;
		case 9: sortTestEx(); break;
		case 0: stop = 0; break;
		default: stop = 0; break;
		}
	}
}
```
















