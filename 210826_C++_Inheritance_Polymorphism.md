** 백준 문제 풀기

### Review
## 3장 구조체

## 4장 정보은닉
- public: 클래스 외부에서 참조 가능
  - 프로그래머 실수를 컴파일러가 잡지 못할 수 있음
  - 제한된 방법으로의 접근만 허용해, 잘못된 값이 저장되지 않도록 해야하고, 
실수를 쉽게 발견할 수 있도록 해야 함.
  > 멤버변수를 private 선언하고 해당 변수에 접근하는 함수를 별도로 정의해, 
안전한 형태로 멤버변수의 접근을 유도하는 것이 정보은닉  

  > 액세스 함수: 멤버변수 private로 선언하면서, 클래스 외부에서 멤버변수 접근을 
위해 정의하는 함수

- const 함수
  - 멤버변수 read만 가능
### 04-3 생성자와 소멸자
- 생성자의 호출: class 변수 선언 시
- 소멸자의 호출: class 변수가 속해 있는 함수의 종료 시
  - 전역변수(main함수에 있는 것): 프로그램이 끝날 때
  - 동적생성된 Class: delete로 삭제 됐을 때

+ 이니셜라이저

## 7장 상속

|Person|<->|UnivStudent|  
|---|---|---|  
|상위 클래스|<->|하위 클래스|  
|기초(base) 클래스|<->|유도(derived) 클래스|  
|슈퍼(super) 클래스|<->|서브(sub) 클래스|  
|부모 클래스|<->|자식 클래스|  


## 8장 상속과 다형성(Inheritance & Polymorphism)
### 08-1 객체 포인터의 참조관계
+ 객체 포인터 변수: 객체의 주소 값을 저장하는 포인터 변수
  - 클래스 기반 포인터 변수 선언 가능
  - Person * ptr;		// 포인터 변수 선언
  - ptr = new Person();	// 포인터 변수의 객체 참조
  - 위 두 문장이 실행되면, 포인터 ptr은 Person 객체를 가리키게 됨
  - Person형 포인터는 Person 객체뿐만 아니라, **Person을 상속하는 유도 클래스의 
객체도 가리킬 수 있다.**
    - class Student : public Person { }
    - **Person * ptr = new Student();**
  - Student 클래스를 상속하는 유도 클래스 PartTimeStudent의 객체도 가리킬 수 있다.
    - class PartTimeStudent : public Student
    - Person * ptr = new PartTimeStudent();
  - 당연히, Student형 포인터 변수도 PartTimeStudent 객체를 가리킬 수 있다.
  - PartTimeStudent는 Person을 직접 상속하지는 않지만, Student를 상속함으로써 
Person 클래스를 간접 상속 중.
  > C++에서, AAA형 포인터 변수는 AAA 객체 또는 AAA를 직접 혹은 간접적으로 상속하는 
모든 객체를 가리킬 수 있다.(객체의 주소 값을 저장할 수 있다.)
```C++
// ObjectPointer.cpp
#include <iostream>
using namespace std;

class Person
{
public:
	void Sleep() { cout << "Sleep" << endl; }
};

class Student : public Person
{
public:
	void Study() { cout << "Study" << endl; }
};

class PartTimeStudent : public Student
{
public:
	void Work() { cout << "Work" << endl; }
};

int main(void)
{
	Person* ptr1 = new Student();		// Student는 Person을 상속하므로, Person형 포인터 변수는 Student 객체를 가리킬 수 있음.
	Person* ptr2 = new PartTimeStudent();	// PartTimeStudent는 Person을 간접 상속하므로, Person형 포인터 변수는 PartTimeStudent 객체를 가리킬 수 있음.
	Student* ptr3 = new PartTimeStudent();	// PartTimeStudent는 Student를 상속하므로, Student형 포인터 변수는 PartTimeStudent 객체를 가리킬 수 있음.
	ptr1->Sleep();
	ptr2->Sleep();
	ptr3->Study();
	delete ptr1; delete ptr2; delete ptr3;
	return 0;
}
```

