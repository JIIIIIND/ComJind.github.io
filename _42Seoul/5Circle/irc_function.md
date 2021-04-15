---
title: "ft_irc_external_function"
excerpt: "42Seoul irc Guide documents"
date: 2021-04-15 22:42:22 +0900
categories: 42Seoul
toc: true
---

## Socket 관련 함수

### socket

```c
int socket(int domain, int type, int protocol)
```

sys/types.h, sys.socket.h를 포함해야 합니다.

해당 소켓을 가리키는 소켓 디스크립터를 반환합니다.
-1이 반환되면 실패, 0 이상의 값은 소켓 디스크립터입니다.

- domain
	- 어떤 영역에서 통신할 것인지에 대한 영역 지정
	- 프로토콜 family를 지정해주는 것
	- AF_UNIX(프로토콜 내부), AF_INET(IPv4), AF_INET6(IPv6)
- type
	- 어떤 타입의 프로토콜을 사용할 것인지에 대해 설정
	- SOCK_STREAM(TCP), SOCK_DGRAM(UDP), SOCK_RAW(사용자 정의)
- protocol
	- 어떤 프로토콜의 값을 결정하는 것
	- 0, IPPROTO_TCP(TCP의 경우), IPPROTO_UDP(UDP의 경우)

### setsockopt

```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen)
```

sys/socket.h를 포함해야 합니다.

생성된 socket에 속성값을 변경하는 함수

```
- sockfd
	- socket, accept 등으로 생성된 소켓 디스크립터
- level
	- optname 값이 socket level인지 특정 protocol에 대한 설정인지를 지정하는 값
	- SOL_SOCKET
		- optname이 socket level에서 설정하는 option명임을 지정
	- IPPROTO_IP
		- optname이 IP protocol level에서 설정하는 option명임을 지정
	- IPPROTO_TCP
		- optname이 TCP protocol level에서 설정하는 option명임을 지정
- optname
	- level의 종류에 따라 다른 설정이름을 가집니다.
	- SOL_SOCKET
		- level의 상수
	- SO_ACCEPTCONN
		- accept된 connection 여부 조회(get only)
		- 1이면 accept된 connection
	- SO_BROADCAST
		- datagram socket에 broadcast flag 값을 얻음(set/get)
	- SO_DOMAIN
		- socket에 설정된 domain값을 얻음(AF_INET, AF_UNIX 등)(get only)
	- SO_ERROR
		- socket error를 읽고 지움(get only)
	- SO_DONTROUT
		- gateway를 통해 전송을 금지하고 직접 연결된 host끼리만 전달하게 함(set/get)
	- SO_KEEPALIVE
		- connection-oriented socket에서 keep alive message를 전송할 수 있도록 함(set/get)
	- SO_LINGER
		- linger option 설정(set/get)
		struct linger {
			int l_onoff;
			int l_linger
		};
		l_onoff를 1로 설정하면 close, shutdown 함수를 실행 시 미전송된 데이터를 정상적으로 전송하거나 linger timeout이 도래하면 함수를 return함. 그렇지 않으면 바로 return 되고 background로 작업하게 됨
	- SO_OOBINLINE
		- out of bound data를 직접 읽을 수 있게 set/get
	- SO_PROTOCOL
		- socket에 설정된 protocol을 읽음
	- SO_RCVBUF
		- socket에서 읽을 수 있는 최대 buffer의 크기를 set/get함
	- SO_REUSEADDR
		- bind 시에 local 주소를 재사용할 것인지 여부를 set/get함
	- SO_SNDBUF
		- socket에서 write할 수 있는 최대 buffer의 크기를 set/get함
	- SO_TYPE
		- 설정된 socket의 type(SOCK_STREMA, SOCK_DGRAM)을 get함
- optval
	- optname에 따른 설정할 값
- optlen
	- optval의 크기
```

정상적으로 option의 값을 설정한 경우 0을, 오류가 발생한 경우 -1이 리턴 됨

상세 내용은 errno에 설정

<https://www.it-note.kr/122?category=1068194>

### getsockname

```c
int getsockname(int sockfd, struct sockaddr *addr, socklen_t *addrlen)
```

sys/socket.h를 포함해야 합니다.

소켓 디스크립터에 할당된 자신의 주소를 얻습니다.
sockaddr 구조체는 socket생성 시에 설정한 Address family의 값에 따라 구조체의 구조가 다르므로 Address Family에 맞는 구조체를 선언하여 사용해야 합니다.

