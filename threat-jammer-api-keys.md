---
title: 'Threat Jammer API keys'
excerpt: 'The API Keys are the authorization method to access the service.'
coverImage: ''
created: '2022-01-12'
updated: '2022-02-10'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/threat-jammer-api-keys.md
  home: /docs/index
  previous: /docs/what-is-threat-jammer
  next: /docs/introduction-threat-jammer-user-api
authors:
  - name: Diego Parrilla
    link: 'https://github.com/orgs/threatjammer/people/diegoparrilla'
ogImage:
  url: ''
---

## Introduction

Threat Jammer supports two end-user REST APIs: the User API and the Report API. The end-user uses the User API to interact with the different databases, heuristics, and machine learning processes. Devices use the Report API to interact with Threat Jammer. This document will explain how create an API Key to access the User API and the Report API.

## How to create a token

The current version of Threat Jammer supports only one API key per user in the only existing region in [Paris](https://dublin.api.threatjammer.com). When a user register in Threat Jammer, he will automatically receive an *API Key*. The Threat Jammer site has a page with all the information about the *API Keys*:

> [`https://threatjammer.com/keys`](https://threatjammer.com/keys)

This page is only available for users who have registered in Threat Jammer. **So, if you are not a user yet, you can create an API key on the [Threat Jammer website](https://threatjammer.com/api/signup).** The signup is free, and we don't take any personal information or the payment method. 

![Threat Jammer API Key menu](/docsimg/api-keys-menu.png)

After the signup, the user will enjoy 30 days of trial. After that, the user can opt for a paid plan. The paid plan is available on the [Threat Jammer plans page](https://threatjammer.com/plans). If not, Threat Jammer will downgrade the user to the free plan. It's possible to upgrade to a paid plan at any time.
  
## The API key format

The Threat Jammer User and Report API key are easy to recognize by humans and machines. Its format should help avoid common mistakes like pushing it to a public repository or using it in a sharable script.

Starting from the left side, the format of the API key is as follows:

- Positions 1 and 2: A two-letter code of the company. In our case, `tj`.
- Position 3: The code of the app using the token. In our case, `a`.
- Position 4: A single underscore `_`.   
- Positions 5 to 40: A [base 62 encoding](https://en.wikipedia.org/wiki/Base62) of 178 bits encode of the ID.

The fixed size of the API key is 36 (payload) + 4 (prefix) = 40

Example:
```
  tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
  12 -> company code
    3 -> app code
     4 -> separator
      567890123456789012345678901234567890 -> payload
```

The API key follows the [token convention defined by Github](https://github.blog/2022-04-05-behind-githubs-new-authentication-token-formats) for its tokens. The rationale to adopt this format is to help the automated safeguards in Github to identify them if somebody publishes in a public repository by mistake.


## The quotas

### The quota values

The user has access to the following quota information:

| Parameter name | Description |
| ---------------- | ------------- |
| `last_month_bucket_init_value` | Maximum number of monthly queries. `last_month_bucket_value` maximum value. |
| `last_month_bucket_value` | The current value of consume quota. |
| `last_month_bucket_refresh` | Next `last_month_bucket_value` reset. |
| `last_minute_bucket_init_value` | Maximum number of per minute queries. |
| `last_minute_bucket_value` | The current value of rate-limit quota. |
| `last_minute_bucket_refresh` | Next `last_minute_bucket_value` reset. |
| `last_minute_bucket_refill_ratio` | The number of request per second to add in the per-minute bucket. |

### The consume montly quota

Every time a user interacts with the User API, the quota information is updated, increasing the consumed quota. The time range of the quota is from the moment the user registers in Threat Jammer plus 30 days. When the time range expires, the monthly quota will restart.

All the requests to the User API will update the quota if the token is valid and the HTTP request is successful. If the service rejects the request because the data passed as an argument is invalid, the quota will be updated.

If the service rejects the request because the quota exceeds the limit, the response will be an HTTP 429 (Too Many Requests). The HTTP response will include a `Retry-After` header with seconds to wait before retrying the request.

During the early phases of the project, the quota will be limited as follows:

| Subscription type | Monthly Quota |
| ------------------- | ------ |
| Free | `5000` |

> Note: The API is still in beta, so only the `Free` type is available. Other subscription types will be available when the API changes to GA version.

### The per-minute rate-limit quota

To avoid abuse, the User API is rate-limited. The number of requests per minute implements a bucket system. The bucket is filled every second. The bucket is reset every minute. The bucket is filled with the number of requests per second defined by the `last_minute_bucket_refill_ratio` parameter. Well, I have to define better this algorithm.

If the service rejects the request because the rate-limit quota exceeds the limit, the response will be an HTTP 429 (Too Many Requests). The HTTP response will include a `Retry-After` header with seconds to wait before retrying the request.

During the early phases of the project, the rate-limit will be limited as follows:

| Subscription type | Per minute Quota |
| ------------------- | ------ |
| Free | `10` |

> Note: The API is still in beta, so only the `Free` type is available. Other subscription types will be available when the API changes to GA version.

## The Authorization header

A **Bearer authentication** schema protects the User API and Report API. **Bearer authentication** (also called **token authentication**) is an HTTP authentication scheme that involves security tokens called **bearer tokens**. All the different endpoints expect a Bearer token in the Authorization header.

Example:
```
curl -X 'GET' \
  'https://REGION.api.threatjammer.com/test' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
```

> Note: The endpoint `test` exists only for testing purposes. It is not part of the API, but it will return a 200 HTTP status code if the token is valid.

The header `Authorization` contains the Bearer token. The Bearer token is composed of the following parts:
- The token type: `Bearer`.
- The token: the API key generated by the Threat Jammer service.

Don't forget to add the `Bearer` prefix to the API key. The service will reject the request if the token is not prefixed with `Bearer` and it will report the missing prefix.

## Reading the consumed and quotas limits

There are two ways to read the consumed and quotas limits:

1. Use the [Threat Jammer site](https://threatjammer.com/keys) to view all the information about API keys in real-time.
2. Use the User API directly.
3. Use the User API from the live testing endpoint.


Visiting the site is the most straightforward way to read the consumed and quotas limits of the API keys, and it does not need any explanation. 

![Threat Jammer API keys page](/docsimg/api-keys-page.png)


### The User API

The User API is the most convenient way to read the consumed and quotas limits with full detail. In our example, Threat Jammer created the API key in the `aws-eu-west-3` region, so the fully qualified domain name of the URL will be `dublin.api.threatjammer.com`.

Example:
```
curl -X 'GET' \
  'https://dublin.api.threatjammer.com/v1/token' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY'
```

The response will be a JSON object with all the fields described in [The quota values section](#the-quota-values).

```json
{
  "self":"/v1/token/tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ",
  "region_id":"aws-eu-west-3",
  "last_month_bucket_init_value":1000000,
  "last_month_bucket_value":1,
  "last_month_bucket_refresh":1644278400000,
  "last_minute_bucket_init_value":100,
  "last_minute_bucket_refill_ratio":1,
  "last_minute_bucket_value":0,
  "last_minute_bucket_refresh":1642504981000,
  "status":"ENABLED",
  "created_at":1636328648000,
  "updated_at":1642504981000
}
```

### The live testing User API site 

Each User API endpoint has a live testing site at `https://REGION.api.threatjammer.com/docs`. The live testing site lets developers test the User API with their API keys and verify how things work beforehand. Each region has its live testing site, so the developer has to change the `REGION` part of the fully qualified domain name according to the region of your API key. Then, they can open the URL in any browser and start testing the User API.

![Threat Jammer Live Test Authorize button](/docsimg/test-live-authorize.png)

As described above, a Bearer token protects all the endpoints of the User API. Each of the endpoints in the API show an **open lock** at the right-hand side. To close the lock and test all the endpoints, the developer has to click on the **Authorize** green button at the top of the page. A form with a single textbox will appear. The developer must enter the API key (only the API key, without the `Bearer` prefix) and click on the **Authorize** button. The locks will close, and the developer can start testing any User API endpoint.

![Threat Jammer Live Test API Key token info](/docsimg/api-keys-token-info.png)

Search for the endpoints named `Token management in this region` and try the only endpoint `/v1/token` available. The response will be the JSON object with all the fields described in [The quota values section](#the-quota-values).

Live to test the User API is the most straightforward way to learn how the Threat Jammer works. Save the URL in your browser and open it at any time.
