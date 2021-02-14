---
title: "ft_irc"
excerpt: "42Seoul irc Guide documents"
date: 2021-02-15 03:44:57 +0900
categories: 42Seoul
---

## external function

### int socket(int domain, int type, int protocol)

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

### int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen)

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

### int getsockname(int sockfd, struct sockaddr *addr, socklen_t *addrlen)

sys.socket.h를 포함해야 합니다.

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

### struct protoent *getprotobyname(const char *name)

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

### struct hostent *gethostbyname(const char *name)

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

### int getaddrinfo(const char *hostname, const char *service, const struct addrinfo *hints, struct addrinfo **result)

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

### void freeaddrinfo(struct addrinfo *res)

sys/types.h, sys/socket.h, netdb.h를 포함해야 합니다.

위의 getaddrinfo의 결과로 만들어진 result를 할당 해제합니다.

<https://linux.die.net/man/3/freeaddrinfo>

### int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen)

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

### int connect(int sockfd, const struct sockaddr *serv_addr, socklen_t addrlen)

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

### int listen(int sockfd, int backlog)

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

### int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen)

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

### uint16_t htons(uint16_t hostshort);

arpa/inet.h를 포함해야 합니다.

short 메모리 값을 호스트 바이트 순서에서 네트워크 바이트 순서로 변경합니다.

호스트 바이트 순서는 지금 사용 중인 시스템에서 2바이트 이상의 큰 숫자 변수에 대해 바이트를 메모리 상에 어떻게 배치하는지 순서를 말하며, 네트워크 바이트 순서는 2바이트 이상의 큰 숫자에 대해 어떤 바이트부터 전송할지에 대한 순서를 말합니다.

2바이트 이상의 큰 숫자를 전송할 때, 큰 단위를 먼저 전송할지 작은 단위를 먼저 전송할지 미리 정하지 않으면 문제가 발생합니다.

네트워크에서는 Big-Endian을 사용합니다.
Big-Endian을 사용하는 시스템은 그대로 전송하면 되지만 Little-Endian을 사용하는 호스트에서는 바이트 순서를 바꾸어서 전송해야 합니다.

하지만 코드를 통해 맞춘다면 시스템에 독립적인 프로그램을 작성할 수 없기 때문에 htons 함수를 사용합니다.

### uint32_t htonl(uint32_5 hostlong)

long 형 호스트 바이트 순서 데이터를 네트워크 바이트 순서값으로 구합니다.

### uint16_t ntohs(uint16_t netshort)

short 형 네트워크 바이트 순서 데이터를 호스트 바이트 순서 데이터 값으로 구합니다.

### uint32_t ntohl(uint32_t netlong)

long 형 네트워크 바이트 순서 데이터를 호스트 바이트 순서 데이터 값으로 구합니다.

### inet_addr

### inet_ntoa

### send

### recv

### lseek

### fstat

### fcntl

### select

### FD_CLR

### FD_COPY

### FD_ISSET

### FD_SET

### FD_ZERO

## mandatory

- IRC server를 CPP로 구현합니다.
- CPP의 버전은 98입니다.
- 다음 RFC를 따릅니다.
	- RFC 1459 <https://tools.ietf.org/html/rfc1459> IRC Protocol
	- RFC 2810 <https://tools.ietf.org/html/rfc2810> IRC Architecture
	- RFC 2811 <https://tools.ietf.org/html/rfc2811> IRC Channel Management
	- RFC 2812 <https://tools.ietf.org/html/rfc2812> IRC Client Protocol
	- RFC 2813 <https://tools.ietf.org/html/rfc2813> IRC Server Protocol
	- RFC 7194 <https://tools.ietf.org/html/rfc7194> IRC TLS/SSL
- client를 구현 할 필요는 없습니다.(보너스 파트)
- 서버간 통신을 처리해야 합니다.
- iostream, string, vector, list, queue, stack, map, algorithm를 추가할 수 있고 사용할 수 있습니다.
- TLS통신 처리를 위해 암호화 라이브러리를 사용할 수 있습니다.
- 보너스 처리에는 다른 함수들도 사용할 수 있습니다.
- 다음과 같은 형식으로 실행됩니다.
	- host는 IRC가 기존 네트워크에 연결하기 위한 호스트 이름입니다.
	- port_network는 IRC가 호스트에서 연결해야 하는 서버 포트입니다.
	- port는 서버가 들어오는 연결을 수락 할 포트 번호입니다. TLS포트를 위해 하나를 추가합니다.
	- 암호는 서버에 연결하려는 IRC클라이언트 똔느 서버에 필요한 암호입니다.
	- host, port_network, password_network가 지정되지 않은 경우 새 IRC 네트워크를 만들어야 합니다.

```bash
./ircserv [host:port_network:password_network] <port> <password>
```

- 서버는 가능한 서버와 함께 여러 클라이언트를 동시에 처리할 수 있어야 하며 중단되지 않아야 합니다. 분기는 허용되지 않으며 모든 IO작업은 차단되지 않아야 하고 모두에 대해 하나의 선택만 사용해야 합니다(읽기, 쓰기, 수신 등등)
- fcntl은 다음과 같은 형태만 사용할 수 있습니다. (fcntl(fd, F_SETFL, O_NONBLOCK)), 그리고 다른 플래그는 금지됩니다.
- 모든 오류를 확인하고 문제가 있을 수 있는 경우에 대한 처리가 필요합니다(일부 데이터만 수신된 경우, 낮은 대역폭 등등)
- 서버가 보낸 모든 것을 올바르게 사용하는지 확인하려면 nc를 사용하여 초기 테스트를 수행 할 수 있습니다.(명령의 일부를 보내려면 ctrl+D 사용)
- 디펜스 중에 여러 IRC 클라이언트가 사용될 수 있습니다.
- 디펜스 중 서버 간 통신의 경우 공용 irc 서버와 상호 작용해야 합니다.