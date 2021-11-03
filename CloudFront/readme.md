# CloudFront

1. It is a Content Delivery Network (CDN)
2. Improves read performance as content is cached at then edge location
3. DDoS protection, integration with Shield and WAF
4. Can expose external https and can talk to internal Https backend services
5. Origins
    1. S3 bucket
        1. For distributing files and caching them at the edge
        2. Enhanced security with cloudFront Origin Access Identity (OAI)
            1. This means Cloudfront can only access the S3 bucket using this OAI user
            2. Buckets will only be accessed through OAI user not anyone else hence better security
            3. Useful for cookies, logging, security
        3. Can be used as an Ingress -> to upload files to S3
    2. Custom origin (HTTP)
        1. Application load balancer -> must be public
        2. EC2 instance —> must be public
        3. S3 website -> you need to first enable the bucket as a static S3 website
        4. Any HTTP backend eg-> on-permise server
        5.  
6. Geo restriction
    1. You can restrict who can access your distribution
        1. WhiteList
        2. BlackList
    2. The county is determined using 3rd party Go IP location system
7. Cloudfront vs S3 CRR
    1. CloudFront
        1. It has a global edge network
        2. Files are cached for a TTL 
        3. Great for static content that must be available everywhere
    2. S3 Cross Region Replication
        1. Must be setup for each region you want replication to happen
        2. Files are updated in near real time
        3. Read only
        4. Great for dynamic content that needs to be available at low-latency in few regions
8. It can take 3 hrs for DNS to be propagated to edge locations
9. Signed URL/Signed cookie
    1. You want to distribute paid shared content to premium user over the world
    2. To use Cloudfront signed url/signed cookie you have to attach. Policy
        1. Includes URLe expiration
        2. Includes IP ranges to access the data from
        3. Trusted signers -> which AWS account can create signed URLs
    3. TTL
        1. Shared content eg movie -> short like a few minutes
        2. Private content that is private tot he user -> can make it last for years
    4. Signed URL
        1. Access to individual files
        2. One signed URL per file
    5. Signed cookies
        1. Access to multiple files
        2. One signed cookie for many files
10. Cloudfront Signed URL VS S3 pre-Signed URL
    1. Cloudfront Signed URL
        1. Allows access to path no matter the origin
        2. Account wide key-pair, only the root account can manage it
        3. Can filter by IP, path, date, expiration
        4. Can leverage caching features
    2. S3 pre-signed URl
        1. Issue a request as the person who pre-signed the URL
        2. Uses the IAM key of the signing IAM principal
        3. Limited lifetime
11. Pricing
    1. Cost of data out per edge location varies
    2. Price Classes
        1. You can reduce the number of edge locations for cost reduction
        2. Price Classes
            1. All -> all regions, best performance
            2. 200 -> most regions but excludes the most expensive regions
            3. 100 -> only the least expensive regions
12. Multiple origin
    1. To route different kind of origins based on content type
    2. Can set origins to be based on path pattern
13. Origin groups
    1. To increase high availability and do failover
    2. One primary and one secondary origin
    3. If primary fails the second one is used
14. Field level encryption
    1. Protect user sensitive information through application stack
    2. Adds an additional layer of security along with HTTPS
    3. Sensitive information encrypted at the edge close to user
    4. Uses asymmetric encryption
    5. Usage
        1. Specify set of files in POST requests that you wan to be encrypted -> max 10 fields
        2. Specify the public key to encrypt them
15. AWS global accelerator
    1. If application is launched in a region is far from the users and you want to reduce latency you use global accelerator
    2. Unicast IP
        1. One server holds one IP address
    3. Anycast IP
        1. All servers hold the same IP address and client is routed to the nearest one
        2. Used in global accelerator
    4. AWS global accelerator leverages AWS edge locations
        1. Connection between application and edge location is private
    5. Works with Elastic IP, EC2 instances, ALB, NLB, public or private IP
    6. Consistent performance
    7. Health checks
    8. Security
        1. Only 2 external IP need to be whitelisted
        2. DDoS protections using AWS Shield
16. Global accelerator Vs Cloudfront
    1. Cloudfront
        1. Improves performance for both cacheable content
        2. Dynamic content such as API acceleration and dynamic site delivery
        3. Content is saved at the edge
    2. Global accelerator 
        1. Improves performance for a wide range of applications over TCP or UDP
        2. Proxying packets at. The edge to applications running in one or more AWS regions
        3. Good fit for non-HTTP use cases such as gaming (UDP), IOT (MQTT) or we need static IP addresses
        4. Has fast failover and health checks 
