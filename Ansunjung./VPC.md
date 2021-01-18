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

