# Technical Documentation
## 📦 Rafikey Project – CI/CD Pipeline Documentation

## Overview

This document outlines the GitHub Actions + Render CI/CD pipeline used for deploying and testing the **Rafikey SRHR AI Platform**, including both **staging** and **production** environments.

---

## ⚙️ Architecture Summary

### CI/CD Platform:
- **GitHub Actions**: Used for automated testing, linting, and deployment.
- **Render.com**: Hosts both staging (Free Tier) and production (Paid) services.

### Services:
- `rafikey-backend`: Python (FastAPI or Flask)
- `rafikey-admin`: Node.js (Admin dashboard)
- `rafikeyAIChatbot`: Node.js (Chat UI)
- `rafikeyAIWeb`: Node.js (Marketing site)
- `rafikeyWebPush-backend`: Node.js (Push service)

---

## 🚀 Workflow Behavior

| Branch | Environment | Auto Deploys | Plan |
|--------|-------------|--------------|------|
| `dev`  | Staging     | ✅ Yes       | Render Free Plan |
| `main` | Production  | ✅ Yes       | Render Paid Plan |

---

## 🔄 Pipeline Steps (per service)

1. **Trigger**:
   - Push to `dev` → triggers staging workflow
   - Push to `main` → triggers production workflow

2. **CI/CD Jobs**:
   - 🔍 Lint code (Python: `flake8`, Node: `eslint`)
   - 🧪 Run tests (Python: `pytest`, Node: `npm test`)
   - 🛠 Build (Node only: `npm run build`)
   - 🚀 Deploy via Render API

3. **Deployment Rules**:
   - ❌ If tests fail → deployment is blocked
   - ✅ Successful test → deploys to target Render service

---

## 🔐 Secrets Management

Secrets are managed via GitHub → **Settings → Secrets → Actions**

### Required Secrets:

| Secret Name | Description |
|-------------|-------------|
| `RENDER_API_KEY` | API key from Render |
| `RAFIKEY_BACKEND_STAGING_SERVICE_ID` | ID of staging backend service |
| `RAFIKEY_BACKEND_PROD_SERVICE_ID` | ID of production backend |
| (Repeat for each service) | `RAFIKEY_ADMIN_*`, etc. |

---

## 🧪 Render Environment Setup

### For Each Service:
- Create **2 Render services**:
  - `*-staging`: use **Free Plan**
  - `*-production`: use **Starter or Pro Plan**

### Find Service ID:
- Open service in Render
- Look at URL: `https://dashboard.render.com/web/srv-xxxxx`
- Copy `srv-xxxxx` → use as `SERVICE_ID`

---

## 🔍 CI/CD Folder Structure in Repo

```bash
.github/
└── workflows/
    ├── rafikey-backend-staging.yml
    ├── rafikey-backend-prod.yml
    ├── rafikey-admin-staging.yml
    ├── rafikey-admin-prod.yml
    └── ...
```

---

## 🛠️ Maintenance Tips

- ✅ Use `dev` for all new features and testing
- 🔁 Merge into `main` only after confirming staging works
- 🧯 If staging is sleeping (free plan), wait 15-30s on first load
- 💡 Keep secrets updated if services are recreated

---

## ✅ Summary

This CI/CD pipeline ensures:
- Safe testing in staging
- Reliable deploys to production
- Cost-effective setup using free tier for non-critical environments
- Easy rollback and transparency via GitHub Actions

---

_This document is maintained by the Mama Tech team_ 🔧🤖
