# Day 25 — CI/CD Pipelines with GitHub Actions

Today I learned to automate everything:
- Automatically run tests on every push
- Automatically build Docker images
- Automatically deploy when tests pass
- No manual steps = no human errors

This is how DevOps teams actually work.

---

## What Is CI/CD

### CI = Continuous Integration

Every time code is pushed:
1. Automatically run tests
2. Automatically build
3. Report results

**Catches bugs immediately** (not days later)

### CD = Continuous Deployment

If tests pass:
1. Automatically deploy to staging
2. Run automated checks
3. Deploy to production

**New features live automatically** (if tests pass)

---

## The Pipeline

```
Developer pushes code
       ↓
GitHub detects push
       ↓
Trigger automated workflow
       ↓
Run tests
       ↓
If tests FAIL:
└─ STOP, notify developer
   (don't deploy broken code)
       ↓
If tests PASS:
├─ Build Docker image
├─ Push to registry
├─ Deploy to production
└─ Done!

Entire process: fully automated
```

---

## GitHub Actions

GitHub's built-in automation tool.

### Workflow File

```yaml
name: Test and Deploy

on:
  push:
    branches: [main]      # trigger on push to main

jobs:
  test:                   # job name
    runs-on: ubuntu-latest  # machine to run on
    
    steps:
      - uses: actions/checkout@v3
        # Clone your code
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      
      - name: Install dependencies
        run: pip install -r requirements.txt
      
      - name: Run tests
        run: pytest
```

### Key Concepts

```
Workflow    = Complete automation process
Job         = Unit of work (test, build, deploy)
Step        = Individual action (install, run, deploy)
Trigger     = When workflow starts (push, PR, schedule)
Secrets     = Encrypted credentials (passwords, SSH keys)
```

---

## Running Tests Automatically

### Workflow

```yaml
name: Test Suite

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11']
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}
      
      - name: Install dependencies
        run: pip install -r requirements.txt
      
      - name: Run tests
        run: pytest -v
```

### What This Does

```
Matrix strategy:
- Run tests on Python 3.9
- Run tests on Python 3.10
- Run tests on Python 3.11

If your app works on all 3, you know it's compatible!
```

---

## Building Docker Automatically

### Workflow

```yaml
name: Build Docker

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .
      
      - name: Test image
        run: docker run myapp:${{ github.sha }}
```

### What This Does

```
github.sha = commit hash (unique ID)
${{ github.sha }} = insert commit hash

Example:
docker build -t myapp:abc123def456 .

Each build has unique tag (the commit hash!)
Easy to track which version is deployed
```

---

## Deploying Automatically

### Workflow

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /opt/myapp
            git pull origin main
            docker-compose build
            docker-compose up -d
```

### Secrets

Store credentials in GitHub (encrypted):

```
GitHub Settings → Secrets and Variables → Actions

Add:
- HOST = your server IP
- USERNAME = SSH username
- SSH_KEY = your private SSH key

Access in workflow:
${{ secrets.HOST }}
${{ secrets.USERNAME }}
${{ secrets.SSH_KEY }}
```

---

## Pull Request Workflow

Pull request triggers CI/CD:

```
1. Create branch (feature-login)
2. Make changes
3. Push to GitHub
4. Create Pull Request
       ↓
GitHub automatically:
├── Runs tests
├── Builds Docker image
├── Checks code quality
└── Shows results on PR
       ↓
If all pass: ✅ Ready to merge
If any fail: ❌ Fix before merging
       ↓
Click "Merge"
       ↓
Deploy workflow runs
```

---

## Status Badges

Add to README.md:

```markdown
[![Test Suite](https://github.com/YOUR_USERNAME/YOUR_REPO/actions/workflows/test.yml/badge.svg)](https://github.com/YOUR_USERNAME/YOUR_REPO/actions/workflows/test.yml)
```

Shows:
- 🟢 Green = tests passing
- 🔴 Red = tests failing
- ⚪ Gray = never run

Visitors see at a glance: is this project healthy?

---


## Real Workflows

### Scenario 1: Testing

```yaml
- Push code
- Tests run automatically
- Results shown
- If fail: can't merge PR
- If pass: PR ready to merge
```

### Scenario 2: Build and Deploy

```yaml
- Push code
- Tests pass
- Build Docker image
- Push to Docker registry
- Deploy to production
- App is live
```

### Scenario 3: Scheduled Tasks

```yaml
on:
  schedule:
    - cron: '0 0 * * *'   # daily

jobs:
  backup:
    # Backup database daily
  cleanup:
    # Clean old logs daily
```

---

## Common Workflows

### Test Matrix
Test on multiple Python versions simultaneously

### Code Quality
Lint, format checks, security scans

### Performance Testing
Benchmark application performance

### Security Scanning
Check for vulnerabilities in dependencies

### Documentation
Build and deploy documentation site

### Coverage Reports
Track test coverage over time

---

## Debugging Workflows

Click "Actions" on GitHub:
- See all workflows
- Click workflow to see logs
- Expand each step to see output
- Look for error messages

If workflow fails:
```
1. Click the failed workflow
2. Click the failed job
3. Read the error message
4. Fix locally
5. Push again
```

---

## Things That Clicked

- CI/CD = automate testing and deployment
- GitHub Actions = workflows defined in YAML
- Triggers = workflows run on push, PR, schedule
- Matrix = run same tests on multiple versions
- Secrets = store credentials securely
- Status badge = show health of project
- Workflows fail fast = catch bugs immediately
- No manual deployment = no human errors

---

## CI/CD Benefits

```
Before:
- Developer: did I test everything?
- Developer: did I build the right version?
- Developer: is this the latest code?
- Mistakes happen regularly

With CI/CD:
- Tests run automatically
- Build happens automatically
- Deploy happens automatically
- Mistakes are caught before deployment
```

---

## Statistics

```
Day 25:
- Concepts: 4 (CI, CD, workflows, secrets)
- Workflow examples: 3 (test, build, deploy)
- Challenges: 4
- Production script: 1 complete setup
```

---