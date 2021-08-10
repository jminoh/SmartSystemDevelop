###복습

+ 코드 작성 시 유의점
  1. 읽기 쉽게(나 && 남)

10장. 1차원 배열

배열과 포인터
  - int, char(단일 문자), 문자열
- 배열 선언
  - 배열 길이([상수], 변수 쓰고 싶으면 const) > 저장공간 확보하기위해
  - 배열 요소 자료형(data type)
  - 배열 이름
- 1차원 배열의 접근
  - 배열 요소의 위치 표현: index[0]~ (Zero base)
- 배열 선언과 접근
  - int array[10];
  - array[0] = 3.... > 인덱스로 접근 가능
-  선언과 동시에 초기화
  - int arr1[5] = {1,2,3,4,5};
  - int arr2[] = {1,3,5,7,9};
  - int arr3[5] = {1,2};

- 문자열(문자의 배열) 상수
  - 문자열이면서 상수의 특징 지님
-  문자열 변수
  - 문자열이면서 변수의 특징
  - char str1[5] = "Good";
  - 문자열 맨 뒤에 '\0'(NULL) 있음

- 문자열과 char형 배열의 차이
  - char arr1[] = "abc"; > NULL O
  - char arr2[] = {'a','b','c'} > NULL X
  - char arr3[] = {'a','b','c','\0'}
  - *arr1 == *arr3

11장. 다차원 배열

- 다차원 배열의 실제 메모리 ㄱ성
  - 1차원 배열과 동일, 접근 방법이 2차원적일 뿐

- 초기화 리스트에 의한 배열 크기의 결정
  - 1차원: int arr[] = {1,2,3,4,5};
  - 2차원: int arr[][4] = {1,2,3,4,5,6,7,8}처럼 열은 줘야 함
		========================

- 3차원 배열의 선언과 의미
  - 3차원적 메모리 구조, 큐빅모양

12장. 다차원 배열 그리고 포인터(*)

- '*': 포인터 변수의 선언자, 간접참조연산자
- 포인터: 주소를 가리키는 변수, 포인터 변수의 값은 어떤 변수의 주소
  - 간점참조연산자를 이용해 저장한 주소에 해당하는 변수의 값을
나타낼 수 있음
	- int: 정수를 가리키는 변수
	- float: 실수를 가리키는 변수
	- char: 문자를 가리키는 변수
- byte: Address로 쓰이는 가장 작은 단위
  - 모든 byte에 주소 있음
- '**' 이중포인터: 포인터를 가리키는 포인터
	|add|data|add|data|add|data|add|data|
	|---|---|---|---|---|---|---|---|
	|0001|1|0002|2|0003|3|0004|4|
	|0005|5|0006|6|0007|7|0008|8|
	|0009|9|000a|a|000b|b|000c|c|
    1. 1차원 배열	
	- int arr1[12];
	- arr1[2] = *(arr1 + 2) = 3
	- arr1 + 2 = 0003(주소)
    - void function(int *a){ }
        - function(arr1);
    
    2. 2차원 배열
	-  int arr2[3][4];
    - void function2(int **a){ }
        - funciton2(arr2); XXXXXXX
	- arr2[1][2] = *(*(a + 1) + 2) = 7

- 1차원 배열 이름의 포인터 타입 결정
  - 포인터가 가리키는 요소의 자료형
  - 포인터 연산(+, ++, -, --) 시 증가하는 바이트의 크기

- 포인터 연산은 데이터 타입에 영향받음
- [ ] == *
- 배열명 = 포인터

+ 실습 1
  - scanf 함수 이용해 문자열 입력 후, 해당 문자열을 한 글자씩 공백
삽입하여 출력
    - 문자열 길이 알아내는 함수

``` C
#include <stdio.h>
// function define
// Prototype: int str_len(char *str)	문자열(문자 배열, 포인터)을 파라미터로 받을 때 * 사용
// 문자열 str을 받아서 해당 문자열의 길이를 되돌려 줌.
int str_len(char* str);
int main(void)		
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
}
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
```
  > strlen이라는 함수 존재(내용은 같음)

    - #include <string.h>
    - return strlen(str);

  + Low level function: 함수 자체에 다른 함수를 포함하지 않은 함수, 
위에서 만든 str_len같은 함수들
	> 100% 본인 자료

## 오늘

+ Swap 함수

- '*' 대신 [ ]사용해도 됨. 초기화를 []로 하지 않았기 때문에 swap 과정에서
[0]로 사용.

``` C
void swap(int *a, int *b) // a, b를 포인터로 선언하고 전달된 매개변수 값으로 설정 (초기화)
{		// 포인터 사용방법: 포인터가 가리키는 주소의 값: *p
		// 		주소 자체: p
	int c = *a;   // 아래에서 &붙인 것은 포인터의 주소 
	// printf("	Input    > a(%08x) : %d, b(%08x) : %d\n", &a, a, &b, b);
	*a = *b;   // 값을 연산하기 위해 * 붙여야
	*b = c;
	// printf("	Result    > a(%08x) : %d, b(%08x) : %d\n", &a, a, &b, b);
}

int SwapTest()  // %8x: %x는 16진수, 8은 8자리, 08이면 빈자리는 0인 8자리
{
	int a = 50, b = 60;
	// printf("Original> a(%08x) : %d, b(%08x) : %d\n", &a, a, &b, b);

	swap(&a, &b); // swap을 위해 값이 아니라 주소 넘김.

	printf("Original> a(%08x) : %d, b(%08x) : %d\n", &a, a, &b, b);

	return 0;
}
```	

