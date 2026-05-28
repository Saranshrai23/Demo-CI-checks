# Jenkins CI Checks Demo Project

## Author Information

| Field      | Details     |
| ---------- | ----------- |
| Author     | Saransh Rai |
| Created On | 27-05-2026  |
| Version    | 1.0         |

---

# 1. Introduction

This project demonstrates a complete Jenkins Continuous Integration (CI) Checks Pipeline using Python and GitHub integration.

The purpose of this demo is to show how Jenkins automatically validates code quality whenever developers push code to GitHub.

The pipeline performs:

* Syntax Validation
* Lint Validation
* Unit Testing

If any validation fails, Jenkins immediately stops the pipeline and marks the build as failed.

---

# 2. Project Overview

This project contains:

* Python Application
* Unit Test File
* Requirements File
* Jenkins Pipeline

Whenever code is pushed to GitHub:

1. Jenkins fetches the latest code
2. Creates Python virtual environment
3. Installs dependencies
4. Runs syntax checks
5. Runs lint checks
6. Runs unit tests
7. Marks build as Success or Failed

---

# 3. Folder Structure

```bash
jenkins-ci-checks-demo/
├── app.py
├── test_app.py
├── requirements.txt
└── Jenkinsfile
```

---

# 4. Project Setup

## Step 1: Create Project Directory

Run the following commands:

```bash
mkdir jenkins-ci-checks-demo
cd jenkins-ci-checks-demo
git init
```

---

## Step 2: Create app.py

Create the main Python application file.

### Command

```bash
cat > app.py << 'EOF'
def add(a, b):
    return a + b


print(add(2, 3))
EOF
```

### app.py Content

```python
def add(a, b):
    return a + b


print(add(2, 3))
```

---

## Step 3: Create test_app.py

Create the unit testing file.

### Command

```bash
cat > test_app.py << 'EOF'
from app import add


def test_add():
    assert add(2, 3) == 5
EOF
```

### test_app.py Content

```python
from app import add


def test_add():
    assert add(2, 3) == 5
```

---

## Step 4: Create requirements.txt

Create dependency file.

### Command

```bash
cat > requirements.txt << 'EOF'
pytest
flake8
EOF
```

### requirements.txt Content

```txt
pytest
flake8
```

---

## Step 5: Create Jenkinsfile

This file defines the Jenkins CI pipeline.

### Command

```bash
cat > Jenkinsfile << 'EOF'
pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Code checkout completed'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 --version
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Syntax Check') {
            steps {
                sh '''
                . venv/bin/activate
                python -m py_compile app.py
                '''
            }
        }

        stage('Lint Check') {
            steps {
                sh '''
                . venv/bin/activate
                flake8 app.py test_app.py
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }

        stage('Security Scan') {
            steps {
                sh '''
                . venv/bin/activate
                bandit -r . -x ./venv
                '''
            }
        }
    }

    post {
        success {
            echo 'CI checks passed successfully.'
        }
        failure {
            echo 'CI checks failed. Please fix the issue.'
        }
    }
}
EOF
```

---

# 5. Push Project to GitHub

Run the following commands:

```bash
git add .
git commit -m "Add Jenkins CI checks demo"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

---

# 6. Jenkins Job Configuration

## Step 1: Open Jenkins Dashboard

Go to Jenkins Dashboard.

---

## Step 2: Create Pipeline Job

Navigate to:

```text
New Item → Pipeline
```

Pipeline Name:

```text
jenkins-ci-checks-demo
```

---

## Step 3: Configure Pipeline

Use the following configuration:

| Setting        | Value                      |
| -------------- | -------------------------- |
| Definition     | Pipeline script from SCM   |
| SCM            | Git                        |
| Repository URL | Your GitHub Repository URL |
| Branch         | main                       |
| Script Path    | Jenkinsfile                |

---

## Step 4: Build Pipeline

Click:

```text
Save → Build Now
```

The first build should pass successfully.

---

# 7. CI Checks Workflow

The Jenkins pipeline performs the following validations:

| Stage                    | Purpose                    |
| ------------------------ | -------------------------- |
| Checkout Code            | Fetch latest code          |
| Setup Python Environment | Create Python environment  |
| Syntax Check             | Validate Python syntax     |
| Lint Check               | Validate coding standards  |
| Unit Test                | Validate application logic |

---

# 8. Demo Scenario 1 – Syntax Check Failure

## Introduce Syntax Error

Update app.py with syntax issue.

### app.py

```python
def add(a, b):
    return a + b


