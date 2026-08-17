# File Organization & Renaming Guide

This guide helps you reorganize your existing Packet Tracer files into the new repository structure.

## 📋 File Mapping - Where Each File Should Go

### 01-Basics (Foundation Labs)
These files teach fundamental networking concepts

**Files to move here:**
- `C1.pkt` → **01-device-connectivity.pkt**
- `c1(a).pkt` → Keep if different, otherwise delete duplicate
- `c2(basicConfig).pkt` → **02-basic-ios-configuration.pkt**
- `C2.pkt` → Delete if duplicate of above
- `c3.pkt` → Review content, may delete if covered in other labs
- `STUDENT1.pkt` → Review content, organize or delete
- `LAB1.pkt` → Review content, organize or delete

---

### 02-VLAN-Configuration (VLAN & Trunking Labs)
These files cover VLAN creation, trunking, and VLAN management

**Files to move here:**
- `c3(StatickTrunking).pkt` → **02-static-trunking.pkt**
- `c4(Vlans&Trunking.pkt` → **04-vlans-and-trunking.pkt** (comprehensive)
- `c4(server).pkt` → Review (may be advanced, move to 05 if complex)
- `vlanImplementation.pkt` → **01-static-vlan-creation.pkt** (or rename to 03-dynamic-vlan-implementation.pkt if it's about dynamic VLANs)
- `vlanImplementation2.pkt` → **03-dynamic-vlan-implementation.pkt**

---

### 03-Routing (Routing Protocol Labs)
These files cover routing, EIGRP, inter-VLAN routing

**Files to move here:**
- `c5(ccna1).pkt` → Review content, likely keep as reference
- `c5(DHCPv4).pkt` → **04-dhcp-configuration.pkt**
- `c6(ccna1).pkt` → Review/organize
- `c7(Inter-VlanRouting).pkt` → **01-inter-vlan-routing.pkt**
- `c8(Router-on-stick).pkt` → **02-router-on-stick.pkt**
- `c8(EIGRP-routing).pkt` → **03-eigrp-routing.pkt**

---

### 04-Wireless (Wireless Networking Labs)
These files cover wireless access points, security, client connectivity

**Files to move here:**
- `c9(WirelessAP).pkt` → **01-wireless-access-point-basic.pkt**
- `c9(WirelessRouter).pkt` → **02-wireless-router-configuration.pkt**
- `WirelessNetwork.pkt` → **05-wireless-network-advanced.pkt** (or review)
- `WireleessConnection_with_access_point.pkt` → **01-wireless-access-point-basic.pkt** (if duplicate)
- `wireless to wired.pkt` → **03-wireless-to-wired-connectivity.pkt**
- `WirelessNetwork.pkt` → Keep/organize

---

### 05-Advanced (Complex Real-World Scenarios)
These files are comprehensive network scenarios combining multiple concepts

**Files to move here:**
- `c10(SOHOnetwork).pkt` → **01-soho-network.pkt**
- `Smart_home_network.pkt` → **02-smart-home-basic.pkt**
- `SmartHomeusingWirelessRouter.pkt` → **03-smart-home-wireless-router.pkt**
- `IOT simulations t11.pkt` → **04-iot-simulation.pkt**

---

### Files to Review/Delete
These files have unclear purposes or are likely duplicates:

**Review & Organize:**
- `packet tracer2.pkt` - Unknown content, review before organizing
- `pr2.pkt` - Unknown content, review before organizing
- `lab2.pkt` - Unknown content, review before organizing
- `t6.pkt` through `t11.pkt` - Test files? Review content
- `instr.pkt` - Instructor file? Review content

**Possible Delete (if duplicates):**
- `C1.pkt`, `C2.pkt` (if duplicates of lowercase versions)
- Numbered test files if covered in organized labs

---

## 🔄 How to Reorganize Your Files

### Method 1: Using Windows File Explorer (Easiest)

1. **Create folders** (if not done already):
   - Right-click in `saves` folder → New Folder
   - Create: 01-Basics, 02-VLAN-Configuration, 03-Routing, 04-Wireless, 05-Advanced, docs, reference

2. **Move files**:
   - Cut/copy files from the main folder
   - Paste into appropriate category folder
   - Rename files to match the mapping guide

3. **Create backups** (recommended):
   - Keep original files in a `_archive` folder
   - Copy renamed files to organized folders
   - Delete originals only after verifying organization works

### Method 2: Using Command Line (Advanced)

```
# Example commands (run in PowerShell/Command Prompt)

# Navigate to saves folder
cd "c:\Users\PC\Cisco Packet Tracer 8.2.2\saves"

# Move file to new location with new name
move "C1.pkt" "01-Basics\01-device-connectivity.pkt"
move "c2(basicConfig).pkt" "01-Basics\02-basic-ios-configuration.pkt"
move "c7(Inter-VlanRouting).pkt" "03-Routing\01-inter-vlan-routing.pkt"

# Verify move
dir 01-Basics\
```

### Method 3: Batch Rename Using Renaming Tool

Use a batch file rename tool:
- Built-in: `Right-click file → Rename`
- Or use free tools like Bulk Rename Utility
- Or use Windows PowerShell scripts

---

## 📝 Detailed File Mapping Table

| Current Filename | New Location | New Filename | Category | Content |
|-----------------|--------------|-------------|----------|---------|
| C1.pkt | 01-Basics | 01-device-connectivity.pkt | Basics | Device connection fundamentals |
| c1(a).pkt | [Review] | [Delete if duplicate] | [Check] | Check for duplicates |
| c2(basicConfig).pkt | 01-Basics | 02-basic-ios-configuration.pkt | Basics | Basic device configuration |
| C2.pkt | [Review] | [Delete if duplicate] | [Check] | Check for duplicates |
| c3(StatickTrunking).pkt | 02-VLAN | 02-static-trunking.pkt | VLAN | Static trunk configuration |
| c3.pkt | [Review] | [Organize] | [Check] | Check content, may delete |
| c4(Vlans&Trunking).pkt | 02-VLAN | 04-vlans-and-trunking.pkt | VLAN | Comprehensive VLAN/Trunk |
| c4(server).pkt | [Review] | [Check] | [Check] | May be advanced, review |
| c5(ccna1).pkt | 03-Routing | [Keep] | Routing | CCNA 1 reference |
| c5(DHCPv4).pkt | 03-Routing | 04-dhcp-configuration.pkt | Routing | DHCP server config |
| c6(ccna1).pkt | 03-Routing | [Keep] | Routing | CCNA 1 reference |
| c7(Inter-VlanRouting).pkt | 03-Routing | 01-inter-vlan-routing.pkt | Routing | Inter-VLAN routing |
| c8(EIGRP-routing).pkt | 03-Routing | 03-eigrp-routing.pkt | Routing | EIGRP protocol |
| c8(Router-on-stick).pkt | 03-Routing | 02-router-on-stick.pkt | Routing | Router subinterfaces |
| c9(WirelessAP).pkt | 04-Wireless | 01-wireless-access-point-basic.pkt | Wireless | Basic AP setup |
| c9(WirelessRouter).pkt | 04-Wireless | 02-wireless-router-configuration.pkt | Wireless | Wireless router |
| c10(SOHOnetwork).pkt | 05-Advanced | 01-soho-network.pkt | Advanced | SOHO network design |
| WireleessConnection_with_access_point.pkt | 04-Wireless | 01-wireless-access-point-basic.pkt | Wireless | Wireless AP (may be duplicate) |
| wireless to wired.pkt | 04-Wireless | 03-wireless-to-wired-connectivity.pkt | Wireless | Hybrid wired/wireless |
| WirelessNetwork.pkt | 04-Wireless | 05-wireless-network-advanced.pkt | Wireless | Advanced wireless |
| vlanImplementation.pkt | 02-VLAN | 01-static-vlan-creation.pkt | VLAN | VLAN creation |
| vlanImplementation2.pkt | 02-VLAN | 03-dynamic-vlan-implementation.pkt | VLAN | Dynamic VLAN |
| Smart_home_network.pkt | 05-Advanced | 02-smart-home-basic.pkt | Advanced | Smart home setup |
| SmartHomeusingWirelessRouter.pkt | 05-Advanced | 03-smart-home-wireless-router.pkt | Advanced | Smart home w/ WiFi |
| IOT simulations t11.pkt | 05-Advanced | 04-iot-simulation.pkt | Advanced | IoT network |

---

## ✅ Step-by-Step Organization Process

### Phase 1: Backup (IMPORTANT!)
```
1. Create folder: _backup
2. Copy ALL .pkt files to _backup
3. Verify backup has all files
4. Now you're safe to reorganize
```

### Phase 2: Create Structure
```
1. Create 5 main category folders (01-Basics through 05-Advanced)
2. Create docs and reference folders
3. Verify all folders created successfully
```

### Phase 3: Move & Rename Files
```
For each file in mapping table:
1. Move file to correct folder
2. Rename to new name
3. Verify in new location
4. Delete from old location (after verification)
```

### Phase 4: Quality Check
```
1. Verify no files left in root directory (except _backup)
2. Count files: should have ~25-30 organized .pkt files
3. Each folder has appropriate README.md
4. reference and docs folders have content
5. Backup folder still has copies
```

### Phase 5: Final Verification
```
1. Open 3-4 files from different categories
2. Verify they open correctly in Packet Tracer
3. Review README files match content
4. Test with a lab (01-Basics/01-device-connectivity.pkt)
```

---

## 📊 Expected Final Structure

After organization, you should have:

```
saves/
├── 01-Basics/
│   ├── README.md
│   ├── 01-device-connectivity.pkt
│   ├── 02-basic-ios-configuration.pkt
│   └── [other related files]
│
├── 02-VLAN-Configuration/
│   ├── README.md
│   ├── 01-static-vlan-creation.pkt
│   ├── 02-static-trunking.pkt
│   ├── 03-dynamic-vlan-implementation.pkt
│   └── 04-vlans-and-trunking.pkt
│
├── 03-Routing/
│   ├── README.md
│   ├── 01-inter-vlan-routing.pkt
│   ├── 02-router-on-stick.pkt
│   ├── 03-eigrp-routing.pkt
│   └── 04-dhcp-configuration.pkt
│
├── 04-Wireless/
│   ├── README.md
│   ├── 01-wireless-access-point-basic.pkt
│   ├── 02-wireless-router-configuration.pkt
│   ├── 03-wireless-to-wired-connectivity.pkt
│   ├── 04-secure-wireless-network.pkt
│   └── 05-wireless-network-advanced.pkt
│
├── 05-Advanced/
│   ├── README.md
│   ├── 01-soho-network.pkt
│   ├── 02-smart-home-basic.pkt
│   ├── 03-smart-home-wireless-router.pkt
│   └── 04-iot-simulation.pkt
│
├── docs/
│   ├── GETTING-STARTED.md
│   ├── NETWORK-BASICS.md
│   ├── VLAN-GUIDE.md
│   ├── ROUTING-GUIDE.md
│   └── TROUBLESHOOTING.md
│
├── reference/
│   ├── CISCO-IOS-COMMANDS.md
│   ├── VLAN-COMMANDS.md
│   ├── ROUTING-COMMANDS.md
│   └── WIRELESS-COMMANDS.md
│
├── _backup/
│   └── [original .pkt files]
│
├── README.md
└── ActivitySequencer/
```

---

## 🎯 Naming Convention Going Forward

For any new labs you create, follow this naming pattern:

```
[NUMBER]-[TOPIC]-[SPECIFIC-DESCRIPTION].pkt

Examples:
01-device-connectivity.pkt
02-basic-ios-configuration.pkt
01-inter-vlan-routing.pkt
03-eigrp-routing.pkt
01-wireless-access-point-basic.pkt
01-soho-network.pkt
```

**Benefits:**
- Easy to sort alphabetically
- Clear progression (01, 02, 03...)
- Descriptive but concise
- Consistent across all folders

---

## ⚠️ Important Notes

### Before Deleting
```
✓ Copy to _backup folder first
✓ Verify file opens in Packet Tracer
✓ Confirm it's a duplicate or obsolete
✓ Keep useful files even if organization unclear
```

### Handling Duplicates
If you find duplicate content:
```
Option 1: Keep both with different names (if slightly different)
Option 2: Keep one, move other to _archive
Option 3: Delete and update README with preferred version
```

### Merge Labs
If two files teach same content:
```
- Keep the more comprehensive one
- Name it as the primary
- Note in README: "Covers concepts from [old filename]"
- Archive the other
```

---

## 🚀 Next Steps After Organization

1. ✅ **Complete organization** (this file)
2. ✅ **Review existing labs** (Read READMEs)
3. ✅ **Start with 01-Basics** (Follow first lab)
4. ✅ **Progress sequentially** (Follow learning path)
5. ✅ **Create new content** (Add your own labs)

---

**Last Updated:** August 2026
**Status:** Ready for file organization
**Time Estimate:** 1-2 hours to complete reorganization
