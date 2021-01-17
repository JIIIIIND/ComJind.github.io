---
title: "CPP Module03"
date: 2020-09-24 18:19:21 +0900
categories: 42Seoul CPP
toc: true
toc_sticky: true
toc_label: "CPP Module03"
---

클래스의 상속과 가상 상속을 익힐 수 있음

## ex00

FragTrap 클래스 구현
여기서는 그냥 Coplien's form에 맞춰서 구현을 하기만 하면 됩니다.

## ex01

FragTrap과 거의 유사한 동작을 하는 ScavTrap 구현
여기서도 마찬가지로 Coplien's form에 맞춰서 구현을 하기만 하면 됩니다.

## ex02

FragTrap과 ScavTrap의 공통된 부분을 뽑아낸 ClapTrap 구현하고 상속 관계를 구현

### 상속

기존에 정의되어 있는 클래스의 모든 멤버 변수와 멤버 함수를 물려받아, 새로운 클래스를 작성하는 것을 의미합니다.
이때 기존에 정의된 클래스를 기초 클래스 또는 부모 클래스, 상위 클래스라고 하며 새롭게 작성된 클래스를 파생 클래스 또는 자식 클래스, 하위 클래스라고 합니다.

#### 파생 클래스

```cpp
class 파생 클래스 : <접근 제어 지시자> 기초 클래스[, <접근 제어 지시자> 기초 클래스]
{
	//파생 클래스 멤버 리스트
}
```

접근 제어 지시자는 기초 클래스의 멤버를 사용할 수 있는 파생 클래스의 접근 제어 권한을 설정합니다.
이때 접근 제어 지시자를 생략하면, 파생 클래스의 접근 제어 권한은 private로 기본 설정됩니다.

쉼표를 사용하여 상속받을 기초 클래스를 여러 개 명시할 수 있습니다.
기초 클래스가 하나라면 단일 상속, 여러 개라면 다중 상속이라고 합니다.

**파생 클래스의 특징**

cpp에서 파생 클래스는 다음과 같은 특징을 가집니다.

- 파생 클래스는 반드시 자신만의 생성자를 작성해야 합니다.
- 파생 클래스에는 기초 클래스의 접근할 수있는 모든 멤버 변수들이 저장됩니다.
- 파생 클래스는 기초 클래스의 접근할 수 있는 모든 멤버 함수를 사용할 수 있습니다.
- 파생 클래스에는 필요한 만큼 멤버를 추가할 수 있습니다.

```cpp
class Person
{
	private:
		std::string	_name;
		int			_age;
	public:
		Person(const std::string &name, int age); //기초 클래스 생성자의 선언
		void showPersonInfo();
};
...
Person::Person(const std::string &name, int age) //기초 클래스 생성자의 정의
{
	_name = name;
	_age = age;
}

class Student: public Person
{
	private:
		int	_student_id;
	public:
		Student(int sid, const std::string &name, int age); //파생 클래스 생성자의 선언
};
...
Student::Student(int sid, const std::string &name, int age) : Person(name, age)
{
	_student_id = sid;
}
```

위의 코드에서 보듯이 파생 클래스의 생성자는 기초 클래스의 생성자를 사용하고 있습니다. 파생 클래스의 생성자에서 기초 클래스의 private 멤버에 접근할 수 없기 때문입니다.
기초 클래스의 생성자를 명시하지 않으면 프로그램은 기초 클래스의 기본 생성자를 사용하게 됩니다.

**파생 클래스의 객체가 생성되는 순서**

파생 클래스의 객체가 생성되는 순서는 다음과 같습니다.
1. 기초 클래스의 생성자를 호출
	- 상속받은 멤버 변수의 초기화를 진행
2. 파생 클래스의 생성자가 호출
3. 파생 클래스의 수명이 다하면, 파생 클래스의 소멸자가 호출되고 이후 기초 클래스의 소멸자가 호출

**참고**

<http://www.tcpschool.com/cpp/cpp_inheritance_derivedClass>

## ex03

새로운 NinjaTrap을 구현하고 각각의 Trap들을 받아서 서로 다른 동작을 하는 특수 공격을 구현

