---
title: "Do It! 코틀린 프로그래밍 챕터02-02"
date: 2020-10-15 02:00:11 +0900
categories: Kotlin
---

## 자료형 검사하고 변환하기

코틀린에서는 변수를 사용할 때 반드시 값이 할당되어 있어야 한다는 원칙이 있습니다.
값이 할당되지 않은 변수를 사용하면 오류가 발생합니다.

코틀린에서 값이 없는 상태는 null이라고 하며 null상태인 변수를 허용하려면 ? 기호를 사용해 선언해야 합니다.

### null을 허용한 변수 검사

**변수에 null 할당하기**

```kotlin
package chap02.section3

fun main() {
	var str1 : String = "Hello kotlin"
	str1 = null
	println("str1: $str1")
}
```
위의 코드는 Null can not be a value of a non-null type String라는 메시지를 출력합니다.
변수에 null 할당을 허용하려면 자료형 뒤에 ? 기호를 명시해야 합니다.

```kotlin
package chap02.section3

fun main() {
	var str1 : String? = "Hello kotlin"
	str1 = null
	println("str1: $str1")
}
```

**세이프 콜과 non-null 단정 기호를 활용하여 null을 허용한 변수 사용하기**

프로그램 실행 도중 문자열의 길이를 구하기 위해 str1.length를 사용하는 경우에 str1이 null이라면 NPE가 발생합니다.

```kotlin
package chap02.section3

fun main() {
	var str1 : String? = "Hello kotlin"
	str1 = null
	println("str1: $str1 length: ${str1.length}")
}
```
해당 코드를 작성하고 나면 str1.length부분에 String?형에서는 세이프 콜(?.)이나 non-null 딘정 기호(!!.)만 허용한다는 팁이 나타납니다.

세이프 콜이란 null이 할당되어 있을 가능성이 있는 변수를 검사하여 안전하게 호출하도록 도와주는 기법입니다. 호출할 변수 이름 뒤에 ?.를 작성하면 됩니다.
```kotlin
println("str1: $str1 length: ${str1?.length}")
```
str1을 검사한 다음 null이 아니면 str1의 멤버 변수인 length에 접근해 값을 읽도록 만든 것입니다.
만약 null이라면 그대로 null을 출력합니다.

만약 non-null 단정 기호를 사용한다면 변수에 할당된 값이 null이 아님을 단정하므로 컴파일러가 null 검사 없이 무시합니다. 따라서 변수에 null이 할당되어 있어도 컴파일은 잘 진행되고 실행 중에 NPE를 발생시킵니다.

**조건문을 활용해 null을 활용한 변수 검사**

조건문으로 null을 허용한 변수를 검사할 수 있습니다.
즉, null을 허용한 변수의 null 상태 가능성을 검사하기만 하면 코틀린 컴파일러는 오류를 발생시키지 않습니다.
```kotlin
fun main() {
	var str1: String? = "Hello Kotlin!"
	str1 = null
	val len = if(str1 != null) str1.length else -1
	println("str1: $str1 length: ${len}")
}
```
str1이 null이면 -1, 아니면 길이가 출력됩니다.

**세이프 콜과 엘비스 연산자를 활용해 null을 허용한 변수 더 안전하게 사용**

null을 허용한 변수를 조금 더 안전하게 사용하려면 세이프 콜 ?.과 엘비스 연산자 ?:를 함께 사용합니다.
엘비스 연산자는 변수가 null인지 아닌지 검사하여 null이 아니라면 왼쪽 식을 그대로 사용하고 null이라면 오른쪽 식을 실행합니다.
```kotlin
package chap02.section3

fun main() {
	var str1 : String = "Hello kotlin"
	str1 = null
	println("str1: $str1 length: ${str1?.length ?: -1}")
}
```

str1?.length ?: -1이라는 표현은 다음 코드와 동일합니다.
```kotlin
if (str1 != null) str1.length else -1
```

세이프 콜과 엘비스 연산자를 사용하면 null인 경우 반환값을 01과 같은 특정 값으로 대체함으로써 null발생을 대비할 수 있으므로 안전하고, 코드를 한 줄에 표현할 수 있어 가독성이 좋아집니다.

