### 1장
- scanf를 대신하는 데이터의 입력
  - std::cin
    - 서식문자 입력이 필요 없음
    - & 붙일 필요 없음(주소 표현 X)
	- reference(변수의 별명)이용해서 포인터 연산과 유사한 연산 가능
  - printf 대신하는 출력은 std::cout

+ 함수 오버로딩
  - 매개변수의 수, 자료형의 차이가 있다면, 동일한 함수명을 이용해 다수의 함수를 
사용할 수 있는 것
  - 반환형의 차이는 오버로딩 조건을 충족하지 않음

+ 디폴트 값
  - 인자 전달하지 않았을 때, 기본적으로 전달되는 값
  - 없어도 됨
  - 뒤에서부터 채워야 함

+ 함수의 inline

+ namespace
  - 큰 모듈(Project...)단위
  - 변수(전역)와 함수들이 포함되어 있음

### 2장
- bool 변수
  - C/C++에서는 int와 혼용 가능
  - 타 언어에서는 그렇지 않기 때문에, 웬만하면 True, False로 사용하기

- reference
  - 참조자
  - 변수의 별명
  - 같은 값을 가리키는 이름이 두 개가 되는 것
    - 주소 같음, 같은 변수

- Call by value & Call by reference
  - Call by value
  - Call by reference
    - 주소 값을 이용하는 것
    - reference를 이용하는것
    - 함수 호출 용이

- new & delete
  - malloc(메모리 동적할당)과 free(메모리 해제)에 해당
  - 크기를 바이트 단위로 계산하지 않아도 됨
  - 둘다 사용 가능
    - double* arr2 = new double[7];

- Header
  - c 더하고 .h빼기
  ```
  - <stdio.h> -> <cstdio>
  - <stdlib.h> -> <cstdlib>
  ```

### 3장 
- 구조체 struct
  - C의 구조체는 변수만 들어갈 수 있음
  - C++은 함수도 가능
  - struct 키워드 생략을 위한 typedef 선언이 불필요
    - strcut Car basicCar -> Car basicCar;
  - Class라고 함

+ 구조체 메모리
  - sizeof로 크기 구할 수 있음
  - 동적 할당은 Heap에
  - 나머지는 Stack에
  > 동일 구조체가 여러 개라도, 하나만 code영역에 있음

  > 호출될 때마다 변수, 함수를 가져다가 Stack에서 연산 후 돌려줌

    > 각 구조체는 같은 영역에 있음

  > 클래스도 마찬가지

  > 구조체 인스턴스의 함수들은 포인터로 코드에 접근
  
  > **실제로는 구조체 변수마다 함수가 독립적으로 존재하는 구조가 아님. 그러나 
논리적으로는 독립적으로 존재하는 형태로 보아도 문제가 없음.**

+ 접근제어 지시자
  - public: (클래스 내부)어디서든 접근 허용
    - 클래스 내부: 클래스 키워드와 중괄호 범위, 클래스에 선언된 함수 내부
  - protected: 상속관계에 놓여있을 때, 유도 클래스에서의 접근 허용
  - private: 클래스 내(클래스 내에 정의된 함수)에서만 접근 허용

+ 객체지향 프로그래밍
  - Class도 argument로 사용할 수 있음

## 4장 정보은닉
### 04-1 정보은닉(Information hiding)
* 좋은 클래스의 조건은 **정보은닉**과 **캡슐화**
* 제한된 방법으로의 접근만 허용해서 잘못된 값이 저장되지 않도록 도와야 하고, 실수가 발생했을 
경우에 이를 쉽게 발견할 수 있도록 만들어야 함
``` C++
class Point 
{
public:
	int x;	// 0 <= x <= 100을 제한할 수 없음
	int y;	// 0 <= y <= 100을 제한할 수 없음
};
```
  - x, y가 public으로 선언되어 외부 접근이 자유로움
``` C++
// Point.h
#ifndef __POINT_H_
#define __POINT_H_

class Point
{
private:
	int x;
	int y;
public:
	bool InitMembers(int xpos, int ypos);
	int GetX() const;
	int GetY() const;
	bool SetX(int xpos);
	bool SetY(int ypos);
};
#endif
```
  - x, y가 private로 선언되어, 임의로 값이 저장되는 것을 막음
    - x와 y라는 정보를 은닉함

