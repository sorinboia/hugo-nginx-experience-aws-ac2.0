+++
title = "Microgateway deployment"
menuTitle = "Microgateway deployment"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++

1. Deploy the microgateway with the following configuration.  

```
cat << EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: microgateway
spec:
  replicas: 1
  selector:
    matchLabels:
      app: microgateway

  template:
    metadata:
      labels:
        app: microgateway
    spec:
      containers:
        - name: microgateway
          image: sorinboia/arcadia-microgateway:v1
          imagePullPolicy: Always
          env:
            - name: ENV_CONTROLLER_API_URL
              value: https://$controller_ip:8443/1.4
            - name: ENV_CONTROLLER_API_KEY
              value: $controller_apikey
          ports:
            - containerPort: 80
            - containerPort: 443
---
apiVersion: v1
kind: Service
metadata:
  name: microgateway
spec:
  selector:
    app: microgateway
  ports:
    - port: 80
      targetPort: 80
      name: http
    - port: 443
      targetPort: 443
      name: https
  externalTrafficPolicy: Local
  type: LoadBalancer
EOF
```

From now on we will only use the Controller GUI do to all of our configuration.  
The end goal will be to expose and protect our APIs both internally within the cluster and externally to other programmers.  

2. Login to the Nginx Controller web UI, click the "N" button in the upper left corner and go to "Infrastructure" -> "Instances".  

You will see the microgateway we just deployed listed. If it is not there wait for about 2 minutes, it might take a little bit of time for the instance to register.

3. Get the EXTERNAL-IP of the microgateway service we just published, we will use it later within our config.  

```
export microhost=$(kubectl get svc microgateway | tr -s " " | cut -d' ' -f4 | grep -v "EXTERNAL-IP") && echo $microhost
```

{{< output >}}
aa2ba08e2b4024a85ba93aa32d0bafac-603500592.eu-central-1.elb.amazonaws.com
{{< /output >}}