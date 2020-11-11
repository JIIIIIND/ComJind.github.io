---
title: "Do it! 코틀린 프로그래밍 챕터10"
date: 2020-11-06 13:35 +0900
---

## 코틀린 표준 함수

코틀린의 표준 함수는 람다식과 고차 함수를 이용해 선언되어 있습니다.

### 람다식과 고차 함수 복습하기

**람다식**

람다식은 항상 중괄호로 묶여 있으며 중괄호 안에 매개변수는 화살표(->) 왼쪽에 배치되고 오른쪽에는 그에 따른 식을 구성합니다.
```
val 변수 이름: 자료형 선언 = { 매개변수[,...] -> 람다식 본문 }
```

```kotlin
val sum: (Int, Int) -> Int = { x, y -> x + y }
val mul = {x: Int, y: Int -> x * y }
```

sum은 익명 함수로 만들어지며 매개변수는 선언부의 자료형에 의해 (Int, Int)를 가지게 됩니다. 반환값은 Int이므로 람다식의 x + y가 반환됩니다. mul은 변수의 자료형 표기가 생략되었지만 람다식에 있는 매개변수의 Int 선언 표현에 의해 반환 자료형을 (Int, Int) -> Int로 추론할 수 있습니다.

매개변수가 1개인 경우, 매개변수를 생략하고 it으로 표기할 수 있습니다.

```kotlin
val add: (Int) -> Int = { it + 1 }
```

만일 추론된 반환 자료형이 Unit이 아닌 경우에는 본문의 마지막 표현식이 반환값으로 처리됩니다.

```kotlin
val isPositive: (Int) -> Boolean = {
    val isPositive = it > 0
    isPositive
}

val isPositiveLabel: (Int) -> Boolean = number@ {
    val isPositive = it > 0
    return@number isPositive //라벨을 사용해 반환됨
}
```

**고차 함수**

고차 함수는 함수의 매개변수로 함수를 받거나 함수 자체를 반환할 수 있는 함수입니다.

 ```kotlin
 fun inc(x: Int): Int {
     return x + 1
 }

 fun high(name: String, body: (Int) -> Int): Int {
     println("name: $name")
     val x = 0
     return body(x)
 }
 ```

 high의 두 번째 매개변수 body는 람다식 함수를 받을 수 있습니다.

 이번에는 다양한 형태의 고차 함수 표현법을 살펴봅시다.

 ```kotlin
 val result = high("Sean", { x -> inc(x + 3) }) //함수를 이용한 람다식

 val result2 = high("Sean") { inc(it + 3) } //소괄호 바깥으로 빼내고 생략

 val result3 = high("Kim", ::inc) //매개변수 없이 함수의 이름만 사용할 때

 val result4 = high("Sean") { x -> x + 3 } //람다식 자체를 넘겨준 형태

 val result5 = high("Sean") { it + 3 } //매개변수가 1개인 경우 생략
 ```

### 클로저

람다식을 사용하다 보면 내부 함수에서 외부 변수를 호출하고 싶을 때가 있습니다. 클로저란 람다식으로 표현된 내부 함수에서 외부 범위에 선언된 변수에 접근할 수 있는 개념을 말합니다. 이때 람다식 안에 있는 외부 변수는 값을 유지하기 위해 람다식이 포획(capture)한 변수라고 부릅니다.

기본적으로 함수 안에 정의된 변수는 지역 변수로 스택에 저장되어 있다가 함수가 끝나면 같이 사라집니다. 하지만 클로저 개념에서는 포획한 변수는 참조가 유지되어 함수가 종료되어도 사라지지 않고 함수의 변수에 접근하거나 수정할 수 있게 해 줍니다. 클로저의 조건은 다음과 같습니다.

* final 변수를 포획한 경우 변수 값을 람다식과 함께 저장한다.
* final이 아닌 변수를 포획한 경우 변수를 특정 래퍼(wrapper)로 감싸서 나중에 변경하거나 읽을 수 있게 한다. 이때 래퍼에 대한 참조를 람다식과 함께 저장한다.

자바에서는 외부의 변수를 포획할 때 final만 포획할 수 있습니다. 따라서 코틀린에서 final이 아닌 변수를 사용하면 내부적으로 변환된 자바 코드에서 배열이나 클래스를 만들고 final로 지정해 사용됩니다.

