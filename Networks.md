### Domain Name System (DNS)
- work station sends request to ISP, ISP asks other servers for name system IP, then returns the IP address for the web address name so that the work station can connect to the site.
    - NS record (name server) - Specifies the DNS server for domain/subdomain
    - MX record (mail exchange) - specifies the mail servers for accepting messages
    - A Record (address) - Points a name to an IP address.
    - CNAME (canonical) - Points a name to another name or CNAME or to an A record.
- CloudFlare and Route53 provide managed DNS services.
- Methods of routing traffic
    - Weighted Round Robin
        - pairs incoming request to a specific machine by cycling through a list of servers capable of handling the request.
        - Prevents traffic from going to servers under maintenance
        - balance between varying cluster sizes
        - A/B testing
    - Latency-based
        - serving based on region that provides the lowest latency
        - steps to use:
            - create latency records for resources in multiple regions
            - when Route 53 receives a DNS query for your domain/sub, then selects a latency record for that region. A response is given in the form of a value from the selected record i.e. IP address or a web server
            
    - Geolocation-based