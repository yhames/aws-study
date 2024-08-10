# Route 53

- [Route 53](#route-53)
  - [Route 53](#route-53-1)
    - [Records](#records)
    - [Record Types](#record-types)
  - [Hosted Zone](#hosted-zone)
  - [TTL](#ttl)

DNS는 Domain Name System의 약자로, 인터넷에서 호스트의 도메인 이름을 IP 주소로 변환하거나 그 반대의 역할을 하는 시스템입니다.

![dns_domain.png](images/dns_domain.png)

DNS는 계층적으로 구성되어 있으며, 최상위 계층부터 차례대로 `루트 도메인`, `상위 도메인`, `하위 도메인`으로 구성되어 있습니다.


## Route 53

Route 53은 AWS에서 제공하는 DNS 서비스입니다.
 
Route 53는 사용잦가 DNS `Record`를 제어하는 것이 가능합니다.

Route 53는 또한 Domain Registrar로서 도메인을 등록하고 관리할 수 있습니다.

Route 53를 통해 리소스에 대한 헬스 체크를 할 수 있습니다.

Route 53의 이름에 있는 53의 의미는 전통적으로 DNS 포트인 53번에서 유래되었습니다.

### Records

레코드란 DNS 서버에 저장된 정보를 의미합니다.

레코드에는 다음과 같은 정보가 포함됩니다.
- 도메인/서브도메인 이름
- 레코드 타입 (e.g. `A`, `AAAA`, `CNAME`, `NS`)
- 도메인 주소 (e.g. 142.58,14,51)
- 라우팅 정책
- TTL (Time to Live) : 레코드가 캐시에 저장되는 시간

### Record Types

- `A` : 도메인 이름을 IPv4 주소로 매핑
- `AAAA` : 도메인 이름을 IPv6 주소로 매핑
- `CNAME` : 도메인 이름을 다른 도메인 이름으로 매핑
  - DNS 네임스페이스의 Top-level domain에 대해서는 CNAME 레코드를 사용할 수 없습니다. (e.g. `example.com`)
- `NS` : 호스팅 영역(`Hosted Zone`)에 대한 네임 서버로 매핑
  - 라우팅 트래픽을 관리하기 위해 사용됩니다.


## Hosted Zone

호스팅 영역(`Hosted Zone`)은 AWS에서 호스팅하는 DNS 레코드 컨테이너입니다.

호스팅 영역은 도메인과 서브도메인으로 가는 트래픽의 라우팅을 관리합니다.

호스팅 영역에는 `Public Hosted Zone`과 `Private Hosted Zone`이 있습니다.

![route53_hosted_zone.png](images/route53_hosted_zone.png)

`Public Hosted Zone`은 인터넷에서 도메인 이름을 IP 주소로 변환하는데 사용되고, `Private Hosted Zone`은 VPC 내에서 도메인 이름을 IP 주소로 변환하는데 사용됩니다.

## TTL

![route53_ttl.png](images/route53_ttl.png)

TTL(Time to Live)은 DNS 레코드가 캐시에 저장되는 시간을 의미합니다.

TTL을 높게 설정하면 캐시에 저장되는 시간이 길어지므로 Route 53에 요청하는 횟수가 줄어들어 비용을 절약할 수 있습니다. 하지만 DNS 레코드를 업데이트하면 캐시에 저장된 레코드가 업데이트되기 전까지 이전 레코드를 사용하게 됩니다.

TTL을 낮게 설정하면 캐시에 저장되는 시간이 짧아지므로 Route 53에 요청하는 횟수가 늘어나게 됩니다. 하지만 DNS 레코드를 업데이트하면 캐시에 저장된 레코드가 빠르게 업데이트되어 새로운 레코드를 사용할 수 있습니다.

TTL은 Alias 레코드에는 적용되지 않습니다. Alias 레코드는 Route 53에서 자동으로 할당합니다.