print(add(2, 3)
```

---

## Push Changes

```bash
git add app.py
git commit -m "Add syntax error"
git push
```

---

## Jenkins Result

Pipeline will fail at:

```text
Syntax Check
```

---

## Fix Syntax Error

### app.py

```python
def add(a, b):
    return a + b


print(add(2, 3))
```

---

## Push Fix

```bash
git add app.py
git commit -m "Fix syntax error"
git push
```

Pipeline becomes green again.

---

# 9. Demo Scenario 2 – Lint Check Failure

## Introduce Lint Errors

### app.py

```python
import os

def add(a,b):
    return a+b


print( add(2,3) )
```

---

## Push Changes

```bash
git add app.py
git commit -m "Add lint errors"
git push
```

---

## Jenkins Result

Pipeline will fail at:

```text
Lint Check
```

---

## Fix Lint Errors

### app.py

```python
def add(a, b):
    return a + b


print(add(2, 3))
```

---

## Push Fix

```bash
git add app.py
git commit -m "Fix lint errors"
git push
```

Pipeline becomes green again.

---

# 10. Demo Scenario 3 – Unit Test Failure

## Introduce Logic Bug

### app.py

```python
def add(a, b):
    return a - b


print(add(2, 3))
```

---

## Push Changes

```bash
git add app.py
git commit -m "Add logic bug"
git push
```

---

## Jenkins Result

Pipeline will fail at:

```text
Unit Test
```

---

## Final Fix

### app.py

```python
def add(a, b):
    return a + b


print(add(2, 3))
```

---

## Push Final Fix

```bash
git add app.py
git commit -m "Fix logic bug"
git push
```

Pipeline becomes green successfully.

---

# 11. Validation Commands

| Validation        | Command                       |
| ----------------- | ----------------------------- |
| Syntax Validation | `python -m py_compile app.py` |
| Lint Validation   | `flake8 app.py test_app.py`   |
| Unit Testing      | `pytest`                      |

---

# 12. Demo Explanation for Presentation

This Jenkins pipeline performs CI checks automatically whenever code is pushed to GitHub.

The pipeline validates:

1. Syntax Errors
2. Coding Standards
3. Application Logic

If any validation fails:

* Jenkins immediately stops the pipeline
* Build becomes Red/Failed
* Developer fixes the issue
* Code is pushed again
* Pipeline becomes Green/Successful

This demonstrates how Jenkins acts as a centralized CI quality gate in DevOps environments.

---

# 13. Advantages of CI Checks

| Advantage            | Benefit                         |
| -------------------- | ------------------------------- |
| Automated Validation | Reduces manual work             |
| Faster Bug Detection | Early issue detection           |
| Better Code Quality  | Standardized code               |
| Reliable Builds      | Prevents faulty code deployment |
| Team Collaboration   | Common quality standards        |

---

# 14. Conclusion

This project successfully demonstrates a Jenkins CI Checks Pipeline integrated with GitHub and Python.

The demo proves how Jenkins automatically validates:

* Syntax
* Linting
* Unit Testing

This ensures only validated and tested code passes through the CI pipeline.

---

# 15. Contact Information

| Field  | Details                                                                         |
| ------ | ------------------------------------------------------------------------------- |
| Author | Saransh Rai                                                                     |
| Email  | [saransh.rai.snaatak@mygurukulam.co](mailto:saransh.rai.snaatak@mygurukulam.co) |
