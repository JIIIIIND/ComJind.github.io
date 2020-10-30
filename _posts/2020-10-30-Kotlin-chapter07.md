---
title: "Do it! 코틀린 프로그래밍 챕터07"
date: 2020-10-30 21:37:24 +0900
---

## 추상 클래스와 인터페이스

### 추상 클래스

* 선언 등의 대략적인 설계 명세와 공통의 기능을 구현한 클래스
* 추상 클래스를 상속하는 하위 클래스에서 내용을 더 구체화 해야 함

```kotlin
abstract class Vehicle
```

위와 같은 방식으로 선언
멤버 프로퍼티나 메서드도 abstract로 선언할 수 있음
추상 프로퍼티나 메서드가 하나라도 있다면 해당 클래스는 추상 클래스가 되어야 함

```kotlin
//추상 클래스, 주 생성자에는 비추상 프로퍼티 선언의 매개변수 3개가 있음
abstract class Vehicle(val name: String, val color: String, val weight: Double) {
	//추상 프로퍼티(반드시 하위 클래스에서 재정의해 초기화 해야 함)
	abstract var maxSpeed: Double

	//일반 프로퍼티(초깃값인 상태를 저장할 수 있음)
	var year = "2018"

	//추상 메서드
	abstract fun start()
	abstract fun stop()

	//일반 메서드
	fun displaySpecs() {
		println("Name: $name, Color: $color, Weight: $weight, Year: $year, Max Speed: $maxSpeed")
	}
}

class Car(name: String, color: String, weight: Double, override var maxSpeed: Double) //maxSpeed의 경우 override
	: Vehicle(name, color, weight) {
		override fun start() {
			println("Car started")
		}
		override fun stop() {
			println("Car stopped")
		}
	}
```

일반 프로퍼티나 메서드도 만들 수 있기 때문에 공통의 프로퍼티와 메서드를 미리 만들 수 있음
보통 연관성이 높은 클래스의 기능이나 속성을 미리 정의함

**단일 인스턴스로 생성 시**

```kotlin
abstract class Printer {
	abstract fun print() //추상 메서드
}

val myPrinter = object: Printer() { //객체 인스턴스
	override fun print() { //추상 메서드 구현
		println("printing...")
	}
}
```

### 인터페이스

* 다른 객체지향 언어와는 다르게 메서드에 구현 내용이 포함될 수 있음
* 프로퍼티를 통해 상태를 저장할 수 없음
* 추상 클래스처럼 객체를 생성할 수 없고 하위 클래스를 통해 구현 및 생성

**인터페이스의 사용 이유**

* 인터페이스는 클래스가 아니기에 상속의 형태로 하위 클래스에 프로퍼티와 메서드를 전하지 않음
	* 하위 클래스가 아닌 구현 클래스라고 부르는 이유
* 구현 클래스와 강한 연관을 가지지 않음
	* 인터페이스가 변하더라도 구현하는 클래스에 크게 영향을 끼치지 않게 할 수 있음

```kotlin
interface Pet {
	var category: String //abstract 키워드가 없어도 기본은 추상 프로퍼티
	fun feeding() //마찬가지로 추상 메서드
	fun patting() { //일반 메서드: 구현부를 포함하면 일반적인 메서드로 기본이 됨
		println("Keep patting!) //구현부
	}
}

class Cat(override var category: String): Pet {
	override fun feeding() {
		println("Feed the cat a tuna can!")
	}
}

fun main() {
	val obj = Cat("small")
	println("Pet Category: ${obj.category}")
	obj.feeding() //구현 메서드
	obj.patting() //기본 메서드
}
```

val로 선언된 프로퍼티는 게터를 통해 필요한 내용을 구현할 수 있습니다.
하지만 보조 필드인 field는 사용할 수 없습니다.

```kotlin
interface Pet {
	var category: String //abstract 키워드가 없어도 기본은 추상 프로퍼티
	val msgTags: String //val 선언 시 게터의 구현이 가능
		get() = "I'm your loveley pet!"
	fun feeding() //마찬가지로 추상 메서드
	fun patting() { //일반 메서드: 구현부를 포함하면 일반적인 메서드로 기본이 됨
		println("keep patting!") //구현부
	}
}
...
println("Pet Message Tags: ${obj.msgTags}")
```

