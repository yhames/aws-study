# SAA (Solutions Architect Associate)

- [SAA (Solutions Architect Associate)](#saa-solutions-architect-associate)
  - [Private \& Public IP](#private--public-ip)
  - [Elastic IP](#elastic-ip)

이 강의에서는 `IPv6`는 다루지 않지만 AWS에서는 `IPv6`를 지원합니다.

> IPv6는 128비트 주소 체계로, IPv4의 주소 고갈 문제를 해결하기 위해 만들어졌습니다.
> 주로 IoT(Internet of Things)와 모바일 기기에서 사용됩니다.

## Private & Public IP


Public ip는 `www`에서 인식할 수 있는 공개된 ip 주소이며,
Private ip는 사설 네트워크에서 사용하는 ip 주소입니다.

- Public IP
  - `www`에서 인식 가능한 공개된 IP 주소
  - 인터넷에 연결된 모든 장치는 고유한 공인 IP 주소를 가져야 합니다.
  - Can be geo-located easily
- Private IP
  - 오직 사설 네트워크 내에서만 인식할 수 있는 IP 주소
  - 사설 IP는 오직 사설 네트워크 내부에서만 고유합니다.
  - 즉, 다른 사설 네트워크에서는 동일한 IP 주소를 사용할 수 있습니다.
  - 인터넷 게이트웨이를 통해 인터넷에 연결할 수 있습니다.
  - 지정된 범위의 주소만 사설 IP로 사용할 수 있습니다.

## Elastic IP

EC2에서 인스턴스를 중지하고 다시 시작하면 public IP 주소가 변경됩니다.
따라서 고정 IP가 필요한 경우에는 Elastic IP를 사용합니다.

`Elastic IP`는 EC2 인스턴스에 할당할 수 있는 고정 IP 주소입니다. 
Elastic IP를 사용하면 하나의 인스턴스가 실패해서 종료되어도 다른 인스턴스로 쉽게 교체할 수 있습니다.
(하지만 Elastic IP는 계정 당 5개로 제한됩니다)

또한 Elastic IP를 사용하는 것은 구조적으로 권장되지 않습니다.   
대신 `DNS` 서비스를 사용하여 도메인 이름을 사용하는 것이 좋습니다. (`Route 53`)  
혹은 `로드 밸런서`를 사용하여 public ip를 사용하지 않는 방법도 있습니다. (`ELB`)






