**# CI/CD Pipeline for React Blog Platform Deployment**

## **Overview**
This document outlines the CI/CD setup for deploying the **React Blog Platform** to **GitHub Pages** using GitHub Actions.

---

## **CI/CD Workflow**
### **1. Trigger Events**
- The workflow runs on **push** and **pull request** events for the `main` branch.

### **2. Build Process**
- Sets up Node.js environment.
- Installs dependencies.
- Builds the React application.

### **3. Deployment**
- Deploys the generated `build/` folder to the `gh-pages` branch using **GitHub Actions**.

---

## **GitHub Actions Configuration (`.github/workflows/ci-cd.yaml`)**
```yaml
name: React Blog Platform CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "16"

      - name: Install dependencies
        run: |
          npm install
          npm install bootstrap  # Ensure Bootstrap is installed
        working-directory: ./blog-platform

      - name: Build the app
        run: npm run build
        working-directory: ./blog-platform

      - name: Deploy to GitHub Pages
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          branch: gh-pages
          folder: blog-platform/build
          token: ${{ secrets.GH_TOKEN }}  # Uses a Personal Access Token (Optional Fix)
```

---

## **Required GitHub Repository Settings**
### **Option 1: Enable GitHub Actions Permissions**
1. Go to **Repository Settings → Actions → General**
2. Under **Workflow Permissions**, select:
   ✅ **"Read and write permissions"** (instead of "Read-only")
3. Click **Save**

### **Option 2: Enable GitHub Pages Deployment Permissions**
1. Go to **Repository Settings → Pages**
2. Ensure that **"GitHub Actions"** is allowed to deploy.
3. If there's an option to grant deployment permissions, enable it.

### **Option 3 (If Needed): Use Personal Access Token (PAT)**
1. Generate a **Personal Access Token (PAT)** with `repo` and `public_repo` permissions.
2. Go to **Repository Settings → Secrets and Variables → Actions**
3. Click **"New repository secret"**, name it: `GH_TOKEN`, and paste the PAT value.
4. Update the `ci-cd.yaml` file to use `GH_TOKEN`.

---

## **Set Up GitHub Pages (`gh-pages` branch)**
1. Go to **Repository Settings → Pages**
2. Under **Source**, select `gh-pages` branch.
3. Click **Save**.
4. Your site will be available at:
   ```
   https://shital-ghodake.github.io/blog-platform
   ```

---

## **Steps to Deploy**
1. Commit and push changes to `main` branch.
2. GitHub Actions will automatically build and deploy.
3. Ensure the **gh-pages branch is selected** in GitHub Pages settings.
4. Visit the deployed site at:
   ```
   https://shital-ghodake.github.io/blog-platform
   ```

---

## **Troubleshooting**
- If deployment fails due to **Bootstrap missing**, ensure it is installed: `npm install bootstrap`
- If **GitHub Pages deployment fails (403 Permission Denied)**:
  - Enable **Workflow Permissions** (Option 1)
  - Enable **GitHub Pages Deployment Permissions** (Option 2)
  - Use a **Personal Access Token (PAT)** (Option 3)
- If the site does not load correctly, ensure the **homepage path is correctly set** in `package.json`:
  ```json
  "homepage": "https://shital-ghodake.github.io/blog-platform"
  ```

---

This CI/CD setup ensures smooth and automated deployments of the **React Blog Platform** to GitHub Pages.

