---
title: 'How Threat Jammer works'
excerpt: 'A high level description of the different services available.'
coverImage: ''
created: '2021-01-12'
updated: '2021-01-12'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/how-threat-jammer-works.md
  home: /docs/index
  previous: /docs/what-is-threat-jammer
  next: /docs/threat-jammer-tokens
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

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