**인터페이스 구현의 필요성**

```kotlin
open class Animal(val name: String)

class Dog(name: String, override val category: String) : Animal(name), Pet {
    override fun feeding() {
        println("Feed the dog a bone")
    }
}

class Master {
    fun playWithPet(dog: Dog) {
        println("Enjoy with my dog.")
    }
	fun playWithPet(cat: Cat) {
        println("Enjoy with my cat.")
    }
}

fun main() {
    val master = Master()
    val dog = Dog("Toto" ,"Small")
    val cat = Cat("Coco", "BigFat")
    master.playWithPet(dog)
    master.playWithPet(cat)
}
```

* Master 클래스의 playWithPet은 놀고자 하는 동물에 따라 매개변수를 다르게 정한 오버로딩된 메서드
* 만약 애완동물의 종류가 늘어난다면 그에 맞는 playWithPet이 추가되어야 함
* 강한 의존성을 가지게 됨

```kotlin
open class Animal(val name: String)

class Dog(name: String, override val category: String) : Animal(name), Pet {
    override var species: String = "dog"
    override fun feeding() {
        println("Feed the dog a bone")
    }
}

class Master {
    fun playWithPet(pet: Pet) { //인터페이스를 객체로 매개변수를 지정
        println("Enjoy with my ${pet.species}.")
    }
}

fun main() {
    val master = Master()
    val dog = Dog("Toto" ,"Small")
    val cat = Cat("Coco", "BigFat")
    master.playWithPet(dog)
    master.playWithPet(cat)
}
```

* 기존의 Master는 Cat이나 Dog 클래스에 의존적이었지만 인터페이스를 이용해 의존성을 제거함

**여러 인터페이스로부터 구현 가능**

```kotlin
interface Bird {
    val wings: Int
    fun fly()
    fun jump() {
        println("bird jump!")
    }
}

interface Horse {
    val maxSpeed: Int
    fun run()
    fun jump() {
        println("jump!, max speed: $maxSpeed")
    }
}

class Pegasus: Bird, Horse {
    override val wings: Int = 2
    override val maxSpeed: Int = 100
    override fun fly() {
        println("Fly!")
    }
    override fun run() {
        println("Run!")
    }
    override fun jump() {
        super<Horse>.jump()
        println("Pegasus Jump!")
    }
}
```

**인터페이스의 위임**

* 인터페이스에서도 by 위임자를 사용할 수 있음

```kotlin
interface A {
    fun functionA()
}

interface B {
    fun functionB()
}

class C(val a: A, val b: B) {
    fun functionC() {
        a.functionA()
        b.functionB()
    }
}

class DelegateC(a: A, b: B): A by a, B by b {
    fun functionC() {
        functionA()
        functionB()
    }
}
```

**위임을 이용한 멤버 접근**

```kotlin
interface Nameable {
    var name: String
}
class StaffName : Nameable {
    override var name: String = "Sean"
}
class Work: Runnable { //스레드 실행을 위한 인터페이스
   override fun run() {
       println("Work...")
   }
}
//각 매개변수에 해당 인터페이스를 위임
class Person(name: Nameable, work: Runnable) : Nameable by name, Runnable by work
fun main() {
    val person = Person(StaffName(), Work()) //생성자를 사용해 객체 바로 전달
    println(person.name) //여기서 StaffName 클래스의 name 접근
    person.run() //여기서 Work클래스의 run 접근
}
```

* Person 클래스가 마치 상속과 같은 형태로 위임을 사용
* StaffName과 Work의 생성자를 이용해 객체 전달
* person객체는 각 클래스의 위임된 멤버에 접근

## 데이터 클래스와 기타 클래스

### 데이터 클래스

* 오로지 데이터 저장만을 위해 사용
* 구현 로직을 가지고 있지 않고 순수한 데이터 객체를 표현
* 다음과 같은 메서드가 내부적으로 자동 생성
	- 프로퍼티를 위한 게터/세터
	- 비교를 위한 equals()와 키 사용을 위한 hashCode()
	- 프로퍼티를 문자열로 변환해 순서대로 보여주는 toString()
	- 객체 복사를 위한 copy()
	- 프로퍼티에 상응하는 component1(), component2() 등

