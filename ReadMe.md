# TCP/IP Layer Simulation Using GNS3

## 📌 Project Overview

This project demonstrates a **TCP/IP Layer Simulation** using **GNS3**, Cisco routers, switches, and virtual PCs. The goal is to simulate inter-branch communication, configure routing using **OSPF**, and analyze **ICMP and TCP traffic** using **Wireshark**.

Two branches (Branch A and Branch B) are connected via a WAN link. Each branch hosts multiple departments on separate subnets, showcasing how the TCP/IP model operates across real network layers.

---

## 🧠 TCP/IP Model Mapping

| TCP/IP Layer   | Role in This Simulation               |
| -------------- | ------------------------------------- |
| Application    | Ping (ICMP), Telnet (TCP)             |
| Transport      | TCP handshake (Telnet), ICMP messages |
| Internet       | IP addressing, OSPF routing           |
| Network Access | Ethernet, switches, physical links    |

---

## 🏗️ Network Topology

### Branch A

* **HR Department**: 192.168.1.0/24
* **IT Department**: 192.168.2.0/24
* **Router A WAN IP**: 10.0.0.1/30

### Branch B

* **Sales Department**: 192.168.3.0/24
* **Customer Support (CS)**: 192.168.4.0/24
* **Router B WAN IP**: 10.0.0.2/30

---

## ⚙️ Router Configuration

### 🔹 Router A Configuration

```bash
conf t
interface GigabitEthernet2/0
 description Gateway for IT Dept
 ip address 192.168.2.1 255.255.255.0
 no shutdown
 exit

interface FastEthernet0/0
 description WAN Link to Branch B
 ip address 10.0.0.1 255.255.255.252
 no shutdown
end
write memory
```

### 🔹 Router B Configuration

```bash
conf t
interface GigabitEthernet1/0
 description Gateway for Sales Dept
 ip address 192.168.3.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet2/0
 description Gateway for CS Dept
 ip address 192.168.4.1 255.255.255.0
 no shutdown
 exit

interface FastEthernet0/0
 description WAN Link to Branch A
 ip address 10.0.0.2 255.255.255.252
 no shutdown
end
write memory
```

---

## 🖥️ PC IP Configuration

Each PC is configured using a **single-line IP command**:

| PC Name   | IP Address  | Gateway     |
| --------- | ----------- | ----------- |
| HR_PC1    | 192.168.1.2 | 192.168.1.1 |
| HR_PC2    | 192.168.1.3 | 192.168.1.1 |
| IT_PC1    | 192.168.2.2 | 192.168.2.1 |
| IT_PC2    | 192.168.2.3 | 192.168.2.1 |
| Sales_PC1 | 192.168.3.2 | 192.168.3.1 |
| Sales_PC2 | 192.168.3.3 | 192.168.3.1 |
| CS_PC1    | 192.168.4.2 | 192.168.4.1 |
| CS_PC2    | 192.168.4.3 | 192.168.4.1 |

---

## ❌ Initial Connectivity Issue

* Internal communication **within each branch worked correctly**.
* Inter-branch communication failed.
* Example issue: **Sales PC did not respond to ping from Branch A**.

### 🔍 Root Cause

* No dynamic routing protocol enabled.
* Routers were unaware of remote networks.

---

## ✅ Solution: Enable OSPF Routing

### 🔹 Router A OSPF Configuration

```bash
conf t
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
end
write memory
```

### 🔹 Router B OSPF Configuration

```bash
conf t
router ospf 1
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.3 area 0
end
write memory
```

### 🎉 Result

* OSPF neighbors formed successfully.
* Full inter-branch connectivity established.
* HR PCs can ping Sales and CS PCs.

---

## 🔄 Verification Commands

* **Ping Test**:

```bash
HR_PC1> ping 192.168.3.2
```

* **Telnet Test**:

```bash
BranchA# telnet 10.0.0.2
```

---

## 📊 Task 2: TCP/IP Traffic Simulation (Wireshark)

### 🔹 ICMP Traffic Capture

1. Start capture on link between **Branch A Router & Switch_HR**.
2. Ping Sales PC from HR_PC1.
3. Observe **ICMP Echo Request/Reply** packets.

### 🔹 TCP Traffic Capture (Telnet)

1. Start capture on **WAN link**.
2. From Router A, run:

```bash
telnet 10.0.0.2
```

3. Observe **TCP 3-way handshake (SYN, SYN-ACK, ACK)**.

---

## 🛠️ Tools & Technologies

* GNS3
* Cisco IOS Routers
* VPCS
* Wireshark
* OSPF Routing Protocol

---

## 🎯 Learning Outcomes

* Practical understanding of TCP/IP layers
* Hands-on routing with OSPF
* Real-time packet analysis using Wireshark
* Troubleshooting inter-network connectivity

---

## 🔮 Future Enhancements

* Add VLANs for departments
* Implement ACLs for security
* Use EIGRP/BGP for comparison
* Add real servers (HTTP/FTP)

---

## 👤 Author

**Ayaz Hussain**
Computer Science | Networking & AI Enthusiast

---

✅ *This project provides a complete end-to-end simulation of TCP/IP networking concepts with routing and traffic analysis.*