```
- sockfd
	- 정상적으로 생성된 소켓 디스크립터
	- 주로 connect, accept, bind된 소켓 디스크립터 입니다
- sockaddr
	- 자신의 주소를 저장할 buffer
	- 정상적으로 실행되면, sockfd에 설정된 자신의 address주소값이 채워집니다.
	- socket에 자신의 주소는 client socket에서는 bind를 하지 않는 이상 connect시에 자동으로 할당되며, server socket도 자신의 주소는 port 번호만 설정하는 것이 일반적이라 accept시에 할당됩니다.
	- 대부분 connect, accept 후의 소켓 디스크립터에 사용합니다.
	- 구조체 종류는 Address Family의 종류에 따라 다른 구조체를 parameter로 전달해야 합니다.
- addrlen
	- 두번째 파라미터인 addr 구조체의 크기를 설정하고 함수를 호출해야 합니다
	- getsockname() 호출이 끝나면 실제로 addr에 채워진 사이즈가 저장됩니다
```

정상적으로 자신의 주소 정보를 얻은 경우 0이, 오류가 발생하는 경우 -1이 리턴되고 errno에 오류 정보가 저장됩니다.

### getprotobyname

```c
struct protoent *getprotobyname(const char *name)
```

netdb.h를 포함해야 합니다.

프로토콜 이름과 name이 일치하는 데이터베이스 항목에 대한 protoent 구조체를 반환합니다.

```c
struct protoent {
	char	*p_name;		// official name of protocol
	char	**p_aliases;	// alias list
	int		p_proto;		// protocol number
};
```

필요한 경우에 데이터베이스에 대한 연결이 열립니다.

정상적인 경우 할당 된 protoent 구조체에 대한 포인터를 반환하지만 오류가 발생하거나 파일의 끝에 도달하면 NULL을 반환합니다.

### gethostbyname

```c
struct hostent *gethostbyname(const char *name)
```

netdb.h를 포함해야 합니다.

```c
struct hostent {
	char	*h_name;				// official name of host
	char	**h_aliases;			// alias list
	int		h_addrtype;				// host address type
	int		h_length;				// length of address
	char	**h_addr_list;			// list of addresses from name server
	#define h_addr h_addr_list[0]	// address, for backward compatibility
};
```

name은 호스트 이름이거나 표준 점 표기법의 IPv4 주소, 콜론 표기법 또는 점 표기법의 IPv6입니다.

주소 정보는 local computer의 /etc/hosts 파일 또는 DNS 서버로부터 얻습니다.

정상적인 경우 hostent 구조체가 반환되며 오류가 발생한 경우 NULL이 반환됩니다.

주소는 h_addr 멤버 변수에 저장되며, 그 주소의 길이는 h_length에 저장됩니다.
h_addr의 format은 network byte order의 주소이며, 인터넷 표준 점표기법으로 표시하려면 inet_ntoa()로 변환이 필요합니다.

<https://www.it-note.kr/22>

### getaddrinfo

```c
int getaddrinfo(const char *hostname, const char *service, const struct addrinfo *hints, struct addrinfo **result)
```

sys/types.h, sys/socket.h, netdb.h를 포함해야 합니다.

domain address를 받아서 네트워크 주소 정보(IP Address)를 가져오는 함수입니다.

즉 Domain Address를 IP Address로 변환하고 싶을 때 사용합니다.

결과는 addrinfo 구조체의 linked list로 4번째 매개변수입니다.

```c
struct addrinfo {
	int				ai_flags;		// 추가적인 옵션을 정의 할 때 사용. 여러 flag를 bitwise or로 넣음
	int				ai_family;		// address family를 나타냄(AF_INET, AF_INET6, AF_UNSPEC 등등)
	int				ai_socktype;	// socket type을 나타냄(SOCK_STREAM, SOCK_DGRAM)
	int				ai_protocol;	// IPv4와 IPv6에 대한 IPPROTO_xxx와 같은 값을 가짐
	socklen_t		ai_addrlen;		// socket 주소인 ai_addr의 길이를 나타냄
	char			*ai_canonname;	// 호스트의 canonical name을 나타냄
	struct sockaddr	*ai_addr;		// socket 주소를 나타내는 구조체 포인터
	struct addrinfo	*ai_next;		// 주소 정보 구조체 addrinfo는 linked list입니다.
};
```

