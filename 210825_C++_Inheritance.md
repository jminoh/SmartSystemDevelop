## 7장 상속 Inheritance
- Parent Class에서 파생된 Child Class(Derived, 유도된)
- 상위 클래스(간단, 단순) / 하위 클래스(구체적)

### 07-1 상속에 들어가기에 앞서
+ 상속에 대한 새로운 관점의 이해
- 과거: 기존에 정의해 놓은 클래스의 재활용을 목적으로 하는 것이 상속

+ 시나리오
- 급여관리 시스템
- 최초 도입 시, 직원의 근무형태는 정규직(Permanent) 하나 -> 정규직 관리를 위한 형태
  - class Permanent
  - 정규직 급여는 입사 시 정해짐.(인상 고려 X)
  - 이름, 급여정보 저장을 위한 class
``` C++
// EmployeeManager1.cpp의 class PermanentWorker 정의
class PermanentWorker
{
private:
	char name[100];
	int salary; // 매달 지불되는 급여
public:
	PermanentWorker(const char* name, int money) : salary(money)	// const 붙여줌
	{
		strcpy(this->name, name);
	}
	int GetPay() const
	{
		return salary;
	}
	void ShowSalaryInfo() const
	{
		cout << "name: " << name << endl;
		cout << "slalry: " << GetPay() << endl << endl;
	}
};
```
- Permanent class 객체를 저장 및 관리하기 위한 클래스
  - class EmployeeHandler
  - Permanent class 객체 저장을 목적으로 배열을 멤버로 지님
  - 저장된 객체의 급여 정보를 출력하기 위한 함수를 멤버로 지님
``` C++
// EmployeeManager1.cpp의 class EmployeeHandler 정의
class EmployeeHandler
{
private:
	PermanentWorker* empList[80];
	int empNum;
public:
	EmployeeHandler() : empNum(0)
	{ }
	void AddEmployee(PermanentWorker* emp)
	{
		empList[empNum++] = emp;
	}
	void ShowAllSalaryInfo() const
	{
		for (int i = 0; i < empNum; i++)
			empList[i]->ShowSalaryInfo();
	}
	void ShowTotalSalary() const
	{
		int sum = 0;
		for (int i = 0; i < empNum; i++)
			sum += empList[i]->GetPay();
		cout << "salary sum: " << sum << endl;
	}
	~EmployeeHandler()
	{
		for (int i = 0; i < empNum; i++)
			delete empList[i];
	}
};
```
- 전자는 데이터적 성격이 강하고, 후자는 기능적 성격이 강함
  - PermanentWorker 객체는 기록의 보전을 위해 파일에 저장할 데이터를 갖고 있음
  - EmployeeHandler 객체는 프로그램을 구성하는 대표 기능들 처리
    - 새 직원정보 등록: AddEmployee
    - 모든 직원들의 이번 달 급여정보 출력: ShowAllSalaryInfo
    - 이번 달 급여의 총액 출력: ShowTotalSalary
> Control Class / Handler Class: 기능의 처리를 실제로 담당하는 클래스

``` C++
//EmployeeManager1.cpp의 main
int main(void)
{
	EmployeeHandler handler;

	//직원등록
	handler.AddEmployee(new PermanentWorker("KIM", 1000));
	handler.AddEmployee(new PermanentWorker("LEE", 1500));
	handler.AddEmployee(new PermanentWorker("JUN", 2000));

	//이번 달에 지불해야 할 급여 정보
	handler.ShowAllSalaryInfo();

	//이번 달에 지불해야 할 급여의 총합
	handler.ShowTotalSalary();
	return 0;
}
```

+ 문제 제시
- 새로운 고용형태 등장
  - 영업직(Sales): 인센티브 개념 도입
  - 임시직(Temporary): 알바
- 급여 계산방식
  - 고용직: 연봉제, 매달 급여 고정
  - 영업직: 기본급여 + 인센티브
  - 임시직: 시간 당 급여 * 일한 시간
- 영업직, 임시직 class 추가 + EmployeeHandler class 변경
  - SalesMan객체와 Temporary객체의 저장을 위한 배열 두 개 추가
  - 각각의 배열에 저장된 객체의 수 별도로 Cnt위한 int 변수 멤버로 추가
  - AddEmployee()는 SalesMan과 Temporary 객체용으로 각각 추가되어야 함
  - 급여정보를 출력하는 나머지 두 멤버함수는 3개의 배열을 연산해야 하니까, 
