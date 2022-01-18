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

The User API follows a [semantic versioning](https://semver.org/) schema. Given a version number MAJOR.MINOR.PATCH, increment the:

1. MAJOR version when the User API has incompatible changes,
2. MINOR version when we add functionality in a backward-compatible manner, and
3. PATCH version when we make backward-compatible bug fixes.

We don't use additional labels in production environments. 

The current MAJOR version is `1`. You can read the current versions at the top of the page closer to the page's title and in the URI of all the endpoints available.

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
 
Example: Read the ASN information of Google in the region of `Paris` with the API version `v1`. The endpoint is `/asn/{number}` with a `GET` verb. The [full detail of the endpoint is in the documentation of the Live Test site](https://paris.api.threatjammer.com/docs#/Autonomous%20Systems%20information/query_asn_v1_asn__number__get).

```
curl -X 'GET' \
  'https://paris.api.threatjammer.com/v1/asn/15169' \
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

### Succesful responses.

Valid responses can only be 200 HTTP OK codes. All the responses will carry a payload in different formats. JSON will be the default format, and other formats allowed are in the endpoint documentation. Some endpoints allow CSV and customized formats for AWS, for example.

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

## Groups

### 1
### 2
### 3
### 4
### 5
### 6

