# Wazuh SIEM with VirusTotal Threat Intelligence Integration

## Overview

This project demonstrates the deployment of Wazuh SIEM on Ubuntu Server 22.04.5 LTS, the enrollment of a Windows endpoint as a Wazuh Agent, and the integration of VirusTotal Threat Intelligence for file reputation analysis.

## Objectives

- Install Wazuh on Ubuntu Server
- Connect a Windows endpoint
- Configure VirusTotal API integration
- Verify threat intelligence alerts


## Step 1: Download Wazuh Installation Script

Downloaded the official Wazuh installation script from the Wazuh package repository to begin the deployment process.

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
```

![Step 1](screenshots/01-system-update-and-upgrade.png)

## Step 2: Download the Wazuh Installation Script

Download the official Wazuh installation script from the Wazuh package repository using the curl command.

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
```

![Step 2](screenshots/02-download-wazuh-installer.png)

## Step 3: Install Wazuh Platform

Run the installation script to deploy the Wazuh Manager, Indexer, and Dashboard in an all-in-one installation.

```bash
sudo bash wazuh-install.sh -a
```

![Step 3](screenshots/03-install-wazuh-platform.png)

## Step 4: Access the Wazuh Dashboard

After the installation is completed successfully, open the Wazuh Dashboard in a web browser using the server IP address and log in with the generated administrator credentials.

![Step 4](screenshots/04-wazuh-dashboard-login.png)

## Step 5: Verify the Dashboard

The Wazuh Dashboard loads successfully. At this stage, no endpoint is connected, so the dashboard initially shows that no agents are registered.

![Step 5](screenshots/05-wazuh-dashboard-overview.png)

## Step 6: Deploy a Windows Agent

From the Wazuh Dashboard, open the Agent Deployment page and select Windows as the target operating system. The dashboard automatically generates the installation command.

![Step 6](screenshots/06-deploy-windows-agent.png)

## Step 7: Install the Windows Agent

Run the generated PowerShell command as Administrator on the Windows host to install and register the Wazuh Agent.

```powershell
# Generated PowerShell installation command from the Wazuh Dashboard
```

![Step 7](screenshots/07-windows-agent-installation.png)

## Step 8: Verify the Agent Connection

After installation, the Windows endpoint successfully connects to the Wazuh Manager and appears as an active agent.

![Step 8](screenshots/08-agent-successfully-connected.png)

## Step 9: Configure VirusTotal API Integration

Edit the Wazuh configuration file and add the VirusTotal integration block containing the API key.

Configuration File:

```text
/var/ossec/etc/ossec.conf
```

![Step 9](screenshots/09-virustotal-api-key-configuration.png)

## Step 10: Restart the Wazuh Manager

Restart the Wazuh Manager service to apply the new VirusTotal configuration.

```bash
sudo systemctl restart wazuh-manager
```

![Step 10](screenshots/10-restart-wazuh-manager-service.png)

## Step 11: Verify the Wazuh Manager Status

Check that the Wazuh Manager service is running correctly after the restart.

```bash
sudo systemctl status wazuh-manager
```

![Step 11](screenshots/11.1-verify-wazuh-manager-status.png)

## Step 12: Verify the VirusTotal Configuration

Confirm that the VirusTotal integration block has been successfully added to the Wazuh configuration file.

```bash
sudo grep -i virustotal /var/ossec/etc/ossec.conf
```

![Step 12](screenshots/12-verify-virustotal-configuration.png)

## Step 13: Generate a Test Event

Create a text file inside a monitored directory on the Windows endpoint. Wazuh detects the new file, calculates its hash, and sends it to VirusTotal for reputation analysis.

![Step 13](screenshots/13-create-test-file-on-agent.png)

## Step 14: Verify the VirusTotal Alert

Open the Discover page in the Wazuh Dashboard and search using the following query:

```text
rule.groups:virustotal
```

The dashboard displays the VirusTotal response. Since the test file is newly created, VirusTotal reports **"No records in VirusTotal database,"** confirming that the integration is working correctly.

![Step 14](screenshots/14-virustotal-alert-verification.png)

