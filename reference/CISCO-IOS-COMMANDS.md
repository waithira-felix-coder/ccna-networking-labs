# Cisco IOS Commands Reference

## Device Access & Management

```
enable                          # Enter privileged mode
configure terminal              # Enter configuration mode
exit                           # Exit current mode (move up one level)
end                            # Exit to privileged mode
no shutdown                    # Enable interface/device
shutdown                       # Disable interface/device
hostname <name>                # Set device hostname
enable password <password>     # Set enable password (unencrypted)
enable secret <password>       # Set enable password (encrypted)
```

## Viewing Configuration & Status

```
show version                   # Display device info and software version
show running-config            # View active configuration in RAM
show startup-config            # View saved configuration in NVRAM
show interfaces brief          # Quick view of all interfaces
show interfaces <interface>    # Detailed interface information
show ip route                  # Display routing table
show ip interfaces brief       # IP info for all interfaces
show mac address-table         # View MAC address table (switches)
show vlan brief                # View VLAN information
show vlan id <number>          # View specific VLAN details
show ip protocols              # Show routing protocols running
show processes                 # CPU and memory usage
show clock                     # Display system time
show logs                      # Display system logs
```

## Configuration File Management

```
copy running-config startup-config    # Save running config to NVRAM
copy startup-config running-config    # Load startup config to RAM
erase startup-config                  # Clear saved configuration
reload                                # Reboot device
```

## Interface Configuration

```
interface <interface>          # Enter interface mode
ip address <IP> <mask>         # Assign IP address
ip address dhcp                # Get IP via DHCP
description <text>            # Add interface description
speed <speed>                  # Set interface speed
duplex <mode>                  # Set duplex (auto/full/half)
mtu <size>                     # Set frame size
no ip address                  # Remove IP address
```

## IP Address Configuration

```
Router(config)# interface <interface>
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# ip default-gateway 192.168.1.254    # Gateway (switches only)
```

## Subinterface Configuration (VLAN Routing)

```
interface <physical>.vlan-id   # Create subinterface
encapsulation dot1Q <vlan-id>  # Enable 802.1Q tagging
ip address <IP> <mask>         # Assign IP to subinterface
no shutdown                    # Enable subinterface
```

## VLAN Configuration

```
vlan <number>                  # Create VLAN
name <vlan-name>               # Name VLAN
no vlan <number>               # Delete VLAN
interface <interface>          # Enter interface mode
switchport mode access         # Set to access mode
switchport access vlan <num>   # Assign to VLAN
switchport mode trunk          # Set to trunk mode
switchport trunk allowed vlan <list>  # Allow VLANs on trunk
switchport trunk native vlan <num>    # Set native VLAN
show vlan brief                # View VLAN summary
show vlan id <number>          # View specific VLAN
show interfaces switchport     # Port VLAN assignments
show trunk                     # Trunk information
show interfaces fastethernet <port> switchport  # Single port details
```

## Switch Configuration

```
switchport mode access              # Access port (connected to device)
switchport mode trunk               # Trunk port (connects to switch/router)
switchport mode dynamic auto        # DTP auto negotiation
switchport mode dynamic desirable   # DTP desirable negotiation
spanning-tree portfast             # Enable PortFast
spanning-tree bpduguard enable     # Enable BPDU Guard
storm-control broadcast level 100  # Broadcast storm control
```

## Routing Configuration

```
router eigrp <AS-number>           # Enable EIGRP
network <network> <wildcard>       # Advertise network
no auto-summary                    # Disable auto-summarization
show ip route                      # View routing table
show ip eigrp neighbors            # Show EIGRP neighbors
show ip eigrp topology             # Show EIGRP topology
ip route <network> <mask> <gateway> <distance>  # Static route
ip default-gateway <gateway>       # Default gateway (switches)
show ip eigrp interfaces           # EIGRP enabled interfaces
```

## DHCP Configuration

```
ip dhcp pool <name>                # Create DHCP pool
network <network> <mask>           # Define pool network
default-router <gateway>           # Set default gateway
dns-server <IP>                    # Set DNS server
lease <days> <hours> <min>         # Set lease duration
ip dhcp excluded-address <IP> <IP> # Exclude IP range
show ip dhcp binding               # View active leases
show ip dhcp pool                  # View DHCP pool info
clear ip dhcp binding *            # Clear all bindings
```

## Static Routing

```
ip route <dest-net> <mask> <next-hop-ip> <metric>
ip route 192.168.1.0 255.255.255.0 10.0.0.254

# Default route
ip route 0.0.0.0 0.0.0.0 <gateway-ip>
```

## NAT Configuration

```
ip nat inside source list <ACL> interface <interface> overload
access-list 1 permit <network> <wildcard>
interface <LAN-interface>
ip nat inside
interface <WAN-interface>
ip nat outside
show ip nat translations
```

## Access Control Lists (ACL)

```
access-list <number> permit|deny <protocol> <source> <dest>
access-list 1 permit 192.168.0.0 0.0.0.255
access-list 1 deny any
ip access-list standard <name>
ip access-list extended <name>
show access-lists
no access-list <number>
```

## Wireless Configuration

```
interface dot11radio 0             # Configure wireless interface
ssid <network-name>                # Define SSID
authentication open               # No authentication
authentication key-management wpa # WPA authentication
wpa-psk ascii 0 <password>        # WPA Pre-Shared Key
channel <number>                  # Set WiFi channel
power local <percentage>          # Set transmit power
show dot11 associations           # View wireless clients
show interfaces dot11radio        # Wireless interface info
```

## Troubleshooting & Diagnostics

```
ping <IP-address>                 # Test connectivity
tracert <IP-address>              # Trace route path
telnet <IP-address>               # Telnet to device
ssh -l <username> <IP>            # SSH to device
show arp                          # View ARP table
show mac address-table            # View MAC table
show ip arp                       # IP to MAC mapping
debug ip routing                  # Debug routing (use carefully!)
undebug all                       # Stop all debugging
show processes cpu                # CPU usage
show memory                       # Memory usage
terminal length 0                 # Disable paging (show full output)
```

## Interface Descriptions

```
interface <interface>
description Link to R2 - 192.168.1.0
description VLAN 10 (Faculty) Access Port
description Trunk to S2 - allowed VLAN 10,20,30
```

## Password Encryption

```
enable secret <password>           # Encrypted enable password
service password-encryption       # Enable password encryption
username <name> secret <password>  # Local auth user
line console 0
password <password>
login
exit
line vty 0 4
password <password>
login
```

## Useful Configuration Tips

```
# Add banner
banner motd # Welcome to Router #

# Set clock
clock set 14:30:00 1 Jan 2025

# Set system logging
logging <syslog-server>
logging on

# Set terminal formatting
terminal length 0
terminal width 80

# Save configuration with annotation
# configuration last updated: Aug 17, 2026
# network topology: see network diagram
```

## Quick Configuration Workflow

```
1. enable                          # Enter privileged mode
2. configure terminal              # Enter config mode
3. hostname <name>                 # Set name
4. [Configure interfaces]
5. [Configure protocols]
6. exit                           # Exit config mode
7. copy running-config startup-config  # Save
8. show running-config            # Verify
```

---

**Last Updated:** August 2026
**Cisco IOS Version:** 15.x+
