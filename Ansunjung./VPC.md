**AWS가 초기 설정으로 생성하는 Default-vpc, 사용자가 임의로 네트워크 주소를 지정하는 Custom-vpc를 생성하는 것이 가능하다.**

## Default-vpc
+ AWS를 생성했을 때 기본적으로 구성되는 VPC네트워크

## Custom-vpc
+ 사용자 정의 vpc
+ vpc 대시보드에는 vpc를 구성하는 vpc마법사를 제공함

<img src = "https://user-images.githubusercontent.com/55094745/104902324-f346dd80-59c1-11eb-9727-6444506d6b9d.png" width = "50%"></img>
+ 4가지의 구성요소

<img src = "https://user-images.githubusercontent.com/55094745/104902727-6c463500-59c2-11eb-8651-17f09637c535.png" width = "20%"></img>

> **vpc 생성 흐름**
> + VPC 네트워크를 생성한다.
> + subnet을 생성한다.
> + route table을 생성한다.
> + internet gateway를 생성한다.
> + network acl을 설정한다.
> + security group을 설정한다.

### vpc 네트워크 생성
+ vpc에서 사용할 네트워크 주소의 범위 설정(EC2의 인스턴스 수와 subnet수를 고려해 설정)
+ vpc -> yourVPCs -> Create VPC

<img src = "https://user-images.githubusercontent.com/55094745/104903655-a6fc9d00-59c3-11eb-9429-78e0214aa816.png" width = "40%"></img>
> 생성하면 route table, network acl, security group이 자동적으로 생성된다.

+ EC2 인스턴스에 접근할 때 public DNS를 사용할 수 있도록 설정

<img src ="https://user-images.githubusercontent.com/55094745/104904823-f7c0c580-59c4-11eb-89f2-79c7b8bada23.png" width = "30%"></img>

### aws CLI로 VPC 생성
`$aws ec2 create-vpc --cidr-block 10.0.0.0/16`

`$aws ec2 modify-vpc-attribute --vpc-id vpc-*/*/* --enable-dns-hostnames`

