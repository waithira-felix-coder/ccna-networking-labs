# Networking Practicals with Cisco Packet Tracer

A comprehensive collection of hands-on networking labs covering CCNA topics, organized by difficulty level and subject matter.

##  Repository Structure

```
networking-practicals/
├── 01-Basics/                    # Fundamental networking concepts
├── 02-VLAN-Configuration/        # VLAN and trunking configuration
├── 03-Routing/                   # Dynamic and static routing protocols
├── 04-Wireless/                  # Wireless networking and connectivity
├── 05-Advanced/                  # Advanced topics (SOHO, IoT, Smart Home)
├── docs/                         # Documentation and guides
├── reference/                    # Common commands and templates
└── README.md                     # This file
```

##  Lab Categories

### 01-Basics
**Difficulty:** Beginner  
**Duration:** 30-45 minutes per lab

Foundational concepts including:
- Device connectivity
- Basic device configuration
- Initial network setup
- IP addressing and subnetting

**Labs:**
- `01-device-connectivity.pkt` - Basic device connection and connectivity
- `02-basic-ios-configuration.pkt` - Switch and router basic configuration
- etc.
---

### 02-VLAN-Configuration
**Difficulty:** Intermediate  
**Duration:** 60-90 minutes per lab

Virtual LAN implementation and management:
- VLAN creation and assignment
- Static and dynamic trunking
- Access and trunk ports
- Inter-VLAN routing basics

**Labs:**
- `01-static-vlan-creation.pkt` - Creating and assigning VLANs statically
- `02-static-trunking.pkt` - Configuring 802.1Q static trunking
- `03-dynamic-vlan-implementation.pkt` - Dynamic VLAN implementation
- `04-vlans-and-trunking.pkt` - Comprehensive VLAN and trunking scenario
- etc.
---

### 03-Routing
**Difficulty:** Intermediate to Advanced  
**Duration:** 90-120 minutes per lab

Routing protocol configuration and troubleshooting:
- Inter-VLAN routing
- Dynamic routing protocols (EIGRP)
- Router-on-a-Stick topology
- Default routing and policy-based routing

**Labs:**
- `01-inter-vlan-routing.pkt` - Routing between VLANs using router interface subcommands
- `02-router-on-stick.pkt` - Single physical interface routing multiple VLANs
- `03-eigrp-routing.pkt` - EIGRP dynamic routing configuration
- `04-dhcp-configuration.pkt` - DHCP server configuration and DHCP relay
- etc.
---

### 04-Wireless
**Difficulty:** Intermediate  
**Duration:** 45-75 minutes per lab

Wireless networking concepts and implementation:
- Wireless AP configuration
- Wireless security
- WPA/WPA2 authentication
- Client connectivity and troubleshooting
- Hybrid wired-wireless networks

**Labs:**
- `01-wireless-access-point-basic.pkt` - Basic wireless AP setup and SSID configuration
- `02-wireless-router-configuration.pkt` - Wireless router setup with integrated security
- `03-wireless-to-wired-connectivity.pkt` - Seamless wired-wireless network
- `04-secure-wireless-network.pkt` - WPA2 and authentication configuration
- `05-wireless-network-advanced.pkt` - Advanced wireless topology
- etc.
---

### 05-Advanced
**Difficulty:** Advanced  
**Duration:** 120+ minutes per lab

Real-world complex scenarios and emerging technologies:
- SOHO network implementation
- Smart home automation
- IoT device integration
- Multi-technology convergence

**Labs:**
- `01-soho-network.pkt` - Small Office/Home Office complete network setup
- `02-smart-home-basic.pkt` - Smart home with basic automation
- `03-smart-home-wireless-router.pkt` - Smart home using wireless connectivity
- `04-iot-simulation.pkt` - IoT devices and sensor networks

---

##  How to Use This Repository

### Getting Started
1. **Start with Basics:** Begin with the 01-Basics folder to understand fundamental concepts
2. **Progress Sequentially:** Each lab builds on prerequisites from previous sections
3. **Read Documentation:** Review each lab's README before starting
4. **Follow Objectives:** Each lab has specific learning objectives

### Lab Workflow
```
1. Read the Lab README
   ↓
2. Review Prerequisites
   ↓
3. Open the .pkt file in Cisco Packet Tracer
   ↓
4. Follow Configuration Tasks
   ↓
5. Verify Configuration (use provided commands)
   ↓
6. Troubleshoot Issues
   ↓
7. Test Connectivity & Document Results
```

### Best Practices
- **Save Backups:** Keep original .pkt files and save copies with your changes
- **Document Changes:** Note configuration steps you perform
- **Use Console:** Practice CLI commands even if GUI is available
- **Test Connectivity:** Use ping, tracert, show commands to verify
- **Review Errors:** Read error messages carefully for clues

##  Common Commands Reference

See `reference/` folder for:
- `CISCO-IOS-COMMANDS.md` - Common IOS commands by category
- `VLAN-COMMANDS.md` - VLAN and trunking commands
- `ROUTING-COMMANDS.md` - Routing protocol commands
- `WIRELESS-COMMANDS.md` - Wireless configuration commands

##  Documentation

In the `docs/` folder:
- `GETTING-STARTED.md` - Quick start guide
- `NETWORK-BASICS.md` - OSI model and network concepts
- `VLAN-GUIDE.md` - VLANs and trunking explained
- `ROUTING-GUIDE.md` - Routing concepts and protocols
- `TROUBLESHOOTING.md` - Common issues and solutions

##  Learning Objectives Summary

| Category | Key Skills |
|----------|-----------|
| Basics | Device connectivity, basic config, IP addressing |
| VLAN | VLAN creation, trunking, access ports, tagging |
| Routing | Inter-VLAN routing, dynamic routing (EIGRP), routing protocols |
| Wireless | Wireless AP setup, security, client connectivity |
| Advanced | Complex topologies, IoT, SOHO networks, real-world scenarios |

##  Tips for Success

1. **Understand Before Configuring** - Read the topology and understand the requirements
2. **Use Simulation Mode** - Debug step-by-step to see packet flow
3. **Save Frequently** - Don't lose your work
4. **Re-do Labs** - Repeat labs without referencing previous attempts
5. **Create Variations** - Modify labs to test different configurations
6. **Document Everything** - Keep notes on what you learned

##  File Naming Convention

```
[NUMBER]-[TOPIC]-[SPECIFIC-FOCUS].pkt is the easiest and more effective way 
to organize your work for quick reference

Examples:
01-device-connectivity.pkt
02-basic-ios-configuration.pkt
01-static-vlan-creation.pkt
03-eigrp-routing.pkt
01-wireless-access-point-basic.pkt
```

##   Prerequisites by Category

```
01-Basics
  └─ None

02-VLAN-Configuration
  └─ 01-Basics (all labs)

03-Routing
  ├─ 01-Basics (all labs)
  └─ 02-VLAN-Configuration (all labs)

04-Wireless
  └─ 01-Basics (all labs)

05-Advanced
  ├─ 01-Basics (all labs)
  ├─ 02-VLAN-Configuration (all labs)
  ├─ 03-Routing (all labs)
  └─ 04-Wireless (at least 1 lab)
```



**Last Updated:** August 2026  
**Cisco Packet Tracer Version:** 8.2.2+  
**CCNA Topics Covered:** Chapters 1-10 of CCNA 200-301
