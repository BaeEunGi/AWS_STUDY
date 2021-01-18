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


### Subnet 생성
+ subnet이란 큰 네트워크를 여러 개의 작은 네트워크로 분할해서 관리할 때의 관리 단위가 되는 네트워크를 나타냄
+ vpc -> subnets -> create subnet

<img src ="https://user-images.githubusercontent.com/55094745/104906737-728ae000-59c7-11eb-97d2-b1778b667320.png" width ="40%"></img>
> ELB를 사용하려면 28로 생성하면 안됨, 최소 24이상의 네트워크 범위로 설정해야함

<img src = "https://user-images.githubusercontent.com/55094745/104907131-f1801880-59c7-11eb-9dce-a7f010251973.png" width = "30%></img>
> auto-assign public ip를 yes라고 설정하면 자동적으로 퍼블릭 ip주소를 할당해준다. 


### AWS CLI로 subnet 생성
`$aws ㅁec2 create-subnet --vpc-id vpc-*/*/* --availability-zone ap-northeast-1a --cidr-block 10.0.0.0/24`
                                                                                                                            
                                                                                      
### route table 생성
+ 해당 subnet 내부에서 가동하는 ec2 인스턴스 네트워크 라우팅을 제한한다.
+ vpc->route tables -> create route table

### AWS CLI로 Route Table생성
`$aws ec2 create-route-table --vpc-id vpc-*/*/*`

### Subnet과 Route Table 연결

+ 생성된 subnet은 하나의 생성된 route table과 연결되어야함

+ 연결 순서
> + subnet 연결을 선택

<img src = "https://user-images.githubusercontent.com/55094745/104908264-8f281780-59c9-11eb-9a84-2e9c9e0d1e75.png" width ="40%"></img>
> + subnet 연결 편집 선택

<img src = "https://user-images.githubusercontent.com/55094745/104908377-b979d500-59c9-11eb-9f2c-ded19fe4519f.png" width = "40%"></img>
> + subnet을 선택하고 저장


### AWS CLI로 연결
`aws ec2 associate-route-table --route-table-id rtb-*/*/* --subnet-id subnet-*/*/*`



### Internet Gateway 생성
+ vpc 네트워크 내부에서 가동하는 ec2인스턴스가 인터넷을 통해 외부와 통신할 때 필요
+ 생성한 internet gateway는 route table의 타깃(대상)으로 사용함
+ vpc -> internet gateway -> create internet gateway

<img src = "https://user-images.githubusercontent.com/55094745/104909670-8d5f5380-59cb-11eb-843b-a19b6d885eec.png" width = "40%"></img>
<img src = "https://user-images.githubusercontent.com/55094745/104909750-aa942200-59cb-11eb-9f7b-4b4df32e9b32.png" width = "30%"></img> 
> 연결 후에 detached에서 attached로 바뀜

### AWS CLI로 Internet Gateway 생성

`aws ec2 create-internet-gateway`

`aws ec2 attach-internet-gateway --internet-gateway-id igw-*/*/* --vpc-id vpc-*/*/*`


### Internet Gateway를 라우팅 대상으로 지정

> + route table목록에 이전에 생성한 route table을 선택하고, routes 탭을 선택한다.

<img src = "https://user-images.githubusercontent.com/55094745/104910146-3dcd5780-59cc-11eb-9897-6558b326a9d0.png" width = "30%"></img>
> 이때 주소를 0.0.0.0/0으로 설정하면 모든 주소와 매치하게 됨