```kotlin
package chap10.section1

fun main() {
    val calc = Calc()
    var result = 0 //외부의 변수
    calc.addNum(2, 3) { x, y -> result = x + y } //클로저
    println(result) //값을 유지하여 5 출력
}

class Calc {
    fun addNum(a: Int, b: Int, add: (Int, Int) -> Unit) { //람다식 add에는 반환값이 없음
        add(a, b)
    }
}
```

위 코드에서 result는 var로 선언되었습니다. addNum이 호출되면 result는 자신의 유효 범위를 벗어나 삭제되어야 하지만 클로저의 개념에 의해 독립된 복사본을 가집니다. 코드에서 보는 것과 같이 result는 final로 선언되었으며 addNum()에서 세 번째 인자는 익ㅁ여 함수로 처리되어 외부의 변수 result의 값을 바꾸도록 invoke()를 호출하고 있습니다.

```kotlin
fun filteredNames(length: Int) {
    val names = arrayListOf("Kim", "Hong", "Go", "Hwang", "Jeon")
    val filterResult = names.filter {
        it.length == length //바깥의 length에 접근
    }
    println(filterResult)
}
```

filter는 arrayList의 멤버 메서드로 람다식을 전달받고 있습니다. 이때 length는 람다식 바깥의 변수로 인자로 입력받은 길이에 일치하는 요소 목록을 반환해 filterResult에 저장하고 출력합니다.
클로저를 사용하면 내부의 람다식에서 외부 함수의 변수에 접근해 처리할 수 있어 효율성이 높습니다. 또 완전히 다른 함수에서 변수에 접근하는 것을 제한할 수 있습니다.

### 코틀린의 표준 라이브러리

**확장 함수의 람다식 접근 방법**

| 함수 이름 | 람다식의 접근 방법 | 반환 방법 |
|--|--|--|
| T.let | it | block 결과 |
| T.also | it | T caller (it) |
| T.apply | this | T caller (this) |
| T.run 또는 run | this | block 결과 |
| with | this | Unit |

위의 모든 함수가 람다식을 이용하고 있으며 접근 방법과 반환 방법의 차이로 구분됩니다.

### let() 함수 활용하기

let()함수는 함수를 호출하는 객체 T를 이어지는 block의 인자로 넘기고 block의 결괏값 R을 반환합니다.

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R { ...return block(this) }
```

let()함수는 제네릭의 확장 함수 형태이므로 어디든 적용할 수 있습니다. 매개변수로는 람다식 형태의 block이 있고 T를 매개변수로 받아 R을 반환합니다.
let() 함수 역시 R을 반환하고 있습니다. 본문의 this는 T를 가리킵니다. 이 함수를 호출한 객체를 인자로 받으므로 이를 사용하여 다른 메서드를 실행하거나 연산을 수행해야 하는 경우 사용할 수 있습니다.

```kotlin
package chap10.section1

fun main() {
    val score: Int? = 32
    // var score = null

    //일반적인 null 검사
    fun checkScore() {
        if (score != null) {
            println("Score: $score")
        }
    }

    //let 함수를 사용해 null검사를 제거
    fun checkScoreLet() {
        score?.let { println("Score: $it") } //1번
        val str = score.let { it.toString() } //2번
        println(str)
    }
    checkScore()
    checkScoreLet()
}
```

1번의 checkScoreLet()을 보면 score에 멤버 메서드를 호출하듯 let함수를 사용했는데 매개변수가 람다식 하나일 때는 let({...})에서 표현이 소괄호가 생략되어 let {...}과 같이 나타날 수 있습니다. 그리고 null에 안전한 호출을 위해 세이프콜(?.)을 사용했고, 만일 score가 null일 경우 람다식 구문은 수행되지 않습니다. null이 아니라면 자기 자신의 값 score를 it으로 받아서 처리할 수 있습니다.

2번을 보면 toString()을 사용해 it을 문자열로 변환한 후 반환된 값을 str에 할당합니다. 이때 세이프콜을 사용하지 않았습니다. 만일 score가 null이라면 str에는 null이 할당됩니다. 세이프콜을 사용하더라도 람다식을 사용하지 않게 되므로 str은 String?으로 추론되어 null이 할당됩니다.

**커스텀 뷰에서 let()함수 활용하기**

let() 함수를 활용하여 안드로이드의 커스텀 뷰에서 Padding(여백) 값을 지정하기 위해 다음과 같은 구문을 사용한다고 해 봅시다.

```kotlin
val padding = TypedValue.applyDimension(
    TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics).toInt()

