# AWS SDK

- 프로그래밍 언어에서 AWS 서비스를 조작할 수 있는 라이브러리

- AWS SDK를 프로그래밍 언어 라이브러리로 설치하면 AWS CLI처럼 AWS 서비스, 리소스를 코드로 조작, 관리 가능

## 지원하는 프로그래밍 언어와 버전

Android, iOS, Java, .NET, Node.js, PHP, Python, Ruby, Go를 지원

### AWS SDK의 기본적인 사용 방법

#### Ruby, PHP

- 예제 프로그램 본체
  + Credentials 설정 파일의 내용을읽고 AWS SDK의 메서드를 실행해서 결과 출력
- Credentials 설정 파일
  + AWS 접근을 위한 Access Key, Secrete Key, 리전을 작성항 파일
  + 실제 환경 전용과 개발 환경 전용으로 구분해서 활용 가능

### Java

- aws-java-sdk
- 예제 프로그램 자체(AwsSdkSample.java)
  + Credentials 설정파일의 내용을 읽고 AWS SDK의 메서드를 실행해서 결과 출력
- build.xml
  + aws-java-sdk를 읽고 AwsSdkSample.java(예제 프로그램 본체)의 빌드를 실행하게 작성
- credentials.yml
  + AWS 접근을 위한 Access Key, Secret Key를 aws configure로 생성한 것과 같은 형식으로 작성
  + 리전은 AwsSdkSample.java(예제 프로그램 본체)에 작성

