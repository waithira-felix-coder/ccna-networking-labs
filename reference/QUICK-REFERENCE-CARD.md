# Quick Reference Card - Print This!

## Basic Navigation
```
enable              Enter privileged mode
configure terminal  Enter config mode
exit                Exit one level up
end                 Exit to privileged mode
?                   Get help (type after command)
```

## Device Setup
```
hostname R1                 Set device name
enable password Cisco@123   Set enable password
no shutdown                 Enable interface
interface Gi0/0            Enter interface mode
```

## IP Configuration
```
ip address 192.168.1.1 255.255.255.0    Assign IP
ip address dhcp                         Get IP from DHCP
ip default-gateway 192.168.1.254        Gateway (switch only)
```

## Viewing Configuration
```
show running-config         Current config in RAM
show startup-config         Saved config in NVRAM
show interfaces brief       All interface status
show ip interfaces brief    IP addresses
show ip route              Routing table
```

## Saving Configuration
```
copy running-config startup-config      Save to NVRAM
reload                                  Reboot device
erase startup-config                    Clear saved config
```

## VLAN Configuration
```
vlan 10                 Create VLAN
  name Faculty          Name VLAN
switchport mode access  Set to access port
switchport access vlan 10   Assign port to VLAN
switchport mode trunk   Set to trunk port
switchport trunk allowed vlan 1,10,20
```

## Trunk Configuration
```
interface Gi0/1
  switchport mode trunk
  switchport trunk allowed vlan 10,20,30
  switchport trunk native vlan 99
  exit
```

## Router-on-a-Stick (VLAN Routing)
```
interface Gi0/0.10
  encapsulation dot1Q 10
  ip address 10.0.10.1 255.255.255.0
  no shutdown
```

## EIGRP Routing
```
router eigrp 100                                 Enable EIGRP
network 192.168.1.0 0.0.0.255                   Add network
network 10.0.0.0 0.0.0.255
no auto-summary                                 Disable auto-summary
show ip eigrp neighbors                         View neighbors
show ip route                                   View routing table
```

## DHCP Server
```
ip dhcp pool OFFICE
  network 10.0.0.0 255.255.255.0
  default-router 10.0.0.1
  dns-server 8.8.8.8
  exit
ip dhcp excluded-address 10.0.0.1 10.0.0.50
```

## Static Routing
```
ip route 192.168.1.0 255.255.255.0 10.0.0.2
ip route 0.0.0.0 0.0.0.0 192.168.1.254   Default route
```

## Wireless Configuration
```
interface dot11radio 0
  ssid CiscoLab
  authentication open
  channel 6
  power local 100
```

## Troubleshooting
```
ping 192.168.1.10              Test connectivity
show interfaces status         Quick interface view
show vlan brief                View all VLANs
show trunk                     Trunk status
show ip arp                    ARP table
show mac address-table         MAC addresses
```

## Access Control List (ACL)
```
access-list 1 permit 192.168.1.0 0.0.0.255
access-list 1 deny any
interface Gi0/0
  ip access-group 1 in
```

## Common Subnets (Quick Reference)
```
/24 = 255.255.255.0      (256 IPs per network)
/25 = 255.255.255.128    (128 IPs per network)
/26 = 255.255.255.192    (64 IPs per network)
/27 = 255.255.255.224    (32 IPs per network)
/28 = 255.255.255.240    (16 IPs per network)
```

## EIGRP Wildcard Masks
```
/24 = 0.0.0.255
/16 = 0.0.255.255
/8  = 0.255.255.255
0.0.0.0 = All networks (be careful!)
```

## Quick Checklist - After Configuration
```
☐ All interfaces UP
☐ IP addresses assigned
☐ Saved configuration
☐ Routing table correct
☐ Ping works locally
☐ Ping works to gateway
☐ Ping works remote networks
☐ VLAN connectivity working
☐ EIGRP neighbors formed
```

## Emergency! Common Fixes
```
Interface down?             → no shutdown
Can't ping?                 → check IP, gateway, routes
Forgot password?            → reload device
Configuration lost?         → copy startup-config to running
VLAN not working?          → check vlan creation + access port assignment
Trunk not working?         → verify switchport mode trunk + allowed vlans
EIGRP neighbors missing?    → check network statements, interfaces UP
No internet?               → check default route
```

---

**Print this page! Keep it handy while working through labs.**

**Note:** Replace example IPs/VLANs with your lab values.

---

For full command reference: See `reference/CISCO-IOS-COMMANDS.md`  
For detailed help: See category README files  
For getting started: See `docs/GETTING-STARTED.md`
