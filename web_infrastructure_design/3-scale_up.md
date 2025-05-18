# 3. Scale Up

## Scenario
You are asked to design a web infrastructure that can scale by splitting the components—web server, application server, and database—onto separate servers. Additionally, a load balancer cluster will be used to ensure high availability.

## Infrastructure Diagram (Mermaid.js)

```mermaid
graph TD
    User[User - HTTPS Browser]
    DNS[DNS - www.foobar.com resolves to Load Balancer Cluster IPs]

    subgraph LB_Cluster
        LB1[Load Balancer 1 - HAProxy]
        LB2[Load Balancer 2 - HAProxy]
    end

    Web[Web Server - Nginx]
    App[Application Server]
    DB[Database Server - MySQL]

    User --> DNS
    DNS --> LB1
    DNS --> LB2

    LB1 --> Web
    LB2 --> Web

    Web --> App
    App --> DB

