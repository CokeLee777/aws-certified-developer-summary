# 개요

- VPC란 애플리케이션을 배포할 수 있는 **private network** 이다.
- 또한 Subnets 은 VPC내에 네트워크들을 나눈 것을 의미한다.
- Public Subnet은 **인터넷에서 액세스가 가능**한 서브넷을 의미한다.
- Private Subnet은 **인터넷에서 액세스가 불가능**한 서브넷을 의미한다.
- 인터넷과 Subnet 사이의 액세스하는 방법을 정의하려면 Route Tables를 사용한다.

## Internet Gateway & NAT Gateway

- Internet Gateway는 VPC내의 **Public Subnet에 있는 인스턴스가 인터넷과 연결**될 수 있도록 해준다.
- NAT Gateway는 **Private Subnet에 있는 인스턴스가 인터넷과 연결**될 수 있도록 해준다.
## Network ACL & Security Group

- NACL(Network ACL)
	- **Subnet**의 인바운드 아웃바운드 트래픽을 제어하는 방화벽이다.
	- ALLOW와 DENY 규칙이 존재한다.
	- Subnet level에 정의가 가능하다.
	- 규칙은 **오직 IP 주소**만 포함된다.
- Security Group
	- **ENI 또는 EC2 인스턴스**의 인바운드 아웃바운드 트래픽을 제어하는 방화벽이다.
	- 오직 ALLOW 규칙만 존재한다.
	- 규칙은 **IP 주소와 또 다른 Security Group**을 포함할 수 있다.
## VPC Flow Logs

- **IP 트래픽의 흐름을 모니터링** 해준다.
- 서브넷과 인터넷, 서브넷과 서브넷, 인터넷과 서브넷 사이의 연결을 모니터링하고 트러블슈팅이 가능하게 해준다.
- 또한, 여러 AWS 관리형 서비스의 네트워크 정보들을 모니터링 가능하게 해준다.
## VPC Peering

- **두 개의 서로다른 VPC를 연결**하는 기술이다.
- 프라이빗하게 AWS 네트워크를 사용하여 연결한다.
	- 마치 서로 같은 네트워크에 있는 것처럼 하게 해준다.
- CIDR이랑은 다른 기술이다.
- VPC Peering 연결은 **전파되지 않는다.**
	- 예를들어 VPC A와 VPC B가 연결되어있고 VPC B와 VPC C가 연결되어 있어도 VPC A와 VPC C는 연결된 상태가 아니라는 것이다.
## VPC Endpoints

- 엔드포인트는 Public Network 대신에 **Private Network를 통해서 AWS 서비스에 연결할 수 있도록 해준다.**
- Private Network를 사용함으로써 **보안성이 강화**되고, **네트워크 지연시간이 줄어든다.**

## Site to Site VPN & Direct Connect

- Site to Site VPN
	- 온프레미스 **VPN을 통해서 AWS에 연결**하는 것을 의미한다.
	- 연결은 자동으로 암호화된다.
	- **Public Internet**을 통해서 이루어진다.
- Direct Connect(DX)
	- 온프레미스와 AWS사이에 **물리적인 연결**을 의미한다.
	- 연결은 프라이빗하고, 암호화되어있고 빠르다.
	- **Private network**를 통해서 이루어진다.
	- 구축하는데 최소한 한달은 걸린다.