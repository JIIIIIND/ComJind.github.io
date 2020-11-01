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

### 고차 함수의 형태

인자나 반환값으로 함수를 사용합니다.

**일반 함수를 인자나 반환값으로 사용하는 고차 함수**

함수를 인자로 사용하는 고차 함수의 예시

```kotlin
package chap03.section3.funcargs

fun main() {
	val res1 = sum(3, 2)
	val res2 = mul(sum(3, 3), 3)

	println("ers1: $res1, res2: $res2")
}

fun sum(a: Int, b: Int) = a + b
fun mul(a: Int, b: Int) = a * b
```

함수를 반환값으로 사용하는 고차 함수의 예시

```kotlin
package chap03.section3.funcfunc

fun main() {
	println("funcFunc: ${funcFunc()}")
}

fun sum(a: Int, b: Int) = a + b

fun funcFunc(): Int {
	return sum(2, 2)
}
```

**람다식을 인자나 반환값으로 사용하는 고차 함수**

람다식을 변수에 할당하는 경우

```kotlin
package chap03.section3

fun main() {
	var result: Int
	val multi = {x: Int, y: Int -> x * y}
	result = multi(10, 20)
	println(result)
}
```

만약 함수 내용에 표현식이 2줄 이상이라면 마지막 표현식이 반환값이 됩니다. 예를 들면 다음과 같습니다.

```kotlin
val multi2: (Int, Int) -> Int = {x: Int, y: Int ->
	println("x * y")
	x * y
}
```

람다식의 매개변수에 자료형이 지정되어 있다면 변수의 자료형은 생략할 수 있습니다. 즉, 다음은 모두 같은 표현입니다.

```kotlin
val multi: (Int, Int) -> Int = {x: Int, y: Int -> x * y} //생략되지 않은 전체 표현
val multi = {x: Int, y: Int -> x * y} //선언 자료형 생략
val multi: (Int, Int) -> Int = {x, y -> x * y} //람다식 매개변수 자료형의 생략
```

하지만 둘 다 생략해 버리면 자료형이 추론되지 않으므로 오류가 발생합니다.

```kotlin
val multi = {x, y -> x * y} //추론이 불가능하므로 오류
```

반환 자료형이 아예 없거나 매개변수가 하나만 있을때의 표현 방법

```kotlin
val greet: () -> Unit = {println("Hello World!")}
val square: (Int) -> Int = {x -> x * x}
```

매개변수를 표현할 필요가 없으므로 화살표 앞쪽과 화살표 자체가 생략되었습니다. 즉, 람다식을 보고 매개변수와 반환값을 추론할 수 있다면 람다식의 매개변수 자료형은 생략할 수 있습니다.

람다식 안에 람다식을 넣는 경우

```kotlin
val nestedLambda: () -> () -> Ujnit = {{println("nested")}}
```

아무것도 없는 람다식 {}에 람다식 {println("nested")}를 넣었습니다. 그러면 자료형은 () -> () -> Unit으로 명시해야 합니다.
다음은 앞의 람다식에서 추론 가능한 부분을 생략한 예시입니다.

```kotlin
val greet = {println("Hello World!")} //추론 가능
val square = {x: Int -> x * x} //square의 자료형을 생략하려면 x의 자료형을 명시해야 함
val nestedLambda = {{println("nested")}} //추론 가능
```

람다식을 매개변수에 사용한 고차 함수의 예

```kotlin
package chap03.section3

fun main() {
	var result: Int
	result = highOrder({x, y -> x + y}, 10, 20) //람다식을 매개변수와 인자로 사용한 함수
	println(result)
}

fun highOrder(sum: (Int, Int) -> Int, a: Int, b: Int): Int {
	return sum(a, b)
}
```

마지막으로 인자와 반환값이 없는 람다식의 예시입니다.

```kotlin
package chap03.section3

fun main() {
	val out: () -> Unit = {println("Hello World!)} //인자와 반환값이 없는 람다식의 선언
	//자료형 추론이 가능하므로 val out = { println("Hello World!")}와 같이 생략 가능

	out() //함수처럼 사용 가능
	val new = out //람다식이 들어있는 변수를 다른 변수에 할당
	new()
}
```

