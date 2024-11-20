---
title: Information
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## [Market API](https://lzt-market.readme.io)

## API free libraries

* [Node.js](https://github.com/NztForum/node-lzt) 
* [Python](https://github.com/AS7RIDENIED/Lolzteam_Python_Api)
* [C#](https://github.com/fanidamn/LolzMarketAPI)

## Rate limit

20 requests per minute (3 seconds delay between per request).

If you exceed the limit, the response code 429 will be returned to you.

Clients can subscribe to certain events to receive real time ping when data is changed within the system.

The subscription system uses the [WebSub   protocol](https://github.com/w3c/websub) to communicate with hubs and subscribers.

## One Time Token

Any client can generate one time token (OTT) using existing token. The format for OTT is as follow:

**user\_id,timestamp,once,client\_id**

With **user\_id** is the ID of authenticated user; **timestamp** is the unix timestamp for OTT expire time, the OTT will work as long as indicated by **timestamp** or by token expire date, whatever comes first; **client\_id** is the client ID; **once** is md5 of a concentration of **user\_id**, **timestamp**, a valid existing token and the client secret. Example code to generate an OTT:

**Configuration**

* TTL of access token: 180 days
* TTL of authorization code: 30 seconds
* TTL of refresh token: 180 days
* Authorization URI: **/oauth/authorize** (Recommended)
* Access token exchange URI: **/oauth/token** (Disabled)
* Token param name: **oauth\_token**
* Token bearer header name: **Bearer**

## Discoverability

System information and availability can be determined by sending a GET request to **/** (index route). A list of resources will be returned. If the request is authenticated, the revisions of API system and installed modules will also made available for further inspection.

## Common Parameters

### i18n

All API requests accept **locale** parameter to switch user facing messages to specified language. The value must be a valid language code (ISO 639-1) with optional inclusion of a valid country code (ISO 3166-1 alpha 2) separated by hyphen ("-"). If no complete match can be found, a language with the same language code (even with different country code) will be used. In the worst case that there are no installed languages of requested language code, the default language will be used. Since api-2015100401.

### Fields filtering

For API method with resource data like a forum or a thread, the data can be filtered to get interested fields only. The general format is "key.sub\_key.deep\_key" if you want to include/exclude a specific field. The including rules take precedence over excluding ones.

* **fields\_include**: comma-separated list of fields of a resource. For additional fields, it is possible to use wildcard (**\***) to include all default fields before specifying additional ones.
* **fields\_exclude**: comma-separated list of fields of a resource to exclude in the response. Since r2016062001, it is possible to use wildcard as a prefix (e.g. "\*.key") to exclude the field everywhere.

### Resource ordering

For API method with list of resources, the resources can be ordered differently with the parameter **order**. List of supported orders will be specified for each method. The default order will always be **natural**. Most of the time, the natural order is the order of which each resource is added to the system (resource id for example).

### Encryption

For sensitive information like password, encryption can be used to increase data security. For all encryption with key support, the **client\_secret** will be used as the key. List of supported encryptions:

* **aes128**: AES 128 bit encryption (mode: ECB, padding: PKCS#7). Because of algorithm limitation, the binary md5 hash of key will be used instead of the key itself.

### Headers

* **Api-Bb-Code-Chr: !youtube**: Replace multimedia tags (except youtube) in bb code html with **tools/chr** link. Since forum-2018100301.
* **Api-Username-Inline-Style**: Return rich username for **username** fields. Since forum-2018052101.

## Subscriptions

List of supported topics:

* **user\_x** (x is the user\_id of the interested user): receives ping when user data is inserted, updated or deleted. The registered callback will be included in GET **/users/:userId** as parameter **subscription\_callback**.

* **user\_notification\_x** (x is the user\_id of the interested user): receives ping when user gets a new notification. Notification data will be included in the ping. The registered callback will be included in GET **/notifications** as parameter **subscription\_callback**.

* **thread\_post\_x** (x is the thread\_id of the interested thread): receives ping when a post in the thread is inserted, updated or deleted. The registered callback will be included in GET **/posts?thread\_id=x** as parameter **subscription\_callback**.

For supported resources, two **Link** HTTP headers will be included. It is recommended to check for these headers before issuing subscribe request because webmaster can disable some or all types of subscriptions.

`Link: <topic url>; rel="self"`

`Link: <hub url>; rel="hub"`

### Example subscribe request

```
curl -XPOST https://api.zelenka.guru/subscriptions     
     -d 'oauth_token=$token'
     -d 'hub.callback=$callback_url'
     -d 'hub.mode=subscribe'
     -d 'hub.topic=$topic'
```

## Content-Type

application/x-www-form-urlencoded.

You can import the API into Postman using [this](https://google.com) file.