+ 유도 클래스의 객체도 가리킬 수 있음.
  - IS-A 관계의 성립으로, 위의 예시가 적절한 상속임을 이해할 수 있음
  - PartTimeStudent(근로학생)
  - Student is a Person
  - PartTimeStudent is a Student
  - PartTimeStudent is a Person	// 간접 상속에서도 IS-A는 유지
    - ~은 ~이다.
    - ~은 ~의 일종이다.

+ 앞선 7장 예제 중, '급여관리 확장성 문제'의 1차적 해결과 함수 오버라이딩
  - 위의 특성(기초 클래스형 포인터 변수는 직접 또는 간접적으로 상속하는 모든 객체를 
가리킬 수 있다.)을 통해 급여관리 확장성 문제를 해결할 실마리를 얻을 수 있음.
    - 추가하려고 했던 영업직(Sales)과 임시직(Temporary)는 모두 고용의 한 형태이며(정규직까지), 
영업직은 정규직의 일종임.
    - class Employee
    - class PermanentWorker : public Employee
      - class SalesWorker : public PermanentWorker
    - class TemporaryWorker : public Employee 

  - 이는 EmployeeHandler 클래스를 다음과 같이 설계할 수 있음.
    > EmployeeHandler 클래스가 저장 및 관리하는 대상이 Employee 객체가 되게 하면, 
이후에 Employee 클래스를 직접 혹은 간접적으로 상속하는 클래스가 추가되었을 때, 
EmployeeHandler 클래스에는 변화가 발생하지 않는다.
```C++
// EmployeeManager2.cpp
#include <iostream>
#include <cstring>
using namespace std;

class Employee		// 고용인 클래스 
{
private:
	char name[100];
public:
	Employee(const char* name)
	{
		strcpy(this->name, name);
	}
	void ShowYourName() const
	{
		cout << "name: " << name << endl;
	}
};

class PermanentWorker : public Employee		//	Employee 클래스 상속
{
private:
	int salary;	// 월급
public:
	PermanentWorker(const char* name, int money)
		: Employee(name), salary(money)
	{	}
	int GetPay() const
	{
		return salary;
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;
	}
};

class EmployeeHandler			// 저장의 대상이 Employee 객체. (PermanentWorker 객체도 Employee 객체의 일종)
{
private:
	Employee* empList[50];		// Employee 객체의 주소 값을 저장하는 방식으로 객체 저장. 따라서, **Employee 클래스를 상속하는 클래스의 객체도 함께 저장 가능.**
	int empNum;
public:
	EmployeeHandler() : empNum(0)
	{ }
	void AddEmployee(Employee* emp)	// 함수 인자로 Employee 객체의 주소 값을 전달해야 하므로, PermanentWorker 객체의 주소 값도 전달 가능.
	{
		empList[empNum++] = emp;
	}
	void ShowAllSalaryInfo() const
	{
		/*
		for(int i = 0; i < empNum; i++)
			empList[i]->ShowsalaryInfo();			// 컴파일 에러
		*/
	}
	void ShowTotalSalary() const
	{
		int sum = 0;
		/*
		for(int i = 0; i < empNum; i++)
			sum += empList[i]->GetPay();			// 컴파일 에러
		*/
		cout << "salary sum: " << sum << endl;
	}
	~EmployeeHandler()
	{
		for (int i = 0; i < empNum; i++)
			delete empList[i];
	}
};

int main(void)
{
	// 직원관리 목적으로 설계된 컨트롤 클래스의 객체 생성
	EmployeeHandler handler;

	// 직원 등록
	handler.AddEmployee(new PermanentWorker("KIM", 1000));
	handler.AddEmployee(new PermanentWorker("LEE", 1500));
	handler.AddEmployee(new PermanentWorker("JUN", 2000));

	// 이번 달에 지불해야 할 급여의 정보
	handler.ShowAllSalaryInfo();

	// 이번 달에 지불해야 할 급여의 총합
	handler.ShowTotalSalary();
	return 0;
}
```

