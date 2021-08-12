## Visual Studio, Github와 연결
- Repositrory: (대용량)저장소
	- Storage: (HDD느낌) 저장소
- commit: 

1. Github에 repos 생성
2. VS에 Repository 복제
3. Local Directory 자동 생성
  > 경로는 기존 폴더 X
4. 기존 작업은 Local 저장소에 복사
5. solution 열기(작업 위해)
6. 작업, 저장
7. Git 변경 내용 선택, commit, push
  > commit 하려면 작업 내용 저장 후 솔루션을 껐다 켜야 가능함.
  > 솔루션 끄면 해당 폴더 bash에서도 작업 가능함

- 온라인으로 이루어짐
- 웹에서 에디팅 가능
- 팀원들과 협업 가능
- 리더가 모아서 테스트하는 것이 좋음


## 6강. 구조체와 사용자 정의 자료형
- Struct 키워드 이용해, 여러 변수들을 그룹으로 묶어서 관리하는 것
  - 배열은 동일한 자료형
  - Struct는 멤버들마다 다른 자료형 사용 가능

- Struct 구조체이름 구조체body
  - body안의 변수를 멤버라고 함.
  - 위의 셋을 묶어서 구조체 자료형 선언, 정의
    - int, char등은 설계자에 의해 선언, 정의되어 있는 것
  - 구조체가 새로운 자료형이 되는 것
``` C
struct point {	// 구조체 정의, Header에 많음
    int x;
    int Y;
} p1, p2, p3;  // p1, p2, p3이라는 point type(구조체) 변수 선언  

    struct point p4;   // 변수 선언
    p4.x = 10;   // '.'을 통해 구조체 변수의 멤버
    p4.y = 20;
    
    struct point p5 = { 40, 50 }; // 선언 및 초기화
```

- 구조체 배열의 선언
``` C
struct person {
    char name[20];
    char phone[20];
    int age;
};

int main(void){
    struct person pArray[10];
    pArray[1].phone[2] = 0;   // 011 -> 010 // pArray[구조체배열의 인덱스]
			// .phone[char[]의 세 번째 자리
```
- 구조체 배열 요소의 접근
  - pArray[1].age = 10;

- 구조체와 포인터
  - 구조체도 data type이기 때문에 포인터 쓸 수 있음
  - 해당 구조체 변수의 멤버를 포인터로 사용시 '->'이용

- 구조체 변수와 주소 값의 관계
  - sizeof함수를 통해 변수의 byte 알아낼 수 없음
  - 어떤 변수로 몇 개가 어떻게 선언되어 있는지에 따라 size 다르기 때문

### 23장. 구조체와 사용자 정의 자료형 2

- 일반 변수
  - Call by Value
  - Call by Reference

- 함수의 인자로 전달되는 구조체 변수
  - 구조체 변수의 인자 전달 방식은 기본 자료형 변수의 인자 전달
방식과 동일
- 구조체 변수의 연산
  - 허용되는 대표적인 연산은 대입연산(=)이며, 이외의 사칙 연산들은
적용 불가능
    - int i = 10, j = 20; // +-*/%!&|^... 등의 연산자 이용 가능
    - 구조체 변수는 위의 것들 전부 불가능, 값 넣는 =만 사용
	> 구조체 변수 내의 멤버들은 각 자료형에 대해 가능한 
연산들 모두 가능.
    - 배열이라고 생각하면 이해가 됨. (배열도 위의 연산 안 됨)
  - 인자 전달의 기본인 Call by Reference로 전달, Call by Value X
  > 구조체도 포인터다!

- 구조체 변수의 리턴 방식
  - 기본 자료형 변수의 리턴 방식과 동일
  - Struct 용량 정해져 있지 않기 때문에 무한히 커질 수 있음
    - 함수를 타고 들어가다 보면 Stack 넘칠 수 있음
	> Stack Overflow
    - 논리분석으로 해결이 안 됨.

> 구조체는 배열과 마찬가지로 포인터로 연산
  - 화살표 많이 쓰게 됨

- 중첩된 구조체
  - 구조체의 멤버로 구조체 변수가 오는 경우
```C
struct point {
  int x;
  int y;
};
struct circle {  // 중점의 좌표 x와 y, 반지름 r
  struct point p;
  double radius;
};

int main()
{
  struct circle c = {1, 2, 3.0};
}
```

- 중첩된 구조체 변수의 초기화 방식
  - 필요한 순서대로 나열
    - struct circle c = {1, 2, 3.0};
        - 멤버의 일부 값만 초기화할 경우 원하는 값이 나오지 않을 수 있음.
  - 내부를 묶어줌
    - struct circle c = {{1, 2}, 3.0};
    - 이 쪽이 더 좋음
        - 멤버의 일부 값만 초기화하는 경우 있을 수 있음
	- struct circle c = {{1}, 2};

