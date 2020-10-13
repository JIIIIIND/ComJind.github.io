---
title: "Do It! 코틀린 프로그래밍 챕터02"
date: 2020-10-13 23:22:11 +0900
categories: Kotlin
---

## 코틀린 패키지

코틀린에서 프로젝트는 모듈, 패키지, 파일로 구성

### 코틀린 프로젝트, 모듈, 패키지, 파일의 관계 이해하기

프로젝트에 모듈이 있고, 모듈은 패키지로 구성되고, 패키지는 파일로 구성
대규모 프로젝트에서는 기능을 모듈로 분리하여 관리함

코틀린 파일은 .kt확장자를 가지며, 파일의 맨 위에 어떤 패키지에 포함된 것인지 컴파일러가 알 수 있도록 패키지 이름을 선언해야 함. 선언하지 않는 경우엔 default패키지에 포함

**패키지를 만들어야 하는 이유**

2명 이상의 프로그래머가 작업을 하는 경우에 우연히 같은 이름의 파일을 생성하는 경우 오류가 발생함. 하지만 서로 다른 패키지에 있다면 같은 이름의 파일(클래스)가 있어도 오류가 발생하지 않음

### 코틀린 프로젝트에 패키지 만들기

**패키지 이름 정하기**

패키지 이름은 파일 맨 위에 적으면 됨. 이때 이름 앞에 package라는 키워드를 입력
패키지 이름은 특수문자나 숫자로 시작할 수 없고 여러 단계의 분류가 필요하면 점을 붙여 이름을 지을 수 있음

탐색기로 살펴보면 패키지 이름 사이의 .을 기준으로 하위 디렉토리가 생성되어 있는 것을 확인할 수 있음

### 기본 패키지 활용

기본 패키지란 코틀린으로 프로그램을 만들 때 자주 사용하는 클래스와 함수 등을 미리 만들어 놓은 것
import를 하지 않아도 사용 가능함

| 패키지 이름 | 설명 |
|---|:---:|
| kotlin.* | Any, Int, Double 등 핵심 함수와 자료형 |
| kotlin.text.* | 문자와 관련된 API |
| kotlin.sequences.* | 컬렉션 자료형의 하나로 반복이 허용되는 개체를 열거 |
| kotlin.ranges.* | if문이나 for문에서 사용할 범위 관련 요소 |
| kotlin.io.* | 입출력 관련 API |
| kotlin.collections.* | List, Set, Map 등의 컬렉션 |
| kotlin.annotation.* | 애노테이션 관련 API |

### 사용자 클래스 가져오기

직접 만든 사용자 클래스를 다른 패키지에서 사용하는 경우에도 패키지의 이름과 함께 패키지의 요소를 import 키워드와 함께 적으면 됨
```kotlin
package chap02.section1

import com.example.edu.Person

fun main() {
	val user1 = Person("kildong", 30)

	println(user1.name)
	println(user1.age)
}
```
만약 chap02.section1 패키지에도 같은 이름의 클래스가 있다면 as라는 키워드로 클래스 이름에 별명을 붙여 사용하면 됨
```kotlin
package chap02.section1

import com.example.edu.Person as User

fun main() {
	val user1 = User("kildong", 30)
	val user2 = Person("A123", "kildong")

	println(user1.name)
	println(user1.age)

	println(user2.id)
	println(user2.age)
}

class Person(val id: String, val name: String)
```

## 변수와 자료형

### 변수를 선언하고 자료형 추론하기

변수는 val, var 키워드를 사용하여 선언할 수 있음
- val
	- 최초로 지정한 변수의 값으로 초기화하고 더 이상 바꿀 수 없는 읽기 전용 변수가 됨
- var
	- 초깃값이 있더라도 값을 바꿀 수 있음

**변수의 선언**

val(선언 키워드) username(변수 이름): String(자료형) = "kildong"(값)
여기서 자료형을 따로 지정하지 않고 값을 통해 자료형을 지정할 수 있음. 이것을 '자료형을 추론한다'고 함
단, 자료형을 지정하지 않은 변수는 반드시 자료형을 추론할 값을 지정해야 함

**변수 이름 규칙**

