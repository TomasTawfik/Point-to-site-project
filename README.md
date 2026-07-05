# Site-to-Site VPN Project on Azure

This project demonstrates how to build a secure Site-to-Site VPN connection between an Azure virtual network and an on-premises-like virtual network using Azure Resource Manager (ARM) templates.

---

## 📦 Project Structure

- `template.json`: ARM template defining all resources
- `parameters.json`: Parameter file for deployment
- `README.md`: Documentation and deployment steps

---

## 🧱 Architecture Overview

- Two VNets:
  - `Azure-HQ-VNet` (Azure side)
  - `HQ-VNet` (simulated on-premises)
- Two VPN Gateways:
  - `Azure_VPN_Getway`
  - `VPNGetWay_onprimises`
- Two Local Network Gateways:
  - `AzureLocalNetwokGetWay`
  - `HQLocalNetworkGetWay`
- Two VMs:
  - `AzureSideVM1` (IP: 10.1.0.4)
  - `HQVM2` (IP: 198.168.0.4)
- NSGs configured to allow RDP, HTTP, HTTPS, and ICMP

---

## 🚀 Deployment Steps

### 1. Create VNets and Subnets
- `Azure-HQ-VNet` with:
  - `default_Supnet`: `10.1.0.0/24`
  - `GatewaySubnet`: `10.1.1.0/27`
- `HQ-VNet` with:
  - `default`: `198.168.0.0/24`
  - `GatewaySubnet`: `198.168.1.0/27`
  - `AzureBastionSubnet`: `198.168.2.0/26`

### 2. Create Public IPs
- `public_ip_forazure_VPN`
- `public_ip2`
- `AzureSideVM1_ip`

### 3. Create Virtual Network Gateways
- `Azure_VPN_Getway` → Azure side
- `VPNGetWay_onprimises` → HQ side

### 4. Create Local Network Gateways
- `AzureLocalNetwokGetWay`:
  - IP: `20.42.48.175`
  - Address Space: `198.168.0.0/16`
- `HQLocalNetworkGetWay`:
  - IP: `20.172.178.12`
  - Address Space: `10.1.0.0/16`

### 5. Create VPN Connections
- `azure_to_hq`: Azure → HQ
- `HQ_to_Azure`: HQ → Azure
- Shared Key: `TomasVPN123!`
- Protocol: `IKEv2`

### 6. Deploy VMs
- `AzureSideVM1` in Azure VNet
- `HQVM2` in HQ VNet
- OS: Windows Server 2022

### 7. Configure NSGs
- Allow:
  - RDP (3389)
  - HTTP (80)
  - HTTPS (443)
  - ICMP (Ping)

### 8. Test Connectivity
- Ping from `AzureSideVM1` → `HQVM2`
- Ping from `HQVM2` → `AzureSideVM1`
- Ensure VPN Connection status is **Connected**
- Open ICMP in Windows Firewall:
  ```powershell
  New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4 -Direction Inbound -Action Allow