### 자료형 비교하고 검사하고 변환하기

앞에서 설명한 것처럼 코틀린의 자료형은 모두 참조형으로 선언됩니다.
하지만 컴파일을 거쳐서 최적화될 때 Int, Long, Short와 같은 자료형은 기본형 자료형으로 변환됩니다.
참조형과 기본형의 저장 방식은 서로 다르기 때문에 자료형을 비교하거나 검사할 때는 이와 같은 특징을 이해하고 있어야 합니다.
코틀린에서는 자료형이 서로 다른 변수를 비교하거나 연산할 수 없습니다. 같은 자료형으로 만들어야 비교를 하거나 연산을 진행할 수 있습니다.

**자료형 변환**

코틀린에서는 자료형이 다르면 변환 함수를 사용해야 합니다. 의도치 않게 자료형이 변하는 것을 방지하기 위해 자료형이 다른 변수에 재할당하면 자동 형 변환이 되지 않고 자료형 불일치 오류가 발생합니다.
```kotlin
val a: Int = 1 //정상
val b: Double = a //자료형 불일치
val c: Int = 1.1 //자료형 불일치
```
위의 코드를 정상적으로 동작하게 하려면 a에 명시적으로 Double형으로 변환하는 toDouble()메서드를 점과 함께 붙여 사용합니다.

```kotlin
val b: Double = a.toDouble()
```

만약 표현식에서 자료형이 서로 다른 값을 연산하면 자료형이 표현할 수 있는 범위가 큰 자료형으로 자동 형 변환하여 연산합니다.

```kotlin
val result = 1L + 3 //Long + Int의 형태이므로 result는 Long
```

**기본형과 참조형 자료형의 비교 원리**

자료형을 비교할 때는 단순히 값만 비교하는 방법과 참조 주소까지 비교하는 방법이 있습니다. 단순히 값ㅁ반 비교할 때는 이중 등호를, 참조 주소를 비교하려면 삼중 등호를 사용합니다.

이중 등호는 참조에 상관 없이 값이 동일하면 true를, 값이 다르면 false를 반환합니다. 삼중 등호는 값과 상관없이 참조가 동일하면 true를 반환합니다. 값이 동일하더라도 참조 주소가 다르면 false를 반환합니다.

참조형으로 선언된 a와 b는 코틀린 컴파일러가 기본형으로 변환하여 저장한다는 점에 주의해야 합니다. null을 허용한 변수는 같은 값을 저장해도 이중 등호화 삼중 등호를 사용한 결과값이 다릅니다.
```kotlin
val a: Int = 128
val b: Int? = 128
println(a == b)	//true
println(a == b)	//false
```
Int형으로 선언된 a는 기본형으로 변환되어 스택에 128이라는 값 자체를 저장하고, Int?형으로 선언된 b는 참조형으로 저장되므로 b에는 128이 저장된 힙의 참조 주소가 저장되어 있습니다.
```kotlin
package chap02.section3

fun main() {
	val a: Int = 128
	val b = a
	println(a === b)		//자료형이 기본형인 Int형이 되어 값이 동일하므로 true

	val c: Int? = a
	val d: Int? = a
	val e: Int? = c

	println(c == d)		//값의 내용만 비교하므로 true
	println(c === d)	//값의 내용은 같지만 참조 주소를 비교해 다른 객체이므로 false
	println(c === e)	//값과 참조 주소가 동일하므로 true
}
```

**저장되는 값이 128보다 작으면 그 값은 캐시에 저장되어 참조됩니다**

참조형으로 선언한 변수의 값이 -128~127범위에 있으면 캐시에 그 값을 저장하고 변수는 캐시의 주소를 가리킵니다. 이렇게 하면 더 좋은 성능의 프로그램을 만들 수 있기 때문입니다. 위의 예제에서 128이 아니라 -128~127사이의 값으로 변경하면 c와 d의 참조 주소 값이 같아집니다.

**스마트 캐스트 알아보기**