``` C++
class TemporaryWorker : public Employee		// 임시직
{
private:
	int workTime;		// 이 달의 근무시간 합계
	int payPerHour;		// 시간 당 급여
public:
	TemporaryWorker(char * name, int pay)
		: Employee(name), workTime(0), payPerHour(pay)
	{ }
	void AddWorkTime(int time)
	{
		workTime += time;
	}
	int GetPay() const
	{
		return workTime * payPerHour;
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;
	}

};

class SalesWorker : public PermanentWorker		// 영업직도 정규직의 일종이기 때문에, Employee가 아닌 PermanentWorker 상속
{
private:
	int salesResult;		// 월 판매실적
	double bonusRatio;		// 상여금 비율
public:
	SalesWorker(char* name, int money, double ratio)
		: PermanentWorker(name, money), salesResult(0), bonusRatio(ratio)
	{ }
	void AddSalesResult(int value)
	{
		salesResult += value;
	}
	int GetPay() const
	{
		return PermanentWorker::GetPay()		// PermanentWorker의 GetPay 함수 호출
			+ (int)(salesResult * bonusRatio);
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;	// SalesWorker의 GetPay 함수가 호출됨
	}
};
```
> 함수 오버라이딩(Function Overriding)
  - PermanenWorker 클래스도 GetPay()와 ShowSalaryInfo()가 있는데, 유도 클래스인 
SalesWorker 클래스에도 동일한 이름과 형태로 두 함수 정의
  - 함수 오버라이딩 시, 오버라이딩 된 기초 클래스의 함수는 오버라이딩을 한 유도 
클래스의 함수에 가려진다.
  - SalesWorker 클래스 내에서 GetPay()를 호출할 경우, SalsesWorker 클래스에 정의된 
GetPay 함수가 호출됨.
  - SalesWorker의 객체를 선언한 뒤, 해당객체로 함수를 사용하는 경우에도 
SalsesWorker에 정의된 GetPay()가 호출됨.
``` C++
int main(void)
{
  SalesWorker seller("Hong", 1000, 0.1);
  cout<<seller.GetPay()<<endl;		// SalesWorker의 GetPay() 호출
  seller.ShowSalaryInfo();
}
```
  - SalesWorker에 정의된 Getpay()를 보면, PermanentWorker::GetPay()가 보임.
    - 이는 오버라이딩 된 기초 클래스의 Getpay() 호출하는 구문
    > 클래스 명시함으로써, 기초 클래스의 오버라이딩 된 함수 호출 가능.
``` C++
int main(void)
{
  SalesWorker seller("Hong", 1000, 0.1);
  cout<<seller.PermanentWorker::GetPay()<<endl; // PermanentWorker의 GetPay() 호출
  seller.PermanentWorker::ShowSalaryInfo();	// PermanentWorker의 ShowSalaryInfo() 호출
}
```
  - seller.PermanentWorker::ShowSalaryInfo();
    > seller 객체의 PermanentWorker 클래스에 정의된 ShowSalaryInfo()를 호출하라.

> 함수 오버라이딩 vs 함수 오버로딩
  - 기초 클래스와 동일한 이름의 함수를 유도 클래스에서 정의한다고 해서 무조건 함수 
오버라이딩이 되는 것 아님.
  - 매개변수의 자료형 및 개수가 다르면, 함수 오버로딩 됨.
  - 전달되는 인자에 따라 호출되는 함수 결정
  > 함수 오버로딩은 상속 관계에서도 구성 가능.

