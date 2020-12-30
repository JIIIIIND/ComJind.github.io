---
title: "소프트웨어 공학 Chapter07"
date: 2020-12-25 22:23:14 +0900
---

## Design and implementation

### Design and implementation

- 소프트웨어 설계 및 구현은 실행 가능한 소프트웨어 시스템이 개발되는 소프트웨어 엔지니어링 프로세스의 단계입니다.
- 소프트웨어 설계 및 구현 활동은 항상 상호 배치됩니다.
	- 소프트웨어 디자인은 고객의 요구 사항에 따라 소프트웨어 구성 요소와 그 관계를 식별하는 창의적인 활동입니다.
	- 구현은 디자인을 프로그램으로 실현하는 과정입니다.

### Build or buy

- 이제 광범위한 도메인에서 사용자의 요구 사항에 맞게 조정하고 조정할 수 있는 기성 시스템을 구입할 수 있습니다.
	- 예를 들어, 의료 기록 시스템을 구현하려는 경우 병원에서 이미 사용중인 패키지를 구입할 수 있습니다. 기존 프로그래밍 언어로 시스템을 개발하는 것보다 이 접근 방식을 사용하는 것이 더 저렵하고 빠를 수 있습니다.
- 이러한 방식으로 응용 프로그램을 개발할 때 설계 프로세스는 해당 시스템의 구성 기능을 사용하여 시스템 요구 사항을 제공하는 방법과 관련이 있습니다.

## Object-oriented design using the UML

### Process Stage

- 프로세스를 사용하는 조직에 따라 달라지는 다양한 객체 지향 디자인 프로세스가 있습니다.
- 이러한 프로세스의 일반적인 활동은 다음과 같습니다.
	- 시스템과의 컨텍스트 및 외부 상호 작용을 정의합니다.
	- 시스템 아키텍처를 설계합니다.
	- 주요 시스템 개체를 식별합니다.
	- 디자인 모델 개발
	- 개체 인터페이스를 지정합니다.

### 시스템 컨텍스트 및 상호 작용

설계중인 소프트웨어와 외부 호나경 간의 관계를 이해하는 것은 필요한 시스템 기능을 제공하는 방법과 해당 환경과 통신하도록 시스템을 구성하는 방법을 결정하는 데 필수적입니다.

컨텍스트를 이해하면 시스템의 경계를 설정할 수도 있습니다. 시스템 경계를 설정하면 설계중인 시스템에 구현되는 기능과 다른 관련 시스템에 있는 기능을 결정하는 데 도움이 됩니다.

### Context and interaction models

Context model과 interaction model은 시스템과 외부환경의 관계에 대해 상호 보완적인 관점을 보여줌

- 시스템 컨텍스트 모델은 개발중인 시스템 환경의 다른 시스템을 보여주는 구조 모델입니다.
	- System context model은 개발되는 시스템의 환경에 있는 다른 시스템들을 보여주는 구조 모델
- 상호 작용 모델은 시스템이 사용되는 동안 환경과 상호 작용하는 방식을 보여주는 동적 모델입니다.
	- 상호작용 모델은 시스템이 사용될 때 어떻게 그 환경과 상호 작용하는지 보여주는 동적 모델

### Architectural design

- 시스템과 환경 간의 상호 작용이 이해되면 이 정보를 사용하여 시스템 아키텍처를 설계합니다.
- 시스템과 그 상호 작용을 구성하는 주요 구성 요소를 식별 한 다음 계층형 또는 클라이언트-서버 모델과 같은 아키텍처 패턴을 사용하여 구성 요소를 구성 할 수 있습니다.

### Object class identification

- 객체 클래스를 식별하는 것은 객체 지향 설계에서 종종 어려운 부분입니다.
- 객체 식별을 위한 마법의 공식은 없습니다. 시스템 설계자의 기술, 경험 및 도메인 지식에 의존합니다.
- 개체 식별은 반복적인 프로세스입니다. 처음에는 제대로 할 수 없을 것입니다.

### Approches to identification

