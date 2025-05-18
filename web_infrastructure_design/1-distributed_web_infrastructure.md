# 1. Distributed Web Infrastructure

## 🌐 Scenario

A user wants to access `www.foobar.com`. This time, the infrastructure is distributed across **three servers** to improve performance and availability.

---

## 🗺️ Infrastructure Diagram (Mermaid.js)

```mermaid
graph TD
    User[User - Browser]
    DNS[DNS - www.foobar.com -> Load Balancer IP]
    LB[Load Balancer - HAProxy]

    subgraph Web Server Cluster
        S1[Web Server 1 - Nginx + App + DB (Primary)]
        S2[Web Server 2 - Nginx + App + DB (Replica)]
    end

    DBP[Primary DB]
    DBR[Replica DB]

    User --> DNS --> LB
    LB --> S1
    LB --> S2
    S1 --> DBP
    S2 --> DBR
```

---

## 🧱 Components Breakdown

### ➕ Additional Elements and Why

| Component     | Reason                                                                 |
|---------------|------------------------------------------------------------------------|
| **Load Balancer (HAProxy)** | Distributes traffic between servers to avoid overloading one machine. |
| **Two Web Servers**         | Improves redundancy and allows parallel handling of user requests. |
| **Primary-Replica DB**      | Supports data redundancy and load sharing in reads. |

---

## 🔄 Load Balancer

- **Tool**: HAProxy
- **Algorithm**: *Round Robin*
  - Distributes requests in order: first to server 1, then server 2, then repeats.
- **Setup**: *Active-Active*
  - Both servers are actively handling requests at the same time.
  - **Active-Active vs. Active-Passive**:
    - **Active-Active**: All servers handle traffic simultaneously.
    - **Active-Passive**: One server handles traffic, the other waits for failover.

---

## 🗃️ Primary-Replica Database (Master-Slave)

### How it works:
- The **Primary** database (on Server 1) handles all **writes** and **updates**.
- The **Replica** database (on Server 2) receives **read-only copies** of the data.
- Replication is usually **asynchronous**, meaning there's a delay between writing to Primary and the Replica getting updated.

### Application-side Difference:
- Writes (insert/update/delete) are always directed to the **Primary** DB.
- Reads can be served by either the Primary or Replica to improve performance.

---

## ⚠️ Infrastructure Issues

| Category        | Issues                                                                 |
|----------------|-------------------------------------------------------------------------|
| **SPOF**        | Load Balancer is a **Single Point of Failure** if not replicated.      |
| **Security**    | No **firewall** or **HTTPS**, meaning data and servers are vulnerable. |
| **Monitoring**  | No monitoring or alerting system to detect failures or spikes.         |

---

