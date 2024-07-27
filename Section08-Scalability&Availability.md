# Scalability and High Availability

- [Scalability and High Availability](#scalability-and-high-availability)
  - [Scalability](#scalability)
    - [Vertical Scaling](#vertical-scaling)
    - [Horizontal Scaling](#horizontal-scaling)
  - [High Availability](#high-availability)
  - [정리](#정리)

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