반복문 각 두 개씩 추가

### 07-2 상속의 문법적인 이해
+ 상속이란?
- UnivStudent class가 Person class를 상속한다.
  - UnivStudent class는 Person class의 모든 멤버를 물려받음
    - UnivStudent 객체에는 UnivStudent class에 선언된 멤버뿐만 아니라, 
Person class에 선언된 멤버도 존재

+ 상속의 방법과 그 결과
``` C++
class Person
{
private:
	int age;
	char name[50];
public:
	Person(int myage, const char* myname) : age(myage)
	{
		strcpy(name, myname);
	}
	void WhatYourName() const
	{
		cout << "My name is " << name << endl;
	}
	void HowOldAreYou() const
	{
		cout << "I'm " << age << " years old" << endl;
	}
};

class UnivStudent : public Person // Person class 상속을 의미
{
private:
	char major[50];
public:
	UnivStudent(const char* myname, int myage, const char* mymajor) : Person(myage, myname)
	{
		strcpy(major, mymajor);
	}
	void WhoAreYou() const
	{
		WhatYourName();
		HowOldAreYou();
		cout << "My major is " << major << endl << endl;
	}
};
```
- UnivStudent class에 WhatYourName 함수와 HowOldAreYou함수가 정의되어 있지 
않음에도 불구하고, 두 함수를 호출할 수 있는 이유는, UnivStudent class가 Person class를 
상속했기 때문.
  - 앞의 두 함수가 Person class의 멤버함수이기 때문에 호출 가능
- 상속(Univ가 Person을 상속)하게 되면, 상속의 ㄷ상이 되는 클래스의 멤버까지도 객체 
내에 포함됨. 호출 가능

+ 상속받은 클래스의 생성자 정의
```
public:
	UnivStudent(char* myname, int myage, char* mymajor) : Person(myage, myname)
	{
		strcpy(major, mymajor);
	}
```
- 위는 UnivStudent class의 생성자임
- Person class를 상속하며, 해당 클래스의 멤버를 모두 받음
- UnivStudent class의 생성자는 Person class의 멤버까지 초기화할 의무 있나??
  > UnivStudent 객체가 생성되면, Person의 멤버변수도 존재하기 때문에, Person class의 
멤버까지 초기화해야 할 의무가 있음.  
  > UnivStudent class의 생성자는, Person class의 생성자를 호출해서 Person calss의 멤버를 
초기화하는 것이 좋다.  
  > 위 코드에서 UnivStudent 생성자 뒤에 붙은 이니셜라이저는 생성자의 호출임  
    > Person class의 생성자를 호출하며, 인자로 myage, myname에 저장된 값 전달  
  > 상속받는 클래스(Univ)는 이니셜라이저를 이용해 상속하는 클래스(Person)의 
생성자 호출을 명시할 수 있다.  

``` C++
int main(void)
{
	UnivStudent ustd1("Lee", 22, "Computer eng.");
	ustd1.WhoAreYou();

	UnivStudent ustd2("Yoon", 21, "Electeng.");
	ustd2.WhoAreYou();
	return 0;
}
```
- 위 class들의 main()
- Q. UnivStudent class의 멤버함수, 생성자에서 Person class에 private으로 선언된 
멤버변수 age, name에 접근 가능?
  - A. **접근제한의 기준은 Class**
  - 클래스 외부에서는 private멤버에 접근 불가(객체에서 접근 불가)
    - 따라서, UnivStudent 멤버함수 내에서는 Person의 멤버변수에 직접 접근 불가
    - cf. private의 접근제한이 객체 기준으로 결정된 것이라면, 접근 가능
      - UnivStudent 객체에는 UnivStudent 멤버함수와 Person의 멤버변수 함께 존재

- Q. private로 선언된 멤버, 상속 안 되는 건 아닌가?
  - A. 상속됨. <- 초기화됨. 해당 정보 출력
  - 다만, 직접 접근이 불가능하기 때문에, Person class에 정의된 public 함수를 통해 
간접적으로 접근해야 함.
    > 정보은닉은 객체 내에서도 진행

+ 용어 정리

|Person|<->|UnivStudent|
|--------|----|---------------|
|상위 클래스|<->|하위 클래스|
|기초(base) 클래스|<->|유도(derived) 클래스|
|슈퍼(super) 클래스|<->|서브(sub) 클래스|
|부모 클래스|<->|자식 클래스|