``` C++
#include <iostream>
#include <cstring>
using namespace std;

class Employee		// 고용인 클래스 
{
private:
	char name[100];
public:
	Employee(const char* name)
	{
		strcpy(this->name, name);
	}
	void ShowYourName() const
	{
		cout << "name: " << name << endl;
	}
};

class PermanentWorker : public Employee		//	Employee 클래스 상속
{
private:
	int salary;	// 월급
public:
	PermanentWorker(const char* name, int money)
		: Employee(name), salary(money)
	{	}
	int GetPay() const
	{
		return salary;
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;
	}
};

class TemporaryWorker : public Employee		// 임시직
{
private:
	int workTime;		// 이 달의 근무시간 합계
	int payPerHour;		// 시간 당 급여
public:
	TemporaryWorker(char* name, int pay)
		: Employee(name), workTime(0), payPerHour(pay)
	{ }
	void AddWorkTime(int time)
	{
		workTime += time;
	}
	int GetPay() const
	{
		return workTime * payPerHour;
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;
	}

};

class SalesWorker : public PermanentWorker		// 영업직도 정규직의 일종이기 때문에, Employee가 아닌 PermanentWorker 상속
{
private:
	int salesResult;		// 월 판매실적
	double bonusRatio;		// 상여금 비율
public:
	SalesWorker(char* name, int money, double ratio)
		: PermanentWorker(name, money), salesResult(0), bonusRatio(ratio)
	{ }
	void AddSalesResult(int value)
	{
		salesResult += value;
	}
	int GetPay() const
	{
		return PermanentWorker::GetPay()		// PermanentWorker의 GetPay 함수 호출
			+ (int)(salesResult * bonusRatio);
	}
	void ShowSalaryInfo() const
	{
		ShowYourName();
		cout << "salary: " << GetPay() << endl << endl;	// SalesWorker의 GetPay 함수가 호출됨
	}
};

class EmployeeHandler			// 저장의 대상이 Employee 객체. (PermanentWorker 객체도 Employee 객체의 일종)
{
private:
	Employee* empList[50];		// Employee 객체의 주소 값을 저장하는 방식으로 객체 저장. 따라서, **Employee 클래스를 상속하는 클래스의 객체도 함께 저장 가능.**
	int empNum;
public:
	EmployeeHandler() : empNum(0)
	{ }
	void AddEmployee(Employee* emp)	// 함수 인자로 Employee 객체의 주소 값을 전달해야 하므로, PermanentWorker 객체의 주소 값도 전달 가능.
	{
		empList[empNum++] = emp;
	}
	void ShowAllSalaryInfo() const
	{
		/*
		for(int i = 0; i < empNum; i++)
			empList[i]->ShowsalaryInfo();			// 컴파일 에러
		*/
	}
	void ShowTotalSalary() const
	{
		int sum = 0;
		/*
		for(int i = 0; i < empNum; i++)
			sum += empList[i]->GetPay();			// 컴파일 에러
		*/
		cout << "salary sum: " << sum << endl;
	}
	~EmployeeHandler()
	{
		for (int i = 0; i < empNum; i++)
			delete empList[i];
	}
};


int main(void)
{
	// 직원관리 목적으로 설계된 컨트롤 클래스의 객체 생성
	EmployeeHandler handler;

	// 정규직 등록
	handler.AddEmployee(new PermanentWorker("KIM", 1000));
	handler.AddEmployee(new PermanentWorker("LEE", 1500));

	// 임시직 등록
	TemporaryWorker* alba = new TemporaryWorker("Jung", 700);
	alba->AddWorkTime(5);
	handler.AddEmployee(alba);

	// 영업직 등록
	SalesWorker* seller = new SalesWorker("Hong", 1000, 0.1);
	seller->AddSalesResult(7000);
	handler.AddEmployee(seller);

	// 이번 달에 지불해야 할 급여의 정보
	handler.ShowAllSalaryInfo();

	// 이번 달에 지불해야 할 급여의 총합
	handler.ShowTotalSalary();
	return 0;
}
```

+ SalesWorker 클래스에서 ShowSalaryInfo 함수를 오버라이딩 한 이유
  > SalesWorker 클래스의 ShowSalaryInfo 함수는 PermanentWorker 클래스의 
ShowSalaryInfo 함수와 완전히 동일한데, 굳이 오버라이딩 한 이유가 무엇?
    - PermanentWorker 클래스의 ShowSalaryInfo 함수는 상속에 의해 SalesWorker 객체에도 
존재하게 됨.
    - 그러나 상속되었다고 해도, PermanentWorker 클래스의 ShowSalaryInfo 함수 내에서 
호출되는 GetPay()는 PermanentWorker 클래스에 정의된 GetPay() 호출로 이어짐.
    - 따라서 SalesWorker 클래스에 정의된 GetPay()가 호출되도록 SalesWorker 클래스에 