매개변수는 다음과 같습니다.

```
- hostname
	- 호스트 이름 혹은 주소 문자열(IPv4의 점으로 구분하는 10진 주소 문자열이거나 IPv6dml 16진 문자열)
- service
	- 서비스 이름 혹은 10진수로 표현한 포트 번호 문자열
- hints
	- getaddrinfo 함수에게 말 그대로 힌트를 줌
	- addrinfo 구조체에 hints로 줄 정보를 채운 뒤 주소값을 넘김
	- 반환받을 result를 제한하는 기준을 지정하는 것
	- 별도의 hint를 제공하지 않는 경우 NULL
- result
	- DNS서버로부터 받은 네트워크 주소 정보를 돌려줌
	- 해당 result의 내용 중 필요한 것들은 copy하여 사용자의 변수로 옮겨두고, result의 사용이 끝나는 즉시 freeaddrinfo함수로 메모리 해제를 해야 함
```

성공 시 0을 반환하고, 실패하면 0이 아닌 아래의 값들 중 하나를 반환합니다.

- EAI_ADDRFAMILY
	- 지정된 네트워크 호스트에 요청했던 address family 주소가 없는 경우
	- IPv4 주소만 가지는 호스트에 IPv6를 요청한 경우
- EAI_AGAIN
	- nameserver가 일시적인 오류 표시를 반환
	- 일정 시간 이후 다시 시도하라는 의미
- EAI_BADFLAGS
	- hints.ai_flags에 잘못된 플래그가 포함된 경우
	- hints.ai_flags에는 AI_CANONNAME이 포함되었고 이름이 null인 경우
- EAI_FAIL
	- nameserver가 지속되는 오류 표시를 반환
	- EAI_AGAIN과 달리 다시 시도해도 실패
- EAI_FAMILY
	- 요청한 address family는 지원되지 않음
- EAI_MEMORY
	- getaddrinfo를 수행하기에 메모리가 부족
- EAI_NODATA
	- 지정한 네트워크 호스트가 있지만 네트워크 주소가 정의되지 않은 경우
- EAI_NONAME
	- 노드 또는 서비스를 알 수 없는 경우
	- AI_NUMERICSERV가 hints.ai_flags에 지정되었지만 service는 포트 번호를 나타내는 숫자 형태의 문자열이 아닌 경우
	- 즉, 입력 매개변수를 잘못 넣은 경우
- EAI_SERVICE
	- 요청한 서비스를 요청한 소켓 유형에 사용할 수 없음
	- 서비스가 쉘이고 hints.ai_protocol이 IPPROTO_UDP거나 hints.ai_socktype이 SOCK_DGRAM인 경우 해당 오류 발생
	- 서비스가 NULL이 아니고 hints.ai_socktype이 SOCK_RAW(서비스 개념을 지원하지 않는 소켓 유형)인 경우
- EAI_SOCKTYPE
	- 요청한 소켓 유형이 지원되지 않음
	- hints.ai_socktype 및 hints.ai_protocol이 일치하지 않는 경우
	- ai_socktype에는 SOCK_DGRAM을 넣고, ai_protocol에는 IPPROTO_TCP를 넣은 hint를 넘기는 경우
- EAI_SYSTEM
	- 위의 모든 경우를 제외한 오류
	- 자세한 내용은 errno를 참고

<https://techlog.gurucat.net/293>

### freeaddrinfo

```c
void freeaddrinfo(struct addrinfo *res)
```

sys/types.h, sys/socket.h, netdb.h를 포함해야 합니다.

위의 getaddrinfo의 결과로 만들어진 result를 할당 해제합니다.

<https://linux.die.net/man/3/freeaddrinfo>

### bind

```c
int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen)
```

sys/types.h, sys/socket.h를 포함해야 합니다.

해당 함수는 소켓에 IP주소와 포트번호를 지정해 줍니다. 소켓을 통신에 사용할 수 있도록 준비가 됩니다.

