# Route 53

1. Route 53 is a managed DNS (Domain Name System)
2. DNS is a collection of rules and records which helps clients understand how to reach a server through its domain name
3. Common records
    1. A: hostname to IPv4
    2. AAAA: hostname to IPv6
    3. CName : hostname to hostname
    4. Alias: hostname to AWS resource
4. Route53 can use
    1. Public domain names you own/ buy
        1. Eg: abc.mypublicdomain.com
    2. Private domain names that can be resolved by your instances in your VPCs
        1. Eg: abc.mycompany.internal
5. Advance features
    1. Load balancing (though DNS —> also known as client load balancing)
    2. Health checks (although limited)
    3. Routing policy :
        1. Simple
            1. Use when you need to need to redirect to a single resource
            2. No health checks
            3. If multiple values are returned then a random one is chosen by the client
        2. Failover
            1. If primary instance fails then failover to secondary 
            2. Uses heal
            3. th checks to detect failed instances
        3. Geolocation
            1. Routing based on user location
            2. We specify traffic from X region should go to specific IP
            3. Should create a default policy in case there is no matched loactions
        4. Latency
            1. Redirect user to server that is closer to user (least latency)
            2. Helpful where latency is a priority
            3. Latency is evaluated in terms of user to designated AWS region
            4. User in Russia might be directed to China region (if latency is least)
        5. Weighted
            1. Controls the % of requests that go to specific endpoints
            2. Total % need not be 100
            3. Helpful to test 1% traffic on new app version
            4. Can use health checks
            5. Can be useful to direct traffic into different regions
        6. GeoProximity
            1. Route traffic to your resources based on the geographic location of user and resources
            2. Ability to shift more traffic to resources based on defined bias (More bias —> more user)
            3. To change the size of the geographic region, specify bias values
                1. To expand (1 to 99 ) - more traffic to the resources
                2. To shrink (-1 to 99) - less traffic to the resources
            4. Resources can be 
                1. AWS resources (Specify AWS region)
                2. Non AWS resources (Specify latitude and longitude)
                3. Use Route 53 advance traffic flow to use this feature
            5. You can shift traffic to one region using bias
        7. Multi value
            1. Use when routing traffic to multiple resources
            2. Use when you Want to associate a Route 53 health checks with records (not possible in Simple policy)
            3. Upto 8 health records are returned for each multi value query
            4. Multi value is not a substitute for having an ELB
            5. Changing TTL for any 1 entry will change TTL of all entries for Muti value records
6. DNS records TTL (Time to live)
    1. Way to cache response from DNS so that DNS is not overloaded
    2. DNS record is updated only after TTL expires
    3. High TTL (eg 24hrs)
        1. Less traffic on DNS
        2. Possible chance of outdated records
    4. Low TTL (eg: 60 sec)
        1. More traffic on DNS
        2. Records are outdated for less time
        3. Easy to change records
    5. TTL is mandatory for each DNS record
7. CNAME vs alias
    1. AWS resources expose AWS hostname eg: dhjh.us-east-1.elb.amazonaws.com and you want my app.mydomain.com
    2. CNAME
        1. Points a hostname to any other hostname (abc.mydoomain.com —> any.blabla.com)
        2. Only works for non root domain
            1. Eg something.abc.com will work fine
            2. Abc.com won’t work
    3. ALias
        1. Point hostname to AWS resource (abc.mydomain.com -> xyzzy.amazonaws.com
        2. works for both root and non root domain
            1. Eg something.abc.com will work fine
            2. Abc.com will work fine
        3. Free of charge
        4. Native health check
8. Health checks
    1. Similar to ALB
    2. Have N health checks have passed —> healthy (default 3)
    3. Have N health checks have failed —> unhealthy (default 3)
    4. Default check interval is 30s (can be set to 10s —> lead to higher cost)
    5. About 15 health checkers will check the endpoint health
    6. One request every 2sec on average
    7. Can have HTTP, TCP and HTTPS health checks (No SSL verification)
    8. Possibility of integrating the health check with cloud watch
    9. Health checks can be linked to route53 DNS queries
9. Route 53 as a registrar
    1. A domain name registrar is an organisation that manages the reservation of internet domain names
    2. Domain registrar !=DNS
    3. You can use 3rd party registrar with AWS Route 53
        1. Create a hosted zone in route 53
        2. Update Name server (NS) records on 3rd party website to use route 53 name server