별도로 ShowSalaryInfo()를 정의해야만 함.

### 08-2 가상함수(Virtual Function)
+ 기초 클래스의 포인터로 객체를 참조하면?
  - 기초 클래스의 포인터로 유도 클래스의 객체를 가리킬 수 있음
  > But, 기초 클래스의 포인터로 유도 클래스의 함수를 사용할 수는 없음. (포인터 연산 불가)
    - bptr이 기초 클래스의 포인터이기 때문
``` C++
int main(void)
{
  Base * bptr = new Derived();	// Compile OK
  bptr->DerivedFunc();	// Compile Error
}
```
  > C++ 컴파일러는 포인터 연산의 가능성 여부를 판단할 때, **포인터의 자료형을 
기준으로 판단함.** 실제 가리키는 객체의 자료형이 기준이 아님.
    - 다음의 코드도 Error. 포인터 bptr의 포인터 형만을 가지고 대입 가능성을 판단하기 때문
``` C++
int main(void)
{
  Base * bptr = new Derived();	// Compile OK
  Derived * dptr = bptr;		// Compile Error
}
```
  - 위의 첫 번째 문장: Derived 클래스는 Base 클래스의 유도 클래스니까, Base 클래스의 
포인터 변수로 Derived 객체의 참조가 가능.
  - 두 번째 문장: 컴파일러는 bptr이 실제로 가리키는 객체가 Derived 객체라는 사실을 
기억하지 않음.
    - bptr은 Base형 포인터니까, bptr이 가리키는 대상은 Base 객체일 수 있는 것. 
그런 경우에는 이 문장 성립 X, 컴파일 에러

- 반면 아래 코드는 가능
``` C++
int main(void)
{
  Derived *dptr = new Derived();
  Base * bptr = dptr;
}
```
  - 컴파일러는 두 번째 문장을 이렇게 이해
    > dptr은 Derived 클래스의 포인터 변수니까, 이 포인터가 가리키는 객체는 분명 
Base 클래스를 직접 혹은 간접적으로 상속하는 객체이다. 그러니 Base형 포인터 
변수로도 참조 가능!  

> 포인터 형에 해당하는 클래스에 정의된 멤버에만 접근이 가능

> C++ 컴파일러는 포인터를 이용한 연산의 가능성 여부를 판단할 때, 포인터의 자료형을 
기준으로 판단하지, 실제 가리키는 객체의 자료형을 기준으로 판단하지 않음.

+ 함수의 오버라이딩과 포인터 형
``` C++
#include <iostream>
using namespace std;

class First
{
public:
	void MyFunc() { cout << "FirstFunc" << endl; }
};

class Second : public First
{
public:
	void MyFunc() { cout << "SecondFunc" << endl; }
};

class Third : public Second
{
public:
	void MyFunc() { cout << "ThirdFunc" << endl; }
};

int main(void)
{
	Third* tptr = new Third();	// Third 객체 생성
	Second* sptr = tptr;	// Third형, Second형, First형 포인터로 이를 참조
	First* fptr = sptr;

	fptr->MyFunc();		// 각 포인터를 이용해 MyFunc() 호출
	sptr->MyFunc();
	tptr->MyFunc();
	delete tptr;
	return 0;
}
```
- 3개 클래스가 상속으로 연결됨. 모두 MyFunc 함수를 통해 오버라이딩 관계 형성
  - sptr->MyFunc();를 본 포인터의 판단
    - sptr이 Second형 포인터이니, 이 포인터가 가리키는 개체에는 First의 MyFunc와 
Second의 MyFunc 함수가 오버라이딩 관계로 존재한다. 그러므로, 오버라이딩 한 
Second의 MyFunc 함수를 호출해야 한다.

+ 가상함수(Virtual Function)
  - C++이 아닌 객체지향의 개념
  - 가상함수의 선언은 virtual 키워드의 선언을 통해 이뤄짐.
  - 가상함수 선언되고 나면, 이 함수를 오버라이딩하는 함수도 가상함수가 됨.
    - First 클래스의 MyFunc()가 virtual로 선언되면, Second와 Third의 MyFunc()도 
