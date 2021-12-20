# VPC (Virtual Private Cloud)
> [Site-to-Site VPN](#Site_to_Site_VPN)  
> [Transit Gateway](#Transit_Gateway)  
> [Cloud Hub](#Cloud_Hub)  
> [VPC Peering](#VPC_Peering)  
> [Direct Connect](#Direct_Connect)  
> [VPC Endpoint](#VPC_Endpoint)  
> [NAT Gateway](#NAT_Gateway)   
> [NAT Instance](#NAT_Instance)  


## Site_to_Site_VPN
![sts](https://docs.aws.amazon.com/ko_kr/vpn/latest/s2svpn/images/vpn-how-it-works-vgw.png)
- `Virtual Private Gateway (VGW)`
    - VPN concentrator on the AWS side of the VPN connection
- `Customer Gateway (CGW)`
    - Software application or physical device on customer side of the VPN connection
- You can attach **one virtual private gateway to a VPC at a time**. To connect the same Site-to-Site VPN connection to multiple VPCs, we recommend that you explore using a *transit gateway* instead. 
- IPSec 지원
- 인터넷 망 (보안성 떨어짐)
- 서비스도입에 많은 시간이 필요하지 않음

### How to increase Site-to-Site VPN connections 
- [Using transit gateway](https://aws.amazon.com/blogs/networking-and-content-delivery/scaling-vpn-throughput-using-aws-transit-gateway/)
- **Use a transit gateway** with equal cost multipath routing and add additional VPN tunnels.

### Using redundant Site-to-Site VPN connections to provide failover
![Site to Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/images/Multiple_Gateways_diagram.png)

- To protect against a loss of connectivity in case your customer gateway device becomes unavailable, you can set up a second Site-to-Site VPN connection to your VPC and virtual private gateway by using a second customer gateway device

## Transit_Gateway
![TransitGateway](https://d1.awsstatic.com/product-marketing/transit-gateway/tgw-after.d85d3e2cb67fd2ed1a3be645d443e9f5910409fd.png)
- *Connect multiple VPC with multiple On Premise Network*
- Use VPN and Direct Connect to establish connection
- Supports IP Multi-cast

## Cloud_Hub
![CH](https://docs.aws.amazon.com/ko_kr/vpn/latest/s2svpn/images/AWS_VPN_CloudHub-diagram.png)
- *One VPC connect with multiple On Premise Network*
- Use VPN to establish connection

## VPC_Peering
![vp](https://docs.aws.amazon.com/vpc/latest/peering/images/peering-intro-diagram.png)
- A VPC peering connection is a networking connection between *two VPCs* that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses.

## Direct_Connect
![dc](https://docs.aws.amazon.com/directconnect/latest/UserGuide/images/direct-connect-overview.png)
- Provides a dedicated **private** connection from a remote network to your VPC
- If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway
- Data in transit is **not encrypted** but is private
- **Maximum resilience is achieved** by separate connections terminating on separate devices in more than one location 
    - [#Maximum Resiliency](https://aws.amazon.com/ko/directconnect/resiliency-recommendation/)
- 전용망 (보안성 높음)
- 도입에 비교적 시간이 소요됨

## VPC_Endpoint
- Every AWS service is publicly exposed (public URL)
- VPC Endpoints (powered by AWS PrivateLink) **allows you to connect to AWS services using a private network** instead of using the public Internet

## NAT_Gateway
- **NAT Gateway**는 Newtwork Address Transition 네트워크 주소변환서비스
  - NAT 게이트웨이: 프라이빗 서브넷에 있는 리소스가 인터넷에 액세스할 수 있게 해주는 고가용성 관리형 네트워크 주소 변환 (NAT) 서비스입니다.
  -  Subnet 을 Public 과 Private 으로 구분하여, Public subnet은 Internet Gateway 를 이용하여 외부와 통신이 가능하도록 설정하였다. 하지만, Private Subnet 의 경우는 외부와의 통신이 단절된 환경이다.
    - Private Subnet에 위치한 instance가 다른 AWS 서비스에 연결해야 하는 경우. 
    - 인터넷에서 Private instance 에 접근 불가 조건은 유지하면서 반대로 instance 에서 외부 인터넷으로 연결이 필요한 경우. 
  -  위와 같은 이유로 Private Subnet에 배포된 instance 라도 외부와의 통신이 필요한 경우가 있다. 이런 경우 가장 간단히 해결 할 수 있는 방법은 NAT 서버를 구축하는 것이다.
  - *NAT Gateway : managed by AWS, provides scalable Internet access to private EC2 instances, IPv4 only*


## NAT_Instance
  - *NAT Instances : gives Internet access to EC2 instances in private subnets. Old, must be setup in a public subnet, disable Source / Destination check flag*
  - *You must manage Security Groups*
  - *Use as Bastion Host*
  - *Support port fowarding*