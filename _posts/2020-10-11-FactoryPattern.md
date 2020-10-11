---
title: "팩토리 메서드 패턴"
date: 2020-10-11 16:22:24 +0900
categories: DesignPattern
---

# Factory method pattern

상위 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며, 하위 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이기도 함
상위 클래스에 구체 클래스 이름을 감추기 위한 방법으로도 사용한다.

## 예시

기존의 코드로 사각형의 도장을 찍을 수 있다.
삼각형, 원 기능이 추가로 필요하며 도장 기능이 추가 될 수 있다.

### 일반적인 경우

각 Stamp들은 어떤 모양을 그리는지만 다를 뿐이고 동작은 비슷한 것을 알 수 있다.
공통점을 묶어내어 CGeneralStamp를 만들고 다른 점은 원래 클래스에 남겨둔 결과 아래와 같은 코드가 완성 된다.

```cpp
class CGeneralStamp {
	public:
		virtual void Draw() = 0;
	...
};

class CSquareStamp : public CGeneralStamp {
	public:
		virtual void Draw() { ...사각형 코드...}
};

class CTriangleStamp : public CGeneralStamp {
	public:
		virtual void Draw() { ...삼각형 코드...}
};

class CCircleStamp : public CGeneralStamp {
	public:
		virtual void Draw() { ...원 코드...}
};
```

삼각형과 원 기능이 추가되었으므로 도장 기능을 사용하는 모든 곳의 코드가 변경되어야 한다.
어떤 종류의 도장이 선택되었는지를 명확하게 알기 위해 도장의 종류를 의미하는 enum을 추가한다.

```cpp
typedef enum {
	eSTAMP_TRIANGLE,
	eSTAMP_SQUARE,
	eSTAMP_CIRCLE
} ESTAMPTYPE;
```

종류에 따른 도장 객체를 생성하기 위해 도장 객체를 사용하는 모든 부분의 코드가 변경된다.
모든 클라이언트 코드가 모든 도장을 알아야 하기 때문에 연관성이 복잡하게 얽히게 된다.
추가되는 도장 기능이 생기면 위의 도장 코드를 copy&paste하는 방식을 사용한다.

```cpp
switch(eStampType) {
	case eSTAMP_TRIANGLE:
		... = new CTriangleStamp;
		break;
	case eSTAMP_SQUARE:
		... = new CSQUAREStamp;
		break;
	case eSTAMP_CIRCLE:
		... = new CCIRCLEStamp;
		break;
	...
}
```

**문제점**

위의 방식에서 확장성은 해결했지만 해당 클래스를 사용하기 위해서는 도장을 의미하는 모든 클래스를 알아야 한다는 불편함이 남게 된다.
결과적으로 위의 코드는 Stamp와 이 Stamp들을 사용하는 부분은 직접적인 연관성을 가지게 된다.

### Factory 패턴의 사용

위의 코드에선 모든 Stamp 클래스를 직접 생성하지만 팩토리 패턴을 적용한다면 팩토리 클래스가 모든 객체 생성을 대신한다.
매개변수로 어떤 타입의 객체를 생성할지 넘겨주면 팩토리 클래스에서 적당한 클래스를 new로 생성하여 넘겨준다.
이 경우에서 반환 값은 팩토리가 알고 있는 모든 클래스의 부모 타입(여기서는 CGeneralStamp)이 된다.

```cpp
class CStampFactory {
	public:
		static CGeneralStamp* create(ESTAMPTYPE a_eType);
};

CGeneralStamp* CStampFactory::Create(ESTAMPTYPE a_eType) {
	switch (a_eType) {
		case eSTAMP_TRIANGLE: return new CTriangleStamp;
		case eSTAMP_SQUARE: return new CSquareStamp;
		case eSTAMP_CIRCLE: return new CCircleStamp;
	}
	return NULL;
}
```

팩토리 패턴을 사용하여 Stamp 클래스별로 많이 알아야 하는 경우를 줄여준다. 한가지 주의할 점은 모든 클래스가 같은 책임, 즉 도장을 그리는 책임을 가지며 CGeneralStamp에 선언된 Draw()라는 가상함수를 통해 하위 클래스들의 함수를 호출할 수 있다. 

**결론**

클래스 상속 트리가 책임이 명확하고 노출된 함수의 형태가 객체지향 언어가 지원하는 다형성에 의해 잘 동작할 수 있을 때 팩토리 패턴은 진정한 가치를 할 수 있다.
책임도 다르고 노출된 함수 형태도 다른데 억지로 팩토리로 묶는다고 효과를 볼 순 없으니 너무 패턴에 집착하지 않는게 중요함