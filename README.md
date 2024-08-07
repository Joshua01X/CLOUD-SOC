# CLOUD-SOC
Deployment of SOC Environment Through Microsoft Azure

<h1>Cloud Honeynet and Security Operations Center (SOC) Implementation</h1>

![Example Image](https://github.com/Joshua01X/CLOUD-SOC/blob/main/Cloud%20SOC%20DIA.jpg?raw=true)

<h2>Introduction</h2>
This project is divided into two major phases. The first phase involves setting up honeynet resources and exposing an unsecured cloud environment globally for 24 hours. During this period, logs are aggregated and analyzed. The second phase focuses on security hardening by employing security procedures and regulatory compliance. The environment is then observed for another 24 hours to compare the secured environment against the unsecured environment, verifying the effectiveness of the security measures. The metrics measured in this lab are as follows:<br>

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

Here's a rephrased version:

## Azure Components and Regulations Employed In This Lab
- Azure Virtual Network (VNet)
- Azure Subnet
- Azure Firewalls
- Azure Private Endpoint Security
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace using Kusto Query Language (KQL)
- Azure Key Vault for Managing Secure Secrets
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Safeguard Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

The first phase of this project consists of three key processes: <i><b>setting up honeynet resources, configuring a central log repository, </i></b>and <b><i>deploying a SIEM tool</i></b>.

## Setting Up the Honeynet Resources:

<b>Microsoft Entra ID</b>: Provides entry points for malicious actors, simulating compromised user accounts. Actors can access and alter configurations within the setup, such as blob storage (for storing large amounts of unstructured data) and key vaults (for storing and <br> managing sensitive information). Suspicious activity logs are generated and ingested by the SIEM tool, Microsoft Sentinel, which collects and aggregates these logs in the Log Analytics Workspace (LAW).<br><br>
<b>SQL Database</b>: An empty SQL Server database setup within a Windows virtual machine serves as another honeypot for malicious activities.<br><br>
<b>Virtual Machines</b>: Three unsecured virtual machines—one Linux VM and two Windows VMs—serve as entry points for malicious activities. Logs from these machines are collected and sent to LAW by configuring Data Collection Rules and deploying agents.<br><br>
<b>Network Security Groups (NSGs) and Firewalls</b>: All ports are allowed to the public, increasing the risk of compromise. A storage account stores the flow logs of the NSGs and directs them to the correct LAW.<br><br>

## Setting Up the Central Log Repository:

<b>Log Analytics Workspace (LAW)</b>: Acts as the central repository for logs from various honeypots. Logs are queried and analyzed using Kusto Query Language (KQL). Microsoft Defender for Cloud is enabled for LAW for basic security setups and continuous exporting of logs.<br><br>

## Setting Up the SIEM Tool:

<b>Microsoft Sentinel</b>: Connects to LAW and acts as the SIEM tool. It maps global incidents and alerts in real-time using geo-IP watchlist data and KQL queries.<br><br>
<h3>The unsecured environment was monitored for 24 hours, during which key security metrics were recorded.</h3><hr>

## Environment Architecture Before Hardening and Security Controls Implementation

![Security Before](https://github.com/Joshua01X/CLOUD-SOC/blob/main/Security%20Before.png?raw=true) <br><br>

<b>The SIEM Workbooks or Attack Maps reflected the following images, pinpointing the various sources of attacks targeting the following resources</b><hr style="border: 0; height: .5px; background: #000;" />
<b>This attack map shows failed RDP and SMB connection attempts from malicious sources. The map shows persistent attempts to exploit the mentioned protocols, highlighting the necessity to secure remote access to resources and file sharing methods and prevent unathorized access to these critical services.
![WINDOWS-RDP-AUTH-FAIL 24HRS BEFORE SECURING](https://github.com/user-attachments/assets/904c024d-5e4c-42a4-b39b-5c38b00fdac6)
<br><br><br>
This attack map provides the locations and frequency of failed SSH login attempts on the Linux virtual machine, highlighting persistent unauthorized access attempts in several areas. Monitoring these attempts is crucial for identifying and mitigating potential security threats.
![LINUX-SSS-AUTH-FAIL 24HRS BEFORE SECURING](https://github.com/user-attachments/assets/62ec780a-55de-4531-a6c2-e2f164cd3ce4)
<br><br><br>
This attack map displays the locations and frequency of authentication failures on the MS SQL Server, pinpointing potential unauthorized access attempts. Tracking these failures is essential for detecting possible breaches and securing database access.
![MSSQL 24HRS BEFORE SECURING](https://github.com/user-attachments/assets/d03e7819-2d1e-4cfe-8823-7f28a77bc673)
<br><br><br>
This attack map reflects the consequences of leaving the NSGs inbound rule traffic open to public. Several malicious flows were able to get into the NSGs. Implementing proper traffic rule is crucial in order to prevent unauthorized access into the systems. </b>
![NSG-ALLOWED-IN 24HRS BEFORE SECURING](https://github.com/user-attachments/assets/ff925858-db99-4d08-ab45-f40607e691f5)
<br><br><br>

## The Following Metrics Before Security Hardening
The following table shows the metrics measured within the unsecured environment for 24 hours: <br>Start Time 2024-07-27 13:25:28 <br>Stop Time 2024-07-28 13:25:28

| Metric                   | Count  |
|--------------------------|--------|
| SecurityEvent            | 111847 |
| Syslog                   | 1087   |
| SecurityAlert            | 35     |
| SecurityIncident         | 276    |
| AzureNetworkAnalytics_CL | 2490   |

<br><hr>

### The second phase of this project involves implementing the following security hardening controls:
1. <b>Configuring the firewalls</b>: The built-in firewalls were disabled to the public to restrict unauthorized access and suspicious connections.<br>
2. <b>Creating private endpoint protections</b> to the azure key vaults and storage account to limit the access to these crucial resources within the private virtual network<br>
3. <b>Changing the NSGs' inbound traffic rule</b> to only allow my IP address to access the resources in the private network. This ensures that only the configured trusted connections will be established. <br>
4. <b>Creating a subnet to NSGs</b> to further isolate the operating environment. Additional subnets can be configured to ensure better control and security within each isolated environment.<br>
5. <b>Implementing security controls</b> recommended by the <b>NIST SP 800-53 Revision 5</b> for Security Controls and <b>NIST SP 800-61 Revision 2</b> for Incident Handling Guidance. These regulatory compliances will further enhance the total security posture of my cloud environment. <br><br><br>

## Environment Architecture After Hardening and Security Controls Implementation

![Security After](https://github.com/Joshua01X/CLOUD-SOC/blob/main/Security%20After.png?raw=true)

##The Attack Maps After Security Hardening

> All map queries returned no results due to no alter instances generated within the 24 hours period after hardening.</i><br><br>

## The Following Metrics After Security Hardening
The following table shows the metrics measured within the unsecured environment for 24 hours: <br>Start Time 2024-07-29 17:02:40 <br> Stop Time 2024-07-30 17:02:40

| Metric                   | Count  |
|--------------------------|--------|
| SecurityEvent            | 0      |
| Syslog                   | 1      |
| SecurityAlert            | 0      |
| SecurityIncident         | 0      |
| AzureNetworkAnalytics_CL | 0      |

<br><hr>

## Comparison Of The Metrics Before & After Security Hardening
Data comparison reflecting the difference of security incident occurence before and after security hardening

| Metric                   | Before   | After | Change |
| ------------------------ | -----    | ----- | ------ |
| SecurityEvent            | 111847   | 0     | 100%   |
| Syslog                   | 1087     | 1     | 99.91% |
| SecurityAlert            | 35       | 0     | 100%   |
| SecurityIncident         | 276      | 0     | 100%   |
| AzureNetworkAnalytics_CL | 2490     | 0     | 100%   |

<br><hr>
## Conclusion
This project involved a real-time interactive honeynet within the Microsoft Azure platform. Log sources from the honeynet were forwarded to a Log Analytics Workspace. Microsoft Sentinel, acting as the SIEM tool, triggered alert incidents and mapped these alerts in real-time. After implementing security hardening, significant decreases in security incidents were observed. A 100% incident elimination was achieved for Windows VM's security events, Microsoft Defender for Cloud alerts, Microsoft Sentinel incidents, and malicious NSG inbound traffic. Linux VM's syslog incidents were reduced by 99.91%, demonstrating the effectiveness of the security measures.




