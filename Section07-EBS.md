# EBS (Elastic Block Store)

- [EBS (Elastic Block Store)](#ebs-elastic-block-store)
	- [What's an EBS Volume](#whats-an-ebs-volume)
	- [EBS Snapshot](#ebs-snapshot)
	- [EBS Volume Types](#ebs-volume-types)
		- [General Purpose SSD (gp2/gp3)](#general-purpose-ssd-gp2gp3)
		- [Provisioned IOPS SSD (io1/io2)](#provisioned-iops-ssd-io1io2)
		- [HDD (st1/sc1)](#hdd-st1sc1)
	- [AMI (Amazon Machine Image)](#ami-amazon-machine-image)
		- [AMI Process](#ami-process)
	- [EC2 Instance Store](#ec2-instance-store)

## What's an EBS Volume

> network usb stick

EBS는 인스턴스가 실행중인 동안 연결 가능한 네트워크 드라이브 입니다.
it allows an instance to persist data even after termination.
물리적 드라이브가 아니라 네트워크를 사용하기 때문에 쉽게 인스턴스를 분리하고 다시 연결할 수 있습니다.

![ebs_example.png](images%2Febs_example.png)

EBS는 오직 하나의 인스턴스에만 연결될 수 있지만, (일부 EBS는 다중 연결이 가능합니다.)
하나의 인스턴스가 여러 EBS를 가질 수 있습니다.

EBS는 해당 가용 영역 내의 인스턴스에만 연결될 수 있습니다.
스냅샷 기능을 이용하면 다른 가용 영역으로 볼륨을 옮길 수 있습니다.

EBS를 사용하기 전에 사용할 용량(`GB`)과 초당 전송 수(`IOPS`)를 지정해야합니다.
해당 프로비전 용량에 따라 요금이 청구되며, 지정 이후에 용량을 늘리거나 줄일 수 있습니다.

`Delete on Termination` 옵션을 사용하여 인스턴스가 종료될 때 EBS가 삭제되도록 설정할 수 있습니다.

## EBS Snapshot

EBS 스냅샷은 EBS 볼륨의 특정 시점에 대한 백업입니다.

EBS 볼륨을 복원하거나 다른 가용 영역으로 이동할 수 있습니다.

EBS 스냅샷은 다음과 같은 기능을 가지고 있습니다.

* EBS 스냅샷 아카이브
  * 최대 75% 저렴한 아카이브 티어로 스냅샷을 옮길 수 있습니다.
  * 스냅샷을 복원하기 전에 아카이브 티어에서 다시 복원해야합니다.
  * 이 작업은 24 ~ 72 시간이 소요됩니다.
* EBS 스냅샷 휴지통
  * 스냅샷을 삭제하는 경우 휴지통에 이동하여 실수로 삭제한 EBS에 대해 복원할 수 있습니다.
  * 휴지통 보관 기간은 1일 ~ 1년 사이로 설정할 수 있습니다.
* FSR (Fast Snapshot Restore)
  * 스냅샷을 완전 초기화하여 첫 사용에서 지연시간을 줄이는 기능입니다.
  * 스냅샷 복구 중 가장 비싼 요금이 청구됩니다.

## EBS Volume Types

> [AWS - Amazon EBS 볼륨 유형](https://docs.aws.amazon.com/ko_kr/ebs/latest/userguide/ebs-volume-types.html)  

EBS 볼륨은 Size, Throughput, IOPS에 따라 종류가 나뉩니다. 

EBS 볼륨은 6가지의 유형이 있습니다.

* gp2/gp3: General Purpose SSD
  * 가장 일반적인 유형으로 대부분의 워크로드에 적합합니다.
  * 가격이 저렴하고 성능이 좋습니다.
* io1/io2: Provisioned IOPS SSD
  * IOPS를 미리 예약하여 높은 성능을 제공합니다.
  * 높은 IOPS가 필요한 워크로드에 적합합니다.
* st1: Throughput Optimized HDD
  * 저 비용의 HDD 볼륨
  * 잦은 접근과 처리량이 많은 워크로드에 적합합니다.
* sc1: Cold HDD
  * 가장 저렴한 HDD 볼륨
  * 자주 접근하지 않는 데이터에 적합합니다.

부팅 볼륨은 오직 gp2/gp3, io1/io2 유형만 사용할 수 있습니다.

### General Purpose SSD (gp2/gp3)

> [AWS - 범용 SSD 볼륨](https://docs.aws.amazon.com/ko_kr/ebs/latest/userguide/general-purpose.html)

General Purpose SSD는 짧은 지연 시간과 효율적인 비용의 스토리지입니다.

부팅 볼륨, 가상 데스크톱, 개발 및 테스트 환경 등 다양한 워크로드에 적합합니다.

크기는 1GB ~ 16TB까지 지원하며, IOPS와 처리량은 gp2와 gp3에 따라 다릅니다.

gp3는 gp2와 다르게 볼륨과 IOPS를 분리하여 볼륨의 크기와 상관없이 IOPS를 선택할 수 있습니다. 

### Provisioned IOPS SSD (io1/io2)

> [AWS - 프로비저닝된 IOPS SSD 볼륨](https://docs.aws.amazon.com/ko_kr/ebs/latest/userguide/provisioned-iops.html)

IOPS 성능을 유지해야하거나 16,000 IOPS 이상이 필요한 워크로드에 적합합니다.
즉, 스토리지 성능과 일관성에 민감한 어플리케이션은 io1/io2를 사용하는 것이 좋습니다.

크기는 4GB ~ 16TB까지 지원하며, IOPS와 처리량은 io1과 io2에 따라 다릅니다.
기본적으로 32,000 IOPS를 지원하며, Nitro EC2를 사용하면 최대 64,000 IOPS까지 선택할 수 있습니다.

io1/io2는 gp3와 같이 스토리지의 크기에 독립적으로 IOPS를 선택할 수 있습니다.
io2는 io1에 비해 더 적은 비용으로 동일한 IOPS와 내구성을 제공합니다.

io2 Block Express의 경우 4 ~ 64TB의 크기를 지원하며, 
밀리초 미만의 저지연 시간과 최대 256,000 IOPS를 제공합니다.(IOPS:GiB 비율 1,000:1 인 경우)

Provisioned IOPS SSD은 EBS 다중 연결을 지원합니다.

### HDD (st1/sc1)

st1/sc1는 부팅 볼륨으로 사용될 수 없습니다.

크기는 125MiB ~ 16TiB까지 지원합니다.

* Throughput Optimized HDD (st1)
  * 자주 접근하지만 큰 파일을 처리하는 워크로드에 적합합니다.
  * 데이터 웨어하우스, 빅데이터, 로그 처리, 등에 적합합니다.
  * 최대 500MiB/s의 처리량과 500 IOPS를 제공합니다.
* Cold HDD (sc1)
  * 자주 접근하지 않는 데이터를 저장하는 워크로드에 적합합니다.
  * 최대 250MiB/s의 처리량과 250 IOPS를 제공합니다.

## AMI (Amazon Machine Image)

AMI는 EC2 인스턴스를 시작하는 데 필요한 정보를 포함하는 템플릿입니다.

AMI를 사용하면 필요한 소프트웨어나 패키지를 미리 패키징하기 때문에
인스턴스를 실행할 때 시간을 단축할 수 있습니다.

AMI는 특정 지역에 구축해야하고 다른 지역으로 복사할 수 있습니다.

* public AMI: AWS에서 제공하는 AMI
* custom AMI: 사용자가 만든 AMI
  * 사용자가 직접 AMI를 관리해야합니다.
* marketplace AMI: AWS Marketplace에서 제공하는 AMI
  * 다른 사람이 구축한 AMI를 구매해서 사용합니다.

### AMI Process

![ami_process.png](images%2Fami_process.png)

* Start with an existing AMI
* Customize the instance
* Stop the instance (for data integrity)
* Build an AMI from the instance (this will also create an EBS snapshot)
* Launch new instances from that AMI

## EC2 Instance Store

만약 높은 디스크 성능이 필요하면 EC2 인스턴스 스토어를 사용할 수 있습니다.

EC2 인스턴스 스토어를 사용하면 해당 서버에 물리적으로 연결된 디스크를 갖습니다.
이때 연결된 디스크를 `Instance Store`라고 합니다.

Instance Store는 더 나은 I/O 성능을 제공하지만, 데이터는 인스턴스가 종료되면 사라집니다.
따라서 임시 데이터를 저장하는 용도로 사용합니다. (e.g. cache, buffer, scratch data)