```
- sockfd
	- 소켓 디스크립터
- *myaddr
	- 주소 정보로 인터넷을 이용하는 AF_INET인지 시스템 내에서 통신하는 AF_UNIX에 따라서 달라짐
	- 인터넷을 통해 통신하는 AF_INET인 경우 struct sockaddr_in을 사용
	
	struct sockaddr_in {
		sa_family_t			sin_family;
		unsigned short int	sin_port;
		struct in_addr		sin_addr;
		unsigned char		__pad[__SOCK_SIZE__ - sizeof(short int) - sizeof(unsigned short int) - sizeof(struct in_addr)];
	};

	- 시스템 내부 통신인 AF_UNIX인 경우에는 struct sockaddr을 사용

	struct sockaddr {
		sa_family_t			sa_family;
		char				sa_data[14];
	};
- addrlen
	- myaddr 구조체의 크기
```

성공 시 0을 반환하고 실패하면 -1이 반환됩니다.

<http://forum.falinux.com/zbxe/index.php?mid=C_LIB&document_srl=430926>

### connect

```c
int connect(int sockfd, const struct sockaddr *serv_addr, socklen_t addrlen)
```

sys/types.h, sys/socket.h를 포함해야 합니다.

생성한 소켓을 통해 서버로 접속을 요청합니다.

```
- sockfd
	- 소켓 디스크립터
- *serv_addr
	- 서버 주소 정보에 대한 포인터
- addrlen
	- serv_addr 포인터가 가리키는 구조체의 크기
```

성공 시 0을 반환하고 실패하면 -1을 반환합니다.

### listen

```c
int listen(int sockfd, int backlog)
```

sys/types.h, sys/socket.h를 포함해야 합니다.

SOCK_STREAM, SOCK_SEQPACKET 등의 연결지향형 socket에 대해서 server socket을 활성화하여 client의 접속을 허용하게 합니다.
client의 접속 요청에 대해 accept를 통해 연결이 맺어지고 accept되지 못한 요청은 backlog의 크기만큼 queue에 쌓입니다. backlog의 크기보다 많은 Connection이 쌓이면 Client는 ECONNREFUSED 오류가 발생합니다.

```
- sockfd
	- 소켓 디스크립터
	- 해당 디스크립터를 생성하는 socket 함수의 2번째 파라미터인 type이 SOCK_STREAM, SOCK_SEQPACKET과 같은 연결지향형이어야 합니다.
- backlog
	- client의 접속 요청에 대해 accept를 통하여 양쪽의 socket이 연결되는데, accept가 빨리 되지 않을 때 대기할 queue의 갯수입니다.
- client의 접속 요청이 많은 경우, 대기되지 않고 빠른 accept를 할 수 있도록 Multi-process, Multi-Thread 등의 방식을 사용합니다.
```

정상적으로 동작한 경우 -1이 아닌 값이, 오류가 발생한 경우 -1이 반환되며 자세한 내용은 errno에 저장됩니다.

<https://www.it-note.kr/115>

### accept

```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen)
```

client의 접속 요청을 받아들여 client와 연결합니다.
accept 함수의 결과로 client와 연결을 유지하는 새로운 socket을 생성합니다.
socket() 호출 시 type이 SOCK_STREAM, SOCK_SEQPACKET과 같은 연결지향성 소켓이어야 합니다.

```
- sockfd
	- 소켓 디스크립터
	- bind 및 listen이 완료된 상태
- addr
	- 접속한 client의 주소 정보를 저장(설정하는 값이 아니라 얻는 값)
	- addr에 NULL을 넘기면, client 주소 정보를 받지 않겠다는 의미
- addrlen
	- addr의 크기를 설정
	- 접속이 완료되면 실제로 addr에 설정된 접속한 client의 주소 정보의 크기를 저장
	- addr에 NULL을 넘기면 addrlen의 값은 무시됩니다.
```

성공한 경우 -1이 아닌 값이 반환되며, client와 연결된 새로운 소켓이 생성됩니다.
server socket은 socket()을 통하여 생성된 소켓 디스크립터, client와 통신할 수 없으며, accept가 return한 socket을 통하여 client와 데이터를 송수신할 수 있습니다.

실패한 경우는 -1이 반환되며 자세한 내용은 errno에 저장됩니다.

<https://www.it-note.kr/116>

### htons

```c
uint16_t htons(uint16_t hostshort)
```

arpa/inet.h를 포함해야 합니다.

short 메모리 값을 호스트 바이트 순서에서 네트워크 바이트 순서로 변경합니다.

호스트 바이트 순서는 지금 사용 중인 시스템에서 2바이트 이상의 큰 숫자 변수에 대해 바이트를 메모리 상에 어떻게 배치하는지 순서를 말하며, 네트워크 바이트 순서는 2바이트 이상의 큰 숫자에 대해 어떤 바이트부터 전송할지에 대한 순서를 말합니다.

