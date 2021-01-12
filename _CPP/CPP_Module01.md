---
title: "CPP Module01"
date: 2020-09-24 18:05:10 +0900
categories: 42Seoul CPP
toc: true
toc_sticky: true
toc_label: "CPP Module01"
---

## ex00

Pony 클래스를 작성하고, Heap 영역과 Stack영역에 생성하는 문제

cpp에서는 동적 할당을 어떻게 사용하는지, Stack에 생성한 것과는 어떤 점에서 차이가 있는지 알 수 있음

동적 할당을 위해서는 다음 키워드를 사용합니다.
- new

```cpp
Pony newPony = new Pony(name, color);
```

new 키워드 뒤에 생성자를 입력

할당 해제를 위해서는 다음 키워드를 사용합니다.
- delete

```cpp
delete newPony;
```

스택 영역으로 생성한 경우 기본 생성자가 정의되어 있어야 합니다.
만약 기본 생성자가 없거나 private에 속한 경우 컴파일시 에러가 발생합니다.

## ex01

주어진 코드 조각을 보고 메모리 누수를 해결하는 문제

앞의 문제를 해결했으면 곧바로 문제점을 알 수 있음

## ex02

Zombie클래스를 작성하고, ZombieEvent 클래스를 통해 새로운 좀비를 생성하고 반환하는 newZombie 메소드와 랜덤한 이름을 가지는 좀비를 생성하는 randomChump 함수를 구현

## ex03

ZombieHorde 클래스를 구현.

객체 배열을 생성하기 위해서는 다음과 같이 사용함

```cpp
arr = new Class[nbr];
```

일반 객체와 마찬가지로 new 키워드를 사용하지만 클래스 뒤에 []를 붙여 배열의 크기를 지정합니다.

위의 방식으로 생성 시 기본 생성자가 호출되기 때문에 기본 생성자를 정의해야 합니다. 혹은 ```Class(int x = 0, int y = 0)``` 과 같이 인자를 받지 않아도 생성자가 정상적으로 호출될 수 있게 default argument 설정을 해줘야 합니다.

또는 다음과 같은 방식으로 생성해도 됩니다.

```cpp
Point pointArr[] = {Point(), Point(), Point()};
```

객체의 포인터에 대한 배열을 선언하고 객체를 동적으로 다음과 같이 할당하는 경우에는 그저 배열을 선언하는 것이기 때문에 어떠한 객체도 생성되지 않습니다.

```cpp
Point *arr[5];
for (int i = 0; i < 5; i++)
	arr[i] = new Point(i, i);
```

정리하자면, CPP에서 객체에 대한 배열은 선언과 동시에 항상 기본 생성자 혹은 default arguments에 의해 인자 없이도 호출 가능한 생성자로 초기화 됩니다.

객체 배열의 제거는 다음과 같습니다.

```cpp
delete[] arr;
```

## ex04

reference와 pointer를 사용하여 문자열을 각각 출력해보는 문제


### Reference type

참조형 변수는 변수를 복사하지 않고 직접 객체를 이용합니다. 참조형 변수는 자신이 참조하는 변수의 별명이라고도 합니다.

cpp에서는 세 종류의 참조형을 지원합니다.
- non-const 값 참조형
- const 값 참조형
- r-value 참조형

#### non-const 값 참조형

non-const 값에 대한 참조형은 자료형 뒤에 &를 붙여서 선언합니다.

```cpp
int	value = 5;
int	&ref = value;
```

위 코드에서 &는 변수의 주소를 의미하지 않고 참조를 의미합니다.

참조형은 참조하는 값과 동일하게 동작하기 때문에 참조되는 객체의 별칭으로 사용됩니다.

```cpp
#include <iostream>

int	main()
{
	int	value = 5;
	int	&ref = value;

	value = 6;
	ref = 7;
	std::cout << value;
	++ref;
	std::cout << value;
	return (0);
}
```

위의 결과로 출력은 7과 8이 나오게 됩니다.
참조형에 주소 연산자(&)를 사용하면 참조되는 값의 주소가 반환됩니다.

**ㅣ-value, r-value**

l_value는 메모리 주소를 가진 객체, r-value는 메모리 주소가 없고, 표현식 범위에만 있는 임시 값

참조형은 선언과 동시에 반드시 초기화 해야 합니다.
null값을 참조할 수 없으며, non-const 값에 대한 참조는 non-const 값으로만 초기화 할 수 있습니다. const값 또는 r-value로 초기화 할 수 없습니다.

