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
