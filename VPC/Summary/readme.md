# VPC Summary

1. CIDR: IP Range 
2. VPC: Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR 
3. Subnets: Tied to an AZ, we define a CIDR 
4. Internet Gateway: at the VPC level, provide IPv4 & IPv6 Internet Access 
5. Route Tables: must be edited to add routes from subnets to the IGW,VPC Peering Connections,VPC Endpoints, etc... 
6. NAT Instances: gives internet access to instances in private subnets. Old, must be setup in a public subnet, disable Source / Destination check flag 
7. NAT Gateway: managed by AWS, provides scalable internet access to private instances, IPv4 only 
8. Private DNS + Route 53: enable DNS Resolution + DNS hostnames (VPC) 
9. NACL: Stateless, subnet rules for inbound and outbound, don’t forget ephemeral ports 
10. Security Groups: Stateful, operate at the EC2 instance level 
11. VPC Peering: Connect two VPC with non overlapping CIDR, non transitive 
12. VPC Endpoints: Provide private access to AWS Services (S3, DynamoDB, CloudFormation, 
13. VPC Flow Logs: Can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Log Insights 
14. Bastion Host: Public instance to SSH into, that has SSH connectivity to instances in private subnets 
15. Site to SiteVPN: setup a Customer Gateway on DC,aVirtual Private Gateway onVPC,and site-to-site VPN over public internet 
16. Direct Connect:setup aVirtual Private Gateway onVPC,and establish a direct private connection to an AWS Direct Connect Location 
17. Direct Connect Gateway: setup a Direct Connect to many VPC in different regions 
18. Internet Gateway Egress: like a NAT Gateway, but for IPv6 
19. Private Link / VPC Endpoint Services:
    1. connect services privately from your service VPC to customers VPC 
    2. Doesn’t need VPC peering, public internet, NAT gateway, route tables 
    3. Must be used with Network Load Balancer & ENI 
20. ClassicLink: connect EC2-Classic instances privately to your VPC
21. VPN CloudHub: hub-and-spoke VPN model to connect your sites
22. Transit Gateway: transitive peering connections forVPC,VPN & DX 
23. IPv4 cannot be disabled for VPC and subnets
24. You can enable IPv6 - they are public IPs
25. If you cannot launch an instance in your subnet
    1. Is because there are no available IPv4 in your subnet
        1. Add new CIDR range
