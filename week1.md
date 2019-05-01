# 1주차 
- - - -

## TCP/IP 5 Layer Network Model
1. Physical Layer : 연결된 물리적 디바이스 
	* ex ) 케이블, 커넥터, …. 
2. Data Link : 신호들을 번역하는 방법들을 정의.
	* cf ) 이더넷 : 같은 네트워크나 링크상의 노드들에 대한 데이터를 get 하는 프로토콜의  책임을 정의한다.
3. Network : 다른 네트워크들이 router(장비)을 통해 서로 통신이 가능하게 해 준다.
	* ex.) IP ( Internet Protocol )
4. Transport : 어느 클라이언트, 혹은 서버 프로그램이 데이터를 취득할지 sort out 한다.
	* ex. ) TCP ( Transmission Control  Protocol ) , UDP ( User Datagram Protocol )
5. Application

ex. ) 택배전송 묘사: 네트워크 5계층을 택배전송에 비유하면 다음과 같다.

	* ![](week1/screenshot%202019-04-30%20PM%209.25.45.png)

	* 트럭, 도로( Physical ) -> 한 블럭 사이를 어떻게 갈것인가( Data Link ) -> a : b 로 가기 위해 어떤길을 선택할것인가 ( Network ) -> 배송지에 어떻게 노크할것인가 ( Transport ) -> 소포 ( Application )

cf ) OSI 7계층 : 5계층과는 달리 Application Layer을 총 3개의 층으로 추상화 한다.


## The Basics Of Networking Devices
1. Cables : 여러 디바이스들을 물리적으로 연결.
	1. 구리 : cat5, cat5e, cat6 
		* 버전이 올라갈수록, cross-talk( :각 선간의 상호 간섭 )현상을 해결.
		* but, 최대길이가 짧다는 단점.
	2. 섬유 : 광통신

2. Hub & Switch
	1. Hub : Physical Layer의 device.  **같은 네트워크** (LAN : Local Area Network)의 여러 컴퓨터와 **동시에** 연결시켜줌.
		* cf ) Collision Domain : 특정 시간엔 오직 한 디바이스만 통신할 수 있는 하나의 네트워크 세그먼트.
	2. Network Switch : Hub와는 달리 한 단계 위(Data Link) 층의 디바이스이다.
		* 스위치는 ethernet 프로토콜 데이터의 내용을 inspect 할 수 있다. -> 스위치는 각각의 포트(연결된 회선 하나)가 하나의 Collision Domain에 있음.
		* ![](week1/screenshot%202019-04-30%20PM%209.44.01.png)

3. Routers : 독립적인 네트워크들 사이에서 데이터를 forward하는 방식을 알고 있는 장비.  ( Network Layer Device)
	* Internal Table
	* ISP : Internet Service Provider
	* BGP ( Border Gateway Protocol ) : 라우터는 이 프로토콜을 통해 서로 데이터를 주고 받는다. 이 프로토콜은 라우터들이 forward traffic에 최적화된 경로를 학습하는것을 도와준다.

4. Server & Clients : 
	* Clients : 정보를 요청하고, 수신하는 노드.
	* Server : 요청받은 정보들을 제공하는 노드.
	* **주의 : 한 노드가 두 역할을 모두 할 수도 있음.**

## The Physical Layer
1. Moving Bits Across The Wire
	* Devices
	* Bit
	* Modulation : 케이블간을 왔다갔다 하는 voltage를 다양화 하는 방법의 표준.

2. Twisted Pair Cabling and Duplexing
	* Duplex Communication : 케이블의 양방향에서 정보가 흘러갈수 있게 하는 개념![](week1/screenshot%202019-05-01%20AM%2011.09.52.png)
	* Simplex Communication : 단방향 ![](week1/screenshot%202019-05-01%20AM%2011.07.14.png)
	
