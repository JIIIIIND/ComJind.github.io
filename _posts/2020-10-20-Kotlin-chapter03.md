---
title: "Do It! 코틀린 프로그래밍 챕터02-02"
date: 2020-10-20 09:37:38 +0900
categories: Kotlin
---

## 함수 선언하고 호출하기

### 함수란 무엇일까

여러 값(인자)를 입력받아 기능을 수행하고 결괏값을 반환하는 코드의 모음

### 함수의 구조 자세히 살펴보기

```kotlin
package chap03.section1

fun sum(a: Int, b: Int): Int {
	var sum = a + b
	return sum
}
```

1. fun 키워드로 함수 선언
2. 함수 이름
	- 함수의 역할에 맞게 작성
3. 매개변수 정의
	- 콜론과 함께 자료형을 명시해야 함
	- 생략 가능함
4. 반환값의 자료형 명시
	- 생략 가능함
5. 함수 기능 작성
6. 값 반환
	- return 키워드 사용
	- 반환값이 없다면 생략 가능

**함수 간략화**

매개변수를 바로 반환값에 사용할 수 있다면 함수 안에서 별도의 변수를 정의하지 않고 다음과 같이 매개변수를 이용한 식을 바로 반환값에 입력해도 됨

```kotlin
fun sum(a: Int, b: Int): Int {
	return a + b
}
```

중괄호가 한 줄이면 중괄호와 return을 생략할 수 있는 대신에 대입 연산자를 사용함

```kotlin
fun sum(a: Int, b: Int): Int = a + b
```

더하는 값이 Int라면 반환 값도 Int이기에 반환값의 자료형을 생략할 수 있음

```kotlin
fun sum(a: Int, b: Int) = a + b
```

### 함수 호출과 프로그램의 실행 순서

```kotlin
package chap03.section1

fun sum(a: Int, b: Int): Int {
	var sum = a + b
	return sum
}

fun main() {
	val result1 =sum(3, 2)
	val result2 = sum(6, 7)

	println(result1)
	println(result2)
}
```

1. 진입점 main함수
2. 함수의 호출과 함수의 인자
	- sum 함수를 호출하며 인자 값을 전달함
3. sum()함수 호출
	- 프로그램의 실행이 main에서 sum으로 이동. sum이 끝나고 return되면 sum함수를 호출한 위치로 이동함

### 함수의 호출과 메모리

**함수와 스택 프레임**

함수의 각 정보는 프레임이라는 정보로 스택 메모리의 높은 주소부터 거꾸로 자라듯이 채워져 갑니다. 가장 아래쪽에는 main함수가 있으며 특정 함수를 호출하면 새로운 스택 프레임이 만들어집니다. 이후 함수가 종료되면 스택 프레임은 종료되며 main함수로 돌아갑니다.

**스택 프레임의 생성과 소멸**

스택 프레임은 각각 분리되어 있으므로 함수에 선언된 변수도 분리하여 생각합니다. 그래서 프레임으로 분리된 변수들을 지역 변수라고 부릅니다.

함수를 지속적으로 호출하여 스택 프레임이 정해진 스택 영역의 경계를 넘어서면서 힙 영역과 겹치는 것을 스택 오버 플로우 오류라고 합니다.

### 반환값이 없는 함수

반환값이 없는 함수는 자료형을 Unit으로 지정하면 됩니다.
void형과는 다르게 특수한 객체를 반환한다는 차이가 있습니다.

### 매개변수 제대로 활용하기

**매개변수의 기본값 기능**

```kotlin
fun add(name: String, email: String = "default") {
	//name과 email을 회원 목록에 저장
	//email의 기본값은 default. 즉, email로 넘어오는 값이 없다면 default로 지정됨
}
```

모든 매개변수의 기본값을 지정하면 인자를 전달하지 않고도 함수를 실행할 수 있음

**매개변수 이름과 함께 함수 호출**

```kotlin
package chap03.section1

fun main() {
	namedParam(x = 200, z = 100)
	namedParam(z = 150)
}

fun namedParam(x: Int = 100, y: Int = 200, z: Int) {
	println(x + y + z)
}
```

