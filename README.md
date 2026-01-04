# ☁️ KumoCMS
### Enterprise-Grade Multi-Region Serverless Content Management

KumoCMS is an open-source, high-performance headless CMS built for global scale. By leveraging AWS Serverless primitives, it provides a "zero-ops" experience with built-in multi-region resilience, perfect for document management and content delivery in high-availability environments.

---

## ✨ Key Features

- 🌍 **Multi-Region Active-Active**: Built-in support for DynamoDB Global Tables and S3 Cross-Region Replication for sub-second data synchronization.
- ⚡ **Ultra-Low Latency**: Global traffic routing via S3 Multi-Region Access Points (MRAP) and Route 53 latency-based records.
- 📦 **Unlimited File Sizes**: Bypasses the 10MB API Gateway limit using S3 Pre-signed URLs for direct, secure client-side uploads.
- 🛡️ **Security First**: Native integration with AWS WAF for rate limiting and Amazon Cognito for robust JWT-based authentication.
- 💰 **Pay-as-you-go**: 100% Serverless. No servers to patch, no idle costs.

---

## 🏗️ Architecture

KumoCMS runs entirely on AWS managed services:

- **Frontend**: React hosted on S3 + CloudFront.
- **Compute**: Python-based AWS Lambda + API Gateway (Regional/Edge).
- **Database**: DynamoDB Global Tables (NoSQL).
- **Storage**: S3 with Multi-Region Access Points.

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.9+** (For Lambda functions)
- **Node.js** (v18+) (For React frontend development)
- **AWS CLI** configured with Administrator access.
- **Terraform**

### 1. Clone the repository

```bash
git clone https://github.com/KumoCMS/kumocms.github.io.git
cd kumocms.github.io
```

### 2. Deploy Infrastructure

```bash
cd infra/terraform
terraform init
terraform apply -var="primary_region=ap-northeast-1" -var="secondary_region=ap-northeast-3"
```

### 3. Start Local Development

```bash
cd client
npm install
npm run dev
```

---

## 🤝 Contributing

We love contributions! Whether it's a bug report, a new feature, or better documentation, please feel free to open an issue or submit a pull request.

For more details, please see our [**Contributing Guidelines**](./CONTRIBUTING.md).

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## ⚖️ License

Distributed under the MIT License. See [**LICENSE**](./LICENSE) for more information.

---

### 📺 Resources
- **[Setting Up Cross-Region Replication for S3 Buckets](https://www.youtube.com/watch?v=dQw4w9WgXcQ)**: A practical guide to configuring S3 replication rules, a core component of the KumoCMS architecture.