가상함수가 됨. 
``` C++
// FunctionVirtualOverride.cpp
#include <iostream>
using namespace std;

class First
{
public:
	//void MyFunc() { cout << "FirstFunc" << endl; }
	virtual void MyFunc() { cout << "FirstFunc" << endl; }
};

class Second : public First
{
public:
	//void MyFunc() { cout << "SecondFunc" << endl; }
	virtual void MyFunc() { cout << "SecondFunc" << endl; }	// 굳이 virtual 안 써도 가상함수 되나, 사용해서 알리는 것이 좋음
};

class Third : public Second
{
public:
	//void MyFunc() { cout << "ThirdFunc" << endl; }
	virtual void MyFunc() { cout << "ThirdFunc" << endl; }	// 굳이 virtual 안 써도 가상함수 되나, 사용해서 알리는 것이 좋음
};

int main(void)
{
	Third* tptr = new Third();
	Second* sptr = tptr;
	First* fptr = sptr;

	fptr->MyFunc();
	sptr->MyFunc();
	tptr->MyFunc();
	delete tptr;
	return 0;
}
```
> 함수가 가상함수로 선언되고 해당 함수 호출 시, 포인터의 자료형을 기반으로 호출대상을 
결정하지 않고, 포인터 변수가 실제로 가리키는 객체를 참조하여 호출 대상 결정.  

    > empList에 들어가는 실 객체는 각각 이름을 가진 사람들(각각 클래스 가짐)  



* 상속의 이유(급여관리 확장성 문제의 해결을 통해 확인한)
  > 상속을 통해 연관된 일련의 클래스에 대한 공통적인 규약을 정의할 수 있음.
    > 상속을 통해 연관된 일련의 클래스 PermanentWorker, TemporaryWorker, 
SalesWorker에 공통적인 규약 정의 가능.
    - 공통적인 규약은 Employee 클래스(적용하고픈 공통 규약 모아 Employee 클래스 정의)
      > Employee 클래스를 상속하는 모든 클래스의 객체는 Employee 객체로 볼 수 있음.
  - 실제 EmployeeHandler 클래스는 저장되는 모든 객체를 Employee 객체로 봄.
    - 새 클래스 추가되어도, EmployeeHandler 클래스 수정 X
    - 객체의 자료형에 따라 호출되는 함수에는 차이가 있어야 함.
      - 함수 오버라이딩과 가상함수로 해결

+ 순수 가상함수와 추상 클래스(Pure Virtual Function & Abstract Class)
- Employee 클래스는 기초 클래스로만 의미가 있음. 객체 생성이 목적이 아님
  - 클래스 중에는 객체 생성을 목적으로 정의되지 않는 클래스 존재
    - Employee * emp = new Employee("Lee Dong Sook"); // 실수
    - 문법 상 문제 없어 컴파일러도 잡을 수 없으므로, 가상함수를 순수 가상함수로 
선언해, 객체 생성을 문법적으로 막는 것이 좋음.
``` C++
class Employee
{
private:
  char name[100];
public:
  Employee(char * name) { ... }
  void ShowYourName() const { ... }
  virtual int GetPay() const = 0;	    // 순수 가상함수
  virtual void ShowSalaryInfo() const = 0; // 순수 가상함수
};
```
> 순수 가상함수: 함수의 몸체가 정의되지 않은 함수
  - 이를 표현하기 위해, 0의 대입을 표시함.
  - 명시적으로 몸체가 정의되지 않았음을 컴파일러에게 알리는 것(0의 대입 의미 X)
    - 컴파일러는 함수의 몸체가 정의되지 않았다고 컴파일 오류를 일으키지는 않으나, 
Employee 클래스는 순수 가상함수를 지닌, 완전치 못한 클래스가 되어, 객체를 생성하려고 
하면 컴파일 에러 발생
  - 이를 통해 잘못된 객체 생성 막을 수 있음.
  - 유도 클래스에 정의된 함수의 호출을 돕는데 의미가 있을 뿐, 실제 실행이 되지 않는 
