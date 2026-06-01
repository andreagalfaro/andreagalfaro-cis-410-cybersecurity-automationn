# Week 9 Security Audit — cis410-deploy-sa

**Project:** <!-- 410cis-andreaga -->
**Date:** <!-- 05/31/26 -->
**Auditor:** <!-- Andrea Gonzalez Alfaro -->

---

## 1. IAM Audit Results

### Before — Week 8 Configuration (over-permissioned)

| Role | Scope | Problem |
|---|---|---|
| roles/run.admin | Project | Overly broad — grants ability to delete services and modify IAM, not just deploy |
| roles/storage.admin | Project | Overly broad — grants access to ALL GCS buckets in the project |
| roles/artifactregistry.writer | Project | Acceptable — scoped to push images only |
| roles/viewer | Project | Acceptable — read-only project metadata |
| roles/iam.serviceAccountUser | Compute SA | Required — needed to act as Compute Engine default SA |

### After — Week 9 Least-Privilege Fix

| Role | Scope | Why Sufficient |
|---|---|---|
| roles/run.developer | Project | Deploy only — cannot delete services or modify IAM |
| roles/storage.admin | tfstate bucket only | Scoped to one bucket — not all storage |
| roles/artifactregistry.writer | Project | Unchanged — push images only |
| roles/viewer | Project | Unchanged — read project metadata |
| roles/iam.serviceAccountUser | Compute SA | Unchanged — required for Cloud Run deployment |

---

## 2. Secret Manager Migration

- **Secret created:** `flask-app-secret`
- **Replication:** automatic
- **Access granted to:** `cis410-deploy-sa` — roles/secretmanager.secretAccessor on this secret only
- **Access granted to:** `PROJECT_NUMBER-compute@developer.gserviceaccount.com` — roles/secretmanager.secretAccessor on this secret only (required for Cloud Run runtime access)
- **Cloud Run update:** APP_SECRET environment variable mounted from Secret Manager at runtime

---

## 3. Monitoring Configuration

- **Log-based alert:** `cis410-flask-app-alert` — fires on severity>=WARNING for cis410-flask-app
- **Notification channel:** <!-- andreagalfaro@students.highline.edu-->
- **Billing budget:** `cis410-monthly-budget` — $20 limit, alerts at 50% / 90% / 100%

---

## 4. Reflection

**Q1: Why is roles/run.admin inappropriate for a CI/CD pipeline service account?**

## The roles/run.admin is way too broad, and gives the most permissions, such as deleting services, modfiying IAM on services, and also able to change up traffic splits. This is too much permission for someone that is just the run.admin.

---

**Q2: What is the security difference between storing a secret in GitHub Secrets vs. Google Secret Manager?**

## Github secrets encrypts the secrets that you manually add, but it is difficult if there is rotation needed or if you need to change the values, as well as who can have access to these secrets. Versus GCP, there is more security; a couple examples are: adui logs, able to view whoever has accessed this, "Fine-Grained IAM", which only allows specific account to read certain secrets.


**Q3: A coworker says "I will clean up IAM permissions after the project launches. For now I need everything to work fast." What is the risk of this approach?**

## The risk to this approach is certain accounts having full access to services that they should not have access to. This poses risk to what one might do, or even 'accidentally' change. It is best to implement the IAM permissions before launching to ensure that the rightful accounts are assigned to their specific permissions to reduce any risk.