- 시스템의 자연어 설명을 기반으로 한 문법적 접근 방식을 사용합니다.
	- 명사: 객체와 속성
	- 동사: 작업 또는 서비스
- 애플리케이션 도메인에서 유형의 사물을 기반으로 식별합니다.
- 시나리오 기반 분석을 사용합니다. 각 시나리오의 개체, 속성 및 방법이 식별됩니다.

### Design models

- 디자인 모델은 개체와 개체 클래스 및 이러한 개체 간의 관계를 보여줍니다.
- 두 종류의 디자인 모델이 있습니다.
	- 구조 모델은 개체 클래스 및 관계 측면에서 시스템의 정적 구조를 설명합니다.
	- 동적 모델은 개체 간의 동적 상호 작용을 설명합니다.

### 디자인 모델의 예

- 개체의 논리적 그룹을 일관된 하위 시스템으로 보여주는 하위 시스템 모델
- 개체 상호 작용의 순서를 보여주는 순서 모델
- 개별 개체가 이벤트에 대한 응답으로 상태를 변경하는 방법을 보여주는 상태 시스템 모델
- 기타 모델에는 사용 사례 모델 및 아키텍처 모델이 포함됩니다.

#### Subsystem models

- 디자인이 논리적으로 관련된 개체 그룹으로 구성되는 방법을 보여줍니다.
	- 클래스 다이어그램의 형태를 사용하여 표현
	- 하위 시스템은 개체가 포함된 패키지로 표시됩니다.
- UML에서는 캡슐화 구조인 패키지를 사용하여 표시됩니다. 이것은 논리적 모델입니다. 시스템에서 개체의 실제 구성은 다를 수 있습니다.

#### Sequence models

- 시퀀스 모델은 발생하는 개체 상호 작용의 시퀀스를 보여줍니다.
	- 개체는 상단에 가로로 배열됩니다.
	- 시간은 수직으로 표시되므로 모델은 위에서 아래로 읽습니다.
	- 상호 작용은 레이블이 지정된 화살표로 표시됩니다. 서로 다른 스타일의 화살표는 서로 다른 유형의 상호 작용을 나타냅니다.
	- 객체 라이프 라인의 얇은 직사각형은 객체가 시스템에서 제어 객체인 시간을 나타냅니다.

#### State diagrams

- 상태 다이어그램은 개체가 서로 다른 서비스 요청에 응답하는 방법과 이러한 요청에 의해 트리거 된 상태 전환을 보여주는 데 사용됩니다.
- 상태 다이어그램은 시스템 또는 개체의 런타임 동작에 대한 유용한 고수준 모델입니다.
- 일반적으로 시스템의 모든 개체에 대한 상태 다이어그램이 필요하지는 않습니다. 시스템의 많은 객체는 상대적으로 단순하며 상태 모델은 설계에 불필요한 세부 사항을 추가합니다.

#### Interface specification

- 개체 및 기타 구성 요소를 병렬로 설계할 수 있도록 개체 인터페이스를 지정해야 합니다.
- 디자이너는 인터페이스 디자인에서 데이터 표현을 디자인하지 않아야하지만 개체 자체에서 이를 숨겨야 합니다(즉, 데이터 액세스 및 업데이트 작업 포함).
- 객체는 제공된 메소드에 대한 관점인 여러 인터페이스를 가질 수 있습니다.
- UML은 인터페이스 사양을 위해 클래스 다이어그램을 사용합니다.

## Design Patterns

### Design patterns

- 패턴은 문제에 대한 설명과 솔루션의 본질입니다.
- 디자인 패턴은 문제와 그 해결책에 대한 추상적인 지식을 재사용하는 방법입니다.
- 다른 설정에서 재사용 할 수 있을만큼 충분히 추상적이어야 합니다.
- 패턴 설명은 일반적으로 상속 및 다형성과 같은 객체 지향 특성을 사용합니다.

디자인패턴은 상속, 다형성과 같은 Object-oriented 특성을 활용합니다.

#### Factory method 패턴

