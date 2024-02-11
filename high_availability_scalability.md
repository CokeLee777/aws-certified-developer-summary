# High Availability & Scalability

### Scalability & High Availability

- Scalability란 어플맄에ㅣ션이나 시스템이 많은 트래픽을 수용할 수 있느냐에 관한 것이다.
- 확장성은 두 가지로 나뉜다.
    - Vertical Scalability
    - Horizontal Scalability
- 확장성은 고가용성과 밀접한 관련이 있지만 둘은 다르다.

### Vertical Scalability

- 수직 확장성은 인스턴스의 스펙을 올리는 것이다.
- 예를들어 t2.micro의 인스턴스를 t2.large로 올리는 것이다.
- 수직 확장성은 분산환경의 시스템이 아닌 곳에서 매우 흔하다. 예를들어 데이터베이스와 같은 시스템 말이다.
- RDS, ElastiCache와 같은 서비스들이 있다.

### Horizontal Scalability

- 수평 확장성은 인스턴스의 개수를 늘리는 것이다.
- 즉, 시스템을 분산환경으로 만드는 것이다.
- 이 확장성은 웹 애플리케이션이나 현대적인 애플리케이션에서 매우 흔한 예시이다.

### High Availability

- 고가용성은 수평 확장성과 매우 밀접한 관련이 있다.
- 고가용성은 애플리케이션을 최소 2개의 데이터센터(가용영역)에서 실행시키는 것을 의미한다.
- 고가용성의 목적은 데이터센터의 손실을 막기 위한 것이다.
- 고가용성은 기본값이 될 수 있고, 또는 선택이 될 수 있다.

### High Availability & Scalability For EC2

- Vertical Scaling: 인스턴스의 스펙을 높이는 것(Scale up / down)
- Horizontal Scaling: 인스턴스의 개수를 높이는 것(Scale out / in)
- High Availability: 여러 가용영역에 걸쳐서 같은 애플리케이션을 여러개 실행시키는 것

### Load Balancing

- 로드 밸런싱은 들어오는 트래픽을 여러 서버에 분산시켜 보내는 것이다.

그렇다면 로드 밸런서는 무엇일까?

- 여러개의 인스턴스에 부하를 분산시켜 보내는 것을 의미한다.
- 같은 엔드포인트로 트래픽을 받는다.
- 인스턴스에 규칙적인 헬스체크를 시행한다.
- 웹사이트에 SSL 종료를 제공한다.
- 쿠키와 함께 Enforce stickiness를 제공한다.
- 고가용성
- 프라이빗 트래픽으로부터 퍼블릭 트래픽을 분리한다.

### Elastic Load Balancer

- Elastic Load Balancer는 관리형 로드밸런서이다.
- 많은 AWS 서비스와 통합될 수 있다.
    - EC2, EC2 ASG, ECS
    - ACM, CloudWatch
    - Route 53, WAF, Global Accelerator

### Health Checks

- 헬스체크는 로드밸런서에 있어서 중요하다.
- 로드밸런서로 하여금 인스턴스가 트래픽 라우팅에 대한 요청에 응답이 가능한지 알 수 있도록 해준다.
- 응답이 200 OK가 오지 않는다면 로드밸런서는 인스턴스가 unhealthy 라고 판단한다.

### Types of load balancer on AWS

- AWS에는 4가지의 로드밸런서가 존재한다.
- Classic Load Balancer(v1 - old generation)
    - HTTP, HTTPS, TCP, SSL(secure TCP)
- Application Load Balancer(v2 - new generation)
    - HTTP, HTTPS, **WebSocket**
- Network Load Balancer(v2 - new generation)
    - TCP, TLS(secure TCP), **UDP**
- Gateway Load Balancer
    - Operates at layer 3(Network layer)
- 몇몇 로드밸런서들은 내부(프라이빗)로 구축할 수 있다.

### Classic Load Balancers(v1)

- Supports TCP(Layer 4), HTTP & HTTPS (Layer 7)
- Health checks 는 TCP 또는 HTTP 기반이다.
- 고정된 엔드포인트가 존재한다.

