# CI/CD Pipeline with Jenkins & SonarQube

Automated CI/CD pipeline that builds, tests, and scans a Python application for code quality on every Git push.

## What This Does

```
GitHub Push → Webhook → Jenkins Pipeline → Run Tests → SonarQube Scan → Build Artifact
```

Every time code is pushed to this repo, Jenkins automatically:
1. Pulls the latest code
2. Runs unit tests
3. Sends the code to SonarQube for static analysis (bugs, code smells, security issues)
4. Reports pass/fail status

## Architecture

```
┌──────────┐     webhook      ┌──────────────┐     scan      ┌─────────────┐
│  GitHub   │ ──────────────→ │   Jenkins    │ ────────────→ │  SonarQube  │
│   Repo    │                 │  (Pipeline)  │               │  (Scanner)  │
└──────────┘                  └──────────────┘               └─────────────┘
                                     │
                                     ↓
                              ┌──────────────┐
                              │ Build Report │
                              │  (Pass/Fail) │
                              └──────────────┘
```

## Tech Stack
- **Jenkins** — CI/CD automation server with Jenkinsfile pipeline
- **SonarQube** — Static code analysis (code quality gate)
- **Python** — Application being built and tested
- **GitHub Webhooks** — Triggers pipeline on every push

## Setup

### Prerequisites
- Jenkins server running (I used Ubuntu VM)
- SonarQube server running (separate Docker container or VM)
- Git, Python 3 installed on Jenkins node

### Steps

1. **Start Jenkins & install plugins:**
   ```bash
   # Jenkins should have these plugins installed:
   # - Pipeline
   # - Git
   # - SonarQube Scanner
   ```

2. **Configure SonarQube in Jenkins:**
   - Go to Jenkins → Manage Jenkins → Configure System
   - Add SonarQube server URL and authentication token

3. **Set up GitHub webhook:**
   - In your GitHub repo → Settings → Webhooks
   - Payload URL: `http://<jenkins-ip>:8080/github-webhook/`
   - Content type: `application/json`

4. **Create Pipeline job:**
   - New Item → Pipeline
   - Point to this repo's `Jenkinsfile`

### Jenkinsfile Overview
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout')    { /* Pull code from GitHub */ }
        stage('Test')        { /* Run Python unit tests */ }
        stage('SonarQube')   { /* Run code quality scan */ }
        stage('Build')       { /* Package application */ }
    }
}
```

## What I Learned
- How Jenkins pipelines work end-to-end (Jenkinsfile syntax, stages, agents)
- Setting up GitHub webhooks for automatic build triggers
- Integrating SonarQube for code quality gates
- Debugging pipeline failures through Jenkins console output
- Why CI/CD matters — catching bugs before they hit production

## Author
**Faizal Bagwan** — [LinkedIn](https://www.linkedin.com/in/faizalbagwan/) | [GitHub](https://github.com/faizalbagwan786/)