만약 어떤 값이 정수일 수도 있고 실수일 수도 있다면 그때마다 자료형을 변환해도 되지만 컴파일러가 자동으로 형 변환을 하는 스마트 캐스트를 사용하는 것이 더 편리합니다. 대표적으로 스마트 캐스트가 적용되는 자료형은 Number형이 있습니다. Number형을 사용하면 숫자를 저장하기 위한 특수한 자료형 객체를 만듭니다. Number형으로 정의된 변수에는 저장되는 값에 따라 정수형이나 실수형 등으로 자료형이 변환됩니다.
```kotlin
package chap02.section3

fun main() {
	var test: Number = 12.2	//12.2에 의해 test는 Float형으로 스마트 캐스트
	println("$test")

	test = 12				//Int형으로 스마트 캐스트
	println("$test")

	test = 120L				//Long형으로 스마트 캐스트
	println("$test")

	test += 12.0f			//Float형으로 스마트 캐스트
	println("$test")
}
```

**자료형 검사하기**

변수의 자료형을 알아내기 위해서는 is 키워드를 사용하면 됩니다. is는 왼쪽 항의 변수가 오른쪽 항의 자료형과 같으면 true를, 아니면 false를 반환합니다.
```kotlin
package chap02.section3

fun main() {
	val num = 256

	if (num is Int) {			//num이 int형일 때
		print(num)
	} else if (num !is Int) {	//num이 int형이 아닐 때, !(num is Int)와 동일
		print("Not a Int")
	}
}
```

is는 변수의 자료형을 검사한 다음 그 변수를 해당 자료형으로 변환하는 기능도 있습니다. 나중에 나올 Any형을 사용하면 자료형을 결정하지 않은 채로 변수를 선언할 수 있습니다. Any형은 코틀린의 최상위 기본 클래스로 어떤 자료형이라도 될 수 있는 특수한 자료형입니다. 이때 is를 사용하여 자료형을 검사하면 검사한 자료형으로 스마트 캐스트됩니다.
```kotlin
val x: Any
x = "Hello"
if (x is String) {
	print(x.length)
}
```
변수 x는 Any형으로 선언되었습니다. 그 다음 "Hello"라는 값을 대입합니다. 아직 x의 자료형은 Any형입니다. 이후 if문에서 is로 x의 자료형을 검사할 때 String으로 스마트 캐스트되어 조건문의 블록을 실행합니다.

**as에 의한 스마트 캐스트**

as로 스마트 캐스트할 수도 있습니다. as는 형 변환이 가능하지 않으면 예외를 발생시킵니다.
```kotlin
val x: String = y as String
```

위의 y가 null이 아니면 String으로 형 변환되어 x에 할당됩니다. y가 null이면 형 변환을 할 수 없으므로 예외가 발생합니다. null 가능성까지 고려하여 예외 발생을 피하려면 다음과 같이 ?기호를 사용할 수 있습니다.
```kotlin
val x: String? = y as? String
```

**묵시적 변환**

Any형은 자료형이 특별히 정해지지 않은 경우에 사용합니다. 코틀린의 Any형은 모든 클래스의 뿌리입니다. Int나 String 그리고 사용자가 직접 만든 클래스까지 모두 Any형의 자식 클래스입니다. 즉, 코틀린의 모든 클래스는 바로 이 Any형이라는 슈퍼클래스를 가집니다.

Any형은 언제든 필요한 자료형으로 자동 변환할 수 있습니다. 이것을 묵시적 변환이라고 합니다.
```kotlin
package chap02.section3

fun main() {
	checkArg("Hello")
	checkArg(5)
}

fun checkArg(x: Any) {
	if (x is String) {
		println("x is String: $x")
	}
	if (x is Int) {
		println("x is Int: $x")
	}
}
```
is연산자를 이용하여 인자로 전달받은 값을 검사하며 자료형을 Any에서 검사한 자료형으로 변환하고 있음을 알 수 있습니다. 실무에서 자주 사용하니 꼭 알아두라고 합니다.

## 코틀린 연산자

### 기본 연산자

기본 연산자에는 산술, 대입, 증가, 감소, 비교, 논리 연산자 등이 있습니다.

**수식의 구조**

연산자는 항의 개수에 따라서 단항 연산자, 이항 연산자, 삼항 연산자로 구분합니다.

**산술 연산자**

