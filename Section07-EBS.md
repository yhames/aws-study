# EBS (Elastic Block Store)

- [EBS (Elastic Block Store)](#ebs-elastic-block-store)
	- [What's an EBS Volume](#whats-an-ebs-volume)
	- [EBS Snapshot](#ebs-snapshot)
	- [AMI (Amazon Machine Image)](#ami-amazon-machine-image)
		- [AMI Process](#ami-process)

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






