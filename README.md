# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

Cybersecurity is a field that is currently in very high demand, and we are seeing a huge surge of individuals interested in joining the field. However, we are all aware of a particular catch 22 that we all face, as we are trying to make the transition into the field: "You need experience to get a job, but you need a job to get experience". This is where hands-on projects are very useful. They allow you to get practical experience and knowledge, which you will be able to apply to real world cases, using industry standard tools. 

The goal of this project was to build my own SOC environment, in the form of a mini honeynet in Azure. I captured logs from various resources, and ingested those logs into a Log Analytics workspace, which was then used by Microsoft Sentinel (SIEM), to build attack maps, trigger alerts, and create incidents. In the effort to learn more about the Azure cloud environment, I analyzed some security metrics in an insecure environment, deploying 2 vulnerable Virtual Machines (1 Windows 10 & 1 Linux Ubuntu) by turning off their firewalls (leaving them wide open), and letting them run for 24 hours over the internet. This created many security incidents and gave me the opportunity to practice incident response following the NIST 800-61 framework (incident response lifecycle). I then applied some security controls in accordance with the NIST 800-53 framework, in order to harden my environment, and measured those same metrics for another 24 hours. This allowed me to compare and contrast the two days, and analyze the difference in the number of security events. Finally, I show the results below. The metrics we will be showing are the following: 

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

Here are the components that were found in the architecture of the mini honeynet in Azure: 

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

We also used Microsoft Azure Active Directory to create users, assign them roles and generate login attempts logs for those same users. 
I used the Kusto Query Language (KQL), a microsoft native query language to gather logs inside of my Log Analytics Workspace, and create my attack world maps.

In order to capture the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. I made sure to leave the Network Security Groups and built-in firewalls wide open for my Virtual Machines, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, I hardened the Network Security Groups by blocking ALL traffic with the exception of my admin workstation (My IP Address), and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-13 14:16:43
Stop Time 2023-12-14 14:16:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15357
| Syslog                   | 15743
| SecurityAlert            | 1
| SecurityIncident         | 161
| AzureNetworkAnalytics_CL | 2420

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-12-22 18:52:40 
Stop Time	2023-12-23 18:52:40

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8044
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

As a cybersecurity / cloud enthusiast and future professional, this project was the perfect way to get some real hands-on, practical cybersecuirty experience on the Microsoft Azure platform. I not only learned how to deploy virtual machines, but also learned how to troubleshoot login issues, how to secure a network, create accounts and generate logs from Azure Active Directory (Now known as Entra ID), ingest logs into a centralized Log Analytics Workspace and link it to microsoft Sentinel to trigger security alerts to spin up incidents for myself, to practice incident response. I was also able to learn how to write KQL queries in order to generate my logs, and to create my attack maps. Although it is difficult to reproduce the amount of traffic activity that you would see on a real organization's network, I was still able to generate a decent amount of traffic, logs and alerts to analyze.  As you can also see, the number of security events and incidents was drastically reduced after hardening our environment and applying security controls to our resources and network, hence demonstrating their effectiveness. It is however worth noting that if the resources within the network were heavily utilized by regular users, it is liklely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls. At the end of the day, we have to realize that it is very unlikely we will completely eradicate the presence of security events, the best we can hope for is a secure network with effective security controls in place, and an effective detection system in place, in case a security incident occurs. 

This falls in line with the Incident Response lifecycle which includes the following steps:

1) Preparation
2) Detection & Analysis
3) Containment, Eradication and recovery
4) Post-Incident Activity

This also falls in line with the NIST Cybersecurity Framework: 

1) Identify
2) Protect
3) Detect
4) Respond
5) Recover

I look forward to doing more hands-on projects similar to this one in order to strengthen my understanding of cybersecurity through the cloud, and in general. If you have any questions about this project, feel free to contact me on linkedin! Please stay tuned, as I will also be posting more in-depth step by step tutorials, walking you through the entirity of this project. 