함수를 명확하게 하는 효과.
  > 하나 이상의 멤버함수를 순수 가상함수로 선언한 클래스를 추상클래스라고 함.


* 다형성(Polymorphism)
- 가상함수의 호출관계에서 보인 특성
- 동질이상
  - 모양은 같은데 형태는 다름
  - 문장은 같은데 결과가 다름
``` C++
class First
{
public:
  virtual void SimpleFunc() { cout<<"First"<<endl; }
};
class Second : public First
{
public:
   virtual void SimpleFunc() { cout<<"Second"<<endl; }
};

int main(void)
{
  First * ptr = new First();
  ptr->SimpleFunc();	// 동일한 문장 존재
  delete ptr;
  
  ptr = new Second();
  ptr->SimpleFunc();	// 동일한 문장 존재
  delete ptr;
  return 0;
}
```
- ptr은 동일한 포인터 변수임에도 불구하고, 실행결과가 다름.
  - ptr이 참조하는 객체의 자료형이 다르기 때문

### 08-3 가상 소멸자와 참조자의 참조 가능성
- 가상함수 말고도, 소멸자에도 virtual 선언이 가능

+ 가상 소멸자(Virtual Destructor)
``` C++
// VirtualDestructor.cpp
#include <iostream>
using namespace std;

class First
{
private:
	char* strOne;
public:
	First(const char* str)
	{
		strOne = new char[strlen(str) + 1];
	}
	~First()
	{
		cout << "~First()" << endl;
		delete[]strOne;		// 생성자에서 동적할당이 있었기에 적절한 소멸자
	}
};

class Second : public First
{
private:
	char* strTwo;
public:
	Second(const char* str1, const char* str2) : First(str1)
	{
		strTwo = new char[strlen(str2) + 1];
	}
	~Second()
	{
		cout << "~Second()" << endl;
		delete[]strTwo;		// 생성자에서 동적할당이 있었기에 적절한 소멸자
	}
};

int main(void)
{
	First* ptr = new Second("simple", "complex");
	delete ptr;		// 바로 윗행에서 생성한 객체 소멸. 이 경우 First와 Second클래스 소멸자가 동시에 호출되어야
	return 0;
}
```
- main에서 객체의 소멸을 First 포인터로 명령
  - First 클래스의 소멸자만 호출
    - 메모리 누수(leak) 발생
  > 객체 소멸과정에서는 delete 연산자에 사용된 포인터의 자료형에 상관없이 
모든 소멸자 호출되어야 함.
    > 소멸자에 virtual 선언 추가하면 됨.
``` C++
virtual ~First()
{
  cout<<"~First()"<<endl;
  delete []strOne;
}
```
- 가상함수와 마찬가지로, 소멸자도 상속의 계층구조상 맨 위에 존재하는 기초 클래스의 
소멸자만 virtual로 선언하면, 이를 상속하는 유도 클래스의 소멸자들도 모두 '가상 소멸자'로 
선언됨. 
- 가상 소멸자가 호출되면, 상속의 계층구조상 맨 아래에 존재하는 유도 클래스의 소멸자가 
대신 호출되면서, 기초 클래스의 소멸자가 순차적으로 호출됨.

``` C++ 
class First
{
  ...
public:
  virtual ~First() { ... }
};
class Second : public First
{
  ...
public:
  virtual ~Second() { ... }
};
class Third : public Second
{
  ...
public:
  virtual ~Third() { ... }
};

int main(void)
{
  First * ptr = new Third();
  delete ptr;
  ...
}
```
- 위 코드에서 모든 소멸자의 호출과정은
  1. 객체 소멸과정에서 virtual ~First() 호출
  2. virtual 소멸자이므로, virtual ~Third()이 대신 호출됨
  3. ~Third() 실행 후, virtual ~Second() 실행
  4. ~Second() 실행 후, virtual ~First() 실행