- 변수 이름은 123abc처럼 숫자로 시작할 수 없음
- 변수 이름에는 while, if와 같이 코틀린에서 사용되는 키워드는 쓸 수 없음
- 변수 이름은 의미 있는 단어를 사용하여 만드는 것이 좋음
- 여러 단어를 사용하여 변수 이름ㅇ르 지을 때 카멜 표기법을 따르는 것이 좋음

여기서 카멜 표기법이란 첫 번째 글자는 소문자로 쓰고 나머지 각 단어의 첫 글자는 대문자로 쓰는 방식을 말함. 보통 변수의 첫 글자는 소문자로, 클래스나 인터페이스의 첫 글자는 대문자로 표기

### 자료형 알아보기

**코틀린의 자료형은 참조형 자료형을 사용**

기본형(Primitive Data Type)은 가공되지 않은 순수한 자료형을 말하며 프로그래밍 언어에 내장되어 있음
참조형( Reference Type)은 객체를 생성하고 동적 메모리 영역에 데이터를 둔 다음 이것을 참조하는 자료형을 말함
자바에서는 int, long, float, double등 기본형과 String, Data와 같은 참조형을 모두 사용하지만 코틀린에서는 참조형만을 사용
참조형으로 선언한 변수는 성능 최적화를 위해 코틀린 컴파일러에서 다시 기본형으로 대체됨

**기본형과 참조형의 동작 원리**

```java
int a = 77; //기본형
Person person = new Person(); //참조형으로 person객체를 위해 참조 주소를 가짐
```

기본형인 a는 주로 임시 메모리인 스택에 저장되며 값이 저장된 메모리의 크기도 고정됨
참조형은 스택에 값이 아닌 참조 주소가 있음. 실제 객체는 동적 메모리인 힙에 저장됨

**정수 자료형**

코틀린의 정수 자료형은 부호가 있는 것과 부호가 없는 것으로 나눌 수 있음
| 자료형 | 크기 | 값의 범위 |
|---|:---:|:---:|
| Long | 8byte(64bit) | -2^63 ~ 2^63 - 1 |
| Int | 4byte(32bit) | -2^31 ~ 2^31 - 1 |
| Short | 2byte(16bit) | -2^15 ~ 2^15 - 1 |
| Byte | 1byte(8bit) | -2^7 ~ 2^7 - 1 |

숫자를 그냥 사용하면 10진수를 나타내지만 접미사나 접두사를 사용하면 2진수나 16진수를 표현 할 수 있음

```kotlin
val exp01 = 123 // int
val exp02 = 123L // long
val exp03 = 0x0F // 접두사 0x를 사용해 16진 표기된 Int
val exp04 = 0b00001011 // 접두사 0b를 사용해 2진 표기된 Int
```

Byte형이나 Short형을 사용하려면 직접 자료형을 명시해야 함

부호가 없는 자료형의 경우 앞에 U를 붙이면 되고, 자료형을 명시해야 값을 할당할 수 있음

**언더스코어로 자릿값을 구분할 수 있음**

1_000_000, 1234_1234_1234L과 같이 언더스코어를 사용해서 값을 표현할 수 있음
언더스코어는 값에 영향을 주지 않기 때문에 원하는 위치 아무 곳에나 넣을 수 있음

**실수 자료형**

말 그대로 실수를 저장하는 데 사용함

| 자료형 | 크기 | 값의 범위 |
|---|:---:|:---:|
| Double | 8byte(64bit) | 약 4.9E - 324 ~ 1.7E + 308 |
| Float | 4byte(32bit) | 약 1.4E - 45 ~ 3.4E + 38 |

정수와 마찬가지로 자료형을 명시하지 않으면 Double형으로 추론함
만약 Float를 사용하고 싶다면 식별자 F를 실수 옆에 붙이면 됨

부동 소수점 방식을 사용함
코틀린에서 소수점의 이동은 숫자 오른쪽에 e나 E와 함께 밑수인 10을 제외하고 지수만 적으면 됨

IEEE 754표준에서는 공간의 제약으로 모든 실수를 표현할 수 없고, 표현 비트의 제한 때문에 약간의 오차가 있으므로 사용에 주의해야 함