``` C++
// Point.cpp
#include <iostream>
#include "Point.h"
using namespace std;

bool Point::InitMembers(int xpos, int ypos)
{
  if(xpos < 0 || ypos < 0)
  {
    cout<<"벗어난 범위의 값 전달"<<endl;
    return false;
  }

  x = xpos;
  y = ypos;
  return true;
}

int Point::GetX() const    // const 함수
{ return x; }
int Point::GetY() const
{ return y; }

bool Point::SetX(int xpos)
{
  if(0 > xpos) || xpos > 100)
  {
    cout<<"벗어난 범위의 값 전달"<<endl;
    return false;
  }
  x = xpos;
  return true;
}
bool Point::SetY(int ypos)
{
  if(0 > ypos) || ypos > 100)
  {
    cout<<"벗어난 범위의 값 전달"<<endl;
    return false;
  }
  y = ypos;
  return true;
}
```
  - 멤버변수에 값을 저장하는 함수 InitMembers, SetX, SetY는 0~100의 값이 전달되지 않으면, 
에러 메시지 출력 & 값의 저장 허용 X

  > 멤버변수를 private로 선언하고, 해당 변수에 접근하는 함수를 별도로 정의해서, 안전한 형태로 
멤버변수의 접근을 유도하는 것이 바로 **정보은닉**

  > 정보은닉은 좋은 클래스의 기본 조건

* Access Function
  - 클래스 외부에서, private로 선언된 멤버변수에 접근하기 위한 함수
  - 정의되었지만 사용되지 않는 경우도 있음
    - 지금은 필요하지 않으나, 앞으로 사용될 것을 예상하여 선언되는 경우 

- 멤버변수의 외부접근을 허용하면 잘못된 값이 저장되는 문제 발생 가능.
  > 정보은닉: 멤버변수의 외부접근을 막는 것
  - struct는 정보은닉X
  - Class에서 사용
> 클래스의 멤버변수를 private로 선언하고, 해당 변수에 접근하는 함수를 
별도로 정의해서, 안전한 형태로 멤버변수의 접근을 유도하는 것

+ const(변경 불가) 함수
  - int GetX() const;
  - const 함수 내에서는 동일 클래스에 선언된 **멤버변수**의 값 변경 불가
    - 자신의 의도와 다르게 멤버변수의 값을 변경했을 경우, 컴파일 에러
  - const 함수는 const가 아닌 함수 호출 불가
    - 간접적인 멤버의 변경 가능성 차단
  - const 참조자를 대상으로 값의 변경 능력을 가진 함수의 호출 허용 X(실제 값의 변경 여부 관련 X)
    > const 참조자를 이용하면 const 함수만 호출 가능
  - const로 상수화 된 객체를 대상으로는 const 멤버함수만 호출 가능

### 04-2 캡슐화 Encapsulation
+ 캡슐화: 관련있는 모든 것을 하나의 클래스 안에 묶어 두는 것
  - 캡슐화가 무너진다면 객체 활용이 어려워질 뿐만 아니라, 클래스 상호관계가 복잡해져, 프로그램 
전체의 복잡도를 높인다.
``` C++
// Encaps1 // 캡슐화 전
#include <iostream>
using namespace std;

class SinivelCap
{
public:
	void Take() const { cout << "콧물이 싹 낫습니다" << endl; }
};

class SneezeCap
{
public:
	void Take() const { cout << "재채기가 멎습니다" << endl; }
};

class SnuffleCap
{
public:
	void Take() const { cout << "코가 뻥 뚫립니다" << endl; }
};

class ColdPatient
{
public:
	void TakeSinivelCap(const SinivelCap& cap) const { cap.Take(); }
	void TakeSneezeCap(const SneezeCap& cap) const { cap.Take(); }
	void TakeSnuffleCap(const SnuffleCap& cap) const { cap.Take(); }
};

int main(void)
{
	SinivelCap scap;
	SneezeCap zcap;
	SnuffleCap ncap;

	ColdPatient sufferer;
	sufferer.TakeSinivelCap(scap);
	sufferer.TakeSneezeCap(zcap);
	sufferer.TakeSnuffleCap(ncap);
	return 0;
}
```

