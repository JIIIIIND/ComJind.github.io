---
title: "ft_irc"
excerpt: "42Seoul irc Guide documents"
date: 2021-02-15 03:44:57 +0900
categories: 42Seoul
---

## external function

[함수 정리]({{ "/42Seoul/5Circle/irc_function" }})

## mandatory

- IRC server를 CPP로 구현합니다.
- CPP의 버전은 98입니다.
- 다음 RFC를 따릅니다.
	- RFC 1459 <https://tools.ietf.org/html/rfc1459> [IRC Protocol](https://www.notion.so/RFC-1459-7d56f67f1dfa4400b1b2daa7bd52ebb9)
	- RFC 2810 <https://tools.ietf.org/html/rfc2810> [IRC Architecture]({{ "/42Seoul/5Circle/RFC2810" }})
	- RFC 2811 <https://tools.ietf.org/html/rfc2811> [IRC Channel Management]({{ "/42Seoul/5Circle/RFC2811" }})
	- RFC 2812 <https://tools.ietf.org/html/rfc2812> [IRC Client Protocol]({{ "/42Seoul/5Circle/RFC2812" }})
	- RFC 2813 <https://tools.ietf.org/html/rfc2813> [IRC Server Protocol]({{ "/42Seoul/5Circle/RFC2813" }})
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
- 디펜스 중에 여러 IRC 클라이언트가 사용될 수 있습니다
- 디펜스 중 서버 간 통신의 경우 공용 irc 서버와 상호 작용해야 합니다

## IRC 사용 방법

C언어로 구현된 Ngircd를 참고하면서 irc의 사용 방법을 익혔습니다.

<https://github.com/ngircd/ngircd>

### ngircd.conf

conf파일은 크게 Global, Limits, Option, SSL, Operator, Server, Channel 영역을 가집니다.

Global에는 해당 ircd에 대한 전반적인 정보가 담겨 있습니다.

```
[Global]
	Name = irc.example.net
	AdminInfo1 = Debian User
	AdminInfo2 = Debian City
	AdminEMail = irc@irc.example.com
	;HelpFile = /usr/share/doc/ngircd/Commands.txt
	Info = Yet another IRC Server running on Debian GNU/Linux
	;Listen = 127.0.0.1,192.168.0.1
	MotdFile = /etc/ngircd/ngircd.motd
	;MotdPhrase = "Hello. This is the Debian default MOTD sentence"
	;Network = aIRCnetwork
	;Password = wealllikedebian
	;Ports = 6667, 6668, 6669
	ServerGID = irc
	ServerUID = irc
```

Name은 말 그대로 현재 ircd의 서버 이름을 나타냅니다. 서버를 특정하기 때문에 고유한 이름을 사용해야 하고, RFC에 정의된 규칙에 따라 작성합니다.

AdminInfo1, 2, Email은 INFO 명령어의 Reply에 사용됩니다.
Info는 서버 간 연결에서 해당 서버를 소개하기 위한 문구입니다.
MotdFile은 Message Of The Day 파일을 나타내고, 클라이언트가 접속할 때 나타낼 파일입니다. 아스키 아트를 넣어도 되고 적당히 하고 싶은 말을 넣어도 됩니다.

여기에 설정되는 Password는 Client로 접속 할 때 입력해야 할 비밀번호입니다.

다음으로 Limits에는 각종 제한 사항들을 설정합니다.

```
[Limits]
	ConnectRetry = 60
	;IdleTimeout = 0
	MaxConnections = 500
	MaxConnectionsIP = 10
	MaxJoins = 10
	;MaxNickLength = 9
	;MaxListSize = 100
	PingTimeout = 120
	PongTimeout = 20
```

연결된 서버/클라이언트가 PingTimeout의 시간 동안 활동이 없다면 Ping을 보내 연결이 제대로 된 건지 확인합니다.
Ping을 보내고 PongTimeout동안 어떠한 응답도 없다면 해당 연결을 끊습니다.

나머지 값들은 모두 이름대로 최대 연결 가능한 개수를 나타냅니다.

JOIN은 채널에 참여 가능한 클라이언트 수를 의미합니다.

Operator에는 네트워크 관리자가 되기 위한 이름과 비밀번호를 설정할 수 있습니다.

```conf
[Operator]
	Name = TheOper
	Password = ThePwd
	;Mask = *!ident@somewhere.example.com
```

