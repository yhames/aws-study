# ELB

- [ELB](#elb)
  - [Scalability and High Availability](#scalability-and-high-availability)
    - [Scalability](#scalability)
      - [Vertical Scaling](#vertical-scaling)
      - [Horizontal Scaling](#horizontal-scaling)
    - [High Availability](#high-availability)
    - [정리](#정리)
  - [ELB](#elb-1)
    - [Load Balancer](#load-balancer)
    - [Why ELB?](#why-elb)
    - [Health Check](#health-check)
    - [Types of ELB](#types-of-elb)
    - [Security Groups for ELB](#security-groups-for-elb)
  - [ALB](#alb)
    - [Target Groups](#target-groups)
    - [Good to Know](#good-to-know)
  - [NLB](#nlb)
    - [Target Groups](#target-groups-1)
  - [GWLB](#gwlb)
    - [Target Groups](#target-groups-2)
  - [Sticky Sessions](#sticky-sessions)
    - [Cookie](#cookie)
  - [Cross-Zone Load Balancing](#cross-zone-load-balancing)
  - [SSL](#ssl)
    - [SNI(Server Name Indication)](#sniserver-name-indication)
  - [Deregistration Delay (Connection Draining)](#deregistration-delay-connection-draining)
- [ASG](#asg)
    - [Launch Template](#launch-template)
    - [ASG with CloudWatch](#asg-with-cloudwatch)
  - [Scaling Policies](#scaling-policies)
    - [Dynamic Scaling Policies](#dynamic-scaling-policies)
    - [Predictive Scaling](#predictive-scaling)
    - [Scheduled Actions](#scheduled-actions)
    - [Good Metrics](#good-metrics)
    - [Scaling Cooldown](#scaling-cooldown)

## Scalability and High Availability

확장성(Scalability)과 고가용성(High Availability)은 모두 시스템의 성능을 향상시키는 방법이지만, 서로 다른 목표를 가지고 있습니다.

### Scalability

확장성은 시스템이 더 많은 요청을 처리할 수 있는 능력을 의미합니다.
이는 더 많은 사용자가 서비스를 사용할 수 있도록 하는 것을 의미합니다.
확장성은 두 가지 방법으로 구현할 수 있습니다.

* 수직 확장(Vertical Scaling): 단일 서버의 성능 향상
* 수평 확장(Horizontal Scaling): 여러 서버를 추가하여 시스템의 성능 향상
   * 탄력성(Elasticity)이라고도 불립니다.


#### Vertical Scaling

수직 확장은 단일 서버의 성능을 향상시키는 것입니다.

예를 들어, t2.micro에서 t2.large로 업그레이드하는 것을 의미합니다.

수직 확장은 RDS나 ElastiCache와 같이 분산되지 않은 서비스에서 주로 사용됩니다.

그러나 하드웨어의 제한이 있기 때문에 수직 확장은 한계가 있습니다.

#### Horizontal Scaling

수평 확장은 여러 서버를 추가하여 시스템의 성능을 향상시키는 것입니다.

수평 확장은 분산 시스템으로 구성된 웹이나 애플리케이션 서버에서 주로 사용됩니다.

EC2를 사용하면 인스턴스를 추가하거나 Auto Scaling을 사용하여 수평 확장을 할 수 있습니다.

### High Availability

고가용성은 시스템이 장애가 발생해도 계속해서 서비스를 제공할 수 있는 능력입니다.

이는 적어도 2개 이상의 서버가 AWS의 AZ나 데이터 센터에 가동 중인 것을 의미합니다.

고가용성의 목표는 하나의 서버가 다운되어도 다른 서버가 서비스를 계속 제공하는 것입니다.

고가용성은 수동적인 방법과 능동적인 방법으로 구현할 수 있습니다.

* 수동적인 방법 e.g. RDS Multi-AZ
* 능동적인 방법 e.g. horizontal scaling

### 정리

* Vertical Scaling: 단일 서버의 성능 향상
  * e.g. t2.micro -> t2.large
* Horizontal Scaling: 여러 서버를 추가하여 시스템의 성능 향상
  * e.g. Auto Scaling, Load Balancer
* High Availability: 시스템이 장애가 발생해도 계속해서 서비스를 제공할 수 있는 능력
  * e.g. Auto Scaling Group with multiple AZ, Load Balancer with multiple AZ

## ELB

### Load Balancer

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

### Why ELB?

ELB는 관리형 서비스로서 사용자가 로드 밸런서를 관리할 필요가 없습니다.
오직 로드 밸런서를 생성하고 설정만 하면 됩니다.

ELB는 AWS가 업그레이드와 유지보수 및 고가용성을 관리하므로 사용자는 서비스에만 집중할 수 있습니다.
또한 Auto Scaling과 통합되어 Auto Scaling 그룹의 인스턴스를 자동으로 관리할 수 있습니다.
그 외에도 EC2, ECS, Lambda, S3, CloudFront 등 AWS 서비스와 통합되어 사용할 수 있습니다.

### Health Check

헬스 체크는 로드 밸런서가 서버가 정상적으로 동작하는지 확인하는 방법입니다.

만약 서버가 정상적으로 동작하지 않는다면 로드 밸런서는 해당 서버로 트래픽을 전달하지 않습니다.

![health_check.png](images%2Fhealth_check.png)

헬스체크는 포트와 라우트에서 이뤄집니다. (대부분 `/health`)

### Types of ELB

ELB에는 3가지(~~4가지~~) 타입이 있습니다.

* CLB는 더 이상 AWS에서 지원하지 않습니다. ~~Classic Load Balancer(CLB): HTTP, HTTPS, TCP 프로토콜과 SSL 암호화를 지원합니다.~~
* Application Load Balancer(ALB): HTTP, HTTPS, WebSocket 프로토콜을 지원합니다.
* Network Load Balancer(NLB): TCP, TLS, UDP 프로토콜을 지원합니다.
* Gateway Load Balancer(GWLB): IP 프로토콜과 3계층 라우팅을 지원합니다.

### Security Groups for ELB

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

## ALB

ALB는 Application Load Balancer의 약자로, 7 계층(Layer 7)에서 동작하는 로드 밸런서입니다.

![alb_example.png](images%2Falb_example.png)

ALB는 7계층 로드밸런서로서 HTTP, HTTPS, WebSocket 프로토콜을 지원합니다.

Load Balancing to multiple HTTP applications across machines (`target groups`).
여러 머신(대상 그룹)에서 여러 HTTP 애플리케이션에 대한 부하 분산을 수행합니다. 또한 하나의 인스턴스 내의 여러 어플리케이션에 대해서도 부하 분산을 수행할 수 있습니다.

HTTP/2.0, WebSocket을 지원합니다. redirects를 지원하기 때문에 HTTP를 HTTPS로 변환하는 데 사용할 수 있습니다.

대상 그룹(target groups)에 대한 라우팅 테이블을 기반으로 라우팅을 할 수 있습니다. 이때 URL의 경로, 호스트 이름, 쿼리 파리미터 혹은 HTTP 헤더를 기반으로 라우팅할 수 있습니다.

* URL: `/users`, `/posts` 등
* one.example.com, two.example.com
* 쿼리 파라미터: `example.com?user=123`

ALB는 Docker나 ECS 등 마이크로서비스나 컨테이너 기반 어플리케이션에 적합합니다.

포트 매핑 기능이 있기 때문에 EC2 인스턴스의 동적 포트로 리다이렉트할 수 있습니다.

### Target Groups

Target Group은 ALB로 라우팅되는 대상입니다.

대상 그룹으로 라우팅 되는 대상은 다음과 같습니다.
* EC2 인스턴스
* ECS tasks
* Lambda functions
* IP addresses

ALB는 여러 대상 그룹을 가질 수 있습니다.

헬스 체크는 대상 그룹마다 설정할 수 있습니다.

### Good to Know

로드 밸런서를 사용하면 고정 호스트 이름이 부여됩니다.

어플리케이션 서버는 클라이언트의 IP를 직접 보지 못하고, `X-Forwarded-For` 헤더를 통해 클라이언트의 IP를 확인할 수 있습니다. `X-Forwarded-Port`, `X-Forwarded-Proto` 헤더도 사용할 수 있습니다.

## NLB

NLB는 Network Load Balancer의 약자로, 4 계층(Layer 4)에서 동작하는 로드 밸런서입니다.

NLB는 4계층 로드밸런서로서 TCP, TLS, UDP 프로토콜을 지원합니다.

NLB는 초당 수백만건의 요청을 처리할 수 있고, 지연시간도 100ms 이하로 매우 짧습니다. (ALB는 400ms 이하)

NLB는 가용 영역 별로 고정 IP를 제공하며, Elastic IP를 사용할 수도 있습니다.

따라서 NLB는 고성능이나 TCP, UDP 트래픽을 처리해야 하는 경우에 사용됩니다.

AWS Free Tier에서는 NLB를 사용할 수 없습니다.

### Target Groups

NLB로 라우팅되는 대상은 다음과 같습니다.

![nlb_target_groups.png](images%2Fnlb_target_groups.png)

* EC2 인스턴스
* IP addresses
  * must be hard coded
  * must be private IP addresses
* ALB
  * for static IP addresses

NLB의 헬스체크는 TCP, HTTP, HTTPS 프로토콜을 지원합니다.

## GWLB

GWLB는 Gateway Load Balancer의 약자로, 3계층(Layer 3)에서 동작하는 로드 밸런서입니다.

Deploy, scale, and manage a fleet of third-party network virtual appliances in AWS.  
Example: firewall, intrusion detection, prevention systems, deep packet inspection, etc.

GWLB를 사용하면 AWS에서 제공하는 서드파티 네트워크 가상 애플라이언스(firewall, 침입 탐지 및 방지 시스템, 딥 패킷 검사 등)를 배포, 확장 및 관리할 수 있습니다.

![gwlb_example.png](images%2Fgwlb_example.png)

GWLB는 IP 프로토콜과 3계층 라우팅을 지원합니다.

GWLB는 다음과 같은 기능을 제공합니다.
* Transparent Network Gateway
  * VPC(Virtual Private Cloud)의 모든 트래픽을 단일 엔트리 포인트(GWLB)로 관리
* Load Balancer
  * 여러 대상 그룹으로 트래픽을 분산

> uses the GENEVE 프로토콜 with 6081 port

### Target Groups

![gwlb_target_groups.png](images%2Fgwlb_target_groups.png)

GWLB의 대상 그룹은 다음과 같습니다.

* EC2 인스턴스
* IP addresses
  * must be hard coded
  * must be private IP addresses

## Sticky Sessions

![sticky_session.png](images%2Fsticky_session.png)

Sticky Session 혹은 stickiness는 ALB(Application Load Balancer)에서 사용자가 한 번 로드 밸런서에 연결되면 동일한 서버로 계속 연결되는 것을 의미합니다.

Sticky Session을 위해서 로드 밸런서는 Cookie를 사용합니다. 이때 사용자는 동일한 서버로 연결되지만, 다른 사용자는 다른 서버로 연결될 수 있습니다.

Sticky Session은 사용자가 로그인한 상태를 유지하거나 쇼핑카트를 유지하는 데 사용됩니다.

Sticky Session는 인스턴스 부하의 불균형을 초래할 수 있습니다.

### Cookie

Sticky Session에는 Application-based 쿠키와 Duration-Based 쿠키가 있습니다.

* Application-based 쿠키
  * Custom 쿠키
    * 인스턴스(혹은 대상그룹의 대상)에서 생성한 쿠키를 사용합니다.
    * 서버에서 추가적인 속성을 포함시킬 수 있습니다.
    * 쿠키 이름은 각 대상 그룹 별로 개별적으로 지정해야하며
    * 쿠키 이름은 AWSALB, AWSALBAPP, AWSALBTG는 이미 예약되어 있으므로 사용할 수 없습니다.
  * Application 쿠키
    * 로드 밸런서에서 생성한 쿠키를 사용합니다.
    * 쿠키 이름은 AWSALBAPP입니다.
* Duration-Based 쿠키
  * 로드 밸런서에서 생성한 쿠키를 사용합니다.
  * 쿠키 이름은 AWSALB입니다.

## Cross-Zone Load Balancing

Cross-Zone Load Balancing을 사용하면 로드 밸런서가 여러 가용 영역에 걸쳐 있는 인스턴스로 트래픽을 분산시킬 수 있습니다.

![cross_zone.png](images%2Fcross_zone.png)

위 사진에서 좌측(cross-zone enabled)은 로드 밸런서가 모든 가용 영역에 있는 인스턴스로 트래픽을 분산시키는 것을 보여줍니다. 반면 우측(cross-zone disabled)은 로드 밸런서가 같은 가용 영역에 있는 인스턴스로만 트래픽을 분산시킵니다.

ALB에서 Cross-Zone Load Balancing은 기본적으로 활성화되어 있고, 대상 그룹 설정에서 비활성화할 수 있습니다. Cross-Zone 설정이 기본값이기 때문에 별도의 inter-AZ 비용은 지불하지 않아도 됩니다.

NLB와 GWLB에서는 Cross-Zone Load Balancing이 기본적으로 비활성화되어 있습니다. 따라서 Cross-Zone을 사용하면 별도의 inter-AZ 비용을 지불해야 합니다.

## SSL

![ssl_lb.png](images%2Fssl_lb.png)

ELB는 SSL을 사용하여 암호화된 트래픽을 처리할 수 있습니다.
ELB에서는 X.509 인증서를 사용하여 SSL을 구현합니다.
인증서는 ACM(AWS Certificate Manager)을 사용하거나, 직접 발급받은 업로드하여 사용할 수 있습니다.

HTTPS listener를 생성하면 기본 인증서를 설정해야합니다.
다중 도메인을 지원하기 위해 다른 인증서를 추가할 수 있습니다.
클라이언트는 SNI(Server Name Indication)를 사용하여 여러 도메인을 지원하는 서버에 연결할 수 있습니다.
또한 보안 정책을 설정하여 SSL/TLS 프로토콜 버전을 지정할 수 있습니다.

### SNI(Server Name Indication)

SNI는 하나의 서버에 존재하는 여러개의 서버들이 각기 다른 TLS 인증서를 사용할 수 있도록 하는 TLS 확장입니다.

![sni_example.png](images%2Fsni_example.png)

SNI는 최초 TLS 핸드셰이크에서 클라이언트가 요청하는 호스트 이름을 전송합니다. 서버는 이 호스트 이름을 기반으로 적절한 인증서를 선택하여 클라이언트에게 제공합니다.

SNI는 ALB, NLB, CloudFront에서만 지원됩니다.

## Deregistration Delay (Connection Draining)

Deregistration Delay 혹은 Connection Draining은 인스턴스가 종료되거나 비활성화되기 전에 로드 밸런서에서 트래픽을 제거하는 시간을 의미합니다.

![deregistration_delay](images%2Fderegistration_delay.png)

로드 밸런서는 인스턴스가 종료되거나 비활성화되기 전에 트래픽을 제거하여 서버에 대한 요청을 완료할 수 있도록 합니다. 이후에 새로운 요청은 다른 인스턴스로 전달됩니다.

Deregistration Delay는 1초부터 3600초(1시간)까지 설정할 수 있습니다. 0초로 설정하면 Deregistration Delay을 비활성화하고 로드 밸런서는 즉시 트래픽을 제거합니다.


# ASG

현실에서는 웹 서비스나 어플리케이션의 트래픽은 일정하지 않습니다.
따라서 서버의 수를 유동적으로 조절할 수 있어야 합니다.

AWS에서는 EC2 인스턴스를 생성하는 API를 호출하여 빠르게 서버를 생성하고 종료할 수 있습니다. ASG(Auto Scaling Group)는 이러한 작업을 자동화하여 서버의 수를 유동적으로 조절할 수 있습니다.

ASG는 트래픽에 맞춰서 인스턴스를 추가하거나 제거하여 서버의 수를 조절합니다. 또한 인스턴스의 최대 수와 최소 수를 설정하여 서버의 수를 제한할 수 있습니다.

![asg_lb.png](images%2Fasg_lb.png)

ASG는 로드 밸런서와 통합하면 ASG에 속한 모든 인스턴스가 로드 밸런서에 등록되어 트래픽을 분산시킬 수 있습니다.

또한 인스턴스가 비정상적으로 종료되면 ASG는 해당 인스턴스를 제거하고 새로운 인스턴스를 생성하여 서버의 수를 유지합니다.

ASG는 무료이며 사용한 인스턴스만 비용을 지불하면 됩니다.

### Launch Template

ASG는 인스턴스를 생성할 때 사용하는 템플릿인 Launch Template을 사용합니다.

![launch_template.png](images%2Flaunch_template.png)

Launch Template은 인스턴스의 AMI, 인스턴스 유형, 보안 그룹, 키 페어, 사용자 데이터 등을 설정할 수 있습니다.

Launch Template 생성할 때 Min, Max, Initial Size를 설정해야하고, Scaling Policy를 설정해야합니다.

### ASG with CloudWatch

ASG는 CloudWatch와 통합하여 트래픽을 모니터링하고, 트래픽에 따라 인스턴스를 추가하거나 제거할 수 있습니다.

![asg_cloudwatch.png](images%2Fasg_cloudwatch.png)

CloudWatch는 모니터링하여 CPU 사용량과 같은 메트릭(Metric)을 수집하고, 이를 기반으로 알람을 설정합니다. ASG와 CloudWatch를 통합하면 CPU 사용량이 증가하는 경우 발생하는 알람을 트리거로 인스턴스를 추가하거나 제거할 수 있습니다.

## Scaling Policies

### Dynamic Scaling Policies

> [Dynamic scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html)  
> [Scheduled scaling for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html)

* *Target Tracking Scaling

Target Tracking Scaling은 CloudWatch `metric를` 통해 특정 지표를 기준으로 인스턴스의 수를 조절하는 방법입니다.

* Simple/Step Scaling

Simple/Step Scaling은 CloudWatch `alarm`를 통해 특정 지표를 기준으로 인스턴스의 수를 조절하는 방법입니다.

### Predictive Scaling

Predictive Scaling은 CloudWatch의 `예측`을 통해 트래픽을 분석하고 예측하여 스케일링 작업을 예약하는 방법입니다.

### Scheduled Actions

Scheduled Actions은 사용자 패턴을 기반으로 확장을 예약하는 방법입니다.
예를들어 매주 금요일 10-5시에 이벤트를 진행하는 경우, 매주 금요일 10시에 인스턴스를 추가하고 5시에 인스턴스를 제거하는 예약을 정의할 수 있습니다.

### Good Metrics

> [Amazon EC2 Auto Scaling의 대상 추적 조정 정책 - 지표선택](https://docs.aws.amazon.com/ko_kr/autoscaling/ec2/userguide/as-scaling-target-tracking.html)

스케일링을 판단하기 위한 메트릭은 다음과 같습니다.

* `ASGAverageCPUUtilization` - Auto Scaling 그룹의 평균 CPU 사용률.
* `ALBRequestCountPerTarget` - 대상당 평균 Application Load Balancer 요청 수
* `ASGAverageNetworkIn` - 모든 네트워크 인터페이스에서 단일 인스턴스가 받은 평균 바이트 수
* `ASGAverageNetworkOut` - 모든 네트워크 인터페이스에서 단일 인스턴스가 보낸 평균 바이트 수

추가로 사용자 지정 지표를 사용하여 대상 추적 크기 조정 정책을 생성할 수 있습니다.

### Scaling Cooldown

Scaling Cooldown은 스케일링 작업을 수행한 후 다음 스케일링 작업을 수행하기 전에 휴지 시간을 갖는 것 입니다. (Default: 300 seconds)

ASG를 통해 추가되거나 삭제된 인스턴스가 안정화된 후 새로운 메트릭을 수집하기 위해서 Cooldown 시간 동안은 ASG가 추가 스케일링 작업을 수행할 수 없습니다.

Scaling Action이 발생하면 먼저 Cooldown이 발생했는지를 먼저 확인한 후 스케일링 작업을 수행하고, 만약 Cooldown이 발생했다면 해당 Scaling Action은 무시됩니다.

AMI를 사용하면 인스턴스를 구성하는데 필요한 시간이 단축되기 때문에 Cooldown 시간도 단축될 수 있습니다.
