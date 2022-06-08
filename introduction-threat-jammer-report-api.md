---
title: 'Introduction to Threat Jammer Report API'
excerpt: 'Developers can use the Report API to automate the reporting of malicious resources asynchronously.'
coverImage: ''
created: '2022-01-12'
updated: '2022-06-08'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/introduction-threat-jammer-report-api.md
  home: /docs/index
  previous: /docs/introduction-threat-jammer-user-api
  next: /docs/threat-jammer-site
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

## Introduction

[Threat Jammer](https://threatjammer.com) supports two end-user REST APIs: the User API and the Report API. The end-user uses the User API to interact with the different databases, heuristics, and machine learning processes. Devices use the Report API to interact with Threat Jammer. This document will explain how to use the Report API and learn about the different services, create a token, interpret quota limits, and create the HTTP request to interact with the API.

## How to connect

The Report API (and also the User API) can run in multiple geographical places called **regions**. A region contains all the technology stack needed to deliver the Threat Jammer experience closer to your location. Here goes the list of regions and the fully qualified domain name for the Report API:

| FQDN | Region | Provider| Enabled? |
|------|--------|---------|----------|
| `dublin.report.threatjammer.com` | Dublin | AWS | Yes |

These regions are **Public**. It means a developer-only needs to obtain an API key when signing up and using the service. **Private Regions** are only available on-demand to selected customers. Don't hesitate to get in touch with sales for more information.

## Credentials

All the endpoints in the different regions only need a valid API key for the specific region. **The API keys are region-specific**. If a developer tries to use an API key of the `dublin.report.threatjammer.com` region in the `virginia.report.threatjammer.com` region or vice versa, it will return an error. Please create a region-specific API key suitable to your needs.

To learn more about the Authentication with the Bearer tokens, please read the chapter [Threat Jammer API keys](/docs/threat-jammer-api-keys).

## The Live Test page

All the endpoints have a Live Test page where developers can test all the different endpoints available. The URI is `/docs` and the full URL is `https://REGION.report.threatjammer.com`. For example, if a developer wants to open the Live Test Page in Dublin, the URL is [https://dublin.report.threatjammer.com/docs](https://dublin.report.threatjammer.com/docs).

## API version

The Report API follows a [semantic versioning](https://semver.org/) schema. Given a version number MAJOR.MINOR.PATCH, increment the:

1. MAJOR version when the User API has incompatible changes,
2. MINOR version when we add functionality in a backward-compatible manner, and
3. PATCH version when we make backward-compatible bug fixes.

We don't use additional labels in production environments. 

The current MAJOR version is `1`. You can read the current versions at the top of the page closer to the page's title and in the URI of all the endpoints available.

![Threat Jammer API version](/docsimg/user-api-version.png)

## Building a HTTP request to the API 

All the Report API endpoints will need the following pieces of information to build a request:

1. The API key. Please learn how to obtain it in the chapter [Threat Jammer API keys](/docs/threat-jammer-api-keys).
2. The region. The region is API-specific. 
3. The version of the API. Currently, it can only be `v1`.
4. Add the HTTP header `'accept: application/json'`.
5. The verb of the endpoint: The Report API only accepts `POST`.
6. The endpoint with the desired service.
7. The requests will include the parameters as a JSON object in the request's body. No query parameters are required.
 
Example: Report the IP version 4 `6.7.8.9` as malicious and ban it for 24 hours in `Dublin` region with the API version `v1`. The endpoint is `/ip` with a `POST` verb. The [full detail of the endpoint is in the documentation of the Live Test site](http://dublin.report.threatjammer.com/docs#/Endpoints/push_ip_address_v1_ip_post).

```
curl -X 'POST' \
  'http://dublin.report.threatjammer.com/v1/ip' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
  "addresses": [
    "6.7.8.9"
  ],
  "ttl": 86400
}'
```

If the request succeeds, the service will return an HTTP code 202 and an empty response. **All the endpoints of the Report API return an empty response and a HTTP code 202 if the request succeeds.**

## Overall description of the HTTP responses

### Succesful responses

The service returns an HTTP code `202` response if the request is accepted. Then, the service will process the request asynchronously sending the information to the abuse reporting engine as soon as possible. The response body is empty. **The service will not return a `200` response code in any case. Assume `202` as the only valid response.**

### Error responses

If the `Bearer` token is not valid, the service returns a `401` response code.

If the object passed in the body is not valid, the service returns a `422` response code. Please check the request format.

There is a limit of `6` hits per minute per token. If the limit is reached, the service returns a `429` response code. Simply wait a minute and try again.

Unhandled error responses will probably return an exception and a stack dump. Please report them to our support team.

## Endpoints

The number of different endpoints available in the Reporting API is smaller than the User API. The Report API implements the internal logic as asyncronously as possible, so the endpoints are not blocking your automated reporting engines in your infrastructure. 

### Ban/unban IP addresses. Building denylists automatically.

The endpoints [`/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/push_ip_address_v1_ip_post) and [`/unban/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/unban_ip_address_v1_unban_ip_post) are used to ban/unban IP addresses. 

When a honeypot like **Cowrie** or a service like [Fail2Ban](/tutorials/how-to-configure-fail2ban-in-ubuntu) detects a set of malicious IP addresses they can report them thanks to the Report API. The ThreatJammer assessment engine will automatically ban the IP addresses with the Time to Live (TTL) passed as argument **only for the user reporting the IP**. The TTL expires, the service will automatically unban the IP addresses. If more users report the same IP address, the ThreatJammer assessment engine will move the IP address to a public `ban` list called `COMMUNITY_REPORTED_LIST`. 

A developer can also unban IP addresses by using the endpoint [`/unban/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/unban_ip_address_v1_unban_ip_post). This endpoint sets the TTL to `0` forcing an immediate expiration of the IP addresss.


**Example:** To ban the IP addresses `6.7.8.9` and `7.8.9.10` for 24 hours (`86400` seconds) in `Dublin` region tagging them of type `ABUSE` and tagging them as `DOCSAMPLE`:

From the command-line:
```bash
curl -X 'POST' \
  'http://dublin.report.threatjammer.com/v1/ip' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ' \
  -H 'Content-Type: application/json' \
  -d '{
  "addresses": [
    "6.7.8.9", "7.8.9.10"
  ],
  "ttl": 86400,
  "type": "ABUSE",
  "tags": ["DOCSAMPLE"]
}'
```

the server returns the HTTP code `202`.

**Example:** To unban the IP addresses `6.7.8.9` and `7.8.9.10` in `Dublin` region:

From the command-line:
```bash
curl -X 'POST' \
  'http://dublin.report.threatjammer.com/v1/unban/ip' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ' \
  -H 'Content-Type: application/json' \
  -d '{
  "addresses": [
    "6.7.8.9", "7.8.9.10"
  ]
}'
```

the server returns the HTTP code `202`.

Both endpoints enqueue the request to the internal queue. The service will process the request asynchronously. It can take a few seconds before the request is processed in the backend and the IP addresses are banned/unbanned. To check the status of the request, use the User API endpoints [`/denylist/reported/ip/*`](https://dublin.api.threatjammer.com/docs#/Denylist%20data%20query%20and%20management). It's also possible to manage the reported IP addresses using the User API synchronously.


### Report false positives IP addresses. Building allowlists automatically.

The endpoints [`/false/add/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/add_ip_addresses_to_the_false_positive_database_v1_false_add_ip_post) and [`/false/remove/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/remove_ip_addresses_from_the_false_positive_database_v1_false_remove_ip_post) are used to create custom allowlists for false positives IP addresses automatically. 

When a user reports a False Positive the service will automatically add it to the **private allowlist** of the user, bypassing any risk assessment. The ThreatJammer assessment engine will ignore the IP addresses during the Time to Live (TTL) passed as argument **only for the user reporting the IP**. When the TTL expires, the service will automatically delete the IP addresses of the allowlist. If more users report the same IP address as a false positive, the ThreatJammer assessment engine will start a reviewing process of the IP address to a public false positive allowlist that will bypass any risk assessment.

A developer can also remove an IP addresses of the allowlist by using the endpoint [`/false/remove/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/remove_ip_addresses_from_the_false_positive_database_v1_false_remove_ip_post). This endpoint sets the TTL to `0` forcing an immediate expiration of the IP addresss.


**Example:** To report as a false positive the IP address `8.8.8.8` for 24 hours (`86400` seconds) in `Dublin` region tagging them of type `ABUSE` and tagging them as `DOCSAMPLE`:

From the command-line:
```bash
curl -X 'POST' \
  'http://dublin.report.threatjammer.com/v1/false/add/ip' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ' \
  -H 'Content-Type: application/json' \
  -d '{
  "addresses": [
    "8.8.8.8"
  ],
  "ttl": 86400,
  "type": "ABUSE",
  "tags": ["DOCSAMPLE"]
}'
```

the server returns the HTTP code `202`.

**Example:** To delete the IP address `8.8.8.8` in `Dublin` region of the private allowlist:

From the command-line:
```bash
curl -X 'POST' \
  'http://dublin.report.threatjammer.com/v1/false/remove/ip' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ' \
  -H 'Content-Type: application/json' \
  -d '{
  "addresses": [
    "8.8.8.8"
  ]
}'
```

the server returns the HTTP code `202`.

Both endpoints enqueue the request to the internal queue. The service will process the request asynchronously. It can take a few seconds before the request is processed in the backend and the IP addresses are processed. To check the status of the request, use the User API endpoints [`/allowlist/*`](https://dublin.api.threatjammer.com/docs#/Denylist%20data%20query%20and%20management). It's also possible to manage the reported IP addresses using the User API synchronously.

## Global database and region allowlists

The Report API is deployed in different regions and each region can only be accessed by a user with a specific regional API key. A regional API will report IP addresses to the global allowlist and denylist no matter the region. A user with an API key in Dublin and Virginia will be able to read the IP addresses reported by each other. 

## Understanding the Report API quotas

**The Report API is free to use and it does not impact on the quotas of the User API**. A user can report as many resources as needed and the User quota will not decrease. Even an API Key with the User quota exhausted can be used to report resources. Each regional Report API endpoint has a limit of `6` hits per minute. We recommend to group several requests on a ten seconds interval and then invoke the API endpoints.

## Downloading the reported IP addresses

ThreatJammer is only a custody of your data, so you can download the reported IP addresses in JSON, CSV or [AWS WAF format](https://docs.aws.amazon.com/waf/latest/APIReference/API_CreateIPSet.html) and import them in your own systems. The endpoints in the User API to download the reported IP addresses are [`/denylist/reported/ip`](https://dublin.api.threatjammer.com/docs#/Denylist%20data%20query%20and%20management/query_all_the_ip_addresses_reported_by_the_user_v1_denylist_reported_ip_get) and [`/allowlist/cidr`](https://dublin.api.threatjammer.com/docs#/Allowlist%20data%20query%20and%20management/query_all_the_ip_addresses_manually_entered_by_the_user_in_the_IP_addresses_allowlist_v1_allowlist_cidr_get).


## What's next

We recommend starting testing our API in the [Live Test site](https://dublin.report.threatjammer.com/docs) first, and then read some of the [Tutorials available](/tutorials) in our site to get a better understanding of all the capabilities of the service.

Visiting the [community site](/community) is also an excellent place to ask for help, or our support services.



