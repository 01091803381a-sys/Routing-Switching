## 1. 네트워크 정의 및 필수 구성 요소 (Definition & Core Components)
네트워크는 복수의 단말이 표준 프로토콜(Protocol)에 따라 상호 간 데이터 자원을 공유하도록 연결된 **데이터 전송 인프라(Data Transmission Infrastructure)**를 의미함.

### 1.1 네트워크 4대 필수 구성 요소 (4 Core Components)
1. **End Device (최종 단말):** 데이터의 발신 및 최종 수신지 (PC, Server, Smartphone, IoT Device 등).
2. **Intermediary Device (중간 장비):** 데이터 경로 결정 및 트래픽 중계 (L2 Switch, L3 Router, Firewall, Wireless AP 등).
3. **Network Media (전송 매체):** 데이터가 물리적으로 이동하는 통로 (UTP, Fiber Optic, Radio Waves 등).
4. **Network Protocol (통신 규약):** 데이터 송수신 형식 및 타이밍을 제어하는 논리적 규칙 (TCP/IP 등).

---

## 2. 데이터 통신 체계 (Data Communication System)

### 2.1 데이터 통신의 5대 요소 (5 Elements)
1. **Message (메시지):** 통신의 대상이 되는 정보 유닛.
2. **Sender (송신자):** 메시지를 생성하여 전송을 개시하는 단말.
3. **Receiver (수신자):** 전송된 데이터를 처리하는 최종 목적지 단말.
4. **Transmission Medium (전송 매체):** 메시지가 전달되는 물리적 경로.
5. **Protocol (통신 규약):** 장치 간 상호 운용성(Interoperability)을 보장하는 통신 규칙.



---

## 3. 네트워크 분류 (Network Classification)

### 3.1 지리적 범위에 따른 분류 (Coverage-based)
- **PAN (Personal Area Network):** 개인 공간 내 디바이스 간 초단거리 무선 통신 (Bluetooth, Zigbee).
- **LAN (Local Area Network):** 제한된 지역 내 고속 통신망. 높은 대역폭과 낮은 지연율(Latency) 특징임.
- **MAN (Metropolitan Area Network):** 도시 규모의 연결망. 복수의 LAN을 통합하는 기간망임.
- **WAN (Wide Area Network):** ISP 회선을 임대하여 국가 및 대륙 간을 연결하는 광역망임.

### 3.2 기술 및 목적 기반 분류 (Specialized Networks)
- **VPN (Virtual Private Network):** 공용망 위에서 터널링과 암호화로 구축한 논리적 가상 전용망임.
- **SDN (Software Defined Networking):** 제어부(Control Plane)와 전송부(Data Plane)를 분리하여 중앙 제어함.
- **CDN (Content Delivery Network):** 분산된 Edge Server를 통해 콘텐츠를 사용자 근거리에서 캐싱 및 전송함.
- **SAN (Storage Area Network):** 서버와 저장 장치 간 블록 레벨 데이터 전송을 위한 전용 고속망(FC, iSCSI)임.

---

## 4. 계층 참조 모델 및 PDU (Reference Models & PDU)

계층화된 모델은 통신 프로세스를 모듈화하여 독립 처리를 가능하게 함. 각 계층의 데이터 단위를 **PDU (Protocol Data Unit)**라고 함.

| OSI 7 Layer (Full Name) | TCP/IP 4 Layer | PDU (Data Unit) | 식별 주소 및 프로토콜 |
| :--- | :--- | :--- | :--- |
| **7. Application Layer** | **4. Application** | **Data / Payload** | Port (HTTP, DNS, FTP) |
| **6. Presentation Layer** | (Application 통합) | **Data** | Encryption, Encoding |
| **5. Session Layer** | (Application 통합) | **Data** | Session Control |
| **4. Transport Layer** | **3. Transport** | **Segment** | **Port Number** (TCP/UDP) |
| **3. Network Layer** | **2. Internet** | **Packet** | **IP Address** (ICMP, ARP) |
| **2. Data Link Layer** | **1. Network Access** | **Frame** | **MAC Address** (Ethernet) |
| **1. Physical Layer** | **1. Network Access** | **Bits** | 전압, 광신호 (L1 Signal) |



---

## 5. 데이터 처리 프로세스 (Encapsulation & De-encapsulation)

### 5.1 Encapsulation (캡슐화) 프로세스
상위 계층 데이터가 하위 계층으로 전달되며 제어 정보인 **Header**가 추가되는 과정임.
1. **L4 (Transport):** Data에 **L4 Header(Port)**를 추가하여 **Segment** 생성함.
2. **L3 (Network):** Segment에 **L3 Header(IP Address)**를 추가하여 **Packet** 생성함.
3. **L2 (Data Link):** Packet에 **L2 Header(MAC)**와 오류 검출용 **L2 Trailer(FCS)**를 추가하여 **Frame** 생성함.
4. **L1 (Physical):** Frame을 물리적 매체용 **Bits**로 변환하여 송출함.



