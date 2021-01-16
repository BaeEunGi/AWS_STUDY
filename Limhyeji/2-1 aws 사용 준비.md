# aws 사용 준비

## IAM

### 정책

- 아직 권한이 없으므로 **권한**을 부여해야 한다. 

- 권한은 IAM 정책으로 관리된다
- 정책은 **Managed Policies**와 **Inline Policies** 두 가지 종류가 있다

#### 1) Managed Policies

- IAM 사용자, IAM 그룹, IAM 규칙과 독립적인 IAM정책 기능이다

- 1개의 Managed Policy에는 여러 개의 사용자, 그룹, 규칙을 부여할 수 있다
- AWS Managed policies와 Customer Managed Policies 두가지 종류로 나뉜다

- **AWS Managed Policies**

  > AWS에서 생성, 관리하는 정책으로 역할 또는 서비스마다 큰 정책을 정의
  >
  > 사용자가 변경 불가
  >
  > 세부적인 설정을 따로 할 필요가 없을 때 적용 -> AWS가 관리하므로 기능 추가 또는 변경 발생 시 자동으로 정책 변경

- **Customer Managed Policies**

  > 사용자가 생성하고 자유롭게 변경할 수 있는 정책
  >
  > ex) 특정 IP에서의 동작만 허용할 경우 사용

#### 2) Inline Policies

- 특정 IAM 사용자, IAM 그룹, IAM 규칙에 직접적으로 적용하는 정책
- 특정 사용자에게만 권한을 부여하고 싶ㅇ느 등의 예외적인 상황에 사용
- 각 계정에 Inline Policies를 확인하려면 하나하나의 계정을 확인해야 하므로 귀찮음 -> Mamaged Policies 추천

### 정책 작성 방법

**JSON** 형식으로 작성

:white_check_mark: Policies -> AdministratorAccess -> Policy Document

- Effect

  > 허가 또는 거부 작성
  >
  > 허가 : Allow, 거부 : Deny

-  Action

  > 설정 대상 서비스와 대상 조작을 작성
  >
  > "*" : 모든 서비스의 모든 조작을 대상
  >
  > "s3:*" : s3의 모든 조작을 대상

- Resource 

  > 설정 대상 리소스를 작성
  >
  > S3의 특정 버킷만들 대상으로 하는 등에 사용
  >
  > 굉장히 세부적으로, 유연하게 설정 가능 -> ex) 특정 ip에서만 조작하게 설정 (회사 내부)

- Condition

  > 정책이 적용될 조건을 지정

**Policy Generator** 

- 정책을 자동으로 생성
- :white_check_mark: IAM 대시보드 -> Create Policy -> Policy Generator

### 고객 관리 정책 생성

AWS 관리 정책은 자동으로 변경 -> 실제 운용 시 고객 관리 정책을 활용하는 형태로 사용

**사용자 관리 정책의 종류**

|           선택지           |                             설명                             |
| :------------------------: | :----------------------------------------------------------: |
| Copy an AWS Managed Policy | AWS  관리 정책에 있는 내용을 복사하고 이를 기반으로 정책 생성 |
|      Policy Generator      |            Policy Generator를 사용해 정책을 생성             |
|   Create Your Own Policy   | Policy Generator을 사용하지 않고 정책을 하나하나 기술해서 생성<br />과거의 정책이 있다면 복사하고 붙여 사용 가능 |

 ### IAM 그룹 생성

**IAM 계정에 정책을 적용하는 방법**

1. 계정에 직접 적용
2. 정책을 적용한 IAM 그룹을 생성하고 계정을 그룹에 소속 -> 더 많이 사용

:white_check_mark: IAM -> Groups -> Create New Group

### IAM 계정을 IAM 그룹에 추가

그룹을 생성하는 것만으로는 IAM 계정에 권한이 부여되지 않는다 -> 그룹에 계정을 추가해야한다

:white_check_mark: Users -> Add Users to Group -> Add Users