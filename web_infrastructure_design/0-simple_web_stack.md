# 0. Simple Web Stack

## 💡 Scenario

A user opens a browser and wants to access `www.foobar.com`.

---

### 🧭 Request Flow

1. The browser requests `www.foobar.com`.
2. DNS resolves `www.foobar.com` to the IP address `8.8.8.8`.
3. The request is sent to the server at `8.8.8.8`.
4. The **Nginx web server** receives the request.
5. Nginx forwards it to the **application server**.
6. The app server processes the logic and queries the **MySQL database**.
7. The result is sent back to the user through the same path.

---

## 🖥️ Infrastructure Diagram (Mermaid.js)

```mermaid
graph TD
    User[User - Browser]
    DNS[DNS: www.foobar.com -> 8.8.8.8]
    Server[Server - IP: 8.8.8.8]
    Nginx[Nginx - Web Server]
    App[Application Server]
    DB[MySQL Database]

    User --> DNS
    DNS --> Server
    Server --> Nginx
    Nginx --> App
    App --> DB
    DB --> App
    App --> Nginx
    Nginx --> User
```


---

## 🔍 Components and Roles

| Component         | Role                                                                 |
|------------------|----------------------------------------------------------------------|
| `foobar.com`      | Domain name that maps to the server's IP address.                   |
| `www`             | Subdomain represented by an A record in DNS pointing to 8.8.8.8     |
| `Server`          | Hosts the full web stack (Nginx, application, and database)         |
| `Nginx`           | Web server that handles HTTP requests and forwards them             |
| `App Server`      | Runs backend code (PHP, Python, etc.) and handles business logic    |
| `MySQL`           | Stores persistent data (e.g., users, posts, products)               |
| `User ↔ Server`   | Communication happens over HTTP/HTTPS (TCP/IP)                      |

---

## 📘 Key Questions

### What is a server?
A physical or virtual computer that provides services to clients. Here, it hosts all components needed to serve the website.

### What is the role of the domain name?
It provides a human-readable way to access the server via DNS, instead of using IP addresses.

### What type of DNS record is `www` in `www.foobar.com`?
An **A record** that maps the `www` subdomain to the server IP address `8.8.8.8`.

### What is the role of the web server?
**Nginx** receives incoming HTTP requests, serves static files, and forwards dynamic requests to the application server.

### What is the role of the application server?
Processes the business logic and interacts with the database to generate dynamic content.

### What is the role of the database?
Stores structured data like user info, orders, blog posts, etc., and responds to SQL queries from the app server.

### How does the server communicate with the user's computer?
Via the **HTTP/HTTPS protocols** over **TCP/IP**.

---

## ⚠️ Infrastructure Limitations

| Issue                       | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| SPOF (Single Point of Failure) | One server handles everything. If it fails, the entire site goes down.      |
| Downtime During Maintenance | Updating code or restarting services can interrupt availability.            |
| Scalability Limitations    | Cannot handle high traffic without replication or load balancing.           |

---
