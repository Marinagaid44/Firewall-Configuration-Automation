# UFW Firewall Manager

## Overview

This project provides a script to manage UFW (Uncomplicated Firewall) rules across multiple servers. It allows users to:
- Apply UFW rules to servers
- Remove/reset UFW rules on servers
- Check the status of UFW rules

The script is designed to automate UFW configuration using a simple and efficient approach, making it ideal for managing firewall settings across multiple servers.

---

## Features

1. **Automated UFW Management**:
   - Install UFW if not already installed.
   - Reset UFW rules.
   - Set default UFW policies (deny incoming, allow outgoing).
   - Apply custom rules from a configuration file.
   - Enable UFW with applied rules.

2. **Customizable Configuration**:
   - Specify servers in an inventory file.
   - Define UFW rules in a separate ports configuration file.
   - Override default inventory, ports file, or SSH user.

3. **Remote Management**:
   - Use SSH to apply, remove, or check UFW rules on remote servers.

4. **Safe Execution**:
   - Uses `set -euo pipefail` for robust error handling.
   - Validates input files and arguments before execution.

---

## Prerequisites

- **Supported OS**: Ubuntu or other Linux distributions that support UFW.
- **Dependencies**:
  - `ufw` package installed on target servers.
  - SSH access to target servers.
  - `bash` shell on the machine running the script.
- **Files**:
  - `servers.txt`: A list of target servers (one per line).
  - `ports.conf`: A list of UFW rules to apply (one rule per line, e.g., `22/tcp` or `80`).

---

## Usage

### Step 1: Clone the Repository
```bash
git clone https://github.com/yourusername/ufw-firewall-manager.git
cd ufw-firewall-manager
```

### Step 2: Create Configuration Files
1. **Inventory File (`servers.txt`)**:
   - Add the IP addresses or hostnames of servers (one per line):
     ```
     192.168.1.1
     192.168.1.2
     ```

2. **Ports File (`ports.conf`)**:
   - Add UFW rules to apply (one per line):
     ```
     22/tcp
     80
     443
     ```

### Step 3: Run the Script

1. **Apply Rules**:
   ```bash
   ./firewall-manager.sh -a apply
   ```

2. **Check Status**:
   ```bash
   ./firewall-manager.sh -a status
   ```

3. **Remove/Reset Rules**:
   ```bash
   ./firewall-manager.sh -a remove
   ```

4. **Override Inventory or SSH User**:
   ```bash
   ./firewall-manager.sh -a apply -i my-servers.txt -u deploy
   ```

---

## Script Options

```bash
Usage: firewall-manager.sh -a ACTION [-i INVENTORY] [-p PORTS] [-u SSH_USER]

  -a ACTION      apply | remove | status
  -i INVENTORY   list of servers (default: servers.txt)
  -p PORTS       list of UFW rules (default: ports.conf)
  -u SSH_USER    user for SSH (default: root)
  -h             display this help
```

---

## Code Documentation

### Key Functions

1. **`run_remote()`**:
   - Executes a command over SSH on a remote server using the specified SSH user.

2. **`apply_rules()`**:
   - Installs UFW if needed.
   - Resets and configures UFW defaults.
   - Applies rules from the ports file.
   - Enables UFW.

3. **`remove_rules()`**:
   - Resets UFW and disables it on target servers.

4. **`status()`**:
   - Displays the current UFW rules on target servers.

5. **Argument Parsing**:
   - Uses `getopts` to handle script arguments (`-a`, `-i`, `-p`, `-u`).

### Error Handling
- `set -euo pipefail`: Ensures the script exits on errors, treats unset variables as errors, and catches pipeline failures.
- Validates the existence of inventory and ports files before execution.

---

## Example Configuration

### Inventory File (`servers.txt`)
```
192.168.1.1
192.168.1.2
```

### Ports File (`ports.conf`)
```
22/tcp
80
443
```

---

## Notes

- This script is designed for Ubuntu systems but may work on other Linux distributions with UFW support.
- Ensure SSH access is configured for the specified user on all target servers.

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.