- 상위 클래스에서 객체를 생성하는 인터페이스를 정의하고, 하위 클래스에서 인스턴스를 생성하도록 하는 방식
- 객체를 생성하는 시점은 알지만 어떤 객체를 생성해야 할지 알 수 없을 때, 객체 생성을 하위 클래스에 위임하여 해결
	- 어떤 클래스의 인스턴스를 생성할지에 대한 결정이 서브클래스에서 이루어짐

#### Singleton 패턴

- 특정 클래스의 객체가 오직 한 개만 존재하도록 보장, 즉 클래스의 객체를 하나로 제한
- 동일한 자원이나 데이터를 처리하는 객체가 불필요하게 여러 개 만들어질 필요가 없는 경우에 주로 사용

```java
public class Singleton {
	private static Singleton instance;
	private Singleton() {...}
	public static Singleton getInstance() {
		if (instance == null)
			instance = new Singleton();
		return instance;
	}
}
```

#### Prototype 패턴

- 생산 비용이 높은 인스턴스를 사용할 때 다시 생성하지 않고 복사해서 사용
	- 인스턴스를 생성하기 위해 원격에 있는 데이터를 여러 번 접근해야 할 때
	- 인스턴스 생성을 위해 필요한 정보를 일일이 기입해야 할 때
- new Object() 보다 clone()을 이용해 기존의 것을 복사하여 일부만 바꿔 인스턴스 생성
- 일반적인 prototype(원형)을 만들어놓고, 그것을 복사한 후 필요한 부분만 수정하여 사용

```java
public class Main {
	public static void main(String[] args) {
		Circle circle1 = new Circle(2, 2, 3);
		Circle circle2 = circle1.copy();
	}
}
//Java에서는 Cloneable Interface를 상속받으면, Object class 내 clone method 사용 가능
public class Shape implements Cloneable {
	private int id;
	public Shape(int id) {
		this.id = id;
	}
}

public class Circle extends Shape {
	private int x, y, radius;
	public Cicle(int x, int y, int radius) {
		this.x = x;
		this.y = y;
		this.radius = radius;
	}

	public Circle copy() {
		Circle circle = (Circle) clone();
		return circle;
	}
}
```

#### Abstract Factory 패턴

- 여러 개의 concrete Product를 추상화시킨 것
- 구체적인 구현은 concreteProduct 클래스에서 이루어짐
- 사용자에게 api를 제공하고, 인터페이스만 사용해서 부품을 조립
- 즉 추상적인 부품을 조합해서 추상적인 제품을 만들어냄

```java
public interface BikeFactory {
	public Body createBody();
	public Wheel createWheel();
}

public interface Body {
}

public interface Wheel {
}

public class SamBikeFactory implements BikeFactory {
	public Body createBody() {
		return new SamBody();
	}

	public Wheel createWheel() {
		return new SamWheel();
	}
}

public class SamBody implements Body {
}

public class SamWheel implements Wheel {
}

public class GtFactory implements BikeFactory {
	public Body createBody() {
		return new GtBody();
	}
	public Wheel createWheel() {
		return new GtWheel();
	}
}

public class GtBody implements Body {
}

public class GtWheel implements Wheel {
}

public class Main {
	public static void main(String args[]) {
		BikeFactory factory = new SamBikeFactory();
		// GtFactory factory = new GtBikeFactory();

		Body body = factory.createBody();
		Wheel wheel = factory.createWheel();

		System.out.println(body.getClass());
		System.out.println(wheel.getClass());
	}
}
```

#### Composite 패턴

- composite object: 부분-전체의 상속 구조로 표현되는 조립 객체
- 사용자가 단일 객체와 복합 객체 모두 동일하게 다루도록 한 것
- 재귀적 구조
	- 디렉토리 안에 파일 또는 다른 디렉토리(서브 디렉토리)가 존재할 수 있는 것
- 그릇(디렉토리)과 내용물(파일)을 동일시해서 재귀적인 구조를 만들기 위한 설계 패턴

