+++
title = "Bot Protection"
date = 2020-07-08T14:37:59+03:00
weight = 40
+++

Our application APIs at the moment are published only for consumption by browsers.  
Therefor we don't want to allow any kind of automated tool to access these endpoints.  
We are going to block not only malicious bots but also tools like curl.


1. Try and access the stock microservice and get data with curl.
```
curl -k https://$nginx_ingress/v1/stock/ticker/all
```

The request will succeed.

2. Apply the bot config

```
cat << EOF | kubectl apply -f -
apiVersion: appprotect.f5.com/v1beta1
kind: APPolicy
metadata: 
  name: attacksigs
spec:
  policy:
    name: attacksigs
    template:
      name: POLICY_TEMPLATE_NGINX_BASE
    applicationLanguage: utf-8
    enforcementMode: blocking
    signature-sets:
    - name: All Signatures
      block: true
      alarm: true
    # The bellow config is in charge of defining what bot to block and whom to allow 
    bot-defense:
      settings:
        isEnabled: true
      mitigations:
        classes:
        - name: trusted-bot
          action: alarm
        - name: untrusted-bot
          action: block
        - name: malicious-bot
          action: block
EOF
```

2. Try again in a few seconds to access the stock microservice and get data with curl.
```
curl -k https://$nginx_ingress/v1/stock/ticker/all
```

This time the request has been blocked. Take the Support Id and look for the logs in Kibana