람다식은 인자가 없거나 반환값이 없을 수 있습니다. 람다식 선언 부분의 화살표 왼쪽에는 비어 있는 형태의 인자를 사용하여 람다식의 인자가 없음을 표현하고 오른쪽에는 Unit을 사용하여 반환값이 Unit임을 표현했습니다.

람다식은 많은 코드를 간략화하고 함수 자체를 인자나 매개변수로 이용할 수 있어 프로그램의 효율성도 높일 수 있습니다.

### 람다식과 고차 함수 호출하기

자바나 코틀린은 함수를 호출할 때 인자의 값만 복사하는 '값에 의한 호출(call by value)'이 일반적입니다. c/c++에서 사용하는 포인터 주소 연산이 없기 때문에 주소 자체를 사용해 호출하는 '참조에 의한 호출(call by reference)'은 자바나 코틀린에서 사용되지 않습니다. 코틀린은 람다식을 사용하면서 몇 가지 확장된 호출 방법을 사용할 수 있습니다.

**값에 의한 호출**

코틀린에서 값에 의한 호출은 함수가 또 다른 함수의 인자로 전달될 경우 람다식 함수는 값으로 처리되어 그 즉시 함수가 수행된 후 값을 전달합니다.

```kotlin
package chap03.section3

fun main() {
	val result = callByValue(lambda()) //람다식 함수를 호출
	println(result)
}

fun callByValue(b: Boolean): Boolean { //일반 변수 자료형으로 선언된 매개변수
	println("callByValue function")
	return b
}

val lambda: () -> Boolean = {
	println("lambda function")
	true
}
```

위의 코드는 lambda function이 실행되고 반환값인 true가 callByValue의 인자가 되어 반환됩니다.

**이름에 의한 람다식 호출**

람다식의 이름이 인자로 전달될 때 실행되지 않고 실제로 호출할 때 실행되도록 하면 다음과 같습니다.

```kotlin
package chap03.section3

fun main() {
	val result = callByName(otherLambda) //람다식 이름으로 호출
	println(result)
}

fun callByName(b: () -> Boolean): Boolean { //람다식 자료형으로 선언된 매개변수
	println("callByName function")
	return b()
}

val otherLambda: () -> Boolean = {
	println("otherLambda funciton")
	true
}
```

앞의 코드와 거의 동일하지만 람다식의 이름을 callByName()함수에서 호출하는 점이 다릅니다.
람다식 자체가 매개변수에 복사되고 사용되기 전까지는 람다식이 실행되지 않기에 상황에 맞춰 즉시 실행할 필요가 없는 코드를 작성하는 경우 이름에 의한 호출 방법을 통해 필요할 때만 람다식이 작동하도록 만들 수 있습니다.

**다른 함수의 참조에 의한 일반 함수 호출**

람다식이 아닌 일반 함수를 또다른 함수의 인자에서 호출하는 고차 함수의 경우를 생각해 봅시다.

```kotlin
fun sum(x: Int, y: Int) = x + y
```

이것을 고차 함수인 funcParam()에서 호출하려고 합니다.

```kotlin
funcParam(3, 2, sum) //sum은 람다식이 아니기에 오류
...
fun funcParam(a: Int, b: Int, c: (Int, Int) -> Int): Int {
	return c(a, b)
}
```

sum()함수는 람다식이 아니므로 위와 같이 이름으로 호출할 수 없습니다. 하지만 sum()과 funcParam()의 매개변수 c의 선언부 구조를 보면 인자 수와 자료형의 개수가 동일합니다. 이때는 다음과 같이 2개의 콜론 기호를 함수 이름 앞에 사용해 소괄호와 인자를 생략하고 사용할 수 있습니다.

```kotlin
funcParam(3, 2, ::sum)
```

최종적으로 다음과 같이 사용할 수 있습니다.

```kotlin
package chap03.section3.funcref

fun main() {
	val res1 = funcParam(3, 2, ::sum)
	println(res1)

	hello(::text)

	val likeLambda = ::sum
	println(likeLambda(6, 6))
}

fun sum(a: Int, b: Int) = a + b

fun text(a: String, b: String) = "Hi! $a $b"

fun funcParam(a: Int, b: Int, c: (Int, Int) -> Int): Int {
	return c(a, b)
}

fun hello(body: (String, String) -> String): Unit {
	println(body("Hello", "World"))
}
```

