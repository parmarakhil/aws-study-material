# AWS Global Infrastructure

1. AWS Regions
    1. AWS regions are global geo locations
    2. Names can be us-east-1, eu-west-3 etc
    3. A region is a cluster of data centres known as Availability Zones
    4. Most AWS services are region scoped
    5. While choosing AWS region
        1. Be close to your customers (proximity)
        2. Check for govt rules (compliance) 
        3. Check if region has required services (Available services)
        4. Check for price of service (pricing)
2. AWS Availability zones (AZ)
    1. Each region has min 2 and max 6 AZ.
    2. Usually each region has 3 AZ
    3. Each AZ is one or more discrete data centres with redundant power, network, connectivity
    4. They are separate from each other
    5. Isolated from disaster
    6. These are connected with high bandwidth, low latency network
3. AWS Points of presence (Edge locations)
    1. Used in AWS cloudfront
    2. Content is delivered to end user with low latency
4. AWS global services
    1. Identity and access management (IAM)
    2. Route 53 (DNS service)
    3. Cloudfront (Content delivery network)
    4. WAF (Web application firewall)

