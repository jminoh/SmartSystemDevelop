### Review

### 7강 C 표준함수
- 가장 중요한 것은 String(문자열, char*) 처리
  - 타 언어에서 클래스로 많이 사용

- 표준 입출력 스트림
  - stdin, stdout: FILE*(ex. fin, fout)대신 사용

- 문자 출력 함수
  - printf
  - fprintf: 파일 출력
  - sprintf: string buffer(메모리 공간) 출력
  - printfk: kernel 출력, 디버깅용
  - 출력 방향이 다를 뿐, 다 printf

- 문자 입력 함수
  - scanf
    - 문자열 입력 시, 공백 단위로 끊어서 입력
  - fgets: 문자열 입력

- 문자열, C에서는 datatype X
  - 문자열 대입(=), 연산(+,-,*,/), 비교(<,>,<=,>=) 불가
  - 표준함수 제공
  - strcpy, strcat(+(결합)만 가능), strcmp

- 문자열을 숫자로 변환하는 함수
  - atoi(ascii to integer)
  - parsing 

- 대소문자의 변환을 처리하는 함수들
  - toupper
  - tolower

### 25장. 파일 입출력
- 파일: 절대적인 위치 필요
  - FILE* fopen(const char* filename, const char* mode)
  - filename에 전체경로 써야
  - current dir일 경우 파일명만 써도 됨
  - 파일 OPEN mode
    - 파일 접근 무도 + 데이터 입출력 모드
    - w(overwrite), a(append), r(read) + t(text, 기본, CR자동), 
b(binary, LF만)

- Random Access

### 26장. 메모리 관리와 동적 할당
- malloc
  - void*
    - 메모리 연산 불가, Value access 불가
  - casting 필수
  - free: 사용 끝나면 메모리 공간 소멸
    - void free(void* ptr)

### 27장 매크로와 전처리기
- #define으로 시작하는 전처리기 지시자
  - 컴파일러가 처리하는 것 X
  - 전처리기에서 단순 치환을 요청 시 사용되는 지시자
  - 불변의 상수에 사용

- 매크로 함수
  - 매크로 기반 정의되는 함수
  - 매크로인데 함수의 특성 가짐
  - 단순한 내용으로 만들어야
    - #define Max(x, y) (x>y)?x:y
    - 자료형에 상관없이 수행 가능

### String parse 실습
- b[0] == *b == *(b + 0)

## GetInt() fgets로 숫자 받고, 개행 문자 없애려고 했을 때
문자 사이를 스페이스로 나누면 뒷 문자 0으로 나옴
- 그냥 fgets가 엔터 아니면 안 넘어감