setPadding(padding, 0, padding, 0) //왼쪽, 오른쪽 padding 설정
```

TypedValue.applyDimension()을 통해 얻은 값을 정수형으로 변환한 후 padding에 할당했습니다. 이때 padding이 한 번만 사용되면 변수 할당을 하느라 자원 낭비가 있을 수 있습니다. 다음과 같이 let()함수를 사용할 수 있습니다.

```kotlin
TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f,
        resources.displayMetrics).toInt().let { padding ->
    setPadding(padding, 0, padding, 0) //계산된 값을 padding이라는 이름의 인자로 받음
    }
```

여기서는 얻은 값을 let()을 통해 람다식으로 보내고 람다식의 본문 코드에서는 setPadding()함수를 호출해 얻은 값을 지정하고 있습니다. 인자가 1개밖에 없으므로 다음과 같이 it으로 간략화 할 수 있습니다.

```kotlin
TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f,
    resources.displayMetrics).toInt().let {
        setPadding(it, 0, it, 0) //padding 대신 it 사용
    }
```

**null가능성 있는 객체에서 let() 함수 활용하기**

let()함수를 세이프 콜과 함께 사용하면 if (null != obj)와 같은 null 검사 부분을 대체할 수 있습니다.

```kotlin
var obj: String? //null일 수 있는 변수 obj
...
if (null != obj) { //obj가 null이 아닐 경우 작업 수행(기존 방식)
    Toast.makeText(applicationContext, obj, Toast.LENGTH_LONG).show()
}
```

이것을 다음과 같이 세이프 콜과 let() 함수를 사용해 변경합니다.

```kotlin
obj?.let {
    Toast.makeText(applicationContext, it, Toast.LENGTH_LOONG).show()
}
```

만일 다음과 같이 else문이 포함된 긴 문장을 변경하려면 어떻게 할까요?

```kotlin
val firstName: String?
val lastName: String
...
//if문을 사용한 경우
if (null != firstName) {
    println("$firstName $lastName")
} else {
    print("$lastName")
}
```

firstName의 변수에 다음과 같이 let()과 엘비스 연산자를 적용해 변경하면 아래와 같이 한 줄로 단순화할 수 있습니다.

```kotlin
firstName?.let { print("$it $lastName") } ?: print("$lastName")
```

**메서드 체이닝을 사용할 때 let() 함수 활용하기**

메서드 체이닝이란 여러 메서드 혹인 함수를 연속적으로 호출하는 기법입니다.

```kotlin
var a = 1
var b = 2

a = a.let { it + 2 }.let {
    val i = it + b
    i
}
println(a)
```

let()함수가 유용하긴 하지만 코드의 가독성을 고려하면 너무 많이 사용하는 것은 권장하지 않습니다.

### also() 함수 활용하기

also() 함수는 함수를 호출하는 객체 T를 이어지는 block에 전달하고 객체 T 자체를 반환합니다. 선언부의 let() 함수와 also() 함수의 차이점을 비교해 봅시다.

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R = block(this)
public inline fun <T> T.also(block: (T) -> Unit): T { block(this); return this }
```

also()함수는 let() 함수와 역할이 거의 동일해 보입니다. 하지만 자세히 보면 반환하는 값이 다른데, let() 함수는 마지막으로 수행된 코드 블록의 결과를 반환하고, also() 함수는 블록 안의 코드 수행 결과와 상관 없이 T인 객체 this를 반환합니다.

```kotlin
var m = 1
m = m.also { it + 3 }
println(m) //원본 값 1
```

위의 코드처럼 연산 결과인 4가 할당되는 것이 아니라 it의 원래의 값 1이 다시 m에 할당됩니다.

```kotlin
package chap10.section1

fun main() {
    data class Person(var name: String, var skills : String)
    var person = Person("Kildong", "Kotlin")
    val a = person.let {
        it.skills = "Android"
        "success" //마지막 문장을 결과로 반환
    }
    println(person)
    println("a: $a") //String
    val b = person.also {
        it.skills = "Java"
        "success" //마지막 문장은 사용되지 않음
    }
    println(person)
    println("b: $b") //person의 객체 b
}
```

**특정 단위의 동작 분리**

디렉터리를 생성하는 함수를 다음과 같이 만들었다고 가정해 봅시다.

```kotlin
fun makeDir(path: String): File {
    val result = File(path)
    result.kmdirs()
    return result
}
```

