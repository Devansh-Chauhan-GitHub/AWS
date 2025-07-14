# 📀 AWS AMI (Amazon Machine Image) Guide

This documentation covers everything about **Amazon Machine Images (AMIs)** including:

- What is an AMI
- Creating an AMI from an EC2 instance
- Launching instances from an AMI
- Deregistering and deleting AMIs
- Differences between AMIs and Launch Templates

---

## 💳 What is an AMI?

An **Amazon Machine Image (AMI)** is a pre-configured template that contains:

- A root volume snapshot (OS, configs, applications)
- Launch permissions
- Block device mapping (EBS volumes)

It allows you to **launch EC2 instances** with the same configuration repeatedly.

---

## ➕ Step 1: Create an AMI from an EC2 Instance

1. Go to **EC2 Dashboard > Instances**.
2. Select the instance you want to create an image of.
3. Click **Actions > Image and templates > Create image**.
4. Provide:
   - Name and description
   - Whether to reboot the instance (recommended)
   - Additional volumes if needed
5. Click **Create Image**.
6. Go to **AMIs** under the EC2 Dashboard to monitor creation.

---

## 🏢 Step 2: Launch EC2 from AMI

1. Go to **EC2 Dashboard > AMIs**.
2. Select the AMI.
3. Click **Launch Instance from Image**.
4. Choose instance type, key pair, network settings, etc.
5. Launch the instance.

---

## ❌ Step 3: Deregister and Delete AMI

If you no longer need an AMI:

1. Go to **EC2 Dashboard > AMIs**.
2. Select the AMI → Click **Actions > Deregister**.
3. Confirm the deregistration.
4. Go to **Snapshots** (AMI creates snapshots).
5. Manually delete any associated snapshots to avoid charges.

---

## 🔹 AMI vs Launch Template: Key Differences

| Feature              | AMI                                      | Launch Template                                  |
| -------------------- | ---------------------------------------- | ------------------------------------------------ |
| What it defines      | OS image + software + config             | Full EC2 config (AMI, instance type, networking) |
| Contains networking? | ❌ No                                     | ✅ Yes                                            |
| Reusability          | Can be used to launch multiple instances | Can be versioned and reused across environments  |
| Version Control      | ❌ No                                     | ✅ Yes (versioned templates)                      |
| Customization        | Only the OS/app snapshot                 | Full instance configuration                      |
| Cost                 | No cost unless storing snapshot          | No cost to store template                        |
| Used with            | EC2, Auto Scaling (indirectly)           | EC2, Auto Scaling Groups, Spot Fleets            |

### 🔑 Summary:

- **Use AMI** to save the state of an EC2 instance.
- **Use Launch Template** when you want repeatable, parameterized EC2 configurations.
- Often used **together** — a launch template references an AMI.

---

## 🔧 Use Cases for AMIs

- Creating golden images (pre-installed software)
- Blue/green deployments
- Creating consistent dev/test environments
- Migrating applications between regions or accounts

---

> ✨ AMIs simplify deployment and consistency across environments. Pair with Launch Templates for scalable automation.

