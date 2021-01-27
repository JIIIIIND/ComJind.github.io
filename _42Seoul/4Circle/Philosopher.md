---
title: "Philosopher"
excerpt: "42Seoul Philosopher Guide documents"
date: 2020-09-25 14:06:22 +0900
categories: 42Seoul
---

## 기본 설명

- 원형의 테이블에 철학자들이 둥글게 앉아 있습니다.
- 포크는 철학자의 수 만큼 제공됩니다.
- 식사를 하기 위해서는 2개의 포크가 필요합니다.
- 모든 철학자들을 굶겨서는 안됩니다.
- 철학자는 생각하거나 밥을 먹거나 잠을 자는 3가지 상태를 가집니다.
- 철학자가 식사를 끝내면 포크를 내려놓고 잠을 잡니다. 잠에서 깨어난 직후 생각을 시작하고, 밥을 먹습니다.
- 철학자들은 서로의 상태를 알 수 없으며, 다른 철학자가 언제 죽을지도 모릅니다.
- 시뮬레이션은 철학자가 사망하면 종료됩니다.
- 인자 값으로 철학자의 숫자, 죽는데 걸리는 시간, 먹는데 걸리는 시간, 잠자는 시간, 각 철학자가 최소한 먹어야 하는 횟수가 제공됩니다.

**time_to_die**

생성 직후부터 해당 시간이 지나도록 식사를 못하거나 마지막 식사의 시작 시간으로부터 해당 시간까지 식사를 못하면 철학자는 사망합니다.

모든 철학자를 최대한 죽지 않도록 하는 것이 이번 과제의 목표입니다.

출력 형식은 다음과 같습니다.

```
timestamp_in_ms X has taken a fork
timestamp_in_ms X is eating
timestamp_in_ms X is sleeping
timestamp_in_ms X is thinking
timestamp_in_ms X died
```

모든 출력은 섞이지 않아야 하며, 철학자의 사망 이후 메시지가 출력되기까지 10ms가 넘어서는 안됩니다.

## philo_one

모든 철학자는 스레드로 구성됩니다.
포크는 철학자 사이에 배치됩니다. 즉, 철학자의 오른쪽과 왼쪽에 하나씩 놓여지게 됩니다.
철학자가 중복된 포크를 집지 않도록 모든 포크의 상태를 mutex로 보호해야 합니다.

컴파일을 하기 위해서는 다음과 같이 pthread 라이브러리를 추가해야 합니다.

```
gcc -Wall -Wextra -Werror -o philo_n *.c -lpthread
```

pthread를 사용하기 위해서는 pthread.h를 포함시키면 됩니다.

### pthread

프로세스 내에서 실행되는 흐름의 단위입니다. 프로세스 내의 주소 공간이나 자원들을 같은 프로세스 내에 스레드끼리 공유할 수 있습니다.

#### int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg)

스레드의 생성을 위해 사용합니다.

**parameter**

- pthread_t *thread
    - 스레드가 성공적으로 생성되었을때 생성된 스레드를 식별하기 위해 사용하는 스레드 식별자
- const pthread_attr_t *attr
    - 스레드 특성을 지정
    - 기본 특성을 사용할 때는 NULL을 사용
- void *(*start_routine)(void *)
    - 분기시켜 실행할 스레드 함수
- void *arg
    - 스레드 함수의 인자 값

**return value**

성공적으로 생성될 경우 0을 리턴
실패한 경우 0이 아닌 에러코드 값을 리턴

#### int pthread_join(pthread_t th, void **thread_return)
특정 스레드가 종료될 때까지 기다리며 스레드 자원을 해제함

**parameter**

- pthread_t th
    - join을 할 스레드 식별자
- void **thread_return
    - 스레드의 리턴 값
    - NULL이 아닐경우 해당 포인터로 스레드 리턴 값을 받아올 수 있음

**return value**

성공할 경우 th에 스레드 식별번호를 저장하고 0을 리턴
실패한 경우 0이 아닌 에러코드를 리턴

#### int pthread_detatch(pthread_t th)
main스레드에서 pthread_create를 이용해 생성된 스레드를 분리함
식별번호 th인 스레드를 detach하는데, detach되었을 경우 해당 스레드가 종료될 경우 pthread_join을 호출하지 않더라도 즉시 모든 자원이 해제됨

**return value**

성공 시 0을 반환
실패 시 0이 아닌 값을 반환(에러 코드)

### mutex

Mutual Exclusion의 약자로 상호배제라고 합니다. 특정 스레드에서 단독으로 들어가야 되는 코드 영역에서 동기화를 위해 사용합니다.

이 문제에서는 하나의 포크를 두명의 철학자가 공유하게 되므로, 누군가 해당 포크를 사용하고 있을 때 다른 철학자가 접근하지 못하게 막는 용도로 사용합니다.
이때 사용중인 포크에 접근하여 mutex에 걸리게 되면 포크가 사용 가능한 상태가 될 때까지 스레드는 블럭됩니다.

