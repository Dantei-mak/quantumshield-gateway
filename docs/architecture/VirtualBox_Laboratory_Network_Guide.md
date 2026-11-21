# VIRTUALBOX LABORATORY NETWORK & SYSTEM CONFIGURATION GUIDE

## Project Title
**Design, Implementation and Performance Evaluation of a Hybrid Post-Quantum Secure VPN for Financial Institution Communication**

**Document Version:** 1.0  
**Target Architecture:** macOS Host (x86_64) <---> Ubuntu Linux Guest VM (VirtualBox)  
**Academic Context:** Final Year Software Engineering Project, University of Dodoma (UDOM)

---

## 1. Laboratory Overview

The laboratory testbed models two independent financial institutions connected across an untrusted underlay network:

```
+-----------------------------------------------------------------------------------------+
|                                    MACBOOK HOST                                         |
|                      (Simulated Institution A: Banque de France)                        |
|                                                                                         |
|   Physical Host IP (Host-Only vboxnet0):   192.168.56.1                                  |
|   Overlay Tunnel IP (Virtual tun0/utun): 10.10.0.2                                     |
|   OpenSSL Version:                       3.6.4 (Native ML-KEM, ML-DSA, Hybrid Support)  |
|   Role:                                  VPN Client / Initiator                         |
+-----------------------------------------------------------------------------------------+
                                             |
                  VirtualBox Host-Only Virtual Switch (192.168.56.0/24)
                                             |
+-----------------------------------------------------------------------------------------+
|                                 UBUNTU LINUX GUEST VM                                   |
|                   (Simulated Institution B: Deutsche Bundesbank)                        |
|                                                                                         |
|   Guest VM Virtual IP (NIC Adapter 1):   192.168.56.10                                  |
|   Overlay Tunnel IP (Virtual tun0):      10.10.0.1                                      |
|   Role:                                  VPN Server Gateway / Responder                 |
+-----------------------------------------------------------------------------------------+
```

---

## 2. Step 1: VirtualBox Host-Only Network Configuration

### 2.1 Why Host-Only Networking?
VirtualBox provides several networking modes:
- **NAT:** Isolates the VM behind private address translation; prevents the Mac host from cleanly initiating connections to the guest without port forwarding.
- **Bridged:** Attaches directly to the physical Wi-Fi/Ethernet; exposes experimental packets to the external local network, violating project isolation constraints (C-03).
- **Host-Only Networking (`vboxnet0`):** Creates an isolated virtual software switch inside macOS. Only the Mac host and the guest VM are attached. This provides pure, noise-free, reproducible benchmarking.

### 2.2 Configuring Host-Only Adapter on macOS
1. Open the **VirtualBox GUI** (`/Applications/VirtualBox.app`).
2. Navigate to: **Tools** -> **Network Manager** (or **Preferences** -> **Network** -> **Host-only Networks**).
3. If `vboxnet0` is not present, click **Create**.
4. Configure the adapter parameters:
   - **IPv4 Address:** `192.168.56.1`
   - **IPv4 Network Mask:** `255.255.255.0`
   - **DHCP Server:** **Disable** (Recommended for fixed static IP assignments in scientific benchmarking).

Alternatively, from macOS Terminal:
```bash
# If VBoxManage is initialized:
VBoxManage hostonlyif create
VBoxManage hostonlyif ipconfig vboxnet0 --ip 192.168.56.1 --netmask 255.255.255.0
```

---

## 3. Step 2: Provisioning the Ubuntu Linux VM (Institution B)

### 3.1 Recommended VM Specifications
- **OS:** Ubuntu Server 22.04 LTS or 24.04 LTS (64-bit).
- **Processors:** 2 vCPUs.
- **Memory (RAM):** 4096 MB (4 GB).
- **Storage:** 25 GB Virtual Disk (Dynamically allocated VDI).

### 3.2 Network Adapter Assignment in VirtualBox
In the VM Settings window (**Settings** -> **Network**):
- **Adapter 1:**
  - Check **Enable Network Adapter**.
  - Attached to: **Host-only Adapter**.
  - Name: `vboxnet0`.
  - Advanced -> Promiscuous Mode: **Allow All**.
