---
title: "디자인 패턴 분류"
date: 2020-09-27 19:30:27 +0900
---
디자인 패턴은 목적에 따라 생성, 구조, 행위로 나눌 수 있으며, 범위에 따라서는 클래스를 대상으로 하는지, 객체를 대상으로 하는지로 나눌 수 있다.

## 생성 패턴
객체의 생성 방식을 결정
### 클래스 대상
- Factory Method: 서브 클래스에 인스턴스 결정 및 책임을 위임

### 객체 대상
- Abstract Method
- Builder
- Prototype
- Singleton

## 구조 패턴
객체간의 관계를 조직

ex) 2개의 인터페이스가 서로 호환이 되지 않을 때, 둘을 연결하기 위해 새로운 클래스를 만들어 연결시킬 수 있음
### 클래스 대상
- Adapter

### 객체 대상
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

## 행위 패턴
객체의 행위를 조직, 관리, 연합

ex) 하위 클래스에서 구현해야 하는 함수 및 알고리즘들을 미리 선언하여, 상속시 이를 필수로 구현하도록 함
### 클래스 대상
- Interpreter
- Template

### 객체 대상
- Chain of Responsibility
- Command
- Iterator
- Mediator
- Memento
- Observer
- State
- Strategy
- Visitor

## 참조
- <https://github.com/gyoogle/tech-interview-for-developer/blob/master/Design%20Pattern/%5BDesign%20Pattern%5D%20Overview.md>
- <https://m.blog.naver.com/PostView.nhn?blogId=jvioonpe&logNo=220227413391&proxyReferer=https:%2F%2Fwww.google.com%2F>