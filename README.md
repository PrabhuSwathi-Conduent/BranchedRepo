# 🤖 BranchedRepo — Katalon Studio Automation Project

> Automated testing project using **Katalon Studio** (Groovy) with a structured Git branching strategy and GitHub Actions CI/CD pipeline.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Tech Stack](#-tech-stack)
- [Repository Structure](#-repository-structure)
- [Branching Strategy](#-branching-strategy)
- [Branch Naming Conventions](#-branch-naming-conventions)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Environment & Execution Profiles](#-environment--execution-profiles)
- [Getting Started](#-getting-started)
- [GitHub Secrets Setup](#-github-secrets-setup)
- [Contributing Guidelines](#-contributing-guidelines)

---

## 📌 Project Overview

This repository contains automated test scripts built with **Katalon Studio** using **Groovy** language. It follows a structured **Git Flow** branching strategy mapped to multiple deployment environments (dev, qa, staging, production).

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| **Katalon Studio** | Test automation framework |
| **Groovy** | Scripting language |
| **GitHub Actions** | CI/CD pipeline |
| **Chrome** | Default browser for execution |

---

## 📁 Repository Structure

```
BranchedRepo/
 ├── .github/
 │    └── workflows/
 │         └── automation.yml       ← CI/CD pipeline
 ├── Test Cases/                    ← Individual test cases
 ├── Test Suites/
 │    ├── Smoke/                    ← Smoke test suite
 │    └── Regression/               ← Full regression suite
 ├── Object Repository/             ← Page objects / locators
 ├── Profiles/
 │    ├── dev.glbl                  ← Dev environment profile
 │    ├── qa.glbl                   ← QA environment profile
 │    ├── staging.glbl              ← Staging environment profile
 │    └── prod.glbl                 ← Production environment profile
 ├── Keywords/                      ← Custom Groovy keywords
 ├── Reports/                       ← Test reports (gitignored)
 └── README.md
```

---

## 🌿 Branching Strategy

This project follows **Git Flow** with environment-mapped branches.

```
main          ←── Production-ready, stable automation suite
  │
develop       ←── Integration branch (all features merged here)
  │
  ├── feature/TC-101-login-tests     ←── New test cases / modules
  ├── fix/TC-202-broken-locator      ←── Fixes for failing scripts
  ├── refactor/page-object-update    ←── Framework improvements
  └── release/v1.2.0                ←── Release freeze & validation
       │
  hotfix/critical-regression         ←── Emergency fix on main
```

### Branch → Environment Mapping

| Branch | Environment | Test Suite Triggered |
|---|---|---|
| `feature/**` | DEV | 🔥 Smoke Tests |
| `fix/**` | DEV | 🔥 Smoke Tests |
| `develop` | QA | 🔁 Full Regression |
| `release/**` | STAGING | 🧪 Full Regression |
| `main` | PRODUCTION | 🏁 Smoke Tests (with approval) |

---

## 🏷 Branch Naming Conventions

| Type | Format | Example |
|---|---|---|
| Feature | `feature/<ticket-id>-<short-desc>` | `feature/TC-101-login-tests` |
| Bug Fix | `fix/<ticket-id>-<short-desc>` | `fix/TC-202-broken-locator` |
| Refactor | `refactor/<short-desc>` | `refactor/page-object-cleanup` |
| Release | `release/v<major>.<minor>.<patch>` | `release/v1.2.0` |
| Hotfix | `hotfix/<short-desc>` | `hotfix/critical-login-regression` |

---

## 🚀 CI/CD Pipeline

The pipeline is defined in `.github/workflows/automation.yml` and is triggered automatically based on the branch being pushed.

### Pipeline Flow

```
Push to feature/** or fix/**
        │
        ▼
  🔥 Smoke Tests (DEV profile)
        │
        ▼
  Pull Request → develop
        │
        ▼
  🔁 Regression Tests (QA profile)
        │
        ▼
  Create release/** branch
        │
        ▼
  🧪 Staging Tests (STAGING profile)
        │
        ▼
  Merge to main (with approval gate)
        │
        ▼
  🏁 Production Smoke Tests (PROD profile)
```

### Job Summary

| Job | Trigger | Suite | Profile |
|---|---|---|---|
| 🔥 Smoke Tests | `feature/**`, `fix/**` push | `Test Suites/Smoke` | `dev` |
| 🔁 Regression Tests | `develop` push | `Test Suites/Regression` | `qa` |
| 🧪 Staging Tests | `release/**` push | `Test Suites/Regression` | `staging` |
| 🏁 Production Smoke | `main` push | `Test Suites/Smoke` | `prod` |
| 🖐 Manual Run | `workflow_dispatch` | Configurable | Configurable |

### Manual Trigger

You can manually trigger a test run from **GitHub Actions → Katalon Studio Automation CI/CD → Run workflow** and select:
- **Test Suite** to execute
- **Target environment** (`dev` / `qa` / `staging` / `prod`)

---

## 🌍 Environment & Execution Profiles

Configure environment-specific values (base URLs, credentials) inside Katalon Execution Profiles:

| Profile | Environment | Usage |
|---|---|---|
| `dev` | Development | feature/fix branch runs |
| `qa` | QA / Testing | develop branch runs |
| `staging` | Staging | release branch runs |
| `prod` | Production | main branch runs |

> ⚠️ Never hardcode credentials. Use Katalon Execution Profiles and GitHub Secrets.

---

## ⚡ Getting Started

### Prerequisites

- [Katalon Studio](https://katalon.com/download) installed locally
- Git installed
- Access to this GitHub repository

### Clone the Repository

```bash
git clone https://github.com/PrabhuSwathi-Conduent/BranchedRepo.git
cd BranchedRepo
```

### Create a Feature Branch

```bash
git checkout develop
git pull origin develop
git checkout -b feature/TC-101-your-test-name
```

### Run Tests Locally (Katalon CLI)

```bash
katalonc -noSplash \
  -runMode=console \
  -projectPath="$(pwd)" \
  -testSuitePath="Test Suites/Smoke" \
  -browserType="Chrome" \
  -executionProfile="dev" \
  -apiKey=<YOUR_KATALON_API_KEY>
```

---

## 🔐 GitHub Secrets Setup

The CI/CD pipeline requires the following secret to be configured:

| Secret Name | Description |
|---|---|
| `KATALON_API_KEY` | Your Katalon Studio API key |

### How to Add

1. Go to **Settings → Secrets and variables → Actions**
2. Click **"New repository secret"**
3. Name: `KATALON_API_KEY`
4. Value: *(your Katalon API key from [katalon.com](https://katalon.com))*

### Production Approval Gate

To enable manual approval before production runs:
1. Go to **Settings → Environments → New environment**
2. Name it `production`
3. Add **Required reviewers**

---

## 🤝 Contributing Guidelines

1. **Always branch from `develop`** — never commit directly to `main` or `develop`
2. **Follow naming conventions** — e.g., `feature/TC-101-login-tests`
3. **Keep branches short-lived** — merge within 2–5 days
4. **Open a Pull Request** for every merge into `develop`
5. **Ensure CI passes** before requesting a review
6. **Link your PR** to the relevant test case / ticket ID
7. **Delete branch** after merging

### PR Checklist

```
[ ] Branch follows naming convention
[ ] Tests pass locally
[ ] No hardcoded credentials or URLs
[ ] Execution Profile used for environment config
[ ] CI pipeline passes on this branch
[ ] PR description includes test case IDs
```

---

## 📊 Test Reports

Test reports are automatically uploaded as **GitHub Actions Artifacts** after each run.

| Branch | Retention |
|---|---|
| `feature/**` / `fix/**` | 7 days |
| `develop` | 14 days |
| `release/**` | 30 days |
| `main` | 90 days |

To download reports:
1. Go to **Actions → select a workflow run**
2. Scroll to **Artifacts** section
3. Download the report zip file

---

## 📞 Support

For issues with the automation framework or pipeline, please open a [GitHub Issue](https://github.com/PrabhuSwathi-Conduent/BranchedRepo/issues).

---

*Maintained by [@PrabhuSwathi-Conduent](https://github.com/PrabhuSwathi-Conduent)*
