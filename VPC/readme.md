# VPC

1. CIDR
    1. Classless Inter Domain Routing
    2. Used for Security group and networking
    3. They have define an IP address range
        1. X.X.X.X/32 -> 32 means 1 IP
        2. X.X.X.X/0 -> 0 means all IP
    4. Has two component
        1. Base IP -> X.X.X.X
            1. Represents an IP contained in the range
        2. The subnet mask (/N)
            1. Defines how many bits can change in the IP
            2. Allows part of the underlying IP to get additional next values from the base IP
            3. /32 -> allows for 1 IP - 2^0=1
            4. /31 -> allows for 2 IP - 2^1=2
            5. /30 -> allows for 4 IP - 2^2=4
            6. /24 -> allows for 256 IP - 2^8=256
2. Default VPC
    1. All new account has a default VPC
    2. New instances are launched in default VPC if no subnet is specified
    3. Default VPC can have internet connectivity and all instances have public IP
    4. We get a public and private DNS name
    5. Subnets belonging to 1 VPC has non overlapping CIDR
3. Own VPC
    1. You can have multiple VPC in a region
    2. Each VPC can have 5 CIDR 
        1. Min Size is /28
        2. Max Size is /16
    3. It is private hence only private IPs are allowed
    4. Your VPC CIDR should not overlap with other networks
4. Subnet
    1. Tied to specific AZ
    2. You can have public and private subnet
    3. AWS reservers 5 IP (first 4 and last 1) in each subnet
        1. 1st -> for Network address
        2. 2nd -> Reserved by AWS for VPC router
        3. 3rd -> Reserved by AWS for mapping Amazon provided DNS
        4. 4th -> Reserved by AWS for some future use
        5. Last one -> Network broadcast address. AWS does not support broadcast in VPC hence address is reserved
    4. Exam Tip -> remember to deduct 5 IP to get available IP
5. Internet gateway & route table
    1. Help VPC instances connect with internet
    2. It is Highly available (HA) and redundant
    3. Must be created separately from VPC
    4. 1 VPC can only be attached to one IGW and vice versa
        1. One Internet gateway per VPC
    5. Internet gateway is also a NAT for the instance that have public IPv4
    6. Internet Gateway on their own don’t allow internet access
        1. Route tables are needed/changed
        2. Route table should point to internet gateway
        3. Instances will get routed through route table
        4. You can have public and private route table
6. Network Address Translation (NAT)
    1. NAT instances — outdated
        1. Allows instances in private subnet to connect to internet
        2. Must be launch in public subnet
        3. Must disable EC2 flag: Source/Destination check
        4. Must have elastic IP attached to it
        5. Route table must be configured to route traffic from private subnet to NAT instance
        6. You need to create EC2 instance for NAT
    2. NAT gateways - New
        1. AWS manages NAT, higher bandwidth, better availability , no administration 
        2. Pay by hour for usage
        3. NAT is created in a specific AZ and uses an Elastic IP
        4. It cannot be used by an instance in that subnet, only be used by other subnets
        5. Requires an IGW (Internet gateway)
        6. 5 Gbps of bandwidth
        7. No security group to manage/required
7. DNS Resolution in VPC
    1. EnableDnsSupport 
        1. DNS Resolution setting, default value is true
        2. Helps decide if DNS resolution is supported for the VPC
        3. If true, queries the AWS DNS server at 169.254.169.253
    2. EnableDnsHostname
        1. DNS hostname setting, default is false for new VPC and true for default VPC
        2. Works only If enableDnsSupport is true
        3. If EnableDnsHostname is true, it will assign public hostname to Ec2 instances if it has a public IP
    3. If you use custom DNS domain names in private zone in Route 53, you must set. Both these attributes to true
8. Network ACL Vs Security Group
    1. Incoming request
        1. NACL sit outside of subnet, When incoming traffic reaches NACL it is evaluated by the Inbound rules and then the traffic is permitted inside subnet
        2. Once the traffic is inside subnet, it is evaluated against the inbound rules of Security group before it enters EC2 instance
        3. Security group requests are stateful that means traffic coming in will go out even if there is an explicit deny.
        4. Once the traffic reaches subnet level it is evaluated by NACL inbound rules because NACL is stateless
    2. Outgoing request
        1. Traffic going out of EC2 is first evaluated by Security group and then traffic reaches subnet level and it is evaluated by NACL
        2. While traffic comes back it is evaluated by NACL as it is stateless and won’t be evaluated by security group as it is stateful
    3. Network ACL
        1. NACL arre like firewall which control traffic from and to subnet
        2. Default NACL allows everything inbound and outbound
        3. One NACL per subnet, new subnets are assigned default NACL
        4. Rules
            1. Rules have a  number (1—32766) and higher precedence with lower number
                1. Eg: rule #100 has priority over rule #200
            2. Last rule is an “*” and denies request in. Case of non rule match
            3. AWS recommends adding rules by increment of 100
        5. Newly created NACL will end everything
        6. NACL are great way of blocking specific IP at subent level
9. VPC peering
    1. Connect two VPC privately using AWS network
    2. CIDR ranges should not overlap
    3. Help 2 VPC behave as if they were in same network
    4. VPC peering is not transitive (a-> b, b-> C is not a->c)
    5. Must update route tables in each VPC subnets to ensure instances can communicate
    6. Works inter-region, cross-account
    7. You can reference a Security group of a peered VPC (works cross account)