``` C++
// Encaps2  // 캡슐화
#include <iostream>
using namespace std;

class SinivelCap
{
public:
	void Take() const { cout << "콧물이 싹 낫습니다" << endl; }
};

class SneezeCap
{
public:
	void Take() const { cout << "재채기가 멎습니다" << endl; }
};

class SnuffleCap
{
public:
	void Take() const { cout << "코가 뻥 뚫립니다" << endl; }
};

class CONTAC600
{
private:
	SinivelCap sin;
	SneezeCap sne;
	SnuffleCap snu;

public:
	void Take() const
	{
		sin.Take();
		sne.Take();
		snu.Take();
	}
};
class ColdPatient
{
public:
	void TakeCONTAC600(const CONTAC600& cap) const { cap.Take(); }
};

int main(void)
{
	CONTAC600 cap;

	ColdPatient sufferer;
	sufferer.TakeCONTAC600(cap);
	return 0;
}
```
-  캡슐화는 정보은닉을 포함한다.


### 04-3 생성자와 소멸자(Constructor & Destructor)
+ 생성자 Constructor
  - 클래스 이름과 동일한 이름의 함수이면서 반환형이 선언되지 않았고, 
실제로 반환하지 않는 함수
``` C++
class SimpleClass
{
private:
  int num;
public:
  SimpleClass(int n) // 생성자
  {
    num = n;
  }
  int GetNum() const
  {
    return num;
  }
};
```
  - 자동생성
  - 객체 생성 시 한번만 호출 -> 멤버변수의 초기화에 사용
    - 객체 생성 과정에서 자동으로 호출되는 생성자에게 전달할 인자의 정보를 추가해야 함
    - SimpleClass sc(20);
    - SimpleClass * ptr = new SimpleClass(30);
  - 생성자도 함수이므로, 오버로딩과 디폴트 값 설정이 가능하다.
    - 함수이므로 오버로딩이 가능하다(여러 개)
    - 매개변수에 디폴트 값 설정 가능
  - 명시적으로 쓰지 않는다면, 컴파일러가 자동으로 코드에는 보이지 않는 
생성자 만듦
``` C++
#include <iostream>
using namespace std;

class SimpleClass
{
private:
	int num1;
	int num2;
public:
	SimpleClass()
	{
		num1 = 0;
		num2 = 0;
	}
	SimpleClass(int n)
	{
		num1 = n;
		num2 = 0;
	}
	SimpleClass(int n1, int n2)
	{
		num1 = n1;
		num2 = n2;
	}
	/*SimpleClass(int n1 = 0, int n2 = 0)  // 매개변수의 디폴트값 설정(타 생성자 주석처리 시)
	{
		num1 = n1;
		num2 = n2;
	}*/

	void ShowData() const
	{
		cout << num1 << ' ' << num2 << endl;
	}
};

int main(void)
{
	SimpleClass sc1;
	sc1.ShowData();

	SimpleClass sc2(100);
	sc2.ShowData();

	SimpleClass sc3(100, 200);
	sc3.ShowData();
	return 0;
}
```
- 타 생성자를 주석처리 하지 않고, 매개변수를 디폴트 값으로 설정하는 생성자를 주석 해제하면 
"어떤 생성자를 호출할지 애매모호함".
  - SimpleClass sc2(100); 의 경우에, 매개변수로 int n을 받는 생성자와 디폴트 값을 받는 생성자, 
두 가지가 호출 가능하게 됨.
- 매개변수가 있는 경우, 객체명 옆에 괄호를 붙여야
- 매개변수가 없는 경우, 객체명 옆에 빈 괄호도 붙이면 안 됨
  - 함수 원형 선언과 같음
  - SimpleClass sc2();

+ 멤버 이니셜라이저(Member Initailizer) 기반의 멤버 초기화
  - 멤버 이니셜라이저는 함수의 선언부가 아닌, 정의부에 명시
  - ex. Rectangle class의 생성자 정의
``` C++
Rectangle::Rectangle(const int &x1, const int &y1, const int &x2, const int &y2) : upLeft(x1, y1), 
lowRight(x2, y2)
{
	//empty
}
```
  - **: upLeft(x1, y1), lowRight(x2, y2)** 가 멤버 이니셜라이저
    - 객체 upLeft의 생성과정에서 x1과 y1을 인자로 전달받는 생성자를 호출하라
    - 객체 lowRight의 생성과정에서 x2과 y2을 인자로 전달받는 생성자를 호출하라
> 멤버 이니셜라이저는 멤버변수로 선언된 객체의 생성자 호출에 활용

