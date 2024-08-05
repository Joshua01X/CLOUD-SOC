# CLOUD-SOC
SOC Environment Deployment Through Microsoft Azure

<h1>Cloud Honeynet and Security Operations Center (SOC) Implementation</h1>

<h4>This project is divided into two major phases. The first phase involves setting up honeynet resources and exposing an unsecured cloud environment globally for 24 hours. During this period, logs are aggregated and analyzed. The second phase focuses on security hardening by employing security procedures and regulatory compliance. The environment is then observed for another 24 hours to compare the secured environment against the unsecured environment, verifying the effectiveness of the security measures.</h4><hr>

The first phase of this project consists of three key processes: <i><b>setting up honeynet resources, configuring a central log repository, </i></b>and <b><i>deploying a SIEM tool</i></b>.

<h2>Setting Up the Honeynet Resources:</h2>

<b>Microsoft Entra ID</b>: Provides entry points for malicious actors, simulating compromised user accounts. Actors can access and alter configurations within the setup, such as blob storage (for storing large amounts of unstructured data) and key vaults (for storing and <br> managing sensitive information). Suspicious activity logs are generated and ingested by the SIEM tool, Microsoft Sentinel, which collects and aggregates these logs in the Log Analytics Workspace (LAW).<br><br>
<b>SQL Database</b>: An empty SQL Server database setup within a Windows virtual machine serves as another honeypot for malicious activities.<br><br>
<b>Virtual Machines</b>: Three unsecured virtual machines—one Linux VM and two Windows VMs—serve as entry points for malicious activities. Logs from these machines are collected and sent to LAW by configuring Data Collection Rules and deploying agents.<br><br>
<b>Network Security Groups (NSGs) and Firewalls</b>: All ports are allowed to the public, increasing the risk of compromise. A storage account stores the flow logs of the NSGs and directs them to the correct LAW.<br><br>

<h2>Setting Up the Central Log Repository:</h2>

<b>Log Analytics Workspace (LAW)</b>: Acts as the central repository for logs from various honeypots. Logs are queried and analyzed using Kusto Query Language (KQL). Microsoft Defender for Cloud is enabled for LAW for basic security setups and continuous exporting of logs.<br><br>

<h2>Setting Up the SIEM Tool:</h2>

<b>Microsoft Sentinel</b>: Connects to LAW and acts as the SIEM tool. It maps global incidents and alerts in real-time using geo-IP watchlist data and KQL queries.<br><br>

<h3>The unsecured environment was monitored for 24 hours, during which key security metrics were recorded.</h3><br>

Environment Architecture Before Hardening and Security Controls Implementation<br>
-- Insert Diagram -- <br><br>

The SIEM Workbooks or Attack Maps reflected the following images, pinpointing the various sources of attacks targeting the following resources<br>
-- Insert Windows RDP Fail --<br>
-- Insert Linux SSH Logon Fail --<br>
-- Insert SQL Server Logon Fail --<br>
-- Insert NSG Malicious traffic that got in --<br><br>

<h3>The Following Metrics Before Security Hardening</h3><br>

| Metric                   | Count  |
|--------------------------|--------|
| SecurityEvent            | 111847 |
| Syslog                   | 1087   |
| SecurityAlert            | 35     |
| SecurityIncident         | 276    |
| AzureNetworkAnalytics_CL | 2490   |



<br><hr>

The second phase of this project involves implementing the following security hardening controls:
1. <b>Configuring the firewalls</b>: The built-in firewalls were disabled to the public to restrict unauthorized access and suspicious connections.
2. <b>Creating private endpoint protections</b> to the azure key vaults and storage account to limit the access to these crucial resources within the private virtual network
3. <b>Changing the NSGs' inbound traffic rule</b> to only allow my IP address to access the resources in the private network. This ensures that only the configured trusted connections will be established. 
4. <b>Creating a subnet to NSGs</b> to further isolate the operating environment. Additional subnets can be configured to ensure better control and security within each isolated environment.
5. <b>Implementing security controls</b> recommended by the <b>NIST SP 800-53 Revision 5</b> for Security Controls and <b>NIST SP 800-61 Revision 2</b> for Incident Handling Guidance. These regulatory compliances will further enhance the total security posture of my cloud environment. <br>
-- Insert Diagram -- <br><br>

<h3>The Attack Maps After Security Hardening</h3>
All map queries returned no results due to no alter instances generated within the 24 hours period after hardening.<br><br>

<h3>The Following Metrics After Security Hardening</h3>
| Metric                   | Count  |
|--------------------------|--------|
| SecurityEvent            | 0      |
| Syslog                   | 1      |
| SecurityAlert            | 0      |
| SecurityIncident         | 0      |
| AzureNetworkAnalytics_CL | 0      |
<br><hr>

<h3>Comparison Of The Metrics Before & After Security Hardening</h3>
| ------------------------------------|
| BEFORE                              |
| ------------------------------------|
| Metric                   | Count    |
| ------------------------ | -----    |
| SecurityEvent            | 111847   |
| Syslog                   | 1087     |
| SecurityAlert            | 35       |
| SecurityIncident         | 276      |
| AzureNetworkAnalytics_CL | 2490     |
| ------------------------------------|
<br><br><hr>

<h2>Conclusion</h2>
This project involved a real-time interactive honeynet within the Microsoft Azure platform. Log sources from the honeynet were forwarded to a Log Analytics Workspace. Microsoft Sentinel, acting as the SIEM tool, triggered alert incidents and mapped these alerts in real-time. After implementing security hardening, significant decreases in security incidents were observed. A 100% incident elimination was achieved for Windows VM's security events, Microsoft Defender for Cloud alerts, Microsoft Sentinel incidents, and malicious NSG inbound traffic. Linux VM's syslog incidents were reduced by 99.91%, demonstrating the effectiveness of the security measures.




