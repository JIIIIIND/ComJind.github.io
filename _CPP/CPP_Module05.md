---
title: "CPP Module05"
date: 2020-09-24 18:25:11 +0900
categories: 42Seoul CPP
toc: true
toc_sticky: true
toc_label: "CPP Module05"
---

이번 모듈에서는 예외 처리 말고는 전부 앞에서 해본 것들이라 귀찮은걸 제외하면 크게 어렵지는 않습니다.

## 예외 처리

### 기존 방식

C언어에서의 예외 처리는 어떤 작업을 수행한 이후 결과 값을 비교하는 방식으로 처리를 했습니다.
여러 함수가 중첩된 상태라면 모든 함수에서 처리 작업을 해둬야 하기 때문에 상당히 귀찮아집니다.

### cpp 방식

throw문을 사용합니다.

vector의 요청한 위치에 있는 원소를 리턴하는 함수인 at 함수를 생각해봅시다.

```cpp
...
const T& at(size_t index) const {
	if (index >= size) {
		throw std::out_of_range("vector 의 index 가 범위를 초과했습니다.");
	}
	return data[index];
}
...
```

cpp에서는 예외를 던지고 싶을 때, throw로 예외로 전달하고 싶은 객체를 써주면 됩니다.
아무 객체나 던져도 상관 없지만 표준 라이브러리에는 이미 여러 종류의 예외가 정의되어 있습니다.
- out_of_range
- overflow_error
- length_error
- runtime_error 등등

throw 한 위치에서 함수는 즉시 종료되고, 예외 처리부까지 점프하게 됩니다. 따라서 throw 밑에 있는 모든 문장은 실행되지 않습니다.
이렇게 함수에서 예외 처리하는 부분에 도달하기 까지 함수를 빠져나가면서 stack에 생성되었던 객체들을 소멸시켜줍니다.

### 예외 처리하기

```cpp
#include <iostream>
#include <stdexcept>

template <typename T>
class Vector {
 public:
  Vector(size_t size) : size_(size) {
    data_ = new T[size_];
    for (int i = 0; i < size_; i++) {
      data_[i] = 3;
    }
  }
  const T& at(size_t index) const {
    if (index >= size_) {
      throw std::out_of_range("vector 의 index 가 범위를 초과하였습니다.");
    }
    return data_[index];
  }
  ~Vector() { delete[] data_; }

 private:
  T* data_;
  size_t size_;
};
int main() {
  Vector<int> vec(3);

  int index, data = 0;
  std::cin >> index;

  try {
    data = vec.at(index);
  } catch (std::out_of_range& e) {
    std::cout << "예외 발생 ! " << e.what() << std::endl;
  }
  // 예외가 발생하지 않았다면 3을 이 출력되고, 예외가 발생하였다면 원래 data 에
  // 들어가 있던 0 이 출력된다.
  std::cout << "읽은 데이터 : " << data << std::endl;
}
```

범위에 벗어난 값을 입력하면 범위를 초과했다는 메시지가 출력됩니다.

try문 내에서는 무언가 예외가 발생할만한 코드가 실행 됩니다. 예외가 발생하지 않는다면 try~catch문이 없는 것과 동일하게 동작하며, 예외가 발생하면 그 즉시 catch문으로 점프합니다.

점프하는 과정에서 스택 상에서 정의된 객체들을 소멸시키는 과정을 스택 풀기라고 합니다.
생성자에서 예외가 발생 할 때에는 소멸자가 호출되지 않기 때문에 만일 예외를 던지기 이전에 획득한 자원이 있다면 catch문에서 잘 해제시켜 줘야만 합니다.

#### 다양한 예외 처리

```cpp
#include <iostream>
#include <string>

int func(int c) {
  if (c == 1) {
    throw 10;
  } else if (c == 2) {
    throw std::string("hi!");
  } else if (c == 3) {
    throw 'a';
  } else if (c == 4) {
    throw "hello!";
  }
  return 0;
}

int main() {
  int c;
  std::cin >> c;

  try {
    func(c);
  } catch (char x) {
    std::cout << "Char : " << x << std::endl;
  } catch (int x) {
    std::cout << "Int : " << x << std::endl;
  } catch (std::string& s) {
    std::cout << "String : " << s << std::endl;
  } catch (const char* s) {
    std::cout << "String Literal : " << s << std::endl;
  }
}
```

