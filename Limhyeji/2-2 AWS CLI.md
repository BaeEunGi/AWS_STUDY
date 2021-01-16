# AWS CLI

- AWS 서비스를 명령어로 조작할 수 있게 해주는 통합 툴

- 기본적으로 AWS 매니지먼트 콘솔과 거의 같은 조작 가능

## AWS CLI 설치와 설정

각 운영체제에 따라 설치

### ASW CLI의 기본 설정

- AWS Access Key ID, AWS Secret Access Key

  > AWS CLI를 실행하기 위한 인증 정보
  >
  > IAM 사용자마다 생성 가능

- Default region name

  > 디폴트로 사용할 리전 이름

- Default output format

  > AWS CLI 실행 결과 출력 형식
  >
  > text, json, table

### 여러개의 profile 설정

AWS CLI는 설정 내용을 profile 형식으로 여러 개 등록해서 원하는 설정을 선택해서 사용 가능

:white_check_mark: aws configure --profile 이름

설정한 profile로 명렁어를 실행할 때 --profile 옵션을 사용해 설정한 profile 지정 가능

### 설정 파일 저장소

AWS CLI 설정 파일은 Windows에서 사용자 폴더 내부에 있는 ".aws" 폴더에 저장된다

## AWS CLI의 기본적인 사용 방법

AWS CLI를 사용하면 AWS의 다양한 서비스의 참조, 생성, 수정, 제거 조작과 특정 서비스에 특화된 조작 가능

### AWS CLI의 기본적인 명령어 형태

:white_check_mark: aws <서비스 이름> <리소스 조작 명령어>

ex) aws ec2 describe-security-groups / aws s3 ls

### AWS CLI의 기본적인 옵션

|   옵션    |                        처리                        |
| :-------: | :------------------------------------------------: |
| --profile |           설정한 profile로 명령어를 실행           |
| --region  |                    리전을 지정                     |
| --output  |                  출력 형식을 지정                  |
| --filters | 참조 계열 명령어 사용 시 검색 조건 지정하여 필터링 |
|  --query  |           실행 결과 내용을 압축하여 출력           |

### --region, --output을 사용한 리전, 출력 형식의 지정

기본 설정에서 설정한 Default region name과 Default output format은 일시적으로 변경 가능

:white_check_mark: aws ec2 describe-security-groups --region us-east-1 --output json

-> 리전을 us-east-1, 출력형식을 JSON으로 지정하여 명령어 실행

### --filters 옵션을 사용한 검색 조건 지정

- **참조 계열 명령어**를 실행할 때 검색 조건 지정 가능

- --filteres 옵션의 필터 이름은 서비스, 리소스에 따라 차이가 있다

```
aws <서비스 이름> <리소스 조작 멸령어> --filters "Name=<필터이름A>, Values=<조건A1>" "Name=<필터이름B>, Values=<조건B1><조건B2>"
```

### --query 옵션을 사용한 출력 결과 압축

- 쿼리 이름 까지의 계층 구조 표현을 작은 따옴표로 감싼다
- 쿼리 이름은 쉼표로 구분한다
- JSON 형식, Table 형식으로 출력시 쿼리 이름에 키(별칭)을 붙여야 한다
- --filters와 마찬가지로 조건을 지정할 수 있다

#### 1) 첫 번째 계층에 출력할 쿼리 이름이 있는 경우

```
aws <서비스이름> <리소스 조작 명령어> -- query '<쿼리 이름(1번째 계층)>'
```

#### 2) 첫 번째 계층이 리스트 구조로 되어 있고 두 번째 계층에 출력할 쿼리 이름이 있는 경우

```
aws <서비스이름> <리소스 조작 명령어> -- query '<항목 이름(1번째 계층)>[].<쿼리 이름(2번째 계층)>'
```

### 3) 첫 번째, 두 번째 계층이 리스트 구조로 되어있고 세 번째 계층에 출력할 쿼리 이름이 있는 경우

```
aws <서비스이름> <리소스 조작 명령어> -- query '<항목 이름(1번째 계층)>[].<항목 이름(2번째 계층)>[].[<쿼리 이름A(3번째 계층)><쿼리 이름B(3번째 계층)>]'
```



