---
title: 'Threat Jammer website'
excerpt: 'The Threat Jammer website is a web application that allows manage basic features of the service.'
coverImage: ''
created: '2021-01-12'
updated: '2021-01-12'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/threat-jammer-site.md
  home: /docs/index
  previous: /docs/introduction-threat-jammer-report-api
  next:
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

## Introduction

[ThreatJammer](https://threatjammer.com/) is an API-as-a-Service (AAAS) and it has been designed to integrate with third party services. Developers and security teams should use the API as a Data Enrichment tool to manage the threat detection and mitigation process with the help of different tools and services. The unsleash the full potential of the API, developers should do a deep dive in the documentation of both User and Report APIs.

The ThreatJammer website is a web application that allows the management of the user account and the different API Keys in the different regions. Plus, the website has a search engine and a dashboard (the cockpit) that permits to see the different reports of the information obtained from the API. The main purpose of this dashboard is to provide a quick overview of the result of the risk assessment process without the need to use the REST API. 

## Risk assessment without being registered

### Search query 
The website can perform a risk assessment without being registered. When a user enters an IP address, the website will perform a risk assessment and return the risk level of the IP address in a visual way. It will also return information about the Autonomous Systems (AS) that are associated to the IP address.

![Threat Jammer Anonymous Risk Assessment](/docsimg/risk-assessment-anonymous.png)

### Risk score
The information returned by the risk assessment is the same as the information returned by the endpoints of the User API [Data Assessment](https://paris.api.threatjammer.com/docs#/Data%20assesment). The guage chart will display the risk score of the IP address in the range of 0 to 100. A human readable score is calculated by the following formula: 

| Range | Risk level |
|-------|------------|
| `0 to 34` | Low|
| `35 to 68` | Medium |
| `69 to 100` | High |

### Ban or report a False Positive
A registered user can ban or report a False Positive IP address. The buttons are grayed and disabled when the user is not logged in. See below when the user is logged in.

### Risk found
The website will also display what type of risk was found based on the information returned by the User API. It will display the information as follows:
- Low risk: Green bar with the message **No risk found**.
- 

| Risk level | Displays |
|-------|------------|
| `Low risk` | Green bar with **No risk found** legend.|
| `Medium risk` | Yellow bar with risk information. |
| `High risk` | Red bar with risk information. |

### Autonomous System information
The website will also display the Autonomous System information of the IP address. It will display the information as follows:
- The Autonomous System Number (ASN) of the IP address.
- The name of the ASN.
- The description of the ASN.
- The country of the ASN.
- The region of the ASN.
- IP address and Prefix.

### Social buttons
The website will also display the social buttons that will allow the user to share the risk assessment in the following social networks:
- Twitter
- Facebook
- LinkedIn
- Telegram
- Whatsapp

The site will open a new window for each social network and it will show a summary of the information obtained. For example, the Twitter button will show the following information:

![Threat Jammer Twitter Risk Assessment](/docsimg/risk-assessment-twitter.png | width=300)