z의 경우 기본값이 설정되지 않았기에 반드시 인자를 전달해야 함

**매개변수의 개수가 고정되지 않은 함수 사용하기**

가변인자를 사용하는 방식으로 해결 가능
함수를 선언할 때 매개변수 왼쪽에 vararg라는 키워드를 붙이면 사용 가능합니다.

```kotlin
package chap03.section1

fun main() {
	normalVarargs(1, 2, 3, 4)
	normalVarargs(4, 5, 6)
}

fun normalVarargs(vararg counts: Int) {
	for (num in counts) {
		print("$num ")
	}
	print("\n")
}
```

가변인자 counts는 Int형 배열이 됩니다.

## 함수형 프로그래밍

코틀린은 함수형 프로그래밍과 객체 지향 프로그래밍을 모두 지원하는 다중 패러다임 언어입니다.

### 함수형 프로그래밍이란?

순수 함수를 작성하여 프로그램의 부작용을 줄이는 프로그래밍 기법을 말합니다.
함수형 프로그래밍에서는 람다식과 고차 함수를 사용합니다.

**순수함수**

어떤 함수가 같은 인자에 대해서 항상 같은 결과를 반환하면 '부작용이 없는 함수'라고 말합니다. 그리고 부작용이 없는 함수가 함수 외부의 어떤 상태도 바꾸지 않는다면 순수함수라고 부릅니다.

```kotlin
fun sum(a: Int, b: Int): Int {
	return a + b
}
```

다음은 순수 함수의 조건을 정리한 것입니다.
- 같은 인자에 대하여 항상 같은 값을 반환
- 함수 외부의 어떤 상태도 바꾸지 않는다

다음 함수는 순수함수가 아닌 경우의 예시입니다.

```kotlin
fun check() {
	val test = User.grade() //check()함수에 없는 외부의 User객체 사용
	if (test != null) process(test) //변수 test는 User.grade()의 실행 결과에 따라 달라짐
}
```

**람다식**

람다식은 람다 대수에서 유래한 것으로 다음과 같은 형태입니다.

```kotlin
{x, y -> x + y}
```

위의 식은 함수의 이름이 없고 화살표가 사용되었습니다. 수학에서 말하는 람다 대수는 이름이 없는 함수로 2개 이상의 입력을 1개의 출력으로 단순화한다는 개념입니다.

함수형 프로그래밍의 람다식은 다른 함수의 인자로 넘기는 함수, 함수의 결괏값으로 반환하는 함수, 변수에 저장하는 함수를 말합니다. 람다식은 나중에 자세히 다룹니다.

**일급 객체**

함수형 프로그래밍에서는 함수를 일급 객체로 생각합니다. 람다식 역시 일급 객체의 특징을 가지고 있는데 일급 객체의 특징은 다음과 같습니다.
- 일급 객체는 함수의 인자로 전달할 수 있다.
- 일급 객체는 함수의 반환값에 사용할 수 있다.
- 일급 객체는 변수에 담을 수 있다.

만약 함수가 일급 객체면 일급 함수라고 부릅니다. 그리고 일급 함수에 이름이 없는 경우 '람다식 함수' 혹은 '람다식'이라고 부를 수 있습니다.

**고차함수**

다른 함수를 인자로 사용하거나 함수를 결괏값으로 반환하는 함수를 말합니다. 두 특징을 모두 가지고 있어도 고차 함수라고 이야기합니다.

```kotlin
fun main() {
	println(highFunc({x. y -> x + y}, 10, 20))
}

fun highFunc(sum: (Int, Int) -> Int, a: Int, b: Int): Int = sum(a, b)
```

다음은 함수형 프로그래밍의 정의와 특징을 다시 한번 정리한 것입니다.
- 순수 함수를 사용해야 한다.
- 람다식을 사용할 수 있다.
- 고차 함수를 사용할 수 있다.

## 고차 함수와 람다식



## 고차 함수와 람다식의 사례 알아보기

## 코틀린의 다양한 함수 알아보기

## 함수와 변수의 범위