``` C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <math.h>
#include <conio.h>
#include <time.h>
#include <iostream>

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

void sort(int* a, int n)
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

#define KBD_BUF_SIZE 20
#define MAX(x,y) x>y?x:y  // datatype에 종속 X
#define MIN(x,y) x<y?x:y

int GetInt()         // fgets 썼을 때 스페이스로 다음 숫자 받지 않음. 무조건 엔터
{
	char buf[KBD_BUF_SIZE];
	//char nu = '\0';
	fgets(buf, KBD_BUF_SIZE, stdin);
	/*if (buf[strlen(buf) - 1] == '\n')    // 얘네도 같음
		buf[strlen(buf) - 1];*/
	//strcpy(buf + (strlen(buf) - 1), &nu);  // 두 수 사이에 스페이스 들어가면 뒷 수가 0나옴
	return(atoi(buf));						// 디버거에서는 괜찮음.
}
double GetDouble()
{
	char buf[KBD_BUF_SIZE];
	//char nu = '\0';
	fgets(buf, KBD_BUF_SIZE, stdin);
	//strcpy(buf + (strlen(buf) - 1), &nu);  // 두 수 사이에 스페이스 들어가면 뒷 수가 0나옴
	return(atof(buf));
}
void StringParse() // 문자열을 입력받아서 int, double, 문자열 입력을 수행(scanf 사용 x)
{
	int k;
	char b[5];
	while (1)
	{
		printf("\n\n\n ==== 문자열 변환 테스트 =================\n");
		printf("1. 정수 (int)\n");
		printf("2. 실수 (double)\n");
		printf("3. 문자열 (공백포함)\n");
		printf("4. 매크로 함수 테스트\n");
		printf("0. 나가기\n");
		printf("==============================================\n");
		printf("	Select Menu\n\n");
		scanf("%d", &k); 
		getchar();  // scanf로 입력할 경우 버퍼에 개행문자가 남기 때문에, 개행문자를 빼 주기 위해
		// fgets(b, 5, stdin);
		
		if (k == 1)   // (b[0] == 0x31)   // 0x31 == 49 == 1
		{
			char buf[KBD_BUF_SIZE];
			printf("정수을 입력하세요 : ");		//fgets는 키보드에서 enter가 입력될 때까지 받음
			fgets(buf, KBD_BUF_SIZE, stdin);    //stdin: Keyboard, 무지막지하게 큰 수를 입력한다면, 반환되는 값은 2147483647이 나옴.(2G)
			int n = atoi(buf);					// 정수값으로 변환할 수 없는 문자가 입력되면, 0을 반환함
			printf("변환된 정수값은 %d 입니다.\n\n", n);
		}
		else if (k == 2)
		{
			char buf[KBD_BUF_SIZE];
			printf("실수을 입력하세요 : ");
			fgets(buf, KBD_BUF_SIZE, stdin);    //stdin: Keyboard
			double f = atof(buf);					// 실수값으로 변환할 수 없는 문자가 입력되면, 0.000000을 반환함
			printf("변환된 실수값은 %lf 입니다.\n\n", f);
		}
		else if (k == 3)
		{
			char buf[KBD_BUF_SIZE];
			printf("문자열을 입력하세요 : ");
			fgets(buf, KBD_BUF_SIZE, stdin);    //stdin: Keyboard, 무지막지하게 큰 수를 입력한다면
			//int n = atoi(buf);
			char nu = '\0';
			strcpy(buf + (strlen(buf) - 1), &nu);
			printf("변환된 문자열은 %s 입니다.\n\n", buf);
		}
		else if (k == 4)
		{
			int x, y; 
			double x1, y1;
			printf("두 개의 정수를 입력하세요 : ");
			//scanf("%d%d", &x, &y);	//GetInt
			x = GetInt(); y = GetInt();
			printf("두 개의 정수 %d와 %d 중 큰 수는 %d 입니다.\n\n\n", x, y, MAX(x, y));
			printf("두 개의 실수를 입력하세요 : ");
			//scanf("%lf%lf", &x1, &y1);	//GetDouble
			x1 = GetDouble(); y1 = GetDouble();
			printf("두 개의 실수 %lf와 %lf 중 작은 수는 %lf 입니다.\n\n\n", x1, y1, MIN(x1, y1));
		}
		else if (k == 0) break;
	}
}


int main()
{
	std::cout << "Hello C PlusPlus (C++) World!!!!";
	int pick = 0, stop = 1;

	while (stop)
	{
		printf("\n\n\n***** 프로그램 목록 *****\n\n\n");
		printf("	1. score();\n");
		printf("	2. Good();\n");
		printf("	3. PointerTest();\n");
		printf("	4. solution1();\n");
		printf("	5. SwapTest();\n");
		printf("	6. sortTestNew();\n");
		printf("	7. VoidTest();\n");
		printf("	8. StreamTest();\n");
		printf("	9. sortTestEx();\n");
		printf("	10. StringParse();\n");
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
		case 10: StringParse(); break;
		default: return 0;
		}
	}
}
```

# C++
- 확장자
  - C: xx.c
  - C++: cpp
  - C#: cs
  - JAVA: java
  - JAVAScript: js
    - JAVA랑 JAVAScript 관계 없음
    - JAVA는 C계열