2바이트 이상의 큰 숫자를 전송할 때, 큰 단위를 먼저 전송할지 작은 단위를 먼저 전송할지 미리 정하지 않으면 문제가 발생합니다.

네트워크에서는 Big-Endian을 사용합니다.
Big-Endian을 사용하는 시스템은 그대로 전송하면 되지만 Little-Endian을 사용하는 호스트에서는 바이트 순서를 바꾸어서 전송해야 합니다.

하지만 코드를 통해 맞춘다면 시스템에 독립적인 프로그램을 작성할 수 없기 때문에 htons 함수를 사용합니다.

### htonl

```c
uint32_t htonl(uint32_5 hostlong)
```

long 형 호스트 바이트 순서 데이터를 네트워크 바이트 순서값으로 구합니다.

### ntohs

```c
uint16_t ntohs(uint16_t netshort)
```

short 형 네트워크 바이트 순서 데이터를 호스트 바이트 순서 데이터 값으로 구합니다.

### ntohl

```c
uint32_t ntohl(uint32_t netlong)
```

long 형 네트워크 바이트 순서 데이터를 호스트 바이트 순서 데이터 값으로 구합니다.

### inet_addr

```c
unsigned long int inet_addr(const char *cp)
```

sys/socket.h, netinet/in.h, arpa/inet.h가 필요합니다.

숫자와 점으로 이루어진 IP 문자열을 long 형의 숫자 IP주소로 바꾸어 줍니다.
struct sockaddr_in에서 .sin_addr.s_add 값을 long형의 숫자 IP 값을 넣어 주어야 하는데 이 때 사용됩니다.

실패 시 -1이 반환됩니다.

### inet_ntoa

```c
char *inet_ntoa(struct in_addr in)
```

network byte order의 주소를 a.b.c.d 형태인 IPv4 주소 형태의 문자열로 반환합니다.
struct in_addr은 AF_INET Address family 주소 체계에 포함된 구조입니다.
accept, getpeername, getsockname 등으로 얻은 주소 정보입니다.

숫자 점 표기법의 IP주소를 반환합니다.

### send

```c
ssize_t send(int sockfd, const void *buf, size_t len, int flags)
```

- sockfd
	- connect, accept로 연결된 socket descriptor
- buf
	- 전송할 데이터
- len
	- 전송할 데이터 길이
- flags
	- 전송할 데이터 또는 읽는 방법에 대한 option. 0 또는 bit or 연산으로 설정 가능
	- MSG_DONTROUTE
		- gateway를 통하지 않고 직접 상대 시스템으로 전송
	- MSG_DONTWAIT
		- non blocking에서 사용하는 옵션으로 전송이 block되면 EAGIN, EWOULDBLOCK 오류로 바로 return함
	- MSG_MORE
		- 더 전송할 데이터가 있음을 설정함
	- MSG_OOB
		- out of band 데이터를 읽습니다. 주로 X.25에서 접속이 끊겼을 때에 전송되는 데이터 flags 값이 0이면 일반 데이터를 전송하며, write를 호출한 것과 같습니다.

flag를 설정하지 않는 경우엔 write와 똑같은 동작을 합니다.
실패 시 -1을 반환하고 오류 내용은 errno에 저장됩니다.

### recv

```c
ssize_t recv(int sockfd, void *buf, size_t len, int flags)
```

- sockfd
	- connect, accept로 연결 된 socket descriptor
- buf
	- data를 수신하여 저장할 buffer
- len
	- 읽을 데이터 크기
- flags
	- 읽을 데이터 유형 또는 읽는 방법에 대한 option
	- MSG_OOB
		- out of band(긴급 데이터) 데이터를 읽음
		- 주로 X.25에서 접속이 끊겼을 때 전송되는 데이터
	- MSG_PEEK
		- receive queue의 데이터를 queue에서 제거하지 않고 확인하기 위한 목적으로 설정
	- MSG_WAITALL
		- 읽으려는 데이터가 buffer에 찰 때까지 대기함

flags의 값이 0이면 일반 데이터를 수신하고 read와 똑같은 동작을 합니다.
실패 시 -1을 반환하고 오류 내용은 errno에 저장됩니다.

### select

```c
int select(int n, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout)
```