콜론 2개를 이용한 표기법 정리

```kotlin
hello(::text) //함수 참조 기호
hello({a, b -> text(a, b) }) //람다식 표현(동일한 결과)
hello{a, b -> text(a, b)} //소괄호 생략(동일한 결과)
```

위의 3가지 표현은 모두 동일한 결과를 출력합니다. 따라서 매개변수와 인자 구조가 동일한 경우 람다식 표현법이 간략화된 함수 참조 기호인 ::을 사용하면 좀 더 편리하게 작성할 수 있습니다.

### 람다식의 매개변수

매개변수 개수에 따라 람다식을 구성하는 방법을 볼 것입니다. 매개변수와 인자 개수에 따라 람다식의 생략된 표현이 가능하기 때문에 코드를 더 간략화할 수 있습니다.

**람다식에 매개변수가 없는 경우**

매개변수가 없는 형태

```kotlin
package chap03.section3

fun main() {
	//매개변수 없는 람다식
	noParam({ "Hello World!" })
	noParam{ "Hello World"} //위와 동일한 결과, 소괄호 생략 가능
}
//매개변수가 없는 람다식이 noParam함수의 매개변수 out으로 지정됨
fun noParam(out: () -> String) = println(out())
```

noParam() 함수의 매개변수는 람다식 1개를 가지고 있는데 이때는 함수 사용 시 소괄호를 생략할 수 있습니다.
main()함수에서 사용된 noParam() 함수의 인자에는 람다식 표현식인 { "..." } 형태의 인자가 있습니다. 이 람다식에는 매개변수가 없으므로 화살표 기호가 사용되지 않았습니다. 여기서 소괄호는 생략할 수 있습니다. 매개변수는 없지만 반환 자료형은 문자열을 반환하고 있습니다. 따라서 println()에 의해 "Hello World"가 출력됩니다.

**람다식의 매개변수가 1개인 경우**

람다식에 매개변수가 1개 있을 경우에는 람다식에 화살표 기호 왼쪽에 필요한 변수를 써줘야 합니다.

```kotlin
...
fun main() {
	//매개변수 없는 람다식
...
	//매갭변수가 1개 있는 람다식
	oneParam({ a -> "Hello World! $a" })
	oneParam { a -> "Hello World! $a" } //위와 동일한 결과, 소괄호 생략 가능
	onePram { "Hello World! $it" } //위와 동일한 결과, it로 대체 가능
}
...
//매개변수가 1개 있는 람다식이 oneParam() 함수의 매개변수 out으로 지정됨
fun oneParam(out: (String) -> String) {
	println(out("OneParam"))
}
```

매개변수가 1개 들어간 람다식을 구성할 때 변수와 화살푤르 추가하여 a ->와 같이 나타냅니다. 이렇게 매개변수가 1개인 경우에는 화살표 표기를 생략하고 $it로 대체할 수 있습니다. $it은 람다식 매개변수로 지정된 String형과 매칭되어 "OneParam" 문자열로 바뀌며 최종적으로 "Hello World! OneParam"을 출력합니다. 따라서 다음 문장은 동일한 결괄르 보여줍니다.

```kotlin
oneParam({ a -> "Hello World! $a" })
oneParam P{ "Hello World! $it" }
```

**람다식의 매개변수가 2개 이상인 경우**

위의 예제에 이어서 매개변수가 2개 있는 람다식을 가지는 moreParam()함수를 만들어 보겠습니다.

```kotlin
...
fun main() {
	...
	//매개변수가 2개 있는 람다식
	moreParam { a, b -> "Hello World! $a $b" } //매개변수 이름 생략 불가
	...
}
//매개변수가 2개 있는 람다식의 moreParam함수의 매개변수로 지정됨
fun moreParam(out: (String, String) -> String) {
	println(out("OneParam", "TwoParm"))
}
```

moreParam()함수의 out에 정의된 대로 a는 매개변수의 첫 번째 String형을 위해 사용되고, b는 두 번째 String형을 위해 사용됩니다. 이때는 매개변수가 2개이므로 $it를 사용해 변수를 생략할 수 없습니다.