3. Network Ports and Patch Panels
	* Network Port : 디바이스에 직접적으로 부착되며, 컴퓨터 네트워크를 구성한다.![](week1/screenshot%202019-05-01%20AM%2011.13.47.png)
	* Patch Panel : 많은 포트들을 containing하는 장비. 하지만 이것 이외의 역할은 없다.


## The Data Link Layer
1. Ethernet and MAC Address
	* Ethernet
		* CSMA/CD : 언제 커뮤니케이션 채널이 clear한지, 언제 디바이스가 데이터를 전송하기에 free 한지 결정하는데에 사용된다.
	* MAC Address : globally unique한 식별자. 네트워크 인터페이스에 부착된다.
		* OVI (Organizationally Unique Identifier ) : MAC Address의 첫 3개의 octet. IEEE 엔지니어 들에 의해 부여된다.
		* VA : 나머지 바이트. 임의의 제조사들에 의해 부여되는 번호.

	-> **이더넷**은 **MAC Address**를 데이터 전송시에, 전송하는 머신과 수신하는 머신의 MAC Address를 모두 가지고 확인하는데에 사용한다. 

2. Unicast, Multicast, Broadcast
	* Unicast : 수신주소가 오직 딱 하나인 전송. ( 이더넷 프레임에서 첫번째 옥텟의 최소비트가 0이다. )![](week1/screenshot%202019-05-01%20PM%201.18.43.png)
	
	* Multicast : 수신주소가 여러곳인 전송.![](week1/screenshot%202019-05-01%20PM%201.17.44.png)
	
	* Broadcast : 수신주소가 모든 LAN에 연결된 대상인 전송, **모든 주소값이 F이다.**![](week1/screenshot%202019-05-01%20PM%201.16.43.png)

3. Dissecting an Ethernet Frame
	* Data Packet : 모든 네트워크 링크를 오고가는 모든 바이너리 데이터들을 나타내는 포괄적인 용어.
	* Ethernet Frame : 상당히 구조화된 특정 순서로 나타내어지는 정보의 Collection (데이터 패킷은 이더넷 레벨 안에 있음)
	 ![](week1/screenshot%202019-05-01%20PM%201.28.32.png)
		* Preamble
			* ~ 7byte : 0과 1의 반복
			* SFD ( Last Byte ) : Start Frame Delimiter
				*  이더넷 프레임의 시작부근에서  이전 7바이트 직후에 붙어지는 1  바이트 짜리의 비트열(10101011)
				* SFD 비트열부터는  바이트 단위로 구성되어 있다는 사실을 알리는  프레임 동기용  비트 임 .
				* 마지막 2개의 연속된 1, 즉 11 에 의해  [프레임](http://ktword.co.kr/abbr_view.php?nav=&m_temp1=849&id=45) 의 시작을  [수신기](http://ktword.co.kr/abbr_view.php?nav=&m_temp1=921&id=1183) 가 알게됨

		* Destination MAC Address : 신호가 향하는 곳(디바이스) 의 하드웨어 주소.
		* Source Address : 이더넷의 출처 주소. -> cf) 6bytes = 48bit = len(mac addr)
		* TAG ( VLAN header ) : VLAN 프레임 자체를 지칭함.
			* VLAN( Virtual LAN ) : 한 물리디바이스가 여러 논리적 lan주소를 가질수 있도록 하는 기법.
		* Ether-Type-Field : 프레임의 contents의 프로토콜을 명시 하는데에 사용됨. 16비트 길이.
		* Payload : 헤더를 제외한 모든 정보. 실제 데이터의 내용이 들어있다.
		* FCS ( Frame Check Sequence ) : 전체 프레임에 대한 **checksum value**를 나타내는 4바이트의 숫자.
			* checksum value : 프레임에 대하여 **순환 중복검사**를 수행한 계산값.
			* 순환중복 검사 : 데이터의 무결성 에 있어서 중요한 개념. 네트워크 전송 뿐만 아니라, 여러 컴퓨팅 테크닉에서 사용된다.

		