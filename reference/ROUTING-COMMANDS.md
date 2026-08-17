# Routing Commands Reference

## EIGRP Configuration

### Enable EIGRP

```
router eigrp <AS-number>
network <network> <wildcard-mask>
no auto-summary
exit
```

**Example:**
```
R1(config)# router eigrp 100
R1(config-router)# network 192.168.1.0 0.0.0.255
R1(config-router)# network 10.0.0.0 0.0.0.255
R1(config-router)# no auto-summary
R1(config-router)# exit
```

### EIGRP Networks

```
network 192.168.1.0 0.0.0.255     # Network 192.168.1.0/24
network 10.0.0.0 0.0.0.255        # Network 10.0.0.0/24
network 0.0.0.0 255.255.255.255   # All networks (use with caution)
```

### Wildcard Mask vs Subnet Mask

```
Subnet Mask    Wildcard Mask
255.255.255.0  0.0.0.255
255.255.0.0    0.0.255.255
255.0.0.0      0.255.255.255
255.255.255.252 0.0.0.3

Rule: Wildcard = 255.255.255.255 - Subnet Mask
```

### Auto-Summary

```
no auto-summary                    # Disable auto-summarization (required for subnets)
auto-summary                       # Enable auto-summarization (default, not recommended)
```

## Static Routing

### Static Route Configuration

```
ip route <destination-network> <subnet-mask> <next-hop-IP> [distance]
ip route <destination-network> <subnet-mask> <exit-interface> [distance]
```

**Examples:**
```
R1(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.2
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2 5
R1(config)# ip route 10.0.0.0 255.255.0.0 serial 0/0
```

### Default Route

```
ip route 0.0.0.0 0.0.0.0 <next-hop-IP>
ip route 0.0.0.0 0.0.0.0 <exit-interface>
```

**Example:**
```
R1(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.254  # Gateway of last resort
```

### Floating Static Route

```
ip route 192.168.1.0 255.255.255.0 10.0.0.2 200   # AD = 200 (backup route)
ip route 192.168.1.0 255.255.255.0 10.0.0.3 100   # AD = 100 (primary)
```

## Viewing Routes

### Display Routing Table

```
show ip route                      # All routes
show ip route ospf                 # Only OSPF routes
show ip route eigrp                # Only EIGRP routes
show ip route static               # Only static routes
show ip route connected            # Only connected routes
show ip route <IP>                 # Route for specific IP
```

### Detailed Route Information

```
show ip route 192.168.1.0          # Details about specific route
show ip protocols                  # Active routing protocols
show ip eigrp neighbors            # EIGRP neighbors
show ip eigrp topology             # EIGRP topology table
show interfaces brief              # Interface summary
```

### Route Output Explanation

```
R1# show ip route

Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area

C       192.168.1.0/24 is directly connected, gigabitethernet 0/0
D       10.0.0.0/24 [90/2680960] via 192.168.1.1, 00:05:32, gigabitethernet 0/0
S       172.16.0.0/16 [1/0] via 192.168.1.254

Breakdown:
  C = Route type (Connected, Static, EIGRP, OSPF, RIP, etc.)
  [90/2680960] = [Administrative Distance / Metric]
  via = Next hop IP address
  00:05:32 = Time since route was learned
  gigabitethernet 0/0 = Exit interface
```

## Administrative Distance (AD)

### AD Values (Lower = More Trusted)

```
Route Source           AD
Connected             0
Static                1
EIGRP Summary         5
External BGP          20
Internal EIGRP        90
OSPF                  110
Intermediate System-to-Intermediate System (IS-IS) 115
RIP                   120
External EIGRP        170
Internal BGP          200
Unknown               255 (not usable)
```

### Using AD to Create Backup Routes

```
R1(config)# ip route 10.0.0.0 255.255.0.0 10.0.1.2 100   # Primary (lower AD)
R1(config)# ip route 10.0.0.0 255.255.0.0 10.0.2.2 150   # Backup (higher AD)
```

## Dynamic Routing Protocols

### EIGRP Configuration Checklist

```
✓ Enable EIGRP with router eigrp <AS>
✓ Add networks with network statements
✓ Disable auto-summary (for subnets)
✓ Ensure interfaces are UP
✓ Check network connectivity between neighbors
✓ Verify neighbors are formed (show ip eigrp neighbors)
✓ Review routing table (show ip route)
```

### EIGRP Commands

