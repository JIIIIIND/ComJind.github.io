---
title: "title"
date: 2021-01-30 23:08:29 +0900
---

## 개요

> Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithms structure. – GoF Design Patterns

알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경 없이 알고리즘을 재정의 하는 패턴입니다. 알고리즘이 단계별로 나누어 지거나, 같은 역할을 하는 메소드이지만 여러곳에서 다른 형태로 사용이 필요한 경우 유용합니다.

## 사용 예시

저장 기능에서 문서의 종류에 따라 아래 기능 중 하나가 선택적으로 일어나도록 수정

- 저장 전에 화면을 캡처
- 저장 전에 쓰기 권한을 확인합니다. 권한이 없다면 저장하지 않습니다.
- 저장이 정상적으로 이루어졌다면 파일 서버에 자동으로 백업합니다.
- 저장이 실패했다면 에러 코드를 서버에 전송합니다.

기존의 코드는 다음과 같습니다.

```cpp
bool CDocument::save()
{
	...(저장 로직)...
	return TRUE;
}
```

간단한 개발을 위해 옵션별로 아래와 같은 플래그 2개를 추가합니다.

- bool m_bCapture; // TRUE이면 save 전에 화면을 캡처
- bool m_bAuthWrite; // TRUE이면 쓰기 권한을 확인

수정된 코드는 다음과 같습니다.

```cpp
class CDocument
{
	...
	private:
		void CaptureScrean();				// 화면 캡처 함수
		void BackupFile();					// 파일 백업 함수
		bool HaveAuthWrite();				// 쓰기 권한 체크 함수
		void ErrorReport(int a_nErrorCode);	// 에러 보고 함수
	...
};

bool CDocument::save()
{
	bool	bSaveOK = false;
	int		nError = 0;

	if (m_bCapture)
		CaptureScrean();
	else if (!HaveAuthWrite())
		return (false);

	...
	(저장 로직)
	1. 저장이 성공하며 bSaveOK를 TRUE로 변경
	2. 실패하면 bSaveOK를 false로 바꾸고 nError를 설정
	...

	if (bSaveOK)
	{
		if (m_bBackup)
			BackupFile();
	}
	else
	{
		ErrorReport(nError);
		return (false);
	}
	return (true);
}
```

플래그를 사용하면 쉽고 빠르게 개발을 할 수 있다는 장점이 있습니다.
하지만 소스의 가독성을 떨어트리고 플래그끼리 꼬여서 불안정한 코드를 생산할 가능성이 매우 크기 때문에 남용해서는 안됩니다. (if-else로 도배된 코드나 switch 내부에 switch가 있는 코드는 해석하기가 어려움)

객체지향에서는 이런 플래그 기반의 코드보다는 플래그에 따라 바뀌는 기능이나 책임을 클래스로 정리해서 구조적으로 이해하기 쉬운 방식을 선호합니다.

해당 요구사항은 크게 저장 전과 저장 후로 나뉩니다. 플래그를 사용해서 수정을 한 이유 중 하나는 저장 전과 저장 후에 해당하는 시점이 없기 때문입니다. 예를 들어 다음과 같은 세가지 시점의 함수를 생각해봅니다.

- bool SaveBefore();		// 저장 전에 호출하는 함수
- bool Save();				// 저장 함수
- bool SaveAfter();			// 저장 후에 호출하는 함수

위와 같은 세 함수가 제공된다면 저장 전에 실행하는 기능은 SaveBefore() 함수에, 저장 후에 실행해야 하는 기능은 SaveAfter() 함수에 추가되었을 것입니다.

하지만 위와 같이 SaveBefore(), Save(), SaveAfter()를 순차적으로 호출해야 한다면 사용이 불편해집니다. Save() 함수를 호출 전에 SaveBefore()를 꼭 호출해줘야 한다는 제약이 있다면 버그를 발생시킬 가능성만 높아집니다. 결론적으로 우리는 클래스 외부에 Save() 함수만을 제공해야 합니다.

Save()함수 내부에 시점을 만듭니다. 기존의 Save()는 저장이라는 기능만을 수행한다면 해당 기능의 이전과 이후가 우리가 원하는 시점이 됩니다.

```cpp
bool CDocument::Save()
{
	'저장 전 시점' (현재 없는 시점)
	...
	(저장)
	...
	'저장 후 시점' (현재 없는 시점)
	return (true);
}
```

위의 코드는 아래와 같이 변경할 수 있습니다.

```cpp
bool CDocument::Save()
{
	InternalSaveBefore();
	InternalSave();
	InternalSaveAfter();
	return (true);
}
```

Internal prefix는 기존의 Save() 함수와 헷갈리지 않도록 하는 점과 클래스 내부에서만 사용한다는 의미를 분명하게 하기 위해 사용되었습니다. 개발자의 기호에 따라 Do나 _와 같은 prefix를 붙일 수 있습니다.

전반적인 코드는 다음과 같습니다.

