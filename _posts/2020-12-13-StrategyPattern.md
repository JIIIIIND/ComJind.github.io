---
title: "Strategy Pattern"
date: 2020-12-13 11:15:12 +0900
---

## Strategy Pattern

객체들이 할 수 있는 행위 각각에 대해 전략 클래스를 생성하고, 유사한 행위들을 캡슐화하는 인터페이스를 정의하며, 객체의 행위를 동적으로 바꾸고 싶은 경우 직접 행위들을 수정하지 않고 전략을 바꿔주기만 함으로써 행위를 유연하게 확장하는 방법

### 사용 이유

Train과 Bus 클래스가 있고, Movable 인터페이스를 구현했다고 가정합니다.
그리고 Train과 Bus 객체를 사용하는 Client도 있습니다.

위의 구조를 코드로 표현하면 다음과 같습니다.

```java
public interface Movable {
	public void move();
}

public class Train implements Movable {
	public void move() {
		System.out.println("선로로 이동");
	}
}

public class Bus implements Movable {
	public void move() {
		System.out.println("도로로 이동");
	}
}

public class Client {
	public static void main(String args[]) {
		Movable train = new Train();
		Movable bus = new Bus();

		train.move();
		bus.move();
	}
}
```

시간이 지나 선로로 이동하는 버스가 개발되었을 때 다음과 같이 Bus의 move() 메서드를 변경할 수 있습니다.

```java
public void move() {
	System.out.println("선로로 이동");
}
```

위와 같은 방식은 SOLID의 원칙 중 OCP에 위배됩니다. 기존의 move()를 수정하지 않으면서 행위가 수정되어야 하지만, 지금은 Bus의 move를 직접 수정했습니다.
그리고 시스템의 확장에 따라 유지보수가 어려워진다는 문제점도 있습니다.
택시, 자가용, 고속버스, 오토바이 등 다양한 탈것이 추가되었을 때 모두 move()를 사용합니다. 위와 같이 도로/선로로 이동하는 새로운 탈것이 개발되었을 때 move()메서드를 일일이 수정해야 할 뿐 아니라 여러 클래스에서 똑같이 정의하고 있으므로 메서드의 중복이 발생합니다.

### Strategy Pattern 구현

현재 운송수단은 선로를 따라 움직이거나 도로를 따라 움직이는 두 가지 방식이 있습니다. 즉, 움직이는 두 방식에 대해 Strategy 클래스를 생성합니다.
두 클래스는 move()메서드를 구현하여, 어떤 경로로 움직이는지에 대해 구현합니다.

두 전략 클래스를 캡슐화하기 위해 MovableStrategy 인터페이스를 생성합니다. 이렇게 캡슐화를 함으로써 운송수단에 대한 전략 뿐만 아니라 다른 전략(주유 방식)이 추가적으로 확장되는 경우를 고려할 수 있습니다.

```java
public interface MovableStrategy {
	public void main();
}

public class RailLoadStrategy implements MovableStrategy {
	public void move() {
		System.out.println("선로로 이동");
	}
}

public class LoadStrategy implements MovableStrategy {
	public void move() {
		System.out.println("도로로 이동");
	}
}
```

다음으로 운송수단에 대한 클래스를 정의합니다.

기차와 버스 같은 운송 수단은 move()메서드로 움직일 수 있습니다. 하지만 이동 방식을 직접 구현하지 않고, 어떻게 움직일 것인지에 대한 전략을 설정하여, 그 전략의 움직임 방식을 사용하여 움직이도록 합니다.

```java
public class Moving {
	private MovableStrategy movableStrategy;

	public void move() {
		movableStrategy.move();
	}

	public void setMovableStrategy(MovableStrategy movableStrategy) {
		this.movableStrategy = movableStrategy;
	}
}

public class Bus extends Moving {
	...
}

public class Train extends Moving {
	...
}
```

이제 Train과 Bus를 사용하는 Client를 구현합니다.

```java
public class Client {
	public static void main(String args[]) {
		Moving train = new Train();
		Moving bus = new Bus();

		train.setMovableStrategy(new RailLoadStrategy());
		bus.setMovableStrategy(new LoadStrategy());

		train.move();
		bus.move();

		bus.setMovableStrategy(new RailLoadStrategy());
		bus.move();
	}
}
```