다음과 같이 선언함

```kotlin
data class Customer(var name: String, var email: String)
```

그리고 다음 조건을 만족해야 함
* 주 생성자는 최소한 하나의 매개변수를 가져야 함
* 주 생성자의 모든 매개변수는 val, var로 지정된 프로퍼티여야 함
* 데이터 클래스는 abstract, open, sealed, inner 키워드를 사용할 수 없음

필요하다면 부 생성자나 init블록을 넣어 데이터를 위한 간단한 로직을 포함할 수 있음

```kotlin
data class Customer(var name: String, var email: String) {
	var job: String = "Unknown"
	constructor(name: String, email: String, _job: String): this(name, email) {
		job = _job
	}
	init {
		//간단한 로직
	}
}
```

| 제공된 메서드 | 기능 |
|--|--|
| equals() | 두 객체의 내용이 같은지 비교하는 연산자(고유 값은 다르지만 의미 값이 같을 때) |
| hashCode() | 객체를 구별하기 위한 고유한 정수값 생성, 데이터 세트나 해시 테이블을 사용하기 위한 하나의 생성된 인덱스 |
| copy() | 빌더 없이 특정 프로퍼티만 변경해서 객체 복사하기 |
| toString() | 데이터 객체를 읽기 편한 문자열로 반환 하기 |
| componentN() | 객체의 선언부 구조를 분해하기 위해 프로퍼티에 상응하는 메서드 |

**객체 디스트럭처링**

객체가 가지고 있는 프로퍼티를 개별 변수로 분해하여 할당하는 것

```kotlin
val (name, email) = cus1
val(_, email) = cus1 //첫번째 프로퍼티 제외
```

개별적으로 프로퍼티를 가져오기 위해 componentN()메서드를 사용할 수 있음

```kotlin
val name2 = cus1.component1()
val email2 = cus1.component2()
```

데이터가 많은 경우 반복문을 통해 분해 가능

```kotlin
val customers = listOf(cus1, cus2, bob, erica) //모든 객체를 컬렉션 List 목록으로 구성
...
for ((name, mail) in customers) {
	println("name = $name, email = $email")
}
```

함수로부터 객체가 반환될 경우에도 사용할 수 있음

```kotlin
fun myFunc(): Customer {
	return Customer("Mickey", "mic@abc.com")
}
...
val (myName, myEmail) = myFunc()
```

람다식을 사용해 분리 가능

```kotlin
val myLamda = {
	(nameLa, emailLa): Customer ->
	println(nameLa)
	println(emailLa)
}
myLamda(cus1)
```

데이터 클래스를 이용하면 각종 부가적인 메서드도 사용하면서 데이터에 집중하여 정의할 수 있으므로 매우 유용한 클래스 기법입니다.

### 내부 클래스 기법

종류
* 중첩 클래스
* 이너 클래스
* 지역 클래스
* 익명 객체 방법

사용 이유
* 독립적인 클래스로 정의하기 모호한 경우
* 다른 클래스에서는 잘 사용하지 않는 내부에서만 사용하고 외부에서는 접근할 필요가 없을 때

남용하게 되면 클래스의 의존성이 커지고 코드가 읽기 어렵게 되므로 주의가 필요합니다.

**자바와 코틀린의 내부 클래스 비교**

| 자바 | 코틀린 |
|--|--|
| 정적 클래스(Static Class) | 중첩 클래스: 객체 생성 없이 사용 가능 |
| 멤버 클래스(Member Class) | 이너 클래스: 필드나 메서드와 연동하는 내부 클래스, inner 키워드 필요 |
| 지역 클래스(Local Class) | 지역 클래스: 클래스의 선언이 블록 안에 있는 지역 클래스 |
| 익명 클래스(Anonymous Class) | 익명 객체: 이름이 없고 주로 일화용 객체를 사용하기 위해 object 키워드를 통해 선언됨 |

**중첩 클래스**

기본적으로 정적 클래스처럼 다뤄집니다. 즉, 객체 생성 없이 접근할 수 있다는 것입니다.

