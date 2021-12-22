# AWS Load Balancer
> [Load Balancer](#LB)  
> [NLB](#NLB)  



## LB
- AWS has 4 kinds of managed Load Balancers
    - Classic Load Balancer (v1 - old generation) – 2009 – CLB
        - HTTP, HTTPS, TCP, SSL (secure TCP)
    - Application Load Balancer (v2 - new generation) – 2016 – ALB
        - HTTP, HTTPS, WebSocket
    - Network Load Balancer (v2 - new generation) – 2017 – NLB
        - TCP, TLS (secure TCP), UDP
    - Gateway Load Balancer – 2020 – GWLB
        - Operates at layer 3 (Network layer) – IP Protocol

## NLB
- Network load balancers (Layer 4) allow to:
    - Forward TCP & UDP traffic to your instances
    - Handle millions of request per seconds
    - Less latency ~100 ms (vs 400 ms for ALB)

### Tips
- Security Groups don't have **deny** rules