사칙 연산자(+, -, *, /)와 나머지 연산자(%)를 산술 연산자라고 부릅니다.
| 연산자 | 의미 | 사용 예 |
|---|:---:|:---:|
| + | 덧셈 | 3 + 2 |
| - | 뺄셈 | 3 - 2 |
| * | 곱셈 | 3 * 2 |
| / | 나눗셈 | 3 / 2 |
| % | 나머지(Modulus) | 3 % 2 |

**대입연산자**

변수에 값을 할당하는 연산자이며 이항 연산자 중 우선순위가 가장 낮습니다. 쉽게 말해서 다른 연산자의 연산이 모두 끝나면 그때 대입 연산자가 동작합니다. 오른쪽에는 값이나 값이 있는 변수, 표현식 등을 사용합니다. 

어떤 변수에 저장된 값으로 연산을 수행한 다음 그 결괏값을 다시 변수에 할당할 때 산술 연산자와 대입 연산자를 함께 사용하여 간결하게 표현할 수도 있습니다.
```kotlin
num = num + 2
num += 2
```

| 연산자 | 의미 | 사용 예 |
|---|:---:|:---:|
| = | 오른쪽 항의 내용을 왼쪽 항에 대입 | num = 2 |
| += | 두 항을 더한 후 왼쪽 항에 대입 | num += 2 |
| -= | 왼쪽 항을 오른쪽 항으로 뺀 후 왼쪽 항에 대입 | num -= 2 |
| *= | 두 항을 곱한 후 왼쪽 항에 대입 | num *= 2 |
| /= | 왼쪽 항을 오른쪽 항으로 나눈 후 왼쪽 항에 대입 | num /= 2 |
| %= | 왼쪽 항을 오른쪽 항으로 나머지 연산 후 왼쪽 항에 대입 | num %= 2 |

**증가 연산자와 감소 연산자**

단항 연산자로 항의 앞이나 뒤에 붙여 사용하며 이름 그대로 1을 더하거나 빼는 연산을 수행합니다.

| 연산자 | 의미 | 사용 예 |
|---|:---:|:---:|
| ++ | 항의 값에 1 증가 | ++num 또는 num++ |
| -- | 항의 값에 1 감소 | --num 또는 num-- |

**비교 연산자**

2개의 항을 비교하기 위해 사용합니다. 모든 비교 연산자는 비교 결과가 참이면 true를, 거짓이면 false를 반환합니다.

| 연산자 | 의미 | 사용 예 |
|---|:---:|:---:|
| > | 왼쪽이 크면 true, 작으면 false | num > 2 |
| < | 오른쪽이 크면 true, 작으면 false | num < 2 |
| >= | 왼쪽이 크거나 같으면 true, 아니면 false | num >= 2 |
| <= | 왼쪽이 작거나 같으면 true, 아니면 false | num <= 2 |
| == | 두 항의 값이 같으면 true, 아니면 false | num1 == num2 |
| != | 2개 항의 값이 다르면 true, 아니면 false | num1 != num2 |
| === | 2개 항의 참조 주소가 같으면 true, 아니면 false | num1 === num2 |
| !== | 2개 항의 참조 주소가 다르면 true, 아니면 false | num1 !== num2 |

**논리 연산자**

| 연산자 | 의미 | 사용 예 |
|---|:---:|:---:|
| && | 논리곱으로 2개 항이 모두 true일 때 true, 아니면 false | exp1 && exp2 |
| \|\| | 논리합으로 2개 항 중 1개 항이 true일 때 true, 아니면 false | exp1 || exp2 |
| ! | 부정 연산자로 true를 false로, false를 true로 바꿈 | !exp |

논리합 연산자의 왼쪽 항이 true면 오른쪽 항을 실행하지 않습니다. 이것을 단축 평가(Short Circuit Evaluation)라고 합니다.

### 비트 연산자

비트 연산자는 0과 1을 처리하는 데 사용합니다. 보통 IoT기기를 위한 컨트롤러나 프로세서의 레지스터에 접근하는 임베디드 시스템 프로그래밍 분야에서 비트 연산을 많이 합니다.

**비트 연산을 위한 비트 메서드**