```kotlin
package com.acaroom.kotlin.chap03.section04.nested

class Outer {
	val ov = 5
	class Nested {
		val nv = 10
		fun greeting() = "[Nested] Hello ! $nv" //외부의 ov에는 접근 불가
	}
	fun outside() {
		val msg = Nested().greeting() //객체 생성 없이 중첩 클래스의 메서드 접근
		println("[Outer]: $msg, ${Nested().nv}) //중첩 클래스의 프로퍼티 접근
	}
}

fun main() {
	//static처럼 객체 생성 없이 사용
	val output = Outer.Nested().greeting()
	println(output)

	//Outer.outside() //오류, 외부 클래스의 경우는 객체를 생성해야 함
	val outer = Outer()
	outer.outside()
}
```

companion 객체를 사용하면 바깥 클래스에 접근할 수 있습니다.

```kotlin
class Outer {
	class Nested {
		...
		fun accessOuter() { //컴패니언 객체는 접근할 수 있음
			println(country)
			getSomething()
		}
	}
	companion object { //컴패니언 객체는 static처럼 접근 가능
		const val country = "Korea"
		fun getSomething() = println("Get something...")
	}
}
```

컴패니언 객체는 객체 생성 없이 고정적인 메모리를 가지기 때문에 사용할 수 있습니다.
외부 클래스의 멤버에 접근하기 위해선느 inner키워드를 사용해 이너 클래스를 만들어야 합니다.

**이너 클래스**

* 중첩 클래스와는 다르게 바깥 클래스의 멤버들에 접근 가능
* private 멤버도 접근할 수 있음

```kotlin
package chap07.section2

class Smartphone(val model: String) {
    private val cpu = "Exynos"

    inner class ExternalStorage(val size: Int) {
        fun getInfo() = "${model}: Installed on $cpu with ${size}Gb" //바깥 클래스의 프로퍼티 접근
    }
}

fun main() {
    val mySdcard = Smartphone("S7").ExternalStorage(32)
    println(mySdcard.getInfo())
}
```

**지역 클래스**

* 특정 메서드의 블록이나 init 블록과 같이 블록 범위에서만 유효한 클래스
* 블록 범위를 벗어나면 더 이상 사용되지 않음

```kotlin
...
class Smartphone(val model: String) {
    private val cpu = "Exynos"
    ...
    fun powerOn(): String {
        class Led(val color: String) { //지역 클래스 선언
            fun blink(): String = "Blinking $color on $model" //외부의 프로퍼티는 접근 가능
        }
        val powerStatus = Led("Red") //여기에서 지역 클래스가 사용됨
        return powerStatus.blink()
    }
}

fun main() {
    ...
    val myphone = Smartphone("Note9")
    myphone.ExternalStorage(128)
    println(myphone.powerOn())
}
```

**익명 객체**

* 자바에서는 익명 이너 클래스라는 것을 제공해 일회성으로 객체를 생성해 사용
* 코틀린은 object키워드를 통해 익명 객체로 이와 같은 기능을 수행
* 익명 객체 기법으로 앞에서 살펴본 다중의 인터페이스를 구현할 수 있음

```kotlin
package chap07.section2

interface Switcher { //인터페이스의 선언
    fun on(): String
}

class Smartphone(val model: String) {
    ...
    fun powerOn(): String {
        class Led(val color: String) {
            fun blink(): String = "Blinking $color on $model"
        }
        val powerStatus = Led("Red")
        val powerSwitch = object : Switcher { //익명 객체를 사용해 Switcher의 on()을 구현
            override fun on(): String {
                return powerStatus.blink()
            }
        } //익명 객체 블록의 끝
        return powerSwitch.on() // 익명 객체의 메서드 사용
    }
}
```

### 실드 클래스와 열거형 클래스

**실드 클래스**

미리 만들어 놓은 자료형들을 묶어서 제공하기 때문에 어떤 의미에서는 열거형 클래스의 확장으로도 볼 수 있음

