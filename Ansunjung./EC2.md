+ EC2 인스턴스 : EC2로 가동한 가상 컴퓨터
+ 퍼블릭 키, 프라이빗 키, security group, 글로벌 ip 생성 필요

## AWS 조작용 퍼블릭 키/프라이빗 키 생성(키 페어)
+ EC2 -> key pairs -> create keypair

<img src ="https://user-images.githubusercontent.com/55094745/105050786-7b57e080-5ab1-11eb-8107-17bf5b62a14c.png" width ="50%"></img>
<img src ="https://user-images.githubusercontent.com/55094745/105050991-ae9a6f80-5ab1-11eb-98bf-42c415898724.png" width = "50%"></img>

### AWS CLI를 사용할 때 
`$ aws ec2 create-key-pair --key-name myKeyPair
`

### AWS 외부에서 생성한 퍼블릭 키 임포트

<img src ="https://user-images.githubusercontent.com/55094745/105052430-3765db00-5ab3-11eb-9804-3052915aeb3c.png" width ="50%"></img>

```
  $ impkey ='cat ~/.ssh/id_rsa.pub'

  $ aws ec2 import-key-pair
  
  --key-name ImportKey
  
  --public-key-material ${impkey}*
```


## Security Group 생성
+ AWS의 방화벽
+ EC2 -> Security Groups -> Create security group

<img src ="https://user-images.githubusercontent.com/55094745/105053767-aa238600-5ab4-11eb-91d3-f7db87ead476.png" width ="50%"></img>

### security group 규칙 설정
+ EC2 인스턴스에 SSH로 로그인할 수 있도록 inbound를 다음과 같이 설정한다. 

<img src ="https://user-images.githubusercontent.com/55094745/105054744-ae03d800-5ab5-11eb-8ee2-7dad094e34a2.png" width ="50%"></img>

<img src ="https://user-images.githubusercontent.com/55094745/105055090-09ce6100-5ab6-11eb-870d-fbdd20db7e10.png" width ="%50"></img>
> inbound는 현재 보안 그룹에서 어떤 연결을 허가하는지 설정하는 것

+ 유형 : 어떤 종류의 통신인지 설정 SSH,HTTP 등등 (특수한 통신은 custom~으로 설정)
+ protocol/port range :  TCP, UDP, ICMP(transport layer) 등의 프로토콜 설정, port range는 0~65535범위의 포트를 지정
+ source : 허가할 연결 대상


## EC2 기동
+ EC2 -> Instances -> Launch Instance

<img src ="https://user-images.githubusercontent.com/55094745/105120735-24d0bd80-5b16-11eb-84c6-75b78af27207.png" width = "50%"></img>

### AMI선택
+ 시작시 가장 처음으로 선택해야할 일

<img src = "https://user-images.githubusercontent.com/55094745/105120919-8002b000-5b16-11eb-8936-0b30ea7c24da.png" width = "50%"></img>

### 인스턴스 유형 선택

<img src = "https://user-images.githubusercontent.com/55094745/105121039-c22bf180-5b16-11eb-8e34-2bd1981a5c6a.png" width = "50%"></img>
<img src = "https://user-images.githubusercontent.com/55094745/105124849-564d8700-5b1e-11eb-9087-4825856503a5.png" width = "50%"></img>
> monitoring을 활성화시키면 추가비용발생

+ 네트워크 인터페이스
> subnut을 지정하면 EC2 인스턴스의 프라이빗 IP를 지정할 수 있다. (아무것도 입력하지 않으면 subnet 내부에서 사용되지 않고 있는 ip주소 자동 할당)

<img src = "https://user-images.githubusercontent.com/55094745/105125266-500bda80-5b1f-11eb-94e8-e4c5aace3c18.png" width ="50%"></img>

### Advanced Details
+ User data
> 셸 스크립트 또는 cloud-init 디렉티브를 작성할 수 있다. 
> 코드를 작성하면 기동 할 때 패키지 설치, 서비스 기동, 사용자 또는 그룹생성을 할 수 있다. 

