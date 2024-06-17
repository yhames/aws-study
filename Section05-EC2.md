# EC2

- [EC2](#ec2)
  - [Budget](#budget)
  - [EC2](#ec2-1)
    - [Sizing and configuration](#sizing-and-configuration)
    - [EC2 User Data](#ec2-user-data)
    - [EC2 Instance Types](#ec2-instance-types)
  - [EC2 Instance Type](#ec2-instance-type)
    - [General Purpose](#general-purpose)
    - [Compute Optimized](#compute-optimized)
    - [Memory Optimized](#memory-optimized)
    - [Storage Optimized](#storage-optimized)

## Budget

root 유저의 account 설정에서 Bill 탭을 클릭하면 현재 사용량을 확인할 수 있습니다.

또한 Budget을 설정하면 사용량이 일정 금액을 넘을 떄 메일로 알림을 받을 수 있습니다.

## EC2

EC2 stands for Elastic Compute Cloud,
which is a service that provides resizable compute capacity in the cloud.

it has capability of

- Renting virtual machines (EC2) : 가상 머신을 빌릴 수 있습니다.
- Storing data on virtual drives (EBS) : 가상 드라이브(EBS 볼륨)에 데이터를 저장할 수 있습니다.
- Distributing load across machines (ELB) : 여러 머신에 부하를 분산할 수 있습니다.
- Scaling the services using an auto-scaling group (ASG) : ASG를 사용하여 서비스를 확장할 수 있습니다.

### Sizing and configuration

- OS : Windows, Linux, MacOS
- CPU
- RAM
- Storage
    - Network attached (EBS, EFS)
    - Hardware (Instance Store)
- Network card
- Firewall rules (security group)
- Bootstrap script (user data)

### EC2 User Data

bootstrap script를 사용하여 EC2 인스턴스를 시작할 때 실행할 명령을 지정할 수 있습니다.
해당 스크립트는 인스턴스가 시작될 때 한 번 실행됩니다.

EC2 user data is used to automate boot tasks such as:

- Installing updates
- Installing software
- Downloading common files from the internet
- etc.

The EC2 User Data Script runs with the root user. (sudo)

### EC2 Instance Types

There are many different instance types, each optimized for different use cases.

| Instance    | vCPU | Mem   | Storage          | Network         | EBS Bandwidth (Mbps) |
|-------------|------|-------|------------------|-----------------|----------------------|
| t2.micro    | 1    | 1GB   | EBS Only         | Low to Moderate |                      |
| t2.xlarge   | 4    | 16GB  | EBS Only         | Moderate        |                      |
| c5d.4xlarge | 16   | 32GB  | 1 x 400 NVMe SSD | Up to 10 Gbps   | 4750                 |
| r5.16xlarge | 64   | 512GB | EBS Only         | 20 Gbps         | 13600                |
| m5.8xlarge  | 32   | 128GB | EBS Only         | 10 Gbps         | 6800                 |

> t2.micro is part of the AWS free tier (up to 750 hours per month)

## EC2 Instance Type

> https://aws.amazon.com/ko/ec2/instance-types/  
> https://instances.vantage.sh/

EC2 Instance has a following naming convention

```
m5.2xlarge
```

- m : instance class
  - more info : https://aws.amazon.com/ko/ec2/instance-types/
- 5 : generation
- 2xlarge : size within the instance class
  - more CPU and RAM

### General Purpose

Instance class for `t`, `m` is a general purpose instance. 

it is used for diversity of workloads such as `web servers`, `code repositories`

it has balance of `CPU`, `Memory`, `Networking`

> `t2.micro` is part of the AWS free tier (up to 750 hours per month)

### Compute Optimized

Instance class for `c` is a compute optimized instance.

it is used for `compute-bound` applications that benefit from high-performance processors.

> e.g. `batch processing`, `media transcoding`, `high-performance web servers`,
`high-performance computing (HPC)`, `scientific modeling`, `dedicated gaming servers`

### Memory Optimized

Instance class for `r`, `u`, `x`, `z` is a memory optimized instance.

it is used for `memory-bound` applications that benefit from large data sets in memory.

> e.g. `High performance databases`, `distributed web scale in-memory caches`,
`In-memory databases optimized for BI(Business Intelligence)`,
`applications performing real-time processing of unstructured big data`

### Storage Optimized

Instance class for `i`, `d`, `h` is a storage optimized instance.

it is used for `storage-bound` applications that require 
high, sequential read and write access
to very large data sets on local storage.

> e.g. `High frequency online transaction processing (OLTP) systems`,
> `Relational & NoSQL databases`, `Data warehousing applications`,
> `Cache for in-memory databases(Redis)`, `Distributed file systems`



