# AuraFitLife - Secure & High-Performance Static Web Architecture on AWS

A professional, production-ready cloud infrastructure hosting a secure, high-performance static website for **AuraFitLife**. This project demonstrates industry-standard cloud engineering practices for content delivery networks (CDNs) and object storage management.

## 🚀 Live Demo
🔗 **URL:** [https://aurafitlife.com](https://aurafitlife.com)

---

## 🏗️ Architecture Breakdown

This project utilizes a highly secure, serverless architecture to deliver static content globally with sub-millisecond latency.

* **Storage (Amazon S3):** Hosts the static source files (`index.html`, CSS, assets) securely in a private bucket. Direct public access to the bucket is blocked completely.
* **Content Delivery Network (Amazon CloudFront):** Acts as the global edge caching layer, serving content via Edge Locations to minimize latency for users worldwide.
* **Security (Origin Access Control - OAC):** Restricts access to the S3 bucket so that files can *only* be fetched via designated CloudFront distributions, protecting the origin from direct exposition.
* **Domain & SSL (AWS Certificate Manager & Route 53):** Integrated a custom domain (`aurafitlife.com`) with automated SSL/TLS termination for secure `HTTPS` traffic.

---

## 🛠️ Tech Stack & AWS Services Used

* **Amazon S3** (Simple Storage Service)
* **Amazon CloudFront** (Global CDN)
* **AWS Origin Access Control** (OAC Secure Policy)
* **AWS Certificate Manager** (ACM for SSL/TLS)
* **Route 53 / Custom DNS** (Domain mapping)

---

## 🔍 Key Implementations & Troubleshooting Highlights

* **MIME-Type & Content-Type Resolution:** Resolved complex browser rendering anomalies by overriding metadata constraints, ensuring strict `text/html` compliance over default system transport streams.
* **Cache Invalidation Pipelines:** Implemented wild-card edge invalidation policies (`/*`) to force synchronous updates across global edge servers instantaneously upon asset deployment.

---

## 🔐 IAM Identity & Access Management (S3 Bucket Policy)

To enforce strict security boundaries, an explicit IAM Bucket Policy was applied to the private S3 bucket. This policy allows **only** the specified CloudFront Distribution ID to fetch objects using **Origin Access Control (OAC)**, while explicitly denying all other direct public internet traffic.

### Production IAM Bucket Policy Configuration:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipalReadOnly",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::azam-aws-portfolio-2026/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::YOUR_AWS_ACCOUNT_ID:distribution/YOUR_CLOUDFRONT_DISTRIBUTION_ID"
                }
            }
        }
    ]
}