### 오버로딩(Overloading)

함수 오버로딩은 다른 매개 변수를 가진 이름의 여러 함수를 만들 수 있는 C++의 기능입니다.

```cpp
int add(int x, int y)
{ return x + y; }
```

위의 함수는 두 개의 정수를 더하여 반환합니다. 부동 소수점 숫자를 더하게 된다면 소수 값을 잃게 되므로 위 함수는 적절하지 않습니다.

함수 오버로딩을 통해 다음과 같은 방식이 가능합니다.

```cpp
int add(int x, int y) { return x + y; }
double add(double x, double y) { return x + y; }
```

네이밍 충돌을 일으킬 것이라 예상할 수도 있지만, 컴ㅂ파일러는 함수 호출에 사용된 인수를 기반으로 호출할 add() 버전을 결정할 수 있습니다. 두 개의 int를 제공하면 add(int, int)를 호출하고 double을 제공하면 add(double, double)을 제공합니다. 각 add()함수가 고유한 매개 변수를 가지고 있는 한 계속 오버로딩할 수 있습니다.

또한 매개 변수의 수가 다른 add() 함수를 정의할 수도 있습니다.

```cpp
int add(int x, int y, int z) { return x + y + z; }
```

**함수의 반환 타입은 함수 오버로딩에서 고려되지 않습니다.** (함수는 인수에만 기반하여 호출됩니다. 반환 값이 포함된 경우 함수의 어떤 버전이 호출되었는지 쉽게 알 수 있는 구문 방식이 없습니다.)

#### 오버로드 함수 호출 시 결과

1. 일치
	- 호출이 특정 오버로드된 함수로 해석됨
2. 불일치
	- 오버로드된 함수 중에 일치하는 함수 없음
3. 모호함
	- 하나 이상의 오버로드된 함수와 인수가 일치

오버로드된 함수가 호출되면 cpp는 다음 프로세스를 통해 호출할 함수의 버전을 결정합니다.

1. cpp는 정확하게 일치하는 함수를 찾으려고 합니다. 이것은 실제 인수가 오버로드된 함수 중 하나의 매개 변수 타입과 정확히 일치하는 경우입니다.

```cpp
void print(char *value);
void print(int value);

print(0) // exact match with print(int)
```

다음 예시에서 print(0)의 0은 char *와도 일치할 순 있지만 int와 정확히 일치함

2. 정확히 일치하는 항목이 없으면 cpp는 승격(promotion)을 통해 일치하는 함수를 찾으려고 합니다.

- char, unsigned char, short -> int
- unsigned short -> int의 크기에 따라 int 또는 unsigned int
- float -> double
- enum(열거형) -> int

3. 승격이 불가능한 경우 C++은 표준 변환을 통해 일치하는 항목을 찾습니다.

- 숫자 타입은 다른 숫자 타입으로 변환(int -> float)
- 열거형(enum)은 위에서 말한 숫자 공식과 같음(enum -> int -> float)
- 0은 포인터 타입 및 숫자 타입과 일치(0 -> char * / 0 -> float)
- 포인터는 void 포인터와 일치함

```cpp
struct Employee;
void print(float value);
void print(Employee value);

print('a');
```

위의 경우 char 타입인 'a'는 print(char), print(int)가 없으므로 print(float)와 일치

4. C++은 사용자 정의 변환을 통해 일치하는 함수를 찾습니다. 클래스는 해당 클래스의 객체와 암시적으로 적용될 수 있는 다른 타입으로 변환을 정의할 수 있습니다. 예를 들어 클래스 X의 사용자 정의 변환을 int로 정의할 수 있습니다.

```cpp
class X;

void print(float value);
void print(int value);

X value;
print(value);
```

value는 클래스 X 타입이지만 사용자가 정의한 int로 변환하므로 print(value)는 print(int)로 변환됩니다.

#### 모호한 일치

```cpp
void print(unsigned int value);
void print(float value);

print('a');
print(0);
print(3.14159);
```

'a'의 경우 일치하는 함수가 없지만 int도 없기 때문에 표준 변환이 됩니다. 그러므로 'a'는 unsigned int 또는 float로 변환될 수 있기에 모호한 일치가 발생합니다.

