# VPC (Virtual Private Cloud)
> [Site-to-Site VPN](#Site_to_Site_VPN)  
> [Transit Gateway](#Transit_Gateway)  
> [Cloud Hub](#Cloud_Hub)  
> [VPC Peering](#VPC_Peering)  
> [Direct Connect](#Direct_Connect)  


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