디렉터리를 만드는 makeDir() 함수에서 경로 path를 매개변수로 받습니다. 그런 다음 File()을 통해 결과를 result에 할당합니다. File 객체의 멤버 메서드 mkdirs()를 호출해 파일 경로를 생성하고 File 객체의 result는 그대로 반환합니다. 여기서 let() 함수와 also() 함수의 특징을 이요하면, 이와 같은 함수를 간단하게 개선할 수 있습니다.

```kotlin
fun makeDir(path: String) = path.let{ File(it) }.also{ it.mkdirs() }
```

체이닝 형태로 구성해 앞에서 만든 함수와 동일한 역할ㅇ르 하게 됩니다. 

### apply() 함수 활용하기

계속해서 이전 함수들가 비교해 선언부를 살펴보겠습니다.

```kotlin
public inline fun <T, R> T.let(block: (T) -> R): R = block(this)
public inline fun <T> t.also(block: (T) -> Unit): T { block(this); return this}
public inlint fun <T> T.apply(block: T.() -> Unit): T { block(); return this}
```

apply()함수는 특정 객체를 생성하면서 함게 호출해야 하는 초기화 코드가 있는 경우 사용할 수 있습니다. apply()함수와 also()함수의 다른 점은 T.()와 같은 표현에서 람다식이 확장 함수로 처리된다는 것입니다.

```kotlin
package chap10.section1

fun main() {
    data class Person(var name: String, varskills : String)
    var person = Person("Kildong", "Kotlin")
        person.apply { this.skills = "Swift" } //여기서 this는 person 객체를 가리킴
    println(person)

    val returnObj = person.apply {
        name = "Sean" //this 는 생략할 수 있음
        skills = "Java" //this 없이 객체의 멤버에 여러 번 접근
    }
    println(person)
    println(returnObj)
}
```

apply()는 확장 함수로서 person을 this로 받아오는데 클로저를 사용하는 방식과 같습니다. 따라서 객체의 프로퍼티를 변경하면 원본 객체에 반영되고 또한 이 객체는 this로 반환됩니다. this.name = "Sean"과 같은 표현은 this가 생략 가능하기 때문에 name = "Sean"과 같이 작성할 수 있습니다. 이때 this로부터 반환된 객체를 returnObj에 할당하고 있습니다.

also함수와는 객체를 넘겨받는 방법이 다릅니다. also()함수에서는 it를 사용해 멤버에 접근합니다. 위 코드에서 person객체의 skills에 접근하는 방법을 보면 차이를 바로 알 수 있습니다.

```kotlin
person.also { it.skills = "Java" } //it으로 받고 생략할 수 없음
person.apply { skills = "Swift" } //this로 받고 생략
```

also()함수에서는 it을 생략할 수 없지만 apply() 함수에서는 this가 생략되어 멤버 이름만 사용하고 있습니다. 이것을 활용하면 특정 객체를 초기화하는 데 아주 유용합니다.

**레이아웃을 초기화할 때 apply() 함수 활용하기**

안드로이드에서 사용하는 레이아웃을 초기화할 때 일반적으로 다음과 같이 새로운 layoutParams 객체를 생성하고 속성을 지정합니다.

```kotlin
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT)
param.gravity = Gravity.CENTER_HORIZONTAL
param.weight = 1f
param.topMargin = 100
param.bottomMargin = 100
```

위 코드에서는 LinearLayout을 초기화하기 위해 생성된 변수에 일일이 멤버를 호출해 값을 지정하고 있습니다. 여기에 apply()함수를 적용하면 다음과 같이 변경할 수 있습니다.

```kotlin
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT).apply {
    gravity = Gravity.CENTER_HORIZONTAL
    weight = 1f //param을 사용하지 않고 직접 값을 지정할 수 있음
    topMargin = 100
    bottomMargin = 100
}
```

apply() 함수를 사용해 param의 각 멤버를 초기화하고 이것을 그대로 반환해 param에 할당할 수 있게 됩니다. 간략하고 훨씬 보기 좋은 코드가 되었습니다.

**디렉터리를 생성할 때 apply()함수 활용하기**

앞에서 본 디렉터리 생성 예제를 apply()에서도 사용할 수 있습니다. 다음은 기존의 디렉터리 생성 함수입니다.

```kotlin
fun makeDir(path: String): File {
    val result = File(path)
    result.mkdirs()
    return result
}
```

사실 이런 함수를 설계할 필요 없이 apply()함수를 사용하면 다음 한 줄로 같은 효과를 볼 수 있습니다.