```java
public abstract class Directory {
	public void open() {...}
	public void close() {...}
}

public class File extends Directory {
	private Object data;
	public String getData() { return data; }
	public void setData(Obj;ect data) { this.data = data; }
}

public class SubDirectory extends Directory {
	List<Directory> children = new ArrayList<>();

	public boolean moveDirectory(Directory directory) {
		return children.add(directory);
	}

	public boolean removeDirectory(Directory directory) {
		return children.remove(directory);
	}
}
```

#### Adapter 패턴

- 기존 클래스를 재사용할 수 있도록 중간에서 맞춰주는 역할
- 호환성이 없는 기존 클래스의 인터페이스를 변환해 재사용할 수 있도록 해준다.

#### Decorator 패턴

- 기존에 구현되어 있는 클래스에 그때그때 필요한 기능을 추가해나가는 설계 패턴
- 기능확장이 필요할 때 상속의 대안으로 사용

```java
public interface VisualComponent {
	public void draw();
}

public class TextView implements VisualComponent {
	public void draw() {
		//draw TextView
	}
}

public class Decorator implements VisualComponent {
	VisualComponent component;
	Decorator(VisualComponent component) {
		super();
		this.component = component;
	}
	public void draw() {
		component.draw();
	}
}

public class BorderDecorator extends Decorator {
	BorderDecorator(VisualComponent component) {
		super(component);
	}
	public void draw() {
		super.draw();
		//draw the function of BorderDecorator
		drawBorderDecorator();
	}
	public void BorderDecorator() {...}
}

public class Main {
	public static void main(String args[]) {
		VisualComponent view = new TextView();
		view = new BorderDecorator(view);
		view.draw();
		view = new ScrollDecorator(view);
		view.draw();
	}
}
```

#### Facade 패턴

- 몇 개의 클라이언트 클래스와 서브시스템의 클라이언트 사이에 facade라는 객체를 세워놓음으로써 복잡한 관계를 정리한 것
- 모든 관계가 전면에 세워진 facade 객체를 통해서만 이루어질 수 있게 단순한 인터페이스를 제공하는 것

#### Proxy 패턴

- 시간이 많이 소요되는 불필요한 복잡한 인스턴스를 생성하는 시간을 간단한 객체로 줄임
- 그림과 텍스트가 섞여있는 경우 텍스트가 먼저 나오고 나중에 그림이 나올 수 있도록 하기 위해 텍스트 처리용 프로세스, 그림 처리용 프로세스를 별도로 두고 운영하는 것 같은 설계

#### Observer 패턴

- 한 인스턴스에 의하여 영향을 받는 인스턴스들을 정리하는데 사용
- 어떤 클래스에 변화가 일어났을 때, 이를 감지하여 다른 클래스에 통보해주는 것

예시

1. 클라이언트가 Observer들에게 상태 변화의 통지를 요청
	- 클라이언트 인스턴스가 Source 인스턴스의 notify() 메소드를 호출
2. notify() 메서드는 집합 관계에 있는 각각의 Observer 인스턴스 안의 update() 메소드를 호출
3. update() 메소드는 Source의 Observer로 연결된 ConcreteObserver들마다 호출
	- update() 메소드 안에서는 Source 인스턴스 상태를 받아서 그래프를 그린다던지 또는 변경된 상태를 이용하여 다른 작업을 함

```java
interface IObserver{
	public void notify(Object data);
}

class Observable{
	private List<IObserver> observers = new ArrayList<IObserver>();
	public void registObserver(IObserver observer) { observers.add(observer); }

	String[] table = {"a", "b", "c", "d", "e"};
	public void update(String data, int index) {
		table[index] = data;
		onUpdate();
	}
	public void onUpdate() {
		for(IObserver observer : observers) {
			observer.notify(table)
		}
	}
}

class Graph implements IObserver {
	public Graph(Observable observable) { observable.registObserver(this); }
	public void notify(Object data) {
		String[] table = (String[])data;
		System.out.println("Graph: ");
		for(int i = 0; i < 5; i++) System.out.println(table[i]);
	}
}

class Display implements IObserver{
	public Display(Observable observable) { observable.registObserver(this); }
	public void notify(Object data) {
		String[] table = (String[])data;
		System.out.println("Display : ");
		for(int i = 0; i < 5; i++) System.out.println(table[i]);
	}
}
```

