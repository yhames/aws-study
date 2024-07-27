# Scalability and High Availability

- [Scalability and High Availability](#scalability-and-high-availability)
  - [Scalability](#scalability)
    - [Vertical Scaling](#vertical-scaling)
    - [Horizontal Scaling](#horizontal-scaling)
  - [High Availability](#high-availability)
  - [정리](#정리)
- [ELB](#elb)
  - [Load Balancer](#load-balancer)
  - [Why ELB?](#why-elb)
  - [Health Check](#health-check)
  - [Types of ELB](#types-of-elb)
  - [Security Groups for ELB](#security-groups-for-elb)

확장성(Scalability)과 고가용성(High Availability)은 모두 시스템의 성능을 향상시키는 방법이지만, 서로 다른 목표를 가지고 있습니다.

## Scalability

확장성은 시스템이 더 많은 요청을 처리할 수 있는 능력을 의미합니다.
이는 더 많은 사용자가 서비스를 사용할 수 있도록 하는 것을 의미합니다.
확장성은 두 가지 방법으로 구현할 수 있습니다.

* 수직 확장(Vertical Scaling): 단일 서버의 성능 향상
* 수평 확장(Horizontal Scaling): 여러 서버를 추가하여 시스템의 성능 향상
   * 탄력성(Elasticity)이라고도 불립니다.


### Vertical Scaling

수직 확장은 단일 서버의 성능을 향상시키는 것입니다.

예를 들어, t2.micro에서 t2.large로 업그레이드하는 것을 의미합니다.

수직 확장은 RDS나 ElastiCache와 같이 분산되지 않은 서비스에서 주로 사용됩니다.

그러나 하드웨어의 제한이 있기 때문에 수직 확장은 한계가 있습니다.

### Horizontal Scaling

수평 확장은 여러 서버를 추가하여 시스템의 성능을 향상시키는 것입니다.

수평 확장은 분산 시스템으로 구성된 웹이나 애플리케이션 서버에서 주로 사용됩니다.

EC2를 사용하면 인스턴스를 추가하거나 Auto Scaling을 사용하여 수평 확장을 할 수 있습니다.

## High Availability

고가용성은 시스템이 장애가 발생해도 계속해서 서비스를 제공할 수 있는 능력입니다.

이는 적어도 2개 이상의 서버가 AWS의 AZ나 데이터 센터에 가동 중인 것을 의미합니다.

고가용성의 목표는 하나의 서버가 다운되어도 다른 서버가 서비스를 계속 제공하는 것입니다.

고가용성은 수동적인 방법과 능동적인 방법으로 구현할 수 있습니다.

* 수동적인 방법 e.g. RDS Multi-AZ
* 능동적인 방법 e.g. horizontal scaling

 
## 정리

* Vertical Scaling: 단일 서버의 성능 향상
  * e.g. t2.micro -> t2.large
* Horizontal Scaling: 여러 서버를 추가하여 시스템의 성능 향상
  * e.g. Auto Scaling, Load Balancer
* High Availability: 시스템이 장애가 발생해도 계속해서 서비스를 제공할 수 있는 능력
  * e.g. Auto Scaling Group with multiple AZ, Load Balancer with multiple AZ

# ELB

## Load Balancer

로드 밸런서는 트래픽을 여러 서버로 분산시켜 서버의 부하를 분산시키는 역할을 합니다.

![load_balancer.png](images%2Fload_balancer.png)

* 로드 밸런서를 사용하면 트래픽 부하를 다수의 인스턴스로 분산시킬 수 있고,
* 다수의 서버가 있어도 단일 엔드포인트로 서비스를 제공할 수 있습니다.
* 또한 서버 장애가 발생해도 다른 서버로 트래픽을 전달하여 서비스를 계속 제공할 수 있고
* 규칙적으로 서버의 상태를 확인하여 장애 서버를 제외시킬 수 있습니다.
* SSL 암호화를 지원하여 보안을 강화할 수 있습니다.
* 쿠키를 사용하여 sticky session을 구현할 수 있습니다.
* 여러 AZ를 사용하여 고가용성을 확보할 수 있습니다.
* 클라우드 내 공개 트래픽과 비공개 트래픽을 분리할 수 있습니다.

## Why ELB?

ELB는 관리형 서비스로서 사용자가 로드 밸런서를 관리할 필요가 없습니다.
오직 로드 밸런서를 생성하고 설정만 하면 됩니다.

ELB는 AWS가 업그레이드와 유지보수 및 고가용성을 관리하므로 사용자는 서비스에만 집중할 수 있습니다.
또한 Auto Scaling과 통합되어 Auto Scaling 그룹의 인스턴스를 자동으로 관리할 수 있습니다.
그 외에도 EC2, ECS, Lambda, S3, CloudFront 등 AWS 서비스와 통합되어 사용할 수 있습니다.

## Health Check

헬스 체크는 로드 밸런서가 서버가 정상적으로 동작하는지 확인하는 방법입니다.

만약 서버가 정상적으로 동작하지 않는다면 로드 밸런서는 해당 서버로 트래픽을 전달하지 않습니다.

![health_check.png](images%2Fhealth_check.png)

헬스체크는 포트와 라우트에서 이뤄집니다. (대부분 `/health`)

## Types of ELB

ELB에는 4가지 타입이 있습니다.

* Classic Load Balancer(CLB): HTTP, HTTPS, TCP 프로토콜과 SSL 암호화를 지원합니다.
* Application Load Balancer(ALB): HTTP, HTTPS, WebSocket 프로토콜을 지원합니다.
* Network Load Balancer(NLB): TCP, TLS, UDP 프로토콜을 지원합니다.
* Gateway Load Balancer(GWLB): IP 프로토콜과 3계층 라우팅을 지원합니다.

## Security Groups for ELB

![elb_security.png](images%2Felb_security.png)

사용자는 HTTP나 HTTPS를 사용해서 로드 밸런서에 접근합니다.

로드 밸런서의 보안그룹에 인바운드 규칙을 추가하여 사용자의 요청을 받습니다.

| Type | Protocol | Port Range |  Source  | Description |
|------|----------|------------|----------|-------------|
| HTTP |    TCP   |     80     | 0.0.0./0 | Allowing HTTP traffic from anywhere |
| HTTPS |   TCP   |    443     | 0.0.0./0 | Allowing HTTPS traffic from anywhere |

어플리케이션에서는 로드 밸런서의 보안그룹을 소스로 설정하여 로드 밸런서의 요청만 받도록 설정합니다.

| Type | Protocol | Port Range |  Source  | Description |
|------|----------|------------|----------|-------------|
| HTTP |    TCP   |     80     | sg-xxxxxx | Allowing HTTP traffic from ELB |