``` C++
//Point.h
#ifndef __POINT_H_
#define __POINT_H_

class Point 
{
private:
  int x;
  int y;
public:
  Point(const int &xpos, const int &ypos);
  int GetX() const;
  int GetY() const;
  bool SetX(int xpos);
  bool SetY(int ypos);
};
#endif
```
```C++
// Point.cpp
#include <iostream>
#include "Point.h"
using namespace std;

Point::Point(const int &xpos, const int &ypos)
{
  x = xpos;
  y = ypos;
}

int Point::GetX() const { return X; }
int Point::GetY() const { return Y; }

bool Point::SetX(int xpos)
{
  if(0 > xpos || xpos > 100)
  {
    cout<<"벗어난 범위의 값 전달"<<endl;
    return false;
  }
  x = xpos;
  return true;
}
bool Point::SetY(int ypos)
{
  if(0 > ypos || ypos > 100)
  {
    cout<<"벗어난 범위의 값 전달"<<endl;
    return false;
  }
  y = ypos;
  return true;
}
```
``` C++
//Rectangle.h
#ifndef __RECTANGLE_H_
#define __RECTANGLE_H_

#include "Point.h"

class Rectangle
{
private:
  Point upLeft;
  Point lowRight;
public:
  Rectangle(const int &x1, const int &y1, const int &x2, const int &y2);
  void ShowRecInfo() const;
};
#endif
```
``` C++
// Rectangle.cpp
#include <iostream>
#include "Rectangle.h"
using namespace std;

Rectangle::Rectangle(const int &x1, const int &y1, const int &x2, const int &y2) : upLeft(x1, y1), 
lowRight(x2, y2)
{
  //empty
}

void Rectangle::ShowRecInfo() const
{
  cout<<"좌 상단: "<<'['<<upLeft.GetX()<<", ";
  cout<<upLeft.GetX()<<']'<<endl;
  cout<<"우 하단: "<<'['<<lowRight.GetY()<<", ";
  cout<<lowRight.GetY()<<']'<<endl<<endl;
}
```
``` C++
//RectangleConstructor.cpp
#include <iostream>
#include "Point.h"
#include "Rectangle.h"
using namespace std;

int main(void)
{
  Rectangle rec(1, 1, 5, 5);
  rec.ShowRecInfo();
  return 0;
}
```
* 객체의 생성과정
  1. 메모리 공간의 할당
  2. 이니셜라이저를 이용한 멤버변수(객체)의 초기화
  3. 생성자의 몸체부분 실행
    - 이니셜라이저가 선언되지 않았다면, 1과 3단계로 객체생성
    - 생성자는 명시적으로 선언하지 않는다면, Default Constructor가 자동으로 삽입되어 호출됨

+ 멤버 이니셜라이저를 이용한 변수 및 const 상수(변수) 초기화
  - 멤버 이니셜라이저는 객체가 아닌 멤버의 초기화에도 사용 가능
``` C++
class SoSimple
{
private:
  int num1;
  int num2;
public:
  SoSimple(int n1, int n2) : num1(n1)
  {
    num2 = n2;
  }
};
```
- **num(n1)**은 num1을 n1의 값으로 초기화 하라는 뜻
- 멤버 변수의 초기화는 이니셜라이저가 선호됨
  - 초기화의 대상 명확히 인식 가능
  - 성능에 약간의 이점
- num(n1); == int num1 = n1;
  - 이니셜라이저를 통해 초기화되는 멤버는 선언과 동시에 초기화가 이뤄지는 것과 같은 유형의 
바이너리 코드를 구성하기 때문
  - 반면, 생성자의 몸체에서 보인 num2 = n2;는
    - int num2;
    - num2 = n2;
    - 위의 두 문장에 비유된다.
  > 이니셜라이저를 이용하면 선언과 동시에 초기화가 이뤄지는 형태로 바이너리 코드 생성
    - 생성자의 몸체에서 대입연산을 통한 초기화를 할 경우, 선언과 초기화를 각각 별도의 문장에서 
진행하는 형태로 바이너리 코드 생성

    > const 멤버변수도 이니셜라이저를 이용하면 초기화가 가능하다  
      > const 변수는 선언과 동시에 초기화 필수

+ 이니셜라이저는 멤버변수로 참조자를 선언할 수 있게 한다.
  - 참조자도 선언과 동시에 초기화해야!
    - 이니셜라이저를 이용하면 참조자도 멤버변수로 선언될 수 있음
``` C++
//ReferenceMember.cpp
#include <iostream>
using namespace std;

class AAA
{
public:
  AAA()
  {
    cout<<"empty object"<<endl;
  }
  void ShowYourName()
  {
    cout<<"I'm class AAA"<<endl;
  }
};

class BBB
{
private:
  AAA &ref;
  const int &num;
public:
  BBB(AAA &r, const int &n)
      : ref(r), num(n)			// 이부분
  { // empty constructor body
  }
  void ShowYourName()
  { 
    ref.ShowYourName();
    cout<<"and"<<endl;
    cout<<"I ref num "<<num<<endl;
  }
};

int main(void)
{
  AAA obj1;
  BBB obj2(obj1, 20);
  obj2.ShowYourName();
  return 0;
}
```