- C++버전의 Hello World 출력
  - 헤더파일의 선언: #include <iostream>
  - 출력의 기본구성: std::cout<<'출력대상 |'<<'출력대상 2'<<'출력대상 3';
  - '::': class 멤버, std클래스의 cout멤버
  - 개행의 진행: std::endl을 출력하면 개행이 이뤄진다.(LF)
  - '<<'
    - C에서는 SHL(bit shift left), *2
    - C++에서는 redirection, cout으로 내보내기

``` C++
#include <iostream>
int main(void)
{
    int = 20;
    std::cout<<"Hello World!"<<std::endl;
    std::cout<<"Hello "<<"World!"<<std::endl;
    std::cout<<num<<'  '<<'A';
    std::cout<<'  '<<3.14<<std::endl;
    return 0;
}
```
	> C에서는 출력 대상에 따라 서식을 달리 지정해야 했지만, C++에서는 그러한 과정이 불필요.
  - 불편하기 때문에 그냥 printf 사용

- scanf를 대신하는 데이터의 입력
  - 입력의 기본구성: std::cin>>'변수'
  - 변수의 선언위치: 함수의 중간 부분에서도 변수의 선언이 가능하다.
  - 서식문자 사용 X, 변수에 그냥 넣을 수 있음
    - 별도의 서식지정 필요 X
``` C++
#include <iostream>
int main(void)
{
    int val1;
    std::cout<<"첫 번째 숫자입력:  ";
    std::cin>>val1;

    int val2;
    std::cout<<"두 번째 숫자입력:  ";
    std::cin>>val2;
    int result = val1 + val2;
    std::cout<<"덧셈결과: "<<result<<std::endl;
    return 0;
}
```

- 변수 두 개 연달아 입력해 받을 수도 있음
  - std::cin>>val1>>val2;
- for문 안에서도 변수의 선언 가능

- 'iostream': Header file
- 배열도 C와 동일



### 함수 오버로딩

- 함수 오버로딩의 이해
  - C++은 함수 호출 시 '함수의 이름'과 '전달되는 인자의 정보'를 동시에 참조해서 호출할
함수를 결정. 따라서 이렇듯 매개변수의 선언이 다르다면 동일한 이름의 함수도 정의 가능.
이러한 형태의 함수 정의를 가리켜 '함수 오버로딩(Function Overloading)'이라고 함.

``` C++
int MyFunc(int num)
{
  num++;
  return num;
}
int MyFunc(int a, int b)
{
  return a+b;
}
int main(void)
{
  MyFunc(20); 	// 첫 번째 함수 호출
  MyFunc(30, 40);	// 두 번째 함수 호출
  return 0;
}
```

- 함수 오버로딩의 예
  - 매개변수의 자료형이 다른 경우: O
  - 매개변수의 수가 다른 경우: O
  - 반환형의 차이: X, 컴파일러는 반환형이 아니라 함수명 이하를 보게 됨
    - 반환형만 다르다면, 함수명부터 모든 것이 같다는 소리이기에 오버로딩 불가능.


### 매개변수의 디폴트 값

- 매개변수에 설정하는 '디폴트 값'의 의미
``` C++
int MyFuncOne(int num=7)
{
  return num+1;
}
```
  - 인자를 전달하지 않으면 7이 전달된 것으로 간주. 여기서의 디폴트 값은 7. 
  - MyFuncOne();의 호출과 MyFuncOne(7);의 호출은 결과가 같음

``` C++
int MyFuncTwo(int num1=5, int num2=7)
{
  return num1+num2;
}
```
  - 인자를 전달하지 않으면 각각 5와 7이 전달된 것으로 간주. 따라서 이 함수를 대상으로 
하는 다음 두 함수의 호출은 결과가 동일.
  - MyFuncTwo(); MyFuncTwo(5, 7);
    - 만약 인자를 하나만 쓴다면, 앞에부터 들어가게 됨.

- 부분적 디폴트 값 설정
```
int YourFunc(int num1, int num2=5, int num3=7) {...}
YourFunc(10);		//YourFunc(10, 5, 7);
YourFunc(10, 20);		//YourFunc(10, 20, 7);
```
  > 매개변수의 일부에만 디폴트 값을 지정하고, 채워지지 않은 매개변수에만 인자를 