```kotlin
File(path).apply { mkdirs() }
```

File(path)에 의해 생성된 결과를 람다식에서 this로 받습니다. File 객체의 mkdirs()를 호출해 파일 경로를 생성한 후 결과가 아닌 객체 this를 받습니다.

### run() 함수 활용하기

run() 함수는 인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태, 2가지로 사용할 수 있습니다. 객체 없이 run()함수를 사용하면 인자 없는 익명 함수처럼 사용할 수 있죠.

```kotlin
public inline fun <R> run(block: () -> R): R = return block()
public inline fun <T, R> T.run(block: T.() -> R): R = return block()
```

이번에는 block이 독립적으로 사용됩니다. 이어지는 block 내에서 처리할 작업을 넣어 줄 수 있으며, 일반 함수와 마찬가지로 값을 반환하지 않거나 특정 값을 반환할 수도 있습니다. 간단한 사용 예를 봅시다.

```kotlin
var skills = "Kotlin"
println(skills) //Kotlin

val a = 10
skills = run {
    val level = "Kotlin Level:" + a
    level //마지막 표현식이 반환됨
}
println(skills) //Kotlin Level: 10
```

run() 함수의 block이 독립적으로 사용되어 마지막 표현식을 반환했습니다. 이번엔 apply()함수와 비교해 봅시다.

```kotlin
package chap10.section1

fun main() {
    data class Person(var name: String, var skills : String)
    var person = Person("Kildong", "Kotlin")
    val returnObj = person.apply {
        this.name = "Sean"
        this.skills = "Java"
        "success" //사용되지 않음
    }
    println(person)
    println("returnObj: $returnObj")
    val returnObj2 = person.run {
        this.name = "Dooly"
        this.skills = "C#"
        "success"
    }
    println(person)
    println("returnObj2: $returnObj2")
}
```

run()함수와 apply()함수의 차이점을 보면 run()함수도 해당 객체를 this로 받아 변경할 수 있지만 apply() 함수는 this에 해당하는 객체를 반환한 반면에, run()함수는 마지막 표현식 "success"를 반환했음을 알 수 있습니다. 물론 마지막 표현식을 구성하지 않으면 Unit이 반환됩니다.

### with() 함수 활용하기

with() 함수는 인자로 받는 객체를 이어지는 block의 receiver로 전달하며 결괏값을 반환합니다. with() 함수는 run()함수와 기능이 거의 동일한데, run() 함수의 경우 receiver가 없지만 with() 함수에서는 receiver로 전달할 객체를 처리하므로 객체의 위치가 달라집니다.

```kotlin
public inline fun <T, R> with(receiver: T. block: T.() -> R): R = receiver.block()
```

with() 함수는 매개변수가 2개이므로 with() {...}와 같은 형태로 넣어 줍니다. 함수 선언에서 보여주듯 with()는 확장 함수 형태가 아니고 단독으로 사용되는 함수입니다. with()함수는 세이프 콜(?.)을 지원하지 않기 때문에 다음의 let()함수와 같이 사용되기도 합니다.

```kotlin
val result = with (user) {
    skills = "Java"
    email = "kildong@example.com"
    "success" //마지막 표현식 반환
}
```

이 경우에 result는 "success"를 할당하는 String형의 변수가 됩니다.

### use() 함수 활용하기

보통 특정 객체가 사용된 후 닫아야 하는 경우가 생기는데 이때 use() 함수를 사용하면 객체를 사용한 후 close() 함수를 자동적으로 호출해 닫아 줄 수 있습니다. 내부 구현을 보면 예외 오류 발생 여부와 상관 없이 항상 close()를 호출을 보장합니다. 선언부를 확인해 봅시다.

```kotlin
public inline fun <T: Closeable?, R> T.use(block: (T) -> R): R
```

먼저 T의 제한된 자료형을 보면 Closeable?로 block은 닫힐 수 있는 객체를 지정해야 합니다. 예를 들면 파일 객체의 경우에 사용하고 나서 닫아야 하는 대표적인 Closeable 객체가 됩니다.

```kotlin
package chap10.section1

import java.io.File
import java.io.FileOutputStream
import java.io.PrintWriter

fun main() {
    PrintWriter(FileOutputStream("d:\\test\\output.txt")).use {
        it.println("hello")
    }
}
```

