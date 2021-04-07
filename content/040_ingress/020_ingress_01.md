+++
title = "Publish the app"
date = 2020-07-08T14:37:59+03:00
weight = 20
+++

Expose all the application services and route traffic based on the HTTP path.
We will start with a basic configuration.  

1. Expose Arcadia to the world.  
  
```
cat << EOF | kubectl apply -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: arcadia  
spec:
  rules:
  - host: $nginx_ingress
    http:
      paths:
      - path: /
        backend:
          serviceName: arcadia-main
          servicePort: 80
      - path: /api/
        backend:
          serviceName: arcadia-app2
          servicePort: 80
      - path: /app3/
        backend:
          serviceName: arcadia-app3
          servicePort: 80
EOF
```

Note how the various HTTP paths (`/, /api/, /app3/`) are routed by Ingress to the relevant K8s services.  
At this stage the basic install is finished and all that's left is to check the connectivity to the Arcadia web application. Get the public hostname of the exposed `nginx-ingress` service.  

3. Browse to the following location and verify that you can access the site: `http://<INGRESS-EXTERNAL-IP>/`  

4. Login to the application using the following credentials:

{{% notice %}}
Username: admin  
Password: iloveblue
{{% /notice %}}

At the moment we still have two key features missing:
- We are serving only http, not https. We want our site to be fully secured therefore all communications need to be encrypted
- We are not actively monitoring the health of the pods through the data path


8. Take a look at the `files/5ingress/2arcadia.yaml` file. It increases the number of pods for our services to two - and also defines how the http health checks will looks like.  

9. Apply this new configuration.
```
kubectl apply -f files/5ingress/2arcadia.yaml
```

10. Look at the Nginx dashboard and click on "HTTP Upstreams", you can see that right now that two HTTP upstreams have 2 members but no health checks are being done.