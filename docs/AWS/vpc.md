# VPC (Virtual Private Cloud)
> [Site-to-Site VPN](#Site_to_Site_VPN)  
> [Transit Gateway](#Transit_Gateway)  
> [Cloud Hub](#Cloud_Hub)  
> [VPC Peering](VPC_Peering)  
> [Direct Connect](Direct_Connect)  


## Site_to_Site_VPN
- Virtual Private Gateway (VGW)
    - VPN concentrator on the AWS side of the VPN connection
- Customer Gateway (CGW)
    - Software application or physical device on customer side of the VPN connection
- You can attach **one virtual private gateway to a VPC at a time**. To connect the same Site-to-Site VPN connection to multiple VPCs, we recommend that you explore using a *transit gateway* instead. 

## Transit_Gateway
- *Connect multiple VPC with multiple On Premise Network*
- Use VPN and Direct Connect to establish connection
- Supports IP Multi-cast

## Cloud_Hub
- *One VPC connect with multiple On Premise Network*
- Use VPN to establish connection

## VPC_Peering
- A VPC peering connection is a networking connection between *two VPCs* that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses.

## Direct_Connect
- If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway