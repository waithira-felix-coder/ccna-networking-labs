# VLAN and Trunking Commands Reference

## VLAN Management

### Creating and Configuring VLANs

```
configure terminal                 # Enter config mode
vlan <vlan-number>                # Create VLAN (1-4094)
  name <vlan-name>                # Name the VLAN
  exit                            # Exit VLAN config
```

**Example:**
```
S1(config)# vlan 10
S1(config-vlan)# name Faculty
S1(config-vlan)# exit
S1(config)# vlan 20
S1(config-vlan)# name Students
S1(config-vlan)# exit
```

### Viewing VLAN Information

```
show vlan                         # View all VLANs (verbose)
show vlan brief                   # Quick VLAN summary
show vlan id <number>             # Specific VLAN details
show vlan name <name>             # VLAN by name
show interfaces <interface> switchport  # Port VLAN info
show vlan static                  # Static VLAN assignments
```

### Deleting VLANs

```
no vlan <number>                  # Delete VLAN
# Note: Ports remain in deleted VLAN until reconfigured
```

## Access Port Configuration

### Assigning Ports to VLANs

```
configure terminal
interface <interface>             # e.g., fastethernet 0/1
  switchport mode access         # Set to access mode
  switchport access vlan <num>   # Assign to VLAN
  exit
```

**Example:**
```
S1(config)# interface fastethernet 0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10
S1(config-if)# exit
```

### Multiple Port Configuration

```
interface range fastethernet 0/1 - 10
  switchport mode access
  switchport access vlan 10
  exit
```

### Removing Port from VLAN

```
interface <interface>
  no switchport access vlan       # Returns port to VLAN 1
  exit
```

## Trunk Configuration

### Enabling Trunk Mode

```
configure terminal
interface <interface>             # e.g., fastethernet 0/24
  switchport mode trunk          # Enable trunk mode
  exit
```

### Specifying Allowed VLANs

```
switchport trunk allowed vlan <vlan-list>

# Examples:
switchport trunk allowed vlan 1,10,20,30
switchport trunk allowed vlan 10-20,30,40
switchport trunk allowed vlan all        # All VLANs
switchport trunk allowed vlan except 1   # All except 1
```

### Setting Native VLAN

```
switchport trunk native vlan <number>    # Set native VLAN
# Native VLAN is used for untagged traffic on trunk
# Default: VLAN 1 (not recommended for production)
```

### Full Trunk Configuration Example

```
S1(config)# interface gigabitethernet 0/1
S1(config-if)# description Trunk to S2
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk allowed vlan 10,20,30,99
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# exit
```

## Trunk Verification Commands

```
show trunk                        # Trunk summary
show interfaces trunk             # Detailed trunk info
show interfaces <interface> trunk # Specific trunk details
show interfaces <interface> switchport  # Switchport info
```

### Example Output Interpretation

```
S1# show trunk

Port        Mode         Encapsulation  Status        Native vlan
Gi0/1       on           802.1q         trunking      99

Port        Vlans allowed on trunk
Gi0/1       1,10,20,30,99

Port        Vlans allowed and active in management domain
Gi0/1       1,10,20,30,99

Port        Vlans in spanning tree forwarding state
Gi0/1       1,10,20,30,99
```

## Dynamic Trunking Protocol (DTP)

### Trunk Negotiation Modes

```
switchport mode dynamic auto      # Will trunk if other side is desirable
switchport mode dynamic desirable # Will actively try to negotiate trunk
switchport mode trunk             # Static trunk (no DTP)
switchport nonegotiate            # No DTP negotiation
```

### DTP Configuration

```
interface <interface>
  switchport mode dynamic desirable
  exit
```

### Disabling DTP

```
interface <interface>
  switchport nonegotiate          # Disables DTP
  exit
```

## VLAN Routing (Router-on-a-Stick)

### Creating Subinterfaces

```
interface <physical>.<subinterface-number>
  encapsulation dot1Q <vlan-number>
  ip address <IP> <mask>
  no shutdown
  exit
```

**Example:**
```
R1(config)# interface gigabitethernet 0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 10.0.10.1 255.255.255.0
R1(config-subif)# exit

R1(config)# interface gigabitethernet 0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 10.0.20.1 255.255.255.0
R1(config-subif)# exit
```

### Verifying Subinterfaces

```
show interfaces <interface>
show interfaces <interface>.<subif>
show ip interfaces brief
show ip route
```

## Common VLAN Numbering Schemes

### Standard Allocation

```
VLAN 1      - Default (native, management)
VLAN 10     - Faculty/Staff
VLAN 20     - Students
VLAN 30     - Guests
VLAN 40     - Servers/Infrastructure
VLAN 50     - Management (OOB)
VLAN 99     - Native VLAN (on trunks)
VLAN 100+   - Additional departments/purposes
```

### Enterprise Example

```
VLAN 1      - Native/Default
VLAN 10-19  - Administration
VLAN 20-29  - Engineering
VLAN 30-39  - Sales
VLAN 40-49  - Support
VLAN 50-59  - Guest Access
VLAN 60-69  - Servers
VLAN 70-79  - IoT Devices
VLAN 100    - Management/OOB
```

## Troubleshooting Commands

```
show vlan brief                   # Quick status check
show interfaces switchport        # All port assignments
show trunk                        # Trunk status
show interfaces <int> switchport  # Specific port
show mac address-table vlan <num> # MACs in VLAN
show interfaces <int> encapsulation # Subinterface encapsulation
```

## Common Issues & Solutions

### Port Not in VLAN

```
# Check current assignment
show interfaces fastethernet 0/1 switchport

# Verify VLAN exists
show vlan id 10

# Reassign port
interface fastethernet 0/1
switchport mode access
switchport access vlan 10
```

### Trunk Not Working

```
# Verify trunk mode
show trunk

# Check encapsulation
show interfaces gigabitethernet 0/1 switchport

# Verify native VLAN matches
show interfaces gigabitethernet 0/1 trunk
```

### Connectivity Between VLANs

```
# VLANs are isolated - need router!
# Configure subinterfaces on router for inter-VLAN routing
# Set default gateway to router subinterface IP
```

## 802.1Q Frame Structure

```
Untagged Frame:
[Dest MAC][Src MAC][Type/Len][Payload][FCS]

802.1Q Tagged Frame:
[Dest MAC][Src MAC][802.1Q Tag][Type/Len][Payload][FCS]

802.1Q Tag Contains:
- TPID (0x8100)
- Priority (3 bits)
- CFI (1 bit)
- VLAN ID (12 bits) - Identifies VLAN 1-4094
```

## Best Practices

```
✓ Use meaningful VLAN names (Faculty, not V10)
✓ Document your VLAN allocation scheme
✓ Use non-overlapping VLAN ranges by department
✓ Set native VLAN to something other than 1 (e.g., 99)
✓ Keep management traffic separate (VLAN 100)
✓ Disable unused ports (shutdown)
✓ Use descriptions on interfaces
✓ Test connectivity before considering job complete
✓ Keep backup of configurations
✓ Use static trunking for stability (disable DTP)
```

---

**Last Updated:** August 2026
**Related:** CISCO-IOS-COMMANDS.md, ROUTING-COMMANDS.md