+ 셸 스크립트 apache 설치

<img src ="https://user-images.githubusercontent.com/55094745/105125702-62d2df00-5b20-11eb-9ec9-3827afc3cda0.png" width="50%"></img>

+ cloud-init 디렉티브 apache 설치

<img src = "https://user-images.githubusercontent.com/55094745/105125811-9a418b80-5b20-11eb-9a14-9668bbd007ca.png" width ="50%"></img>


### 스토리지 추가
+ EBS, 무료 로컬 디스크 추가 가능

<img src = "https://user-images.githubusercontent.com/55094745/105125893-d4ab2880-5b20-11eb-9bce-88b0100063d0.png" width = "50%"></img>


### 태그 설정
+ 인스턴스 이름, 실행한 사람 이름, 사용하고 있는 프로젝트 이름등을 사용해서 누가 무엇을 했는지 알 수 있다. 
+ API에 태그값을 추출하여 input값으로도 이용가능하다. 

<img src = "https://user-images.githubusercontent.com/55094745/105125983-0d4b0200-5b21-11eb-9e4d-10cac736dafd.png" width = "50%"></img>

### 보안 그룹 설정
 
<img src ="https://user-images.githubusercontent.com/55094745/105126106-5e5af600-5b21-11eb-8e68-1c1b384264e4.png" width = "50%"></img>

### AWS CLI 로 EC2 인스턴스 기동

+ $aws ec2 run-instances
> 이때 나오는 instanceid를 미리 적어두고
+ $aws ec2 create-tags
> 아까 적어둔 instanceid로 지정한다.

### EC2인스턴스에 로그인
+ EC2 -> Instances에 들어가서 인스턴스를 클릭한 후 DNS를 복사한다. 
+ $ chmod 0600 myKeyPair.pem
+ $ ssh -i myKeyPair.pem [DNS 붙여넣기]

## AMI 생성
+ AMI에는 OS정보 같은 인스턴스를 기동하기 위해 필요한 정보가 포함되어 있음
+ EBS에 저장되어 있는 데이터는 AMI에 포함되지 않는다.
+ AMI가 생성되는 동시에 EBS 스냅샷이 동시에 생성되는데 인스턴스가 어떤 EBS 스냅샷과 연결되어 있는지 알수 있는 정보가 제시된다

+ EC2 -> instance -> action -> create image

<img src = "https://user-images.githubusercontent.com/55094745/105127388-273a1400-5b24-11eb-827c-8deb955caeb4.png" width = "50%"></img>
+ images -> ami를 들어가보면 생성중인 것을 확인 할 수 있다. --> EC2에 로그인 불가

<img src = "https://user-images.githubusercontent.com/55094745/105127699-bfd09400-5b24-11eb-990b-73b0ea806846.png" width = "50%"></img>


## Elastic Ip사용
+ EC2에 할당할 수 있는 글로벌 ip는 두종류가 있는데, 하나는 퍼블릭ip(계속 바뀜) ,EIP(고정적인 ip)

+ EC2 -> Elastic ips-> allocate new address -> yes, allocate

<img src ="https://user-images.githubusercontent.com/55094745/105127938-44bbad80-5b25-11eb-859a-6089a90da143.png" width = "50%"></img>

+ 할당 후 ec2인스턴스에 적용

<img src ="https://user-images.githubusercontent.com/55094745/105128091-a67c1780-5b25-11eb-9190-720ccd8bc8d2.png" width ="50%"></img>
> 탄력적 ip주소 연결 선택

+ 인스턴스 선택 후 연결 클릭
+ EC2 -> instance에서 연결을 화긴 할 수 있음. 

<img src = "https://user-images.githubusercontent.com/55094745/105128318-230ef600-5b26-11eb-9d32-b1c7975fb995.png" width = "50%"></img>
> 탄력적 ip에 추출한 ip가 뜨면 성공
