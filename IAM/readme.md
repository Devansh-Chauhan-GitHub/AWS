# 📛 AWS IAM Complete Guide

This documentation covers everything you need to understand and set up **IAM (Identity and Access Management)** in AWS, including:

- IAM Users
- IAM Groups
- IAM Policies
- IAM Roles
- Differences between Roles and Policies
- Best Practices

---

## 🌐 What is IAM?

**AWS IAM (Identity and Access Management)** is a service that allows you to securely manage access to AWS services and resources. IAM is used to:

- Create and manage AWS users and groups.
- Grant permissions using policies.
- Delegate access using roles.

---

## 👤 IAM Users

- An **IAM user** is an entity you create in AWS to represent a person or application.
- A user can have **long-term credentials** (username/password, access keys).
- Each user is unique and can be assigned permissions via **policies**.

### ✅ How to Create IAM User (Console)

1. Go to IAM → Users → Add users.
2. Enter username.
3. Select access type: Programmatic access / AWS Management Console access.
4. Attach policies directly or add to a group.
5. Review & create.

---

## 👥 IAM Groups

- An **IAM Group** is a collection of IAM users.
- Policies attached to a group apply to all users in the group.
- Simplifies permission management.

### ✅ How to Create IAM Group (Console)

1. Go to IAM → User groups → Create group.
2. Enter group name.
3. Attach one or more policies.
4. Add users to the group.

---

## 📄 IAM Policies

- **Policies** are JSON documents that define **what actions are allowed or denied**, on which resources, and under what conditions.
- Can be attached to:
  - Users
  - Groups
  - Roles

### 𞷾 Example: Policy to Allow S3 Read Access

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "s3:GetObject"
    ],
    "Resource": "arn:aws:s3:::your-bucket-name/*"
  }]
}
```

---

## 🧑‍🚀 IAM Roles

- An **IAM Role** is an AWS identity **with permissions but no credentials**.
- Used for temporary access by:
  - AWS services (e.g., EC2, Lambda)
  - Other AWS accounts
  - Federated users (SSO, Identity Federation)

### ✅ Use Case:

- An EC2 instance accessing an S3 bucket securely without hardcoding credentials.

### 𞷾 Example: Trust Policy for EC2 Role

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}
```

---

## 🆖 IAM Policies vs IAM Roles

| Feature        | IAM Policy                            | IAM Role                                             |
| -------------- | ------------------------------------- | ---------------------------------------------------- |
| Definition     | Document that defines permissions     | Entity with permissions, assumed by trusted entities |
| Use            | Assign permissions to user/group/role | Temporary access to AWS resources                    |
| Authentication | Needs user credentials                | Assumes temporary security credentials               |
| Attachment     | Attached to users, groups, roles      | Assumed by users, services, accounts                 |
| Example        | Allow S3 access to user               | EC2 assuming a role to access S3                     |

---

## ✅ IAM Best Practices

- ❌ **Never use the root user** for daily tasks.
- ✅ **Enable MFA (Multi-Factor Authentication)** for all users.
- ✅ **Use groups** to manage permissions.
- ✅ **Grant least privilege access** — only what is necessary.
- ✅ **Rotate access keys** regularly.
- ✅ **Use IAM Roles for applications/services**, not users with hardcoded credentials.
- ✅ **Monitor IAM activity** using AWS CloudTrail.

---

## 📂 Suggested Folder Structure for GitHub

```
AWS-IAM-DOCS/
├── README.md
├── policies/
│   ├── s3-read-policy.json
│   └── trust-policy.json
```

---

## 🧠 Tip for Revision

Bookmark this GitHub repo and keep updating it with:

- Real IAM policies you use.
- Error resolutions.
- Key insights and concepts.

---

> 🔒 **Secure access is the foundation of a safe cloud environment. Master IAM to master AWS!**

