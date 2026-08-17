# 03-Routing: Routing Protocols and Inter-VLAN Communication

**Difficulty Level:** Intermediate to Advanced  
**Estimated Time per Lab:** 90-120 minutes  
**Prerequisites:** Complete 01-Basics and 02-VLAN-Configuration (all labs)

## 📖 Overview

This section covers routing concepts, inter-VLAN routing, static routing, and dynamic routing protocols (EIGRP). You'll learn how to enable communication between different networks and VLANs.

## 🎯 Learning Objectives

After completing this section, you will be able to:

- [ ] Explain routing concepts and routing table
- [ ] Configure inter-VLAN routing using router-on-a-stick
- [ ] Configure inter-VLAN routing using router subinterfaces
- [ ] Implement static default routes
- [ ] Configure and troubleshoot EIGRP
- [ ] Understand dynamic routing protocol operation
- [ ] View and interpret routing tables
- [ ] Troubleshoot routing issues using trace and debug commands

## 📋 Labs in This Section

### Lab 1: Inter-VLAN Routing
**File:** `01-inter-vlan-routing.pkt`  
**Duration:** 90 minutes

#### Description
Configure a router to enable communication between VLANs. Use subinterfaces on a single router interface to connect to multiple VLANs on a switch.

#### Network Topology
```
                VLAN 10 (Faculty)    VLAN 20 (Students)    VLAN 30 (Guests)
                    PC1                  PC3                   PC5
                    |                    |                     |
                (Fa0/1)             (Fa0/2)                (Fa0/3)
                    |                    |                     |
        ┌───────────────────────────────────────────────────────┐
        │            Switch S1 (Access Layer)                    │
        │  Port 0/24 → Trunk to Router                          │
        └───────────────────────────────────────────────────────┘
                         |
                      (Gi0/0)
                      Trunk Link
                         |
        ┌───────────────────────────────────────────────────────┐
        │       Router R1 (with subinterfaces)                   │
        │  Gi0/0.10 - VLAN 10 (10.0.10.1)                       │
        │  Gi0/0.20 - VLAN 20 (10.0.20.1)                       │
        │  Gi0/0.30 - VLAN 30 (10.0.30.1)                       │
        └───────────────────────────────────────────────────────┘
```

#### Configuration Tasks

1. **Configure Switch S1**
   ```
   S1# configure terminal
   S1(config)# vlan 10
   S1(config-vlan)# name Faculty
   S1(config-vlan)# exit
   S1(config)# vlan 20
   S1(config-vlan)# name Students
   S1(config-vlan)# exit
   S1(config)# vlan 30
   S1(config-vlan)# name Guests
   S1(config-vlan)# exit
   
   S1(config)# interface fastethernet 0/1
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 10
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/2
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 20
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/3
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 30
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/24
   S1(config-if)# switchport mode trunk
   S1(config-if)# switchport trunk allowed vlan 1,10,20,30
   S1(config-if)# exit
   S1(config)# exit
   ```

2. **Configure Router R1 (Router-on-a-Stick)**
   ```
   R1# configure terminal
   R1(config)# hostname R1
   R1(config)# interface gigabitethernet 0/0
   R1(config-if)# no shutdown
   R1(config-if)# exit
   
   # Configure VLAN 10 Subinterface
   R1(config)# interface gigabitethernet 0/0.10
   R1(config-subif)# encapsulation dot1Q 10
   R1(config-subif)# ip address 10.0.10.1 255.255.255.0
   R1(config-subif)# exit
   
   # Configure VLAN 20 Subinterface
   R1(config)# interface gigabitethernet 0/0.20
   R1(config-subif)# encapsulation dot1Q 20
   R1(config-subif)# ip address 10.0.20.1 255.255.255.0
   R1(config-subif)# exit
   
   # Configure VLAN 30 Subinterface
   R1(config)# interface gigabitethernet 0/0.30
   R1(config-subif)# encapsulation dot1Q 30
   R1(config-subif)# ip address 10.0.30.1 255.255.255.0
   R1(config-subif)# exit
   
   R1(config)# exit
   R1# copy running-config startup-config
   ```

3. **Configure PC IP Addresses**
   - PC1 (VLAN 10): 10.0.10.10 / 255.255.255.0 / Gateway 10.0.10.1
   - PC3 (VLAN 20): 10.0.20.10 / 255.255.255.0 / Gateway 10.0.20.1
   - PC5 (VLAN 30): 10.0.30.10 / 255.255.255.0 / Gateway 10.0.30.1

#### Verification Steps

```
R1# show ip route
R1# show interfaces gigabitethernet 0/0
R1# show interfaces gigabitethernet 0/0.10
R1# show interfaces brief
S1# show vlan brief
S1# show interfaces fastethernet 0/24 trunk

PC1> ipconfig
PC1> ping 10.0.10.1         (Ping gateway - should work)
PC1> ping 10.0.20.10        (Ping PC in different VLAN - should work now!)
PC1> ping 10.0.30.10        (Ping PC3 in another VLAN - should work)
PC3> tracert 10.0.30.10
```

