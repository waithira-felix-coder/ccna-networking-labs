# 02-VLAN-Configuration: Virtual LAN Implementation

**Difficulty Level:** Intermediate  
**Estimated Time per Lab:** 60-90 minutes  
**Prerequisites:** Complete 01-Basics (all labs)

##  Overview

This section focuses on Virtual LAN (VLAN) concepts and configuration. You'll learn to create multiple VLANs, assign ports, configure trunking protocols, and manage VLAN traffic using 802.1Q tagging.

##  Learning Objectives

After completing this section, you will be able to:

- [ ] Explain VLAN concepts and benefits
- [ ] Create and delete VLANs
- [ ] Assign ports to VLANs (access mode)
- [ ] Configure trunk ports with 802.1Q
- [ ] Understand VLAN tagging
- [ ] Implement static and dynamic trunking
- [ ] Troubleshoot VLAN connectivity issues
- [ ] Configure VLANs across multiple switches

##  Labs in This Section

### Lab 1: Static VLAN Creation
**File:** `01-static-vlan-creation.pkt`  
**Duration:** 60 minutes

#### Description
Create and manage VLANs on switches, assign interfaces to VLANs, and verify VLAN configuration. This lab demonstrates basic VLAN functionality without inter-VLAN routing.

#### Network Topology
```
VLAN 10 (Faculty):        VLAN 20 (Students):       VLAN 30 (Guests):
    PC1                        PC3                       PC5
    |                          |                         |
    0/1 (S1)              0/3 (S1)                   0/5 (S1)
    
    Switch S1 (Local VLANs)
    
    (Same VLAN broadcast domain - Cannot communicate between VLANs)
```

#### Configuration Tasks

1. **Create VLANs on Switch S1**
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
   ```

2. **Assign Ports to VLANs (Access Mode)**
   ```
   S1(config)# interface fastethernet 0/1
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 10
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/3
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 20
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/5
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 30
   S1(config-if)# exit
   S1(config)# exit
   ```

3. **Configure PC IP Addresses**
   - VLAN 10 (Faculty): 10.0.10.0/24 → PC1: 10.0.10.10
   - VLAN 20 (Students): 10.0.20.0/24 → PC3: 10.0.20.10
   - VLAN 30 (Guests): 10.0.30.0/24 → PC5: 10.0.30.10

#### Verification Steps

```
S1# show vlan brief
S1# show interfaces fastethernet 0/1 switchport
S1# show vlan id 10
PC1> ipconfig
PC1> ping 10.0.10.10        (Should work - same VLAN)
PC1> ping 10.0.20.10        (Should fail - different VLAN, no router)
```

#### Key Concepts

- **VLAN Benefits**: Logical segmentation, broadcast control, security
- **Access Ports**: Connected to end devices, belong to single VLAN
- **Access VLAN**: VLAN assigned to an access port
- **Native VLAN**: Untagged traffic (typically VLAN 1)

---

### Lab 2: Static Trunking Configuration
**File:** `02-static-trunking.pkt`  
**Duration:** 75 minutes

#### Description
Configure trunk links between switches using 802.1Q tagging. Learn how to allow multiple VLANs across a single physical link.

#### Network Topology
```
       VLAN 10 (Faculty)
          PC1
          |
       (Access Port 0/1)
          |
    ┌─────────────────────────────────┐
    │       Switch S1                  │
    │  (Trunk Port 0/24 ←→ 0/24)       │
    └─────────────────────────────────┘
             |Trunk Link|
    ┌─────────────────────────────────┐
    │       Switch S2                  │
    │  (Access Ports 0/2, 0/3)         │
    └─────────────────────────────────┘
          |              |
        (0/2)          (0/3)
          |              |
        PC3            PC5
       VLAN 20        VLAN 30
     (Students)      (Guests)
