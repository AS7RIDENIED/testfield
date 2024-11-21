---
title: Add Proxy
excerpt: >-
  Add single proxy or proxy list.



  To add single proxy use this parameters:



  + **proxy_ip** (required) - proxy ip or host

  + **proxy_port** (required) - proxy port

  + **proxy_user** (optional) - proxy username

  + **proxy_pass** (optional) - proxy password


  To add proxy list use this parameters:



  + **proxy_row** (required) - proxy list in String format ip:port:user:pass.
  Each proxy must be start with new line (use \n separator)
api:
  file: market.json
  operationId: proxySettings.add
hidden: false
---