+ Default Constructor
- 객체가 되기 위해서는 반드시 하나의 생성자가 호출되어야 함
- 생성자를 정의하지 않은 클래스는 컴파일러에 의해 디폴트 생성자 자동 삽입
- 인자 X, 내부적으로 일 X
- new 연산자를 이용한 객체의 생성에도 해당
  > malloc함수를 이용하면 생성자 호출 
  - malloc함수 호출 시, 실제로는 클래스의 크기정보만 바이트 단위로 전달
  - 객체를 동적으로 할당 시 반드시 new 연산자 이용
- 생성자가 하나라도 존재한다면, 디폴트 생성자 삽입 X

+ private 생성자
- 위의 생성자는 전부 public
  - 객체 생성이 클래스의 외부에서 진행되기 때문에 생성자는 public으로 선언되어야!
  - 클래스 내부에서 객체를 생성한다면, 생성자가 private으로 선언되어도 됨
- 객체의 생성방법을 제한하려고 할 때 유용

**힙에 할당된 메모리 공간은 변수로 간주하여, 참조자를 통한 참조가 가능하다**

+ 소멸자의 이해와 활용
- 객체 소멸 시 반드시 호출
- 클래스 이름 앞에 ~ 붙임
- 반환형 선언 X, 실제로 반환 X
- 매개변수는 void형으로 선언되어야 하기 때문에, 오버로딩과 디폴트 값 설정 둘다 불가능
- 디폴트 소멸자 존재, 자동 삽입
- 생성자에서 할당한 리소스의 소멸에 사용(delete 연산자 이용해 메모리 공간 소멸)
``` C++
//Destructor.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person
{
private:
  char * name;
  int age;
public:
  Person(char * myname, int myage)
  {
    int len = strlen(myname) + 1; 	// 문자열 길이만큼 메모리 공간 동적 할당
    name = new char[len];
    strcpy(name, myname);
    age = myage;
  }
  void ShowPersonInfo() const
  {
    cout<<"이름: "<<name<<endl;
    cout<<"나이: "<<age<<endl;
  }
  ~Person()			// 소멸자
  {
    delete []name;
    cout<<"called destructor!"<<endl;
  }
};

int main(void)
{
  Person man1("Lee dong woo", 29);
  Person man2("Jang dong hun", 41);
  man1.ShowPersonInfo();
  man2.ShowPersonInfo();
  return 0;
}
```

+ 객체 배열과 객체 포인터 배열
- 클래스명 배열명[배열길이];
- 배열명 * ptr = new 클래스명[배열길이];
  - 객체가 모여 배열 구성
  - 구조체 배열 선언과 차이 없음
  - 배열 선언의 경우에도 생성자는 호출됨
  - 단, 배열의 선언과정에서는 호출할 생성자를 별도로 명시할 수 없음(생성자에 인자 전달 불가)
    - 위의 형태로 배열 생성하려면, 해당 형태의 생성자 존재해야
  - 배열 선언 이후, 각 요소를 원하는 값으로 초기화하려면, 일일이 초기화 과정을 별도로 거쳐야
  - 객체 배열 생성 시 void형 생성자 호출

+ 객체 포인터 배열
- 객체의 주소 값 저장이 가능한 포인터 변수로 이뤄진 배열

+ this 포인터
- 객체 자신을 가리키는 용도로 사용되는 포인터
  - 객체 자신의 주소 값을 의미
- 주소 값과 자료형이 정해져 있지 않은 포인터
- 객체의 포인터로는 지역변수에 접근이 불가능
  > 멤버변수 의미

+ Self-Reference의 반환
- 객체 자신을 참조할 수 있는 참조자
``` C++
#include <iostream>
using namespace std;

class SelfRef
{
private:
	int num;
public:
	SelfRef(int n) : num(n)
	{
		cout << "객체생성" << endl;
	}
	SelfRef& Adder(int n)	// 객체 자신 반환, 반환형 고려하면 참조 값 반환
	{
		num += n;
		return *this;
	}
	SelfRef& ShowTwoNumber()
	{
		cout << num << endl;
		return *this;
	}
};

int main(void)
{
	SelfRef obj(3);					// 객체생성 프린트, num에 3 저장
	SelfRef& ref = obj.Adder(2);	// obj의 참조자 ref, num이 5로 증가

	obj.ShowTwoNumber();			// 5 출력
	ref.ShowTwoNumber();			// 5 출력

	ref.Adder(1).ShowTwoNumber().Adder(2).ShowTwoNumber();	// 참조자로 num 1증가(6), 6 출력, 2증가(8), 8 출력
	return 0;
}
```