+ 정렬 Sorting

``` C
// Sort, 이중 반복문이 기준. 가장 normal한 정렬방식
#include <stdio.h>
void sort(int *arr, int nArr);
int main(void)
{
	int nArr = 11;
	int arr[] = { 25, 27, 30, 47, 35, 68, 40, 79, 45, 85, 50 };

	printf("Original : "); for (int i = 0; i < nArr; i++) printf("%d  ", arr[i]);
	printf("\n\n");
	
	sort(arr, nArr);
	printf("Sorted : "); for (int i = 0; i < nArr; i++) printf("%d  ", arr[i]);
	printf("\n\n");

}

void sort(int *arr, int nArr)  // a[i]기준으로 그 뒤의 것들 비교
{								// a[i] 이전 것들은 이미 큰 것부터 정렬됨.
	int i, j, temp;
	for (i = 0; i < nArr; i++)
	{
		for (j = i; j < nArr; j++) // j = i;여도 됨
		{
			if (arr[i] < arr[j])
			{
				temp = arr[i];
				arr[i] = arr[j];
				arr[j] = temp;
			}
			// if(a[i] < a[2]) swap(&a[i], &a[j]); // swap(a +  i, a + j);
		}
	}
}
```

### 실습

+ 두 과목의 성적이 다음과 같을 때 배열을 이용하여 초기화하고,
각각의 성적에 가중치를 곱한 후 개인별 합계를 구하여 합이 큰 순서대로
정렬하여 출력하라. (국어 가중치 0.3, 영어 가중치 0.7)
  - 자료(예)
    |이름|A|B|C|D|E|F|G|
    |----|---|----|----|----|----|----|----|
    국어| 82|  93|  71|  69|  78|  84 | 75 |
    영어| 76|  91|  67|  73|  86|  63 | 83 | 

``` C
//4강 마지막 실습
#include <stdio.h>
const int students = 7;
void swapTest(double sorting[][students]);  // 선언, 정의 때는 배열정보도!!!
int main(void)
{
	int kor[] = { 82, 93, 71, 69, 78, 84, 75 };
	int eng[] = { 76, 91, 67, 73, 86, 63, 83 }; // 배열 개수: sizeof(배열명)/sizeof(배열요소)
	double sort[2][students];
	int i, j;

	for (i = 0; i < students; i++)
	{
		sort[0][i] = (kor[i] * 0.3) + (eng[i] * 0.7);
		sort[1][i] = i + 65;    // 아스키코드 A
	}

	swapTest(sort);      // 호출 때는 무조건 배열명만!!!

	// 안 되는 거.
	//for (j = 0; j < students; j++)
	//{
	//	sort[1][i] = (int)sort[1][i];		// 배열이 double, (int)형변환 이후 다시 double에 넣어주면 
	//	printf("%d\n", sort[1][i]);			// 소수점만 없애고 double에 다시 넣은 격. 안 됨.
	//}

	printf("순위:  이름  국어  영어  합계\n");
	for (i = 0; i < students; i++)
	{
		printf("%4d:%5c%7d%7d%7.2lf\n", i+1, (int)sort[1][i], kor[(int)sort[1][i] - 65], eng[(int)sort[1][i] - 65], sort[0][i]);
	}										// 다른 배열이기 때문에 직접적으로 표시할 수는 없으나, 이름도 최초에 정렬되었기 때문에 이름을 이용
}							// 이름 표현 위해 붙어있는 아스키코드 사용. int로 바꾸고 -65 하면 최초의 인덱스 나옴. 해당 인덱스를 kor배열에 사용.

void swapTest(double sorting[][students])
{
	double temp, tempch;
	int i,j;
	/*for (j = 0; j < students; j++)
	{
		for (i = 0; i < students; i++)
		{
			if (sorting[0][i] < sorting[0][i + 1])
			{
				temp = sorting[0][i];
				sorting[0][i] = sorting[0][i + 1];
				sorting[0][i + 1] = temp;

				tempch = sorting[1][i];
				sorting[1][i] = sorting[1][i + 1];
				sorting[1][i + 1] = tempch;

			}
		}
	}*/
	for (i = 0; i < students; i++)
	{
		for (j = i; j < students; j++)
		{
			if (sorting[0][i] < sorting[0][j])
			{
				temp = sorting[0][i];
				sorting[0][i] = sorting[0][j];
				sorting[0][j] = temp;

				tempch = sorting[1][i];
				sorting[1][i] = sorting[1][j];
				sorting[1][j] = tempch;
			}
		}
	}
}
```
### 강사님 코드
``` C
// 4강 마지막 실습 강사님
#include <stdio.h>
void sortEx(double* arr, int n); 
void swap(int* a, int* b);
void swapEx(double* a, double* b);
void swapEx1(char* a, char* b);
int kor[] = {67, 70, 77, 65, 68, 72, 79, 55, 85, 61};  //  전역 변수화: 이하의 함수에서 사용 가능
int eng[] = {70, 75, 80, 60, 65, 55, 80, 95, 67, 84};
char nam[] = "ABCDEFGHIJK";

int main(void)
{
	const int nArr = 10;
	double f_kor = 0.3, f_eng = 0.7;

	double tot[nArr];
	int i, j, k;

	for (i = 0; i < nArr; i++)
	{
		tot[i] = kor[i] * f_kor + eng[i] * f_eng;
	}
	printf("Original :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7c ", nam[i]); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", kor[i]); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", eng[i]); printf("\n\n");
	printf("합계: "); for (int i = 0; i < nArr; i++) printf("%7.2lf ", tot[i]); printf("\n\n");

	sortEx(tot, nArr);

	printf("Sorted :\n");
	printf("이름: "); for (int i = 0; i < nArr; i++) printf("%7c ", nam[i]); printf("\n\n");
	printf("국어: "); for (int i = 0; i < nArr; i++) printf("%7d ", kor[i]); printf("\n\n");
	printf("영어: "); for (int i = 0; i < nArr; i++) printf("%7d ", eng[i]); printf("\n\n");
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

				swapEx(arr + i, arr + j);
				swap(kor + i, kor + j);
				swap(eng + i, eng + j);
				swapEx1(nam + i, nam + j);
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
```
- Quiz
  - 생문, 사문, 보초 둘, 하나는 거짓말쟁이
  - 질문 하나로 어떻게 선택?
    - "어디가 나가는 곳입니까?"라고 옆에 애라고 물으면 뭐라고 할까요?"