I/O 멀티플렉싱을 가능하게 해줍니다.
사용자 여러 명의 접속을 받아서 그 사용자로부터 입력, 출력, 예외 처리를 수행 할 수 있습니다.

해당 함수를 사용하기 전에 fd_set을 사용할 수 있어야 하는데 간단하게 다음 함수들만 알고 있으면 됩니다.

- FD_CLR(int fd, fd_set *set)
	- fd_set의 fd를 0으로 세팅
- FD_ISSET(int fd, fd_set *set)
	- fd_set의 fd가 1이라면 true 반환
- FD_SET(int fd, fd_set *set)
	- fd_set의 fd를 1로 세팅
- FD_ZERO(fd_set *set)
	- fd_set을 0으로 초기화
- FD_COPY(fd_set *fdest_orig, fd_set *fdest_copy)
	- 이미 할당 된 dest_copy를 dest_orig로 복사함

각 파라미터는 다음과 같습니다.

- n
	- 최대 fd값
- readfds
	- read가 가능한지 확인 할 fd_set
- writefds
	- write가 가능한지 확인 할 fd_set
- exceptfds
	- except가 가능한지 확인 할 fd_set
- timeout
	- 대기 시간

timeout에 설정된 시간 만큼 대기를 하면서 readfds, writefds, exceptfds에서 입출력을 수행할 수 있다면 해당 fd_set에 값이 세팅되고, 준비 된 fd의 개수가 반환됩니다.
특정 기능만 사용하고 싶다면 readfds, writefds, exceptfds 대신 NULL을 넣어 해당 기능은 사용하지 않아도 됩니다.

fd_set의 값들이 변경되기 때문에 특정 fd를 보관하는 용도로 사용하고 있었다면 복사해둔 fd_set을 사용해야 합니다.

해당 함수를 사용하면 read/write 동작이 값을 반환할 때만 실행할 수 있기에 시스템 자원을 아낄 수 있습니다.

과제에서 비동기 통신을 요구하기 때문에 select를 사용하지 않아도 블록되지는 않지만 시스템 자원이 계속해서 낭비가 되기 때문에 0점으로 기록됩니다.

### fcntl

```c
int fcntl(int fd, int cmd, long arg)
```

fcntl을 사용해서 할 수 있는 일은 cmd로 결정됩니다.
하지만 과제에서 요구하는 fcntl의 사용법은 정해져 있기 때문에 fcntl(fd, F_SETFL, O_NONBLOCK)만을 사용해야 합니다.
이 이외의 인자를 사용하게 되면 0점입니다.

- F_SETFL
	- 파일 지정자에 대한 값을 세팅
- O_NONBLOCK
	- fd를 NON_BLOCK 모드로 변경

NON_BLOCK으로 fd를 변경하게 되면 read/write/send/recv/connect 등 기본적으로 BLOCK모드로 사용되던 함수들이 NON_BLOCK으로 동작하게 됩니다.

- read/recv
	- BLOCK
		- read 버퍼가 비었을 때 block
	- NON_BLOCK
		- read 버퍼가 비었을 때 -1을 반환
		- errno는 EWOULDBLOCK/EAGAIN 설정
- write/send
	- BLOCK
		- write 버퍼가 가득 찼을 때 block
	- NON_BLOCK
		- write 버퍼가 가득 찼을 때 -1 반환
		- errno는 EWOULDBLOCK/EAGAIN 설정
- accept
	- BLOCK
		- backlog(현재의 connection 요청 큐)가 비었을 때 block
	- NON_BLOCK
		- backlog가 비었을 때 -1 반환
		- errno는 EWOULDBLOCK/EAGAIN 설정
- connect
	- BLOCK
		- connection이 완전히 이루어질 때 까지 block
	- NON_BLOCK
		- connection이 완전히 이루어지지 않아도 return됨
		- 이 때 -1이 반환되고 나중에 getsockopt로 확인 가능
			- 근데 getsockopt는 허용된 함수가 아님
		- errno는 EWOULDBLOCK/EAGAIN 설정

### lseek

```c
off_t lseek(int fd, off_t offset, int whence)
```

파일의 읽기/쓰기 위치를 파일의 처음 위치로 초기화 합니다.
파일의 위치는 기준 옵션에 따라 앞으로 또는 뒤로 읽기/쓰기 위치로 건너 뜁니다.

