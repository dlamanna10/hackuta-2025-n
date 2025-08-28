# GitHub Actions Setup

This repository includes 7 GitHub Actions workflows that run **sequentially** in the following order:

## Workflows Created

## Sequential Workflow Order:

### 1. 🔓 Gatekeeper (`gatekeeper.yml`)
- **Trigger**: On PR open, sync, or reopen (pull_request_target)
- **Purpose**: Auto-approves workflow runs to bypass manual approval requirement
- **Runs**: First - enables all other workflows to run

### 2. 🏷️ Label PRs (`auto-labeler.yml`)  
- **Trigger**: After Gatekeeper completes successfully
- **Purpose**: Automatically labels PRs based on changed files
- **Labels Applied**:
  - `ci` - Config files, workflows, package.json
  - `javascript` - .js, .jsx files
  - `typescript` - .ts, .tsx files  
  - `json` - JSON data files
  - `css` - Styles and CSS files
  - `assets` - Images, fonts, public files
  - `doc` - Markdown documentation

### 3. 🧪 All Test Workflows (Run in Parallel)
These all trigger **after Label PRs completes**:

#### 3a. 🧹 Lint Check (`lint.yml`)
- **Purpose**: Runs ESLint validation
- **Features**: Uses pnpm, comments on PR with results

#### 3b. 🏗️ Build Check (`build.yml`) 
- **Purpose**: Ensures application builds successfully
- **Features**: TypeScript checking, Next.js build, uploads artifacts

#### 3c. 🔒 Security Check (`security-check.yml`)
- **Purpose**: Scans for vulnerabilities and security issues
- **Features**: npm audit, CodeQL analysis, weekly scheduled scans

#### 3d. 🚀 Vercel Deployment (`vercel.yml`)
- **Purpose**: Handles preview/production deployments
- **Features**: Preview deployments for PRs, graceful skip if not configured

### 4. 📊 Status Reporter (`status-reporter.yml`)
- **Trigger**: **Runs LAST** - after all test workflows complete
- **Purpose**: Provides consolidated reporting of all workflow results
- **Features**:
  - Creates comprehensive status table
  - Shows final results of all workflows
  - Updates with real-time status and durations

## Labels to Create

Create these labels in your GitHub repository:
- `CI Changes` (color: #0052cc)
- `JavaScript Changes` (color: #f1e05a)
- `JSON Changes` (color: #292929)
- `Styles Changes` (color: #563d7c)
- `Assets Changes` (color: #89e051)
- `Documentation Changes` (color: #083fa1)

All workflows are independent and will run based on their specific triggers.