```

#### Configuration Tasks

1. **Create VLANs on Both Switches**
   
   *On S1 and S2:*
   ```
   (config)# vlan 10
   (config-vlan)# name Faculty
   (config-vlan)# exit
   (config)# vlan 20
   (config-vlan)# name Students
   (config-vlan)# exit
   (config)# vlan 30
   (config-vlan)# name Guests
   (config-vlan)# exit
   ```

2. **Configure Trunk Ports (S1 to S2)**
   
   *On S1:*
   ```
   S1(config)# interface fastethernet 0/24
   S1(config-if)# switchport mode trunk
   S1(config-if)# switchport trunk allowed vlan 1,10,20,30
   S1(config-if)# switchport trunk native vlan 1
   S1(config-if)# exit
   S1(config)# exit
   ```
   
   *On S2:*
   ```
   S2(config)# interface fastethernet 0/24
   S2(config-if)# switchport mode trunk
   S2(config-if)# switchport trunk allowed vlan 1,10,20,30
   S2(config-if)# switchport trunk native vlan 1
   S2(config-if)# exit
   S2(config)# exit
   ```

3. **Assign Access Ports**
   
   *On S1:*
   ```
   S1(config)# interface fastethernet 0/1
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 10
   S1(config-if)# exit
   ```
   
   *On S2:*
   ```
   S2(config)# interface fastethernet 0/2
   S2(config-if)# switchport mode access
   S2(config-if)# switchport access vlan 20
   S2(config-if)# exit
   
   S2(config)# interface fastethernet 0/3
   S2(config-if)# switchport mode access
   S2(config-if)# switchport access vlan 30
   S2(config-if)# exit
   ```

4. **PC Configuration**
   - PC1 (VLAN 10): 10.0.10.10 / 255.255.255.0
   - PC3 (VLAN 20): 10.0.20.10 / 255.255.255.0
   - PC5 (VLAN 30): 10.0.30.10 / 255.255.255.0

#### Verification Steps

```
S1# show trunk
S2# show trunk
S1# show vlan brief
S2# show vlan brief
S1# show interfaces fastethernet 0/24 trunk
PC1> ping 10.0.10.10     (Should work - same VLAN)
PC1> ping 10.0.20.10     (Should fail - different VLAN, no router)
```

#### Key Concepts

- **802.1Q Tagging**: Frame tagging for VLAN identification
- **Trunk Ports**: Carry traffic for multiple VLANs
- **Native VLAN**: Untagged traffic on trunk
- **Allowed VLANs**: Control which VLANs traverse the trunk
- **DTP (Dynamic Trunking Protocol)**: Automatic trunk negotiation (not used in this lab)

#### Troubleshooting Tips

- Check trunk status: `show trunk`
- Verify allowed VLANs: `show interfaces trunk`
- Check switchport mode: `show interfaces switchport`
- Use simulation mode to see tagged frames

---

### Lab 3: Dynamic VLAN Implementation
**File:** `03-dynamic-vlan-implementation.pkt`  
**Duration:** 90 minutes

#### Description
Implement VLANs using dynamic trunking protocol (DTP) and automatic trunk negotiation where applicable.

#### Configuration Tasks

(This lab demonstrates dynamic trunking and more complex VLAN scenarios)

See individual lab README for detailed steps.

---

### Lab 4: VLANs and Trunking (Comprehensive)
**File:** `04-vlans-and-trunking.pkt`  
**Duration:** 90 minutes

#### Description
Complex topology with multiple switches, VLANs, and trunks. Combines all VLAN concepts in a realistic scenario.

See individual lab README for detailed topology and steps.

---

##  Skills to Practice

- Creating and managing VLANs
- Understanding VLAN numbering schemes (10, 20, 30, etc.)
- Access port configuration
- Trunk port configuration
- 802.1Q tagging concepts
- Allowed VLAN lists
- Native VLAN configuration
- VLAN verification and troubleshooting

##  Key Commands Reference

```
vlan <number>                      # Create VLAN
name <vlan-name>                   # Name VLAN
no vlan <number>                   # Delete VLAN
switchport mode access             # Set port to access mode
switchport access vlan <number>    # Assign port to VLAN
switchport mode trunk              # Set port to trunk mode
switchport trunk allowed vlan <list> # Allow VLANs on trunk
switchport trunk native vlan <number> # Set native VLAN
show vlan brief                    # View all VLANs
show vlan id <number>              # View specific VLAN
show interfaces switchport         # View port VLAN assignments
show trunk                         # View trunk information
show interfaces trunk              # Detailed trunk info
```

##  VLAN ID Allocation Reference

```
VLAN 1:     Reserved (Default/Native) - Never assign to user traffic
VLAN 2-1001: Standard Range (Stored in VLAN.dat)
VLAN 1002-1005: Reserved for Token Ring and FDDI
VLAN 1006-4094: Extended Range (Stored in running-config)
```

### Common VLAN Numbering Scheme
```
VLAN 10: Faculty/Staff
VLAN 20: Students
VLAN 30: Guests/Temporary
VLAN 40: Servers/Infrastructure
VLAN 50: Management
VLAN 99: Native/Administrative (on trunks)
```

##  Next Steps

Once you've completed all labs in this section:
1. Combine multiple switches into complex topologies
2. Practice trunk configuration without reference materials
3. Troubleshoot VLAN connectivity issues
4. Move on to **03-Routing** for inter-VLAN communication

## ✅ Completion Checklist

- [ ] Created multiple VLANs with meaningful names
- [ ] Assigned ports to VLANs correctly
- [ ] Configured trunk links between switches
- [ ] Verified VLAN configuration with show commands
- [ ] Tested connectivity within and between VLANs
- [ ] Understood 802.1Q tagging
- [ ] Documented VLAN allocation scheme
- [ ] Troubleshot connectivity issues

---

**Last Updated:** August 2026