### Application Load Balancer(v2)

- Layer 7 (HTTP)에서 동작하는 로드밸런서이다.
- 여러개의 인스턴스에서 동작하는 여러개의 HTTP 어플리케이션에 대해서 부하 분산을 해준다.(target groups)
- 같은 인스턴스에서 동작하는 여러개의 애플리케이션에 대해서 부하 분산을 해준다.(containers)
- HTTP/2와 WebSocket을 지원한다.
- 리다이렉트도 지원한다.(from HTTP to HTTPS)

- 여러 다른 타겟 그룹에 해당하는 라우팅 테이블 구축 가능
    - path in URL(example.com/users & example.com/posts)
    - hostname in URL(one.example.com & other.example.com)
    - Query String, Headers(example.com/users?id=123&order=false)
- ALB는 마이크로서비스나 컨테이너 기반의 애플리케이션에 적합하다.

### Application Load Balancer(v2) Target Groups

- EC2 인스턴스
- ECS 작업
- Lambda functions
- IP Addresses

- ALB는 여러개의 타겟 그룹을 라우팅할 수 있다.
- 헬스체크는 타겟그룹 레벨에서 진행된다.

### Network Load Balancer(v2)

- Layer 4에서 동작하는 로드밸런서이다.
    - TCP & UDP 트래픽을 인스턴스로 보낸다.
    - 초의 수백만건의 요청을 다룰 수 있다.
    - 낮은 지연시간을 보인다.
- NLB는 가용영역당 하나의 고정된 IP를 가지고 있고, 탄력적 IP도 할당할 수 있다.

### Network Load Balancer - Target Groups

- EC2 인스턴스
- IP 주소 - 반드시 프라이빗 IP이어야 한다.
- Application Load Balancer
- 헬스체크는 TCP, HTTP, HTTPS를 지원한다.

### Gateway Load Balancer

- Layer 3에서 동작하는 로드밸런서이다. - IP Packets
- 다음과 같은 함수와 함께 사용이 가능하다.
	- Trasparent Network Gateway: 모든 트래픽에 대해서 하나의 입구 또는 출구가 있음
	- Load Balancer: 가상 어플라이언스에 대해서 트래픽을 분산시켜줌

### Gateway Load Balancer - Target Groups

- EC2 instances
- IP Addresses - private IP 만 가능

### Sticky Sessions(Session Affinity)

- 같은 사용자가 항상 같은 인스턴스로 리다이렉트 할 수 있도록 하는 속성이다.
- 이 속성은 Classic Load Balancer, Application Load Balancer, Network Load Balancer에서 동작한다.
- CLB & ALB 둘 다 stickiness로 사용되는 cookie는 만료 시간이 있다.(내가 만료시간을 제어할 수 있음)
- 사용자는 이 세션 데이터를 잃지 않는다.
- 하지만 이 stickiness 속성을 활성화하면 부하 분산에 대해서 불균형을 초래할 수 있다.

### Sticky Sessions - Cookie Names

- Application-based Cookies
	- Custom cookie
		- target에 의해서 생성된다.
		- 애플리케이션이 필요로하는 커스텀 속성을 포함시킬 수 있다.
		- 쿠키 이름은 각각의 타겟 그룹에 대해서 구체적이어야 한다.
		- 쿠키 이름으로 **AWSALB**, **AWSALBAPP**, **AWSALBTG**를 사용하면 안된다.(ELB가 이미 사용하는 예약어이다.)
	- Application Cookie
		- 로드밸런서에 의해서 생성된다.
		- 쿠키 이름은 **AWSALBAPP** 이다.
- Duration-based Cookies
	- 로드밸런서에 의해서 생성된다.
	- 쿠키 이름은 ALB를 위한 쿠키로 AWSALB, CLB를 위한 쿠키로 AWSELB가 있다.

### Cross-Zone Load Balancing

