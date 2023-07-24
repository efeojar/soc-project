# AZURE HONEYNET: Simulating Live Cyber Attack

![SOC](https://github.com/efeojar/soc-project/assets/66268247/f0d2785d-bdf2-4b95-acac-3ba214a2fe97)
## Introduction
For this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I made it an insecure environment for 24 hours, and monitored the attacks & incidents, after 24 hours I applied some security controls to harden the environment, measured metrics for another 24 hours, then show the results below. The metrics shown are:
* SecurityEvent (Windows Event Logs)
* Syslog (Linux Event Logs)
* SecurityAlert (Log Analytics Alerts Triggered)
* SecurityIncident (Incidents created by Sentinel)
* AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
## Objective
The objective of this project is to create a honeynet that will allow me to analyze live attacks, conduct incident response and investigate logs and understand the attacker's intentions, tactics, techniques, and procedures. Also to learn how to secure an environment that is insecure by changing the Firewall rules, implementing Azure Private Links, and administering regulatory compliance i.e. NIST 800-53, and Microsoft Defender for Cloud recommendations.
The architecture of the mini honeynet in Azure consists of the following components:
* Virtual Network (VNet)
* Network Security Group (NSG)
* Virtual Machines (2 Windows, 1 Linux)
* Log Analytics Workspace for performing Kusto Query (KQL) queries to filter data
* Azure Key Vault to securely store secrets
* Azure Storage Account for storing data
* Microsoft Sentinel for Security Information and Event Management (SIEM)
* Microsoft Defender for Cloud to monitor resources
* Powershell for performing simulated attacks
* NIST SP 800-53 Revision 4 for Security Controls
* NIST SP 800-61 Revision 2 for Incident Handling Guidance
## Architecture Before Hardening / Security Controls

![Unsecure connection](https://github.com/efeojar/soc-project/assets/66268247/03ff90e3-0055-48ee-93fd-e9dcd35abdeb)

In the "BEFORE" stage, all resources were initially deployed with public exposure to the Internet. This setup was intentionally insecure to attract attackers and observe their tactics. Both the machines, their Network Security Groups (NSGs), and built-in firewalls were left open, allowing unrestricted access from any source. Additionally, all other resources, such as storage accounts and databases, were deployed with public endpoints visible to the internet, without utilizing any Private Endpoints for added security.
## Attack Maps Before Hardening / Security Controls
## Windows-MSSQL-auth-fail
<img width="1312" alt="Unsecured - mssql-auth-fail" src="https://github.com/efeojar/soc-project/assets/66268247/b7427530-ea4f-4593-b967-6a1a11fb4a8d">

## NSG-malicious-allowed-in
<img width="1324" alt="Unsecured - nsg-malicious-allowed-in" src="https://github.com/efeojar/soc-project/assets/66268247/87439995-3f0d-4129-a7fc-04f7210f49d1">

##  Linux-Syslog-ssh-auth-fail
<img width="1303" alt=" Unsecured- syslog-ssh-auth-fail" src="https://github.com/efeojar/soc-project/assets/66268247/fd6cf9e3-1890-4fda-aef6-0c29908a025a">

## Windows-RDP-auth-fail 
<img width="1312" alt="Unsecured - windows-rdp-auth-fail " src="https://github.com/efeojar/soc-project/assets/66268247/d484550f-8f52-4888-a100-af952a3ab643">

# Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours: Start Time 2023-07-19 15:59:44PM Stop Time 2023-07-20 15:59:44PM

Metric	| Count |
--------|-------|
Windows VM SecurityEvent	| 306983 |
Linux VM Syslog	| 772 |
SecurityAlert	| 18 |
SecurityIncident	| 220 |
NSG Inbound Malicious Flows Allowed	| 954 |

# Incidents Alert Before Hardening / Security Controls 

<img width="1336" alt="Incidents in the last 24hrs of Unsecure enviroment " src="https://github.com/efeojar/soc-project/assets/66268247/2dfefda8-7b46-4a30-b9db-741ff391794d">

Be informed that a set of custom analytics rules were added, this was part of the preparation made to anticipate the attacks. When the rules are triggered, an incident event is created.


## Architecture After Hardening / Security Controls

![Secured](https://github.com/efeojar/soc-project/assets/66268247/35afd1e6-c5ce-4551-bb91-ebbbe3e0716d)

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

# Metrics After Hardening / Security Controls
The following table shows the metrics we measured in our insecure environment for 24 hours: Start Time 2023-07-21 01:07:52PM Stop Time 2023-07-22 01:07:52PM

Metric	| Count |
--------|-------|
Windows VM SecurityEvent	| 25245 |
Linux VM Syslog	| 24 |
SecurityAlert	| 0 |
SecurityIncident	|0 |
NSG Inbound Malicious Flows Allowed	| 0 |

# Incidents Alert After Hardening / Security Controls 

<img width="1340" alt="Incidents after hardening environment" src="https://github.com/efeojar/soc-project/assets/66268247/218ae6ea-38e6-4064-aeb0-141817cd7129">

# Conclusion
In this project, a mini honeynet was set up in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and generate incidents based on the collected logs. Additionally, metrics were recorded in the vulnerable environment prior to implementing security measures and after applying the security controls. The results showed a significant reduction in the number of security events and incidents after implementing these measures, indicating their effectiveness.

However, it's important to acknowledge that if the network's resources were heavily utilized by regular users, there could have been a higher volume of security events and alerts during the 24-hour period following the security control implementation.

