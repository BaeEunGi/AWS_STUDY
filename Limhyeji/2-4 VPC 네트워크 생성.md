# VPC 네트워크 생성

## VPC란?

- VPC를 사용하여 할 수 있는 일들 
  + EC2 실행 
  + 서브넷 구성
  + 보안 설정(Ip block, 인터넷에 노출되지 않은 EC2 구성 등) 가능
- VPC Peering: VPC간에 연결
  + Transitive Peering 불가능 : 한 다리 건너 열결 되어 있다고 해서 Peering 된 것 아님
- VPC Flow log
  + VPC 로그를 CloudWatch에 저장 가능
- IP 대역 지정 가능
- Region에 하나 : 다른 Region으로 확장 불가능

**EC2 또는 RDS 등의 인스터스는 VPC 네트워크에서 동작하는 것이 표준이다**

<img src = "https://user-images.githubusercontent.com/59350891/104988726-276edc80-5a5c-11eb-8b22-5e87fb9432be.png" width = 70%>

## Default-VPC

AWS 계정을 생성했을 때 기본적으로 구성되는 VPC 네트워크

 ## Custom-VPC

#### 생성

1. VPC 네트워크 생성
2. Subnet 생성
3. Route Table 생성
4. Internet Gateway 생성
5. Network ACL 설정
6. Security Group 설정

### VPC 네트워크 생성

VPC에서 사용할 개인 네트워크 주소의 범위 설정 -> 네트워크 주소는 나중에 변경 불가 -> 여유를 두고 설정하기

**VPC 네트워크 설정 항목**

| 설정 항목  | 필수 |                             설명                             |
| :--------: | :--: | :----------------------------------------------------------: |
|  Name tag  |      |                  설정을 식볗하기 위한 이름                   |
| CIDR block |  O   |             최소 "/28"에서 최대 "/16"의 네트워크             |
|  Tenancy   |  O   | Default<br />Dedicated : EC2 인스턴스 가동하는 호스트 서버 지정 <br />-> 호스트 서버를 다른 시스템과 공유하는 것이 허가 됮 않는 경우 사용 |

- VPC를 새로 생성하면 Route Table, Network ACL, Securtiy Group이 자동 생성된다

- EC2 인스턴스에 접근하기 위해서 Public DNS를 사용할 수 있게 DNS Hostnames를 "yes"로 설정

#### AWS CLI로 VPC 생성

```
- "10.0.0.0/16"의 VPC 네트워크를 생성하는 경우
$aws ec2 create-vpc --cidr-block 10.0.0.0/16
$aws ec2 modify-vpc-attribute --vpc-id vpc-*/*/* --enable-dns-hostnames
```

### Subnet 생성

subnet이란 큰 네트워크를 여러 개의 작은 네트워크로 분할해서 관리할 때의 관리 단위가 되는 네트워크

:white_check_mark: VPC -> Subnet -> Create Subnet

**Subnet 설정 항목**

|     설정 항목     | 필수 |                설명                |
| :---------------: | :--: | :--------------------------------: |
|     Name tag      |      |     설정을 식별하기 위한 이름      |
|        VPC        |  O   |      Subnet을 생설할 VPC 지정      |
| Availability Zone |      |      Subnet을 생성할 AZ 지정       |
|    CIDR block     |  O   | Subnet에 할당할 네트워크 범위 지정 |

**CIDR Block 설정 시 제한 사항**

- ELB 생성 시점에서 ELB를 생성하는 Subnet에 20IP 주소 이상의 빈공간 필요 -> ELB를 생성하려는 Subnet은 "/24" 이상 추천

#### AWS CLI로 Subnet 생성

```
$ aws ec2 create-subnet --vpc-id vpc-*/*/* --availability-zone ap-northeast-1a --cidr-block 10.0.0.0/24
```

### Route Table 생성

- subnet 단위로 설정
- 인터넷을 통해 통신할 경우 -> Internet Gateway
- VPC 연결로 이외 부서와 통신할 경우 -> VIrtual Private Gateway

**Route Table 설정 항목**

| 설정 항목 | 필수 |                설명                |
| :-------: | :--: | :--------------------------------: |
| Name tag  |      |     설정을 식별하기 위한 이름      |
|    VPC    |  O   | Route Table을 생성할 VPC의 ID 지정 |

#### AWS CLI로 Route Table 생성

```
$ aws ec2 create-route-table --vpc-id vpc-*/*/*
```

### Subnet과 Route Table 연결

- 설정한 Subnet은 VPC 내부위 1개의 Route Table과 연결해야 한다

#### 연결

1. Route Table 목록에서 선택
2. Subnet Associations 탭 선택
3. Edit 클릭
4. 이전에 생성한 Subnet 체크
5. Save 클릭

#### AWS CLI로 연결

```
$ aws ec2 associate-route-table --route-table-id rtb-*/*/* --subnet-id subnet-*/*/*
```

### Internet Gateway 생성

- VPC 네트워크 내부에서 가동하는 EC2 인스턴스가 인터넷을 통해 외부와 통신할 떄 필요
- 생성한 Internet Gateway는 Route Table의 타깃으로 사용

:white_check_mark: VPC -> Internet Gateway -> Create Internet Gateway

#### Internet Gateway와 VPC 연결

- 생성한 Internet Gateway는 VPC와 연결해야 한다 -> State 값이 attached가 되도록 한다

#### AWS CLI로 Internet Gateway 생성

- Internet Gateway 생성
  `$ aws ec2 create-internet-gateway`

- VPC와 Internet Gateway 연결

  `$ aws ec2 attach-internet-gateway --internet-gateway-id igw-*/*/* --vpc-id vpc-*/*/*`

### Internet Gateway를 라우팅 대상으로 지정

1. Route Table 선택
2. Routes 탭 선택
3. Edit 클릭 후 Save 클릭

#### AWS CLI로 Route Table과 연결

```
$ aws ec2 create-route --route-table-id rtb-*/*/*
--destination-cidr-block 0.0.0.0/0
--gateway-id igw-*/*/*
```

