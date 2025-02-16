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
- [VNet vs VPC: Key Differences Between Azure VNet and AWS VPC](#vnet-vs-vpc-key-differences-between-azure-vnet-and-aws-vpc)
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


# **VNet vs VPC: Key Differences Between Azure VNet and AWS VPC**

Both **Azure Virtual Network (VNet)** and **AWS Virtual Private Cloud (VPC)** provide networking solutions for **isolated and secure communication** in the cloud. This document explains their differences, use cases, and when to choose one over the other.

---

## **1️⃣ Overview**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Definition** | Virtual Network (VNet) is Azure’s network solution to create isolated cloud networks. | Virtual Private Cloud (VPC) is AWS’s equivalent for creating an isolated cloud network. |
| **Purpose** | Provides an isolated **private network** for resources in Azure. | Provides a logically **isolated cloud network** for AWS resources. |
| **IP Addressing** | Uses **CIDR blocks** (`10.0.0.0/16`, `192.168.0.0/24`). | Uses **CIDR blocks**, but needs to be **manually defined** during VPC creation. |

---

## **2️⃣ IP Addressing & CIDR Block**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **IP Ranges (CIDR)** | Supports **IPv4 and IPv6** CIDR blocks. | Requires **manual CIDR** allocation (`/16` to `/28`). |
| **Private & Public Subnets** | Subnets **automatically route** between each other. | Subnets require **explicit route table configuration**. |
| **Default CIDR** | Default: `10.0.0.0/16` (Expandable) | Must be defined manually. Cannot be changed later. |

---

## **3️⃣ Subnetting**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Subnet Communication** | **Automatic** between subnets in the same VNet. | Requires **explicit route table configuration** between subnets. |
| **Regional Availability** | VNets are **regional** and cannot span across regions. | VPCs are **regional**, but **subnets** are **Availability Zone-specific**. |
| **Subnet Isolation** | Can isolate subnets using **Network Security Groups (NSG)**. | Can isolate subnets using **Security Groups & NACLs**. |

---

## **4️⃣ Peering & Connectivity**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Peering** | Supports **VNet Peering** for private connectivity across VNets. | Supports **VPC Peering**, but requires manual setup. |
| **Cross-Region Peering** | Supported (Global VNet Peering). | Supported but incurs additional cost. |
| **Gateway for External Communication** | Uses **VPN Gateway or ExpressRoute** for hybrid connectivity. | Uses **VPN Gateway or Direct Connect** for hybrid connectivity. |

---

## **5️⃣ Security & Access Control**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Access Control** | Uses **NSGs (Network Security Groups)** for subnet and resource-level security. | Uses **Security Groups (SGs) and NACLs (Network ACLs)** for access control. |
| **Firewalls** | **Azure Firewall** for centralized security. | **AWS Firewall Manager** for security policies. |
| **DDoS Protection** | Azure **DDoS Protection** for advanced security. | AWS **Shield** for DDoS protection. |

---

## **6️⃣ Integration with Other Services**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Load Balancing** | Supports **Azure Load Balancer** and **Application Gateway**. | Supports **AWS ELB (Elastic Load Balancer)** and **ALB (Application Load Balancer)**. |
| **DNS Service** | **Azure Private DNS** for private domain resolution. | **AWS Route 53** for private and public DNS. |
| **Hybrid Connectivity** | **ExpressRoute** (Private connection to on-premises). | **AWS Direct Connect** (Private link to on-premises). |

---

## **7️⃣ Pricing Model**
| Feature | **Azure VNet** | **AWS VPC** |
|---------|--------------|------------|
| **Cost of VNet/VPC** | Free to create, charges apply for **VPN, NAT Gateway, and bandwidth usage**. | Free to create, charges apply for **VPN, NAT Gateway, and data transfer**. |
| **Peering Costs** | **VNet Peering** incurs charges **only for cross-region traffic**. | **VPC Peering** is charged **based on data transfer** between VPCs. |
| **Data Transfer Costs** | Intra-region VNet data transfer is **free**. | Intra-region VPC data transfer **incurs charges**. |

---

## **🔹 Key Differences Summary**
1️⃣ **Subnets in Azure VNet** communicate automatically, while **AWS VPC requires manual route table setup**.  
2️⃣ **Azure VNet Peering** is simpler and cost-effective for intra-region traffic, while **AWS VPC Peering incurs charges**.  
3️⃣ **AWS VPC requires explicit route table configuration**, whereas **Azure VNet does not**.  
4️⃣ **Azure VNets are regional**, while **AWS VPCs are regional but subnets are AZ-specific**.  
5️⃣ **Security setup differs**: Azure uses **NSGs** while AWS uses **Security Groups + NACLs**.  

---

### **🎯 Which One to Choose?**
| **Scenario** | **Best Choice** |
|-------------|----------------|
| **If you're using Azure cloud** | ✅ Use **Azure VNet** |
| **If you're using AWS cloud** | ✅ Use **AWS VPC** |
| **For hybrid multi-cloud networking** | 🔄 Both **VNet & VPC** can be connected using **VPN or ExpressRoute/Direct Connect** |

---

## **Conclusion**
🔹 **Azure VNet** and **AWS VPC** serve the same purpose but differ in their **implementation and configuration**.  
🔹 **VNet is simpler for intra-subnet communication**, while **VPC requires more manual routing**.  
🔹 **Choose VNet for Azure workloads and VPC for AWS workloads**, but they can also be **interconnected for multi-cloud architectures**.  



## **Contributing**
We welcome contributions! Feel free to submit a pull request with improvements.

---

## **License**
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

