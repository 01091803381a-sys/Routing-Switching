# [NO.2] 네트워크 주소 체계 및 VLAN 구성 가이드

이 문서는 네트워크 주소의 기본 개념부터 VLAN 설계, 그리고 실무에서 사용되는 CLI 설정법까지의 핵심 내용을 담고 있습니다.

---

## 1. 네트워크 기초 주소 체계 및 토폴로지

### **1.1 주소 체계 (Addressing)**
| 구분 | 비트 수 | 특징 | 비고 |
| :--- | :--- | :--- | :--- |
| **MAC 주소** | 48비트 | 하드웨어 고유 식별 주소 | L2 계층 물리 주소 |
| **IPv4** | 32비트 | 10진수 4개 마디 ($2^{32}$) | 논리적 주소 |
| **IPv6** | 128비트 | 16진수 8개 마디 ($2^{128}$) | 차세대 주소 체계 |

### **1.2 LAN 토폴로지 (LAN Topology)**
* **Bus형:** 중앙 케이블에 모든 노드 연결.
* **Ring형:** 루프 형태로 인접 노드 간 연결.
* **Mesh형:** 모든 노드가 상호 연결 (높은 신뢰성).
* **Star형:** 중앙 스위치를 중심으로 연결 (가장 일반적인 형태).

### **1.3 라우터(Router)의 존재 이유**
1. **길 찾기 (Path Determination):** 서로 다른 네트워크 간 최적의 경로 설정.
2. **관리 및 보안:** 브로드캐스트 도메인 분리 및 트래픽 제어.

---

## 2. VLAN (Virtual LAN)의 개념 및 설계

### **2.1 VLAN 개요**
* **개념:** 물리적 경계를 넘어선 논리적인 네트워크 분리 기술.
* **필요성:** 트래픽(Broadcast) 감소, 보안 향상, 관리 효율성 증대.

### **2.2 VLAN ID 구조**
| ID 범위 | 분류 | 용도 |
| :--- | :--- | :--- |
| **1** | 기본(Default) | 모든 포트의 기본값, 관리용 |
| **2 - 1001** | 일반 | 사용자 정의 및 새로운 VLAN 할당 |
| **1002 - 1005** | 예약 | FDDI, 토큰링 등 고정용 |
| **1006 - 4094** | 확장 | 대규모 서비스 사업자용 |

---

## 3. VLAN 통신 기술 및 태깅 (Tagging)

### **3.1 포트 모드 비교**
* **Access Port:** 하나의 포트에 하나의 VLAN만 할당. 주로 종단 장치(PC) 연결.
* **Trunk Port:** 하나의 물리 링크로 여러 VLAN 트래픽을 통합 전달. 스위치 간 연결 시 필수.

### **3.2 IEEE 802.1Q (태깅 규격)**
트렁크 링크를 통과할 때 프레임에 삽입되는 4바이트 구조입니다.
* **TPID:** 태그 프로토콜 식별자.
* **PCP:** 우선순위(QoS) 설정.
* **VID:** VLAN 번호 식별.

> **Native VLAN:** 태그가 없는 트래픽을 수용하는 특별한 VLAN으로, 양단 스위치의 설정이 반드시 일치해야 합니다.

---

## 4. Inter-VLAN Routing

서로 다른 VLAN 간 통신을 위해서는 3계층 장비(라우터 또는 L3 스위치)의 길 안내가 필수적입니다.

### **Router-on-a-Stick 방식**
* 하나의 물리 포트를 가상화하여 여러 개의 **Sub-interface**로 분할.
* 각 서브 인터페이스에 개별 VLAN ID와 IP(Gateway)를 할당.

---

## 5. 실무 필수 CLI 명령어 세트 (Cisco IOS 기준)

### **5.1 VLAN 생성 및 포트 할당**
```bash
# VLAN 생성
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config-vlan)# exit

# Access 포트 할당
Switch(config)# interface f0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,99
Router(config)# interface g0/0
Router(config-if)# no shutdown

# Sub-interface 설정
Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

# 5.4 모니터링 및 검증
show vlan brief: VLAN 생성 및 포트 매핑 확인.

show interfaces trunk: 트렁크 상태 및 허용 VLAN 확인.

show ip interface brief: IP 할당 및 인터페이스 활성화 상태 확인.