#### Chain of responsibility 패턴

- 책임들이 연결되어 있어 내가 책임을 못 질 것 같으면 다음 책임자에게 자동으로 넘어가는 구조
- 소프트웨어 개발에서도 이렇게 자동으로 연결되는 구조의 구현은 작업을 수행하는 인스턴스를 체인처럼 엮어 놓았다가 외부의 요청이 있으면 차례대로 넘김

```java
public class Main {
	public static void main(String args[]) {
		Calculator plus = new PlusCalculator();
		Calculator sub = new SubCalculator();

		plus.setNextCalculator(sub);

		Request request1 = new Request(2, 3, "+");
		Request request2 = new Request(4, 2, "-");

		plus.process(request1);
		plus.process(request2);
	}
}

public abstract class Calculator {
	Calculator nextCalculator;
	public void setNextCalculator (Calculator nextCalculator) {
		this.nextCalculator = nextCalculator;
	}
	public boolean process(Request request) {
		if (operator(request)) {
			return true;
		} else {
			if (nextCalculator != null)
				return nextCalculator.process(request);
		}
		return false;
	}
	protected abstract boolean operator(Request request);
}

public class Request {
	private int a, b;
	private String operator;
	public Request(int a, int b, int operator) {
		super();
		this.a = a;
		this.b = b;
		this.operator = operator;
	}

	public int getA() { return a; }
	public int getB() { return b; }
	public String getOperator() { return operator; }
}

public class PlusCalculator extends Calculator {
	protected boolean operator(Request request) {
		if (request.getOperator().equals("+")) {
			int a = request.getA();
			int b = request.getB();
			int result = a + b;
			System.out.println(a + " + " + b + " = " + result);
			return true;
		}
		return false;
	}
}

public class SubCalculator extends Calculator {
	protected boolean operator(Request request) {
		if (request.getOperator().equals("-")) {
			int a = request.getA();
			int b = request.getB();
			int result = a - b;
			System.out.println(a + " - " + b + " = " + result);
			return true;
		}
		return false;
	}
}
```

#### Command 패턴

- 명령들을 하나의 객체로 표현
- 명령어를 각각 구현하는 것보다는 하나의 추상 클래스에 execute() 메서드를 하나 만들고 각 명령이 들어오면 그에 맞는 서브 클래스가 선택되어 실행하는 것이 효율적
- 문서 편집기의 복사, 붙여넣기, 잘라내기 등
- 단순히 명령어를 추상 클래스와 구체 클래스로 분리하여 단순화한 것으로 끝나지 않고, 명령어에 따른 취소 기능까지 포함
	- 이유: 사용자 입장에서도 해당 명령어를 실행했다가 취소하기도 하기 때문
	- 이렇게 프로그램의 명령어를 구현할 때 command 패턴을 활용

#### State 패턴

- 동일한 동작을 객체 상태에 따라 다르게 처리해야 할 때 사용
- 객체 상태를 클래스화함으로써 상태를 객체로 표현하고, 각 상태에 따라 행동을 다르게 처리(upState, stopState, downState) 할 수 있도록 한 것
- 변경 시 원시 코드의 수정을 최소화할 수 있고, 유지보수를 쉽게 할 수 있다.

```java
public class Main {
	public static void main(String[] args) {
		Light light = new Light();
		light.off();
		lifht.off();
		light.off();
		light.on();
		light.on();
		light.on();
		light.off();
		light.on();
		light.off();
		light.on();
		light.off();
		light.on();
	}
}

public class Light {
	protected LightState lightState;
	private LightState offState = new LightState() {
		public void on() {
			System.out.println("Light ON");
			lightState = onState;
		}
		public void off() { System.out.println("Nothing"); }
	};
	private LightState onState = new LightState() {
		public void on() { System.out.println("Nothing"); }
		public void off() {
			System.out.println("Light OFF");
			lightState = offState;
		}
	};

	public Light() {
		lightState = offState;
	}
	public void on() { lightState.on(); }
	public void off() { lightState.off(); }
}

public interface LightState {
	public void on();
	public void off();
}
```