위와 같이 여러 종류의 예외를 처리할 수도 있습니다.

#### 상속 관계에서의 예외 처리

```cpp
#include <exception>
#include <iostream>

class Parent : public std::exception {
 public:
  virtual const char* what() const { return "Parent!\n"; }
};

class Child : public Parent {
 public:
  const char* what() const { return "Child!\n"; }
};

int func(int c) {
  if (c == 1) {
    throw Parent();
  } else if (c == 2) {
    throw Child();
  }
  return 0;
}

int main() {
  int c;
  std::cin >> c;

  try {
    func(c);
  } catch (Parent& p) {
    std::cout << "Parent Catch!" << std::endl;
    std::cout << p.what();
  } catch (Child& c) {
    std::cout << "Child Catch!" << std::endl;
    std::cout << c.what();
  }
}
```

새로운 예외 클래스를 작성하기 위해서는 위의 Parent 클래스와 같이 std::exception을 상속 받고 virtual const char *what() const throw()를 구현합니다. what은 std::exception에 정의된 함수로, 이 예외가 무엇인지 설명하는 문자열을 리턴합니다.

또한 위의 예제를 실행시키면 Child를 받는 catch문이 아닌 Parent를 받는 catch문이 실행됩니다.
Parent &p = Child()가 가능하기 때문에 Parent catch가 먼저 처리하게 됩니다.
따라서 위와 같은 문제를 방지하기 위해 언제나 부모 클래스를 자식 클래스보다 뒤에 작성하는게 좋습니다.
Child &c = Parent()는 가능하지 않기에 같은 문제가 발생하지 않기 때문입니다.

#### 모든 종류의 예외 처리

특정 예외 객체를 받는게 아니라 모든 예외를 처리하고 싶다면 다음과 같이 사용합니다.

```cpp
try {
    func(c);
  } catch (int e) {
    std::cout << "Catch int : " << e << std::endl;
  } catch (...) {
    std::cout << "Default Catch!" << std::endl;
  }
```

만약 템플릿으로 정의되는 클래스의 경우 어떠한 방식의 템플릿이 인스턴스화 되냐에 따라 던지는 예외의 종류가 달라질 수 있습니다. 이 때문에 해당 객체의 catch 에서는 모든 예외 객체를 고려해야 합니다.

#### 예외를 발생시키지 않는 함수

```cpp
int foo() noexcept {}
```

위와 같이 함수 정의 옆에 noexcept를 붙이면 컴파일러에서 해당 함수는 예외를 발생시키지 않는다고 인식하고 컴파일을 합니다.

noexcept로 명시된 함수가 예외를 발생시킨다면 예외가 제대로 처리되지 않고 프로그램이 종료됩니다.

## ex00

관료 클래스 구현
등급이 너무 높거나 너무 낮을 때 예외를 발생시켜야 합니다.

## ex01

Form 클래스 구현
마찬가지로 등급이 너무 높거나 너무 낮을 때 예외를 발생시킵니다.

## ex02

Form을 상속받는 3가지 클래스 구현

- PresidentialPardonForm
	- 해당 Form은 그저 출력을 하기 때문에 신경 쓸 부분은 없습니다.
- RobotomyRequestForm
	- rand() 함수를 사용해야 합니다.
	- 좀 더 좋은 성능을 가진 mt19937의 경우 c++98에서 지원되지 않습니다.
- ShrubberyCreationForm
	- 문자를 사용해 나무를 그리고 출력하면 됩니다.

## ex03

3가지 Form을 반환하는 Intern 클래스 구현
여기서 중요한 점은 위의 Form을 판단하기 위해 if, else if, else if, else를 사용해서는 안된다는 점입니다.
