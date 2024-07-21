## EC2 Instance Metadata(IMDS)

- AWS EC2 Instance Metadata(IMDS)는 실행중인 인스턴스를 구성하거나 관리하는데 사용되는 인스턴스에 대한 데이터를 의미한다.
- URL은 http://169.254.169.254/latest/meta-data 이다.
- Metadata: EC2 Instance에 대한 정보
- Userdata: EC2 Instance의 실행 스크립트
## IMDSv2 vs. IMDSv1

- IMDSv1 은 http://169.254.169.254/latest/meta-data 로 직접 액세스가 가능하다.
- IMDSv2는 더 보안성이 강화되었고 다음과 같은 방식으로 액세스가 가능하다.
	- 만료일이 있는 Session Token을 발급한다.
	- Session Token으로 IMDSv2를 액세스한다.
## MFA with CLI

- MFA를 사용하여 CLI를 접속하려면 반드시 임시 세션을 발급받아야 한다.
- 이를 위해서는 STS GetSessionToken API를 호출해야한다.
- 다음과 같은 명령어를 사용하여 발급받는다.
```shell
aws sts get-session-token --serial-number arn-of-the-mfa-device --token-code code-from-token --duration-seconds 3600
```
## AWS SDK Overview

- CLI를 사용하는 것 이외에 애플리케이션에서 AWS에 직접적으로 액세스할 수 있는 방식이다.
- 공식적으로 제공하는 SDK는 다음과 같다.
	- Java
	- .NET
	- Node.js
	- PHP
	- Python(named boto3 / botocore)
	- Go
	- Ruby
	- C++
## AWS Limits(Quotas)

- API Rate Limits
	- EC2를 위한 DescribeInstances API는 초당 100개의 제한이 존재한다.
	- S3의 GetObject는 prefix당 초당 5500개의 제한이 존재한다.
	- Rate Limit에 대한 간헐적인 에러는 지수 백오프 전략을 구현하여 해결하면 된다.
	- Rate Limit에 대한 지속적인 에러는 스로틀링 제한을 증가시키는 API를 호출하여 제한을 늘리는 방식으로 해결하면 된다.
- Service Quotas(Service Limits)
	- 실행중인 온디맨드 인스턴스에 대한 제한은 1152 vCPU의 제한이 존재한다.
	- 서비스의 제한을 증가시키고 싶다면 ticket을 열어서 요청할 수 있다.
	- 서비스의 할당량을 증가시키고 싶다면 Service Quotas API를 사용하여 해결할 수 있다.
## Exponential Backoff(any AWS Service)

- 간헐적으로 ThrottlingException이 발생한다면 지수 백오프 전략을 사용하여 해결하는것을 추천한다.
- 재시도 메커니즘은 이미 AWS SDK API 호출에 포함되어있다.
## AWS CLI Credentials Provider Chain

- CLI는 계정정보를 다음과 같은 순서로 확인한다.

1. Command line options: --region, --output, --profile
2. Environment variables: AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN
3. CLI credentials file: aws configure ~/.aws/credentials
4. CLI configuration file: aws configure ~/.aws/config
5. Container credentials: ECS tasks
6. Instance profile credentials: EC2 Instance Profiles
## AWS SDK Default Credentials Provider Chain

- Java System properties: aws.accessKeyId and aws.secretKey
- Environment variables: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
- 기본 credential profiles file: ~/.aws/credentials
	- 다른게 없으면 로컬에 있는 ~/.aws/credentials를 보고 설정한다.
- Amazon ECS container credentials: ECS containers
- Instance profile credentials: EC2 Instance
## Signing AWS API requests

- AWS HTTP API를 호출할 때 AWS Credentials를 사용하여 요청에 sign을 하여 AWS가 인지할 수 있도록 할 수 있다.
- S3에 요청하는 **몇몇 요청은 sign이 필요하지 않다.**
- 만약 SDK, CLI를 사용한다면 자동으로 sign을 해준다.
- Signature v4(SigV4)를 사용하여 AWS HTTP 요청에 sign해야한다.
- HTTP Header 또는 Query String에 signature를 포함하여 sign이 가능하다.