전달하는 것이 가능.
```
int YourFunc(int num1, int num2, int num3=30) { ... }		O
int YourFunc(int num1, int num2=20, int num3=30) { ... }		O
int YourFunc(int num1=10, int num2=20, int num3=30) { ... }	O

int WrongFunc(int num1=10, int num2, int num3) { ... }		X
int WrongFunc(int num1=10, int num2=20, int num3) { ... }	X
```
  > 전달되는 인자가 왼쪽에서부터 채워지므로, 디폴트 값은 오른쪽에서부터 채워져야 한다.

  > 전달되는 인자가 왼쪽에서부터 채워지므로, 오른쪽이 빈 상태로 왼쪽의 매개변수에만 
일부 채워진 디폴트 값은 의미가 없음. 컴파일 에러.

### 인라인 함수

- 매크로 함수의 장점과 함수의 inline 선언
  - inline은 매크로와 유사
> 컴파일 전, 우리가 보는 소스
``` C++
#define SQUARE(x) ((x)*(x))
int main(void)
{
  std::cout<< SQUARE(5) <<std::endl;
  return 0;
}
```
> 컴파일 처리 후, 컴파일러가 보는 소스
``` C++
int main(void)
{
  std::cout<< ((5)*(5)) <<std::endl;
  return 0;
}
```
  - 컴파일 처리 후, 컴파일러가 보는 소스는 inline이나 define된 부분이 전체 소스에 들어가 
있는 것으로 보임
  - 장점: 성능향상
  - 단점: 복잡함

- 인라인 함수에는 없는 매크로 함수만의 장점
  - data type에 독립적
  - inline 선언된 함수를 이와 같이 호출하려면, 각 data type별로 함수가 오버로딩 되어야 함. 

### namespace에 대한 소개

- namespace의 기본 원리
  - 프로그램의 모듈화를 위해 도입
  - 모든 식별자(변수, 함수, 형식 등의 이름)가 고유하도록 보장하는 코드 영역을 정의한 것
  - 프로젝트 하 같은 namespace라면 한 덩어리처럼 사용 가능
  - namespace가 다르면, 동일한 이름의 함수 및 변수를 선언하는 것이 가능
  - 프로젝트의 진행에 있어서 발생할 수 있는 이름의 충돌을 막을 목적으로 존재하는 것이
namespace이다.

- namespace 기반의 함수 선언과 정의의 분리
  - :: -> class, namespace에 사용

- 동일한 namespace내에서의 함수호출
  - 선언된 namespace의 이름이 동이랗다면, 이 둘은 동일한 namespace으로 간주함.
  - 다른 파일로 떨어져 있어도, 컴파일러가 알아서 해줌
  - namespace을 명시하지 않고 함수를 호출하면, 함수의 호출문이 존재하는 함수와 동일한 
namespace 안에서 호출할 함수를 찾게 됨. 따라서 해당 namespace에 있는 다른 함수를
직접 호출할 수 있음.

- std::cout, std::cin, std::endl 
  - std::cout -> 이름공간 std에 선언된 cout
  - std::cin -> 이름공간 std에 선언된 cin
  - std::endl  -> 이름공간 std에 선언된 endl
  - iostream에 선언되어 있는 cout, cin, endl은 namespace std안에 선언되어 있음
  - C++표준에서 제공하는 다양한 요소들 모두 namespace std 안에 선언되어 있음

- using을 이용한 namespace의 명시
  - using std::cin; => cin은 std::cin을 의미한다는 선언
  - namespace std에 선언된 것은 std라는 namespace의 선언없이 접근하겠다는 선언
  - 너무 빈번한 using namespace의 선언은 이름의 충돌을 막기 위한 namespace의 선언을 
의미없게 만듦. 제한적으로 사용해야. 

- namespace의 별칭 지정과 전역변수의 접근
  - 전역변수: namespace내에서 함수의 내부에 있지 않은(외부에 있는) 변수


