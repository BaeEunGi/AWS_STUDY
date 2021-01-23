# ELB 사용하기

**ELB** : 접근을 여러 대의 EC2 인스턴스로 분산하는 역할

**특징**

- 트래픽 분산
- 자동 확장
- 인스턴스의 상태를 자동 감지해서 오류가 있는 시스템은 배제
- 사용자의 세션을 특정 인스턴스에 고정

**접근 분산 기능을 사용하는 목적**

- 부하 분산 : 대량의 접근을 여러 개의 인스턴스로 처리
- 가용성 보장 : 1대의 인스턴스에 장애가 발생한 경우에도 다른 인스턴스로 서비스 제공 지속

## ELB 서비스 상세

부하 분산이나 가용성 보장 이 외에도 가지고 있는 다양한 특징과 기능들 살펴보기

### ELB 서비스의 가용성

- ELB는 **요청 처리 능력**을 항상 감시 -> 접속 부하에 맞게 자동적으로 리소스의 확장/축소 수행 -> 병목 현상 거의 X
- but 리소스 자동 확장까지 일정한 시간이 걸림 -> 일시적으로 병목 현상 발생 

- 확장 & 축소

  + 스케일업/스케일 다운 : EC2 인스턴스를 높은 성능/ 낮은 성능으로 교체(한 대의 컴퓨터)
  + 스케일아웃/스케일인 : 여러 대의 컴퓨터로 분산 or 컴퓨터 개수 줄이기

  + 모두 ELB 접근 IP가 변경되므로 ELB에 접근하는 경우 도메인을 사용해 접근하도록 구성해야 함

### 싱글 AZ 구성과 멀티 AZ 구성

ELB에 등록된 EC2 인스턴스가 단일 AZ에만 존재하는 경우와 여러 개의 AZ에 존재하는 경우 ELB 분산 방식이 다르다

**ELB에 등록된 EC2 인스턴스가 여러 개의 AZ에 존재하는 경우**

- ELB 생성 때에 생성된 도메인 이름에 각 AZ에 있는 ELB의 글로벌 IP 주소가 함께 등록된다
- ELB에 요청이 들어오면 DNS 라운드 로빈 방식으로 각각의 AZ의 ELB에 요청이 분산된다
- 각 AZ 내부의 EC2 인스턴스 수를 동일하게 두어야 한다

### Exernal-ELB와 Internal-ELB

**External-ELB**

- 인터넷에서 요청을 받음
- VPC의 퍼블릿 Subnet 내부에 생성해야 한다

**Internal-ELB**

- VPC Subnet 내부의 요청만을 받음
- 글로벌 IP 주소가 부여 X -> 퍼블릿 Subnet을 만들어도 인터넷으로 접근 불가

2개의 ELB를 조합하면 **3계층 시스템**처럼 애플리케이션의 여러 계츠응로 요청을 분산하는 아키텍처 구성 가능

### 헬스 체크

등록되어 있는 인스턴스들의 상태 감시

- 포트 감시
- 서비스 감시

### SSL 터미네이션

HTTPS 통신에서 사용하는 SSL 증명서를 인증하는 기능 -> SSL 증명서를 인스턴스들에 따로 설정할 필요가 없어짐

-> 인스턴스의 부하를 낮출 수 있음

### 스티키 세션

사용자의 요청을 같은 인스턴스에서 처리하게 만드는 기능

하지만 최근에는 ELB 아래에 있는 인스턴스들의 정보를 공유하는 시스템을 사용하는 것이 주류

### 로그 추출 기능

ELB로 첯리한 요청 로그 추출 가능 -> S3에 저장

## ELB 생성

인스턴스를 다른 AZ에 생성하고 ELB 전용 Security Group 미리 생성하기

### Subnet 추가

확장화 구성을 위한 EC2 인스턴스를 배치할 Subnet 추가하기

앞에서 생성한 Subnet과 다른 AZ를 지정하기

### 다른 AZ에 EC2 인스턴스 생성

확장화 구성을 하려면 ELB에 등록할 EC2 인스턴스 2개 이상 필요

위에서 생성한 Subnet으로 Subnet 지정하기

### ELB 전용 Security Group 생성

ELB와 EC2 인스턴스 모두 Security Group을 사용해서 통신 조건을 제한

### ELB 생성

:white_check_mark: EC2 -> Load Balancers -> Create Load Balancer

|            설정 항목             |                             설명                             |
| :------------------------------: | :----------------------------------------------------------: |
|        Load Balancer name        | ELB 이름 입력<br />이 이름이 ELB에 접근하기 위한 DNS의 일부로 구성 |
|         Create LB inside         |                  ELB를 생성할 대상 VPC 선택                  |
| Create an internal load balancer |              Internal-ELB를 생성한 경우는 체크               |
|      Listener Configuration      | ELB로 수신할 Protocol과 Port, ELB에서 인스턴스에 연결을 위한 Protocol과 Port 지정 |

#### Subnet 선택

ELB에 등록할 EC2 인스턴스가 존재하는 Subnet 선택

Available subnets -> 필요한 서브넷 선택 -> Select subnets -> Next Assigin Security Groups

#### Security Group 선택

ELB 통신 요건을 정할 수 있게 Security Group 선택

### ELB 인스턴스 설정

ELB에 등록할 EC2 인스턴스 생성

|               설정 항목               |      디폴트 값      |                             설명                             |
| :-----------------------------------: | :-----------------: | :----------------------------------------------------------: |
| Enable Cross-Zone<br />Load Balancing |       활성화        |      활성화화면 AZ 사이를 넘나들며 인스턴스 처리를 분산      |
|    Enable Connection<br />Draining    | 활성화<br />(300초) | ELB로 등록한 EC2 인스턴스 제거 후 제거한 인스턴스의 처리를 <br />바로 제거하지 않고 설정 시간만큼 유지 |

### AWS CLI로 생성

```
$ aws elb create-load-balancer
--load-balancer-name External-Web
--listeners Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80
--tag Key=Name,Value=External-Web
---subnets subnet-****
--security-groups sg-****
```