#### int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutex_attr *attr)
 mutex는 여러개의 스레드가 공유하는 데이터를 보호하기 위해 사용하는 도구
 보호하고자 하는 데이터를 다루는 코드 영역을 한번에 하나의 스레드만 실행가능 하도록 하는 방법으로 공유되는 데이터를 보호함
 이러한 영역을 critical section이라고 함

 **parameter**

 - pthread_mutex_t *mutex
    - 초기화 할 mutex 객체
 - const pthread_mutex_attr *attr
    - mutex의 특성을 변경
    - 기본 값을 사용하기 위해서는 NULL사용

**return value**

성공시 0 반환
실패 시 -1을 반환하며 errno설정됨

#### int pthread_mutex_destroy(pthread_mutex_t *mutex)
인자로 주어지는 mutex객체 mutex를 제거하기 위해 사용됨
pthread_mutex_t *mutex는 pthread_mutex_init함수를 이용해 생성된 mutex객체
반드시 unlock상태여야 함

#### int pthread_mutex_lock(pthread_mutex_t *mutex)
critical section에 들어가기 위해 mutex lock을 요청
만약 이미 다른 스레드에서 mutex lock을 얻어서 사용하고 있다면 다른 스레드에서 mutex lock을 해제할 때까지 블럭됨

모든 작업을 마치고 mutex lock이 더 이상 필요 없다면 pthread_mutex_unlock을 호춯해서 mutex lock을 돌려줘야 함

#### int pthread_mutex_unlock(pthread_mutex_t *mutex)
critical section에서의 모든 작업을 마치고 mutex lock을 돌려주기 위해 사용함

### 해결 방안

철학자 문제는 워낙에 유명한 문제라서 다양한 해결 방법이 있습니다.
우선 가장 먼저 생각할 수 있는 방법은 한번에 하나의 포크를 잡는것이 아니라 두개의 포크를 동시에 잡도록 강제하는 것입니다.

또 다른 해결 방법으로는 관리자/웨이터를 두는 방식입니다. 철학자가 포크를 잡기 위해서는 관리인에게 허락을 받아야 합니다. 위의 방식 또한 관리자를 두는 것과 큰 차이가 없지만, 여기에선 좀 더 확실하게 어떤 철학자 그룹이 밥을 먹을 수 있는지 알 수 있습니다.

위의 방식들을 사용하더라도 철학자들은 사망할 수 있습니다. 어떤 방식으로 철학자들이 안정적으로 식사를 할 수 있을지 고민하고 구현하시면 됩니다.

## philo_two

philo_two에서는 여전히 스레드를 사용하지만, 더이상 mutex를 사용하지 않고 semaphore를 사용합니다.

이 문제에서 포크는 철학자들의 사이가 아니라 테이블의 가운데에 모여있게 됩니다. 그리고 여러 포크를 만드는 것이 아니라 하나의 세마포어 값으로 사용해야 합니다.

세마포어를 사용하기 위해서는 semaphore.h를 포함시키면 됩니다.

### semaphore

mutex는 한번에 하나의 스레드만이 접근할 수 있도록 했지만 semaphore는 여러 스레드가 접근할 수 있도록 할 수 있습니다.
세마포어는 특정 값을 가지고 있는데, 이 값이 0보다 크면 자원에 접근할 수 있고, 0이면 접근할 수 없습니다.
그러므로 세마포어의 값이 1이라면 mutex와 동일하게 사용할 수 있습니다.

#### sem_t* sem_open(const char *name, int oflag, mode_t mode, unsigned int value)
이름 있는 세마포어의 경우 sem_open으로 생성
파일로 만들어지기 때문에, 파일 이름과 권한 설정이 필요

#### int sem_close(sem_t *sem)
세마포어 자원을 반환함

#### int sem_post(sem_t *sem)
세마포어를 되돌려줌
세마포어 값이 하나 증가

#### int sem_wait(sem_t *sem)
세마포어를 얻을 때까지 기다림
sem_wait함수는 만약 세마포어 값이 0보다 크면 프로세스는 세마포어를 얻고 세마포어를 감소하고 즉시 반환
세마포어 값이 0이라면 세마포어가 0보다 더 커지거나 시그널이 발생할 때까지 대기

#### int sem_unlink(const char *name)
세마포어를 삭제(이름 있는 세마포어를 삭제하고자 할 때 사용)
이름 있는 세마포어의 경우 파일로 생성되기 때문에 제거를 해야 함

## philo_three

philo_three에서는 철학자들이 스레드가 아닌 프로세스로 동작하게 됩니다. 그에 따라 프로세스간에 공유가 불가능해집니다.
프로세스간에 데이터를 공유하기 위해서는 파일을 사용하는 방식과 공유 메모리를 사용하는 방식이 있지만 허용된 함수 내에 해당 방식을 사용할 수 없기 때문에 공유되는 자원 없이 철학자를 구현해야 합니다.

