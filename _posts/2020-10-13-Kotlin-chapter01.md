---
title: "Do It! 코틀린 프로그래밍 챕터01"
date: 2020-10-13 23:22:11 +0900
categories: Kotlin
---

## 기본 설정

### JDK 설치

Oracle JDK는 특정 기능을 사용할 때 라이선스 비용을 지불하는 정책을 시행하고 있기에 여기서는 Open JDK에 부가기능을 추가한 Zulu라는 JDK를 사용한다.

JDK를 설치하고 환경변수를 설정해주면 끝남.

### IDE

Intellij IDEA를 사용한다.

#### 주요 단축키

| 기능 | 단축키 |
|---|:---:|
| Message | Alt + 0 |
| Projects | Alt + 1 |
| Favorites | Alt + 2 |
| Run | Alt + 4 |
| Debug | Alt + 5 |
| TODO | Alt + 6 |
| Structure | Alt + 7 |
| Terminal | Alt + F12 |

#### main 함수 인자 값 설정

[RUN > Edit Configurations]의 Program arguments에 필요한 명령행 인자를 지정해주면 된다.

## main함수

자바 같은 객체지향 언어에서는 최소한 하나의 클래스와 그 안에 main()함수가 있어야 실행이 가능하다.
하지만 코틀린의 경우 main()함수만 작성해도 JVM에서 실행되며, main()함수가 있는 파일을 기준으로 자바 클래스가 자동으로 생성이 된다.

[Tools > Kotlin > Show Kotlin Bytecode]메뉴를 누른 후 생성된 화면에서 [Decompile] 버튼을 눌러 확인 가능하다.

ex)
```Kotlin
fun main() {
	println("Hello Kotlin!")
}
```
위의 코드는 아래와 같은 소스를 볼 수 있다.(버전마다 결과가 다를 순 있음)

```Kotlin
public final class HelloKotlinKt {
	public static final void main() {
		String var0 = "Hello Kotlin!;
		System.out.println(var0);
	}

	public static void main(String[] var0) {
		main();
	}
}
```
public은 가시성 지시자로 해당 메서드의 접근 방법을 가리킴.
static은 정적 메서드임을 나타냄. 따라서 저억 메모리 영역에 객체가 생성되기에 객체의 생성 없이 호출해서 사용할 수 있음
final은 최종 메서드임을 나타냄

### 프로그램의 메모리 영역

- 코드 영역
	- 명령어
- 데이터 영역
	- 문자열이나 정적 변수
	- JVM에서는 메서드 정적 영역으로도 부름
- 힙
	- 실행 중 생성되는 객체
	- 동적 메모리 영역
- 스택
	- 코드 블록인 중괄호 안에 사용한 변수나 함수 호출 블록
	- 임시로 사용한 변수는 중괄호 블록이 끝나면 제거됨

너무 많은 메모리를 할당하면 OutofMemory 오류가, 함수 호출이 재귀적으로 너무 많이 일어나면 Stack Overflow오류가 발생할 수 있음

JVM을 사용하는 프로그램에는 동적 메모리 영역의 객체가 사용된 뒤 아무 참조가 없으면 자동으로 삭제하는 GC(Garbage Collector)가 있음

### main()메서드에서 매개변수를 사용하는 경우

```Kotlin
fun main(args: Array<String>) {
	println(args[0])
	println(args[1])
	println(args[2])
}
```
자바에서는 String[] args로 변환되어 사용됨
인자 값은 위에서 설명한대로 설정을 해주면 되며, 안했다면 ArrayIndexOutOfBoundsException이 발생함