**정수 자료형과 실수 자료형의 최솟값과 최댓값**

```kotlin
package chap02.section2

fun main() {
	println("Byte min: " + Byte.MIN_VALUE + " max: " + Byte.MAX_VALUE)
	println("Short min: " + Short.MIN_VALUE + " max: " + Short.MAX_VALUE)
	println("Int min: " + Int.MIN_VALUE + " max: " + Int.MAX_VALUE)
	println("Long min: " + Long.MIN_VALUE + " max: " + Long.MAX_VALUE)
	println("Float min: " + Float.MIN_VALUE + " max: " + Float.MAX_VALUE)
	println("Double min: " + DoubleMIN_VALUE + " max: " + DoubleAX_VALUE)
}
```

코틀린은 코드를 작성할 때 각 자료형에서 표현할 수 있는 최댓값이나 최솟값을 넘는 값을 할당하면 오류를 발생시킴. 단, 비트 연산의 경우 오류를 미리 발생시키지 않기에 2의 보수 표현에 의해 의도하지 않은 값으로 바뀔 수 있음

**논리 자료형**

| 자료형 | 크기 | 값의 범위 |
|---|:---:|:---:|
| Boolean | 1bit | true, false |

보통 검사 용도의 변수를 만들 때 사용함

**문자 자료형**

| 자료형 | 크기 | 값의 범위 |
|---|:---:|:---:|
| Char | 2Byte(16bit) | 0 ~ 2^15 - 1 |

문자를 표현하기 위해 사용하며 작은 따옴표로 감싸 표현함
숫자를 사용하여 선언하는 것은 금지되기 대문에 정수값을 변환하는 함수 toChar()를 이용하여 문자 자료형을 선언

### 문자열 자료형

문자열 자료형은 기본형에 속하지 않는 배열 형태로 되어 있는 특수한 자료형

**문자열 자료형 선언과 저장 방식 이해하기**

문자열 자료형도 자료형의 이름을 지정하거나 추론 방식을 ㅗ선언할 수 있음

```kotlin
package chap02.section2

fun main() {
	var str1: String = "Hello"
	var str2 = "World"
	var str3 = "Hello"
}
```

위의 예제에서 str1과 str3은 같은 문자열이 저장됨
Hello를 스택에 2번 저장하는 것보다 이미 저장된 값을 활용하는 것이 효율적이기 때문
코틀린은 힙 영역의 String pool이라는 공간에 문자열인 "Hello"를 저장해두고 이 값을 str1, str3이 참조

**표현식과 $기호 사용하여 문자열 출력**

변수의 값이나 표현식을 문자열 안에 넣어 출력하기 위해 $를 사용함

```kotlin
var a = 1
val s1 = "a is $a" //String 자료형의 s1을 선언하고 초기화, 변수 a를 사용
```

$a가 변수 a의 값은 1로 대체되어 "a is 1"이라는 문자열이 s1에 할당 됨
만약 변수가 아니라 표현식을 문자열에 포함시키려면 중괄호를 사용
```kotlin
package chap02.section2

fun main() {
	var a = 1
	val str1 = "a = $a"
	val str2 = "a = ${a + 2}"

	println("str1: \"$str1\", str2: \"$str2\"")
}
```

**형식화된 다중 문자열 사용**

문자열에 줄바꿈, 탭 등의 특수문자가 포함되는 경우 """을 사용함
그리고 이러한 문자열을 형식화된 다중 문자열이라고 부름
```kotlin
package chap02.section2

fun main() {
	val num = 10
	val formattedString = """
		var a = 6
		var b = "Kotlin"
		println(a + num) //num의 값은 $num
		"""
	println(formattedString)
}
```

위의 결과는 """으로 감싸진 영역의 모든 내용이 그대로 출력 됨

**자료형에 별명 붙이기**

변수의 자료형이 복잡한 구조를 가지면 자료형에 별명을 붙일 수도 있음
typealias라는 키워드를 사용하며 고차 함수와 람다식에서 많이 사용
```kotlin
typealias Username = String //String을 Username이라는 별명으로 대체
val user: Username = "kildong" //Username과 String은 같은 표현
```