## Implementation Issue

### Implementation Issue

여기서 초점은 프로그래밍이 아니라 분명히 중요하지만 프로그래밍 텍스트에서 다루지 않는 다른 구현 문제에 있습니다.
- 재사용
	- 대부분의 최신 소프트웨어는 기존 구성 요소 또는 시스템을 재사용하여 구성됩니다. 소프트웨어를 개발할 때는 기존 코드를 최대한 활용해야 합니다.
- 구성 관리
	- 개발 프로세스 중에 구성 관리 시스템에 있는 각 소프트웨어 구성 요소의 다양한 버전을 추적해야 합니다.
- 호스트 대상 개발
	- 소프트웨어는 일반적으로 소프트웨어 개발 환경과 동일한 컴퓨터에서 실행되지 않습니다. 대신 한 컴퓨터에서 개발하고 별도의 컴퓨터에서 실행합니다.

### Reuse

- 1960년대부터 1990년대까지 대부분의 새로운 소프트웨어는 모든 코드를 고급 프로그래밍 언어로 작성하여 처음부터 개발되었습니다.
	- 중요한 재사용 또는 소프트웨어는 프로그래밍 언어 라이브러리에서 함수와 객체를 재사용하는 것뿐이었습니다.
- 비용 및 일정 압박으로 인해 특히 상용 및 인터넷 기반 시스템에서 이 접근 방식이 점점 더 어려워졌습니다.
- 기존 소프트웨어의 재사용을 기반으로 한 개발 접근 방식이 등장했으며 이제는 일반적으로 비즈니스 및 과학 소프트웨어에 사용됩니다.

#### Reuse Levels

- 추상화 수준
	- 이 수준에서는 소프트웨어를 직접 재사용하지 않고 성공적인 추상화 지식을 소프트웨어 설계에 사용합니다.
- 개체 수준
	- 이 수준에서는 코드를 직접 작성하는 대신 라이브러리의 개체를 직접 재사용합니다.
- 구성 요소 수준
	- 구성 요소는 응용 프로그램 시스템에서 재사용하는 개체 및 개체 클래스의 모임입니다.
- 시스템 수준
	- 이 수준에서는 전체 응용 프로그램 시스템을 재사용합니다.

#### Reuse costs

소프트웨어 재사용 시 새로운 소프트웨어보다 신뢰도가 높으나, 재사용에 따른 비용이 발생함

- 재사용 할 소프트웨어를 찾고 요구사항을 충족하는지 평가하는 데 소요되는 시간 비용
- 해당되는 경우, 재사용 가능한 소프트웨어 구매 비용. 대형 기성품 시스템의 경우 이러한 비용이 매우 높을 수 있습니다.
- 개발 중인 시스템의 요구 사항을 반영하기 위해 재사용 가능한 소프트웨어 구성 요소 또는 시스템을 조정하고 구성하는 비용
- 재사용 가능한 소프트웨어 요소를 서로 통합하고 개발한 새 코드를 통합하는 비용

### Configuration management

- 구성 관리는 변화하는 소프트웨어 시스템을 관리하는 일반적인 프로세스에 부여 된 이름입니다.
- 구성 관리의 목표는 모든 개발자가 제어 된 방식으로 프로젝트 코드 및 문서에 액세스하고 변경 사항을 확인하고 구성 요소를 컴파일 및 링크하여 시스템을 만들 수 있도록 시스템 통합 프로세스를 지원하는 것입니다.

#### Configuration management activities

- 버전 관리
	- 소프트웨어 구성 요소의 여러 버전을 추적하기 위한 지원이 제공됩니다. 버전 관리 시스템에는 여러 프로그래머의 개발을 조정하는 기능이 포함되어 있습니다.
