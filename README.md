# computer-network
컴퓨터 네크워크 정리

![스크린샷 2024-10-16 162717](https://github.com/user-attachments/assets/ad8ae816-3762-427f-85a9-79f4eb3df123)

host = end systems (네트워크 앱을 실행하는 부분) -> clients, servers
communication links(wireless links, wired links)
packet switches(routers, switches)

인터넷 프로토콜: 네트워크 상에서 메시지를 주고 받는 동안 포맷, 순서등을 결정해주는 약속

host가 메시지를 보낼 때 pakcet이라고 알려져있는 L비트의 chunks로 쪼개서 보낸 후 패킷을 access network로 R의 transmission rate로 보낸다. application-layer에 있는 메시지를 쪼개는 것을 packet switching이라고 함
이 때, 바로 보내지는 것이 아니라 delay가 발생하는데 이 때의 delay를 packet transmission delay라고 하고 L(bits)/R(bits/sec)로 나타낸다.

## circuit-switching(전화에서 사용)
1.메시지를 보내기 전 경로 셋업, 작업 예약을 미리 한 후 정해진 경로대로 메시지를 전송하는 방법
2.경로가 미리 정해져있기 때문에 store and forward가 아니라 쭉 데이터를 전송함
![스크린샷 2024-10-16 142822](https://github.com/user-attachments/assets/70c357e2-229d-4f8a-8a0e-37468f466732)

## packet-switching(인터넷에서 사용)
1.메시지를 보내기 전의 작업은 이루어지지 않고 메시지를 라우터에 전송한 후 가능한 경로로 쭉 이동하는 방법. 패킷 크기 제한, 받는 사람, 보내는 사람 정보가 패킷 안에 들어있어야 하고 경로가 상황에 따라 계속 바뀜.
2.store and forwatd 방식으로, 패킷 전체가 라우터에 전송되어야만 다음 링크로 전송을 할 수가 있다. 
3.링크로 들어오는 패킷이 링크의 transmission rate를 초과한다면 queue에 패킷이 쌓이고(delay 발생) 쌓인 패킷들은 버려질 수도 있다(loss). queueing delay, queueing loss라고 
![스크린샷 2024-10-16 142337](https://github.com/user-attachments/assets/97ecc46a-454e-4edb-8996-83f78c2d85b6)


packet switching이 circuit switching보다 더 많은 유저를 받을 수 있다. circuit switching 방법은 L/R만큼의 유저만 수용할 수 있지만 packet switching 방법은 유저를 많이 수용 가능하지만 그만큼 성능 저하가 일어남( queueing delay, loss)
성능 저하 확률 : 수용 가능 buffer의 크기를 n, 패킷 수를 m 이라고 할 때 1 - (m에 대한 0부터 n까지의 binormal distribution) 으로 구함 , 버퍼의 크기에 꽉 차면 성능 저하가 발생하기 때문에

packet & circuit switching을 통해 transmission delay와 propagation delay를 구해보았다.
tansmission delay : L/R , propagation delay: 링크 길이 D / 빛의 속도 C 로 구한다.
![KakaoTalk_20241016_145922321](https://github.com/user-attachments/assets/7ae1aa51-dad3-4d72-8726-a0346e57e21b)
![KakaoTalk_20241016_145450620](https://github.com/user-attachments/assets/d9b19156-954a-4d71-a7fb-85b1133d6d84)

다음과 같이 두 방법의 delay는 달라지고 각각의 방법이 더 유용하게 쓰이는 곳이 존재함. 상황에 따라 두 방법 중에 한 방법을 사용한다.

네트워크 연결이 일어날 때, access network끼리만 연결 할 수 있다면 너무 복잡해지기 때문에 여러 개의 aceess network를 연결 할 수 있는 router와 router끼리 연결할 수 있는 ixp(interner exchange poiont)로 구성되어져 있다. 그리고 비슷한 end users들끼리 묶어서 regional network를 이뤄 라우터에 연결되기도 한다.

### four sources of packet delay
Dnodal = Dproc + Dqueue + Dtrans + Dprop
Dtrans와 Dprop은 위에서 구한 delay들이고 Dproc는 비트 에러를 check하고 다음 연결될 링크를 결정하는 데 발생하는 delay이다. 매우 작기 때문에 보통 무시한다.
Dqueue는 queueing delay이고 패킷이 전달되기 전 링크에서 기다리는 시간이며 router의 복잡도에 의존한다. a를 평균 패킷 도착 rate라고 하면 queueing delay는 La/R 로 나타내고 이 값이 0.7~0.8정도 되면 delay가 급격히 상승한다. (평균 값이기 때문에 1이 안되도 delay 발생)
buffer가 꽉 차 있는 상태에서 packet이 들어오면 그 패킷은 loss된다.
![스크린샷 2024-10-16 151344](https://github.com/user-attachments/assets/4ec2f36e-9e78-40f5-9785-6c534e23be45)


throughtput : sender에서 reciever로 bit가 전달되는 rate
실제 sender에서 reciever로 메시지를 전송할 때 sender의 링크에서 bits/sec, reciever의 링크에서 bits/sec에 따라서  throughput이 대게 영향을 받는다.

### protocol layers
네트워크 계층에서는 layer를 나누어 데이터를 보내고 받는다.

##### layering을 하는 이유
1. 복잡한 시스템을 다루기 때문에 명확한 구조를 나누어 각각 서비스를 제공하기 위해
2. 유지보수를 쉽게 하기 위해 modulization을 하고 시스템을 업데이트 하기 위해

##### internet protocol stack
###### application
네트워크 application을 지원(FTP, SMTP, HTTP)
###### transport
process-process data transfer(TCP, UDP)
###### network
source에서 destination까지 datagrams를 라우팅(IP, routing protocols)
###### link
data transfer between neighboring network elements(Ethernet, WiFi)
###### physical
bits on the wire


## Application architecture
possible structure of applications:
1.client-server
2.peer-to-peer(p2p)

#### client-server architecture
server : 고정된 ip 주소를 가짐, 항상 host이다. client의 접촉을 기다림
client : 필요할 때만 server에 접근, 동적 ip 주소를 가짐. server에게 대화를 사도함

#### p2p architecture
server가 항상 켜져있지 않음
메시지를 받은 사람이 동시에 다른 사람에게 메시지를 보내 줄 수 있기 때문에 사용자 수가 늘어나도 네트워크를 증설할 필요가 없다.
client인 동시에 server라고 생각하면 편할 것 같다.

#### Sockets
sender와 reciever가 메시지를 주고 받을 때 각각의 소켓을 통해 주고받는다.
![Uploading 스크린샷 2024-10-16 160644.png…]()



## Transport 
app이 필요한 transport의 service : data inergrity, throughput, timingm security
data integrity: 어떤 app은 100% reliable한 데이터를 필요로 하기도 하고 loss를 허락하는 app도 있다.
timing: 어떤 app들은 낮은 delay를 요구하기도 한다.
throughput: 어떤 app들은 최대한 낮은 throughput을 요구한다.
security: 보안에 관련된 기능들을 요구한다.

#### internet tansport protocols services
##### TCP 
reliable transport
flow control: sender won't overwhelm receiver
congestion control: throttle sender when network overloaded
does not provide: timing, minimum throughput gurantee, security
connection-oriented: setup between client and server
ex)mail , web, file transfer ...

##### UDP
unreliable data transfer
does not provide: reliability, flow control, congestion control ......
ex) streaming multimedia, internet telephony ....

## Web and HTTP
웹 페이지는 여러 objects들로 구성되어져 있고 object는 html파일, jpeg 파일, java applet등 여러가지 파일로 구성되어져 있다.
웹 페이지는 base HTML-file이라는 파일을 가진는데 참조 오브젝트들을 포함하는 파일이다.

#### HTTP: hyper text transfer protocol
웹의 application layer protocol이다.
client/server model로 구성된다.
TCP를 사용하며 클라이언트와 서버가 TCP로 연결해 메시지를 주고 받고 TCP 커넥션을 닫는다. 과거의 정보를 저장하지 않는다.
cookie를 사용하면 정보를 저장할 수 있다.

##### non-persistent HTTP
최대 한 개의 object가 TCP에 연결될 수 있으며 여러 개의 object를 다운 받으려면 여러 개의 connection이 필요하다.
![스크린샷 2024-10-16 162717](https://github.com/user-attachments/assets/d3ae7ff6-47bc-4acf-baa7-bde0dd6935d5)
![스크린샷 2024-10-16 162723](https://github.com/user-attachments/assets/d6d65a1c-3f24-4a48-a858-314da35572b8)
다음과 같은 방식으로 메시지를 주고 받는다.
RTT: 작은 패킷이 client로부터 서버까지 갔다가 다시 돌아오는데 걸리는 시간이라고 함.
첫 RTT는 TCP 커넥션을 시작하기 위해 필요, 두 번째 RTT는 HTTP request와 적은 bytes의 HTTP 응답을 위해 필요함, 그 후 transmission time 소요
따라서 non-persistent HTTP에서 response time = 2RTT + file transmission time이다.

##### persistent HTTP
다수의 object들이 하나의 TCP 커넥션을 통해 전달되어질 수 있다.
응답을 보낸 후에도 서버는 열려있다.
일련의 HTTP 메시지가 열려있는 서버를 통해 접촉을 함

세 개의 작은 이미지 파일을 포함한 HTML을 주고 받을 때
non-persistent HTTP with no parallel TCP connections에서는 TCP, base HTML을 연결한 2RTT + object 주고 받을 때 TCP연결, object 주고 받는 2RTT이므로 2RTT + 3 * 2RTT = 8RTT
non-persistent HTTP이지만 parallel TCP connections에서는 2RTT + 2RTT = 4RTT가 필요하다
persistent HTTP에서는 object마다 TCP 연결이 필요없기 때문에 3RTT가 필요하고 초기 설정 2RTT가 필요하기 때문에 5RTT가 필요함
persistent HTTP with windowing에서는 2RTT + RTT = 3RTT가 필요함. windowing이 있으면 데이터를 동시에 다 보내버리고 받기 때문에 (매우 작은 파일이기 때문에)RTT만 필요함

http request 형식
![스크린샷 2024-10-16 165354](https://github.com/user-attachments/assets/28a279ff-9d09-448f-8877-2f2fd71648ca)

http response 형식
![스크린샷 2024-10-16 165405](https://github.com/user-attachments/assets/37bc75b7-def1-4f89-9fa5-ce99c92bf01c)

HTTP response status code
200: OK
301: Moved permanently
400: Bad request
404: Not Found

#### web Caches
웹 캐싱이란 클라이언트가 서버에 요청하 내용을 서버에 저장해두는 것을 말한다.
다음에 클라이언트가 같은 요청을 했을 때 서버에서 가지고 있다면 바로 전송해준다.
웹 캐싱을 사용하면 response time을 줄일 수 있고 access link에 들어오는 traffic을 줄일 수 있다.
![스크린샷 2024-10-16 171342](https://github.com/user-attachments/assets/dcf749dd-fb93-46f7-9e7e-079da5e38393)

그림에서 access link의 스피드에 따라서 delay 차이가 많이 난다.
local web cache를 그림에서 추가한다면 delay가 매우 짧아질 수 있다.
![스크린샷 2024-10-16 171452](https://github.com/user-attachments/assets/4e1cbc93-3302-4abe-a4a0-39b23e682f55)



### FTP
파일을 transmit할 때 사용하는 protocol
ftp : RFC 959
ftp server: port 21
TCP를 사용한다. 서버에서 요청을 받으면 control과 data를 분리하여 control은 21번 포트 , data는 20번 포트를 통해 전송해준다.

### SMTP
![스크린샷 2024-10-16 171938](https://github.com/user-attachments/assets/04db8ee6-4983-433a-a244-cc7a66d8bb55)

mail server끼리 데이터를 주고 받음.
port 번호 25번을 사용하고 TCP방식으로 구현함
보내는 사람의 메일 서버에 메시지를 두고 TCP로 받는 사람과 연결, 받는 사람의 mail서버에 메시지를 받는 방식이다.

### DNS
application layer 프로토콜로 www.naver.com을 네이버로 검색하는 것 같은 기능을 한다.(ip 주소와 특정 이름을 mapping한다)
hierarchical databse이다. root DNS 서버가 있고 그 밑에 .com DNS , .org DNS등 다양한 DNS가 있고 클라이언트가 원하는 DNS를 경로를 따라 찾아가서 IP주소를 구한다.
DNS는 reliable하지 않은 UDP 방식을 사용한다.
DNS로 name server를 찾는 방법
![스크린샷 2024-10-16 172947](https://github.com/user-attachments/assets/de62454f-5708-4ac1-a65c-179c16935253)

url의 top level부터 DNS server를 통해 ip주소를 찾는다 중간에 server가 캐시를 가지고 있다면 바로 host에게 정보를 전달해준다.
www.naver.com의 ip 주소를 DNS를 통해 찾았다면 다음에 www.samsung.com의 ip주소를 찾을 때 root DNS말고 .com NS 먼저 컨택한다.


### P2P
P2P는 파일을 받는 동시에 보낼 수 있고 peer들끼리 원할 때만 접촉한다.
비트토렌트나 스카이프 등에서 사용한다
centralized directory에서는 server에서 소유중인 파일 직접 보내주기 때문에 저작권 문제가 발생함
guntella 프로토콜은 이 문제점을 보완하기 위해 나옴, 그저 guntella 집단을 만들어서 그 안에서만 p2p 방식으로 소통

### 파일 분배하는 과정에서 p2p와 client-server의 차이점
하나의 파일 크기를 F, n개의 파일을 보낸다고 가정했을 때 서버에서 업로드 하는 시간을 Us, 각각의 peer가 다운로드 받고 업로드 하는 시간을 Di, Ui라고 하면
client-server에서의 delay는 max(NF/Us, F/Dmin)으로 구할 수 있다. n개의 파일을 업로드 하는 속도와 파일을 peer가 다운로드 받는 속도 중 더 큰것이 delay에 영향을 미친다.
p2p에서의 delay는 max(F/Us, F/Dmin, NF/Us+Ui)로 구할 수 있다. 파일 한 개를 서버가 업로드 하는 시간, 파일을 다운로드 받는 데 가장 오래 걸리는 시간, 모든 파일을 업로드 하고 각각의 peer들이 업로드 하는데 걸리는 시간 중 가장 큰 것이 delay에 영향을 미친다.

n이 커질수록 둘 다 delay가 커지지만 client-server는 선형적으로 증가하고 p2p는 n이 커지는 만큼 분모에서도 Ui가 커지기 때문에 기울기가 더 작게 증가한다.


# Transport Layer
#### TCP
-congestion control
-flow control
-connection setup

#### UDP
-unreliable, unordered delivery
-best effort service
-lost, delivered out-of-order to app

Multiplexting / demultiplexing
transport의 header에 source_port와 destination_port를 설정하여 주고받는 경로를 지정함
tcp에서는 dest ip address, dest port number, source IP address, source port number를 기준으로 소켓을 나눔
udp에서는 dsetination port와 ip가 같다면 같은 소켓에서 처리함

## Principles of reliable data transfer(rdt)
send side에서 rdt_send()를 통해 보내고 udt_send()를 통해 받은 후 unreliable channel을 통해 reciver side에게 rdt_rcv()를 통해 전달

#### rdt1.0
-bit error, packet loss 고려 x
![스크린샷 2024-12-02 185605](https://github.com/user-attachments/assets/d1dc07d1-45c6-4f2d-b15f-880c47de4a84)
application으로부터 받은 데이터를 packet으로 만든 후 receiver에게 전달

#### rdt2.0
-bit error를 고려하여 check sum 도입
-bit error를 확인하기 위해 ack와 nack를 도입함 receiver에서 ack를 보내면 no bit error이고 nack이면 bit error가 존재한다고 인지
-bit error가 발생했다고 하면 packet다시 보냄
![image](https://github.com/user-attachments/assets/cce4de3b-ac8f-4d9a-8b08-176b2647fcb2)

#### rdt2.0 버전에서의 문제
ack와 nack의 전송 도중 문제가 발생한다면 sender는 무슨 일이 일어난 지 알 수 없음
그냥 재전송을 해버렸는데 단순히 ack나 nack가 늦게 도착한거라면 receiver 입장에서는 재전송인지 새로운 전송인지 모름
-> 문제를 해결하기 위해 각 packet과 ack에 sequence를 붙이기로 생각함, stop and wait 프로토콜이기 때문에 번호는 단순히 0,1 두 개만 필요함 -> rdt2.1 프로토콜 고안

#### rdt2.1
![image](https://github.com/user-attachments/assets/227828f4-fc24-443b-948c-e17e45d94436)
sender 입장에서 rdt2.1 프로토콜이 진행되는 모습이다.
-sequence number를 추가함으로써 보내는 도중 corrupt가 발생했어도 재전송인지 새로운 전송인지 알 수가 있다.
![image](https://github.com/user-attachments/assets/2c9d412c-104e-42eb-869d-cf38ce92d104)
receiver 입장에서 rdt2.1 프로토콜이 진행되는 모습이다.
다른 건 쉽게 이해할 수 있는데 헷갈리는 부분이 sequence 번호를 잘못 받은 부분이다. 만약 0번 sequence 데이터를 기다리는 중 1번 data를 전송 받았다면 재전송이라고 인지하고 ack를 보내줘야 한다

#### rdt2.1 에서의 개선할 점
ack와 nack를 둘 다 사용하면 sender 입장에서 자신이 몇 번을 보냈는지 계속 기억해야해서 state가 2배가 됨.
receiver 입장에서 마지막 ack나 nack가 잘 전송된 지 알 수 없음 -> rdt2.2 고안

#### rdt2.2
![image](https://github.com/user-attachments/assets/0cb2d86e-a48a-4a2e-8335-7a21e2b22c9e)
rdt2.2에서는 nack의 불필요함을 인지하고 nack를 없애는 대신 ack에 packet과 같이 sequence number를 붙여주어 몇 번 packet에 대한 ack인지 나타내주었다.
-> 문제는 없지만 sender 입장에서 재전송이 필요할 때 언제 재전송을 해야할 지 설정해줘야 해서 timer를 이용한 rdt3.0을 고안

#### rdt3.0
![image](https://github.com/user-attachments/assets/3a9fa246-176a-4a2c-b328-ae3fc21c5b9b)
타이머를 이용한 rdt3.0에서는 재전송을 시키는 게 아니라 time out을 시킨다.
timer안에 receiver에서 응답이 안 오면 패킷을 재전송하고 중복된 응답은 폐기한다.

지금까지 stop-and-wait 프로토콜에 대한 설명이었는데 이것의 utilization을 확인해보면 (L/R) / (RTT + L/R)로 약 0.00027정도이다.
효율성이 너무 떨어진다고 생각해 pipelining이라는 것을 고안해냈다.
## Pipelineing: 동시에 정해진 수의 packet을 한 번에 보냄으로써 utilization을 높이는 것이다.
![image](https://github.com/user-attachments/assets/61409e29-fd0a-4e12-a579-ae9c4a9b191c) 이런 형식이다.

#### pipelineing protocol #1 Go-Back-N
GBN 프로토콜은 base를 0으로 정해놓고 보낼 수 있는 packet의 수인 window size를 정해놓고 그것만큼 패킷을 한 번에 전송한다. 잘 수신된 패킷이 있다면 base를 1씩 증가시킨다. (0,1 잘 전송되었다면 base는 2)
만약 0번부터 3번까지 보냈고 0,1번 패킷은 잘 수신되고 2번 패킷에서 loss가 발생했다면 receiver는 3번 패킷을 받았을 때 잘 받은 패킷 번호인 ack1을 전송한다. 그 후 잘 전송되었더라도 ack를 못 받은 이전 패킷이 있다면 다 폐기한다.
만약 어떤 packet이 timeout 돼었다면 그 packet부터 window size만큼 한 번에 재전송 한다.
![image](https://github.com/user-attachments/assets/0bb10c4e-dbbb-45c9-8056-e0bbb6c333fb)
이런 식으로 동작한다.

#### pipelineing protocol #2 Selective repeat
selective repeate에서는 go-back-n과 비슷한 방식으로 동작하는데 차이점은 go-back-n 에서는 ack를 못 받은 패킷이 이전에 있다면 그 후의 패킷은 다 discard 하는 것이었는데 selective repeat에서는 discard하지 않고 buffer에 저장해준다.
그 후 timeout이 발생한다면 해당 패킷만 재전송하고 buffer에 저장해놓은 packet들을 사용한다.


## TCP segment structure
![image](https://github.com/user-attachments/assets/5ac60154-347c-4bd2-9a70-09a43ca16d3e)
source, dest port번호 존재, sequence number, ack bumber 존재
U: urgent data(긴급 데이터) 먼저 읽는 데이터, 1이면 urgent 0이면 아무 것도 아님
A: ACK가 유효한지 나타내는 부분
P: application에게 데이터 전달
R: reset, tcp connection 다시 연결
S: Syn flag 연결 설정이면 1, 아니면 0
F: finisth 연결 끊을 때 1, 아니면 0
![image](https://github.com/user-attachments/assets/dd446e92-9a54-4402-8efa-f9879cf377b0)
이런 방식으로 통신
seq: byte 단위 , ack: 잘 받은 데이터 byte + 1 전송
tcp 전송에서 seq:92 보내고 ack: 100을 받은 후 seq:100 ack:120을 받았을 때 ack:100이 loss되었어도 120을 잘 받았으면 seq:92를 재전송 할 필요가 없다.
cumulative하기 때문에 ack 120을 받았으면 그 전 것도 잘 받은 것임
-- tcp fast tranmission
tcp 전송에서 duplicated ack를 3개이상 받는다면 time out이 되기 전에 패킷을 보낸다.
손실된 packet의 ack, 그 후에 전송되는 3개의 같은 ack를 sender에서 받는다면 그 즉시 패킷을 재전송


# Network Layer
 router들끼리의 소통
routing algorithm , forwarding table을 통해 패킷 이동 경로를 정한다.

32bit ip-address
network address와 host address로 나눔. newwork address가 어디까지인지 알기 위해서 prefix 사용
forwarding table에 network address와 prefix를 설정해주고 그것에 맞는 ip들은 그 경로를 통해 이동함

address range    port
165.35.16.0/23    1 
165.35.28.0/23    2
이런식으로 존재 한다고 가정하자

dest address = 165.35.17.4라면 165.35.0001000/1.00000100 으로 나타낼 수 있는데 이것은 165.35.16.0/23에 부합하기 때문에 port1을 통해 전송된다.
dest address = 165.35.19.4라면 같은 방식으로 port2번을 통해 전송된다.

만약  1번과 2번을 둘 다 만족하는 ip가 있다면 더 긴 prefix를 가지는 포트를 통해 전송된다. ---> longest prefix matching

inha univ가 165.35.16.0/23을 가지는 상태에서 4개의 subnet을 가지고 싶다면 prefix를 2 늘리면 된다.
165.35.16.0/25, 165.35.16.128/25, 165.35.17.0/25, 165.35.17.128/25 이렇게 4개의 subnet을 가지게 된다.
각가가 2^7개의 host를 가질 수 있기 때문에 4를 곱하면 주소 낭비는 되지 않는다.
inha univ subnet mask : 255.255.255.128 -> network address로 사용하는 부분을 모두 1로 읽고 아니면 0으로 읽는다.

inha univ 165.35.16.0/23의 관점에서 사용할 수 있는 주소의 수는 2^9 -2이다. (host bit 모두 0이면 초기 address 지칭, 모두 1이면 broadcasting address이기 때문에 제외)
inha univ subnet 4개의 관점에서 바라보면 각각 2개의 broadcasting address와 초기 address 주소를 가지므로 2개씩 빠져서 총 2^9 - 8이다. -> 실질적 호스트 생성에 차이점이 존재한다.

라우터 내부에는 input ports, switch fabric, output ports로 구성되어져있고 input port memory의 포워딩을 사용해 switch fabric을 통해 output port로 전달한다. 
switch fabric에는 memory, bus, crossbar 등이 존재한다.
memory: 패킷이 system의 memory에 저장된다, 메모리의 bandwidth로 스피드 제한
bus: bus의 bandwidth로 스피드 제한
interconnection network: bus의 bandwidth 제한을 극복, 가장 빠름

input, output 둘 다에서 quequing을 하지만 주로 output buffer에서 queueing delay, loss 발생
![스크린샷 2024-12-02 203539](https://github.com/user-attachments/assets/96de7c9a-e802-4a3f-9ed5-be726f8d8850)

input port의 queue에서 delay 발생 할 수도 있음.
Head-of-the-Line(HOL) blocking: 큐를 통과한 datagram들이 다음 큐를 통과하는 datagram들의 움직임을 막는 것
![KakaoTalk_20241202_203805432](https://github.com/user-attachments/assets/ce524eae-a9d5-403d-b172-db61af19d9e7)
다음 상황에서 빨간색 datagram은 동시에 switch fabric으로 전달 불가능 해서 통과를 기다려야 하고 두 번째 그림에서는 green packet은 HOL blocking이 발생함.

### Interner Network Layer
- routung protocols(RIP, OSPF, BGP)를 통한 데이터 이동 경로 설정
- IP protocol : datagram format, addressing conventions, packet handleing conventions
- ICMP protocol : error reporing, router signaling 문제가 생기면 진행하는 프로토콜

## IP datagram format
![KakaoTalk_20241202_204738379](https://github.com/user-attachments/assets/0fc79423-5a4c-46be-b16f-d7882a504a55)
32bit destination IP address까지가 default header이다.
ver: IP protocol version number
head len: header length(bytes)
type of service : type of data
length: total datagram length(bytes) data 포함
16-bit identifier, flags, fragment offset: fragmentation/ reassembly를 위한 부분. 같은 패킷이라면 같은 identifier를 가지고 flags는 자신 다음에 쪼개진 패킷이 존재하면 1 아니면 0이고, offset은 전송 byte를 8로 나누어 설정
time to live: loop 방지, ttl은 라우터를 하나 지날 때마다 1씩 줄어들고 0인데 dest가 아니라면 폐기한다.
upper layer: protocol 종류 알려줌
header checksum: data check 하지 않고 header만 check
data: transport 계층에서 넘겨주는 tcp or udp segment
overhead: 20 bytes of TCP + 20bytes of IP + app layer overhead

#### IP fragmentation, reassembly
어떤 라우터의 max transfer size(MTU)가 IP datagram 보다 작다면 ip datagram은 쪼개져서 보내진다.
![KakaoTalk_20241202_205652396](https://github.com/user-attachments/assets/db66f440-8e15-4807-9df5-b779965b42c9)
다음과 같이 fragment 하고 header는 쪼개진 개수만큼 다 존재한다. offset은 시작 pointer 위치를 정해준다

### IPV4 addressing
각각의 router와 host는 32-bit 인터페이스륵 가진다.
인터페이스는 router/host 사이의 관계를 말하고 라우터는 보통 다수의 인터페이스를 가진다.
라우터의 ip가 223.1.1.4라면 host는 223.1.1.1 ~ 223.1.1.3까지 존재할 수 있다. subnetpart가 host part보다 high order bits를 가진다.

### CIDR
classless interDomain Routing: 가변적인 subnet portion of address

### DHCP
Dynamic Host Configuration Protocol: dynamically get address from as server
동적으로 모든 호스트에게 ip address를 부여해준다. 실시간 모니터링은 불가능하므로 렌탈시간을 줌(1시간을 줬을 때 절반이 되면 연장 패킷 보냄)
host가 DHCP discover 메시지를 broadcasting -> DHCP 서버가 DHCP offer로 응답 -> host가 ip address 요청 -> DHCP 서버 응답 (모두 broadcasting)
transaction ID: 첫 번째 두 단계가 같고 그 다음 두 단계는 +1해서 같다.

### NAT : network address translation
logical network(home network)에서 rest of internet으로 연결되는 라우터가 있고 그 라우터를 통과하면 home network에서 10.0.0.1 같은 형태가 138.75.6.2 같이 정해진 형태로 변환됨
home network에서 나오는 datagram들은 같은 NAT ip address를 가진다.
로컬 네트워크에서 한 개의 ip 주소로 여러 개의 host들을 가질 수 있게 된다.
나가는 데이터는 source IP address, port # -> NAT IP address, new port #로 변경, 들어오는 데이터는 NAT IP ADDRESS, new port # -> source IP address, port #로 전송된다.
나가는 데이터는 다른 데이터라면 모두 다른 포트로 이동해야한다.
port 번호는 transport layer인데 ip layer에서 바꾸기 때문에 layering concept를 위반
![KakaoTalk_20241202_212615227](https://github.com/user-attachments/assets/40751ede-692e-4b47-8d90-eedd1e6ee2aa)
다음과 같이 변경

### ICMP : internet control message protocol
host와 routers가 소통하기 위해 사용, 오류 보고, ping 보내고 받기 등
ICMP message : type, code plus first 8 bytes of IP datagram causing error
type  code       description
 0     0        echo reply(ping)
 3     1        dest host unreachable
 11    0          TTL expired
 8     0           echo request
 이런식으로 구성되오져 있음
source는 UDP segment를 여러개 보내는데 데이터그램의 n번째 set이 nth router에 도착하면 router는 datagram을 버리고 ICMP type 11, code0 번을 보낸다. ICMP 메시지는 라우터와 ip address를 포함한다

### IPV6
IPv4로는 공간이 부족할 것이라고 생각하고 나옴
ipv4는 20 byte기본 header + option 이었지만 ipv6는 고정적인 40 바이트 헤더를 가지며 fragmentation이 불가능하다.
ipv6 datagram format
![KakaoTalk_20241202_214033300](https://github.com/user-attachments/assets/0efb7425-7c8e-49b4-968d-472bd4cda8dd)
pri : priority 확인
flow label: 같은 곳에서 오는 flow
next hdr: 다음에 올 헤더 내용에 대한 정보
hop limit: TTL
header field: 8개
ipv4와 차이점: 속도를 높이기 위해 checksum을 없앰, options가 허락되지만 header의 바깥쪽에 추가해야 한다(next header field를 통해)
ipv4와 ipv6를 통해 소통이 가능하다. tunneling: ipv4 tunnel connecging ipv6 routers


## Routing algorithm classification
global: 모든 라우터들 사이의 cost를 알아야함 "link state algorithms" , static
decentralized: 라우터들은 이웃들의 cost를 알아야함. "distance vector algorithms", dynamic

link state routing algorithm : Dijkstra's algorithm -> D(v) = min(D(v), D(w) + C(w,v)) 이런식으로 계속 구해 나감 -> n(n+1) / 2 comparisions: O(n^2)
distance vector algorithm: 전체 네트워크의 topology를 알 필요없음, 주변의 노드와의 거리를 통해 하나하나 구해나가는 방식

같은 autonomous system에 속한 라우터들은 같은 routing protocol을 사용한다
각 AS에서는 주변의 AS를 통해 어디로 이동할 수 있는지를 구한다. 그 경로를 통해 움직일 수 있고 만약 양쪽 둘다로 이동할 수 있다면 어디로 갈지는 정책마다 다르게 설정한다

RIP : Routing Infromation Protocol
라우터를 통해 dest까지 가는데 next router와 hops to dest를 구함
hops는 라우터를 통해 이동할 때마다 1씩 증가함
![KakaoTalk_20241202_220836699](https://github.com/user-attachments/assets/37ac7c31-a8cc-4cbe-9c65-bd7cb2f75329)
180초 안에 응답 안 오면 고장이라고 판단 -> link 버리고 다른 경로 계산해서 broadcasting, detect는 오래 걸리지만 propagation은 빠름