명령어들 중 일부는 관리자 권한이 있을 때에만 실행이 가능한 명령어들이 있습니다.
권한이 없다면 NOPRIVILEGES 오류를 출력합니다.

채널 관리자와는 다릅니다. 이 Operator는 네트워크의 관리자이며 명령어를 실행하면 ```MODE <nick> :+o ```를 전체 네트워크에 전파하여 해당 유저가 관리자가 되었음을 알립니다.
```MODE <nick> :+o``` 명령은 따로 사용할 수 없고 OPER 명령어를 통해서만 사용할 수 있습니다.

다음으로 서버 간 연결을 위해 사용되는 Server 파트입니다.

```
[Server]
	Name = irc2.example.net
	Host = irc.example.net
	Bind = 10.0.0.1
	Port = 6667
	MyPassword = MySecret
	PeerPassword = PeerSecret
	;Group = 123
	;Passive = no
	;SSLConnect = yes
	;ServiceMask = *Serv,Global
```

Name은 연결 될 서버의 이름입니다. Global의 Name과 마찬가지로 전체 네트워크에서 고유한 값을 가집니다.
Host는 해당 서버가 연결 될 서버이며 서버 일므으로 작성하거나 외부 네트워크에 접속된다면 해당 IP로 설정합니다.
Port는 해당 서버가 연결 될 Port 번호입니다.

예를 들어 Port에 4000을 설정했다면 다음과 같이 연결할 수 있습니다.

```sh
./ircserv <host ip>:6667:<MyPassword> 4000 <PeerPassword>
```

MyPassword와 PeerPassword는 위의 예에서 작성한 것과 같이 연결 할 서버와 연결 될 서버의 비밀번호입니다.

### 동작 순서

로컬 호스트에 ngircd를 실행했다면 다음과 같이 nc 명령어를 사용하여 접속할 수 있습니다.

```shell
nc 127.0.0.1 6667
```

만약 Global 영역의 비밀번호를 설정한 상태라면 USER와 NICK 명령어 이전에 PASS를 설정해야 합니다.
PASS는 USER/NICK을 입력하기 전에는 계속하여 변경할 수 있으며 가장 마지막에 입력한 값을 사용합니다.

```shell
PASS asdf
PASS asdfasdfasdf
PASS wealllikedebian
NICK jinwkim
USER <username> <hostname> <servername> <realname>
```

RFC에서는 PASS->NICK->USER 순서를 사용하라고 하지만 ngircd에서는 PASS->USER->NICK을 사용해도 문제는 없습니다.
RFC 2812에서는 USER의 매개 변수가 다르게 나타나는데, 사실 hostname과 servername은 사용자가 입력한 값은 무시하고 서버측에서 관리하기 때문에 아무 값이나 넣어도 됩니다.

정상적으로 PASS와 NICK, USER를 입력했다면 irc 서버에 접속하게 됩니다.
MOTD와 서버 및 네트워크에 대한 정보가 출력되고 그 이후부터는 각종 명령어를 사용하며 채팅을 하면 됩니다.

기본적인 채널 생성 및 메시지 전송은 다음과 같습니다.

```shell
JOIN #test
PRIVMSG #test :hihihi
PRIVMSG <nick> :hihihihhihihi
```

PRIVMSG 다음에 채널 이름이 오면 해당 채널에 속한 유저들에게 메시지가 전송이 되며, nick이 들어오면 해당 유저에게만 메시지를 전송합니다.

특정 서버에 연결을 하거나 특정 서버의 연결을 끊고 싶다면 관리자 권한을 가져야 합니다.
위의 설정 파일에 설정해둔 이름과 비밀번호를 다음과 같이 OPER 명령을 사용하여 실행하면 관리자가 될 수 있습니다.

```shell
SQUIT irc2.example.net :i want close that server
#:<prefix> 481 <nick> :Permission denied
OPER TheOper ThePwd
SQUIT irc2.example.net :i want close that server
```

아쉽게도 ngircd에서는 서버가 어떤 메시지를 받았는지, 어떻게 처리했는지 자세히 알려주진 않지만 연결이 종료되었음을 알 수 있습니다.

위의 동작들 이후론 RFC를 읽어보면서 명령어들을 하나씩 실행하면 됩니다.
ngircd는 RFC 1459를 기반으로 서버간 연결에서는 RFC 2813을 따릅니다.
하지만 1459에 설명된 것과 조금씩은 다른 동작을 하기도 합니다.
