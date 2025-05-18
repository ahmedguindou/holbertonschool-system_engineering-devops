# 3. Scale Up

## Scenario

You are asked to design a web infrastructure that can scale by splitting the components—web server, application server, and database—onto separate servers. Additionally, a load balancer cluster will be used to ensure high availability.

---



## Infrastructure Diagram (Mermaid.js)

```mermaid
graph TD
    User[User - HTTPS Browser]
    DNS[DNS - wwwfoobar.com resolves to Load Balancer Cluster IPs]

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
# Explanation

## Why add these elements?

### Load Balancer Cluster (2 HAProxy instances):
- Adding two load balancers configured as a cluster provides **high availability** and **fault tolerance**.
- If one load balancer goes down, the other can continue handling incoming user traffic without interruption.

### Dedicated Web Server:
- The web server handles all HTTP/S requests, serving static files like images, CSS, and JavaScript.
- It acts as the first point of contact after the load balancer and improves performance by offloading static content delivery.

### Dedicated Application Server:
- The application server runs the business logic and processes dynamic requests.
- Separating it from the web server allows easier scaling and maintenance.

### Dedicated Database Server:
- The database server stores and manages persistent data.
- Isolating it improves data security, backup management, and database performance.

---

## Application Server vs Web Server

### Web Server (e.g., Nginx):
- Responsible for handling client HTTP requests, serving static content, and proxying dynamic requests to the application server.

### Application Server:
- Responsible for executing the application’s backend logic, processing data, handling authentication, and interacting with the database.

By splitting the web server and application server, each can be scaled or maintained independently, improving flexibility and reliability.

---

## Summary

This infrastructure design enables better scalability, high availability through load balancer clustering, and improved performance by isolating web, application, and database components on separate servers.
