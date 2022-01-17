---
title: 'Threat Jammer tokens'
excerpt: 'The Tokens are the key to access the service.'
coverImage: ''
created: '2021-01-12'
updated: '2021-01-12'
readTime: 2
navigation:
  github: https://github.com/threatjammer/markdown-docs/blob/main/threat-jammer-tokens.md
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

Threat Jammer supports two end-user REST APIs: the User API and the Report API. The end-user uses the User API to interact with the different databases, heuristics, and machine learning processes. Devices use the Report API to interact with Threat Jammer. This document will explain how create the API Key or Token to access the User API and the Report API.

## How to create a token

The current version of Threat Jammer supports only one token per user in the only existing region in [Paris](https://paris.api.threatjammer.com). When a user register in Threat Jammer, he will automatically receive a *token*. The Threat Jammer site has a page with all the information about the *token*:

> [`https://threatjammer.com/keys`](https://threatjammer.com/keys)

This page is only available for users who have registered in Threat Jammer. **So, if you are not a user yet, you can create a token on the [Threat Jammer website](https://threatjammer.com/api/signup).** The signup is free, and we don't take any personal information or the payment method. 

After the signup, the user will enjoy 30 days of trial. After that, the user can opt for a paid plan. The paid plan is available on the [Threat Jammer plans page](https://threatjammer.com/plans). If not, Threat Jammer will downgrade the user to the free plan. It's possible to upgrade to a paid plan at any time.
  
## The token format

The Threat Jammer User and Report token are easy to recognize by humans and machines. Its format should help avoid common mistakes like pushing it to a public repository or using it in a script.

Starting from the left side, the format of the token is as follows:

- Positions 1 and 2: A two-letter code of the company. In our case, `tj`.
- Position 3: The code of the app using the token. In our case, `a`.
- Position 4: A single underscore `_`.   
- Positions 5 to 40: A [base 62 encoding](https://en.wikipedia.org/wiki/Base62) of 178 bits encode of the ID.

The fixed size of the token is 36 (payload) + 4 (prefix) = 40

Example:
```
  tja_ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
  12 -> company code
    3 -> app code
     4 -> separator
      567890123456789012345678901234567890 -> payload
```

The token follows the [token convention defined by Github](https://github.blog/2021-04-05-behind-githubs-new-authentication-token-formats) for its tokens. The rationale to adopt this format is to help the automated safeguards in Github to identify them if somebody publishes in a public repository by mistake.


## The quotas

### The quota values

The user has access to the following quota information:

| Parameter name | Default value | Description |
| ---------------- | -------------- | ------------ |
| `last_month_bucket_init_value` | `Depends on subscription type` | Maximum number of monthly queries. `last_month_bucket_value` maximum value. |
| `last_month_bucket_value` | `0` | The current value of consume quota. |
| `last_month_bucket_refresh` | `Unix timestamp in milliseconds` | Next `last_month_bucket_value` reset. |
| `last_minute_bucket_init_value` | `Depends on subscription type` | Maximum number of per minute queries. |
| `last_minute_bucket_value` | `0` | The current value of rate-limit quota. |
| `last_minute_bucket_refresh` | `Unix timestamp in milliseconds` | Next `last_minute_bucket_value` reset. |
| `last_minute_bucket_refill_ratio` | `Depends on the subscription type` | The number of request per second to add in the per-minute bucket. |

### The consume montly quota

Every time a user interacts with the User API, the quota information is updated, increasing the consumed quota. The time range of the quota is from the moment the user registers in Threat Jammer plus 30 days. When the time range expires, the monthly quota will restart.

All the requests to the User API will update the quota if the token is valid and the HTTP request is successful. If the service rejects the request because the data passed as an argument is invalid, the quota will be updated.

If the service rejects the request because the quota exceeds the limit, the response will be an HTTP 429 (Too Many Requests). The HTTP response will include a `Retry-After` header with seconds to wait before retrying the request.

Depending on the subscription type, the quota limits may vary:

| Subscription type | Monthly Quota |
| ------------------- | ------ |
| Free | `5000` |
| Trial | `10000` |
| Basic | `30000` |
| Premium | `100000` |
| Enterprise | `5000000` |

### The per-minute rate-limit quota

To avoid abuse, the User API is rate-limited. The number of requests per minute implements a bucket system. The bucket is filled every second. The bucket is reset every minute. The bucket is filled with the number of requests per second defined by the `last_minute_bucket_refill_ratio` parameter. Well, I have to define better this algorithm.

If the service rejects the request because the rate-limit quota exceeds the limit, the response will be an HTTP 429 (Too Many Requests). The HTTP response will include a `Retry-After` header with seconds to wait before retrying the request.

Depending on the subscription type, the rate-limit quota limits may vary:

| Subscription type | Per minute Quota |
| ------------------- | ------ |
| Free | `10` |
| Trial | `60` |
| Basic | `300` |
| Premium | `1000` |
| Enterprise | `5000` |

## The Authorization header

A **Bearer authentication** schema protects the User API and Report API. **Bearer authentication** (also called **token authentication**) is an HTTP authentication scheme that involves security tokens called bearer tokens. It authenticates the user. All the different endpoints expect a Bearer token in the Authorization header.

Example:
```
curl -X 'GET'
  'https://REGION.api.threatjammer.com/test'
  -H 'accept: application/json'
  -H 'Authorization: Bearer YOUR_API_KEY'
```

> Note: The endpoint `test` exists only for testing purposes. It is not part of the API, but it will return a 200 HTTP status code if the token is valid.

The header `Authorization` contains the Bearer token. The Bearer token is composed of the following parts:
- The token type: `Bearer`.
- The token: the token generated by the Threat Jammer service.

Don't forget to add the `Bearer` prefix to the token. The service will reject the request if the token is not prefixed with `Bearer` and report the missing prefix.