---
title: "[OCI] OCI와 Azure 간 VPN 통신"

---

### [OCI] OCI와 Azure 간 VPN 통신

OCI와 Azure 간 IPsec VPN 연결을 위한 방법 설명입니다.(static 연결)

#### 1. 개요

* OCI와 Azure 가 사용하는 private IP 대역이 달라야 합니다(예: OCI(10.5.0.0/16), Azure(10.0.0.0/16))
* IPsec 연결을 위해 Azure VPN(virtual network gateway) 장비 생성 후 OCI에서 VPN 연결 생성, 그 다음 Azure에서 OCI의 VPN tunne에 대응하는 local network gateway 생성 후 route table로 연결하는 순서로 작업 하시면 됩니다.  그 밖에 방화벽 설정도 필요합니다.

#### 2. 작업 순서

* OCI와 Azure에서 가상 네트워크 생성

* Azure에서 marketplace의 virtual network gateway 생성(CPE 장비(VPN))
  * 경로 기반 설정 static으로
  * virtual network gateway가 사용할 subnet 주소 설정(Azure의 가상 네트워크의 다른 subnet으로 지정)
  * public IP 생성
![]({{ 'assets/images/azure_virtual_network_gateway.jpg' | relative_url }})
  
* OCI에서 IPsec 설정 
  * Azure의 virtual network gateway의 public IP 정보로 OCI CPE 의 public IP 설정
  
  * Static route 주소에 Azure의 private network CIDR 정보 입력
![]({{ 'assets/images/OCI_VPN_STATIC_ROUTE.jpg' | relative_url }})
  
  * default 2개의 tunnel 생성(각 tunnel의 public IP와 Shared secret 생성됨)
![]({{ 'assets/images/oci_ipsec_01.jpg' | relative_url }})

* Azure에서 marketplace의 local network gateway 생성

  * OCI IPsec에서 만들어진 2개의 tunnel에 대응되는 설정

  * 생성시 설정사항

    * 연결형식: 사이트 간(IPsec)

    * IP 주소: OCI IPsec tunnel에 배정된 public IP

    * 주소 공간: 해당 local network gateway가 라우팅할 OCI의 가상 네트워크 IP 대역(CIDR)
    
    * IKE 버전을 OCI tunnel 설정과 맞춰줘야 한다 1 아니면 2
![]({{ 'assets/images/local_network_gateway.jpg' | relative_url }})

* Azure에서 Route table 생성

  * 경로 설정

    * OCI의 가상 네트워크 주소가 가상 네트워크 게이트웨이로 보내지도록 설정

  * 서브넷 설정

    * Azure의 어떤 subnet이 이 Route table 을 사용할 지 설정
  ![]({{ 'assets/images/azure_route_table.jpg' | relative_url }})

* OCI security List

  * Azure에서 사용할 서비스 포트 오픈
  * ping test시 ingress의 ICMP 포트 오픈 확인

* Azure 네트워크 보안 그룹

  * OCI에서 사용할 서비스 포트 오픈

![]({{ 'assets/images/vpn_connection_test.png' | relative_url }})

