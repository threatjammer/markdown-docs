---
title: 'What is a Threat Jammer?'
excerpt: 'A short introduction to the Threat Jammer service.'
coverImage: ''
created: '2021-01-12'
updated: '2021-01-12'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/what-is-threat-jammer.md
  home: /docs/index
  previous: /docs/index
  next: /docs/threat-jammer-tokens
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

## Introduction

[ThreatJammer.com](https://threatjammer.com) is a service that allows developers, security engineers, and other IT professionals to access high-quality threat intelligence data from a variety of sources and integrate it into their applications with the sole purpose of detecting and blocking malicious activity.

The preferred way to access these threat intelligence data is through the [Threat Jammer REST API](https://threatjammer.com/api/). The API is a RESTful API that allows anybody to consume different threat intelligence sources easily and securely. Threat Jammer guarantees the highest level of data accuracy and freshness.

## The abuse detection problem

Every company offering a digital service has a different approach to abuse detection because all business models are different, and so are their needs. For example, a company may want to detect and block all kinds of Tor traffic, but others may want to let users sign up without any friction. This naive example highlights how an out-of-the-box approach to abuse detection is insufficient. 

On the other side of the spectrum, the company can dedicate a lot of resources to detect and block all kinds of malicious activity, but they may not want to spend all their time on this. Or they don't have enough resources. Dedicating resources to repetitive tasks or merely doing things not part of the business model is not a good idea.

Finally, cybercriminals are always thinking about how to exploit the services. It becomes a never-ending cat and mouse game no matter how much a company dedicates to protection from abuse. The response to new ways of exploiting the services must be agile, prompt, and effective.

## The Threat Jammer approach

### A tool for builders
Threat Jammer is a **tool for builders**. Threat Jammer is a set of services acting as building blocks of the threat detection strategy of digital companies. We understand how hard it is to find a solution to detect and block malicious activity, and that's why our services are built to solve this problem: you know what to do, we offer the tools to make it. 

The Threat Jammer approach to Threat detection relies on the following principles:
- It is a **multi-factor process**. It's not a one-time thing.
- It is **constantly evolving**. A set of factors used to detect and block malicious activity can change over time.
- It is **flexible**. To use in different ways, from a simple web application to a complex system.

### Multi-factor process

A multi-factor process is a process that is composed of multiple factors like IP address, user data, device, location, network provider, the behavior of the user, historical data, etc. Each of these factors can signal a different kind of threat and threat level. When combining the signals obtained from various factors, a system can detect and block malicious activity.

Threat Jammer can evaluate the different factors and assess when the signals are relevant to detect and block malicious activity. Threat Jammer performs the **assessment** and then offers a **recommendation** to the user.

### Constantly evolving

A Threat detection system should constantly be evolving. The factors used to detect and block malicious activity can change over time. For example, the user's IP address can vary from one network provider to another over time. The level of the threat associated with the network provider can change over time, so the user's overall risk level.

Threat Jammer updates the processes and data to maintain the most accurate and fresh information available. The assessments performed will always use the most up-to-date information available. As a result, the system's accuracy will always be the highest, plus reducing the false positives to the bare minimum.

### Flexible

We don't care how you use Threat Jammer. We want to make it easy to use and easily integrate it into your application. That's all. Choose what services you need and how you want to use them. That's it. The rest is up to you.

Threat Jammer is a set of services that digital builders can use in different ways. Some of them will not apply to all the use cases at one point in the evolution of the user's service. The lifetime of a digital service is long, and we want to be a partner both now and in the future.

## How Threat Jammer works

### REST API: A single point of contact

The Threat Jammer [User REST API](/docs/introduction-threat-jammer-user-api) is a single point of contact for all the different threat detection services. It allows you to consume the threat intelligence data from other sources and integrate it into your application. We designed the API to lower the entry barrier as much as possible to facilitate the integration of the threat intelligence data into your application, other threat intel services, and even already existing applications that already support our API.

The API needs a Key to work. Anybody can obtain a key from the [Threat Jammer site](https://threatjammer.com/keys/) after signing up for free. The key is a unique identifier to use the API for a specific region. Hence, if a developer wants to use the API in a particular region, they must obtain a key for the specified region.

> **Note: The service only supports one region and one API Key per region in this version. Future versions will allow more advanced configurations.** 

### OSINT databases

Open Source Intelligence is a crucial ingredient of any threat intel strategy. The value of Threat Jammer is not just to gather all the different sources and integrate them into a single point of contact. The most relevant value is  **categorize them and rank the datasets according to a pre-calculated risk score obtained using different techniques and algorithms transparent to the user.**

The Scoring Engine categorizes every dataset in Threat Jammer with an integer from 0 to 100. The higher the number, the higher the risk of malicious activity. Not all the datasets are equal, so each dataset has a risk score associated with it. If a resource belongs to multiple datasets, the risk score is the highest of all the datasets.

The Data Gathering Engine is responsible for gathering the OSINT data from different sources. Depending on how frequently the data changes at the origin, the engine will instantly update the datasets in all the regions. Hence, the users will always have access to the freshest and most accurate data with the most updated risk score. The users do not need to care about how these processes work.

### CSINT databases

Closed Source Intelligence is an ingredient that increases the quality of any threat intel strategy results. The highest quality and more reliable datasets are often obtained from custom-built extraction processes or directly acquired to external data providers for a fee. Following the same strategy of freshness and accuracy as the OSINT databases, the CSINT databases increase the capacity of our Assessment Engine to detect malicious activity.

We at Threat Jammer obtain some of the datasets available by paying a fee to the data providers—others with ad-hoc extraction processes. The CSINT databases are only available to paying subscribers in the Assessment Engine. Using CSINT datasets increases the risk score's accuracy and reduces the false positives.

### Heuristic Analysis

Heuristic Analysis -or rule-based analysis- is a very effective threat intel tool that, combined with rich and accurate data from the OSINT and CSINT databases—can detect malicious activity. This algorithmic approach is part of the existing set of Threat Jammer services.

Threat Jammer can calculate a more precise risk score in the Assessment Engine using the heuristic analysis with the help of OSINT and CSINT datasets. Moreover, it can be customized to obtain a risk score for a specific menace when applying dataset-specific searches would bypass the threat. 

### Machine Learning

The amount of data available in the Threat Jammer backend is massive. We have enough data to train different machine learning models to detect malicious activity. Threat Jammer uses the data obtained from OSINT and CSINT sources and the users' behavior, heuristic analysis, and automatic data report to train machine learning models.

The Assessment Engine can use these pre-trained models as another predictive analysis tool to detect malicious activity and return a risk score based on the results. Adding a pre-trained model to the Threat Jammer system is a great way to increase the accuracy of the risk score and reduce the false positives.


### Crowdsourced Intelligence

Crowdsourced Intelligence means that our users can report their data to the Threat Jammer system. When users feed Threat Jammer with their data using the [Report API](/docs/introduction-threat-jammer-report-api), they increase the chance of finding malicious activity and reducing the false positives. 

If a user reports malicious activity, Threat Jammer will assess the data and decide whether aggregate the data to the crowdsourced intelligence database. If the data has enough quality and is ready to consume, Threat Jammer will share it with the community.