```cpp
int	x = 5;
int	&ref1 = x; //true

const int	y = 7;
int	&ref2 = y; //false

int	&ref3 = 6; //false
```

초기화가 된 이후에는 다른 변수를 참조하도록 변경할 수 없습니다.

#### 사용 예시

함수 매개 변수로 가장 많이 사용됩니다. 이때 매개 변수는 인수의 별칭으로 사용되며, 복사본이 만들어지지 않습니다. 이렇게 하면 복사하는데 비용이 많이 드는 경우 성능이 향상될 수 있습니다.

참조형은 인수의 별칭으로 상요되므로 참조 매개 변수를 사용하는 함수는 전달된 인수를 수정할 수 있습니다.

```cpp
#include <iostream>

void	changeN(int &ref)
{
	ref = 6;
}

int		main()
{
	int	n = 5;
	std::cout << n << std::endl;
	changeN(n);
	std::cout << n << std::endl;
	return (0);
}
```

참조형의 또 다른 장점은 중첩된 데이터에 쉽게 접근할 수 있다는 점입니다.

```cpp
struct Something
{
	int		value1;
	float	value2;
};

struct Other
{
	Somthing	somthing;
	int			otherValue;
}

Other other;
```

other.somthing.value1에 접근하는 경우 타이핑 양도 많아지고 코드가 엉망이 될 수 있습니다. 하지만 참조를 통해 좀 더 쉽게 접근할 수 있습니다.

```cpp
int &ref = other.somthing.value1;

other.somthing.value1 = 5;
ref = 5;
```

위의 두 명령문은 같은 동작을 하게 됩니다.

#### Reference vs Pointer

참조형은 접근할 때 암시적으로 역참조되는 포인터와 같은 역할을 합니다.(참조형은 내부적으로 포인터를 사용하여 컴파일러에서 구현됩니다)

```cpp
int value = 5;
int *const ptr = &value;
int &ref = value;
```

*ptr과 ref는 동일하게 평가됩니다. 그러므로 다음 두 명령문은 같은 효과를 냅니다.

```cpp
*ptr = 5;
ref = 5;
```

참조형은 선언과 동시에 유효한 객체로 초기화 해야 하고, 일단 초기화되면 변경할 수 없으므로 포인터보다 사용하는 것이 훨씬 안전합니다.

#### const 값 참조형

const값에 대한 포인터를 선언하는 것처럼 const 값에 대한 참조를 선언할 수 있습니다.

```cpp
const int value = 5;
const int &ref = value;
```

const 참조는 non-const 값, const 값 및 r-value로 초기화 할 수 있습니다.

```cpp
int x = 5;
const int &ref1 = x;

const int y = 6;
const int &ref2 = y;

const int &ref3 = 6;
```

상수를 가리키는 포인터처럼 const 값에 대한 참조는 const가 아닌 값을 참조할 수 있습니다. 이때 참조 접근을 하면 const가 아니더라도 const로 간주합니다.

```cpp
int	value = 5;
const int	&ref = value;

value = 6; //true
ref = 7; // false, ref is const
```

const에 대한 참조는 간단히 상수 참조라고 합니다.

r-value는 값이 생성된 표현식 끝에서 소멸하지만 const에 대한 참조가 r-value로 초기화된다면 r-value의 수명이 참조형 변수의 수명에 맞게 확장됩니다.

함수의 매개변수로 사용되는 참조는 const일 수 있습니다. 이렇게 하면 복사본을 만들지 않고 인수에 접근하면서도 함수는 참조된 값을 변경하지 못합니다.

```cpp
void changeN(const int &ref)
{
	ref = 6; //not allowed, ref is const
}
```

## ex05

const를 어떤 방식으로 사용하는지 익힐 수 있습니다.

```const std::string identify(void) const;```
와 같은 함수의 경우, 해당 메서드 내부에서는 어떠한 변수도 바꿀 수 없고, const가 아닌 메서드를 부를 수 없게 됩니다.


## ex06

포인터와 레퍼런스의 차이점 및 사용 방식
main함수의 코드를 보고 포인터를 사용하는 Human 클래스와 레퍼런스를 사용하는 Human 클래스를 구현합니다.

## ex07

sed동작과 유사한 기능을 하는 프로그램 작성
