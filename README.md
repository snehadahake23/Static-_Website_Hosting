# 🌐 Static Website Hosting on AWS S3

![AWS](https://img.shields.io/badge/AWS-S3-orange?style=for-the-badge&logo=amazon-aws)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)

A modern, responsive static website hosted on Amazon S3, demonstrating cost-effective and scalable web hosting solutions using AWS cloud infrastructure.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Deployment](#deployment)
- [Project Structure](#project-structure)
- [Cost Estimation](#cost-estimation)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project showcases how to host a static website on Amazon S3, one of the most cost-effective and reliable hosting solutions available. The website features a clean, modern design with responsive layouts suitable for portfolios, landing pages, or business websites.

## ✨ Features

- **Lightning Fast**: Static files served directly from S3 with minimal latency
- **Highly Scalable**: Automatic scaling to handle traffic spikes
- **Cost Effective**: Pay only for storage and bandwidth used
- **Responsive Design**: Mobile-first approach ensuring great UX across devices
- **Modern UI**: Clean interface with smooth animations and transitions
- **Easy Maintenance**: Simple file updates without server management

## 🏗️ Architecture

```
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │
       │ HTTPS
       │
┌──────▼──────────┐
│   Amazon S3     │
│   Bucket        │
│  (Static Host)  │
└─────────────────┘
```

**Optional Enhancements:**
- CloudFront CDN for global content delivery
- Route 53 for custom domain management
- Certificate Manager for SSL/TLS

## 📦 Prerequisites

Before you begin, ensure you have:

- An AWS account ([Sign up here](https://aws.amazon.com/))
- AWS CLI installed and configured
- Basic knowledge of AWS S3
- Git installed on your machine

## 🚀 Setup Instructions

### Step 1: Create an S3 Bucket

```bash
# Replace 'your-website-bucket-name' with your unique bucket name
aws s3 mb s3://your-website-bucket-name --region us-east-1
```

### Step 2: Enable Static Website Hosting

```bash
aws s3 website s3://your-website-bucket-name \
  --index-document index.html \
  --error-document error.html
```

### Step 3: Configure Bucket Policy

Create a file named `bucket-policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-website-bucket-name/*"
    }
  ]
}
```

Apply the policy:

```bash
aws s3api put-bucket-policy \
  --bucket your-website-bucket-name \
  --policy file://bucket-policy.json
```

### Step 4: Disable Block Public Access

```bash
aws s3api put-public-access-block \
  --bucket your-website-bucket-name \
  --public-access-block-configuration \
  "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

## 📤 Deployment

### Manual Upload

1. **Via AWS Console:**
   - Navigate to your S3 bucket
   - Click "Upload"
   - Add `index.html`, `styles.css`, and other assets
   - Click "Upload"

2. **Via AWS CLI:**

```bash
# Upload all files
aws s3 sync . s3://your-website-bucket-name --exclude ".git/*" --exclude "README.md"
```

### Automated Deployment (GitHub Actions)

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to S3

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      - name: Deploy to S3
        run: aws s3 sync . s3://your-website-bucket-name --delete
```

## 📁 Project Structure

```
static-website-s3/
│
├── index.html          # Main HTML file
├── styles.css          # Stylesheet
├── images/             # Image assets
├── js/                 # JavaScript files (if any)
├── README.md           # Project documentation
└── bucket-policy.json  # S3 bucket policy
```

## 💰 Cost Estimation

**Monthly Cost Breakdown** (assuming 10,000 visitors):

| Service | Usage | Estimated Cost |
|---------|-------|----------------|
| S3 Storage | 1 GB | $0.023 |
| S3 Requests | 50,000 GET | $0.02 |
| Data Transfer | 10 GB | $0.90 |
| **Total** | | **~$0.95/month** |

*Note: Costs may vary based on region and actual usage.*

## 🔧 Troubleshooting

### Website Not Loading

- **Check bucket policy**: Ensure the bucket is publicly accessible
- **Verify static hosting**: Confirm it's enabled in bucket properties
- **Clear cache**: Try accessing with `Ctrl + Shift + R`

### 403 Forbidden Error

```bash
# Verify bucket policy is applied
aws s3api get-bucket-policy --bucket your-website-bucket-name
```

### CSS Not Loading

- Check file paths in HTML (case-sensitive)
- Ensure all files are uploaded to the bucket
- Verify Content-Type metadata

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🌟 Acknowledgments

- AWS Documentation for S3 Static Hosting
- Community contributions and feedback

## 📞 Contact

Project Link: [https://github.com/yourusername/static-website-s3](https://github.com/yourusername/static-website-s3)

Website URL: `http://your-website-bucket-name.s3-website-us-east-1.amazonaws.com`

---

**Made with ❤️ and hosted on AWS S3**x
