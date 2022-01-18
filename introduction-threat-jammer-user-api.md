---
title: 'Introduction to Threat Jammer User API'
excerpt: 'Developers can use the User API interact with the different databases, heuristics and machine learning processes.'
coverImage: ''
created: '2021-01-12'
updated: '2021-01-12'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/introduction-threat-jammer-user-api.md
  home: /docs/index
  previous: /docs/threat-jammer-api-keys
  next: /docs/introduction-threat-jammer-report-api
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

## Introduction

Threat Jammer supports two end-user REST APIs: the User API and the Report API. The end-user uses the User API to interact with the different databases, heuristics, and machine learning processes. Devices use the Report API to interact with Threat Jammer. This document will explain how to use the User API and interact with the different services, create a token, interpret the quota information, and create the HTTP request to interact with the User API.

## How to connect

The User API can run in multiple geographical places called **regions**. A region contains all the technology stack needed to deliver the Threat Jammer experience closer to your location. Here goes the list of regions and each fully qualified domain name:

| FQDN | Region | Provider| Enabled? |
|------|--------|---------|----------|
| `nuremberg.api.threatjammer.com` | Nuremberg | Hetzner TESTING | Yes |
| `paris.api.threatjammer.com` | Paris | Amazon Web Services | Yes |
| `virginia.api.threatjammer.com` | Virginia | Amazon Web Services | No |
| `california.api.threatjammer.com` | California | Amazon Web Services | No |

These regions are **Public**. It means a developer-only needs to obtain an API key when signing up and using the service. **Private Regions** are only available on-demand to selected customers. Don't hesitate to get in touch with sales for more information.

## Credentials

All the endpoints in the different regions only need a valid API key for the specific region. **The API keys are region-specific**. If a developer tries to use an API key of the `paris.api.threatjammer.com` region in the `virginia.api.threatjammer.com` region or vice versa, it will return an error. Please create a region-specific API key suitable to your needs.

To learn more about the Authentication with the Bearer tokens, please read the previous chapter [Threat Jammer API keys](/docs/threat-jammer-api-keys).

## The Live Test page

All the endpoints have a Live Test page where developers can test all the different endpoints available. The URI is `/docs` and the full URL is `https://REGION.api.threatjammer.com`. For example, if a developer wants to open the Live Test Page in Paris, the URL is [https://paris.api.threatjammer.com/docs](https://paris.api.threatjammer.com/docs).

The Threat Jammer site also has a direct access from the dropdown menu:

![Threat Jammer Test Live menu access](/docsimg/test-live-menu.png)


## API version

## Groups

### 1
### 2
### 3
### 4
### 5
### 6