10. VPC endpoint
    1. Helps access AWS global services (Services outside VPC) privately instead of public network
        1. DynamoDB
        2. Cloudfront
        3. S3
    2. Scales horizontally and redundant
    3. They remove need of IGW, NAT to access AWS services
    4. Interface
        1. Provisions an ENI (private address) as an entry point (Must attack security group) -> most AWS services
    5. Gateway
        1. Provisions a target and must be used in route table
            1. S3
            2. DynamoDB
    6. Incase of issues
        1. Check DNS setting resolution in your VPC
        2. Check route table
11. VPC flow logs
    1. VPC flow log
        1. Subnet flow log
            1. Elastic Network interface flow log
    2. Flow logs data can go to S3 or cloudwatch
    3. Apart from customer managed services it can also capture logs from AWS managed services
    4. Log syntax
        1. Srcaddr, dtsaddr help identify problematic IP
        2. Srcport, dstport help identify problematic ports
        3. Action: success or failure of the request due to Security group/NACL
        4. Can be used for analytics or malicious behaviour
        5. Query VPC flow logs using Athena on S3 or cloudwatch log insights
12. Bastion Hosts
    1. We can use Bastion host to SSH into private instances
    2. The bastion is in the public subnet which is then connected to all other private subnet
    3. Bastion host security group must be tightened
    4. Make sure the bastion host only has port 22 traffic from the IP you need not from the security group of your instances
    5. Public instance sitting in public instance allowing SSH on port 22 is bastion host
13. Site to Site VPN
    1. Connect corporate datacenter to AWS network
    2. You need to create customer gateway on corporate datacenter
    3. On AWS side you need to create Virtual private gateway
    4. Then you can link both using site to site VPN
14. Virtual Private gateway
    1. VPN concentrator on the AWS side of the VPN connection
    2. VGW is created and attached to the VPC which you want to create the site to site VPN connection
    3. Possibility to customise the ASN
15. Customer gateway
    1. Software application or physical device or customer side of the VPN
    2. IP address
        1. Use static, internet-routable IP address for you customer gateway device
        2. If behind a CGQ behind NAT (with NAT-T), use the public IP address of the Nat
16. Direct Connect (DX)
    1. Dedicated private connection from remote network to your VPC
    2. Dedicated connection must be setup between your DC and AWS connect locations
    3. You need to setup a virtual private gateway on your VPC
    4. You can access public resources (S3) and private (EC2) on same connection
    5. Supports both IPv4 and IPv6
    6. Connection types
        1. Dedicated connections
            1. 1Gbps and 10 Gbps capacity
            2. Physical ethernet dedicated to a customer
        2. Hoster connections
            1. 50 Mbps, 500 Mbps to 19 Gbps
            2. Connection requests are made via AWS direct connect. Partners
            3. Capacity can be added or removed on demand
    7. Takes 1 month to establish
    8. Data in transit is not encrypted but it is private
    9. AWS direct connect + VPN provides an IPsec-encrypted private connection
    10. Resiliency
        1. High resiliency for critical workloads
            1. One connection at multiple locations
        2. Maximum resiliency for critical workload
            1. Achieved using separate connections terminating on separate devices in more than one location
17. Direct Connect Gateway
    1. To setup direct connect to one or more VPC in many different regions of same account
18. Egress only internet gateway
    1. Egress means outgoing
    2. Works only for IPv6
    3. Similar to NAT but NAT is for IPv4
    4. IPv6 are all public addresses
    5. Egress only internet gateway gives our IPv6 instances access the internet but they won’t be directly reachable by the internet
    6. After creating it you need to edit route table
19. AWS privatelink
    1. Solves problem of Exposing services in your VPC to other VPC without making it public and not using VPC peering (opens the whole network)
    2. AWS privatelink is also called VPC endpoint services
    3. Most secure & scalable way toe expose a service to many VPC
    4. Requires a load balancer (Service VPC) and ENI (Customer VPC)
    5. If the NLB is in Multi AZ and ENI is Multi AZ then solution is fault tolerant
20. AWS ClassicLink - EC2 Classic
    1. It is deprecated
    2. EC2 classic -> instances run in a single network shared with other customers
    3. Amazon VPC -> Your instances run logically isolated to your AWS account
    4. ClassicLink 
        1. Allows you to link EC2 classic instances to a VPC in your account
        2. Must associate a security group
        3. Enables communication using private IPv4 addresses
        4. Removes the need to make use of public IPv4 addresses or Elastic IP addresses
21. VPN CloudHub
    1. Provides secure communication between sites, if you have multiple VPN connections
    2. Low cost hub-and-spoke model for primary or secondary network connectivity between locations
    3. Its VPN so it goes over public network
22. Transit Gateway
    1. For having transitive peering between 1000s of VPC and on-premise, Site-to-site VPN, hub-and-spoke (star) connection
    2. Regional resource, can work cross region
    3. Share cross-account using Resource Access Manager (RAM)
    4. You can peer Transit Gateways across regions
    5. Route tables - limit which VPC can talk with other VPC
    6. Works with Direct connect gateway, VPN connections
    7. Supports IP multicast which is not supported by any other AWS service
    8. To increase bandwidth of site-to-site VPN using ECMP
        1. Equal Cost Multi path routing
        2. Routing strategy to allow to forward a packet over multiple best path
        3. Use case - Create multiple Site-to-site VPN connections to increase the bandwidth go your connection to AWS
        4. Share direct connect connection between multiple accounts

