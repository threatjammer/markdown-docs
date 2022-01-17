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
  next: /docs/how-threat-jammer-works
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
