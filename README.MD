# Portfolio Deployment using AWS S3, CloudFront, GitHub Actions and Custom Domain

This project demonstrates how to deploy a **static portfolio website**
using **AWS S3 + CloudFront CDN + ACM SSL + Custom Domain (Hostinger) +
GitHub Actions CI/CD**.

The portfolio is publicly accessible at:

**https://www.techprerana.cloud**

This setup provides a **secure, scalable, automated, and
production-ready hosting architecture**.

------------------------------------------------------------------------

# Architecture

User\
↓\
CloudFront (CDN + HTTPS)\
↓\
AWS S3 (Static Website Hosting)\
↑\
GitHub Actions (CI/CD Deployment)\
↑\
GitHub Repository

------------------------------------------------------------------------

# Technologies Used

-   AWS S3 -- Static website hosting\
-   AWS CloudFront -- Global CDN distribution\
-   AWS Certificate Manager (ACM) -- SSL certificate for HTTPS\
-   Hostinger -- Domain & DNS management\
-   GitHub -- Source code repository\
-   GitHub Actions -- CI/CD pipeline for automated deployment

------------------------------------------------------------------------

# Deployment Steps

## 1 Upload Portfolio to AWS S3

1.  Create an **S3 bucket**
2.  Upload portfolio files (HTML, CSS, JS, assets)
3.  Enable **Static Website Hosting**
4.  Configure:

Index document: `index.html`

------------------------------------------------------------------------

## 2 Create CloudFront Distribution

1.  Open **AWS CloudFront**
2.  Create a new **Distribution**
3.  Select the **S3 bucket as origin**
4.  Configure:

Viewer protocol policy → Redirect HTTP to HTTPS\
Default root object → `index.html`

CloudFront provides **global CDN caching and faster content delivery**.

------------------------------------------------------------------------

## 3 Create SSL Certificate (ACM)

1.  Go to **AWS Certificate Manager**
2.  Request a **public certificate**
3.  Add domain names:

techprerana.cloud\
www.techprerana.cloud

4.  Choose **DNS Validation**
5.  Add the **CNAME validation record** in Hostinger DNS

After validation, the certificate status becomes **Issued**.

------------------------------------------------------------------------

## 4 Configure CloudFront with Custom Domain

In CloudFront Distribution settings:

Add **Alternate Domain Names (CNAME)**

techprerana.cloud\
www.techprerana.cloud

Attach the **ACM SSL certificate** created earlier.

------------------------------------------------------------------------

## 5 Configure DNS in Hostinger

Update the DNS records in Hostinger:

  Type            Name   Points To
  --------------- ------ -----------------------
  CNAME           www    dxxxxx.cloudfront.net
  ALIAS / CNAME   @      dxxxxx.cloudfront.net

This connects your **custom domain to CloudFront**.

------------------------------------------------------------------------

# CI/CD Automation using GitHub Actions

To automate deployments, **GitHub Actions** is used to upload files to
S3 whenever changes are pushed to the repository.

### CI/CD Workflow

Developer pushes code\
↓\
GitHub Repository\
↓\
GitHub Actions Workflow\
↓\
Sync files to S3 Bucket\
↓\
Invalidate CloudFront Cache\
↓\
Updated website deployed automatically

------------------------------------------------------------------------

# GitHub Actions Workflow

Create the file:

`.github/workflows/deploy.yml`

``` yaml
name: Deploy Portfolio to AWS

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-south-1

      - name: Upload files to S3
        run: |
          aws s3 sync . s3://your-bucket-name --delete

      - name: Invalidate CloudFront cache
        run: |
          aws cloudfront create-invalidation           --distribution-id YOUR_DISTRIBUTION_ID           --paths "/*"
```

------------------------------------------------------------------------

# Required GitHub Secrets

In **GitHub → Repository → Settings → Secrets → Actions**, add:

  Secret Name                  Description
  ---------------------------- ----------------------------------
  AWS_ACCESS_KEY_ID            AWS IAM access key
  AWS_SECRET_ACCESS_KEY        AWS IAM secret key
  AWS_REGION                   AWS region (example: ap-south-1)
  S3_BUCKET_NAME               Your S3 bucket name
  CLOUDFRONT_DISTRIBUTION_ID   CloudFront distribution ID

------------------------------------------------------------------------

# Live Website

https://www.techprerana.cloud

------------------------------------------------------------------------

# Final Result

✔ Portfolio deployed on AWS\
✔ Custom domain connected\
✔ HTTPS enabled\
✔ Global CDN via CloudFront\
✔ Automated deployment using GitHub Actions\
✔ Production-ready CI/CD pipeline

------------------------------------------------------------------------

# Author

**Prerana Lambhate**

Aspiring **Cloud & DevOps Engineer**
