![스크린샷 2024-10-16 162717](https://github.com/user-attachments/assets/ad8ae816-3762-427f-85a9-79f4eb3df123)# computer-network
컴퓨터 네크워크 정리

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