- 시스템 통합
	- 개발자가 각 시스템 버전을 만드는 데 사용되는 구성 요소 버전을 정의하는 데 도움이 되는 지원이 제공됩니다. 이 설명은 필요한 구성 요소를 컴파일하고 연결하여 시스템을 자동으로 구축하는데 사용됩니다.
- 문제 추적
	- 사용자가 버그 및 기타 문제를 보고 할 수 있도록 지원하고 모든 개발자가 이러한 문제를 해결하고 있는 사람과 수정 시기를 확인할 수 있도록 지원합니다.

### Host-target development

- 대부분의 소프트웨어는 하나의 컴퓨터에서 개발되지만 별도의 컴퓨터에서 실행됩니다.
- 일반적으로 개발 플랫폼과 실행 플랫폼에 대해 이야기 할 수 있습니다.
	- 플랫폼은 단순한 하드웨어 그 이상입니다.
	- 여기에는 설치된 운영 체제와 데이터베이스 관리 시스템 또는 개발 플랫폼의 경우 대화형 개발 환경과 같은 기타 지원 소프트웨어가 포함됩니다.
- 개발 플랫폼에는 일반적으로 실행 플랫폼과 다른 설치된 소프트웨어가 있습니다. 이러한 플랫폼은 다른 아키텍처를 가질 수 있습니다.

### Development platform tools

- 코드를 생성, 편집 및 컴파일 할 수 있는 통합 컴파일러 및 구문 지향 편집 시스템
- 언어 디버깅 시스템
- UML 모델을 편집하는 도구와 같은 그래픽 편집 도구
- 프로그램의 새 버전에서 일련의 테스트를 자동으로 실행할 수 있는 JUnit과 같은 테스트 도구
- 다양한 개발 프로젝트에 대한 코드를 구성하는 데 도움이 되는 프로젝트 지원 도구

#### Integrated development environments(IDE)

- 소프트웨어 개발 도구는 종종 통합 개발 환경을 만들기 위해 그룹화됩니다.
- IDE는 일부 공통 프레임 워크 및 사용자 인터페이스 내에서 소프트웨어 개발의 다양한 측면을 지원하는 소프트웨어 도구 세트입니다.
- IDE는 Java와 같은 특정 프로그래밍 언어로 개발을 지원하기 위해 만들어집니다. 언어 IDE는 특별히 개발되거나 특정 언어 지원 도구를 사용하여 범용 IDE의 인스턴스 화일 수 있습니다.

### Component/System deployment factors

- 구성 요소가 특정 하드웨어 아키텍처 용으로 설계되었거나 다른 소프트웨어 시스템에 의존하는 경우 반드시 필요한 하드웨어 및 소프트웨어 지원을 제공하는 플랫폼에 배포해야 합니다.
- 고 가용성 시스템은 둘 이상의 플랫폼에 구성 요소를 배포해야 할 수 있습니다. 즉, 플랫폼 장애시 구성 요소의 대체 구현을 사용할 수 있습니다.
- 구성 요소간에 높은 수준의 통신 트래픽이 있는 경우 일반적으로 동일한 플랫폼 또는 물리적으로 서로 가까운 플랫폼에 배포하는 것이 좋습니다. 이렇게 하면 한 구성 요소에서 메시지를 보내고 다른 구성요소에서 메시지를 받는 시간 사이의 지연이 줄어듭니다.

## Open source development

### Open source development

- 오픈 소스 개발은 소프트웨어 시스템의 소스 코드가 공개되고 자원 봉사자가 개발 프로세스에 참여하도록 초대되는 소프트웨어 개발 접근 방식입니다.
- 그 뿌리는 자유 소프트웨어 재단에 있으며, 소스코드는 독점이 아니라 사용자가 원하는대로 검토하고 수정할 수 있도록 항상 제공되어야 한다고 주장합니다.
- 오픈 소스 소프트웨어는 인터넷을 사용하여 훨씬 더 많은 자원 개발자를 모집함으로써 이 아이디어를 확장했습니다. 그들 중 다수는 코드 사용자이기도 합니다.