- 교차영역 로드밸런싱은 부하를 인스턴스에 개수에 해당하는 비율에 고르게 부하 분산을 해주는 것이다.
- 다음 그림을 보면 이해가 빠를 것이다.
![[crosszone_load_balancing.png]]
- 교차영역 로드밸런싱을 활성화하지 않으면 다음 그림과 같이 부하 분산이 이루어진다.
![[crosszone_load_balancing2.png]]
- 각 로드밸런서에 해당하는 교차영역 로드밸런싱의 특징은 다음과 같다.
	- Application Load Balancer
		- 기본값으로 활성화 되어있다.(Target Group 레벨에서 비활성화할 수 있다.)
		- 가용영역 데이터간의 추가 비용이 발생하지 않는다.
	- Network Load Balancer & Gateway Load Balancer
		- 기본값으로 비활성화 되어있다.
		- 만약 활성화되어 있다면 가용영역 데이터간의 추가 비용이 발생한다.
	- Classic Load Balancer
		- 기본값으로 비활성화 되어있다.
		- 만약 활성화되어 있다면 가용영역 데이터간의 추가 비용이 발생하지 않는다.

### SSL/TLS - Basics

- SSL Certificate 는 클라이언트와 로드밸런서 사이의 암호화된 전송을 가능하게 한다.
- SSL는 Secure Sockets Layer을 뜻하고, 암호화된 커넥션을 만들 때 사용한다.
- TLS는 Transport Layer Security를 뜻하고, 새로운 버전의 SSL이다.
- 요즘은 TLS Certificate 가 기본적으로 쓰인다. 하지만 사람들은 아직 SSL을 많이 참조한다.
- Public SSL certificate는 Certificate Authorities(CA)에서 발급된다.
- SSL certificate는 만료기한이 있으므로 갱신해야 한다.

### Load Balancer - SSL Certificates

- Load Balancer는 X.509 certificate를 사용한다.
- ACM을 이용하여 인증서를 관리할 수 있다.(AWS Certificate Manager)
- 대안으로 다른 곳에서 인증서를 발급받아 업로드가 가능하다.
- HTTPS listener
	- 반드시 기본 certificate를 명시해야 한다.
	- 여러개의 도메인이 존재하는 선택적 리스트의 인증서를 등록할 수 있다.
	- 그들이 도달하는 호스트명을 구체하하기 위해서 SNI(Server Name Indication)을 사용할 수 있다.
	- 오래된 버전의 SSL/TLS를 지원하기 위한 보안 정책을 구체화할 수 있다.

### SSL - Server Name Indication(SNI)

- SNI는 여러개의 SSL 인증서를 하나의 웹 서버에 대해서 로드하기 위한 솔루션이다.
- 새로운 프로토콜이며, 클라이언트가 타겟 서버의 호스트명을 알 수 있도록 해준다.
- 서버는 그런다음 올바른 인증서를 찾거나 기본 인증서를 반환한다.

SNI 는 오직 ALB & NLB, CloudFront에서만 동작한다. CLB에서는 동작하지 않는다.

### Elastic Load Balancers - SSL Certificates

- Classic Load Balancer(v1)
	- 오직 하나의 SSL 인증서를 지원한다.
	- 여러개의 SSL 인증서를 사용하기 위해서는 여러개의 CLB를 사용해야 한다.
- Application Load Balancer(v2)
	- 여러개의 SSL 인증서와 함께 여러개의 리스너를 지원한다.
	- 이를 가능하게 하기 위해서 내부적으로 SNI를 사용한다.
- Network Load Balancer(v2)
	- 여러개의 SSL 인증서와 함께 여러개의 리스너를 지원한다.
	- 이를 가능하게 하기 위해서 내부적으로 SNI를 사용한다.

### Connection Draining

- 이름 규칙
	- Connection Draining - CLB
	- Deregistration Delay - ALB, NLB
- 인스턴스가 등록 해제중이거나 unhealthy 상태일 때 in-flight-request를 완료하기까지의 시간을 의미한다.
- 등록 해제중인 인스턴스로 가는 새로운 요청을 보내는 것을 막는 역할을 한다.
- 기본값을 300초이고, 1~3600초까지 설정이 가능하다.
- 비활성화할 수 있다.
- 만약 너 요청이 짧다면 낮은 값으로 설정해도 상관없다.

