---
title: 'Threat Jammer website'
excerpt: 'The Threat Jammer site is a web application that allows anyone to query resources and manage some basic features.'
coverImage: ''
created: '2021-01-12'
updated: '2021-02-24'
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

## Risk assessment without being registered (anonymous search)

### Search query 
The website can perform a risk assessment without being registered. When a user enters an IP address, the website will perform a risk assessment and return the risk level of the IP address in a visual way. It will also return information about the Autonomous Systems (AS) that are associated to the IP address.

![Threat Jammer Anonymous Risk Assessment](/docsimg/risk-assessment-anonymous.png)

### Risk score
The information returned by the risk assessment is the same as the information returned by the endpoints of the User API [Data Assessment](https://dublin.api.threatjammer.com/docs#/Data%20assesment). The guage chart will display the risk score of the IP address in the range of 0 to 100. A human readable score is calculated by the following formula: 

| Range | Risk level |
|-------|------------|
| `0 to 34` | Low|
| `35 to 68` | Medium |
| `69 to 100` | High |

### Ban or report a False Positive
A registered user can ban or report a False Positive IP address. The buttons are grayed and disabled when the user is not logged in. See below when the user is logged in.

### Risk found
The website will also display what type of risk was found based on the information returned by the User API. It will display the information as follows:

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

![Threat Jammer Twitter Risk Assessment](/docsimg/risk-assessment-twitter.png)

### Assessment details
The website can also display to registered users multiple details of the IP address from different sources. The user needs to be logged in to see  the details. A user can sign up for a free account in the website and then the user can see the details of the IP address. You can sign up now clicking on the **Sign up** button at any time.

## Risk assessment with a registered user

### Search query 
The main difference between a registered and anonymous search is the details displayed in the tabs section below the general risk information shown. A registred user can query an IP address from the search box at the top of the page. All the result details of the query will consume users quota.

![Threat Jammer Registered Risk Assessment](/docsimg/risk-assessment-registered.png)

### Ban an IP address
A registered user can ban an IP address clicking on the **Report this IP** button. When a user bans an IP address, it will be included in the private list of reported or banned IP addresses of the user. In the [Report API](/docs/introduction-threat-jammer-report-api), the endpoints [`/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/push_ip_address_v1_ip_post) and [`/unban/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/unban_ip_address_v1_unban_ip_post) let users ban and unban IP addresses. The management of the banned IP addresses is done in the [User API](/docs/introduction-threat-jammer-user-api) with the endpoints [`/denylist/reported/ip/*`](https://dublin.api.threatjammer.com/docs#/Denylist%20data%20query%20and%20management). 

A banned IP address will score a risk level of `High` and will bypass any other risk assessment tests. By default, an IP address banned from the website will be in the `Banned` status for the next 24 hours.

### Report a False Positive
A registered user can report a False Positive IP address clicking on the **False Positive** button. When a user reports a False Positive IP address, it will be included in the private allowlist of addresses of the user. In the [Report API](/docs/introduction-threat-jammer-report-api), the endpoints [`/false/add/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/add_ip_addresses_to_the_false_positive_database_v1_false_add_ip_post) and [`/false/remove/ip`](http://dublin.report.threatjammer.com/docs#/Endpoints/remove_ip_addresses_from_the_false_positive_database_v1_false_remove_ip_post), users can create custom allowlists for false positives IP addresses automatically. 

A False Positive IP address will score a risk level of `Low` and will shortcut any other risk assessment tests. By default, a False Positive IP address from the website will be in the `Allowlist` status for the next 24 hours.

### Assessment details

The different tabs at the bottom of the screen will display the different details of the IP address. The user can click on the tab to see the details of each group. Each tab contains information referenced in the result of the risk assessment endpoint in the User API.


#### JSON

The JSON tab contains the example request performed with the [CURL](https://curl.se/) tool, and the response returned by the API. A user can copy and paste the content of the `Request` window in a shell with `curl` installed on the system and reply the assessment of the website.


#### Deny Datasets

The `Deny Datasets` tab contains information about the type of abuse committed by the related IP address. It shows a descriptive information about the dataset, and hovering over the dataset name will show more details. 

A `green` shields means the IP address was not identifed as a threat of the dataset. A `red` shield means the IP address was identified as a threat.

The [User API](/docs/introduction-threat-jammer-user-api) has the endpoint group [Platform Datasets](https://dublin.api.threatjammer.com/docs#/Platform%20datasets) with different ways to extract more information about the dataset and the IP address.

#### Deny Sources

The `Deny Sources` tab contains information about what source of data the IP address found. It shows a descriptive information about the source, and hovering over the source name will show more details. 

A `green` shields means the IP address was not identifed as a threat by the source. A `red` shield means the IP address was identified as a threat.

At the right side of the source description there is a red icon displaying the time range of the source. The time range is the time interval when the source was active and can be:

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

The [User API](/docs/introduction-threat-jammer-user-api) has the endpoint group [Data sources](https://dublin.api.threatjammer.com/docs#/Data%20sources) with different methods to extract more details about the source and the IP address.

#### Historical Data

The `Historical Data` tab will show more details about when an IP address show up in a source of data or when it was remove. This information is strongly related to the `Deny Sources` information and can be used to understand the history of the IP address. 

A green icon with an up arrow means the IP address was not found any more in a source and hence it was no longer a threat starting the timestamp indicated. A red icon with a down arrow means the IP address was found in a source and it was a threat starting the timestamp indicated.

#### Network

The `Network` tab will display two sets of information about the IP address. The first set is about the Autonomous System (AS) that owns the IP address, and the second set is about the specific prefix where the IP address was found.

A very relevant information obtained by Threat Jammer is the Risk Score of the Autonomous System and the Prefix owning the IP address. This value can give the user a hint about how safely the provider handle the security in the network.

#### Datacenter

The `Datacenter` tab will display the information about the datacenter where the IP address was found. An IP address is considered part of a Datacenter when it belongs to an entity that owns the IP address and host them in a physical location to deliver hosting or cloud services. A residential or mobile IP address is not considered part of a Datacenter.

Following the same data structure of the Autonomous Systems, the first set is about the Datacenter Provider that owns the IP address, and the second set is about the specific prefix where the IP address was found.

The Risk Score of the Datacenter Provider and the Prefix owning the IP address are very relevant for threat detection. This value can give the user a hint about how safely the datacenter provider manage the activity of their customers.

#### Private lists

The `Private lists` tab will display two different datases:

The **private allowlists** are the IP addresses that are allowed to bypass any kind of assessment process and will always return a `Low` risk score. The different allowlists are:
- False positives
- CIDR blocks
- Countries
- Continents
- Autonomous Systems

The **private denylists** are the IP addresses that are manually banned and will always raise the highest level of risk `High` in the assessment process. The different denylists are:
- Banned IP addresses / Reported IP addresses
- CIDR blocks
- Countries
- Continents
- Autonomous Systems

#### Geolocation

The `Geolocation` tab will display the information about the geolocation of the IP address. If available, it will display information about:

- Latitude and longitude
- Time zone
- Zip code
- City
- Region
- Country
- Continent

There is also a link to a map showing the location of the IP address. We don't embed the map in the website to avoid any kind of tracking with cookies or other technologies. **If you open the link, you will be redirected to a third party website at your own risk.**

## Signup and Login

### Sign up page
To obtain an API key, the developer must sign up at the [Signup page](/api/signup) of the website. Users can create an account with:

- An email and a password.
- A Github account.
- A Google account.
- A Microsoft account.

> **Note: By registering with the email/password or any of the social providers, the users must agree with the [Privacy Policy](https://threatjammer.com/privacy) and the [Terms of Service](https://threatjammer.com/tos).**

![Threat Jammer Signup Page](/docsimg/signup-page.png)

### Verify account
If the user sign up with an email and password, they must verify their email address before being able access. The user will receive an email with a link to verify the email address and the following page:

![Threat Jammer Signup Verify Page](/docsimg/signup-verify.png)

The email to verify the account is sent to the email address used to sign up:

![Threat Jammer Signup Verify Email](/docsimg/signup-verify-email.png)

If the user does not verify the email in 24 hours, the account will be deleted.

> **Note: By registering with any of the social providers, we assume that you have already verified your identity and we will not display any verification step.**

### The Quickstart page
The [Quickstart page](/quickstart) is the page a user see the first time they log in and it is a guide to get started with the Service. The user will also receive the same information in the mailbox. If the user wants to see the Quickstart page again, they can click on the link in the top menu of the page.

![Threat Jammer Signup Quicktsart](/docsimg/signup-quickstart.png)

### The Login page

The user will stay permanently logged in until they log out or not active for 3 days. Regardless of the activity, the users will be forced to login after 30 days. The user can log out by clicking on the [`Logout`](/api/logout) button in the top menu.


## Profile account

### User data
The [Profile account](/profile) page is the page where basic information about the account and the user. To access the profile page, the user must be logged in and click on the dropdown menu at the top right of the page `Profile`.

It contains the following information:

- Nickname and Emal address (information obtained from the Email address).
- Name and Last name: Empty if the user did not provide it. The social login providers can provide this information.
- Monthly newsletter: The user can opt-in or opt-out of the monthly newsletter here. By default, the user is opted-out.
- Quota alerts: The user can opt-in or opt-out of the quota alerts here. By default, the user is opted-in because it's a basic feature of the service. Opting-out will remove the quota alerts from the user's mailbox and will not know when the quota will be reached.

> **Note: We strongly recommend to opt-in for the Monthly newsletter and the Quota alerts.**

![Threat Jammer Profile](/docsimg/profile.png)

### Delete account
The Delete account button will delete the account and all the data associated with it. The user will be asked to confirm the deletion. After confirming the deletion, it will 24 hours to process the request and 30 days to fully delete the account. During this grace period, the user will not be able to access the account, but the user can request to restore the account to support.


