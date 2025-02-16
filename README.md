## **Overview**
This repository provides a detailed explanation of **Subnetting, CIDR, and VNet Peering** in cloud environments, covering both **Azure VNets and AWS VPC Peering**. It includes:
- **Understanding Subnetting**
- **Dividing a subnet into smaller parts**
- **Finding subnet using NID and HID**
- **CIDR (Classless Inter-Domain Routing)**
- **VNet Peering and VPC Peering communication in cloud environments**

## **Table of Contents**
- [IP Addressing](#ip-addressing)
- [Subnetting & Dividing a Subnet](#subnetting--dividing-a-subnet)
- [Finding the Subnet for an IP](#finding-the-subnet-for-an-ip)
- [CIDR (Classless Inter-Domain Routing)](#cidr-classless-inter-domain-routing)
- [VNet Peering and VPC Peering](#vnet-peering-and-vpc-peering)
- [Contributing](#contributing)
- [License](#license)

---

## **IP Addressing**
An **IP address** is a unique identifier for devices on a network. It exists in two versions:
- **IPv4 (32-bit)** → Example: `192.168.1.1`
- **IPv6 (128-bit)** → Example: `2001:0db8:85a3::8a2e:0370:7334`
  
Each IP address is divided into two parts:

**Network ID (NID):** Identifies the network/subnet.

**Host ID (HID):** Identifies a specific device within that subnet.


## **Subnetting & Dividing a Subnet**
Subnetting allows dividing a large network into smaller, manageable subnetworks.

### **Step 1: Understanding /26 (64 IPs per Subnet)**
A **/26 subnet** means:
- **26 bits are reserved for the network ID (NID)**.
- **6 bits are reserved for the host ID (HID)**.

#### **How many IPs in a /26 subnet?**
- **Formula:**  
Total IPs = 2^(32 - 26) = 2^6 = 64

#### **How many usable IPs?**
- **Total IPs = 64**
- **2 IPs are reserved** (Network & Broadcast Address)
- **Usable Hosts** = **64 - 2 = 62**

Each **/26 subnet** contains:
- **1 Network ID** (first IP)
- **62 Usable Hosts**
- **1 Broadcast Address** (last IP)

### **Step 2: Dividing /24 into 3 Subnets**
We start with **`192.168.1.0/24`** (256 IPs) and divide it into **/26 subnets (64 IPs each)**.

| **Subnet**  | **Network ID (NID)** | **First Host** | **Last Host** | **Broadcast Address** | **Total IPs** |
|------------|------------------|--------------|-------------|------------------|------------|
| **Subnet 1** | `192.168.1.0/26` | `192.168.1.1` | `192.168.1.62` | `192.168.1.63` | **64** |
| **Subnet 2** | `192.168.1.64/26` | `192.168.1.65` | `192.168.1.126` | `192.168.1.127` | **64** |
| **Subnet 3** | `192.168.1.128/26` | `192.168.1.129` | `192.168.1.190` | `192.168.1.191` | **64** |

---

## **Finding the Subnet for an IP**
To determine which subnet an IP belongs to:

#### **Example: Find the subnet for `192.168.1.25/26`**
- **Network ID (NID) = `192.168.1.0`**
- **Host ID (HID) = `25`**
- **Subnet Range = 192.168.1.0 - 192.168.1.63**

---

## **CIDR (Classless Inter-Domain Routing)**
CIDR is used for efficient IP address allocation.

### **Example**
| **CIDR Notation** | **IP Range** | **Total Addresses** |
|------------------|------------|----------------|
| `192.168.1.0/24` | `192.168.1.0 - 192.168.1.255` | **256** |
| `192.168.1.0/26` | `192.168.1.0 - 192.168.1.63` | **64** |

---

## **VNet Peering and VPC Peering**
VNet Peering (Azure) and VPC Peering (AWS) allow direct communication between different networks **without using the public internet**.

### **Azure VNet Peering**
- **VNet1 (`10.0.0.0/16`) in East US**
- **VNet2 (`10.1.0.0/16`) in West US**
- **Peering enables private communication** between the VNets.

### **AWS VPC Peering**
- **VPC1 (`192.168.1.0/24`) in `us-east-1`**
- **VPC2 (`192.168.2.0/24`) in `us-west-1`**
- **Peering connection established** for direct, private traffic.


## **Contributing**
We welcome contributions! Feel free to submit a pull request with improvements.

---

## **License**
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