- **Adapter 2 (Optional for Package Downloads):**
  - Attached to: **NAT** (Allows downloading packages like `iperf3`, `build-essential`, `python3-pip`). *Disable during active benchmarking.*

---

## 4. Step 3: Guest Static IP Configuration (`netplan`)

Inside the Ubuntu VM terminal, configure static IP `192.168.56.10`:

1. Identify the virtual network interface name:
   ```bash
   ip link show
   # Commonly enp0s3 or eth0
   ```

2. Edit the netplan configuration (`/etc/netplan/01-netcfg.yaml` or `/etc/netplan/50-cloud-init.yaml`):
   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       enp0s3:
         dhcp4: no
         addresses:
           - 192.168.56.10/24
         nameservers:
           addresses: [8.8.8.8, 1.1.1.1]
   ```

3. Apply network configuration:
   ```bash
   sudo netplan apply
   ip addr show enp0s3
   ```

---

## 5. Step 4: Verification of Underlay Connectivity

Execute the following checks to confirm physical/virtual connectivity before proceeding to the cryptographic layer:

### Check 1: Ping from macOS Host to Linux Guest
```bash
ping -c 4 192.168.56.10
```
*Expected Result:*
```
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.312/0.450/0.612/0.098 ms
```

### Check 2: Ping from Linux Guest to macOS Host
```bash
ping -c 4 192.168.56.1
```
*Expected Result:* 0% packet loss.

### Check 3: SSH Reachability
Enable SSH on the Ubuntu guest:
```bash
sudo apt update && sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```
Connect from macOS:
```bash
ssh user@192.168.56.10
```

---

## 6. Step 5: Cryptographic Toolchain Verification

Our inspection confirmed that your macOS host has **OpenSSL 3.6.4** installed with **native support** for:
- **ML-KEM (FIPS 203):** `ML-KEM-512`, `ML-KEM-768`, `ML-KEM-1024`
- **ML-DSA (FIPS 204):** `ML-DSA-44`, `ML-DSA-65`, `ML-DSA-87`
- **Hybrid Groups:** `X25519MLKEM768`, `SecP256r1MLKEM768`, `X448MLKEM1024`, `SecP384r1MLKEM1024`

### Verifying OpenSSL Capabilities on macOS:
```bash
# Verify KEM algorithms:
openssl list -kem-algorithms

# Verify Signature algorithms:
openssl list -signature-algorithms | grep -E "ML-DSA|SLH-DSA"

# Generate a test ML-KEM-768 keypair:
openssl genpkey -algorithm ML-KEM-768 -out test_mlkem768_priv.pem
openssl pkey -in test_mlkem768_priv.pem -pubout -out test_mlkem768_pub.pem

# Generate a test ML-DSA-65 keypair:
openssl genpkey -algorithm ML-DSA-65 -out test_mldsa65_priv.pem
openssl pkey -in test_mldsa65_priv.pem -pubout -out test_mldsa65_pub.pem
```

---

## 7. Step 6: Overlay Virtual Tunnel IP Addressing (`10.10.0.0/24`)

Once the underlay network (`192.168.56.0/24`) is active, the VPN tunnel engine dynamically creates virtual `tun` interfaces:

| Endpoint | Virtual Interface | Tunnel IP Address | Subnet Mask | Destination Route |
| :--- | :--- | :--- | :--- | :--- |
| **macOS Host (Inst. A)** | `tun0` (or `utunX`) | `10.10.0.2` | `255.255.255.0` | `10.10.0.1` |
| **Linux VM (Inst. B)** | `tun0` | `10.10.0.1` | `255.255.255.0` | `10.10.0.2` |

When an application on macOS transmits to `10.10.0.1`:
1. macOS routing table directs the packet to `tun0`.
2. The VPN client intercepts the raw IP packet.
3. The packet is encrypted with **AES-256-GCM** using the hybrid session key.
4. The encrypted packet is wrapped into an underlay UDP/TCP packet sent to `192.168.56.10:1194`.
5. The Linux server receives, decrypts, and injects the packet into its local `tun0`, where the listening settlement receiver accepts it.

---
*End of Laboratory Network & System Configuration Guide*
