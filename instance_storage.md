# Amazon EC2 - Instance Storage

### EBS Volume

- EBS(Elastic Block Store) Volume은 **실행중인 인스턴스에 부착할 수 있는 네트워크 스토리지**이다.
- 인스턴스가 종료되어도 **데이터를 영구 보관**할 수 있도록 해준다.
- **한 번에 하나의 인스턴스에만 마운트**될 수 있다.
    - 하나를 여러개의 인스턴스에 부착은 불가능하지만 여러개를 하나의 인스턴스에 부착은 가능하다.
- **특정 가용영역에 국한**되어 있다.
- 프로비저닝된 용량으로 사용 가능하다.(사용 중간에 용량을 늘릴 수 있다.)

### EBS - Delete on Termination attribute

- EC2 인스턴스의 종료에 따른 **EBS의 생명주기를 지정**할 수 있다.
- 기본값으로 EC2의 종료에 따라서 부착된 EBS 볼륨은 같이 삭제된다.
- 기본값으로, 다른 부착되지 않은 EBS 볼륨은 삭제되지 않는다.
- 추천하는 설정은 인스턴스가 종료되더라도 EBS 볼륨을 보존하는 것을 추천한다.

### EBS Snapshots

- EBS Volume을 제때 백업하는 것이다.
- 필수 설정은 아니지만 추천하는 설정이다.
- 이 스냅샷은 가용영역과 지역을 넘어서 복사하여 사용할 수 있다.

### EBS Snapshots Features

- EBS Snapshot Archive
    - Snapshot archive tier로 스냅샷을 이동시키는 것이 **75% 저렴하게 이용**할 수 있다.
    - archive tier에 존재하는 스냅샷을 복원시키는데 24~72시간이 소요된다.
- Recycle Bin for EBS Snapshots
    - 잘못 삭제된 볼륨을 되살리기 위해서 삭제된 스냅샷을 휴지통에 보관하는 설정이다.
    - **1일에서 최대 1년까지 보존이 가능**하다.
- Fast Snapshot Restore(FSR)
    - 빠른 스냅샷 복원도 사용이 가능하지만 매우 비싼 단점이 존재한다.

### EBS Volume Types

- EBS Volume의 종류는 그게 6개로 구분된다.
    - gp2 / gp3 (SSD): 범용 목적의 SSD 볼륨으로 다양한 워크로드에 있어서 균형잡힌 가격과 성능을 보인다.
    - io1 / io2 (SSD): 고성능의 SSD 볼륨으로 매우 낮은 지연성과 고성능의 워크로드에 적합하다.
    - st1 (HDD): 저렴한 가격의 HDD 볼륨으로 자주 액세스되는 데이터들에 있어서 적합하다.
    - sc1 (HDD): 가장 저렴한 HDD 볼륨으로 덜 자주 액세스되는 워크로드에 적합하다.
- 오직 gp2/gp3 그리고 io1/io2 볼륨만이 시작 볼륨으로 사용할 수 있다.

### General Purpose SSD

- 비용 효율적인 스토리지로 낮은 지연시간을 선보인다.
- 시스템의 시작 볼륨, 가상 데스크탑, 그리고 개발 및 테스트 목적으로 쓰인다.
- 1GiB ~ 16TiB 까지 존재한다.
- gp3
    - 기본 3000 IOPS 그리고 125 MiB/s의 성능을 보인다.
    - IOPS를 최대 16000까지 올릴 수 있으며, 출력성능 또한 1000MiB/s 까지 독립적으로 올릴 수 있다.
- gp2
    - 작은 gp2 볼륨은 3000 IOPS까지 올릴 수 있다.
    - IOPS가 연결되어있다면, 최대 IOPS는 16000이다.

### Provisioned IOPS(PIOPS) SSD

- 지속적인 IOPS 성능이 중요한 비즈니스 앱에 적합하다.
- 또한 16000 IOPS 이상이 필요한 앱에 적합하다.
- 데이터베이스 워크로드에 적합하다.
- io1/io2(4GiB ~ 16TiB)
- io2 Block Express(4GiB ~ 64TiB)
- EBS 여러개 부착을 지원한다. 즉, 하나의 볼륨을 여러개의 인스턴스에 부착할 수 있다.

### Hard Disk Drives(HDD)

- 시작 볼륨으로 사용할 수 없다.
- 125GiB ~ 16TiB 까지 존재한다.
- st1: 최적화된 HDD
    - 빅데이터, 데이터 웨어하우스, 로그 프로세싱에 적합하다.
    - 최대 출력은 500MiB/s ~ IOPS 500이다.
- sc1: 콜드 HDD
    - 자주 액세스되지 않는 데이터에 적합하다.
    - 비용이 가장 중요하다면 좋은 선택이다.
    - 최대 출력은 250MiB/s ~ 최대 250 IOPS 이다.

### EBS Multi-Attach - io1/io2 family

- io1/io2 볼륨은 같은 볼륨을 같은 가용영역에 있는 여러 개의 인스턴스에 부착할 수 있다.
- 한번에 최대 16개의 인스턴스에 부착할 수 있다.
- 반드시 cluster-aware한 파일 시스템을 사용해야 한다.

### EFS - Elastic File System

- 콘텐츠 관리, 웹 서빙, 데이터 쉐어링, 워드프레스에 많이 쓰이는 파일 시스템이다.
- NFSv4.1 프로토콜이다.
- EFS에 대한 액세스를 제어하기 위해서 시큐리티 그룹을 사용한다.
- Linux 기반의 AMI와 호환된다.
- KMS를 사용하여 암호화가 가능하다.
- 파일 시스템은 자동으로 스케일링이 되고, 사용량만큼 돈을 지불하고, 용량 계획은 존재하지 않는다.

### EFS - Storage Classes

- Storage Tiers(생명주기 관리 특성 - N일뒤에 파일 이동)
    - Standard: 자주 액세스되는 파일들
    - Infrequent access(EFS-IA): 저장하는데에는 적은 가격 지불, 복원하는데에 비용을 지불해야 한다.
- Availability and durability
    - Standard: Multi-AZ, 프로덕션 환경에 적합하다.
    - One Zone: One AZ, 개발 환경에 적합하고, 백업은 기본적으로 활성화되어 있다. IA와 호환된다.

### EBS vs EFS - Elastic Block Storage

- EBS Volumes
    - 하나의 인스턴스에만 부착이 가능하다.(io1/io2를 제외한)
    - 하나의 가용영역에 국한되어 있다.
    - gp2: 디스크 용량이 늘어남에 따라서 IO도 증가한다.
    - io1: IO를 독립적으로 증가시킬 수 있다.
- EBS 볼륨을 다른 AZ로 마이그레이션 하려면 스냅샷을 찍고, 이 스냅샷으로 다른 AZ에서 복원 절차를 걸쳐야 한다.
- 인스턴스의 루트 EBS 볼륨은 기본값으로 EC2의 종료에 따라 볼륨도 삭제된다.(이 속성을 비활성화 해야한다.)

### EBS vs EFS - Elastic File System

- AZ를 넘어 몇백개의 인스턴스에 부착할 수 있다.
- EFS를 공유하는 웹사이트 파일(워드프레스)을 만들 수 있다.
- 오직 리눅스 인스턴스와 호환된다.
- EFS는 EBS보다는 비용이 비싸다.
- 비용 절감을 위해서 EFS-IA를 고려할 수 있다.