print(0)의 경우 0은 int이지만 일치하는게 없기 때문에 위와 마찬가지로 모호한 일치가 발생합니다.

print(3.14159)의 경우 f접미사가 없기에 double형이 됩니다. 마찬가지로 표준 변환을 통해 모호한 일치가 발생합니다.

모호한 일치는 컴파일 타임 오류로 간주됩니다. 따라서 컴파일 전에 모호한 일치를 명확히 해야 합니다.
- 호출하려고 하는 타입의 매개 변수를 가지는 새로운 오버로드된 함수를 정의합니다. 그러면 C++는 정확히 일치하는 함수를 찾을 수 있습니다.
- 모호한 인수를 호출할 함수의 타입으로 명시적으로 변환합니다. 예를 들면 다음과 같습니다.

```cpp
print(static_cast<unsigned int>(x));
```

인수가 리터럴인 경우 리터럴 접미사를 사용해 리터럴이 올바른 타입으로 해석되도록 합니다.

```cpp
print(0u); //unsigned int
```

#### 인수가 여러개인 경우

C++은 일치하는 규칙을 차례로 각 인수에 적용합니다.

그러한 함수가 발견된 경우, 그 함수는 가장 일치하는 함수입니다. 그러나 그런 함수를 찾을 수 없는 경우, 호출은 모호함(또는 불일치)으로 간주됩니다.

```cpp
#include <iostream>

void fcn(char c, int x) { std::cout << 'a'; }
void fcn(char c, double x) { std::cout << 'b'; }
void fcn(char c, float x) { std::cout << 'c'; }

int main()
{
	fcn('x', 4);
}
```

모든 함수는 첫 번째 인수와 정확히 일치합니다. 그리고 fcn(char, int)는 두 번째 매개변수와 정확히 일치하며, 다른 함수는 변환이 필요합니다. 따라서 fcn(char, int)가 가장 일치하는 함수입니다.

**참고**

<https://boycoding.tistory.com/221>

## ex04

NinjaTrap과 FragTrap을 상속받는 SuperTrap을 구현

### 다중 상속

다중 상속은 여러 개의 기초 클래스가 가진 멤버를 모두 상속받을 수 있다는 점에서 매우 강력한 상속 방법입니다.
하지만 다중 상속은 단일 상속에는 없는 여러 가지 새로운 문제를 발생시킵니다.

1. 상속받은 여러 기초 클래스에 같은 이름의 멤버가 존재할 가능성이 있습니다.
2. 하나의 클래스를 간접적으로 두 번 이상 상속받을 가능성이 있습니다.
3. 가상 클래스가 아닌 기초 클래스를 다중 상속하면, 기초 클래스 타입의 포인터로 파생 클래스를 가리킬 수 없습니다.

이번 문제에서는 같은 기초 클래스를 상속받은 클래스를 다중 상속하는 문제점을 가지고 있습니다.
이런 경우, 파생 클래스는 두 개의 기초 클래스를 상속받기 때문에 모호함에 빠지게 됩니다.
이를 막기 위해 가상 상속을 사용합니다.

#### 가상 상속

가상 상속은 상속 앞에 virtual 키워드를 붙여 주는 것입니다. 예를 들면 다음과 같습니다.

```cpp
class A
{
	public:
		A() {}
		~A() {}
		int _num_a;
};
class B : virtual public A
{
	public:
		B() {}
		~B() {}
	private:
		int _num_b;
};
class C : virtual public A
{
	public:
		C() {}
		~C() {}
	private:
		int _num_c;
};
class D : public B, public C
{
	public:
		D() {}
		~D() {}
	private:
		int _num_d;
};
```

가상 상속을 하게 되면 vbptr이라는 offset을 가리키는 포인터가 생성되며, virtual로 상속된 클래스는 메모리 구조에서 제일 아래로 가게 됩니다.
시작 offset은 0이 될 수도 있고, 음수가 될 수도 있습니다.
가상 테이블로 인해 메모리 공간이 커질 수 있으며, offset을 이용해 자료를 찾기 때문에 성능에 영향을 줄 수 있습니다.