& 반대로 나가면 됨
  - T * F = F or F * T = F
  - 1 * 0 = 0 or 0 * 1 = 0
  - 논리곱

## 5강 포인터 > C에서 가장 중요한 부분
### 13장 포인터의 이해
- 포인터와 포인터 변수
  - 메모리의 주소 값을 저장하기 위한 변수
  - 주소 값과 포인터는 다름
  - 32bit system 기반: 4byte(Address 체계)
    - 포인터는 4byte(int, long, float와 같음)
    - 포인터의 증감연산: 포인터의 Data type에 따라 달라짐
	- data type이 int면 증감 단위는 4byte, char면 1byte

- 포인터의 타입과 선언
  - 포인터 선언 시 사용되는 연산자 '*'
  - A형 포인터(A*): A형 변수의 주소 값을 저장.
  - '*a = a[0]' (선언부는 x)

- 주소 관련 연산자
  - & 연산자: 변수의 주소 값 반환(변수에 단독으로 붙어있을 경우)
    - cf. 두 피연산자 사이의 &은 비트연산자
  - '*' 연산자: 포인터가 가리키는 메모리 참조(값)

- 포인터에 다양한 타입이 존재하는 이유
  - 포인터 타입은 참조할 메모리의 크기 정보를 제공

> 포인터 선언과 동시에 초기화 하지 않으면, 
쓰레기 값으로 초기화. 그 후에 값 넣게 되면, 쓰레기값이 있던
영역이 어디냐에 따라(시스템 영역) 오류 가능성 존재.

> 포인터 초기화를 일반 상수로 사용. 일반 상수가 가리키는 영역이 
어디일지 모름

### 14장 포인터와 배열! 함께 이해하기
- 배열명은 배열 첫 번째 요소의 주소 값
- 배열명과 포인터 비교

|비교대상|포인터|배열명|
|------|------|-----|
|이름이 존재하는가|있다|있다|
|무엇을 나타내는가|메모리의 주소|메모리의 주소|
|변수인가 상수인가|변수|상수|

- 배열명의 타입
  - 배열명도 포인터이므로 타입이 존재
  - 배열명이 가리키는 배열요소에 의해 결정

- 포인터와 배열을 통해 얻을 수 있는 결론
  > [ ] == '*'

- 문자열 표현 방식의 이해
  - 배열 기반의 문자열 변수
  - 포인터 기반의 문자열 상수
  - 문자열 맨 뒤에는 항상 '\0'(NULL)
    - 문자로 하나하나 넣으면 NULL 안 들어감

- 포인터 배열
  - 배열의 요소로 포인터를 지니는 배열
  - 어떻게 보면 2차원 배열과 동일
  - 문자열 배열에 가장 많이 사용
``` C
char* arr[3] = {"Fervent-lecture", 
		"TCP/IP", 
		"Socket Programming"};
```
	+ arr[0] -> "Fervent-lecture"
	+ arr[1] -> "TCP/IP"
	+ arr[2] -> "Socket Programming"




- 내일
# 6강 구조체와 사용자 정의 자료형
## 22장. 구조체와 사용자 정의 자료형 1
+ C++ 베이스로 MFC Programming
  - 독자적인 Package 만들기
  + C#으로 쉬운 Application 만들기






