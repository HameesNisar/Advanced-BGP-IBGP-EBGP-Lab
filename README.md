# Multi-AS BGP Lab (GNS3)

This project simulates a **7-Autonomous System Border Gateway Protocol (BGP)** environment using GNS3.  
The lab focuses on mastering core BGP behaviors including **eBGP peering**, **iBGP relationships**,  
**route reflectors**, **loopback peering**, **next-hop-self**, and proper **network advertisement**.

---

## 📌 Topology Diagram

<img width="1861" height="895" alt="topology" src="https://github.com/user-attachments/assets/63813f65-f48e-4463-b598-6ed4f34eb08e" />


---

## 📘 Overview

This advanced BGP design consists of **seven separate Autonomous Systems**, each containing multiple routers.  
Every AS establishes **external BGP (eBGP)** adjacency with its neighboring autonomous systems, and  
**internal BGP (iBGP)** sessions within each AS.  
The purpose of this lab is to demonstrate:

- How BGP forms peerings  
- How eBGP and iBGP behave differently  
- How next-hop behavior influences routing  
- Why route reflection is required  
- How full-mesh iBGP limitations are solved  
- How to properly advertise networks using `network` statements and masks  
- How loopback interfaces simplify BGP peering  
- Route propagation rules between iBGP and eBGP

All neighbor statements, updates, and advertisements were configured manually.

---

## 🧩 Key Concepts Practiced

### **1. eBGP Configuration**
Each AS establishes eBGP sessions with its neighboring ASNs.  
Routers exchange external BGP routes using:

- Manual `neighbor` configuration  
- Directly connected interfaces  
- Loopback-based peering where applicable  
- Correct remote-as values  

### **2. iBGP Configuration**
Inside each AS, routers form iBGP sessions.  
Important behaviors demonstrated:

- iBGP does NOT advertise routes learned from another iBGP peer  
- Full mesh is required unless route reflectors are used  
- Loopback interfaces used to maintain stable peering  
- Update-source loopback used where needed  

### **3. Route Reflectors**
To prevent full-mesh complexity inside larger ASNs:

- One router was selected as **Route Reflector (RR)**  
- Other routers registered as **RR clients**  
- RR relayed iBGP-learned prefixes to clients properly

This demonstrated how RR breaks the full-mesh requirement.

### **4. Next-Hop-Self**
Routers receiving routes via eBGP were configured with:
```
next-hop-self
```
This prevents internal routers from depending on unreachable next-hop addresses and ensures clean route propagation inside the AS.

### **5. BGP Network Advertisement**
All networks were advertised using:

- `network` statements  
- Proper subnet masks  
- Loopback network advertisements  
- Ensuring routes are present in the routing table before advertisement  

### **6. Multi-AS Path Learning**
The network demonstrates:

- How AS_PATH grows  
- How external routes propagate across multiple ASNs  
- How BGP prevents loops using AS_PATH and rule-based filtering  

---

## 🔍 Troubleshooting & Validation Commands

### **Check BGP Neighbor Relationships**
```
show ip bgp summary
show ip bgp neighbors
show ip bgp
show ip bgp neighbors <ip> advertised-routes
show ip bgp neighbors <ip> received-routes
```

## Check Next-Hop Reachability

```
show ip route
traceroute <prefix>
```
## Check Route Reflector Behavior
```
show ip bgp
show ip bgp neighbors | include client
```
## Check Loopback Peering
```
show ip bgp summary
show running-config | include update-source
```

## 🏁 Conclusion
This BGP lab demonstrates complete multi-AS communication using:

- eBGP and iBGP peering
- Route reflectors
- Next-hop-self behavior
- Loopback-based BGP sessions
- Proper network advertisement
- Multi-AS path learning

This project represents real-world service-provider style routing fundamentals, built entirely on Cisco IOS within GNS3.

---
