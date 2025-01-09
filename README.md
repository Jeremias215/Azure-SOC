# Azure-SOC

# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I designed a mini honeynet within Azure to monitor and analyze potential threats. Various log sources from connected resources were ingested into a Log Analytics workspace, which was integrated with Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. To evaluate the effectiveness of security measures, I first recorded baseline metrics in an unprotected environment over a 24-hour period. Next, I implemented security controls to harden the environment and conducted another 24-hour monitoring phase to compare results. The key metrics analyzed include:

SecurityEvent: Windows Event Logs

Syslog: Linux Event Logs

SecurityAlert: Alerts triggered in Log Analytics

SecurityIncident: Incidents created in Microsoft Sentinel

AzureNetworkAnalytics_CL: Malicious network flows allowed into the honeynet


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The Azure-based mini honeynet is structured with the following key components:

Virtual Machines: Two Windows VMs and one Linux VM serving as the honeypots.

Virtual Network (VNet): Provides network connectivity for all deployed resources.

Network Security Group (NSG): Controls inbound and outbound traffic to the virtual network.

Log Analytics Workspace: Central hub for aggregating and analyzing log data.

Microsoft Sentinel: Integrated with the workspace for threat detection, alerting, and incident management.

Azure Storage Account: Used for storing logs and other critical data.

Azure Key Vault: Secures sensitive information such as access keys and secrets.


BEFORE Metrics:
In the initial setup, all resources were deployed with unrestricted exposure to the internet. Virtual Machines had their Network Security Groups (NSGs) configured to allow all traffic, and built-in firewalls were left completely open. Additionally, other resources were accessible via public endpoints, with no implementation of Private Endpoints for security.

AFTER Metrics:
Following security enhancements, the Network Security Groups (NSGs) were reconfigured to block all traffic except from the designated admin workstation. Built-in firewalls were enabled and configured for all resources, and Private Endpoints were utilized to restrict access and secure the environment.


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was deployed in Microsoft Azure, with log sources configured to feed into a Log Analytics workspace. Microsoft Sentinel was utilized to monitor the environment, triggering alerts and creating incidents based on the ingested logs. Metrics were collected during an initial 24-hour period in an unprotected setup and compared to those gathered after implementing security controls. The results showed a significant reduction in security events and incidents, highlighting the effectiveness of the applied measures.

It’s important to note that if the network resources were subjected to higher user activity, the volume of security events and alerts within the 24-hour post-hardening phase might have been greater.