+ 유도 클래스의 객체 생성과정
- **기초 클래스의 생성자 호출 중요**
``` C++
// DerivCreOrder.cpp
#include <iostream>
using namespace std;

class SoBase
{
private:
	int baseNum;
public:
	SoBase() : baseNum(20)
	{
		cout << "SoBase()" << endl;
	}
	SoBase(int n) : baseNum(n)
	{
		cout << "SoBase(int n)" << endl;
	}
	void ShowBaseData()
	{
		cout << baseNum << endl;
	}
};

class SoDerived : public SoBase
{
private:
	int derivNum;
public:
	SoDerived() : derivNum(30)
	{
		cout << "SoDerived()" << endl;
	}
	SoDerived(int n) : derivNum(n)
	{
		cout << "SoDerived(int n)" << endl;
	}
	SoDerived(int n1, int n2) : SoBase(n1), derivNum(n2)
	{
		cout << "soDerived(int n1, int n2)" << endl;
	}
	void ShowDerivData()
	{
		ShowBaseData();
		cout << derivNum << endl;
	}
};
int main(void)
{
	cout << "case1..... " << endl;
	SoDerived dr1;
	dr1.ShowDerivData();
	cout << "---------------------------" << endl;
	cout << "case2..... " << endl;
	SoDerived dr2(12);
	dr2.ShowDerivData();
	cout << "---------------------------" << endl;
	cout << "case3..... " << endl;
	SoDerived dr3(23, 24);
	dr3.ShowDerivData();
	cout << "---------------------------" << endl;
}
```
- 유도 클래스의 객체생성 과정에서 기초 클래스의 생성자는 100% 호출
  - 유도 클래스의 객체생성 과정에서 생성자 두 번 호출됨
    - 기초 클래스의 생성자와 유도 클래스의 생성자
- 유도 클래스의 생성자에서 기초 클래스의 생성자 호출을 명시하지 않으면, 
기초 클래스의 void 생성자가 호출
  - main에서 SoDerived(int n) : derivNum(n)으로, 기초 클래스의 생성자 호출 X
    - SoBase() : baseNum(20) 호출해서 20으로 이니셜라이즈
  - SoDerived(int n1, int n2) : SoBase(n1), derivNum(n2)로 기초 클래스의 생성자 호출
    - SoBase(int n) : baseNum(n)으로, n1으로 전달된 23 반환

> 유도 클래스의 객체생성 과정에서는 생성자 두 번 호출  

  > 하나는 기초 클래스의 생성자, 하나는 유도 클래스의 생성자  
  > 생성자는 메모리 공간 할당 후 호출  

> 클래스의 멤버는 해당 클래스의 생성자를 통해서 초기화해야 한다.

+ 유도 클래스 객체의 소멸과정
``` C++
// DerivDestOrder.CPP
#include <iostream>
using namespace std;

class SoBase
{
private:
	int baseNum;
public:
	SoBase(int n) : baseNum(n)
	{
		cout << "SoBase() : " << baseNum << endl;
	}
	~SoBase()
	{
		cout << "~SoBase() : " << baseNum << endl;
	}
};

class SoDerived : public SoBase
{
private:
	int derivNum;
public:
	SoDerived(int n) : SoBase(n), derivNum(n)
	{
		cout << "SoDerived() : " << derivNum << endl;
	}
	~SoDerived()
	{
		cout << "~SoDerived() : " << derivNum << endl;
	}
};

int main(void)
{
	SoDerived drv1(15);
	SoDerived drv2(27);
	return 0;
}
```
- 유도 클래스의 객체가 소멸될 때에는, 유도 클래스의 소멸자가 실행되고 난 다음에 
기초 클래스의 소멸자가 실행된다.
- 스택에 생성된 객체의 소멸순서는 생성순서와 반대이다.(LIFO)
- 객체 소멸의 특성
  - 기초 클래스의 소멸자와 유도클래스의 소멸자가 모두 호출된다.
  > 생성자에서 동적 할당한 메모리 공간은 소멸자에서 해제한다.