메서드처럼 사용해도 되지만 연산자처럼 사용할 수도 있습니다. 예를 들어 전체 비트를 왼쪽으로 이동시키는 shl() 메서드는 4.shl(1)또는 4 shl 1과 같은 방법으로 사용할 수 있습니다.

| 사용 예 | 설명 |
|---|:---:|
| 4.shl(bits) | 4를 표현하는 비트를 bits만큼 왼쪽으로 이동(부호 있음) |
| 7.shr(bits) | 7을 표현하는 비트를 bits만큼 오른쪽을 이동(부호 있음) |
| 12.ushr(bits) | 12를 표현하는 비트를 bits만큼 오른쪽으로 이동(부호 없음) |
| 9.and(bits) | 9를 표현하는 비트와 bits를 표현하는 비트로 논리곱 연산 |
| 4.or(bits) | 4를 표현하는 비트와 bits를 표현하는 비트로 논리합 연산|
| 24.xor(bits) | 24를 표현하는 비트와 bits를 표현하는 비트의 배타적 연산 |
| 78.inv() | 78을 표현하는 비트를 모두 뒤집음 |

**비트 이동 연산자 shl, shr**

비트를 왼쪽이나 오른쪽으로 밀어낸 다음 사라진 비트의 값은 0으로 채우며 부호 비트는 그대로 둡니다.
비트 이동 연산자는 컴퓨터가 쉽게 다룰 수 있는 비트를 이용하여 연산하기 때문에 연산 속도가 아주 빠르다는 장점이 있습니다.
단, 아주 큰 값에 비트 이동 연산자를 사용할 때는 부호 비트에 주의해야 합니다.
```kotlin
package chap02.section4

fun main() {
	var x = 4
	var y = 0b0000_1010
	var z = 0x0F

	println("x shl 2 -> ${x shl 2}")	//16
	println("x.inv() -> ${x.inv()}")	//-5

	println("y shr 2 -> ${y/4}, ${y shr 2}")	2, 2
	println("x shl 4 -> ${x*16}, ${x shl 4}")	64, 64
	println("z shl 4 -> ${z*16}, ${z shl 4}")	240, 240

	x = 64
	println("x shr 4 -> ${x/4}, ${x shr 2}")	16, 16
}
```

**비트 이동 연산자 ushr**

부호 비트까지 포함하여 비트를 밀어냅니다. 음수인 경우 부호 비트가 1에서 0으로 바뀌기 때문에 조심해서 사용해야 합니다.

```kotlin
package chap02.section4

fun main() {
	val number1 = 5
	val number2 = -5

	println(number1 shr 1)
	println(number1 ushr 1)	//변화 없음
	println(number2 shr 1)	//부호 비트 1로 유지됨
	println(number2 ushr 1)	//부호 비트가 0이 되면서 변경 됨
}
```

음수는 2의 보수로 표현되기에 부호 비트가 0이 되며 큰 값이 출력 됨

**논리합 연산자 or**

or는 두 수의 비트를 일대일 대응으로 비교하여 비트의 값이 하나라도 1이면 1을 반환합니다.

```kotlin
package chap02.section4

fun main() {
	val number1 = 12
	val number2 = 25
	val result: Int

	result = number1 or number2
	println(result)
}
```

위의 결과는 29가 출력됨

**논리곱 연산자 and**

두 비트 값을 비교하여 둘 다 1이면 1을 반환합니다. 그 이외는 모두 0을 반환합니다.

**배타적합 연산자 xor**

두 비트 값을 비교하여 같으면 0을, 다르면 1을 반환합니다. or연산자와 반대의 기능을 한다고 보면 됩니다.

xor의 독특한 성질을 이용하여 2개의 변수 값을 바꿀 수도 있습니다. 이런 기법으로 swap기법이라고 합니다.
```kotlin
package chap02.section4

fun main() {
	var number1 = 12
	var number2 = 25

	number1 = number1 xor number2
	number2 = nubmer1 xor number2
	number1 = number1 xor number2
}
```

**반전 연산자 inv**

inv는 비트를 모두 반대로 뒤집습니다. 이 연산자는 단항 연산자이므로 중위 표현법을 사용하지 않고 메서드처럼 사용합니다.