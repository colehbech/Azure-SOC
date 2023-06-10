# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/yynJukJ.jpg)

## Introduction

This project consisted of ingesting logs from a few different resources into Log Analytics workspace which was used by Azure Sentinel to create incidents and attack maps. To create these incidents, the VMs were left fully open for anyone on the internet to attack therefore creating a honeynet. These logs were taken from the Tenant level (Azure Active Directory), the management plane (Subscription level), and the Data plane (Resource level) allowing for a holistic incident response environment with various alerts. The goal of this project was to compare the results of two 24 hour periods, one with the environment unsecure and the other hardened according to NIST 800-53; specifically using SC-7. Below are the metrics used.


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/GLfG9ww.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/7a0pZae.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/nF6a69m.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/WpiFVmn.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ffryvEy.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time: 2023-05-15 12:57:13
Stop Time: 2023-05-16 12:57:13

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 28819
| Syslog                   | 12681
| SecurityAlert            | 5
| SecurityIncident         | 237
| AzureNetworkAnalytics_CL | 626

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time: 2023-05-20 13:02
Stop Time: 2023-05-21 13:02

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10003
| Syslog                   | 18
| SecurityAlert            | 1
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