- 유도 클래스의 생성자 및 소멸자 정의 모델
```C++
// DestModel.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Person
{
private:
	char* name;
public:
	Person(const char* myname)
	{
		name = new char[strlen(myname) + 1];		
		strcpy(name, myname);
	}
	~Person()
	{
		delete[]name;			// 소멸자는 위 생성자에서 할당한 메모리 공간을 해제하도록 정의
	}
	void WhatYourName() const
	{
		cout << "My name is " << name << endl;
	}
};

class UnivStudent : public Person
{
private:
	char* major;
public:
	UnivStudent(const char* myname, const char* mymajor)
		: Person(myname)
	{
		major = new char[strlen(mymajor) + 1];
		strcpy(major, mymajor);
	}
	~UnivStudent()
	{
		delete[]major;			// 자신의 생성자에서 할당한 메모리 공간에 대한 해제만 책임지고 있음. 어차피 기초클래스의 소멸자 호출되며
	}							// 기초 클래스의 생성자에서 할당한 메모리 공간을 해제하기 때문.
	void WhoAreYou() const
	{
		WhatYourName();
		cout << "My major is " << major << endl << endl;
	}
};

int main(void)
{
	UnivStudent st1("Kim", "Mathematics");
	st1.WhoAreYou();
	UnivStudent st2("Hong", "Physics");
	st2.WhoAreYou();
	return 0;
}
```

### 07-3 protected 선언과 세 가지 형태의 상속
+ protected로 선언된 멤버가 허용하는 접근의 범위
  - C++ 접근제어 지시자의 접근 범위
    - private < protected < public
    - private와 protected의 범위 매우 유사
    > protected로 선언된 멤버변수는 이를 상속하는 유도 클래스에서 접근이 가능  
    - 유도 클래스에게만 제한적으로 접근 허용한다는 면에서 유용하나, 
기본적으로 기초 클래스와 이를 상속하는 유도 클래스 사이에서도 **정보은닉**은 
지켜지는 것이 좋다.

+ 세 가지 형태의 상속
- public, protected, private 상속
  - 상속 형태 명시
  - class Derived : public Base
  - class Derived : protected Base
  - class Derived : private Base

+ protected 상속
> protected보다 접근의 범위가 넓은 멤버는 protected로 변경시켜 상속하겠다.  
> 기초 클래스의 private 멤버는 유도 클래스에서 접근 불가
```C++
// ProtectedHeri.cpp
#include <iostream>
using namespace std;

class Base
{
private:
	int num1;
protected:
	int num2;
public:
	int num3;

	Base() : num1(1), num2(2), num3(3)
	{ }
};

class Derived : protected Base { };	// empty

int main(void)
{
	Derived drv;
	cout << drv.num3 << endl;			// Base class의 num3에 접근 불가
	return 0;
}
```
> public 멤버변수인 num3은 Derived 클래스에서 protected 멤버가 되어, 외부에서 
접근이 불가능

+ private 상속
  - private보다 접근의 범위가 넓은 멤버는 private으로 변경시켜 상속하겠다.
    - 기초 클래스의 private 멤버는 접근 불가, protected와 public멤버는 private 멤버로 
유도 클래스 내에서만 접근 가능한 멤버가 된다.
    - private 상속이 이뤄진 클래스를 다시 상속할 경우, 멤버함수를 포함해 모든 멤버가 
접근불가가 되기 때문에 의미 없는 상속이 된다.

+ public 상속
  - public보다 접근의 범위가 넓은 멤버는 public으로 변경시켜 상속하겠다.
  - public이 가장 넓은 범위의 접근을 허용하기 때문에,
    > private를 제외한 나머지는 그대로 상속한다  
    > public은 public으로, protected는 protected로.

### 07-4 상속을 위한 조건
+ 상속을 위한 기본 조건인 IS-A 관계의 성립
  - "is a"
    - 전화기를 기초 클래스, 무선 전화기를 유도 클래스라고 할 때,
    - 무선 전화기는 일종의 전화기입니다.
    - 유도 클래스는 is a 기초 클래스
    > 상속관계가 성립하려면, 기초 클래스와 유도 클래스 간에 IS-A 관계가 성립해야 한다.
