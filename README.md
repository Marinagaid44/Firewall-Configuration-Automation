# IPtables Firewall Manager Script

## Project Overview

This project provides a Bash script to manage and apply `iptables` firewall rules across multiple remote servers. It offers a simple and efficient way to configure consistent firewall settings using SSH. The script is ideal for system administrators who need to automate firewall management tasks across multiple Linux systems.

---

## Features**

1. Automated Firewall Management:
   - Backs up existing `iptables` rules on the target servers to `/root/` with a timestamp.
   - Applies predefined firewall rules using `iptables-restore`.
   - Verifies and displays the `INPUT` chain after applying the rules.

2. Predefined Rules**:
   - Drops all incoming and forwarded traffic by default.
   - Allows all outgoing traffic.
   - Permits incoming traffic on specific ports:
     - `22` (SSH)
     - `80` (HTTP)
     - `443` (HTTPS)
   - Includes rules for loopback (`lo`) traffic and established/related connections.

3. Remote Management:
   - Uses SSH to connect to and manage multiple servers.
   - Supports key-based authentication for secure and automated access.

4. Safe Execution:
   - Ensures existing firewall rules are backed up before applying changes.
   - Displays success or failure messages for each server.

---

## Prerequisites

1. **Supported Environment**:
   - Target servers must have `iptables` installed.
   - The local machine should have `bash` installed to run the script.

2. SSH Configuration:
   - SSH key-based authentication must be configured between the local machine and the target servers.
   - Ensure the SSH user (`root` by default) has the necessary permissions to manage `iptables`.

3. Dependencies:
   - `iptables`, `iptables-save`, and `iptables-restore` must be installed on all target servers.

---

## Setup Instructions

### Step 1: Prepare the Script
1. Save the script as `firewall-manager.sh`:
   ```bash
   nano firewall-manager.sh
   ```
   Copy and paste the script into the file and save it.

2. Make the script executable:
   ```bash
   chmod +x firewall-manager.sh
   ```

---

### Step 2: Configure the Script
1. Set the SSH User and Private Key:
   - Update the `SSH_USER` variable with the SSH user (default: `root`).
   - Update the `SSH_KEY` variable with the path to the private SSH key.

2. Define the Target Servers:
   - Add the IP addresses or hostnames of the target servers to the `SERVERS` array:
     ```bash
     SERVERS=(
       "192.0.2.10"
       "192.0.2.11"
       "server03.example.com"
     )
     ```

3. Review the Firewall Rules:
   - Modify the `IPTABLES_RULES` block to change the firewall rules as needed.
   - The default rules allow SSH, HTTP, and HTTPS traffic while blocking all other incoming traffic.

---

### Step 3: Run the Script
1. Execute the script to apply rules to all servers in the `SERVERS` array:
   ```bash
   ./firewall-manager.sh
   ```

2. Check the output for success or failure messages for each server.

---

## How It Works

1. The script loops through each server in the `SERVERS` array.
2. For each server:
   - The script connects via SSH using the specified `SSH_USER` and `SSH_KEY`.
   - It backs up existing `iptables` rules on the server to `/root/iptables-<timestamp>.bak`.
   - It applies the new firewall rules using `iptables-restore`.
   - Displays the `INPUT` chain to verify the applied rules.
3. After processing all servers, it prints a final success message:
   ```
   ✔ The firewall configuration has been successfully applied to all servers.
   ```

---

## Troubleshooting

1. SSH Connection Issues:
   - Ensure the SSH private key path is correct and accessible.
   - Verify that the target servers are reachable and have SSH enabled.
   - Add the target server to the `known_hosts` file manually if prompted:
     ```bash
     ssh-keyscan -H <server> >> ~/.ssh/known_hosts
     ```

2. Permission Issues:
   - Ensure the `SSH_USER` has sufficient privileges to manage `iptables` (e.g., root or a sudo-enabled user).

3. Firewall Rule Errors:
   - Test the rules on a single server manually before applying them across multiple servers:
     ```bash
     echo "$IPTABLES_RULES" | sudo iptables-restore
     sudo iptables -L INPUT
     ```

   4.Debugging:
   - Add `-x` to the script execution for detailed logs:
     ```bash
     bash -x ./firewall-manager.sh
     ```

---

## Additional Notes
- This script is designed to manage `iptables` on Linux systems and may not work on systems without `iptables`.
- Always test the script on a staging server before applying it to production servers.
- Misconfigured firewall rules can lock you out of the server. Ensure you have console or recovery access before executing the script.

---

## License
This script is open-source and can be modified or extended as needed. 