#### Key Concepts

- **Router-on-a-Stick**: Single router interface connecting multiple VLANs
- **Subinterfaces**: Logical interfaces created on physical interface
- **802.1Q Encapsulation**: Encapsulation method for VLAN tagging
- **Inter-VLAN Routing**: Routing between different VLANs
- **Gateway Address**: Default gateway for each VLAN subnet

#### Troubleshooting Tips

- Check subinterface IP: `show interfaces subif`
- Verify encapsulation: `show interfaces Gi0/0.10 encapsulation`
- Check routing table: `show ip route`
- Use debug: `debug ip routing` (advanced)

---

### Lab 2: Router-on-Stick Configuration
**File:** `02-router-on-stick.pkt`  
**Duration:** 90 minutes

#### Description
Advanced router-on-a-stick topology with multiple switches and more complex VLAN scenarios.

(See individual lab README for detailed configuration)

---

### Lab 3: EIGRP Routing
**File:** `03-eigrp-routing.pkt`  
**Duration:** 120 minutes

#### Description
Configure EIGRP (Enhanced Interior Gateway Routing Protocol) for dynamic routing. Learn how routers automatically discover networks and routes.

#### Network Topology
```
        ┌─────────────────┐
        │   Router R1     │ - 172.16.0.0/24 (Fa0/0)
        │ (HQ Router)     │ - 192.168.1.0/24 (Fa0/1 to R2)
        └────────┬────────┘
                 |
                (Fa0/1)
                 |
            192.168.1.0/24
                 |
                (Fa0/0)
        ┌────────┴────────┐
        │   Router R2     │ - 10.0.0.0/24 (Fa0/0)
        │ (Branch 1)      │ - 192.168.1.0/24 (Fa0/1 to R1)
        │                 │ - 192.168.2.0/24 (Fa1/0 to R3)
        └────────┬────────┘
                 |
            192.168.2.0/24
                 |
                (Fa1/0)
        ┌────────┴────────┐
        │   Router R3     │ - 10.1.0.0/24 (Fa0/0)
        │ (Branch 2)      │ - 192.168.2.0/24 (Fa0/1 to R2)
        └─────────────────┘
```

#### Configuration Tasks

1. **Configure Router R1 (HQ)**
   ```
   R1# configure terminal
   R1(config)# hostname R1
   R1(config)# router eigrp 100
   R1(config-router)# network 172.16.0.0 0.0.0.255
   R1(config-router)# network 192.168.1.0 0.0.0.255
   R1(config-router)# no auto-summary
   R1(config-router)# exit
   
   R1(config)# interface fastethernet 0/0
   R1(config-if)# ip address 172.16.0.1 255.255.255.0
   R1(config-if)# no shutdown
   R1(config-if)# exit
   
   R1(config)# interface fastethernet 0/1
   R1(config-if)# ip address 192.168.1.1 255.255.255.0
   R1(config-if)# no shutdown
   R1(config-if)# exit
   R1(config)# exit
   ```

2. **Configure Router R2 (Branch 1)**
   ```
   R2# configure terminal
   R2(config)# hostname R2
   R2(config)# router eigrp 100
   R2(config-router)# network 10.0.0.0 0.0.0.255
   R2(config-router)# network 192.168.1.0 0.0.0.255
   R2(config-router)# network 192.168.2.0 0.0.0.255
   R2(config-router)# no auto-summary
   R2(config-router)# exit
   
   R2(config)# interface fastethernet 0/0
   R2(config-if)# ip address 10.0.0.1 255.255.255.0
   R2(config-if)# no shutdown
   R2(config-if)# exit
   
   R2(config)# interface fastethernet 0/1
   R2(config-if)# ip address 192.168.1.2 255.255.255.0
   R2(config-if)# no shutdown
   R2(config-if)# exit
   
   R2(config)# interface fastethernet 1/0
   R2(config-if)# ip address 192.168.2.1 255.255.255.0
   R2(config-if)# no shutdown
   R2(config-if)# exit
   R2(config)# exit
   ```

3. **Configure Router R3 (Branch 2)**
   ```
   R3# configure terminal
   R3(config)# hostname R3
   R3(config)# router eigrp 100
   R3(config-router)# network 10.1.0.0 0.0.0.255
   R3(config-router)# network 192.168.2.0 0.0.0.255
   R3(config-router)# no auto-summary
   R3(config-router)# exit
   
   R3(config)# interface fastethernet 0/0
   R3(config-if)# ip address 10.1.0.1 255.255.255.0
   R3(config-if)# no shutdown
   R3(config-if)# exit
   
   R3(config)# interface fastethernet 0/1
   R3(config-if)# ip address 192.168.2.2 255.255.255.0
   R3(config-if)# no shutdown
   R3(config-if)# exit
   R3(config)# exit
   ```