하지만 모든 포크가 테이블 가운데에 놓여 있다는 점과 이름 있는 세마포어의 경우 파일을 사용한다는 점에서 프로세스간에 세마포어를 사용하여 포크에 접근할 수 있습니다.

메인 프로그램은 철학자일 필요가 없기 때문에 어떤 방식으로 구현할지는 자유롭게 구상하시면 됩니다.

### 프로세스 관련 명령어

#### fork, exit, kill

위의 명령어들은 앞 서클의 미니쉘에서 다루었을 것이기 때문에 따로 다루지 않음
kill을 사용하기 위해서는 signal.h를 포함시켜야 함


#### pid_t waitpid(pid_t pid, int *statloc , int options)

해당 함수도 미니쉘에서 다룰 수 있지만 다른 wait 함수들도 있었기에 설명합니다.

```"sys/wait.h"``` 헤더가 필요함

**parameter**

- pid_t pid
	- pid == -1
		- 임의의 자식 프로세스를 기다림
	- pid > 0
		- 프로세스 ID가 pid인 자식 프로세스를 기다림
	- pid < -1
		- 프로세스 그룹 ID가 pid의 절댓값과 같은 자식 프로세스를 기다림
	- pid == 0
		- waitpid를 호출한 프로세스의 프로세스 그룹 PID와 같은 프로세스 그룹 ID를 가진 프로세스를 기다림
- int *statloc
	- 자식 프로세스가 정상적으로 종료된 경우
		- WIFEXITED(statloc) 매크로가 true를 반환
		- 하위 8비트를 참조하여 자식 프로세스가 exit, _exit, _Exit에 넘겨준 인자 값을 얻을 수 있음
		- WEXITSTATUS(statloc)
	- 자식 프로세스가 비정상적으로 종료(시그널에 의한 종료)
		- WIFSIGNALED(statloc) 매크로가 true를 반환
		- 비정상 종료 이유를 WTERMSIG(statloc) 매크로를 사용하여 구할 수 있음
	- waitpid 함수 오류
		- 함수의 반환 값이 -1이다.
		- ECHILD
			- 호출자의 자식 프로세스가 없는 경우
		- EINTR
			- 시스템 콜이 인터럽트 되었을 경우
- int options
	- WCONTINUED
		- 중단 되었다가 재개된 자식 프로세스의 상태를 받음
	- WNOHANG
		- 기다리는 PID가 종료되지 않아서 즉시 종료 상태를 회수 할 수 없는 상황에서 호출자는 차단되지 않고 반환값으로 0을 받음
	- WUNTRACED
		- 중단된 자식 프로세스의 상태를 받음(블럭됨)

**return value**

성공시 프로세스 id를 반환하고, 실패시 -1을 반환
오류에 대해서는 statloc이 가지고 있음

### 좀비 프로세스와 고아 프로세스

부모 프로세스가 자식 프로세스가 먼저 종료될 경우, 자식은 고아 프로세스가 됩니다.

자식 프로세스가 종료되었지만 부모 프로세스가 자식의 종료 상태를 회수하지 않을 경우 자식 프로세스는 좀비 프로세스입니다.

해당 과제에서 고아 프로세스가 나올 일은 없겠지만 그래도 같이 알아두는게 좋습니다.

#### 고아 프로세스

부모가 자식보다 먼저 종료된 경우, init 프로세스가 자식의 새로운 부모가 됩니다.

종료되는 프로세스가 발생할 때 커널은 이 프로세스가 누구의 부모인지 확인 후, 커널이 자식 프로세스의 부모 프로세스ID를 1로 바꿔 줍니다.

init 프로세스의 경우 유닉스 계열의 운영체제에서 부팅 과정 중 생성되는 최초의 프로세스이며, 시스템이 종료될때까지 계속 살아있는 데몬 프로세스입니다.

#### 좀비 프로세스

자식 프로세스가 exit 시스템콜을 호출하면서 종료되면 이 프로세스에 관련된 모든 메모리와 리소스가 해제되어 다른 프로세스에서 사용할 수 있게 됩니다. 하지만 종료 이후에도 부모 프로세스가 자식 프로세스의 상태를 알고 싶어할 수 있기 때문에 커널은 최소한의 정보(프로세스 ID, 프로세스 종료 상태 등)를 가지고 있게 됩니다.

부모 프로세스가 좀비 프로세스의 종료 상태를 회수하게 되면(wait 시스템콜 호출) 좀비 프로세스는 제거됩니다.

좀비 프로세스가 쌓이게 되면 리소스의 유출을 야기할 수 있기 때문에 좀비 프로세스 상태를 오래 유지하지 않도록 부모 프로세스는 wait 시스템 콜 함수를 사용하여 자식 프로세스의 종료 상태를 읽어들이는 것이 필요합니다.

**참고자료**

<https://codetravel.tistory.com/31?category=993122>