PrintWriter()는 파일 등에 내용을 출력합니다. 이때 인자로 FileOutputStream()을 사용해 파일 output.txt를 지정하고 있습니다. 따라서 output.txt에 hello를 출력하고 use()에 의해 내부적으로 파일을 닫게 됩니다.

다음은 output.txt파일에 "hello"라는 문자열을 저장하는 소스 코드입니다. 일반적으로 파일 작업을 하고 나면 close()를 명시적으로 호출해야 하는데, use 블록 안에서는 그럴 필요가 없습니다. 물론 파일에서 읽어들일 때도 사용할 수 있습니다. 먼저 d:\test\contents.txt에 파일을 생성하고 "Hello World"문자열을 작성해 둡니다. 이제 use() 함수를 사용해 다음과 같이 읽을 수 있습니다.

```kotlin
val file = File("d:\\test\\contents.txt")
file.bufferedReader().use {
    println(it.readText())
}
```

### 기타 함수의 활용


**takeIf() 함수와 takeUnless() 함수의 활용**

takeIf() 함수는 람다식이 true이면 결과를 반환하고, takeUnless() 함수는 람다식이 false이면 결과를 반환합니다.

```kotlin
public inline fun <T> T.takeIf(predicate: (T) -> Boolean): T?
    = (predicate(this)) this else null
```

takeIf()함수의 정의에서 볼 수 있듯이 predicate는 T 객체를 매개변수로서 받아오고, true이면 this를 반환하고 아니면 null을 반환합니다. takeUnless() 함수는 !predicate()가 사용되어 false일 때 반환됩니다.

```kotlin
//기존 코드
if (someObject != null && someObject.status) {
    doThis()
}
//개선한 코드
if (someObject?.status == true) {
    doThis()
}
//takeIf()함수를 사용해 개선한 코드
someObject?.takeIf { it.status }?.apply { doThis() }
```

null검사와 someObject 객체의 status의 상태를 검사해 true인 경우에 apply()를 적용해 doThis()를 호출합니다.
다음과 같이 엘비스 연산자(?:)를 함께 사용해 처리할 수도 있습니다.

```kotlin
val input = "Kotlin"
val keyword = "in"

//입력 문자열에 키워드가 있으면 인덱스를 반환하는 함수를 takeIf() 함수를 사용하여 구현
input.indexOf(keyword).takeIf { it >= 0 } ?: error("keyword not found")

//takeUnless()함수를 사용하여 구현
input.indexOf(keyword).takeUnless { it < 0 } ?: error("keyword not found")
```

**시간의 측정**

코틀린에는 람다식을 사용하는 시간 측정 함수를 표준 라이브러리에서도 제공합니다. 코틀린 kotlin.system 패키지에 있는 2개의 측정 함수 measureTimeMillis()와 measureNanoTime()을 사용할 수 있습니다. 표준 라이브러리의 Timing.kt에 보면 두 함수는 다음과 같이 선언되어 있습니다.

```kotlin
public inline fun measureTimeMillis(block: () -> Unit): Long {
    val start = System.currentTimeMillis()
    block()
    return System.currentTimeMillis() - start
}

public inline fun measureNanoTime(block: () -> Unit): Long {
    val start = System.nanoTime()
    block()
    return System.nanoTime() - start
}
```

표준 라이브러리 Timing.kt 파일의 코드를 보면 밀리초와 나노초를 측정하는 함수 2개가 람다식으로 작성되어 block코드의 내용을 측정할 수 있습니다.

```kotlin
val executionTime = measureTimeMillis {
    //측정할 코드
}
println("Execution Time = $executionTime ms")
```

측정하려는 코드를 measureMillis()함수의 본문에 작성하면 측정 시간을 Long형 값으로 얻을 수 있습니다. 함수의 성능을 평가할 때 유용합니다.

**난수 생성하기**

난수를 생성하려면 자바의 java.util.Random을 사용할 수도 있었지만 JVM에만 특화된 난ㅍ수를 생성하기 때문에 코틀린에서는 멀티 플랫폼에서도 사용 가능한 kotlin.random.Random패키지를 제공합니다. 다음 소스 코드의 number는 0부터 21사이의 난수를 제공합니다.

```kotlin
import kotlin.random.Random

val number = Random.nextInt(21) //숫자는 난수 발생 범위
println(number)
```

## 람다식과 DSL

### 코틀린에서 DSL사용하기

### DSL을 사용한 사례

## 파일 입출력

### 표준 입출력의 기본 개념

### 파일에 쓰기

### 파일에서 읽기
