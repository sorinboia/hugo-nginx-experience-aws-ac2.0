+++
title = "Bot Protection"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++


1. Apply the bot config

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
