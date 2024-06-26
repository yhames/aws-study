# SAA (Solutions Architect Associate)

- [SAA (Solutions Architect Associate)](#saa-solutions-architect-associate)
  - [Private \& Public IP](#private--public-ip)
  - [Elastic IP](#elastic-ip)
  - [Placement Group](#placement-group)
    - [Cluster](#cluster)
    - [Spread](#spread)
    - [Partition](#partition)

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

## Placement Group

`Placement Group`는 EC2 인스턴스가 AWS 인프라에 배치되는 방식을 제어하는 기능입니다.

`Placement Group`를 사용할 떄 사용할 수 있는 전략은 다음과 같습니다.

### Cluster

![partition_group_cluster.png](images%2Fplacement_group_cluster.png)

단일 AZ에 저지연 하드웨어 인스턴스를 클러스터링합니다.

높은 성능을 제공하지만, 위험 또한 높습니다.

* 장점:
    * 높은 네트워크 성능 (10 Gbps bandwidth between instances)
    * 클러스터 내의 인스턴스 간의 저지연 통신
* 단점:
    * 하드웨어(랙) 장애가 발생할 경우 모든 인스턴스가 영향을 받음
    * 다른 AZ로 확장이 어려움
* 사용 사례:
    * 빅데이터 분석
    * 높은 네트워크 성능과 저지연이 필요한 어플리케이션

### Spread

![partition_group_spread.png](images%2Fplacement_group_spread.png)

여러 AZ에 인스턴스를 분산합니다. (max 7 instances per AZ)

위험이 분산되기 때문에 고가용성 및 안정성이 높아집니다.

* 장점:
    * 하드웨어 장애가 발생할 경우 다른 AZ의 인스턴스가 영향을 받지 않음 (고가용성)
    * 다른 AZ로 확장이 쉬움
* 단점:
    * Limit of 7 instances per AZ
* 사용 사례:
    * 고가용성이 필요한 어플리케이션
    * 인스턴의 장애를 서로 격리해야하는 어플리케이션

### Partition

![partition_group_partition.png](images%2Fplacement_group_partition.png)

인스턴스를 여러 파티션으로 분할하여 배치합니다.

이 파티션은 가용 영역 내의 여러 하드웨어 렉에 분산됩니다.

* 장점:
    * 여러 AZ에 인스턴스를 분산하여 고가용성을 제공
    * 100개 이상의 인스턴스를 배치할 수 있음
    * 파티션의 인스턴스들은 다른 파티션과 물리적으로 격리되어 있음
      * `Partition01`에 장애가 생겨도 `Partition02`는 영향을 받지 않음
    * 파티션에 대한 메타 데이터를 사용하여 인스턴스가 같은 파티션에 있는지 확인할 수 있음
* 단점:
    * Limit of 7 partition per AZ
* 사용 사례:
  * HDFS(Hadoop Distributed File System) HBase, Cassandra, Kafka 등
  * 파티션을 인식하는 빅데이터 어플리케이션






