## 첫 설정
+ cmd로 전에 발급받았던 csv파일의 정보를 바탕으로 작성하여 기본 설정을 한다.

<img src = "https://user-images.githubusercontent.com/55094745/104980231-73645600-5a49-11eb-83a1-cbc20bb140d4.png" width = "40%"></img>
> Default ouput format 으로는 text, json, table으로 이루어져있다. 

### 설정 확인
+ 미리 만든 디폴트 security group 참조

<img src = "https://user-images.githubusercontent.com/55094745/104980472-f38abb80-5a49-11eb-8f77-b0c02afc477e.png" width = "30%"></img>
> 나같은 경우는 이전에 먼저 vpc를 하나 사용자 정의로 만든 것이 하나 있어서 vpc가 2개가 뜸

### 여러 개의 profile 설정
+ --profile 옵션을 사용해서 별도로 프로필을 설정 파일로 저장

<img src = "https://user-images.githubusercontent.com/55094745/104980790-c7bc0580-5a4a-11eb-96e4-3ddcc62d7936.png" width = "40%"></img>
+ 설정한 profile로 명령어 실행 

<img src = "https://user-images.githubusercontent.com/55094745/104980923-0356cf80-5a4b-11eb-907b-33980a68ff34.png" width = "50%"></img>
`--profile <설정할 이름>
`

## AWS CLI의 사용방법

`aws <서비스 이름> <리소스 조작 명령어>
`
### AWS CLI의 기본적인 옵션

**--region : 리전을 일시적으로 바꿈**

**--output : 출력형식을 일시적으로 바꿈**

<img src = "https://user-images.githubusercontent.com/55094745/104981698-73b22080-5a4c-11eb-9ca6-1ebff0403499.png" width = "50%" ></img>

**--filters : 참조 계열 명령어를 사용할 때 검색 조건을 지정해서 필터링함**

+ 10.0.0.10 private 주소를 가진 인스턴스 참조

<img src ="https://user-images.githubusercontent.com/55094745/104982010-136fae80-5a4d-11eb-825d-756b688b2778.png" width = "50%"></img>

+ t2.medium 또는 m3.medium유형의 인스턴스 참조

<img src= "https://user-images.githubusercontent.com/55094745/104982138-60ec1b80-5a4d-11eb-965f-b943c2c39051.png" width = "50%"></img>
> "Name에 필터이름, Value에 필터이름에 대응하는 조건"


**--query : 실행 결과를 압축해서 보여줌**