* 실드 클래스 그 자체는 추상 클래스와 같기 때문에 객체를 만들 수 없음
* 생성자도 기본적으로 private이며 private이 아닌 다른 생성자는 허용하지 않음
* 같은 파일 안에서는 상속이 가능하지만, 다른 파일에서는 상속이 불가함
* 블록 안에 선언되는 클래스는 상속이 필요한 경우 open키워드로 선언될 수 있음

```kotlin
package chap07.section2

sealed class Result {
	open class Success(val message: String): Result()
	class Error(val code: Int, val message: String): Result()
}

class Status: Result() //실드 클래스 상속은 같은 파일에서만 가능
class Inside: Result.Success("Status") //내부 클래스 상속
```

다음과 같은 방식으로도 선언할 수 있음

```kotlin
sealed class Result 

open class Success(val message: String): Result()
class Error(val code: Int, val message: String): Result()

class Status: Result()
class Inside: Success("Status")
...
fun main() {
    //Success에 대한 객체 생성
    val result = Result.Success("Good!")
    val msg = eval(result)
    println(msg)
}
//상태를 검사하기 위한 함수
fun eval(result: Result): String = when(result) {
    is Status -> "in progress"
    is Result.Success -> result.message
    is Result.Error -> result.message
    //모든 조건을 가지므로 else가 필요 없음
}
```

이 경우 내부 클래스를 상속할 때 점 표기로 접근하지 않고 그대로 사용합니다.
실드 클래스는 특정 객체 자료형에 따라 when문과 is에 의해 선택적으로 실행할 수 있습니다. 여기에서는 모든 경우가 열거되었으므로 else문이 필요 없습니다. 이너 클래스나 중첩 클래스로 구현하려고 하면 모든 경우의 수를 컴파일러가 판단할 수 없어 else를 필요로 합니다.

**열거형 클래스**

* 여러 개의 상수를 선언하고 열거된 값을 조건에 따라 선택할 수 있는 특수한 클래스
* 실드 클래스처럼 다양한 자료형을 다루지 못함

```kotlin
enum class <클래스 이름> [(생성자)] {
	상수1[(값)], 상수2[(값)], 상수3[(값)], ...
	[; 프로퍼티 혹은 메서드]
}
```

주 생성자를 가지고 값을 초기화 할 수 있음

```kotlin
enum class DayOfWeek(val num: Int) {
	MONDAY(1), TUESDAY(2), WEDNESDAY(3),
	THURSDAY(4), FRIDAY(5), SATURDAY(6), SUNDAY(7)
}
```

필요한 경우 메서드를 포함할 수 있음

```kotlin
enum class Color(val r: Int, val g: Int, val b: Int) {
	RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(255, 255, 0),
	GREEN(0, 255, 0), BLUE(0, 0, 255), INDIGO(75, 0, 130),
	VIOLET(238, 130, 238); //세미콜론으로 끝을 알림
	fun rgb() = (r * 256 + g) * 256 + b //메서드 포함
}
```

### 애노테이션

코드에 부가 정보를 추가하는 역할을 합니다. @ 기호와 함께 나타내는 표기법으로 주로 컴파일러나 프로그램 실행 시간에서 사전 처리를 위해 사용합니다.
애노테이션 클래스를 직접 만드는 것은 고급 기법에 해당하기 때문에 프레임워크를 제작하지 않는 한 사용할 일은 별로 없습니다.

**애노테이션 선언하기**

사용자 애노테이션을 만들기 위해서는 키워드 annotation을 사용해 클래스를 다음과 같이 선언합니다.

```kotlin
annotation class 애노테이션 이름
```

애노테이션 클래스를 선언하면 @기호를 붙여서 다음과 같이 사용할 수 있습니다.

```kotlin
annotation class Fancy //선언
...
@Fancy class MyClass{...} //사용
```

애노테이션은 다음과 같은 몇 가지 속성을 사용해 정의될 수 있습니다.

* @Target
    - 애노테이션이 지정되어 사용할 종류(클래스, 함수, 프로퍼티 등)를 정의
* @Retention
    - 애노테이션을 컴파일된 클래스 파일에 저장할 것인지 실행 시간에 반영할 것인지 결정
* @Repeatable
    - 애노테이션을 같은 요소에 여러 번 사용 가능하게 할지를 결정
* @MustBeDocumented
    - 애노테이션이 API의 일부분으로 문서화하기 위해 사용