``` C++
// ISAInheritance.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Computer			// 모든 컴퓨터의 공통적인 특성
{
private:
	char owner[50];				// 소유자 정보
public:
	Computer(const char* name)
	{
		strcpy(owner, name);
	}
	void Calculate()			// 계산
	{
		cout << "요청 내용을 계산합니다." << endl;
	}
};

class NotebookComp : public Computer
{
private:
	int Battery;				// 배터리 있어서 이동하면서 사용 가능
public:
	NotebookComp(const char * name, int initChag) 
		: Computer(name), Battery(initChag)		
	{ }
	void Charging() { Battery += 5; }
	void UseBattery() { Battery += 1; }
	void MovingCal()						// 컴퓨터 사용 시마다 배터리 소모
	{
		if (GetBatteryInfo() < 1)
		{
			cout << "충전이 필요합니다." << endl;
			return;
		}
		cout << "이동하면서 ";
		Calculate();
		UseBattery();
	}
	int GetBatteryInfo() { return Battery; }
};

class TabletNotebook : public NotebookComp		// 펜 있는 컴퓨터
{
private: 
	char regstPenModel[50];
public:
	TabletNotebook(const char* name, int initChag, const char* pen)
		: NotebookComp(name, initChag)
	{
		strcpy(regstPenModel, pen);
	}
	void Write(const char* penInfo)			// 등록된 펜 사용해야 필기가 가능
	{
		if (GetBatteryInfo() < 1)
		{
			cout << "충전이 필요합니다." << endl;
			return;
		}
		if (strcmp(regstPenModel, penInfo) != 0)
		{
			cout << "등록된 펜이 아닙니다.";
			return;
		}
		cout << "필기 내용을 처리합니다." << endl;
		UseBattery();
	}
};

int main(void)
{
	NotebookComp nc("이수종", 5);
	TabletNotebook tn("정수영", 5, "ISE-241-242");
	nc.MovingCal();
	tn.Write("ISE-241-242");
	return 0;
}
```
- UML(Unified Modeling Language)에서, 화살표(상속)의 머리는 기초 클래스를 향하도록 
표시해야 한다.

+ HAS-A의 관계도 상속의 조건은 되지만 복합 관계로 이를 대신하는 것이 일반적이다.
  - 소유의 관계도 상속이 형성 가능
- 예제1
``` C++
// HASInheritance.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Gun
{
private:
	int bullet;		// 장전된 총알 수
public:
	Gun(int bnum) : bullet(bnum)
	{ }
	void Shot()
	{
		cout << "BBANG!" << endl;
		bullet--;
	}
};

class Police : public Gun
{
private:
	int handcuffs;	 // 소유한 수갑의 수
public:
	Police(int bnum, int bcuff)
		: Gun(bnum), handcuffs(bcuff)
	{ }
	void PutHandcuff()
	{
		cout << "SNAP!" << endl;
		handcuffs--;
	}
};

int main(void)
{
	Police pman(5, 3);
	pman.Shot();
	pman.PutHandcuff();
	return 0;
}
```
- 경찰 has a 총(~을 소유한다)
- 예제 2
``` C++
//HASComposite.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Gun
{
private:
	int bullet;
public:
	Gun(int bnum) : bullet(bnum)
	{ }
	void Shot()
	{
		cout << "BBANG" << endl;
		bullet--;
	}
};

class Police
{
private:
	int handcuffs;
	Gun* pistol;			// 상속이 아니라 객체 생성해서 이를 참조
public:
	Police(int bnum, int bcuff)
		: handcuffs(bcuff)
	{
		if (bnum > 0)
			pistol = new Gun(bnum);		// 상속이 아니라 객체 생성해서 이를 참조
		else
			pistol = NULL;
	}
	void PutHandcuff()
	{
		cout << "SNAP!" << endl;
		handcuffs--;
	}
	void Shot()			//	상속이 아니라, Gun 객체를 멤버변수를 통해 참조하는 구조라서, 별도의 함수 정의해야 함
	{
		if (pistol == NULL)
			cout << "Hut BBANG!" << endl;
		else
			pistol->Shot();
	}
	~Police()
	{
		if (pistol != NULL)
			delete pistol;
	}
};

int main(void)
{
	Police pman1(5, 3);
	pman1.Shot();
	pman1.PutHandcuff();

	Police pman2(0, 3);			// 총 소유하지 않은 경찰 객체로 생성.
	pman2.Shot();
	pman2.PutHandcuff();
	return 0;
}
```
- 예제 2가 더 좋음
  - 예제 1은 권총을 소유하지 않은 경찰 표현이 어렵다.
  - 경찰이 권총, 수갑뿐만 아니라 전기봉도 소유했을 겨우 표현하기 어렵다.

> 상속으로 묶인 두 클래스는 강한 연관성을 띤다.  

> 상속은 IS-A 관계의 표현에 매우 적절하나, HAS-A 관계의 표현에 사용 시 프로그램의 
변경에 많은 제약을 가할 수 있다.
