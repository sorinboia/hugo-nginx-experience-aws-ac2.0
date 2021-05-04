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
apiVersion: k8s.nginx.org/v1
kind: VirtualServer
metadata:
  name: arcadia
spec:
  host: $nginx_ingress  
  upstreams:
    - name: arcadia-users
      service: arcadia-users
      port: 80
    - name: arcadia-login
      service: arcadia-login
      port: 80
    - name: arcadia-stocks
      service: arcadia-stocks
      port: 80
    - name: arcadia-stock-transaction
      service: arcadia-stock-transaction
      port: 80
    - name: arcadia-frontend
      service: arcadia-frontend
      port: 80
  routes:
    - path: /v1/user      
      action:
        pass: arcadia-users
    - path: /v1/login      
      action:
        pass: arcadia-login
    - path: /v1/stock      
      action:
        pass: arcadia-stocks
    - path: /v1/stockt      
      action:
        pass: arcadia-stock-transaction
    - path: /      
      action:
        pass: arcadia-frontend
EOF
```

Note how the various HTTP paths (`/v1/user, /v1/login, /v1/stockt`) are routed by Ingress to the relevant K8s services.  
At this stage the basic install is finished and all that's left is to check the connectivity to the Arcadia web application. Get the public hostname of the exposed `nginx-ingress` service.  

3. Browse to the following location and verify that you can access the site: `http://<INGRESS-EXTERNAL-IP>/`  

4. Login to the application using the following credentials:

{{% notice %}}
Username: satoshi@bitcoin.com  
Password: bitcoin
{{% /notice %}}

At the moment we still have two key features missing:
- We are serving only http, not https. We want our site to be fully secured therefore all communications need to be encrypted
- We are not actively monitoring the health of the pods through the data path


8. Take a look at the `files/4ingress/1arcadia_increase.yaml` file. It increases the number of pods that our services use.  

9. Apply this new configuration.
```
kubectl apply -f files/4ingress/1arcadia_increase.yaml
```

10. Look at the Nginx dashboard and click on "HTTP Upstreams", you can see that right now all services have two members but no health check.