속성과 함께 정의된 애노테이션 클래스의 예는 다음과 같습니다.

```kotlin
@Target(AnnotationTarget.CLASS, AnnotationTarget.FUNCTION,
        AnnotationTarget.VALUE_PARAMETER, AnnotationTarget.EXPRESSION)
@Retention(AnnotationRetention.SOURCE) //애노테이션의 처리 방법 - SOURCE: 컴파일시간에 제거됨
@MustBeDocumented
annotation class Fancy
```

**리플렉션**

프로그램을 실행할 때 프로그램의 특정 구조를 분석해 내는 기법으로 사용됩니다. 어떤 함수를 정의하는데 함수의 매개변수로 클래스 타입을 선언하고 실행할 때 매개변수로 전달된 클래스 이름, 클래스의 메서드나 프로퍼티를 알아내는 작업이 리플렉션입니다.

실행 시간에 클래스를 분석하려면 클래스에 대한 정보를 표현하는 클래스 레퍼런스로부터 알아 냅니다. 따라서 특정 클래스의 정보를 분석하기 위해 클래스 타입인 KClass<*>로 정의하고 클래스 레퍼런스는 클래스 이름::class와 같은 형태로 표현할 수 있습니다.

```kotlin
package chap07.section2

import kotlin.reflect.KClass
import kotlin.reflect.full.memberFunctions
import kotlin.reflect.full.memberProperties

class User(val id: Int, val name: String, var grade: String = "Normal") {
    fun check() {
        if (grade == "Normal") println("You need to get the Silver grade")
    }
}

fun main() {
    //타입을 출력
    println(User::class) //클래스 레퍼런스를 위해 ::class 사용
    //클래스의 프로퍼티와 메서드의 정보를 출력
    val classInfo = User::class
    classInfo.memberProperties.forEach {
        println("Property name: ${it.name}, type: ${it.returnType}")
    }
    classInfo.memberFunctions.forEach {
        println("Function name: ${it.name}, type: ${it.returnType}")
    }
    //함수에 전달해 자료형을 알아냄
    getKotlinType(User::class)
}

fun getKotlinType(obj: KClass<*>) {
    println(obj.qualifiedName)
}
```

User 클래스의 정보를 일기 위해 User::class의 변수인 classInfo를 사용해 정보를 출력했습니다.

**애노테이션의 위치**

애노테이션이 들어갈 수 있는 곳은 다음과 같습니다.

```kotlin
@Fancy class MyClass {
    @Fancy fun myMethod(@Fancy myProperty: Int): Int {
        return (@Fancy 1)
    }
}
```

클래스의 앞이나 메서드 앞, 프로퍼티의 앞에 애노테이션을 사용할 수 있고, 반환할 때에는 값 앞에 표기하고 소괄호로 감쌉니다.

생성자에 애노테이션을 사용하면 constructor를 생략할 수 없습니다.

애노테이션은 다음과 같이 프로퍼티의 게터/세터에도 사용할 수 있습니다.

```kotlin
class Foo {
    var x: MyDependency? = null
        @Fancy set
}
```

**애노테이션의 매개변수와 생성자**

애노테이션에 매개변수를 지정하고자 하려면 다음과 같이 생성자를 통해 정의합니다.

```kotlin
annotation class Special(val why: String) //애노테이션 클래스의 정의
@Special("example") class Foo {} //애노테이션에 매개변수를 지정
```

매개변수로 사용될 수 있는 자료형은 다음과 같습니다.
* 자바의 기본형과 연동하는 자료형
* 문자열
* 클래스(클래스 이름::class)
* 열거형
* 기타 애노테이션
* 위의 목록을 가지는 배열

애노테이션이 또 다른 애노테이션을 가지고 사용할 때는 @ 기호를 사용하지 않아도 됩니다.

```kotlin
annotation class ReplaceWith(val expression: String) //첫 번째 애노테이션 클래스 정의

annotation class Deprecated( //두 번째 애노테이션 클래스 정의
    val messgae: String,
    val replaceWith: REplaceWith = ReplaceWith(""))
...
//ReplaceWith는 @ 기호가 생략됨
@Deprecated("This functino is deprecated, use ===insted", ReplaceWith("this === other"))
```