```
router eigrp <AS>                  # Enable EIGRP
network <network> <wildcard>       # Advertise networks
no network <network> <wildcard>    # Stop advertising
no auto-summary                    # Don't summarize at classful boundaries
passive-interface <interface>      # Don't send EIGRP on this interface
no passive-interface <interface>   # Resume sending EIGRP
show ip eigrp neighbors            # Active neighbors
show ip eigrp topology             # EIGRP topology
show ip eigrp statistics           # EIGRP statistics
clear ip eigrp neighbors           # Reset neighbors
redistribute static               # Redistribute static routes
```

### EIGRP Concepts

- **Autonomous System (AS)**: Routing domain (all routers must have same AS)
- **Feasible Successor**: Backup route to destination
- **Metric**: Composite of bandwidth, delay, reliability, load, MTU
- **Convergence**: Time for all routers to agree on topology
- **Hello/Dead Intervals**: Neighbor discovery and keepalive timers

## Subinterfaces for Inter-VLAN Routing

### Router-on-a-Stick Configuration

```
interface <physical-interface>
no shutdown

interface <physical-interface>.<subinterface>
encapsulation dot1Q <vlan-number>
ip address <IP> <mask>
no shutdown
```

**Full Example:**
```
R1(config)# interface gigabitethernet 0/0
R1(config-if)# no shutdown
R1(config-if)# exit

R1(config)# interface gigabitethernet 0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 10.0.10.1 255.255.255.0
R1(config-subif)# exit

R1(config)# interface gigabitethernet 0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 10.0.20.1 255.255.255.0
R1(config-subif)# exit
```

### Verify Subinterfaces

```
show interfaces gigabitethernet 0/0
show interfaces gigabitethernet 0/0.10
show interfaces gigabitethernet 0/0.10 encapsulation
show ip interfaces brief
```

## Troubleshooting Routing

### Step 1: Verify Connectivity

```
R1# ping 10.0.0.1           # Can reach directly connected network?
R1# ping 192.168.1.1        # Can reach remote network?
```

### Step 2: Check Routing Table

```
R1# show ip route
```

**Troubleshooting Guide:**
```
- Is destination network in routing table?
- Is there a route to destination?
- Is next hop reachable?
- Is exit interface operational (UP)?
```

### Step 3: Verify Neighbors

```
R1# show ip eigrp neighbors     # Any neighbors formed?
R1# show ip eigrp topology      # Is destination in topology table?
```

**If no neighbors:**
```
1. Check network statements
2. Verify interfaces are UP
3. Confirm same EIGRP AS number
4. Check if interface is passive
5. Verify no ACLs blocking EIGRP (UDP 224.0.0.10, port 88)
```

### Step 4: Review Running Configuration

```
R1# show running-config | section eigrp
R1# show running-config | section "router eigrp"
```

### Common Routing Issues

```
Problem: Routes not appearing in routing table
Solution: Check network statements, verify interfaces UP

Problem: Neighbors not forming
Solution: Check AS numbers match, verify connectivity between devices

Problem: Routes appear then disappear
Solution: Check EIGRP hello/dead timers, network congestion

Problem: Slow convergence
Solution: Reduce hello/dead intervals (advanced), add more bandwidth

Problem: Route loops
Solution: Check for incorrect static routes, verify network design
```

## DHCP Configuration for Routing

### DHCP Server on Router

```
configure terminal
ip dhcp pool <pool-name>
network <network> <mask>
default-router <gateway-IP>
dns-server <DNS-IP>
lease <days> [hours] [minutes]
exit

ip dhcp excluded-address <start-IP> <end-IP>
ip dhcp excluded-address 10.0.0.1 10.0.0.50
```

**Example:**
```
R1(config)# ip dhcp pool OFFICE
R1(dhcp-config)# network 10.0.0.0 255.255.255.0
R1(dhcp-config)# default-router 10.0.0.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# exit

R1(config)# ip dhcp excluded-address 10.0.0.1 10.0.0.100
```

### DHCP Relay (for remote subnets)

```
interface <interface>
ip helper-address <DHCP-server-IP>
exit
```

### View DHCP Information

```
show ip dhcp pool
show ip dhcp binding
show ip dhcp statistics
clear ip dhcp binding *
```

## Best Practices for Routing

```
✓ Use consistent EIGRP AS numbers across domain
✓ Disable auto-summary for proper CIDR support
✓ Use meaningful network numbers (10.x, 172.16.x, 192.168.x)
✓ Document your network design and IP allocation
✓ Test routes after configuration
✓ Monitor neighbors and routing convergence
✓ Use passive-interface for non-router devices
✓ Plan for redundancy and failover
✓ Use static routes for default gateways
✓ Implement access lists carefully (can break routing)
```

---

**Last Updated:** August 2026
**Related:** CISCO-IOS-COMMANDS.md, VLAN-COMMANDS.md