| whence | 설명 |
| -- | -- |
| SEEK_SET | 파일의 시작 |
| SEEK_CUR | 현재 읽기/쓰기 포인터 위치 |
| SEEK_END | 파일의 끝 |

건너 뛴다는건 인수로 받은 숫자의 위치로 이동하는게 아닌 건너 뛰듯이 Count 한다는 뜻입니다.

SEEK_END에서 0 위치에 해당하는 읽기 쓰기 포인터의 위치가 파일의 끝이기에 크기를 구할 수 있습니다.

```C
sz_file = lseek(fd, 0, SEEK_END);
```

- fd
	- file descriptor
- offset
	- 이동할 바이트 수
- whence
	- 시작 시점

반환 값은 변경된 읽기/쓰기 포인터입니다.

### fstat

```c
int fstat(int fd, struct stat *buf)
```

- fd
	- file descriptor
- buf
	- 파일의 상태 및 정보를 저장할 buf 구조체

```c
struct stat {
	dev_t		st_dev;		// ID of Device containing file
	ino_t		st_ino;		// inode number
	mode_t		st_mode;	// 파일의 종류 및 접근 권한
	nlink_t		st_nlink;	// hardlink 된 횟수
	uid_t		st_uid;		// 파일의 owner
	gid_t		st_gid;		// group ID of owner
	dev_t		st_rdev;	// device ID (if special file)
	off_t		st_size;	// 파일의 크기
	blksize_t	st_blksize;	// blocksize for file system I/O
	blkcnt_t	st_blocks;	// number of 512B blocks allocated
	time_t		st_atime;	// time of last access
	time_t		st_mtime;	// time of last modification
	time_t		st_ctime;	// time of last status change
};
```

이 함수는 아직 써보질 않았는데 앞으로도 안쓸꺼 같음

정상적으로 조회되면 0을 반환하고 에러가 발생하면 -1을 반환하고 errno에 오류가 설정 됨

## ctime

### time

```c
time_t time(time_t *timeptr)
```

가장 기본적인 시간 관련 함수로 1970년 1월 1일 0시(UTC)부터 인자 값까지 흐른 시간을 초 단위로 반환합니다.

인자로 NULL을 전달하면 현재까지 흐른 시간이 반환됩니다.

```C
time_t	t;
t = time(NULL);
print("%ld\n", t); // time_t는 long을 typedef한 값
```

### localtime

```c
struct tm *localtime(const time_t *timeval)
```

time 함수를 사용하면 초 단위로 계산을 해야 하기에 지금이 몇년 몇월 몇일인지 구분하기 힘듭니다.

```c
struct tm {
	int	tm_sec;			//초
	int	tm_min;			//분
	int	tm_hour;		//시
	int	tm_mday;		//일
	int	tm_mon;			//월(0부터 시작)
	int	tm_year;		//1900년부터 흐른 년
	int	tm_wday;		//요일(0부터 일요일)
	int	tm_yday;		//현재 년부터 흐른 일수
	int	tm_isdst;
};
```

### ctime

```c
char *ctime(const time_t *time)
```

구조체를 사용하고 싶지 않고 곧바로 문자열로 받고 싶을 때 사용합니다.

반환되는 값은 다음과 같습니다.

```
WWW MMM DD HH:MM:SS YYYY

WWWW	요일이 영어 3글자로 표현
MMM	월이 영어 3글자로 표현
DD	날짜를 숫자 두 글자로 표현
HH	시를 숫자 두 글자로 표현
MM	분을 숫자 두 글자로 표현
SS	초를 숫자 두 글자로 표현
YYYY	년도를 숫자 네 글자로 표현

ex)
Sun Jan 27 21:07:50 2019
```

## 참고

- socket 관련
	- <https://www.it-note.kr/category/C%EC%96%B8%EC%96%B4%20header/sys%20%7C%20socket.h>
	- select
		- <https://jangsalt.tistory.com/entry/C-%EC%96%B8%EC%96%B4-select-%ED%95%A8%EC%88%98>
		- <https://man7.org/linux/man-pages/man2/select.2.html>
	- fcntl
		- <https://www.joinc.co.kr/w/Site/system_programing/File/Fcntl>
- BLOCK/NON_BLOCK
	- <https://jangpd007.tistory.com/251>
- fstat
	- <https://www.it-note.kr/161>
- ctime
	- <https://reakwon.tistory.com/66>