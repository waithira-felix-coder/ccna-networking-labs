# 01-Basics: Fundamental Networking Concepts

**Difficulty Level:** Beginner  
**Estimated Time per Lab:** 30-45 minutes  
**Prerequisites:** None

## 📖 Overview

This section covers fundamental networking concepts and basic device configuration. You'll learn how to set up network devices, establish basic connectivity, and perform essential configuration tasks.

## 🎯 Learning Objectives

After completing this section, you will be able to:

- [ ] Connect networking devices using appropriate cable types
- [ ] Access device CLI using console connection
- [ ] Perform basic switch configuration (hostname, enable password)
- [ ] Configure router interfaces with IP addresses
- [ ] Test basic connectivity using ping command
- [ ] Understand physical and logical network topologies
- [ ] Practice proper naming and documentation
- [ ] Use basic Packet Tracer simulation features

## 📋 Labs in This Section

### Lab 1: Device Connectivity
**File:** `01-device-connectivity.pkt`  
**Duration:** 30 minutes

#### Description
Learn the basics of connecting networking devices using Cisco Packet Tracer. You'll connect switches, routers, and end devices using appropriate cable types.

#### Network Topology
```
PC1 ─── Switch 0/1 ─── Router Gi0/0
            │
PC2 ─── Switch 0/2
            │
        (Uplink to Router)
```

#### Configuration Tasks

1. **Device Connection**
   - Connect PC1 to Switch port 1 (use straight-through cable)
   - Connect PC2 to Switch port 2 (use straight-through cable)
   - Connect Switch to Router (use crossover cable)

2. **Basic Router Configuration**
   ```
   Router# configure terminal
   Router(config)# hostname R1
   Router(config)# enable password Cisco123
   Router(config)# interface gigabitethernet 0/0
   Router(config-if)# ip address 192.168.1.1 255.255.255.0
   Router(config-if)# no shutdown
   Router(config-if)# exit
   Router(config)# exit
   Router# copy running-config startup-config
   ```

3. **Basic Switch Configuration**
   ```
   Switch# configure terminal
   Switch(config)# hostname S1
   Switch(config)# enable password Cisco123
   Switch(config)# exit
   Switch# copy running-config startup-config
   ```

4. **PC Configuration**
   - PC1: IP 192.168.1.10 / Mask 255.255.255.0 / Gateway 192.168.1.1
   - PC2: IP 192.168.1.11 / Mask 255.255.255.0 / Gateway 192.168.1.1

#### Verification Steps

Test your configuration:
```
PC1> ping 192.168.1.11          (Ping PC2)
PC1> ping 192.168.1.1           (Ping Router)
Router# ping 192.168.1.10       (Ping from Router)
Router# show interfaces brief
```

#### Common Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| No connectivity | Wrong cable type | Use straight-through for PC-Switch, crossover for Switch-Router |
| Can't access CLI | Device not connected | Ensure console cable from PC to router |
| Interface is down | Port disabled | Use `no shutdown` command on interface |
| Wrong IP address | Configuration error | Use `show running-config` to verify |

#### Key Commands to Practice

```
show running-config       # View current configuration
show startup-config       # View saved configuration
show interfaces brief      # View interface status
ping <IP-Address>         # Test connectivity
interface <name>          # Enter interface configuration
no shutdown               # Enable an interface
exit                      # Move up one configuration level
```

---

### Lab 2: Basic iOS Configuration
**File:** `02-basic-ios-configuration.pkt`  
**Duration:** 45 minutes

#### Description
Practice comprehensive basic configuration of switches and routers including hostnames, passwords, interface configuration, and basic management settings.

#### Network Topology
```
        S1 (Switch)
        / |  \
      PC1 PC2 Router
          |
          S2 (Switch)
          |
         PC3
```

#### Configuration Tasks

1. **Configure S1 (Access Switch)**
   ```
   S1# configure terminal
   S1(config)# hostname S1
   S1(config)# enable secret Cisco@123
   S1(config)# interface vlan 1
   S1(config-if)# ip address 192.168.1.5 255.255.255.0
   S1(config-if)# no shutdown
   S1(config-if)# exit
   S1(config)# ip default-gateway 192.168.1.1
   S1(config)# banner motd # Welcome to S1 #
   S1(config)# exit
   S1# copy running-config startup-config
   ```

2. **Configure S2 (Access Switch)**
   ```
   S2# configure terminal
   S2(config)# hostname S2
   S2(config)# enable secret Cisco@123
   S2(config)# interface vlan 1
   S2(config-if)# ip address 192.168.1.6 255.255.255.0
   S2(config-if)# no shutdown
   S2(config-if)# exit
   S2(config)# ip default-gateway 192.168.1.1
   S2(config)# exit
   S2# copy running-config startup-config
   ```

3. **Configure Router**
   ```
   Router# configure terminal
   Router(config)# hostname R1
   Router(config)# enable secret Cisco@123
   Router(config)# interface gigabitethernet 0/0
   Router(config-if)# ip address 192.168.1.1 255.255.255.0
   Router(config-if)# no shutdown
   Router(config-if)# exit
   Router(config)# exit
   Router# copy running-config startup-config
   ```

4. **Configure PCs**
   - PC1: 192.168.1.10 / 255.255.255.0 / Gateway 192.168.1.1 / DNS 192.168.1.1
   - PC2: 192.168.1.11 / 255.255.255.0 / Gateway 192.168.1.1 / DNS 192.168.1.1
   - PC3: 192.168.1.12 / 255.255.255.0 / Gateway 192.168.1.1 / DNS 192.168.1.1

#### Verification Steps

```
R1# show running-config
R1# show interfaces brief
S1# ping 192.168.1.1
S2# ping 192.168.1.1
PC1> ipconfig
PC1> ping 192.168.1.12
```

#### Challenges

Once you complete the basic configuration:
1. Add different enable passwords for different devices
2. Create a backup of running-config by copying to NVRAM
3. Practice login authentication (optional on Router)
4. Add descriptions to interfaces

---

## 🔧 Skills to Practice

- Device naming conventions
- Password security best practices
- Interface assignment and numbering
- Configuration file management (running vs startup)
- Basic CLI navigation and syntax
- Device backup and recovery

## 📚 Recommended Reading

- Cisco IOS Command Hierarchy
- Switch vs Router differences
- IP Addressing fundamentals
- Network cable types and standards

## 🚀 Next Steps

Once you've completed all labs in this section:
1. Redo the labs without referencing instructions
2. Modify topologies to add more devices
3. Create your own simple network topology
4. Move on to **02-VLAN-Configuration**

## ✅ Completion Checklist

- [ ] Connected all devices successfully
- [ ] Configured hostnames on all devices
- [ ] Set enable passwords
- [ ] Configured IP addresses on interfaces
- [ ] Tested connectivity between all devices
- [ ] Saved running configuration
- [ ] Documented the network topology
- [ ] Understood VLAN 1 (default management VLAN)

---

**Last Updated:** August 2026
