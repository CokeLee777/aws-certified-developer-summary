# S3 Advanced
## Moving between Storage Classes

- 스토리지 클래스간의 객체를 옮길 수 있다.
- 덜 자주 액세스하는 객체는 Standard IA로 옮길 수 있고, 빠른 액세스가 필요하지 않은 보관용 객체의 경우에는 Glacier 또는 Glacier Deep Archive로 옮길 수 있다.
- 객체를 **스토리지간의 옮기는 메커니즘은 Lifecycle Rule에 의해서 자동화**될 수 있다.
## Lifecycle Rules

- Transition Actions: 다른 스토리지 클래스간의 이동에 대한 설정정보이다.
	- ex1) 생성 후 60일이 지난 객체들을 Standard IA로 옮기는 설정
	- ex2) 생성 후 6개월이 지난 객체들을 Glacier으로 옮기는 설정
- Expiration actions: 몇몇 기간이 지나면 객체를 만료(삭제)시킬 지에 대한 설정정보이다.
	- 만료(삭제)된 이후에 1년까지는 로그파일에 액세스는 가능하다.
	- 버전관리가 활성화 되어있다면 오래된 버전의 객체를 만료(삭제)하게 할 수 있다.
	- Multi-part 업로드가 미완료된 객체를 삭제시키게 할 수 있다.