- typedef 키워드의 이해
  - typedef int INT; // INT라는 이름을 기본 자료형 int에게 지어줌
	- int를 INT로 쓰겠다고 컴파일러에게 알려주는 것
  - 자료형에 새로이 이름을 지어주는 것. > 별칭!!!!
  - struct에서 많이 사용 <- 구조체 변수를 선언하거나 사용할 때
매번 struct를 사용해서 구조체임을 명시해야 하기 때문에
    - struct point ....
    - ->Point로 바꿔서 사용 가능(struct 안 써도 됨)
  - 가독성 향상이 목표
  - 별명을 붙여주는 것이기 때문에, 원래대로 써도 됨.

``` C
struct Data
{
	int data 1;
	int data 2;
};
typedef struct Data Data;  // data 타입의 data 구조체 변수 선언
```
	> Data a1, a2; 가능해짐
	- 사용하는 case 있음
	- 위와 아래 같은 것.

``` C
typedef struct Data
{
	int data1;
	int data2;
}Data;
```


- 구조체 이름의 생략
``` C
typedef struct Data{	
  	int data1;
  	int data2;
  }Data;
```
  - Data로 쓸 때는 문제 없음
  - 밑에 가서 sturct Data... 라고 써도 됨(이름이 있기 때문)

 !=

``` C
typedef struct    // 구조체 이름 없음
{
	int data1;
	int data2;
}Data;               // 별칭
```
  - 별칭으로 구조체 변수 선언, 접근 가능
    - Data d1; 과 같이 하면 됨


- 공용체(union)의 특성   *** C에서의 모든 keyword는 소문자!
  - 생김새는 구조체랑 동일
  - 하나의 메모리 공간을 둘 이상의 변수가 공유하는 형태
  - only C, 타 언어에서는 불가능
  - 고속처리를 필요로 하는 통신(1byte단위) 영역에서 사용하는 경향 있음
``` C
union u_data {
  int d1;
  double d2;
  char d3;
};
```
	### 많이 쓰는 형태
``` C 
union data {
	char arr[8];    // 8byte
	int arr1[2];    // 8byte
	double a;     // 8byte
};		// H/W 약속된 시간에 연달아 정보 보내줄 때
```
	> 통신영역(1byte) -> char로 받음
	> int나 double로 전환과정 없이 사용 가능

- Enum(열거형)의 정의와 의미
  - 무조건 정수
  - 정수 데이터에 다른 별명 붙여 놓은 것
  - enum color {RED=1, GREEN=3, BLUE=5};
	- color라는 이름의 자료형 선언
	- 상수 RED(1), GREEN(3), BLUE(5)의 선언
  - enum color c	// 열거형 color의 변수 c를 선언
	- c = RED;	// c에 RED 대입
	- c = GREEN;	// c에 GREEN 대입 -> 3을 넣음
	- c = BLUE;	// c에 BLUE 대입
			// 중간에 대입 x -> 알아서 +1함 
	
	> 0, 1, 2, 3, 4, 5....
	> enum week { sun, mon, tue, wed ... sat};
	> 순서대로(sun=0, mon=1....)
	> **날짜처리** 에 많이 사용

  - 열거형 사용하는 이유
    - 특정 정수 값에 의미 부여 가능
    - 프로그램 가독성 높일 수 있음

### C 문법 끝

### 성적처리 프로그램 구조체로 바꾸기
``` C
struct student {
	int kor;
	int eng;
	char name[10];
};
```
- 위의 구조체를 이용하여 사용자 정의 자료형을 선언하고, 10명의 학생에
대한 데이터를 입력한 후 정렬하여 출력하시오.
  - 사용자 정의 자료형: typedef

``` C
// 6장 구조체 실습 강사님
/*struct student {
	int kor;
	int eng;
	char name[10];
};*/

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

int kor[] = { 67, 70, 77, 65, 68, 72, 79, 55, 85, 61 };  //  전역 변수화: 이하의 함수에서 사용 가능
int eng[] = { 70, 75, 80, 60, 65, 55, 80, 95, 67, 84 };
const int nArr = 10;
char nam[] = "ABCDEFGHIJK";
const char* name[] = { "김씨", "이씨", "박씨", "최씨", "안씨", "정씨", "소씨", "조씨", "허씨", "심씨" };  // 포인터 배열, String Array
STU student[nArr];

int main(void)
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
		//tot[i] = (student[i].kor = kor[i]) * f_kor + (student[i].eng = eng[i]) * f_eng; // 위 세 주석 한 줄로 합침.
	}											// 점수를 구조체의 멤버변수에 대입하고, 가중치 곱함. 가중치 곱해진 두 점수를 더해 tot에 대입.
	printf("Original :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", tot[i]); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7s ", student[i].name); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].kor); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", student[i].eng); printf("\n\n");
	printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", tot[i]); printf("\n\n");
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





