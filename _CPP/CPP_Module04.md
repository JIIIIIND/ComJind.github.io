---
title: "CPP Module04"
date: 2020-09-24 18:23:33 +0900
categories: 42Seoul CPP
toc: true
toc_sticky: true
toc_label: "CPP Module04"
---

가상 함수와 순수 가상함수, 인터페이스를 익힐 수 있음

## ex00

제공된 main함수의 결과에 맞도록 Victim, Peon, Socerer class를 구성합니다.

아마 별다른 조치 없이 그저 구현만 했을 때 main과 동일한 결과를 얻을 수 있을 것입니다. 하지만 ```Victim *joe = new Peon("joe")``` 처럼 포인터형으로 사용하게 되면 joe는 Peon이 아닌 Victim으로 동작하는 것을 알 수 있고, delete를 했을 때에도 Peon의 소멸자가 호출되지 않는다는 것을 알 수 있습니다.

이를 방지하기 위해 virtual 키워드를 사용한 가상 함수를 사용합니다.

가상 함수로 선언된 멤버 함수가 있고, 이를 오버라이딩한 자식 클래스가 존재 한다면 형변환이 이루어진 포인터 변수는 자식 클래스의 오버라이딩된 함수를 호출할 수 있는 기능이 생깁니다.

모든 자식을 형변환을 통해 부모 클래스로 묶어서 관리를 할 때 많이 사용됩니다. 모든 자식 클래스들은 부모 클래스로 형변환되어 관리되다가 자식 클래스에 있는 변수 정보가 필요할 때 가상함수를 통해 접근할 수 있도록 한 것입니다.

가상 함수를 만들면 기본적으로 V-table이 생성되는데 컴파일러는 V-table에 있는 함수 주소값을 토대로 호출 대상을 찾게 됩니다.

```cpp
#include <iostream>
class A
{
	public:
		virtual void show_p()
		{ std::cout << "class A" << std::endl; }
		void show_normal()
		{ std::cout << "class A" << std::endl; }
};
class B : public A
{
	public:
		virtual void show_p()
		{ std::cout << "class B" << std::endl; }
		void show_normal()
		{ std::cout << "class B" << std::endl; }
};

int main(void)
{
	A *a = new A();
	A *b = new B();
	B b_normal;

	a->show_p();
	a->show_normal();
	b->show_p();
	b->show_normal();
	b_normal.show_p();
	b_normal.show_noraml();

	return (0);
}
```

위의 코드의 결과는 다음과 같습니다.

```
class A
class A
class B
class A
class B
class B
```

**참고**
<https://hwan-shell.tistory.com/60>

## ex01

순수 가상함수란 함수의 몸체를 정의하지 않는 함수입니다.
이러한 순수 가상함수를 하나 이상 멤버함수로 두고 있는 클래스를 추상 클래스라고 합니다.

```cpp
virtual type func_name() = 0;
```

순수 가상 함수의 선언은 위와 같은 형태가 되며, 해당 클래스에서 구현을 할 수 없고, 상속받은 자식 클래스에서 구현을 합니다.

## ex02

cpp에서의 인터페이스는 소멸자와 순수 가상 함수만 선언되어 있는 클래스 입니다.

```cpp
class IClass
{
	public:
		virtual ~IClass();
		virtual void myFunc1() = 0;
		...
};
```

순수 가상 함수만으로 구성되어 있기 때문에 스스로 인스턴스를 생성할 수 없습니다. 해당 인터페이스를 상속받은 자식에서 반드시 구현을 해야 합니다.

## ex03

인터페이스 활용