특정 람다식의 매개변수를 사용하고 싶지 않을때는 언더스코어(_)로 대체할 수 있습니다.

```kotlin
moreParam { _, b -> "Hello World! $b" } //첫번째 문자열은 사용하지 않고 생략
```

**일반 매개변수와 람다식 매개변수를 같이 사용하기**

일반적인 함수의 매개변수와 람다식 매개변수가 포함된 함수 형태의 예시입니다.

```kotlin
...
fun main() {
	...
	//인자와 함께 람다식을 사용하는 경우
	withArgs("Arg1", "Arg2", { a, b -> "Hello World! $a $b" })
	//withAargs()함수의 마지막 인자가 람다식인 경우 소괄호 바깥으로 분리 가능
	withArgs("Ar1", "Arg2") { a, b -> "Hello World! $a $b" }
}
...
//withArgs()함수는 일반 매개변수 2개를 포함, 람다식을 마지막 매개변수로 가짐
fun withArgs(a: String, b: String, out: (String, String) -> String) {
	println(out(a, b))
}
```

위의 예시처럼 마지막 인자가 람다식인 경우 소괄호를 생략할 수 있습니다.

**일반 함수에 람다식 매개변수를 2개 이상 사용하기**

이런 경우는 소괄호를 생략할 수 없습니다.

```kotlin
package chap03.section3

fun main() {
	twoLambda( { a, b -> "First $a $b" }, { "Second $it" } )
	twoLambda( { a, b -> "First $a $b" }) { "Second $it" } //위와 동일
}

fun twoLambda(first: (String, String) -> String, second: (String) -> String) {
	println(first("OneParam", "TwoParam"))
	println(second("OneParam"))
}
```

마지막 인자 값에 대해서는 소괄호를 생략할 수 있습니다.

```kotlin
({첫 번째}, {두 번째}, ...) {마지막}
```

## 고차 함수와 람다식의 사례 알아보기

### 동기화를 위한 코드 구현 구경하기

동기화란 변경이 일어나면 안 되는 특정 코드를 보호하기 위한 잠금 기법을 말합니다. 동기화로 보호되는 코드는 임계 영역이라고도 부릅니다.

다음은 자바에서 사용하는 Lock과 ReentrantLock의 기본적인 구성입니다.

```java
Lock lock = new Reentrantㅣock();
lock.lock(); //잠금
try {
	//보호할 임계 영역의 코드
	//수행할 작업
} finally {
	lock.unlock(); //해제
}
```

이 코드를 특정 함수를 보호하기 위한 고차 함수를 만들고 활용해 봅시다.

```kotlin
fun <T> lock(reLock: ReentrantLock, body: ()->T): T {
	reLock.lock()
	try {
		return body()
	} finally {
		reLock.unlock()
	}
}
```

이제 공유 자원에 접근하는 criticalFunc()라는 함수가 있다고 가정해 봅시다. 이 함수가 사용하는 공유 자원을 보호하기 위해 다음과 같이 활용할 수 있습니다.

```kotlin
package chap03.section4

import java.util.concurrent.locks.ReentrantLock

var sharable = 1 //보호가 필요한 공유 자원

fun main() {
	val reLock = ReentrantLock()

	lock(reLock, { criticalFunc() })
	lock(reLock, { criticalFunc() })
	lock(reLock, ::criticalFunc)

	println(sharable)
}

fun criticalFunc() {
	//공유 자원 접근 코드 사용
	sharable += 1
}

fun <T> lock(reLock: ReentrantLock, body: ()->T): T {
	reLock.lock()
	try {
		return body()
	} finally {
		reLock.unlock()
	}
}
```

해당 예제의 표현식은 모두 동일하지만 마지막의 함수 참ㅈ도로 호출하는 방법을 사용하는 것이 좋습니다.

### 네트워크 호출 구현 구경하기

네트워크로부터 무언가를 호출하고 성공하거나 실패했을 때 특정 콜백 함수를 처리하는 프로그램을 만든다고 가정해 봅시다.

자바에서 네트워크 전송 작업을 위해 성공과 실패에 대한 onSuccess(), onError()를 콜백 함수로 호출하는 인터페이스를 만든다고 가정합시다.