+ 참조의 정보(참조 값)에 대한 이해
- int num = 8;
- int &ref = num;
  - 변수 num을 참조할 수 있는 참조의 정보(참조 값)이 전달
    - 변수 num을 참조할 수 있는 참조 값이 참조자 ref에 전달되어, ref가 변수 num을 참조하게 됨



## 6장 friend와 static 그리고 const
### 06-1 const
+ const 객체와 const 객체의 특징들
- 변수의 상수화
  - const int num - 10;
- 객체의 상수화 
  - const SoSimple sim(20);
  - 객체에 const 선언이 붙으면, 이 객체를 대상으로는 const 멤버함수만 호출 가능
  - 객체의 const 선언
    > 이 객체의 데이터 변경을 허용하지 않겠다.
    - 변경시킬 능력 있는 함수는 아예 호출을 X
``` C++
// ConstObject.cpp
#include <iosteam>
using namespace std;

class SoSimple
{
private:
  int num;
public:
  SoSimple(int n) : num(n)
  { }
  SoSimple& AddNum(int n)
  {
    num += n;
    return *this;
  }
  void ShowData() const
  {
    cout<<"num: "<<num<<endl;
  }
};

int main(void)
{
  const SoSimple obj(7);		// const 객체 생성
  // obj.AddNum(20);		// AddNum은 const 함수가 아니라서 호출 불가
  obj.ShowData();			// ShowData는 const 함수라서 호출 가능
  return 0;
}
```
- 멤버변수에 저장된 값을 수정하지 않는 함수는 가급적 const로 선언
  - const 객체ㅔㅇ서도 호출이 가능하도록 할 필요 있음

+ const와 함수 오버로딩
- 함수 오버로딩 성립 위해서, 매개변수의 수나 자료형이 달라야 함!
- +const의 선언 유무
  - void SimpleFunc() {  }
  - void SimpleFunc() const {  }

``` C++
//ConstOverloading.cpp
#include <iostream>
using namespace std;

class SoSimple 
{
private:
  int num;
public:
  SoSimple(int n) : num(n)
  { }
  SoSimple& AddNum(int n)
  {
    num += n;
    return *this;
  }
  void SimpleFunc()
  {
    cout<<"SimpleFunc: "<<num<<endl;
  }
  void SimpleFunc() const
  {
    cout<<"conust SimpleFunc: "<<num<<endl;
  }
};

void YourFunc(const SoSimple &obj)	// 전달되는 인자를 const 참조자로 받음
{
  obj.SimpleFunc();
}

int main(void)
{
  SoSimple obj1(2);		// 객체 생성
  const SoSimple obj2(7);	// const 객체 생성
  
  obj1.SimpleFunc();	// 일반 멤버함수 호출
  obj2.SimpleFunc();	// const 멤버함수 호출

  YourFunc(obj1);		// 함수 호출 결과로, SimpleFunc() const 호출
  YourFunc(obj2);
  return 0;
}
```

### 06-2 클래스와 함수에 대한 friend 선언

+클래스의 friend 선언
- A class가 B class를 대상으로 friend 선언 시, B class는 A class의 private 멤버에 
직접 접근 가능
- 단, A class도 B class의 private 멤버에 직접 접근 하려면, B class가 A class에 대상으로 
friend 선언을 해줘야 가능
``` C++
// MyFirendClass.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Girl;		// 정의가 뒤에 나오는 함수의 호출을 위해 함수의 원형을 선언하듯, 
		// 클래스도 선언 가능.
class Boy
{
private:
  int height;
  friend class Girl;		// private영역에도 friend 선언 가능
public:
  Boy(int len) : height(len);
  { }
  void ShowYourFriendInfo(Girl &frn);	// 정의되지 않은 클래스 등장, 위에서 선언해서 가능
};

class Girl
{
private:
  char phNum[20];
public:
  Girl(char * num)
  {
    strcpy(phNum, num);
  }
  void ShowYourFriendInfo(Boy &frn);
  friend class Boy;
};

void Boy::ShowYourFriendInfo(Girl &frn)	// phNum을 컴파일러가 알아야 하기 때문에 
{				// 함수의 정의가 Girl 클래스 정의보다 뒤에 위치
    cout<<"Her phone number: "<<frn.phNum<<endl;
}

void Girl::ShowYourFriendInfo(Girl &frn)
{
    cout<<"His height: "<<frn.height<<endl;
}

int main(void)
{
  Boy boy(170);
  Girl girl("010-1234-5678");
  boy.ShowYourFriendInfo(girl);
  girl.ShowYourFriendInfo(boy);
  return 0;
}
```
- 맨 위의 class Girl;이 없어도, class Boy의 private에서 friend class Girl;을 했기 때문에 
컴파일에 문제는 없다
- friend class Girl; 의 역할
  - Girl은 클래스명
  - 그리고 해당 클래스를 friend로 선언

