# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/zZGnfiq.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and incidents creation. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment (enabled NIST 800-53 and satisfied some controls such as SC-7 Boundary Protection), measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/fZ8Zbb0.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/cmurLuw.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/KlwHcyY.jpg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/goVqnZn.jpg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/9LcasIo.jpg)<br>
![MSSQL Authorization Failures](https://i.imgur.com/8FlSahp.jpg)<br>
![Incidents Reported](https://i.imgur.com/SBhVaRq.jpg)<br>
![Security Score](https://i.imgur.com/dw8F4ii.jpg)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 12/28/2023, 10:47:13 PM
Stop Time 12/29/2023, 10:47:13 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 32245
| Syslog                   | 6
| SecurityAlert            | 1
| SecurityIncident         | 189
| AzureNetworkAnalytics_CL | 68057

## Attack Maps Before Hardening / Security Controls

```Most map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/SkWyEm0.jpg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/sfvEMws.jpg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/pokwdxE.jpg)<br>
![MSSQL Authorization Failures](https://i.imgur.com/p645cVd.jpg)<br>
![Incidents Reported](https://i.imgur.com/BzWIB7b.jpg)<br>
![Security Score](https://i.imgur.com/PYVV5kK.jpg)<br>


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 12/29/2023, 11:28:58 PM
Stop Time	12/30/2023, 11:28:58 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 23531
| Syslog                   | 6
| SecurityAlert            | 0
| SecurityIncident         | 4
| AzureNetworkAnalytics_CL | 6

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. Keep in mind that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. Note. If the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.