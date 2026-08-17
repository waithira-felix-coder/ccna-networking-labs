# 04-Wireless: Wireless Networking Implementation

**Difficulty Level:** Intermediate  
**Estimated Time per Lab:** 45-75 minutes  
**Prerequisites:** Complete 01-Basics (highly recommended: 02-VLAN-Configuration)

## 📖 Overview

This section covers wireless networking concepts, access point configuration, security settings, and integration of wireless networks with wired infrastructure.

## 🎯 Learning Objectives

After completing this section, you will be able to:

- [ ] Explain wireless networking fundamentals (802.11, SSID, channels)
- [ ] Configure wireless access points
- [ ] Set wireless network names (SSIDs)
- [ ] Implement wireless security (WPA2, WPA3)
- [ ] Configure wireless client connectivity
- [ ] Troubleshoot wireless connectivity issues
- [ ] Integrate wireless with wired networks
- [ ] Understand wireless standards and channels
- [ ] Configure wireless routers with integrated switching

## 📋 Labs in This Section

### Lab 1: Wireless Access Point - Basic
**File:** `01-wireless-access-point-basic.pkt`  
**Duration:** 45 minutes

#### Description
Configure a basic wireless access point with SSID, security settings, and connect wireless clients. Learn fundamental wireless concepts.

#### Network Topology
```
                        ┌──────────────┐
                        │   Router R1  │ (DHCP Server)
                        │  172.16.0.1  │ Gateway for WLAN
                        └──────┬───────┘
                               |
                        (Wired Connection)
                               |
                        ┌──────┴───────┐
                        │ Access Point │
                        │   AP-01      │ SSID: "CiscoLab"
                        │  172.16.0.50 │ Channel 6
                        └──────────────┘
                        Wireless Range: ~30m

            ┌─────────────────┬─────────────────┐
            |                 |                 |
          Laptop1         Laptop2          Desktop1
        (DHCP Client)   (DHCP Client)   (DHCP Client)
```

#### Configuration Tasks

1. **Configure Access Point IP Settings**
   ```
   AP# configure terminal
   AP(config)# hostname AP-01
   AP(config)# interface vlan 1
   AP(config-if)# ip address 172.16.0.50 255.255.255.0
   AP(config-if)# no shutdown
   AP(config-if)# exit
   AP(config)# ip default-gateway 172.16.0.1
   AP(config)# exit
   AP# copy running-config startup-config
   ```

2. **Configure Wireless Settings (SSID)**
   
   *Via GUI (easier for basic config):*
   - Navigate to Wireless tab
   - SSID: "CiscoLab"
   - Channel: 6
   - Mode: 802.11g (or mixed)
   - Width: 20 MHz
   - Power: High

   *Or via CLI:*
   ```
   AP(config)# interface dot11radio 0
   AP(config-if)# ssid CiscoLab
   AP(config-ssid)# authentication open
   AP(config-ssid)# guest-mode
   AP(config-ssid)# exit
   AP(config-if)# channel 6
   AP(config-if)# power local 100
   AP(config-if)# exit
   ```

3. **Configure Router for DHCP**
   ```
   R1# configure terminal
   R1(config)# interface fastethernet 0/0
   R1(config-if)# ip address 172.16.0.1 255.255.255.0
   R1(config-if)# no shutdown
   R1(config-if)# exit
   
   R1(config)# ip dhcp pool WIRELESS
   R1(dhcp-config)# network 172.16.0.0 255.255.255.0
   R1(dhcp-config)# default-router 172.16.0.1
   R1(dhcp-config)# dns-server 8.8.8.8
   R1(dhcp-config)# exit
   R1(config)# exit
   ```

4. **Configure Wireless Client**
   - Right-click laptop → Wireless
   - Select "CiscoLab" SSID
   - No authentication (Open)
   - Get IP via DHCP

#### Verification Steps

```
AP# show interfaces vlan 1
AP# show dot11 associations
R1# show ip dhcp binding
Laptop> ipconfig
Laptop> ping 172.16.0.50  (Ping AP)
Laptop> ping 172.16.0.1   (Ping Router)
```

#### Key Concepts

- **SSID**: Service Set Identifier - wireless network name
- **BSSID**: Basic Service Set Identifier - AP MAC address
- **Channel**: Frequency used by wireless (1-13 available)
- **802.11 Standards**: 802.11a/b/g/n/ac/ax
- **Power Level**: Transmission strength (range)

---

### Lab 2: Wireless Router Configuration
**File:** `02-wireless-router-configuration.pkt`  
**Duration:** 60 minutes

#### Description
Configure a wireless router with integrated switching, bridging wired and wireless networks.

#### Configuration Tasks

(See individual lab README for detailed configuration)

---

### Lab 3: Wireless to Wired Connectivity
**File:** `03-wireless-to-wired-connectivity.pkt`  
**Duration:** 60 minutes

#### Description
Integrate wireless network with wired infrastructure using VLANs and access points.

#### Network Topology
```
Wired Network (VLAN 10)          Wireless Network (VLAN 20)
      PC1 ──────┐                    Laptop1 ┐
      PC2 ──────┼── Switch ───┬──── AP ─────┬── Laptop2
      PC3 ──────┘             |              └── Laptop3
                              │
                           Router
                         (172.16.0.1)
```

#### Configuration Tasks

1. **Create VLANs on Switch**
   ```
   S1(config)# vlan 10
   S1(config-vlan)# name Wired
   S1(config-vlan)# exit
   S1(config)# vlan 20
   S1(config-vlan)# name Wireless
   S1(config-vlan)# exit
   
   S1(config)# interface fastethernet 0/1
   S1(config-if)# switchport mode access
   S1(config-if)# switchport access vlan 10
   S1(config-if)# exit
   
   S1(config)# interface fastethernet 0/24
   S1(config-if)# switchport mode trunk
   S1(config-if)# switchport trunk allowed vlan 10,20
   S1(config-if)# exit
   ```