+ friend선언은 언제?
- friend는 정보은닉 붕괴시킴
  > 소극적으로 사용해야

+함수의 friend 선언
- 전역함수, 멤버함수 모두 friend 선언 가능
  - friend로 선언된 함수는 자신이 선언된 클래스의 private 영역에 접근 가능

``` C++
// MyFriendFunction.cpp
#include <iostream>
using namespace std;

class Point;	// class PointOP의 public 영역에서 아용하기 위해 선언

class PointOP
{
private:
  int opcnt;
public:
  PointOP() : opcnt(0);
  { }
  
  Point PointAdd(const Point&, const Point&);
  Point PointSub(const Point&, const Point&);
  ~PointOP()
  {
    cout<<"Operation times: "<<opcnt<<endl;
  }
};

class Point
{
private:
  int x;
  int y;
public:
  Point(const int &xpos, const int &ypos) : x(xpos), y(ypos)
  { }
  friend Point PointOP::PointAdd(const Point&, const Point&); // PointOP class의 멤버함수에
  friend Point PointOP::PointSub(const Point&, const Point&); // friend 선언
  friend void ShowPointPos(const Point&); // ShowPointPos 함수에 friend 선언이자
};				// 함수prototype 선언

Point PointOP::PointAdd(const Point& pnt1, const Point& pnt2)
{
  opcnt++;
  return Point(pnt1.x + pnt2.x, pnt1.y + pnt2.y); // friend선언되어 private멤버에 접근 가능
}

Point PointOP::PointSub(const Point& pnt1, const Point& pnt2)
{
  opcnt++;
  return Point(pnt1.x - pnt2.x, pnt1.y - pnt2.y); // friend선언되어 private멤버에 접근 가능
}

int main(void)
{
  Point pos1(1, 2);
  Point pos2(2, 4);
  Point op;

  ShowPointPos(op.PointAdd(pos1, pos2));
  ShowPointPos(op.PointSub(pos2, pos1));
  return 0;
}

void ShowPointPos(const Point& pos)
{
  cout<<"x: "<<pos.x<<", ";
  cout<<"y: "<<pos.y<<endl;
}
```

### 06-3 C++에서의 static
+ C에서의 static -> 정적 변수
  - 주로 전역 변수에서 사용
    - 선언된 파일 내에서만 참조를 허용하겠다
  - 함수 내에 선언된 static
    - 한번만 초기화되고(호출될 때마다 새로이 할당 X), 함수를 빠져나가도 소멸되지 않음
    - static 변수는 전역변수와 같이, 초기화 X 시 0으로 초기화

+ static 멤버변수(Class 변수)
  - static 변수는 객체 별로 존재하는 변수가 아닌, 프로그램 전체 영역에서 하나만 
존재하는 변수.(Class당 하나씩만 생성)
  - 프로그램 실행과 동시에 초기화되어 메모리 공간에 할당.
  - 각 객체의 멤버함수(생성자)에서는 멤버변수에 접근하듯, stiatic 멤버변수에 접근 가능
    - 객체 내에 존재하는 것 아님
    > 변수 외부에 존재, 접근 권한만 있을 뿐
  - 생성, 소멸의 시점도 전역변수와 동일
  - **static 변수를 생성자에서 초기화하면 안 되는 이유**
    > 객체가 생성될 때마다 초기화
    - static 멤버변수의 초기화 문법은 별도로 정의되어 있음
    - int SoSimple::simObjCnt = 0;
      - SoSimple class의 static 멤버변수 simObjCnt가 메모리 공간에 저장될 때, 0으로 
초기화하라

