# Web Infrastructure Design

This README provides a comprehensive overview of web infrastructure concepts and components essential for building robust, scalable, and secure web applications.

## Network Basics

Networks are the foundation of web infrastructure, enabling computers to communicate and share resources. A network consists of:

- **Nodes**: Devices like computers, servers, or routers
- **Links**: Physical or wireless connections between nodes
- **Protocols**: Rules governing communication (TCP/IP, HTTP, etc.)

The **TCP/IP model** has four layers:
1. **Network Access Layer**: Hardware-level connectivity (Ethernet, Wi-Fi)
2. **Internet Layer**: Routing and addressing (IP)
3. **Transport Layer**: End-to-end communication (TCP, UDP)
4. **Application Layer**: Application-specific protocols (HTTP, FTP, SMTP)

**IP addressing** allows devices to find each other:
- IPv4: 32-bit addresses (e.g., 192.168.1.1)
- IPv6: 128-bit addresses (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)

## Server

A server is a computer program or device that provides functionality to other programs or devices (clients). In web infrastructure:

- **Physical servers**: Dedicated hardware optimized for server workloads
- **Virtual servers**: Software-defined servers running on shared hardware
- **Cloud servers**: Virtual servers hosted in cloud platforms (AWS, GCP, Azure)

Servers typically have:
- Higher processing power than consumer devices
- Larger memory capacity
- Redundant components (power supplies, storage)
- Server-grade operating systems (Linux distributions, Windows Server)

Common server roles include:
- Web servers
- Application servers
- Database servers
- File servers
- Mail servers
- DNS servers

## Web Server

A web server's primary function is to serve web content to clients (typically browsers) using HTTP. Key responsibilities:

- Accept and process HTTP requests
- Deliver static content (HTML, CSS, images)
- Handle connection management and security
- Implement HTTP protocol features (caching, compression)

Popular web servers:
- **Nginx**: Known for high performance and low resource usage
- **Apache HTTP Server**: Highly configurable with extensive module system
- **Microsoft IIS**: Integrated with Windows Server

Web servers typically handle:
1. Receiving HTTP requests
2. Parsing request headers
3. Locating requested resources
4. Generating appropriate response headers
5. Sending requested content back to client

## DNS (Domain Name System)

DNS translates human-readable domain names (like example.com) into IP addresses computers can use for routing. The DNS system works hierarchically:

1. **Root servers**: Directory for top-level domains
2. **TLD servers**: Manage domains like .com, .org, .net
3. **Authoritative nameservers**: Store DNS records for specific domains

The DNS resolution process:
1. User types domain name in browser
2. Local DNS resolver checks its cache
3. If not found, query travels up hierarchy until resolved
4. Result is cached for future use

## Load Balancer

Load balancers distribute network traffic across multiple servers to ensure:
- No single server becomes overwhelmed
- High availability (service continues if one server fails)
- Scalability (easily add more servers as needed)

Load balancing algorithms include:
- **Round Robin**: Requests distributed sequentially
- **Least Connection**: Sends to server with fewest active connections
- **IP Hash**: Uses client IP to determine server (ensures session persistence)
- **Weighted methods**: Servers with higher capacity receive more traffic

Load balancers can operate at different network layers:
- **Layer 4 (Transport)**: Based on IP and port information
- **Layer 7 (Application)**: Content-aware, can route based on request content

## Monitoring

Monitoring systems continuously track infrastructure health and performance to:
- Detect issues before they impact users
- Provide insights for capacity planning
- Establish performance baselines
- Alert when metrics exceed thresholds

Key monitoring areas:
- **Server health**: CPU, memory, disk usage
- **Network metrics**: Bandwidth, latency, packet loss
- **Application performance**: Response times, error rates
- **Security events**: Unauthorized access attempts, anomalous behavior

Popular monitoring tools:
- Nagios
- Prometheus
- Grafana
- New Relic
- Datadog

## Database

A database is an organized collection of structured data, typically stored electronically. Databases provide:
- Structured data storage
- Data retrieval via queries
- Data integrity through constraints
- Concurrent access control
- Backup and recovery mechanisms

Database types:
- **Relational (SQL)**: Structured data in tables with relationships (MySQL, PostgreSQL)
- **NoSQL**: Various models for different needs:
  - Document stores (MongoDB)
  - Key-value stores (Redis)
  - Column-family stores (Cassandra)
  - Graph databases (Neo4j)

In web applications, databases store:
- User information
- Content
- Transactions
- Application state
- Analytics data

## Web Server vs. Application Server

While there's some overlap, these serve different purposes:

**Web Server**:
- Primarily handles HTTP requests and responses
- Serves static content efficiently
- Basic security and authentication
- Example workload: Delivering HTML/CSS/JS files

**Application Server**:
- Executes application/business logic
- Processes dynamic content
- Connects to databases and other services
- Manages application state
- Example workload: Processing form submissions, running business calculations

In modern architectures, web servers often forward dynamic requests to application servers, which process them and return results to the web server for delivery to the client.

## DNS Record Types

DNS records store different types of information about a domain:

- **A Record**: Maps domain name to IPv4 address
- **AAAA Record**: Maps domain name to IPv6 address
- **CNAME Record**: Creates an alias pointing to another domain name
- **MX Record**: Specifies mail servers for the domain
- **TXT Record**: Stores text information (often for verification or SPF records)
- **NS Record**: Delegates a subdomain to a set of name servers
- **SOA Record**: Contains administrative information about the zone
- **PTR Record**: Reverse lookup (IP to domain name)
- **SRV Record**: Specifies location of services (hostname and port)

## Single Point of Failure (SPOF)