2. **Configure Router Subinterfaces**
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

3. **Configure AP for VLAN 20**
   ```
   AP(config)# interface vlan 20
   AP(config-if)# ip address 10.0.20.50 255.255.255.0
   AP(config-if)# no shutdown
   AP(config-if)# exit
   AP(config)# ip default-gateway 10.0.20.1
   ```

4. **Configure IP Addressing**
   - PC1-PC3 (VLAN 10): 10.0.10.10-12 / Gateway 10.0.10.1
   - Laptops (VLAN 20): DHCP from 10.0.20.0/24

---

### Lab 4: Secure Wireless Network
**File:** `04-secure-wireless-network.pkt`  
**Duration:** 75 minutes

#### Description
Implement WPA2 security with PSK (Pre-Shared Key) authentication on wireless network.

#### Configuration Tasks

1. **Configure WPA2 Security**
   
   *Via GUI:*
   - AP > Wireless tab
   - Security: WPA2-Personal
   - Password: cisco@123
   - Cipher: AES

   *Via CLI:*
   ```
   AP(config)# interface dot11radio 0
   AP(config-if)# ssid CiscoSecure
   AP(config-ssid)# authentication open
   AP(config-ssid)# authentication key-management wpa
   AP(config-ssid)# wpa-psk ascii 0 cisco@123
   AP(config-ssid)# exit
   AP(config-if)# exit
   ```

2. **Client Configuration**
   - Right-click wireless client
   - Wireless tab > SSID: CiscoSecure
   - Authentication: WPA2-PSK
   - Password: cisco@123

#### Key Concepts

- **WPA/WPA2**: Wi-Fi Protected Access (security standards)
- **PSK**: Pre-Shared Key (password-based authentication)
- **802.1X**: Enterprise authentication (optional)
- **Cipher**: AES encryption standard
- **Passphrase**: Strong password (12+ characters recommended)

---

### Lab 5: Advanced Wireless Network
**File:** `05-wireless-network-advanced.pkt`  
**Duration:** 75 minutes

#### Description
Complex wireless scenario with multiple APs, roaming, and integration with enterprise network.

(See individual lab README for detailed configuration)

---

## 🔧 Skills to Practice

- Access point configuration
- SSID creation and management
- Wireless security implementation
- Wireless client troubleshooting
- Channel selection and interference
- Power level adjustment
- Integration with wired networks
- DHCP for wireless clients
- Wireless monitoring and diagnostics

## 📚 Wireless Command Reference

### Access Point Configuration
```
configure terminal               # Enter configuration mode
hostname <name>                 # Set AP hostname
interface vlan 1                # Access management VLAN
ip address <IP> <mask>          # Set management IP
ip default-gateway <IP>         # Set gateway
```

### Wireless (802.11) Commands
```
interface dot11radio 0          # Configure wireless interface
ssid <network-name>             # Define SSID
authentication open             # Open authentication
channel <number>                # Set wireless channel
power local <percentage>        # Set transmit power
```

### Security Commands
```
wpa-psk ascii 0 <password>      # WPA2 Pre-Shared Key
authentication key-management wpa  # Enable WPA
```

## 🔍 Wireless Standards Comparison

| Standard | Frequency | Speed | Range | Notes |
|----------|-----------|-------|-------|-------|
| 802.11b  | 2.4 GHz   | 11 Mbps | 100m | Legacy |
| 802.11g  | 2.4 GHz   | 54 Mbps | 100m | Backward compat with 11b |
| 802.11a  | 5 GHz     | 54 Mbps | 50m | Less interference |
| 802.11n  | 2.4/5 GHz | 600 Mbps | 200m | MIMO technology |
| 802.11ac | 5 GHz     | 1.3 Gbps | 100m | Wide channels |
| 802.11ax | 2.4/5 GHz | 9.6 Gbps | 100m | WiFi 6, OFDMA |

## 🔍 Wireless Channels

**2.4 GHz Band (3 non-overlapping):**
- Channels 1, 6, 11 (North America)
- Channels 1-13 (Europe)

**5 GHz Band:**
- 36-48, 52-144, 149-165 (many non-overlapping channels)

## ⚠️ Common Wireless Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| No wireless network visible | SSID broadcast disabled | Enable SSID broadcast |
| Can't connect to network | Wrong security type/password | Verify WPA2 settings and PSK |
| Low signal strength | Distance or interference | Move closer, change channel |
| Slow data rate | Channel congestion | Switch to less-congested channel |
| Intermittent drops | Adjacent AP interference | Use non-overlapping channels |
| Can't get IP address | DHCP not configured | Verify DHCP pool on gateway |

## 🚀 Next Steps

Once you've completed all labs in this section:
1. Add roaming between multiple APs
2. Implement band steering (2.4 to 5 GHz)
3. Configure wireless VLANs for guest networks
4. Move on to **05-Advanced** for real-world scenarios

## ✅ Completion Checklist

- [ ] Configured basic wireless access point
- [ ] Set SSID and radio channel
- [ ] Implemented WPA2 security
- [ ] Connected wireless clients
- [ ] Configured DHCP for wireless
- [ ] Integrated wireless with wired network using VLANs
- [ ] Verified connectivity between wired and wireless
- [ ] Understood wireless standards and channels
- [ ] Troubleshot wireless connectivity issues
- [ ] Configured roaming (if multi-AP lab)

---

**Last Updated:** August 2026