+ static 멤버변수의 또 다른 접근방법
- static 멤버변수는 어디서든 접근 가능
  - static 멤버가 private로 선언되면, 해당 클래스의 객체들만 접근 가능
``` C++
// PulbicStaticMember.cpp
#include <iostream>
using namespace std;

class SoSimple
{
public:
  static int simObjCnt;		// static멤버변수가 public으로 선언됨
public:	// 변수와 함수 구분 목적, 선택
  SoSimple()
  {
    simObjCnt++;
  }
};
int SoSimple::simObjCnt = 0;		// static 멤버변수는 항상 이렇게 초기화

int main(void)
{
  cout<<SoSimple::simObjCnt<<"번째 SoSimple 객체"<<endl; // 객체 생성X인데 클래스의 이름을 
  SoSimple sim1; // 이용해서 simObjCnt에 접근(static변수가 객체 내에 X임을 증명, 
  SoSimple sim2; // 이런 식으로 어디서든 접근 가능)

  cout<<SoSimple::simObjCnt<<"번째 SoSimple 객체"<<endl;
  cout<<sim1.simObjCnt<<"번째 SoSimple 객체"<<endl; // 객체 이용해서도 static 멤버변수 접근 가능
  cout<<sim2.simObjCnt<<"번째 SoSimple 객체"<<endl; // 멤버변수 접근과 헷갈림
  return 0;
}
```
- public static 멤버에 접근할 때는
  - cout<<SoSimple::simObjCnt; 처럼 접근하는 것이 나음(클래스 이름 이용)
  - cout<<sim1.simObjCnt;는 일반 멤버와 헷갈림

+ static 멤버함수
- 선언된 클래스의 모든 객체가 공유
- public으로 선언되면, 클래스의 이름 이용해서 호출이 가능
- **객체의 멤버로 존재하는 것이 아님**
  - static 멤버변수와 특성 동일

``` c++
class SoSimple
{ 
private:
  int num1;
  static int num2;
public:
  SoSimple(int n): num1(n)
  { }
  static void Adder(int n)
  { 
    num1 += n;
    num2 += n;
  }
};
int SoSimple::num2 = 0;
```
- 위의 예시에서, static 멤버함수인 Adder에서 멤버변수인 num1에 접근하는 것은 잘못
  - 객체의 멤버가 아닌데 어떻게 멤버변수에 접근?
  - 객체 생성 이전에도 호출 가능. 그런데 어떻게 멤버변수에 접근 가능?
  - 멤버변수에 접근한다 쳐도, 어떤 객체의 멤버변수에 접근 해야 함?
> static 멤버함수 내에서는, static으로 선언되지 않은 멤버변수의 접근도, 
멤버함수의 호출도 불가능!!  
  > static 멤버함수 내에서는 static 멤버변수와 static 멤버함수만 호출 가능!!

+ const static 멤버
- 클래스 내에 선언된 const 멤버변수(상수)의 초기화는 이니셜라이저를 통해야!!
  - 그러나, const static으로 선언되는 멤버변수(상수)는 다음과 같이 선언과 동시에 
초기화가 가능
``` C++
// ConstStaticMember.cpp
#include <iostream>
using namespace std;

class CountryArea // 국가별 면적의 크기 저장한 클래스, const static 상수는 보통 하나의 
{		// 클래스에 둘 이상 모임
public:
  const static int RUSSIA		= 1707540;
  const static int CANADA		= 998467;
  const static int CHINA		= 957290;
  const static int SOUTH_KOREA	= 9922;
};
int main(void)
{
  cout<<"러시아 면적: "<<CountryArea::RUSSIA<<"km^2"<<endl; // 상수 접근 위해
  cout<<"캐나다 면적: "<<CountryArea::CANADA<<"km^2"<<endl; // 객체 생성 X
  cout<<"중국 면적: "<<CountryArea::CHINA<<"km^2"<<endl;
  cout<<"한국 면적: "<<CountryArea::SOUTH_KOREA<<"km^2"<<endl;
  return 0;
}
```
- const static 멤버변수는 클래스가 정의될 때 지정된 값이 유지되는 상수이기 때문에, 
위와 같이 초기화가 가능하도록 문법으로 정의됨.

+ mutable
- 사용 빈도 낮춰야
- const 함수 내에서의 값 변경을 예외적으로 허용

+ 컴파일러가 생성하는 템플릿 기반의 함수
 - = 컴파일러가 자동으로 생성하는 오버로드 <- 최적화 X
