```cpp
class CDocument
{
	...
	public:
		bool Save();
	protected:
		virtual bool InternalSaveBefore() { return (true); }
		virtual bool InternalSave();
		virtual bool InternalSaveAfter() { return (true); }
};

bool CDocument::InternalSave()
{
	...
	기존의 Save() 함수에 있던 코드
	...
	return (true);
}

bool CDocument::Save()
{
	InternalSaveBefore();	// 지금은 동작 안함
	InternalSave();
	InternalSaveAfter();	// 지금은 동작 안함

	return (true);
}
```

함수의 흐름은 다음과 같습니다.

- 저장 전
	1. 쓰기 권한 확인
	2. 권한이 없다면 리턴
- 저장
- 저장 후
	1. 성공이면 서버에 백업
	2. 실패하면 에러 레포트

이러한 과정을 일반화 시키면 다음과 같습니다.

- 저장 전
	- InternalSaveBefore()의 리턴 값이 false이면 리턴
- 저장
	- 리턴 값을 로컬 변수로 저장
- 저장 후
	- InternalSaveAfter()로 리턴 값을 전달

이러한 시나리오를 토대로 아래와 같은 일반화된 로직을 구성할 수 있습니다.

```cpp
class CDocument
{
	...
	virtual bool InternalSaveAfter(bool a_bSave)
	{ return (true); }
	...
};

bool CDocument::Save()
{
	bool bSaveReturn = false;

	if (!InternalSaveBefore())
		return (false);
	
	bSaveReturn = InternalSave();

	if (!InternalSaveAfter(bSaveReturn))
		return (false);
	return (true);
}
```

부모 클래스의 기본 뼈대가 완성됐으므로 플래그가 아닌 클래스의 확장을 통해 요구사항을 만족시킵니다.

CDocument를 상속받는 CCaptureDocument, CErrorReportDocument, CWriteAuthDocument, CAutoBackupDocument를 구현합니다.

자식 Document 클래스들은 저장 전, 후 시점에 자신만의 행동을 취합니다. 저장 전, 후에 호출하는 함수는 아래와 같은 의미를 가집니다.

- virtual bool InternalSaveBefore()
	- 리턴 값이 true이면 저장 로직을 계속 수행
	- 리턴 값이 false이면 저장 로직은 실패
- virtual bool InternalSaveAfter()
	- a_bSave가 true이면 저장이 성공, false이면 실패
	- 리턴 값이 true이면 저장 로직을 계속 수행
	- false이면 저장 로직은 실패

각 함수의 의미를 파악했으므로 Document마다 다음과 같은 구현을 이끌어 낼 수 있습니다.

```cpp
class CCaptureDocument : public CDocument
{
	protected:
		virtual bool InternalSaveBefore()
		{
			CaptureScreen();
			return (true);
		}
	private:
		void CaptureScreen();
};

class CWriteAuthDocument : public CDocument
{
	protected:
		virtual bool InternalSaveBefore()
		{
			return CheckWriteAuth();
		}
	private:
		bool CheckWriteAuth();
};

class CAutoBasckupDocument : public CDocument
{
	protected:
		virtual bool InternalSaveAfter(bool a_bSave)
		{
			if (!a_bSave)
				return (false);
			BackupFile();
			return (true);
		}
	private:
		void BackupFile();
};

class CErrorReportDocument : public CDoument
{
	protected:
		virtual bool InternalSaveAfter(bool a_bSave)
		{
			if (a_bSave)
				return (true);
			ErrorReport();
			return (false);
		}
	private:
		void ErrorReport();
};
```

이처럼 함수의 동작 방식의 구조가 부모 클래스에서 정의되고 구체적인 동작 방식은 자식 클래스에서 정의되는 형태를 템플릿 메소드 패턴이라고 합니다.

InternalSaveBefore(), After() 등은 순수 가상 함수여도 무방하지만, 이럴 경우 자식 클래스에서 반드시 재정의 해주어야만 인스턴스를 생성할 수 있기 때문에 아무 일도 하지 않는 빈 껍데기 함수를 부모 클래스에서 재정의 해주는 편이 개발하기 쉽습니다.

## 정리

템플릿 메소드 패턴은 객체지향에서 가장 기본이 되는 패턴입니다. 익숙해 지면 굉장히 편하지만 부모 클래스에서 미리 구조를 정의한다는 점에서 어려움을 많이 느낍니다.

많은 경우에 부모 클래스는 분석을 통해 구조가 미리 정의되기 보다는 요구사항이 점점 늘어나며 책임이 많아집니다.
관련 없는 기능을 한 클래스에 넣는 것 보다는 자식 클래스로 나누어서 클래스를 경량화할 필요가 있습니다.

상속을 통한다면 물리적으로 클래스가 나뉘기 때문에 자식 클래스에서 원하는 곳, 원하는 시점에 코드를 넣을 수 없습니다. 이 때 부모 클래스에서 자식 클래스들이 오버라이딩 할 수 있는 함수들을 적절한 시점을 잡아서 정의해주면 부모 클래스의 수정 없이 기능을 확장할 수 있습니다.

이런 방법을 몇 번 거치다 보면 부모 클래스는 자연스럽게 동작의 구조를 정의하게 되며 자식은 특화된 동작을 가지게 됩니다.