A SPOF is any component that would cause system failure if it malfunctions. Examples include:
- Single web server
- Single database server
- Single network connection
- Single power supply
- Single load balancer

SPOFs create system vulnerability because:
- No failover capability exists
- Maintenance requires downtime
- Component failure means complete service outage

Best practices to eliminate SPOFs:
- Implement redundancy at all levels
- Use high availability configurations
- Design for graceful degradation

## Avoiding Downtime During Deployment

Strategies to deploy code without service interruption:

**Blue-Green Deployment**:
- Maintain two identical environments (blue and green)
- New code deployed to inactive environment
- Testing performed on new deployment
- Traffic switched from old to new once verified
- Easy rollback by switching back if issues arise

**Rolling Deployment**:
- Update servers in small batches
- Keep enough capacity online during updates
- Gradually shift traffic to updated servers
- More complex but requires less infrastructure

**Canary Deployment**:
- Deploy to small subset of servers/users first
- Monitor performance and errors
- Gradually increase deployment scope if successful
- Minimizes impact of problematic deployments

**Feature Toggles**:
- Deploy code with features disabled
- Enable features gradually or for specific users
- Allows separating deployment from feature release
- Provides fine-grained control over functionality

## High Availability Cluster

High availability clusters ensure continuous operation by eliminating single points of failure:

**Active-Active Configuration**:
- Multiple nodes simultaneously process requests
- Load distributed across all nodes
- Full capacity utilization
- Node failure reduces capacity but service continues
- Example: Multiple web servers behind load balancer

**Active-Passive Configuration**:
- Primary node handles all requests
- Standby nodes remain ready but idle
- Failover mechanism activates standby if primary fails
- Less efficient resource utilization but simpler configuration
- Example: Primary database with standby replica

Key components of HA clusters:
- **Shared storage or data replication**
- **Heartbeat network** for node status monitoring
- **Cluster management software**
- **Load balancing mechanism**

## HTTPS

HTTPS (HTTP Secure) is HTTP over an encrypted TLS/SSL connection, providing:

**Security benefits**:
- **Encryption**: Data transmitted is encrypted, preventing eavesdropping
- **Authentication**: Verifies server identity using certificates
- **Integrity**: Ensures data hasn't been modified in transit

**Implementation components**:
- **SSL/TLS certificates**: Issued by Certificate Authorities
- **Public key infrastructure**: Manages encryption keys
- **HTTPS protocol**: Handles secure handshake and communication

Modern web applications use HTTPS by default because:
- Browsers mark HTTP sites as "Not Secure"
- Search engines favor HTTPS sites
- Many modern web features require secure contexts

## Firewall

A firewall is a security system that monitors and controls network traffic based on predetermined rules:

**Types of firewalls**:
- **Packet filtering**: Examines packets against rules
- **Stateful inspection**: Tracks active connections
- **Application layer**: Analyzes application-specific traffic
- **Next-generation**: Combines multiple security functions

**Common firewall rules control**:
- Source and destination IP addresses
- Ports and protocols
- Application-specific patterns
- Time-based access

In web infrastructure, firewalls typically:
- Allow HTTP/HTTPS traffic (ports 80/443)
- Restrict database access to application servers only
- Block unauthorized SSH/admin access
- Prevent outbound connections to malicious sites

## Web Stack Diagram & Components

Below is a visual representation of a complete web stack with redundancy and security features:

```
                                  INTERNET
                                     |
                                     ▼
                              [DNS SERVERS]
                                     |
                                     ▼
                               [FIREWALL]
                                     |
                                     ▼
                         [LOAD BALANCER (HA Pair)]
                           /               \
                          /                 \
                         ▼                   ▼
                 [WEB SERVER 1]        [WEB SERVER 2]
                 (Nginx/Apache)        (Nginx/Apache)
                         |                   |
                         ▼                   ▼
                 [APP SERVER 1]        [APP SERVER 2]
                         |                   |
                         \                   /
                          \                 /
                           ▼               ▼
                       [DATABASE CLUSTER]
                    (Primary & Replica Setup)
                               |
                               ▼
                       [STORAGE SYSTEM]
                               |
                               ▼
                     [MONITORING SYSTEM]
```

**Component Explanations**:

1. **DNS Servers**: Translate domain names to IP addresses
2. **Firewall**: Filters traffic to protect internal systems
3. **Load Balancers**: Distribute traffic across web servers (configured as HA pair)
4. **Web Servers**: Handle HTTP requests, serve static content
5. **Application Servers**: Process application logic, dynamic content
6. **Database Cluster**: Stores application data with primary-replica setup
7. **Storage System**: Provides persistent storage for files and backups
8. **Monitoring System**: Tracks performance and health across all components

## System Redundancy

Redundancy is implemented at multiple levels:

1. **Server redundancy**: Multiple servers for each layer
   - If one web server fails, others handle the load
   - If one application server fails, others continue processing

2. **Database redundancy**:
   - Primary-replica configuration
   - Automatic failover if primary goes down
   - Regular backups

3. **Network redundancy**:
   - Multiple network paths
   - Redundant load balancers
   - Multiple internet connections

4. **Geographic redundancy** (in larger systems):
   - Multiple data centers
   - Content delivery networks (CDNs)

## Acronyms

**LAMP**:
- **L**inux: Operating system
- **A**pache: Web server
- **M**ySQL: Database system
- **P**HP/Python/Perl: Programming language

**SPOF**:
- **S**ingle **P**oint **O**f **F**ailure
- Any non-redundant component that can cause system failure

**QPS**:
- **Q**ueries **P**er **S**econd
- Metric measuring system capacity and performance
- Used for capacity planning and scaling decisions
