# Wazuh SIEM/XDR & File Integrity Monitoring (FIM) Lab

This repository contains a comprehensive guide for setting up an enterprise-wide SIEM/XDR platform using **Wazuh**. The lab demonstrates deploying a central Wazuh manager on an Ubuntu virtual machine, onboarding a Windows endpoint agent, and configuring File Integrity Monitoring (FIM).

## Objective
To provide a clear, step-by-step deployment and configuration guide for building an open-source SIEM/XDR platform using Wazuh, complete with Windows endpoint agent onboarding and Real-Time File Integrity Monitoring (FIM).

---

## Skills Learned
* **SIEM & XDR Deployment:** Architected and deployed an open-source Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) platform utilizing Wazuh.
* **Server & Infrastructure Management:** Installed, configured, and managed a Wazuh central server stack—including the manager, search indexer, and dashboard—on an Ubuntu Linux environment.
* **Endpoint Onboarding & Management:** Deployed lightweight security agents on Windows endpoints, configured agent connectivity, and established secure communication channels back to the central manager.
* **File Integrity Monitoring (FIM):** Configured real-time system monitoring policies using Syscheck to track file creations, modifications, and deletions on critical directories.
* **Log Analysis & Threat Detection:** Processed system events and application logs through centralized analysis rules to detect potential security anomalies, rootkits, and unauthorized changes.
* **Vulnerability & Compliance Assessment:** Utilized automated scanning capabilities to check software inventories against CVE databases and evaluated systems against compliance benchmarks.

---

## 1. Overview
* Wazuh is an open-source SIEM/XDR platform that provides enterprise-wide visibility and threat detection through a central server and lightweight endpoint agents.
* Setup involves installing the Wazuh server, which includes the manager, search indexer, and dashboard, then deploying agents on endpoints.
* Agents send system events and application logs for centralized analysis against detection rules.
* Wazuh monitors sensitive files for unauthorized changes or deletions, detects intrusions using signatures, rootkit checks, and anomaly detection, and scans software inventories against CVE databases for unpatched vulnerabilities.
* When threats or policy violations exceed severity thresholds, it sends real-time alerts to the dashboard or integrations such as Slack, email, and webhooks.

---

## 2. Lab Architecture

| Component | Host | Role |
| :--- | :--- | :--- |
| **Wazuh Manager** | Ubuntu (VMware) | Collects, analyzes, and stores data from agents |
| **Wazuh Agent** | Windows | Sends logs and system events to the Wazuh manager |

### 2.1 Server Requirements
* **RAM (Minimum):** 2 GB
* **RAM (Recommended):** 4 GB
* **CPU (Minimum):** 2 Cores
* **CPU (Recommended):** 8 Cores

---

## 3. Installing the Wazuh Manager (Ubuntu Server) 
For official instructions, refer to the [Wazuh Installation Guide](https://documentation.wazuh.com/current/installation-guide/wazuh-server/installation-assistant.html)[cite: 1].

Add the Wazuh GPG key:
   ```bash
   curl -s [https://packages.wazuh.com/key/GPG-KEY-WAZUH](https://packages.wazuh.com/key/GPG-KEY-WAZUH) | sudo gpg --dearmor -o /usr/share/keyrings/wazuh-archive-keyring.gpg
   ```
   
Download and Run the Installation Assistant

```bash
curl -sO [https://packages.wazuh.com/4.14/wazuh-install.sh](https://packages.wazuh.com/4.14/wazuh-install.sh) && sudo bash ./wazuh-install.sh -a -i

```

> **Note:** Make sure to take note of your username and password; you will need them for the next step.
> **Note:** The installation depicts v4.12 being installed. You can upgrade to 4.14 following the Wazuh Upgrade Guide.

---

Wazuh Dashboard

After the installation is complete, log in to the Wazuh Dashboard.

1. Check your VM’s IP address:
```bash
ifconfig

```


Open a browser and go to:
```text
https://<vm-ip>:443

```


Accept any browser security warning due to the self-signed certificate and log in using the credentials displayed at the end of the installation script.

---

### 4. Install the Wazuh Agent (Windows Host)

1. Download the latest Wazuh agent MSI installer from the official documentation at Wazuh Agent MSI Guide.
2. Install the MSI package on your Windows system using the default settings.

---

### 5. Register the Agent

1. Generate an agent key on the Ubuntu Manager with the following command:
```bash
sudo /var/ossec/bin/manage_agents

```


* Select **A** to add an agent.
* Assign a name.
* Add the Windows Host IP.
* After creation, select **E** to extract the key.
* Copy the key output.


2. Apply Key in the Windows Agent:
* Open Wazuh Agent Manager GUI from the Start Menu.
* Paste the copied key into the appropriate field.
* Save and apply the key.
* Add the manager's IP address (IP address of your Ubuntu manager).
* Restart the agent service.
* You can then go to the Wazuh dashboard and see the agent onboarded.



---

### 6. File Integrity Monitoring on Windows

Wazuh supports real-time monitoring of file and folder changes using Syscheck.

1. **Edit Agent Configuration:**
* Open the following configuration file:
`C:\Program Files (x86)\ossec-agent\ossec.conf`
* Add the following entry inside the Directory block to monitor the specified folder in real-time:
```xml
<directories realtime="yes">C:\Users\<your-username>\Documents\Wazuh_test</directories>

```




**Restart the Agent:**
* After saving the changes, restart the Wazuh agent service to apply the configuration.



---

### 7. Verifying Setup

1. Open the Wazuh Dashboard in your browser.
2. Navigate to **Agents** → ensure the Windows agent is listed and status is **Active**.
3. Go to the **Integrity Monitoring** section.
4. Perform actions (create/modify/delete files) in the monitored folder.
5. Confirm that alerts appear in the dashboard.

```

```
