+++
title = "ELK deployment"
date = 2020-07-08T14:37:59+03:00
weight = 10
+++


1. Deploy ELK in order to be able to visualize and analyze the traffic going through the Nginx App Protect web application firewall

```
kubectl apply -f files/6waf/elk.yaml
```

2. In order to connect to our ELK pod, we will need to find the public address of this service:  

```
kubectl get svc elk-web
```

{{< output >}}
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP                                                                  PORT(S)                                        AGE
elk-web   LoadBalancer   172.20.179.34   a28bd2d8c94214ae0b512274daa06211-2103709514.eu-central-1.elb.amazonaws.com   5601:32471/TCP,9200:32589/TCP,5044:31876/TCP   16h
{{< /output >}}
3. Verify that ELK is up and running by browsing to: `http://[ELK-EXTERNAL-IP]:5601/`.  

{{% notice warning %}}
Please note that it might take some time for the DNS name to become available.
{{% /notice %}}