- 규칙은 특정 prefix에 대해서 생성이 가능하다.(ex. s3://mybucket/mp3/*)
- 규칙은 특정 객체의 태그에 대해서 생성이 가능하다.(ex. Department: Finance)
## S3 Analytics - Storage Class Analysis

- 언제 객체를 올바른 스토리지 클래스에 옮길지에 대한 결정을 하게끔 분석을 해주는 서비스이다.
- Standard와 Standard IA만 동작한다.
	- One-Zone IA와 Glacier에는 동작하지 않는다.
- 분석은 일별로 업데이트된다.
- 24~48시간내에 데이터에 대한 분석을 볼 수 있다.
## S3 Event Notifications

- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication
- 객체의 이름에 대한 필터링도 가능하다.(*.jpg)
- S3에 이미지를 업로드할 때 썸네일을 만드는 용도로 많이 사용한다.(Lambda와 통합하는 듯)
- 원하는 만큼 많은 S3 Event 생성이 가능하다.
## S3 Event Notifications with Amazon EventBridge

- JSON 형식의 고급 필터링 옵션이다.(메타데이터, 객체 사이즈, 이름 등)
- 여러 목적지 설정이 가능하다.(Step Functions, Kinesis Streams / Firehose)
- EventBridge 트리거는 보관, 다시 재생 이벤트 등이 가능하다.
## S3 Baseline Performance

- S3는 오토스케일링이 가능하다.
- 최소 3500번의 PUT/COPY/POST/DELETE 또는 5500번의 GET/HEAD 요청을 버킷내의 prefix당 초당 요청을 보낼 수 있다.
- 버킷 내의 prefix의 수는 제한이 없다.
## S3 Performance

- Multi-Part upload
	- 100MB보다 큰 파일의 업로드에 추천한다.
	- 5GB보다 큰 파일은 필수이다.
	- 병렬적인 업로드가 가능하다.
- S3 Transfer Acceleration
	- AWS 엣지 로케이션을 이용하여 파일의 전송 속도를 높이는 방법이다.
	- Multi-Part upload와 호환된다.
## S3 Performance - S3 Byte-Range Fetches

- 특정 byte 범위를 요청함으로써 병렬적으로 객체를 조회할 수 있다.
	- 이는 즉, byte를 쪼개서 한 객체를 병렬적으로 가져온다는 의미이다.
- 이는 다운로드 시 속도의 향상을 기대할 수 있다.
- 오직 데이터의 특정 부분만 보관할 때 사용된다.
## S3 Select & Glacier Select

- 서버 사이드 필터링을 통한 SQL을 사용하여 특정 부분의 데이터를 검색할 수 있다.
- 레코드와 컬럼 수를 필터링할 수 있다.
- network 전송 비용을 아끼고, 클라이언트 사이드의 CPU 비용을 절약할 수 있다.
## S3 User-Defined Object Metadata & S3 Object Tags

- S3 User-Defined Object Metadata
	- 객체를 업로드할 때, 메타데이터를 할당할 수 있다.
	- Name-value(Key-value) 쌍으로 할당 가능하다.
	- 이 메타데이터의 이름은 반드시 'x-amz-meta-'로 시작해야한다.
	- AWS는 key를 소문자로 저장하게된다.
	- 메타데이터는 객체를 검색할 때 메타데이터로 검색할 수 있게 된다.
- S3 Object Tags
	- Key-Value 쌍으로 구성되어있다.
	- 태그별로 권한을 부여할 때 유용하다.
	- 분석의 목적으로 사용할 때 유용하다.
- 객체의 메타데이터 또는 태그 그 자체를 검색할 수는 없다.
	- 이를 사용하려면 DynamoDB와 같은 인덱스가 달린 외부 DB를 사용해야 한다.
# S3 Security
## Object Encryption

- S3 bucket에 존재하는 객체를 4가지 방식으로 암호화할 수 있다.
- Server-Side Encryption(SSE)
	- Amazon S3-Managed Keys(SSE-S3)
		- 기본값으로 지정되어있다.
		- AWS에 의해서 관리되고 소유되는 암호화 키를 이용해서 S3 객체를 암호화한다.
	- AWS KMS(SSE-KMS)
		- 암호화 키를 관리하는 AWS Key Management Service를 이용하여 객체를 암호화한다.
	- Customer-Provided Keys(SSE-C)
		- 내가 제공한 암호화 키를 이용하여 객체를 암호화한다.
- Client-Side Encryption
## SSE-S3

- AWS에 의해서 관리되고 소유되는 암호화 키를 이용해서 S3 객체를 암호화한다.
- 객체는 서버 사이드에서 암호화된다.
- AES-256 해시 알고리즘을 이용하여 암호화된다.
- 헤더에 "x-amz-server-side-encryption": "AES256" 을 반드시 포함해야한다.
- 새로운 버킷이나 새로운 객체에 대해서 기본값으로 활성화 되어있다.
## SSE-KMS

- AWS KMS(Key Management Service)에 의해서 소유되고 관리되는 암호화 키를 이용하여 S3 객체를 암호화한다.
- KMS를 사용하는 이점으로는 CloudTrail을 이용하여 암호화 키가 사용되는 것을 제어하거나 추적할 수 있다.
- 객체는 서버 사이드에서 암호화된다.
- 헤더에 "x-amz-server-side-encryption": "aws:kms" 을 반드시 포함해야한다.
## SSE-KMS Limitation

- S3 객체 암호화를 SSE-KMS를 사용한다면 KMS 사용량 제한이 걸릴 수있다.
	- 암호화(업로드)할 때 KMS의 GenerateDataKey API를 사용
	- 복호화(다운로드)할 때 Decrypt API를 사용
- 사용량 제한을 Service Quotas Console에서 증가시키도록 요청할 수 있다.
## SSE-C

- 사용자에 의해 관리되는 암호화 키를 이용해서 업로드 시 같이 포함하여 요청하여 서버 사이드에서 객체를 암호화하는 방식이다.
- S3는 사용자가 제공하는 암호화 키를 저장(보관)하지 않는다.
- **반드시 HTTPS로 요청**해야한다.
- 암호화 키는 매 HTTP 요청마다 반드시 HTTP 헤더에 포함되어야 한다.
## Client-Side Encryption

- 사용자가 암호화 키와 암호화 로직을 모두 관리하는 암호화 방식이다.
- 객체를 업로드하기 전에 사용자는 반드시 객체를 암호화하여 업로드 해야한다.
- 객체를 다운로드할 떄 사용자는 반드시 객체를 복호화하여 다운로드 해야한다.
## Encryption in transit(SSL/TLS)

- 전송중 암호화를 SSL/TLS라고 부른다.
- S3는 두 가지의 엔드포인트를 제공한다.
	- HTTP Endpoint: 암호화 제공 X
	- HTTPS Endpoint: 전송중 암호화 제공
- HTTPS가 추천되어지고, SSE-C를 사용한다면 HTTPS가 필수이다.
## CORS

- 사용자가 S3 버킷에 cross-origin 요청을 한다면, S3는 응답으로 올바른 CORS 헤더를 내뱉어야 한다.
- 특정 Origin에 대해서 지정이 가능하다.
## MFA Delete

- S3에서 중요한 API Call을 할 때 MFA 인증이 필요하다.
- 객체의 특정 버전을 영구적으로 삭제할 때, 또는 버킷의 버전관리를 중지할 때 필요하다.
- 버전관리를 활성화할 때나 삭제된 버전들을 조회할 때는 필요하지 않다.
- MFA Delete를 사용하기 위해서는 반드시 버킷에서 버전관리를 활성화해야한다.
- 오직 버킷 소유자가 MFA Delete를 활성화/비활성화가 가능하다.
## S3 Access Logs

- 모니터링 목적으로 S3 버킷에 들어오는 모든 요청에 대한 로그를 로깅용 버킷에 수집할 . 수있다.
- 로깅용 데이터는 데이터 분석 툴을 이용하여 분석될 . 수있다.
- 로깅용 버킷은 같은 Region에 위치해야 한다.
- 로깅용 버킷에 대한 로그를 다시 로깅용 버킷에 수집하는 행위는 절대 하면 안된다.
	- 버킷의 용량이 기하급수적으로 늘어날 수 있다.
## Pre-Signed URL

- S3 콘솔, CLI, SDK를 이용하여 pre-signed URL을 생성할 수 있다.
- pre-signed URL을 발급하여 객체를 조회하거나 업로드하는 행위를 만료기간동안 할 수 있다.
## Access Points

- S3 버킷을 위한 보안 관리자를 말한다.
- 각각의 Access Points는 DNS name이 될 . 수있고, access point policy가 될 수 있다.
## Access Points - VPC Origin

- access point를 오직 VPC 내부에서만 접근이 가능하게끔 할 수 있다.
- Access Point에 접근할 수 있도록 VPC Endpoint를 반드시 만들어야한다.
- VPC Endpoint 정책은 반드시 타깃 버킷과 Access Point에 접근이 허용하게끔 해야한다.
## S3 Object Lambda

- 객체를 검색하여 객체를 얻기 전에 Lambda 함수를 통해서 변화된 객체를 얻게할 수 있다.