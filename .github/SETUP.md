# GitHub Actions Setup for DAB Bundle Validation

This guide explains how to set up GitHub Actions to automatically validate your Declarative Automation Bundle (DAB).

## Overview

The workflow automatically validates your DAB bundle configuration on:
* **Push** to `main` or `dev` branches
* **Pull requests** to `main` or `dev` branches
* **Manual trigger** via GitHub Actions UI

## Setup Instructions

### 1. Create GitHub Secrets

You need to add two secrets to your GitHub repository:

1. Go to your repository: `https://github.com/prasaducan-cell/dltframework`
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** and add:

#### Required Secrets:

**`DATABRICKS_HOST`**
* **Value:** `https://dbc-ce550af6-f726.cloud.databricks.com`
* **Description:** Your Databricks workspace URL

**`DATABRICKS_TOKEN`**
* **Value:** Your personal access token
* **Description:** Databricks authentication token

### 2. Generate a Databricks Token

To create a personal access token:

1. Log in to your Databricks workspace
2. Click your profile icon (top-right) → **Settings**
3. Navigate to **Developer** → **Access tokens**
4. Click **Generate new token**
5. Give it a name (e.g., "GitHub Actions - DAB Validation")
6. Set expiration (recommended: 90 days for security)
7. Click **Generate**
8. **Copy the token immediately** (you won't see it again!)

### 3. Add Secrets to GitHub

Paste the values into GitHub secrets:
* `DATABRICKS_HOST`: `https://dbc-ce550af6-f726.cloud.databricks.com`
* `DATABRICKS_TOKEN`: Your generated token (starts with `dapi...`)

### 4. Commit and Push

```bash
git add .github/workflows/validate-bundle.yml
git commit -m "Add GitHub Actions workflow for DAB validation"
git push origin main
```

### 5. Verify Workflow

1. Go to your repository on GitHub
2. Click the **Actions** tab
3. You should see the "Validate DAB Bundle" workflow
4. The workflow will run automatically on the next push/PR

## Workflow Details

### What the Workflow Does:

1. **Checkout** - Clones your repository
2. **Setup Python** - Installs Python 3.10
3. **Install Databricks CLI** - Downloads and installs the latest CLI
4. **Configure CLI** - Sets up authentication using your secrets
5. **Validate Bundle** - Runs `databricks bundle validate --target dev`
6. **Display Summary** - Shows bundle information if validation succeeds

### Manual Trigger

You can also trigger the workflow manually:
1. Go to **Actions** tab
2. Select "Validate DAB Bundle"
3. Click **Run workflow**
4. Select branch and click **Run workflow**

## Troubleshooting

### Validation Errors

If validation fails, check:
* `databricks.yml` syntax is correct
* All referenced files exist
* Resource configurations are valid
* Secrets are set correctly

### Authentication Errors

If you see authentication errors:
* Verify `DATABRICKS_HOST` matches your workspace URL
* Ensure `DATABRICKS_TOKEN` is valid and not expired
* Check token has necessary permissions

### Need Help?

For issues or questions:
* Check workflow logs in the Actions tab
* Review error messages in the validation step
* Ensure your bundle validates locally first: `databricks bundle validate`

## Security Best Practices

* **Never commit tokens** to the repository
* **Rotate tokens regularly** (every 90 days recommended)
* **Use least privilege** - Token should only have permissions needed for validation
* **Monitor token usage** in Databricks Settings → Access tokens

## Next Steps

Once validation passes, you can extend the workflow to:
* Deploy to dev environment on merge to `dev` branch
* Deploy to prod environment on merge to `main` branch
* Run bundle tests
* Generate deployment reports
