---
title: 'Introduction to Threat Jammer User API'
excerpt: 'Developers can use the User API interact with the different databases, heuristics and machine learning processes.'
coverImage: ''
created: '2022-01-12'
updated: '2022-07-01'
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

[Threat Jammer](https://threatjammer.com) supports two end-user REST APIs: the User API and the Report API. The end-user uses the User API to interact with the different databases, heuristics, and machine learning processes. Devices use the Report API to interact with Threat Jammer. This document will explain how to use the User API and interact with the different services, create a token, interpret the quota information, and create the HTTP request to interact with the User API.

## How to connect

The User API can run in multiple geographical places called **regions**. A region contains all the technology stack needed to deliver the Threat Jammer experience closer to your location. Here goes the list of regions and each fully qualified domain name:

| FQDN | Region | Provider| Enabled? |
|------|--------|---------|----------|
| `dublin.api.threatjammer.com` | Dublin | AWS | Yes |


These regions are **Public**. It means a developer-only needs to obtain an API key when signing up and using the service. **Private Regions** are only available on-demand to selected customers. Don't hesitate to get in touch with sales for more information.

## Credentials

All the endpoints in the different regions only need a valid API key for the specific region. **The API keys are region-specific**. If a developer tries to use an API key of the `dublin.api.threatjammer.com` region in the `virginia.api.threatjammer.com` region or vice versa, it will return an error. Please create a region-specific API key suitable to your needs.

To learn more about the Authentication with the Bearer tokens, please read the previous chapter [Threat Jammer API keys](/docs/threat-jammer-api-keys).

## The Live Test page

All the endpoints have a Live Test page where developers can test all the different endpoints available. The URI is `/docs` and the full URL is `https://REGION.api.threatjammer.com`. For example, if a developer wants to open the Live Test Page in Dublin, the URL is [https://dublin.api.threatjammer.com/docs](https://dublin.api.threatjammer.com/docs).

The Threat Jammer site also has a direct access from the dropdown menu:

![Threat Jammer Test Live menu access](/docsimg/test-live-menu.png)


## API version

The User API follows a [semantic versioning](https://semver.org/) schema. Given a version number MAJOR.MINOR.PATCH, increment the:

1. MAJOR version when the User API has incompatible changes,
2. MINOR version when we add functionality in a backward-compatible manner, and
3. PATCH version when we make backward-compatible bug fixes.

We don't use additional labels in production environments. 

The current MAJOR version is `1`. You can read the current versions at the top of the page closer to the page's title and in the URI of all the endpoints available.

> Note: The User API is in General Availability (GA), but the overall service is still in Beta. So only the `Free` subcription type is available. Other subscription types will be available when the API changes to GA version.

![Threat Jammer API version](/docsimg/user-api-version.png)

## Building a HTTP request to the API 

All the User API endpoints will need the following pieces of information to build a request:

1. The API key. Please learn how to obtain it in the previous chapter [Threat Jammer API keys](/docs/threat-jammer-api-keys).
2. The region. The region is API-specific. 
3. The version of the API. Currently, it can only be `v1`.
4. Add the HTTP header `'accept: application/json'`.
5. The verb of the endpoint: `POST`, `GET`, `PUT` or `DELETE`.
6. The endpoint with the desired service.
7. The requests with `GET` and `DELETE` verbs will have parameters in the Querystring.
8. The requests with `POST` and `PUT` verbs will include the parameters as a JSON object in the request's body.
 
Example: Read the ASN information of Google in the region of `Dublin` with the API version `v1`. The endpoint is `/asn/{number}` with a `GET` verb. The [full detail of the endpoint is in the documentation of the Live Test site](https://dublin.api.threatjammer.com/docs#/Autonomous%20Systems%20information/query_asn_v1_asn__number__get).

```
curl -X 'GET' \
  'https://dublin.api.threatjammer.com/v1/asn/15169' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY'
```

If the request succeeds, the service will return an HTTP code 200 and a JSON object with the information. For a complete description of the JSON object, read the documentation in the Live Test site.

```json
{
  "self":"/v1/asn/15169",
  "name":"GOOGLE",
  "description":"",
  "country_code":"US",
  "registry_date":"20000330",
  "registry":"/v1/asn/registry/arin",
  "status":"/v1/asn/status/assigned",
  "prefixes":"/v1/asn/15169/prefixes",
  "score":0,
  "risk":"LOW"
}
```

## Overall description of the HTTP responses

### Succesful responses

Valid responses can only be a 200 HTTP OK code. All the responses will carry a payload in different formats. JSON will be the default format, and other formats allowed are in the endpoint documentation. Some endpoints allow CSV and customized formats for AWS, for example.

The JSON object can contain four different types of values:

- the `self` reference to the object. It plays the role of a unique ID.
- references to other objects reachable thanks to other API calls to other endpoints. In our example, the list of prefixes of the ASN is a different endpoint.
- Values as strings, integers, or floats.
- A nested array of JSON objects.

### Error responses

All handled error responses will return a 4xx HTTP error code. It will also produce a JSON object with the following structure:

```JSON
{
  "title": SHORT_DESCRIPTION_OF_THE_ERROR,
  "detail": MORE_DETAILS_ABOUT_THE_ERROR,
  "type:": LINK_TO_A_LONGER_DESCRIPTION_OF_THE_ERROR
}
```

Unhandled error responses will probably return an exception and a stack dump. Please report them to our support team.

## Service Groups

The number of different endpoints available in the User API are constantly growing. They have been grouped by common use cases. 

![Threat Jammer User API endpoint groups](/docsimg/user-api-groups.png)

### Data assesment

[The Data assessment endpoints](https://dublin.api.threatjammer.com/docs#/Data%20assesment) in this group answer the following questions:

> How risky is a resource and why?

The different endpoints process the resources and return the highest risk found in why a specific test is relevant. 

**Example:** [Get the risk score and the different factors of an IP address](https://dublin.api.threatjammer.com/docs#/Data%20assesment/assess_ip_v1_asses_ip__ip_address__get).

From the command-line:
```bash
curl -X 'GET' \
  'https://dublin.api.threatjammer.com/v1/assess/ip/212.231.12.22' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ'
```

From the Live Test site:
![Threat Jammer User API Assess](/docsimg/user-api-assess.png)



the JSON object returned:

```JSON
{
  "self": "/v1/assess/ip/212.231.12.22",
  "score": 70,
  "risk": "MEDIUM",
  "datasets": [
    "/v1/dataset/ip/FORUM_ACCOUNT_ABUSE"
  ],
  "sources": [
    "/v1/source/ip/STOPFORUMSPAM_TOXIC/range/7D"
  ],
  "first_appearance": [
    "/v1/log/ip/id/3394366"
  ],
  "last_appearance": [
    "/v1/log/ip/id/4249652"
  ],
  "asn": "/v1/asn/15704",
  "asn_prefix": "/v1/asn/prefix/212.231.0.0.0%252F16",
  "reason": "Found in one or more denylist datasets.",
  "reported": "",
  "denylisted": "",
  "allowlisted": "",
  "datacenter": ""
}
```

The computed risk score obtained is `70` with a `MEDIUM` risk because the Assessment Engine found the IP address in a denylist database. This information can give the user enough confidence to make an informed decision at their side.

Suppose the user wants to go deeper into the assessment decision process. In that case, it can query the information about:
- what datasets were affected
- the name of the denylists
- when IP addresses appeared
- the Autonomous System
- the Datacenter provider
- if the user reported it
- or if the user entered the IP address in private deny or allow lists.

The developer will have to follow the links given and build a larger picture of the potential threat to access this information.

There are endpoints that allow batch processing of the resources posting a large JSON object or a CVS file.

### Data logging

[The Data logging endpoints](https://dublin.api.threatjammer.com/docs#/Data%20logging) in this group give insights about when a resource appeared in the database or when it left the database. 

This list of changes is an influential factor in the risk score calculation. It's also a piece of valuable information in threat intel, forensics, and data analysis.

Datasets can filter the information returned by the endpoint and the first date found. 

### Autonomous System information

[The Autonomous System information endpoints](https://dublin.api.threatjammer.com/docs#/Autonomous%20Systems%20information) has information about the prefixes of the internet AS providers plus a pre-calculated risk score based on the activity of the networks and the frequency of report of the IP addresses in suspicious activities.

The AS has a risk and score pre-calculated, and each IPv4 and IPv6 prefix found have also a pre-calculated risk and score for a fine-grained assessment.

### Datacenter Information
[The Datacenter information endpoints](https://dublin.api.threatjammer.com/docs#/Datacenter%20information) have information about datacenter-only networks ranges and their owners. Following the AS approach, it also has a pre-calculated risk score based on the activity of the networks and the frequency of reports of the IP addresses in suspicious activities. 

The Datacenter has risks and scores pre-calculated, and each IPv4 and IPv6 network found has a pre-calculated risk and score for a fine-grained assessment. 

AS and Datacenter endpoints can look the same but are not the same. Sometimes the risk factors can be the same, but they should be interpreted as different signals.

### Platform datasets

[The Platform datasets endpoints](https://dublin.api.threatjammer.com/docs#/Platform%20datasets) is a superset of different sources of data that Threat Jammer classify by the type of suspicious activity they perform. Examples of Platform Datasets are:

| Platform datasets | Description |
| ------------------- | ------ |
| `ABUSE` | IP addresses reported abusive actions. |
| `ANONYMOUS_PROXY` | Anonymous Proxy providers. |
| `ANONYMOUS_VPN` | Anonymous VPN providers. |
| `BOGONS` | IP addresses and CIDRs of BOGONS. |
| `FORUM_ACCOUNT_ABUSE` | IP addresses reported abusive actions in public forums. |
| `REPUTATION` | IP addresses reported malicious actions. |
| `TOR` | Tor, The Onion Router. |

### Data sources

[The Data sources endpoints](https://dublin.api.threatjammer.com/docs#/Data%20sources) return information about OSINT or CSINT denylists that Threat Jammer uses to calculate the risk and score in the assessment processes.

Threat Jammer picks several relevant time frames for each Data source depending on how frequently the data changes. It can range from 1 hour to 365 days. A Data source with a 1-hour time range will contain the resources found in the last hour. A Data source with 365 days time range will include the resources found in the last 365 days. The time ranges are:

| Time ranges| Description |
| ---------- | ----- |
| `1H` | Last hour |
| `6H` | Last six hours |
| `12H` | Last 12 hours |
| `1D` | Last 24 hours |
| `7D` | Last seven days |
| `30D` | Last 30 days |
| `90D` | Last 90 days |
| `180D` | Last 180 days |
| `365D` | Last 365 days |

Other relevant information about the Data sources are:
- The Dataset it belongs to.
- Where it was found.
- How often does it change.
- The minimum risk and score in the time range.
- The maximum risk and score in the time range.

### Denylist data query and management

[Denylist data query and management](https://dublin.api.threatjammer.com/docs#/Denylist%20data%20query%20and%20management) lets users manage their private denylists of resources. **The Assessment Engine will classify the resource with the maximum risk and score if there is a match during the assessment process. Maximium risk.** 

There are two different sets of features:

1. Reported IP quey and data management
2. Private denylist IP query and data management

A user can only insert into the Reported IP addresses storage from the [Report API](/docs/introduction-threat-jammer-report-api). The Report API design allows fast asynchronous communication from external devices like Honeypots, CDNs with serverless functions, and other threat detection agents that need to publish the information in a non-blocking way. A user can manage these resources with this endpoint.

Thanks to the Private denylist IP query and data management, a user can insert, update, delete or retrieve any IP address using the API. Threat Jammer will handle this set differently from the Reported IP addresses.

### Allowlist data query and management

[Allowlist data query and management](https://dublin.api.threatjammer.com/docs#/Allowlist%20data%20query%20and%20management) lets users manage their private allowlist of resources. **The Assessment Engine will classify the resource with the lowest risk and score if there is a match during the assessment process. All the assessment processes will stop if the resource is in the whitelist discarding any other factors.**

A user can insert, update, delete or retrieve any IP address using the API. 

### Public Allowlist data query and management

[Public allowlist data query and management]() lets users manage what public datasets of resources will not take part in the assessment process of the Assessment Engine. 

Thanks to this feature, a company could bypass any kind of assessment of the traffic coming from:
- Apple Private Relay
- Google Bots
- Tor
- etc.

### Geolocation

[Geolocation](https://dublin.api.threatjammer.com/docs#/Geolocation) lets users obtain an accurate location of an IP address. It also allows batch processing or CVS file upload.


## What's next

We recommend starting testing our API in the [Live Test site](https://dublin.api.threatjammer.com/docs) first, and then read some of the [Tutorials available](/tutorials) in our site to get a better understanding of all the capabilities of the service.

Visiting the [community site](/community) is also an excellent place to ask for help, or our support services.



