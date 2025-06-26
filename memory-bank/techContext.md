+++
id = "mac-dev-playbook-tech-context"
title = "Mac Development Ansible Playbook Technical Context"
type = "tech-context"
status = "active"
created_date = "2025-06-26"
last_updated = "2025-06-26"
author = "Juliano Barbosa"
related_docs = ["memory-bank/projectbrief.md", "memory-bank/systemPatterns.md"]
tags = ["ansible", "automation", "technical", "dependencies", "tools", "setup"]
+++

# Mac Development Ansible Playbook Technical Context

## Technologies Used

### 1. Core Technologies

1. **Ansible**
   - Version: Latest stable
   - Role: Primary automation tool
   - Installation: via pip3
   - Dependencies: Python 3.x

2. **Python**
   - Version: 3.9+ (system Python)
   - Role: Required for Ansible
   - Location: `/Library/Python/3.9/bin`
   - Package Manager: pip3

3. **Homebrew**
   - Version: Latest
   - Role: Package manager
   - Components:
     - brew (CLI packages)
     - brew cask (GUI applications)

4. **mas-cli**
   - Version: Latest
   - Role: Mac App Store integration
   - Installation: via Homebrew

### 2. Supporting Technologies

1. **Shell Environments**
   - Bash (default)
   - Zsh support
   - Shell completion

2. **Version Control**
   - Git for playbook management
   - GitHub for distribution and CI

3. **Testing Tools**
   - GitHub Actions for CI
   - UTM/Tart for local VM testing
   - Homebrew Doctor for validation

## Development Setup

### 1. Prerequisites

1. **System Requirements**
   - macOS (Intel or Apple Silicon)
   - XCode Command Line Tools
   - Internet connection
   - Admin privileges

2. **Python Environment**
   - System Python 3.9+
   - pip3 (Python package manager)
   - PATH configuration

3. **Environment Variables**
   ```bash
   export PATH="$HOME/Library/Python/3.9/bin:/opt/homebrew/bin:$PATH"
   ```

### 2. Installation Steps

1. **Initial Setup**
   ```bash
   xcode-select --install
   sudo pip3 install --upgrade pip
   pip3 install ansible
   ```

2. **Repository Setup**
   ```bash
   git clone <repo>
   cd mac-dev-playbook
   ansible-galaxy install -r requirements.yml
   ```

3. **Configuration**
   - Copy default.config.yml to config.yml
   - Customize config.yml as needed
   - Verify inventory file

## Technical Constraints

### 1. System Constraints

1. **macOS Limitations**
   - Some settings require manual configuration
   - App Store requires manual login
   - System security restrictions
   - OS version compatibility

2. **Ansible Constraints**
   - Idempotency requirements
   - Module availability
   - Permission requirements
   - Shell environment limitations

3. **Network Constraints**
   - Internet required for package installation
   - Download bandwidth impact
   - Repository availability

### 2. Integration Constraints

1. **Package Management**
   - Homebrew formula availability
   - Cask application compatibility
   - App Store limitations
   - Version pinning constraints

2. **Configuration Management**
   - File permission requirements
   - User environment access
   - System preference restrictions
   - Application settings limitations

## Dependencies

### 1. Direct Dependencies

1. **Required Packages**
   ```yaml
   - ansible
   - python3
   - git
   - homebrew
   - mas
   ```

2. **Optional Dependencies**
   ```yaml
   - bash-completion
   - ansible-lint
   - yamllint
   ```

### 2. Package Dependencies

1. **Homebrew Packages**
   - Core utilities
   - Development tools
   - Language environments
   - System tools

2. **Cask Applications**
   - Development IDEs
   - Utilities
   - Browsers
   - Development tools

3. **App Store Applications**
   - Licensed software
   - System utilities
   - Development tools

## Tool Usage Patterns

### 1. Ansible Usage

1. **Playbook Execution**
   ```bash
   # Full installation
   ansible-playbook main.yml --ask-become-pass

   # Tagged execution
   ansible-playbook main.yml -K --tags "dotfiles,homebrew"
   ```

2. **Configuration Override**
   ```yaml
   # config.yml
   homebrew_installed_packages:
     - git
     - go

   mas_installed_apps:
     - { id: 497799835, name: "Xcode" }
   ```

### 2. Package Management

1. **Homebrew Operations**
   ```bash
   # Validation
   brew doctor

   # Updates
   brew update && brew upgrade
   ```

2. **App Store Integration**
   ```bash
   # Installation via mas
   mas install [app-id]

   # List installed apps
   mas list
   ```

### 3. Maintenance Patterns

1. **Update Process**
   ```bash
   # Update dependencies
   ansible-galaxy install -r requirements.yml --force

   # Run playbook
   ansible-playbook main.yml -K
   ```

2. **Testing Process**
   ```bash
   # Local testing
   ansible-playbook main.yml --check

   # CI testing
   GitHub Actions workflow
   ```

This document serves as the technical foundation for understanding the tools, dependencies, and patterns used in the project.