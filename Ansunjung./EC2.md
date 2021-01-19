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
