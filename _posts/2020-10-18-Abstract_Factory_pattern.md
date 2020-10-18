---
title: "추상 팩토리 패턴"
date: 2020-10-18 16:21:11 +0900
---

## 추상 팩토리 패턴이란

구체적인 클래스에 의존하지 않고 서로 연관되거나 의존적인 객체들의 조합을 만드는 인터페이스를 제공하는 패턴

즉, 관련성 있는 여러 종류의 객체를 일관된 방식으로 생성하는 경우에 유용하다

## 예시

네모 모양의 버튼, 프로그래스 바, 텍스트 박스와 별 모양의 버튼, 프로그래스 바, 텍스트 박스가 있다. 버튼, 프로그래스 바, 텍스트 박스의 기능이 동일하며 다른 모양이 추가될 수 있다. 어떻게 구성을 하는게 좋을까?

### 일반적인 팩토리 패턴을 사용한 경우

```cpp
class CGeneralButton
{
    ...
};

class CRectButton : public CGeneralButton
{
    ...
};

class CStarButton : public CGeneralButton
{
    ...
};
```

프로그래스바, 텍스트 박스 역시 마찬가지로 구성된다.

|  | 버튼 | 텍스트 박스 | 프로그래스 바 |
|--|:--:|:--:|:--:|
| 공통 | CGeneralButton | CGeneralTextBox | CGeneralProgressBar |
| Rect | CRectButton | CRectTextBox | CRectProgressBar |
| Star | CStarButton | CStarTextBox | CStarProgressBar |

객체 생성시 연관성을 줄이기 위해 각 컨트롤마다 팩토리를 구성

```c++
typedef enum
{
  eUI_RECT,
  eUI_STAR
} EUITYPE;

class CButtonFactory
{
public:
    static CGeneralButton* Create(EUITYPE a_eType)
    {
        switch(a_eType)
        {
        case eUI_RECT:
            return (new CRectButton);
        case eUI_STAR:
            return (new CStarButton);
        }
        return (NULL);
    }
}
```

팩토리를 사용하여 얻는 이점은 구체적인 생성을 팩토리 내부로 캡슐화 하는 것이다. 하지만 위의 경우에서 컨트롤이 확장된다면 Client가 알아야 할 팩토리의 수가 점점 증가한다. 이런 경우 관련성 있는 객체들의 집합을 생성하는 방향으로 팩토리를 생성하면 복잡도를 줄일 수 있다.

### 추상 팩토리 패턴을 사용한 경우

팩토리를 컨트롤 중심이 아닌 스타일 중심으로 생성한다는 것이 주요 변경사항이다. 각 스타일에 해당하는 컨트롤별로 버튼, 텍스트 박스, 프로그래스 바가 존재하므로 각각을 생성할 수 있는 인터페이스를 작성한다.

```cpp
class IUIControlFactory
{
    public:
        virtual CGeneralButton* CreateButton() = 0;
        virtual CGeneralTextBox* CreateTextBox() = 0;
        virtual CGeneralProgressBar* CreateProgressBar() = 0;
}
```

이후 스타일 별 팩토리를 만든다.

```cpp
class CRectControlFactory : public IUIControlFactory
{
    public:
        virtual CGeneralButton* CreateButton()
        { return (new CRectButton); }
        virtual CGeneralTextBar* CreateTextBar()
        { return (new CRectTextBar); }
        virtual CGeneralProgressBar* CreateProgressBar()
        { return (new CRectProgressBar); }
}

class CStarControlFactory : public IUIControlFactory
{
    public:
        virtual CGeneralButton* CreateButton()
        { return (new CStarButton); }
        virtual CGeneralTextBar* CreateTextBar()
        { return (new CStarTextBar); }
        virtual CGeneralProgressBar* CreateProgressBar()
        { return (new CStarProgressBar); }
}
```

스타일별로 팩토리가 있기에 a_eType이 필요하지 않다.

## 정리

추상 팩토리 패턴은 관련성을 가지는 객체의 집합을 생성하는 인터페이스를 제공하는 것이 목적이다. 하지만 팩토리가 무분별하게 많아진다면 그 자체로도 코드의 이해도는 떨어질 수 있기에 사용에 주의해야 한다.

## Reference

<https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html>

문우식, 패턴 그리고 객체지향적 코딩의 법칙, 한빛 미디어