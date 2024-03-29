# ![TryHackMe-Splunk-Basics](https://miro.medium.com/v2/resize:fit:726/0*lAFon1NDL8rhE2y2.png)

## Introduction

For this project, I am documenting my journey learning the basics of Splunk. Within this module, I delved into the fundamentals of Splunk, examining its features and demonstrating how it enhances visibility into network activites, thereby expediting the detection process.

## About Splunk

- Splunk stands out as a prominent Security Information and Event Management (SIEM) solution, offering the capability to gather, assess, and correlate real-time network and machine logs. 
  
## Components of Splunk

- Forwarder
- Idexer
- Search Head

- Forwarder - is installed on the endpoint to be monitored
Its main task is to collect data and send it to the Splunk instance such as (event logs, website logs, firewall logs, etc)
-   web server generating web traffic
- windows machine generating windows event logs, powershell, and Sysmon data
- Linux host generating host-centric logs
- database generating DB connection requests, responses and errors
o	Indexer-processes the data it received from forwarders. Takes data, normalizes it into field-value pairs, determines the datatype of the data, and stores them as events
o	Search Head – this is a place within the Search & Reporting app where users can search the indexed logs 
	Searches can be for a term or use a Splunk Search Processing Language which is where the requests is sent to the indexer and the relevant events are returned in the form of field-value pairs
	Search Head also provides the ability to transform the results into presentable tables, visualizations (pie charts, bar charts etc)


- Creating the Honeynet: Initially, I deployed multiple virtual machines with known vulnerabilities in Azure, creating a simulated insecure environment.
- Monitoring and Analysis: Next, I configured Azure to collect logs from various sources and consolidated them in the Log Analystics Workspace. Using Microsoft Sentinel, I built attack maps, setup alert triggers, and created incidents based on the gathered data.
- Measurement of Secuirty Metrics: During the insecure state of the environment, I monitored and recorded key security metrics for a 24-hour period. This established a baseline for comparision once remediation measures were implemented.
- Incident Response and Remediation: After addressing the identified incidents and vulnerabilities, I proceeded to stregthen the environment by implementing security best practices and following Azure-specific recommendations.
- Analysis After Remediation: Fianlly, in order to evaluate the effectiveness of the implemented measures, I observed the environment for an additional 24-hour period, measuring security metrics once again and compared them against the intial baseline.
  
## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/1tLjWY9.png)
Prior to Implementing Hardening Measures and Security Controls:

At the "BEFORE" stage of the project, the resources were deliberately set up with public exposure to the internet. This intentionally insecure configuration aimed to attract potential cyber attackers and monitor their techniques. The Virtual Machines had both their Network Security Groups (NSGs) and built-in firewalls configured with wide-open rules, granting unrestricted access from any source. Furthermore, all other resources, including storage accounts and databases, were deployed with public endpoints that were visible to the internet, without utilizing private endpoints for enhanced security.


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/ch1cAMU.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

In the "AFTER" stage, I implemented a series of measures to strengthen the security and protect the environment. These enhancements comprised the following:

- Network Security Groups (NSGs): I fortified the NSGs by configuring rules to block all inbound and outbound traffic, except for my own public IP address. This ensured that only authorized traffic from a trusted source was permitted to access the virtual machines.

- Built-in Firewalls: I fine-tuned the settings of the virtual machine's built-in firewalls to restrict access and prevent unauthorized connections. This involved adjusting the firewall rules based on the specific requirements of each virtual machine, minimizing the potential attack surface.

- Private Endpoints: To bolster the security of other Azure resources, I replaced the public endpoints with Private Endpoints. This transition restricted access to sensitive resources, such as storage accounts and databases, to the virtual network, isolating them from the public internet. This added layer of protection safeguarded the resources against unauthorized access and potential attacks.

By comparing the security metrics before and after implementing these hardening measures and security controls, I was able to demonstrate the effectiveness of each step in improving the overall security posture of the Azure environment.

## Attack Maps Before Hardening / Security Controls
- The showcased attack map serves as a visual representation of the repercussions caused by leaving the Network Security Group (NSG) open, enabling unrestricted flow of malicious traffic. This visualization emphasizes the significance of implementing adequate security measures, such as enforcing restrictive NSG rules. By doing so, unauthorized access can be prevented, and potential threats can be minimized effectively.
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/Aa8Nnjj.png) <br>

- The showcased attack map brings attention to the significant number of syslog authentication failures encountered by the Linux server that was deployed. These failures suggest unauthorized access attempts originating from external sources. This serves as a crucial reminder of the utmost importance of implementing robust authentication mechanisms to secure Linux servers and diligently monitoring system logs for any indications of intrusion attempts.
![Linux Syslog Auth Failures](https://i.imgur.com/ETLwFd9.png) <br>

- The showcased attack map presents multiple instances of RDP and SMB failures, illustrating the persistent efforts of potential attackers to exploit these protocols. This visualization strongly emphasizes the necessity of securing remote access and file sharing services to safeguard against unauthorized access and potential cyber threats. It highlights the critical importance of implementing robust security measures to protect these services and maintain a secure network environment.
![Windows RDP/SMB Auth Failures](https://i.imgur.com/7XXQ2xB.png) <br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2023-06-16 5:17:32 AM
Stop Time 2023-06-17 5:17:32 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 139088
| Syslog                   | 6035
| SecurityAlert            | 10
| SecurityIncident         | 341
| AzureNetworkAnalytics_CL | 2526

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-06-17 11:15:31 AM
Stop Time	2023-06-18 11:15:31 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12546
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

For this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to activate alerts and generate incidents based on the collected logs. Furthermore, security metrics were assessed in the insecure environment prior to implementing security controls, and then once again after the implementation of these measures. It is important to highlight that the number of security events and incidents significantly decreased after the security controls were implemented, providing evidence of their effectiveness.

It is worth mentioning that if the network resources were extenzively used by regular users, it is probable that more security events and alerts could have been generated within the 24-hour period following the implementation of the security controls.