```java
public interface Callback {
	void onSuccess(ResultType result);
	void onError(Exception exception);
}

//networkCall 선언
public void networkCall (Callback callback) {
	try {
		//성공하면 onSuccess() 콜백 함수 호출
		callback.onSuccess(myResult);
	} catch(e: Throwable) {
		//실패하면 onError() 콜백 함수 호출
		callback.onError(e);
	}
}
...
//networkCall 사용 - 인자에서 인터페이스 구현을 익명 객체를 만들어 처리
networkCall(new Callback() {
	public void onSuccess(ResultType result) {
		//네트워크 호출에 성공했을 때의 구현부
	}
	public void onError(Exception e) {
		//네트워크 호출에 실패했을 때의 구현부
	}
})
```

이제 이것을 코틀린 설계로 바꿔 봅시다.

```kotlin
//코틀린으로 만든 네트워크 호출 구현부
// 람다식 함수의 매개변수를 가진 networkCall 함수 선언
fun networkCall(onSuccess: (ResultType) -> Unit, onError: (Throwable) -> Unit) {
	try {
		onSuccess(myResult)
	} catch (e: Throwable) {
		onError(e)
	}
}

...
//networkCall()함수 사용 - 인자 형식에 람다식을 사용
networkCall(result -> {
	//네트워크 호출에 성공했을 때 구현부
}, error -> {
	//네트워크 호출에 실패했을 때 구현부
});
```

자바에서 사용한 인터페이스나 익명 객체 없이 networkCall() 함수에서 바로 람다식 형태로 네트워크의 성공과 실패에 대한 내용을 구현할 수 있습니다.

## 코틀린의 다양한 함수 알아보기

### 익명 함수

익명 함수란 일반 함수이지만 이름이 없는 것입니다. 물론 람다식 함수도 이름 없이 구성할 수 있지만 이것은 일반 함수의 이름을 생략하고 사용하는 것입니다.

```kotlin
fun(x: Int, y: Int): Int = x + y //함수 이름이 생략된 익명 함수
```

이것은 변수 선언에 그대로 사용할 수 있습니다.

```kotlin
val add: (Int, Int) -> Int = fun(x, y) = x + y
val result = add(10, 2)
```

선언 자료형을 람다식 형태로 써 주면 변수 add는 람다식 함수처럼 add()와 같이 사용할 수 있는 것이죠. 만일 매개변수에 자료형을 써 주면 선언부에 자료형은 생략할 수 있습니다.

```kotlin
val add = fun(x: Int, y: Int) = x + y
```

이것은 람다식 표현법과 매우 유사하며 다음과 같이 동일하게 만들 수 있습니다.

```kotlin
val add = { x: Int, y: Int -> x + y }
```

람다식에서는 return, break, continue처럼 제어문을 사용하기 어렵기 때문에 익명 함수를 사용합니다.
물론 람다식으로 표현할 수 있는데도 일부러 익명 함수를 사용하면 코드가 읽기 어려우므로 적재적소에 맞게 사용할 것을 권장합니다.

### 인라인 함수

인라인 함수는 이 함수가 호출되는 곳에 함수 본문의 내용을 모두 복사해 넣어 함수의 분기 없이 처리되기 때문에 코드의 성능을 높일 수 있습니다.
인라인 함수는 람다식 매개변수를 가지고 있는 함수에서 동작합니다. 보통 함수는 호출되었을 때 다른 코드로 분기해야 하기 때문에 내부적으로 기존 내용을 저장했다가 다시 돌아올 때 복구하는 작업에 프로세스와 메모리를 꽤 사용해야 하는 비용이 듭니다.

```kotlin
package chap03.section5

fun main() {
	//인라인 함수 shortFunc()의 내용이 복사되어 shortFunc으로 들어감
	shortFunc(3) { println("First call: $it") }
	shortFunc(5) { println("Second call: $it") }
}

inline fun shortFunc(a: Int, out: (Int) -> Unit) {
	println("Before calling out()")
	out(a)
	println("After calling out()")
}
```

**인라인 함수 제한하기**

다음과 같은 인라인 함수를 살펴봅시다.

```kotlin
inline fun sub(out1: () -> Unit, out2: () -> Unit) {...}
```