---

## 6. Layer 2 Switch: 물리 주소 기반 통신

### 6.1 L2 Switch 주요 기능 (5F Process)
**MAC (Media Access Control) Address Table**을 참조하여 **Collision Domain(충돌 도메인)**을 격리함.
1. **Learning:** 수신 프레임의 Source MAC을 식별하여 포트와 매핑 기록함.
2. **Flooding:** Destination MAC 정보가 없거나 Broadcast일 경우 수신 포트 제외 전 포트 송출함.
3. **Forwarding:** 목적지 MAC 포트가 확인된 경우 해당 포트로만 1:1 전송함.
4. **Filtering:** 목적지 포트가 수신 포트와 동일할 경우 타 포트 전송 차단함.
5. **Aging:** 타이머를 통해 일정 시간 통신 없는 엔트리를 삭제하여 메모리 최적화함.

### 6.2 물리 특성 및 장단점
- **Full-Duplex (전이중 통신):** TX/RX 선로를 물리적으로 분리하여 충돌 방지 및 동시 송수신 지원함.
- **장점:** ASIC 기반 하드웨어 스위칭으로 저지연 고속 전송 및 **VLAN**을 통한 브로드캐스트 도메인 격리 지원함.
- **단점:** 루프(Loop) 발생 시 데이터 무한 복제되는 **Broadcast Storm**에 취약함 (STP 적용 필수임).



---

## 7. Layer 3 Router: 논리 주소 기반 통신

### 7.1 주요 기능 및 서비스
- **Routing:** 네트워크 간 **Best Path (최적 경로)**를 결정하고 패킷을 중계함.
- **NAT (Network Address Translation):** Private IP를 Public IP로 변환하여 주소 효율화 및 내부 시스템 은닉함.
- **DHCP (Dynamic Host Configuration Protocol):** IP, Subnet Mask, Gateway 정보를 자동 할당함.

### 7.2 최적 경로 결정 로직
- **Metric:** 경로 선정 수치 (Hop Count, Bandwidth, Delay 등).
- **LPM (Longest Prefix Match):** 라우팅 테이블에서 목적지 IP와 서브넷 마스크가 가장 길게 일치하는 경로를 우선 선택함.

---

## 8. 주소 해석 및 게이트웨이 프로세스 (Addressing Logic)

### 8.1 Subnet Mask & Default Gateway
- **Subnet Mask:** IP의 Network ID와 Host ID를 구분하는 비트 연산자임. 내부/외부망 판별 기준임.
- **Default Gateway:** 외부 네트워크 통신을 위해 패킷을 전달하는 라우터 인터페이스 IP 주소임.

### 8.2 ARP (Address Resolution Protocol) 프로세스
1. **ARP Request (Broadcast):** 목적지 IP에 대응하는 MAC 주소 식별을 위해 전체 질의함.
2. **ARP Reply (Unicast):** 해당 IP 보유 단말이 자신의 MAC 주소를 응답함.
3. **ARP Cache:** 학습된 결과를 메모리에 유지하여 효율성 제고함.



---

## 9. 네트워크 아키텍처의 진화 (Architecture Evolution)

### 9.1 Traditional 3-Tier Architecture
- **Structure:** Core - Aggregation - Access 계층 구조임.
- **Constraint:** L2 기반 설계 시 STP 사용으로 가용 대역폭 제한 발생함.

### 9.2 Modern Spine-Leaf Architecture (Data Center)
- **Structure:** Spine Node (Backbone) - Leaf Node (Gateway) 2계층 구조임.
- **Mechanism:** **L3 Fabric** 구성으로 STP를 제거하고 **ECMP**로 다중 경로 동시 활용함.
- **Effect:** 동-서(East-West) 트래픽 지연 최소화 및 선형적 확장성 보장함. 고성능 ASIC 칩의 연산력 활용함.



---

## 10. 실무 트러블슈팅 및 체크포인트 (Troubleshooting)

| 계층 | 주요 점검 항목 (State Table) | 실무 점검 팁 및 명령어 |
| :--- | :--- | :--- |
| **L1/L2** | Link LED, **MAC Address Table** | `show mac address-table`로 물리 연결 및 MAC 학습 상태 우선 확인해야 함. |
| **L3** | **IP Address**, **ARP Table**, **Routing Table** | **팁:** `arp -a` 결과에 Gateway MAC 없으면 L2/L3 단절 상태임. |
| **L4** | **TCP Session State** | `netstat -an`으로 Port Listening 및 Established 여부 확인해야 함. |
| **L7** | **DNS Resolve**, **Service Log** | `nslookup` 통한 도메인 해석 및 응답성 점검해야 함. |
