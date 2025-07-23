# 📀 AWS EBS (Elastic Block Store) Guide

This guide provides step-by-step instructions to work with **AWS EBS (Elastic Block Store)** including:

- Creating and attaching EBS volumes
- Mounting EBS volumes to EC2 instances
- Detaching and deleting EBS volumes
- Creating and managing snapshots
- Copying EBS snapshots to another region

---

## 💼 What is Amazon EBS?

Amazon EBS provides **block-level storage volumes** for use with EC2 instances. It is ideal for:

- Databases
- Filesystems
- Applications requiring persistent storage

Key Features:

- Persistent (data survives instance termination if EBS is not deleted)
- Can be detached and attached to other instances
- Can be snapshotted and restored

---

## 📦 Step 1: Create an EBS Volume

1. Open the **EC2 Dashboard** in AWS Console.
2. Go to **Elastic Block Store > Volumes**.
3. Click on **Create Volume**.
4. Choose:
   - Volume type (e.g., gp3 for general purpose)
   - Size (e.g., 8 GiB)
   - Availability Zone (MUST match your EC2 instance's AZ)
5. Click **Create Volume**.

---

## ➕ Step 2: Attach EBS Volume to EC2 Instance

1. After the volume is created, select it.
2. Click **Actions > Attach Volume**.
3. Choose the instance ID you want to attach it to.
4. Keep the device name default (e.g., `/dev/xvdf`) or modify if needed.
5. Click **Attach**.

---

## 🌐 Step 3: Mount EBS Volume to EC2 (Linux)

Once attached, SSH into the EC2 instance:

### 1. Confirm the disk:

```bash
lsblk
```

You should see the new volume (e.g., `/dev/xvdf`).

### 2. Format the disk:

```bash
sudo mkfs -t ext4 /dev/xvdf
```

### 3. Create a mount point:

```bash
sudo mkdir /data
```

### 4. Mount the volume:

```bash
sudo mount /dev/xvdf /data
```

### 5. Make mount persistent (optional):

Edit `/etc/fstab` and add:

```
/dev/xvdf   /data   ext4   defaults,nofail   0   2
```

---

## ❌ Step 4: Detach and Delete EBS Volume

### To Detach:

1. Unmount the volume from EC2:

```bash
sudo umount /data
```

2. Go to EC2 Console → Volumes.
3. Select the volume → **Actions > Detach Volume**.

### To Delete:

1. After detaching, select the volume.
2. Click **Actions > Delete Volume**.

**Note:** Data will be lost permanently unless a snapshot is taken before deletion.

---

## 📊 Step 5: Create a Snapshot of EBS Volume

1. Go to **EC2 Console > Volumes**.
2. Select the volume you want to snapshot.
3. Click **Actions > Create Snapshot**.
4. Provide a description.
5. Click **Create Snapshot**.

Use cases:

- Backup
- Disaster recovery
- Launch new volumes in other regions

---

## 🔹 Step 6: Copy EBS Snapshot to Another Region

1. Go to **EC2 > Snapshots**.
2. Select the snapshot.
3. Click **Actions > Copy Snapshot**.
4. Select the destination region.
5. Set encryption if needed.
6. Click **Copy Snapshot**.

### Restore in Another Region:

1. Switch to the target region.
2. Go to **Snapshots** and find the copied one.
3. Click **Actions > Create Volume** from it.
4. Attach it to an EC2 instance in the new region.

---

## 🔧 Common Commands (Linux EC2)

```bash
# Check attached devices
lsblk

# Format volume
sudo mkfs -t ext4 /dev/xvdf

# Create mount point
sudo mkdir /data

# Mount volume
sudo mount /dev/xvdf /data

# Unmount
sudo umount /data
```

---

## 🔒 Best Practices

- Tag your volumes for easy identification.
- Take regular snapshots.
- Monitor IOPS and throughput for performance.
- Use **gp3** for better cost-performance.
- Use encrypted volumes for sensitive data.

---

> ⚡ Mastering EBS gives you power to scale, backup, and protect your AWS workloads with confidence!