위 코드의 경우 out1과 out2의 람다식이 그대로 복사되므로 코드의 양이 많아집니다. 그렇다면 일부 람다식을 인라인되지 않게 하기 위해서 어떻게 할까요? 다음과 같이 noinline 키워드를 사용하는 것입니다.

```kotlin
package chap03.section5.noinline

fun main() {
	shortFunc(3) { println("First call: $it") }
}

inline fun shortFunc(a: Int, noinline out: (Int) -> Unit) {
	println("Before calling out()")
	out(a)
	println("After calling out()")
}
```

**인라인 함수와 비지역 반환**

인라인 함수에서 사용한 람다식을 빠져나오려면 어떻게 해야 할까요? 인라인 함수에서 사용한 람다식에서는 return을 사용할 수 있습니다.

```kotlin
package chap03.section5.localreturn

fun main() {
	shortFunc(3) {
		println("First call: $it")
		return
	}
}

inline fun shortFunc(a: Int, out: (Int) -> Unit) {
	println("Before calling out()")
	out(a)
	println("After calling out()")
}
```

out(a)는 인라인되어 대체되기 때문에 return문까지 포함됩니다. 따라서 println("After calling out()") 문장은 실행되지 않습니다. 이러한 반환ㅇ르 비지역 반환이라고 부릅니다.

만일 shortFunc()가 inline 키워드로 선언되지 않으면 return문은 람다식 본문에 사용할 수 없으므로 return문을 허용할 수 없다는 오류가 납니다. 그 밖에 out()을 직접 호출해 사용하지 않고 또 다른 함수에 중첩하면 실행 문맥이 달라지므로 return을 사용할 수 없습니다. 이때 비지역 반환을 금지하는 방법이 있습니다.

```kotlin
package chap03.section5.crossinline

fun main() {
	shortFunc(3) {
		println("First call: $it")
		//return 사용 불가
	}
}

inline fun shortFunc(a: Int, crossinline out: (Int) -> Unit) {
	println("Before calling out()")
	nestedFunc { out(a) }
	println("After calling out()")
}

fun nestedFunc(body: () -> Unit) {
	body()
}
```

crossinline을 사용하면 람다식에서 return 문이 사용되었을 때 코드 작성 단계에서 오류를 보여줘 잘못된 비지역 반환을 방지할 수 있습니다.

### 확장 함수

기존 멤버 메서드는 아니지만 기존의 클래스에 내가 원하는 함수를 하나 더 포함시켜 확장하고 싶을때 사용합니다. 클래스와 같은 확장 대상에 확장 함수를 사용하기 위해 다음과 같이 정의할 수 있습니다.

```kotlin
fun 확장 대상.함수 이름(매개변수, ...): 반환값 {
	...
	return 값
}
```

최상위 클래스인 Any에 확장 함수를 구현하면 모든 클래스에 확장 함수를 추가할 수 있습니다.

**String 클래스에 나만의 확장 함수 추가하기**

```kotlin
package chap03.section5

fun main() {
	val source = "Hello World!"
	val target = "Kotlin"
	println(source.getLongString(target))
}

fun String.getLongString(target: String): String =
	if (this.length > target.length) this else target
```

### 중위 함수

중위 표현법이란 클래스의 멤버를 호출할 때 사용하는 점을 생략하고 함수 이름 뒤에 소괄호를 붙이지 않아 직관적인 이름을 사용할 수 있는 표현법입니다.
즉, 중위 함수란 일종의 연산자를 구현할 수 있는 함수를 말합니다.

**중위 함수의 조건**

* 멤버 메서드 또는 확장 함수여야 함
* 하나의 매개변수를 가져야 함
* infix 키워드를 사용하여 정의함

기존에 자주 사용하던 자료형 클래스인 Int에 확장 함수 multiply()를 만들고 이것을 중위 표현법으로 사용하는 예제입니다.

```kotlin
package chap03.section5

fun main() {
	//일반 표현법
	//val multi = 3.multiply(10)

	//중위 표현법
	val multi = 3 multiply 10
	println("multi: $multi")
}

//Int를 확장해서 multiply() 함수를 하나 더 추가함
infix fun Int.multiply(x: Int): Int { //infix로 선언되므로 중위 함수
	return this * x
}
```

### 꼬리 재귀 함수

## 함수와 변수의 범위