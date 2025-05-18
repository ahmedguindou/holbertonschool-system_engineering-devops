# Distributed Web Infrastructure for www.foobar.com

## Infrastructure Design

```
                                    ┌─────────────────┐
                                    │                 │
                                    │  User/Client    │
                                    │                 │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │                 │
                                    │  DNS Server     │
                                    │  www.foobar.com │
                                    │                 │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │                 │
                                    │   HAproxy       │
                                    │ Load Balancer   │
                                    │                 │
                                    └─────┬─────┬─────┘
                                          │     │
                              ┌───────────┘     └───────────┐
                              │                             │
                     ┌────────▼─────────┐         ┌─────────▼────────┐
                     │                  │         │                  │
                     │    Server 1      │         │    Server 2      │
                     │                  │         │                  │
                     │  ┌────────────┐  │         │  ┌────────────┐  │
                     │  │ Web Server │  │         │  │ Web Server │  │
                     │  │  (Nginx)   │  │         │  │  (Nginx)   │  │
                     │  └──────┬─────┘  │         │  └──────┬─────┘  │
                     │         │        │         │         │        │
                     │  ┌──────▼─────┐  │         │  ┌──────▼─────┐  │
                     │  │Application │  │         │  │Application │  │
                     │  │  Server    │  │         │  │  Server    │  │
                     │  └──────┬─────┘  │         │  └──────┬─────┘  │
                     │         │        │         │         │        │
                     │  ┌──────▼─────┐  │         │  ┌──────▼─────┐  │
                     │  │Application │  │         │  │Application │  │
                     │  │   Files    │  │         │  │   Files    │  │
                     │  └──────┬─────┘  │         │  └──────┬─────┘  │
                     │         │        │         │         │        │
                     │  ┌──────▼─────┐  │         │  ┌──────▼─────┐  │
                     │  │  Database  │  │◄───────►│  │  Database  │  │
                     │  │  (MySQL)   │  │         │  │  (MySQL)   │  │
                     │  │  Primary   │  │         │  │  Replica   │  │
                     │  └────────────┘  │         │  └────────────┘  │
                     │                  │         │                  │
                     └──────────────────┘         └──────────────────┘
```

## Component Explanations

### Load Balancer (HAproxy)
- **Purpose**: Distributes incoming traffic between the two servers to ensure better resource utilization, improved reliability, and availability.
- **Distribution Algorithm**: Round-robin - requests are distributed sequentially to each server in turn. When the end of the server list is reached, the algorithm starts again from the first server. This ensures equal distribution of requests if servers have similar capabilities.
- **Setup Type**: 
  - **Active-Active**: Both servers are actively handling requests simultaneously. This maximizes resource utilization and provides higher throughput.
  - **Active-Passive** (not used here): One server actively handles requests while the other remains on standby, only taking over if the active server fails. This setup provides redundancy but underutilizes resources.

### Servers (2x)
- **Purpose**: Provides redundancy, improves availability, and allows for distribution of the workload.

### Web Server (Nginx)
- **Purpose**: Handles HTTP requests from clients, serves static content, and forwards dynamic content requests to the application server.

### Application Server
- **Purpose**: Executes the application code, processes business logic, and generates dynamic content.

### Application Files (Code Base)
- **Purpose**: Contains the actual code that the application server executes to provide the website's functionality.

### Database (MySQL Primary-Replica Cluster)
- **Purpose**: Stores and manages the website's data with improved reliability and read performance.
- **Primary-Replica Setup**:
  - **Primary Node (Server 1)**: Handles all write operations (INSERT, UPDATE, DELETE) and replicates these changes to the replica node.
  - **Replica Node (Server 2)**: Receives and applies changes from the primary, handles read operations (SELECT) to distribute database load.
  - **Replication Process**: Changes on the primary are recorded in binary logs, which are sent to replica servers, which then apply these changes to their own datasets.
- **Application Interaction Difference**:
  - The application typically writes data to the primary node only.
  - Read operations can be directed to either the primary or replica nodes, often with preference to replicas to reduce load on the primary.

## Issues with this Infrastructure

### Single Points of Failure (SPOF)
1. **Load Balancer**: If the single HAproxy load balancer fails, clients can no longer access the website.
2. **Primary Database**: If the primary MySQL server fails before replication occurs, data can be lost and write operations become impossible.

### Security Issues
1. **No Firewall**: The infrastructure lacks firewalls, making it vulnerable to unauthorized access and attacks.
2. **No HTTPS**: Traffic is not encrypted, allowing sensitive information to be intercepted by attackers.

### Monitoring Concerns
1. **No Monitoring**: The infrastructure lacks monitoring tools, making it difficult to:
   - Detect performance issues before they affect users
   - Identify security breaches
   - Track resource usage and plan for scaling
   - Troubleshoot problems efficiently
   - Ensure services are running properly

### Other Technical Issues
1. **Database Replication Risk**: Asynchronous replication can lead to data inconsistency if the primary fails before changes are replicated.
2. **No Redundancy for Load Balancer**: The load balancer itself is a single point of failure.
3. **Deployment Complexity**: Making updates to the application requires careful coordination across multiple servers.