``` C++
// VirtualDestructor2.cpp
#include <iostream>
using namespace std;

class First
{
private:
	char* strOne;
public:
	First(const char* str)
	{
		strOne = new char[strlen(str) + 1];
	}
	virtual ~First()
	{
		cout << "~First()" << endl;
		delete[]strOne;		// 생성자에서 동적할당이 있었기에 적절한 소멸자
	}
};

class Second : public First
{
private:
	char* strTwo;
public:
	Second(const char* str1, const char* str2) : First(str1)
	{
		strTwo = new char[strlen(str2) + 1];
	}
	virtual ~Second()
	{
		cout << "~Second()" << endl;
		delete[]strTwo;		// 생성자에서 동적할당이 있었기에 적절한 소멸자
	}
};

int main(void)
{
	First* ptr = new Second("simple", "complex");
	delete ptr;		// 바로 윗행에서 생성한 객체 소멸. 이 경우 First와 Second클래스 소멸자가 동시에 호출되어야
	return 0;
}
```

+ 참조자의 참조 가능성
- C++에서, AAA형 포인터 변수는 AAA 객체 또는 AAA를 직접 혹은 간접적으로 상속하는 
모든 개체를 가리킬 수 있다.(객체의 주소값을 저장할 수 있다.)
  > C++에서, AAA형 **참조자**는 AAA 객체 또는 AAA를 직접 혹은 간접적으로 상속하는 
모든 개체를 참조할 수 있다.

- First형 포인터 변수를 이용하면 First 클래스에 정의된 MyFunc 함수가 호출되고, 
Second형 포인터 변수를 이용하면 Second 클래스에 정의된 MyFunc 함수가 호출되고, 
Third형 포인터 변수를 이용하면 Third 클래스에 정의된 MyFunc 함수가 호출된다.
  > First형 참조자를 이용하면 First 클래스에 정의된 MyFunc 함수가 호출되고, 
Second형 참조자를 이용하면 Second 클래스에 정의된 MyFunc 함수가 호출되고, 
Third형 참조자를 이용하면 Third 클래스에 정의된 MyFunc 함수가 호출된다.

``` C++
// ReferenceAttribute.cpp
#include <iostream>
using namespace std;

class First
{
public:
	void FirstFunc() { cout << "FirstFunc()" << endl; }
	virtual void SimpleFunc() { cout << "First's SimpleFunc()" << endl; }
};

class Second : public First
{
public:
	void SecondFunc() { cout << "SecondFunc()" << endl; }
	virtual void SimpleFunc() { cout << "Second's SimpleFunc()" << endl; }
};

class Third : public Second
{
public:
	void ThirdFunc() { cout << "ThirdFunc()" << endl; }
	virtual void SimpleFunc() { cout << "Third's SimpleFunc()" << endl; }
};

int main(void)
{
	Third obj;
	obj.FirstFunc();
	obj.SecondFunc();
	obj.ThirdFunc();
	obj.SimpleFunc();

	Second& sref = obj; // obj는 Second를 상속하는 Third객체이므로, Second형 참조자로 참조 가능
	sref.FirstFunc();	// 컴파일러는 참조자의 자료형을 가지고 함수의 호출 가능성 판단, First 클래스에 정의된 FirstFunc 함수와 
	sref.SecondFunc();	// Second 클래스에 정의된 SecondFunc함수는 호출이 가능하지만, ThirdFunc는 불가
	sref.SimpleFunc();		// SimpleFunc는 가상함수이므로, Third 클래스에 정의된 SimpleFunc()가 호출됨.

	First& fref = obj;	// obj는 First를 간접 상속하는 Third 객체이므로, First형 참조자로 참조 가능
	fref.FirstFunc();	// 컴파일러는 참조자 자료형 갖고 함수 호출 가능성 판단하므로, First 클래스에 정의된 함수만 호출 
	fref.SimpleFunc();	// 가능. 이 중 SimpleFunc는 가상함수이므로, Third 클래스에 정의된 SimpleFunc()가 호출
	return 0;
}
```
- void GoodFunction(const First &ref) { ... }
  > First 객체 또는 First를 직접 혹은 간접적으로 상속하는 클래스의 객체가 인자의 대상
  
  > 인자로 전달되는 개체의 실제 자료형에 상관없이, 함수 내에서는 First 클래스에 정의된 
함수만 호출 가능