애노테이션의 인자로 특정 클래스가 필요하면, 코틀린의 KClass를 사용해야 합니다. 그러면 코틀린 컴파일러가 자동적으로 자바 클래스로 변환할 것입니다.

```kotlin
import kotlin.reflect.KClass
annotation class Ann(val arg1: KClass<*>, val arg2: KClass<out Any>)
...
@Ann(String::class, Int::class) class MyClass
```

String::class와 Int::class는 코틀린의 리플렉션 표현입니다. 이것은 ```KClass<T>```를 반환합니다.

**표준 애노테이션**

코틀린 표준 라이브러리가 가지고 있는 애노테이션

```kotlin
@JvmName("filterStrings")
fun filter(list: List<String>): Unit

@JvmName("filterInts")
fun filter(list: List<Int>): Unit
```

@JvmName은 filter()라는 이름을 자바에서 각각 filterStrings() 와 filterInts()로 바꿔주는 것입니다. 자바로 바뀌면 해당 애노테이션에 의해 다음과 같이 표현됩니다.

```kotlin
public static final void filterStrings(java.util.List<java.lang.String>);
...
public static final void filterInts(java.util.List<java.lang.Integer>);
```

그 외에도 @JvmStatic은 자바의 정적 메서드로 생성할 수 있게 해주고, @Throw는 코틀린의 throw구문이 자바에서도 포함되도록 합니다.

```kotlin
class File(val path: String) {
    @Throws(FileNotFoundException::class)
    fun exists(): Boolean {
        if (!Paths.get(path).toFile().exists())
        throw FileNotFoundException("$path does not exist")
        return true
    }
}
```

@JvmOverloads는 코틀린에서 기본값을 적용한 인자에 함수를 모두 오버로딩해 줍니다.

```kotlin
public void checkFile() throws FileNotFoundException {
    boolean exists = new fILE("SOMEFILE.TXT").exists();
    System.out.println("File exists");
}
```

## 연산자 오버로딩

연산자 오버로딩도 클래스의 다형성의 한 경우로 플러스와 같은 연산자에 여러 가지 다른 의미의 작동을 부여할 수 있습니다.
코틀린에서는 특정 연산자의 역할을 함수로 정의하고 있습니다. 이를 일종의 협약이라고 합니다.

**연산자의 우선 순위**

| 우선순위 | 분류 | 심볼 |
|--|:--:|:--:|
| 높음 | 접미사(Postfix) | ++, --, ., ?., ? |
|  | 접두사(Prefix) | -, +, ++, --, !, 라벨 선언(이름@) |
|  | 오른쪽 형식(Type RHS) | :, as, as? |
|  | 배수(Multiplicative) | *, /, % |
|  | 첨가(Additive) | +, - |
|  | 범위(Range) | .. |
|  | 중위 함수(Infix Function) | SimpleName |
|  | 엘비스(Elvis) | ?: |
|  | 이름 검사(Name Check) | in, !in, is, !is |
|  | 비교(Comparison) | <, >, <=, >= |
|  | 동등성(Equality) | ==, != |
|  | 결합(Conjunction) | && |
|  | 분리(Disjunction) | \|\| |
| 낮음 | 할당(Assignment) | =, +=, -=, *=, /=, %= |

### 연산자의 작동 방식

연산자를 사용하면 관련된 멤버 메서드를 호출하는 것과 같습니다. 예를 들어 a + b 는 a.plus(b) 함수가 내부적으로 호출되는 것입니다.

```kotlin
val a = 5
val b = 10
print(a.plus(b))
```

기본형을 위한 오버로딩된 plus()함수를 나열하면 다음과 같습니다.

```kotlin
//표준 라이브러라의 Primitives.kt파일의 일부
...
//기본형을 위한 + 연산자
operator fun plus(other: Byte): Int
operator fun plus(other: Short): Int
operator fun plus(other: Int): Int
operator fun plus(other: Long): Long
operator fun plus(other: Float): Float
operator fun plus(other: Double): Double

//문자열 연결
operator fun String?.plus(other: Any?): String
```

