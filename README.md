# README.md for rmtechtransfer1/github-actions-security

# GitHub Actions Security Starter Workflows

This repository contains workflow templates that help you get started with security-focused GitHub Actions. These templates are presented in the GitHub Actions workflow creation UI to help you quickly set up security scanning for your projects.

## Available Workflow Templates

### CI Workflows (Build + Security)

#### **CMake with Security Scanning**
- **File**: `ci/cmake-security.yml`
- **Use for**: C++ projects using CMake that need security analysis
- **Features**: Build, test, and run Clang-tidy security analysis
- **Requires**: CMakeLists.txt, C++ source files

#### **Python Security CI** 
- **File**: `ci/python-security.yml`
- **Use for**: Python projects that need comprehensive testing and security
- **Features**: pytest testing, flake8 linting, Bandit security scanning
- **Requires**: Python files, optionally requirements.txt

#### **CMake on a single platform**
- **File**: `ci/cmake-single-platform.yml` 
- **Use for**: Standard C++ CMake projects (from GitHub's official starters)
- **Features**: Build and test with CMake/CTest
- **Requires**: CMakeLists.txt

### Code Scanning Workflows

#### **Bandit Security Scanner**
- **File**: `code-scanning/bandit.yml`
- **Use for**: Python security analysis with SARIF output
- **Features**: Runs Bandit security linter, uploads results to GitHub Security tab
- **Requires**: Python files

## How to Use

### Method 1: From GitHub UI (Recommended)
1. Go to your repository on GitHub
2. Click the **"Actions"** tab
3. Click **"New workflow"**
4. Look for workflows from **rmtechtransfer1** organization:
   - "CMake with Security Scanning"
   - "Python Security CI"
   - "Bandit"

### Method 2: Copy and Customize
Copy the workflow files from this repository to your project's `.github/workflows/` directory and customize as needed.

## Quick Start Examples

### For a C++ Project
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  BUILD_TYPE: Release

jobs:
  build-and-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Install security tools
      run: |
        sudo apt-get update
        sudo apt-get install -y clang-tidy
    # ... rest follows cmake-security.yml pattern
```

### For a Python Project
```yaml
# .github/workflows/security.yml
name: Security Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test-and-scan:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write
    steps:
    - uses: actions/checkout@v4
    - name: Set up Python 3.11
      uses: actions/setup-python@v5
      with:
        python-version: "3.11"
    # ... rest follows python-security.yml pattern
```

### For Microservices (C++ + Python)
Combine both templates in a single workflow - see examples in the artifacts above.

## 🔧 Customization

### Environment Variables
Customize these variables in your workflows:

```yaml
env:
  BUILD_TYPE: Release        # CMake build type
  PYTHON_VERSION: "3.11"     # Python version
  CLANG_TIDY_CHECKS: "-*,security-*"  # Clang-tidy check filters
  BANDIT_SEVERITY: "medium"  # Bandit severity level
```

### Security Tool Configuration

#### Clang-tidy
Create `.clang-tidy` in your repo root:
```yaml
# .clang-tidy
Checks: 'readability-*,security-*,performance-*,-readability-magic-numbers'
WarningsAsErrors: 'security-*'
```

