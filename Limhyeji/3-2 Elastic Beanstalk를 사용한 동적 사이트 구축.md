# Elastic Beanstalk를 사용한 동적 사이트 구축

앞에서 만든 것과 같은 것을 Elastic Beanstalk를 사용해 구축해보기

### Elastic Beanstalk를 사용한 재구축

AWS 매니지먼트 콘솔, AWS CLI, eb 명령어로 조작 가능

### WordPress 다운로드

Elastic Beanstalk는 zip 파일로 애플리케이션 압축해서 업로드 가능

WordPress 파일을 zip파일로 내려받기  [사이트](https://ko.wordpress.org/)

### Beanstalk 애플리케이션 생성

:white_check_mark: Elastic Beanstalk -> Create new Application

#### New Envirionment

환경 유형 설정하기

-  WordPress 이므로 Web server envirionment 체크

#### Platform과 Application Code

플랫폼 : PHP 선택

Application Code : 기동할 애플리케이션 선택 -> 이전에 내려받은 WordPress 파일 선택

#### Network

애플리케이션 환경을 기동할 VPC 선택

퍼블릭 서브넷들을 선택

보안 그룹 선택

#### Database

RDS 인스턴스도 함께 생성하도록 설정

#### 애플리케이션 접근

Environment Information에서 지정한 URL 뒤에 "/wordoress"를 붙여 접근

### Elastic Beanstalk로 생성한 인스턴스

설정한 Key Pair를 사용해서 SSH로 로그인 가능

## Elastic Beanstalk를 사용한 로드밸런싱과 HTTPS 사이트 구축

Elastic Beanstalk를 사용하면 ELB와 Auto Scaling을 사용해서 확장 가능성 높은 시스템 간단하게 구축 가능

 AWS 매니지먼트 콘솔에서 애플리케이션 디플로이 뿐만 아니라 **eb** 명령어로도 디플로이 가능

### eb 명령어 설치

eb 명령어 : AWS에서 제공되는 Elastic Beanstalk 전용 CLI -> AWS CLI와 별도

```
$ pip install awsebcli
```

### eb 명령어로 애플리케이션 생성

- WordPress의 압축 해재 후 해당 폴더로 이동
- -p 옵션으로 플랫폼을 지정 

```
$ eb init -p php
```

- RDS와 애플리케이션 환경 생성

```
$ eb create wordpress-env --database --timeout 30
```

### WordPress 수정

WordPress를 HTTPS에 대응하게 하려면 코드를 수정해야 함

### ELB, RDS, Auto Scaling 설정 변경

- ELB : HTTPS, Sticky Session 설정
- RDS : Multi-AZ 설정

IAM에 저장되어 있는 ARN 추출하기

```
$ aws iam list-server-certifi cates
```

WordPress 디플로이

```
$ eb deploy --timeout 30
```

### 접근 확인

```
$ eb open
```

