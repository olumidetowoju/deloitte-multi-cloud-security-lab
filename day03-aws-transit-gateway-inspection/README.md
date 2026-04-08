# 📘 Day 03 — AWS Transit Gateway + Centralized Inspection

---

## 🎯 Objective

Design and deploy an **AWS hub-and-spoke architecture** using **Transit Gateway (TGW)** with centralized traffic inspection.

By the end of this lab, you will:
- Create multiple VPCs
- Deploy a Transit Gateway
- Attach VPCs to TGW
- Configure route tables
- Simulate centralized inspection architecture
- Understand AWS vs Azure network differences

---

## 🧠 Concept (Think Like an Architect)

### ✈️ Analogy: Air Traffic Control System

- Transit Gateway = Air Traffic Control Tower
- VPCs = Airports
- Route Tables = Flight paths
- Inspection VPC = Security checkpoint hub

👉 No aircraft (traffic) moves without passing through controlled routing

---

## 🏗️ Architecture

```mermaid
flowchart TD
    A[Internet] --> B[Inspection VPC]
    B --> C[Transit Gateway]
    C --> D[Spoke VPC 1]
    C --> E[Spoke VPC 2]
    D --> F[Workloads]
    E --> G[Workloads]
🧱 Lab Architecture Design
Component	CIDR
Inspection VPC	10.10.0.0/16
Spoke VPC 1	10.11.0.0/16
Spoke VPC 2	10.12.0.0/16
Region	us-east-1
🧪 Lab Step 1 — Create VPCs
Inspection VPC
aws ec2 create-vpc \
  --cidr-block 10.10.0.0/16
Spoke VPC 1
aws ec2 create-vpc \
  --cidr-block 10.11.0.0/16
Spoke VPC 2
aws ec2 create-vpc \
  --cidr-block 10.12.0.0/16
🧪 Lab Step 2 — Create Transit Gateway
aws ec2 create-transit-gateway \
  --description "clab-tgw"
🧪 Lab Step 3 — Attach VPCs to TGW

👉 Replace IDs accordingly

aws ec2 create-transit-gateway-vpc-attachment \
  --transit-gateway-id <TGW_ID> \
  --vpc-id <VPC_ID> \
  --subnet-ids <SUBNET_ID>

Repeat for:

Inspection VPC

Spoke VPC 1

Spoke VPC 2

🧪 Lab Step 4 — Create Subnets

Example:

aws ec2 create-subnet \
  --vpc-id <VPC_ID> \
  --cidr-block 10.10.1.0/24
🧪 Lab Step 5 — Configure Route Tables
Key Concept

👉 AWS does NOT automatically route like Azure

You must define routes manually.

Example Route (Spoke → Inspection)
aws ec2 create-route \
  --route-table-id <ROUTE_TABLE_ID> \
  --destination-cidr-block 0.0.0.0/0 \
  --transit-gateway-id <TGW_ID>
🧠 Critical Concept — Centralized Inspection

In AWS:

You typically use:

Gateway Load Balancer

Network Firewall

Palo Alto / Check Point

👉 Traffic flow:

Spoke → TGW → Inspection VPC → Firewall → Internet
🧪 Lab Step 6 — Deploy Test EC2 Instance
aws ec2 run-instances \
  --image-id ami-0c02fb55956c7d316 \
  --count 1 \
  --instance-type t2.micro \
  --subnet-id <SUBNET_ID>
🧪 Lab Step 7 — Validate Connectivity

Check instance:

aws ec2 describe-instances
🔥 Azure vs AWS (Important Interview Section)
Feature	Azure	AWS
Hub Routing	Built-in	Requires TGW
Peering	Simple	Limited
Routing	Automatic	Manual
Firewall Placement	Native hub	Inspection VPC
Complexity	Lower	Higher

👉 This is a VERY common interview discussion.

🚨 Troubleshooting
No connectivity

Check:

aws ec2 describe-route-tables
TGW attachment issues
aws ec2 describe-transit-gateway-attachments
Instance unreachable

Check:

Security Groups

NACLs

Route Tables

✅ Validation Checklist

 VPCs created

 Transit Gateway deployed

 Attachments created

 Subnets created

 Routes configured

 EC2 instance deployed

 Connectivity verified

🎯 Key Takeaways

AWS networking = explicit control

Transit Gateway = central routing hub

Inspection VPC = security layer

Routing tables = critical control point

More complex than Azure → more flexible

🚀 Next Step

➡️ Day 04 — GCP Hub-and-Spoke + Cloud Armor

You will:

Build GCP networking

Implement edge protection

Compare all 3 clouds
