# Networking Cost

1. Traffic coming in is free
2. If private IP is used to communicate between same AZ it is free
3. For inter-AZ on private IP is 50 % less costly than on public IP
4. Inter region data transfer is not free
5. Use private IP wherever possible
6. Use same AZ for maximum savings - at the cost of HA
7. Egress traffic - outbound traffic (from AWS to outside)
8. Ingress traffic - inbound traffic - from outside to AWS
9. Try to keep as much internet traffic within AWS to minimize costs
10. If using direct connect, use the co-location in same region
11. S3
    1. Ingress is free
    2. S3 to internet is charged
    3. S3 to cloudfront is free
    4. Cloudfront to internet is charge - less than S3 to internet