필요하다면 사용자가 추가적으로 함수를 오버로딩할 수 있습니다.

```kotlin
package chap07.section3

class Point(var x: Int = 0, var y: Int = 10) {
    //plus() 함수의 연산자 오버로딩
    operator fun plus(p: Point) : Point {
        return Point(x + p.x, y + p.y)
    }
}

fun main() {
    val p1 = Point(3, -8)
    val p2 = Point(2, 9)

    var point = Point()
    point = p1 + p2 //Point 객체의 + 연산이 가능하게 됨
    println("point = (${point.x}, ${point.y})")
}
```

### 연산자의 종류

**산술 연산자**

| 표현식 | 의미 |
|--|:--:|
| a + b  | a.plus(b) |
| a - b | a.minus(b) |
| a * b | a.times(b) |
| a / b | a.div(b) |
| a % b | a.rem(b) (코틀린 1.1부터), a.mod(b) (지원 중단) |
| a..b | a.rangeTo(b) |

**호출 연산자**

함수 호출을 돕는 데 사용됩니다. 특정 객체에 인수를 넣어 처리하기 위해 다음과 같은 표현이 가능합니다.

```kotlin
class Manager {
    operator fun invoke(value: String) {
        println(value)
    }
}

fun main() {
    val manager = Manager()
    manager("Do something for me!") //manager.invoke("...") 형태로 호출되며 invoke가 생략됨
}
```

**인덱스 접근 연산자**

| 표현식 | 의미 |
|--|--|
| a[i] | a.get(i) |
| a[i, j] | a.get(i, j) |
| a[i_1, ..., i_n] | a.get(i_1, ..., i_n) |
| a[i] = b | a.set(i, b) |
| a[i, j] = b | a.set(i, j, b) |
| a[i_1, ..., i_n] = b | a.set(i_1, ..., i_n, b) |

**단일 연산자**

| 표현식 | 의미 |
|--|--|
| +a | a.unaryPlus() |
| -a | a.unaryMinus() |
| !a | a.not() |

**범위 연산자**

in 연산자는 특정 객체를 반복하기 위해 반복문에 사용하거나 범위 연산자와 함께 포함 여부를 판단할 수도 있습니다.

```kotlin
if (i in 1..10) { // 1 <= i && i <= 10와 동일
    println(i)
}
for (i in 1..4) print(i)
```

따라서 이 연산자를 오버로딩하려면 contains() 메서드를 이용할 수 있습니다.
!in 형식은 반대의 경우로 범위에 없는 경우를 가리킵니다.

| 표현식 | 의미 |
|--|--|
| a in b | b.contains(a) |
| a !in b | !b.contains(a) |

**대입 연산자**

주의해야 할 점은 +에 대응하는 plus()를 오버로딩하면 +=는 자동으로 구현됩니다. 따라서 plusAssign()을 따로 오버로딩할 필요가 없습니다.

| 표현식 | 의미 |
|--|--|
| a += b | a.plusAssign(b) |
| a -= b | a.minusAssign(b) |
| a *= b | a.timesAssign(b) |
| a /= b | a.divAssign(b) |
| a %= b | a.remAssign(b), a.modAssign(b) |

**동등성 연산자**

일치와 불일치에 대한 연산자는 두 객체의 값의 동등성을 판별합니다.

| 표현식 | 의미 |
|--|--|
| a == b | a?.equals(b) ?: (b === null) |
| a != b | !(a?.equals(b) ?: (b === null)) |

a와 b가 둘 다 null이라면 true를 반환한다는 점에 주의해야 합니다. equals는 Any 안에 operator 키워드가 붙어서 구현되어 있기 때문에 하위 클래스에서는 override 키워드를 사용해서 ==와 치환할 수 있습니다. 또한 이런 특이점 때문에 equals는 확장 함수로 구현할 수 없습니다.

**비교 연산자**

모든 비교 연산자는 compareTo()를 호출해 반환되는 정수를 보고 비교합니다.

| 표현식 | 의미 |
|--|--|
| a > b | a.compareTo(b) > 0 |
| a < b | a.compareTo(b) < 0 |
| a >= b | a.compareTo(b) >= 0 |
| a <= b | a.compareTo(b) <= 0 |