4. **Configure PCs**
   - PC1 (connected to R1): 172.16.0.10 / 255.255.255.0 / Gateway 172.16.0.1
   - PC2 (connected to R2): 10.0.0.10 / 255.255.255.0 / Gateway 10.0.0.1
   - PC3 (connected to R3): 10.1.0.10 / 255.255.255.0 / Gateway 10.1.0.1

#### Verification Steps

```
R1# show ip route
R1# show ip eigrp neighbors
R1# show ip eigrp topology
R1# show interfaces brief

R2# show ip route
R2# show ip eigrp neighbors
PC1> ping 172.16.0.10
PC1> ping 10.0.0.10        (Should work - EIGRP discovered route)
PC1> ping 10.1.0.10        (Should work)
PC1> tracert 10.1.0.10     (Trace route through R2)
```

#### Key Concepts

- **Autonomous System (AS)**: EIGRP routing domain
- **Network Statements**: Define which interfaces participate in EIGRP
- **Wildcard Mask**: Inverse subnet mask (0 = must match, 255 = ignore)
- **Auto-summary**: Automatic summarization at classful boundaries (disable for subnets)
- **Neighbor Adjacency**: EIGRP routers forming adjacency for route exchange
- **Metric Calculation**: Based on bandwidth, delay, and other factors

#### EIGRP Commands Reference

```
router eigrp <AS-number>          # Enable EIGRP
network <ip> <wildcard-mask>      # Advertise networks
no auto-summary                   # Disable auto-summarization
show ip route                     # View routing table
show ip eigrp neighbors           # View EIGRP neighbors
show ip eigrp topology            # View EIGRP topology
show interfaces brief             # View interface status
show ip eigrp interfaces          # EIGRP-enabled interfaces
debug eigrp packets               # Debug EIGRP (advanced)
```

---

### Lab 4: DHCP Configuration
**File:** `04-dhcp-configuration.pkt`  
**Duration:** 90 minutes

#### Description
Configure a router as a DHCP server and use DHCP relay agents for distributed DHCP services.

(See individual lab README for configuration details)

---

## 🔧 Skills to Practice

- Understanding routing fundamentals
- Configuring static routes
- Inter-VLAN routing with subinterfaces
- EIGRP configuration and troubleshooting
- Reading and interpreting routing tables
- Network design with multiple VLANs and routers
- DHCP concepts and configuration

## 📚 Key Routing Commands Reference

### General Routing Commands
```
show ip route                     # Display routing table
show ip route eigrp               # Show only EIGRP routes
show interfaces brief             # Show interface status and IP
ping <destination>                # Test connectivity
tracert <destination>             # Trace route path
show ip protocols                 # Show active routing protocols
```

### EIGRP Specific Commands
```
router eigrp <AS>                 # Enter EIGRP configuration
network <network> <wildcard>      # Advertise network
no auto-summary                   # Disable auto-summarization
show ip eigrp neighbors           # Show EIGRP neighbors
show ip eigrp topology            # Show EIGRP topology table
show ip eigrp statistics          # Show EIGRP statistics
clear ip eigrp neighbors          # Reset neighbor relationships
```

### Subinterface Commands
```
interface <physical>.<VLAN>       # Create subinterface
encapsulation dot1Q <VLAN>        # Enable 802.1Q tagging
ip address <IP> <mask>            # Assign IP to subinterface
```

## 🔍 Routing Concepts Summary

| Concept | Description |
|---------|-------------|
| Routing Table | Database of known routes and metrics |
| Default Route | Route used when no specific route matches (0.0.0.0/0) |
| Administrative Distance | Trust metric for routing protocol (lower = more trusted) |
| Metric | Cost value used to determine best route |
| Neighbor | Adjacent router running same routing protocol |
| Route Advertisement | Sending routing updates to neighbors |
| Route Convergence | Time for all routers to have consistent view of network |

## 🚀 Next Steps

Once you've completed all labs in this section:
1. Create multi-router, multi-VLAN topologies
2. Configure routing without reference materials
3. Troubleshoot complex routing issues
4. Combine with wireless networking
5. Move on to **04-Wireless** or **05-Advanced**

## ✅ Completion Checklist

- [ ] Configured inter-VLAN routing successfully
- [ ] Created and configured subinterfaces
- [ ] Understood router-on-a-stick topology
- [ ] Configured EIGRP on multiple routers
- [ ] Viewed and interpreted routing tables
- [ ] Tested connectivity between different networks
- [ ] Verified neighbor adjacency
- [ ] Used tracert to trace route paths
- [ ] Understood wildcard masks and network statements
- [ ] Troubleshot routing issues

---

**Last Updated:** August 2026
