# AWS Systems Manager (SSM)

AWS Systems Manager (SSM) is a management service that helps you automate, monitor, configure, and operate AWS resources at scale.

It provides a centralized way to manage EC2 instances, on-premises servers, virtual machines, and other AWS resources without needing direct SSH or RDP access.

---

## Why Use Systems Manager?

Managing hundreds of servers manually can be difficult:

- Logging into each server individually
- Running software updates
- Applying configuration changes
- Collecting inventory information
- Troubleshooting issues

Systems Manager simplifies these tasks through a single management interface.

---

## How It Works

```text
Administrator
      │
      ▼
AWS Systems Manager
      │
      ▼
SSM Agent
      │
 ┌────┴────┐
 ▼         ▼
EC2      On-Prem Servers
```

For an instance to be managed by SSM:

1. SSM Agent must be installed.
2. The instance must have an IAM role attached.
3. The IAM role should include:

```text
AmazonSSMManagedInstanceCore
```

4. The instance must be able to communicate with AWS Systems Manager endpoints.

---

## Core Components

### Session Manager

Provides secure shell access to instances without:

- SSH keys
- Bastion hosts
- Opening port 22

Example:

```bash
aws ssm start-session --target i-1234567890abcdef0
```

---

### Run Command

Execute commands remotely on one or many instances.

Example:

```bash
sudo yum update -y
```

Run once and apply across multiple servers.

---

### Patch Manager

Automates operating system patching.

Features:

- Scan instances for missing updates
- Install patches automatically
- Generate compliance reports
- Schedule maintenance windows

Supported platforms:

- Amazon Linux
- Ubuntu
- Windows Server
- macOS

---

### Fleet Manager

Provides a centralized dashboard for managed instances.

View:

- Operating System
- Installed software
- Network configuration
- File system details
- Performance information

---

### Inventory

Collect metadata from managed instances.

Examples:

- Installed applications
- Operating system details
- Network configuration
- Hardware information

Useful for compliance and auditing.

---

### Parameter Store

Secure storage for configuration values.

Examples:

```text
/database/hostname
/database/password
/application/environment
```

Benefits:

- Centralized configuration management
- Encryption support
- Version control
- Access control using IAM

---

### Automation

Automate operational tasks using predefined documents.

Examples:

- Restart EC2 instances
- Create AMI backups
- Patch servers
- Configure networking

AWS provides built-in automation documents such as:

```text
AWS-RunPatchBaseline
AWS-RunShellScript
AWS-RestartEC2Instance
```

---

## Common Workflow Example

### Step 1: Create IAM Role

Attach:

```text
AmazonSSMManagedInstanceCore
```

### Step 2: Launch EC2 Instance

Attach the IAM role to the instance.

### Step 3: Verify Instance Appears in Fleet Manager

```text
Systems Manager
 └── Fleet Manager
```

### Step 4: Connect Using Session Manager

```text
EC2
 └── Connect
      └── Session Manager
```

### Step 5: Run Commands

```text
Systems Manager
 └── Run Command
```

Execute commands across multiple instances simultaneously.

### Step 6: Patch Instances

```text
Systems Manager
 └── Patch Manager
      └── Scan and Install
```

---

## Advantages

✅ No SSH keys required

✅ No open inbound ports

✅ Centralized management

✅ Automated patching

✅ Configuration management

✅ Inventory collection

✅ Operational automation

✅ Supports AWS and on-premises servers

---

## Common AWS Managed Documents

| Document | Purpose |
|-----------|----------|
| AWS-RunShellScript | Execute Linux shell commands |
| AWS-RunPowerShellScript | Execute Windows commands |
| AWS-RunPatchBaseline | Patch operating systems |
| AWS-RestartEC2Instance | Restart EC2 instances |
| AWS-ConfigureAWSPackage | Install software packages |

---

## Typical Use Cases

### Server Administration

- Run commands remotely
- Troubleshoot systems
- Manage large fleets

### Security & Compliance

- Automated patching
- Inventory collection
- Configuration enforcement

### DevOps Automation

- Deployment tasks
- Server configuration
- Maintenance operations

### Cost Optimization

- Manage resources centrally
- Reduce operational overhead

---

## Summary

AWS Systems Manager (SSM) is a centralized management service that allows administrators to securely manage, patch, automate, and monitor EC2 instances and other managed nodes without direct server access.

Key features include:

- Session Manager
- Run Command
- Patch Manager
- Fleet Manager
- Inventory
- Parameter Store
- Automation

It is one of the most important services used by DevOps Engineers, Cloud Engineers, and Site Reliability Engineers (SREs) for managing infrastructure at scale.