### Auto Scaling Group

- 부하가 늘어남에 따라서 Scale out을 자동으로 진행
- 부하가 줄어듦에 따라서 Scale in을 자동으로 진행
- 최소와 최대 개수의 인스턴스를 보장해준다.
- 자동적으로 새로운 인스턴스를 로드밸런서에 등록한다.
- 하나의 인스턴스가 종료되거나 unhealthy 상태이면 인스턴스를 새로 만든다.

### Auto Scaling Group Attributes

- Launch Template을 사용하여 등록해야 한다.
- 최소 사이즈, 최대 사이즈, 최초 용량을 기입해야 한다.
- Scaling Policies 필요하다.

### Auto Scaling - CloudWatch Alarms & Scaling

- CloudWatch alarms 기반의 ASG를 사용하여 스케일링 가능하다.
- 알람은 평균 CPU 또는 커스텀 매트릭과 같은 매트릭 정보를 모니터링하고 있다.
- 알람으로 인스턴스를 스케일 아웃 하거나 스케일 인 할 수 있다.

### Auto Scaling Groups - Dynamic Scaling Policies

- Target Tracking Scaling
	- 가장 간단하고 쉬운 방법이다.
	- 평균 40%의 CPU 점유율을 유지하고 싶을 때
- Simple / Step Scaling
	- 만약 (CPU 점유율이 70%가 넘어가는) CloudWatch alarm이 트리거 된다면, 2개로 인스턴스를 늘린다.
	- 만약 (CPU 점유율이 30% 아래로 떨어지는) CloudWatch alarm이 트리거 된다면, 인스턴스 1개를 지운다.
- Scheduled Actions
	- 특정 규칙에 따라서 스케일링을 수행한다.
	- 금요일 10시부터 5시까지 원하는 최소 용량을 늘린다.

### Auto Scaling Groups - Predictive Scaling

- 지속적으로 부하를 예측한 다음 미리 스케일링을 예약하는 방법이다.
- 갑작스런 부하 증가에 따른 애플리케이션의 다운을 예방할 수 있다.

### Good metrics to scale on

- CPUUtilization: 평균 CPU 사용량
- RequestCountPerTarget: 각각의 EC2 인스턴스에 가해지는 요청 수
- Average Network In / Out: 애플리케이션이 네트워크 바운드 기반이라면
- Any Custom metric

### Auto Scaling Groups - Scaling Cooldowns

- 스케일링이 일어나면 cooldown 기간이 존재한다.(기본값은 300초이다.)
	- 이 쿨다운이 만약 0초라면 예를들어 cpu 사용량이 50%가 넘는다면 즉시 인스턴스를 추가하게 된다. 0초이기 때문에 50%가 계속 넘어가있다면 계속해서 인스턴스를 추가하게 되는 것이다.
	- 따라서 쿨다운을 적절히 설정해야지만 cpu 사용량이 50%가 넘었다가 쿨다운 기간에 50% 이하로 떨어진다면 인스턴스를 추가하지 않을 것이다.
- Cooldown 기간에는 ASG는 추가적인 인스턴스를 실행시키지도 종료시키지도 않는다.
- 추천하는 방식은 준비된 AMI를 사용하는 것이다. 이로 인해서 환경설정하는 시간을 단축시켜서 요청을 빠르게 처리하고, cooldown 기간을 줄일 수 있다.

### Auto Scaling - Instance Refresh

- 시작 템플릿을 업데이트하고 모든 인스턴스를 다시 만드는 것이다.
- 이를 위해서는 minimum healthy percentage를 잘 설정해야 할 것이다.
	- minimum healthy percentage가 60%라면 인스턴스가 모두 종료되고 다시 생성되는 것이 아닌, 60%의 정상적인 인스턴스의 개수를 유지하면서 종료하고 실행시킬 것이다.
- 구체적인 웜업 타임을 지정해야 할 것이다.
