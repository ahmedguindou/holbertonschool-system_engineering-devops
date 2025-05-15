# Simple Web Stack

## Infrastructure Design

```
User's Computer <---> DNS System
       |
       v
+-----------------------------------+
|        Server (IP: 8.8.8.8)       |
|                                   |
| +-------------------------------+ |
| |      Web Server (Nginx)       | |
| +---------------+---------------+ |
|                 |                 |
| +---------------v---------------+ |
| |     Application Server        | |
| +---------------+---------------+ |
|                 |                 |
| +---------------v---------------+ |
| |   Application Files (Code)    | |
| +---------------+---------------+ |
|                 |                 |
| +---------------v---------------+ |
| |      Database (MySQL)         | |
| +-------------------------------+ |
|                                   |
+-----------------------------------+
```

**Request Flow:**
1. User types "www.foobar.com" in browser
2. DNS resolves www.foobar.com to IP address 8.8.8.8
3. Browser sends HTTP request to server at 8.8.8.8
4. Web server (Nginx) receives request
5. Application server processes the request using codebase
6. Database provides necessary data
7. Response flows back through application server and web server
8. User receives the webpage

## Infrastructure Explanation

### User Accessing the Website
1. A user types `www.foobar.com` in their browser
2. Their computer performs a DNS lookup to translate the domain name to an IP address
3. The DNS returns the IP address `8.8.8.8` which is associated with the `www` record of `foobar.com`
4. The user's browser sends an HTTP request to the server at IP `8.8.8.8`
5. The request is received by the web server (Nginx)

### Components of the Infrastructure

#### Server
- A server is a physical or virtual computer dedicated to running specific software and delivering data to other computers (clients)
- Our server has the IP address `8.8.8.8` and hosts all components of our web application

#### Domain Name
- The domain name (`foobar.com`) serves as a human-readable alias for the server's IP address
- It allows users to access the website using an easy-to-remember name instead of numeric IP addresses
- Domain names are managed through the Domain Name System (DNS)

#### DNS Record (www)
- The `www` in `www.foobar.com` is a subdomain
- It is configured as a CNAME (Canonical Name) record in DNS
- This CNAME record points to the domain or to an A record that contains the IP address (`8.8.8.8`)

#### Web Server (Nginx)
- Nginx handles incoming HTTP requests from users' browsers
- It serves static content (HTML, CSS, images) directly to the user
- For dynamic content, it forwards requests to the application server
- It also handles load balancing, security, and SSL termination

#### Application Server
- Executes the application's codebase (business logic)
- Processes dynamic requests forwarded by the web server
- Generates dynamic content by running the application code
- Communicates with the database to retrieve or store data
- Returns processed data to the web server, which then sends it to the user

#### Application Files (Code Base)
- Contains the website's source code/scripts (HTML, CSS, JavaScript, backend code)
- Includes all the logic that makes the website function
- Is executed by the application server to handle user requests

#### Database (MySQL)
- Stores and organizes the website's data in a structured way
- Handles data operations (create, read, update, delete)
- Ensures data persistence and integrity
- Communicates only with the application server, not directly with users

#### Communication Protocol
- The server uses HTTP/HTTPS protocols to communicate with the user's computer
- These protocols define how messages are formatted and transmitted over the web
- TCP/IP is used as the underlying communication protocol

## Issues With This Infrastructure

### Single Point of Failure (SPOF)
- The entire infrastructure depends on a single server
- If the server crashes or experiences hardware failure, the entire website becomes unavailable
- No redundancy or failover mechanisms exist

### Downtime During Maintenance
- When deploying new code or performing updates, the web server (Nginx) needs to be restarted
- During this restart period, the website will be temporarily unavailable
- No parallel system exists to handle requests during maintenance

### Scaling Limitations
- This infrastructure cannot easily handle high traffic volume
- With only one server processing all requests, performance will degrade under heavy load
- No horizontal scaling capability (cannot add more servers easily)
- Vertical scaling (adding more resources to the existing server) has physical limitations
