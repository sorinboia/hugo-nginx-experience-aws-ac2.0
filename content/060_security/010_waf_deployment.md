+++
title = "Waf deployment"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++

1. Create the Nginx WAF config, which can be found in the "files/7waf/waf-config.yaml" file.  

```
kubectl apply -f files/7waf/waf-config.yaml
```

The WAF policy is json based and from the example bellow, you can observe how all the configuration can be changed based on the application needs:

```
{
      "name": "nginx-policy",
      "template": { "name": "POLICY_TEMPLATE_NGINX_BASE" },
      "applicationLanguage": "utf-8",
      "enforcementMode": "blocking",
      "signature-sets": [
      {
          "name": "All Signatures",
          "block": false,
          "alarm": true
      },
      {
          "name": "High Accuracy Signatures",
          "block": true,
          "alarm": true
      }
    ],
      "blocking-settings": {
      "violations": [
          {
              "name": "VIOL_RATING_NEED_EXAMINATION",
              "alarm": true,
              "block": true
          },
          {
              "name": "VIOL_HTTP_PROTOCOL",
              "alarm": true,
              "block": true,
              "learn": true
          },
          {
              "name": "VIOL_FILETYPE",
              "alarm": true,
              "block": true,
              "learn": true
          },
          {
              "name": "VIOL_COOKIE_MALFORMED",
              "alarm": true,
              "block": false,
              "learn": false
          }
      ],
          "http-protocols": [{
          "description": "Body in GET or HEAD requests",
          "enabled": true,
          "learn": true,
          "maxHeaders": 20,
          "maxParams": 500
      }],
          "filetypes": [
          {
              "name": "*",
              "type": "wildcard",
              "allowed": true,
              "responseCheck": true
          }
      ],
          "data-guard": {
          "enabled": true,
              "maskData": true,
              "creditCardNumbers": true,
              "usSocialSecurityNumbers": true
      },
      "cookies": [
          {
              "name": "*",
              "type": "wildcard",
              "accessibleOnlyThroughTheHttpProtocol": true,
              "attackSignaturesCheck": true,
              "insertSameSiteAttribute": "strict"
          }
      ],
          "evasions": [{
          "description": "%u decoding",
          "enabled": true,
          "learn": false,
          "maxDecodingPasses": 2
      }]}
    }
```

2. Deploy ELK in order to be able to visualize and analyze the traffic going through the Nginx WAF

```
kubectl apply -f files/7waf/elk.yaml
```

3. In order to connect to our ELK pod, we will need to find the public address of this service:  

```
kubectl get svc elk-web
```

{{< output >}}
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP                                                                  PORT(S)                                        AGE
elk-web   LoadBalancer   172.20.179.34   a28bd2d8c94214ae0b512274daa06211-2103709514.eu-central-1.elb.amazonaws.com   5601:32471/TCP,9200:32589/TCP,5044:31876/TCP   16h
{{< /output >}}
4. Verify that ELK is up and running by browsing to: `http://[ELK-EXTERNAL-IP]:5601/`.  

{{% notice warning %}}
Please note that it might take some time for the DNS name to become available.
{{% /notice %}}

5. Next, we need to change our deployment configuration so it includes the Nginx WAF
```
kubectl apply -f files/7waf/arcadia-main.yaml
kubectl apply -f files/7waf/arcadia-app2.yaml
kubectl apply -f files/7waf/arcadia-app3.yaml
kubectl apply -f files/7waf/arcadia-backend.yaml
```

All of our services are protected and monitored.