### Open source systems

- 가장 잘 알려진 오픈 소스 제품은 물론 서버 시스템 및 데스크톱 환경으로 널리 사용되는 Linux입니다.
- 기타 중요한 오픈 소스 제품으로는 Java, Apache 웹 서버 및 mySQL 데이터베이스 관리 시스템이 있습니다.

### Open source issues

- 개발중인 제품이 오픈 소스 구성 요소를 사용해야 합니까?
- 소프트웨어 개발에 오픈 소스 접근 방식을 사용해야 합니까?

### Open source business

- 점점 더 많은 제품 회사가 개발에 오픈 소스 접근 방식을 사용하고 있습니다.
- 그들의 비즈네스 모델은 소프트웨어 제품 판매에 의존하지 않고 해당 제품에 대한 지원 판매에 의존합니다.
- 그들은 오픈 소스 커뮤니티를 포함하면 소프트웨어를 더 저렴하고 빠르게 개발할 수 있으며 소프트웨어 사용자 커뮤니티를 만들 수 있다고 믿습니다.

### Open source licensing

- 오픈 소스 개발의 기본 원칙은 소스 코드를 자유롭게 사용할 수 있어야 한다는 것입니다. 그렇다고 누구나 해당 코드로 원하는대로 할 수 있다는 의미는 아닙니다.
	- 법적으로 코드 개발자가 여전히 코드를 소유합니다. 오픈 소스 소프트웨어 라이선스에 법적 구속력이 있는 조건을 포함하여 사용방법을 제한할 수 있습니다.
	- 일부 오픈 소스 개발자는 새로운 시스템을 개발하는 데 오픈 소스 구성 요소를 사용하는 경우 해당 시스템도 오픈 소스여야 한다고 생각합니다.
	- 다른 사람들은 이렇나 제한없이 자신의 코드를 기꺼이 사용할 수 있습니다. 개발된 시스템은 독점적이며 폐쇄 소스 시스템으로 판매될 수 있습니다.

### License models

- GNU 일반 공중 사용 허가서(GPL)
	- 이것은 소위 상호 라이선스로, GPL 라이선스에 따라 라이선스가 부여된 오픈 소스 소프트웨어를 사용하는 경우 해당 소프트웨어를 오픈 소스로 만들어야 함을 의미합니다.
- GNU Lesser General Public License (LGPL)
	- 이러한 구성 요소의 소스를 게시하지 않고도 오픈 소스 코드에 연결되는 구성 요소를 작성할 수 있는 GPL 라이선스의 변형입니다.
- BSD(Berkley Standard Distribution)
	- 이것은 비 상호 라이선스이므로 오픈 소스 코드에 대한 변경 사항이나 수정 사항을 다시 게시 할 의무가 없습니다. 판매되는 독점 시스템에 코드를 포함 할 수 있습니다.

#### GPL

- 어떤 프로그램을 개발할 때, GPL 코드를 일부라도 사용하면, 그 프로그램은 GPL
- GPL을 가진 프로그램을 유료로 판매하는 것은 가능하지만, 반드시 전체 소스 코드는 무료로 공개해야 함
- GPL 코드를 사용한 SW를 내부적인 목적으로만 사용할 때에는 소스코드를 공개할 필요가 없음
- 어떤 형태로든 외부에 공표/배포할 때에는 전체 소스코드를 공개해야 함

#### LGPL

- LGPL 코드를 정적 또는 동적 라이브러리로 사용한 프로그램을 개발하여 판매/배포할 경우에 프로그램의 소스코드를 공개하지 않아도 되나, LGPL 코드의 사용을 명시함
- LGPL 코드를 단순히 이용하는 것이 아니라 이를 수정하거나 파생된 라이브러리를 개발하여 배포하는 경우에는 전체 코드를 공개해야 함

#### BSD

- 소스코드 공개의 의무가 없으며 상용 소프트웨어에서도 무제한 사용 가능한 라이선스
- 대표적인 프로그램으로 OpenCV가 있음
