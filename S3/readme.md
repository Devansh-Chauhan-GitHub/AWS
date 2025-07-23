# AWS S3 Full Documentation

## 📌 Table of Contents

1. [What is Amazon S3?](#what-is-amazon-s3)
2. [Creating an S3 Bucket](#creating-an-s3-bucket)
3. [Enabling Versioning](#enabling-versioning)
4. [Hosting a Static Website on S3](#hosting-a-static-website-on-s3)
5. [Cross-Region Replication (CRR)](#cross-region-replication-crr)
6. [Best Practices](#best-practices)

---

## What is Amazon S3?

Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance.

Use Cases:

* Static website hosting
* Data backup and restore
* Disaster recovery
* Data archiving

---

## Creating an S3 Bucket

1. **Go to AWS Console → S3 → Create Bucket**
2. **Bucket Name**: Choose a globally unique name (e.g., `my-static-site-devops`)
3. **Region**: Choose your preferred region
4. **Block Public Access**: Uncheck to allow public access (for static website hosting)
5. **Enable Versioning**: (Optional here – covered in next section)
6. **Create Bucket**

---

## Enabling Versioning

Versioning helps you preserve, retrieve, and restore every version of every object stored in an S3 bucket.

1. Go to your bucket
2. Click on **Properties**
3. Scroll to **Bucket Versioning** → Click **Edit**
4. Enable versioning and click **Save changes**

Effect:

* Every PUT creates a new version of the object
* DELETE becomes a delete marker

---

## Hosting a Static Website on S3

1. Go to the bucket → **Properties**
2. Scroll down to **Static Website Hosting** → Click **Edit**
3. Enable it, set:

   * Index document: `index.html`
   * Error document: `error.html`
4. Save changes

### Upload Files

1. Upload `index.html` and `error.html` to the bucket
2. Set public access permissions:

   * Bucket Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

3. Save and access via **S3 Static Website Endpoint**

---

## Cross-Region Replication (CRR)

1. Enable versioning in both **source** and **destination** buckets
2. Go to **Management** tab in source bucket → Click **Replication rules** → **Create replication rule**

### Rule Setup:

* Name: `replicate-to-other-region`
* Status: Enabled
* Source: Entire bucket
* Destination: Choose destination bucket in another region
* Permissions:

  * Create an IAM role or use an existing one

### Effects:

* Any new object or new version is copied to the destination bucket
* Useful for compliance, disaster recovery, and latency optimization

---

## Best Practices

* Enable versioning for critical data
* Use lifecycle policies to transition older versions to Glacier or delete them
* Use logging and AWS CloudTrail to track access
* Restrict public access unless hosting a static website
* Use bucket policies/IAM policies for fine-grained access control
* Use replication for DR

---

## ✅ Summary

| Feature        | Purpose                    |
| -------------- | -------------------------- |
| Versioning     | Maintain object history    |
| Static Website | Host HTML/CSS/JS content   |
| Replication    | Backup & disaster recovery |

Let me know if you'd like to extend this with:

* Terraform or AWS CLI version
* IAM policy best practices
* Real-world use cases with diagrams

