---
title: "Singleton Pattern"
date: 2020-11-29 16:40:12 +0900
categories: DesignPattern
---

## 싱글턴 패턴

전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴.

즉, 싱글턴 패턴은 단 하나의 인스턴스만 생성하여 사용하는 디자인 패턴입니다.
* 인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용함

생성자가 처음 호출되면 객체를 생성하여 반환하고, 그 이후부터는 생성된 객체를 반환합니다.

### 사용 이유

싱글턴으로 구현한 인스턴스는 전역이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하는 것이 가능합니다.

그리고 고정된 메모리 영역을 얻으면서 한번의 new로 인스턴스를 사용하기 때문에 메모리 낭비를 방지할 수 있습니다.

주로 공통된 객체를 여러 곳에서 사용하는 경우에 많이 사용합니다.
- 로그 관리
- gameManager, audioManager 등 여러 관리용 객체들
- 인스턴스가 단 하나만 존재하는 것을 보증하고 싶은 경우

### 단점

객체 지향 설계 원칙 중 개방-폐쇄 원칙이 있습니다. 싱글턴 인스턴스가 혼자 너무 많은 일을 하거나, 많은 데이터를 공유하는 경우 다른 클래스들과 결합도가 높아지게 되는데, 이때 개방-폐쇄 원칙에 위배됩니다.

```
개방-폐쇄 원칙(open-close principle)

확장에 대해 열려 있고, 수정에 대해 닫혀 있음.
이 설계는 모듈을 확장할 수 있어야 하지만 모듈의 확장이 그 모듈의 소스코드나 바이너리 코드의 변경을 초래하지 않는 설계입니다.
```

또한, 멀티 스레드 환경에서 동기화 처리를 하지 않았을 때, 인스턴스가 2개 이상 생성되는 문제도 발생할 수 있습니다.

따라서 반드시 싱글턴이 필요한 상황이 아니면 지양하는 것이 좋습니다.

### 멀티스레드 환경에서 안전한 싱글턴 만드는 방법

1. Lazy Initialization(초기화 지연)
```java
public class ThreadSafe_Lazy_Initialization {
	private static ThreadSafe_Lazy_Initialization instance;
	private ThreadSafe_Lazy_Initialization() {}

	public static synchronized ThreadSafe_Lazy_Initialization getInstance() {
		if (instance == null)
			instance = new Thread_Safe_Lazy_Initialization();
		return instance;
	}
}
```

private static으로 인스턴스 변수를 만들고 private로 생성자를 만들어 외부에서의 생성을 막습니다.
synchronized 동기화를 활용해 스레들르 안전하게 만들어줍니다.
	- 하지만 synchronized는 큰 성능저하를 발생시키므로 권장하지 않습니다.

2. Lazy Initialization + Double-checked Locking
	- 1번의 성능 저하를 완화시키는 방법

```java
public class ThreadSafe_Lazy_Initialization{
    private volatile static ThreadSafe_Lazy_Initialization instance;

    private ThreadSafe_Lazy_Initialization(){}

    public static ThreadSafe_Lazy_Initialization getInstance(){
    	if(instance == null) {
        	synchronized (ThreadSafe_Lazy_Initialization.class){
                if(instance == null){
                    instance = new ThreadSafe_Lazy_Initialization();
                }
            }
        }
        return instance;
    }
}
```

1번과는 달리, 먼저 조건문으로 인스턴스의 존재 여부를 확인한 다음 두 번째 조건문에서 synchronized를 통해 동기화를 시켜 인스턴스를 생성합니다.

스레드를 안전하게 만들면서, 처음 생성 이후로는 synchronizied를 실행시키지 않기 때문에 성능저하 완화가 가능합니다. 하지만 완벽한 방법은 아닙니다.

3. Initialization on demand holder idiom (holder에 의한 초기화)

클래스 안에 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 로드되는 시점을 이용한 방법

```java
public class Something {
    private Something() {
    }
 
    private static class LazyHolder {
        public static final Something INSTANCE = new Something();
    }
 
    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

2번처럼 동기화를 사용하지 않는 방법을 안하는 이유는, 개발자가 직접 동기화 문제에 대한 코드를 작성하면서 회피하려고 하면 프로그램 구조가 그만큼 복잡해지고 비용 문제가 발생할 수 있습니다. 또한 코드 자체가 정확하지 못할 때가 많습니다.

이 때문에, 3번과 같은 방식으로 JVM의 클래스 초기화 과정에서 보장되는 원자적 특성을 이용해 싱글톤의 초기화 문제에 대한 책임을 JVM에게 떠넘기는 걸 활용합니다.

클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩 시점에서 한번만 호출됩니다. 또한 final을 사용해서 다시 값이 할당되지 않도록 만드는 방식을 사용한 것입니다.

실제로 일반적으로 많이 사용되는 방식이 3번입니다.
