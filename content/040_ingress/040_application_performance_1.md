+++
title = "Speed up application performance and enable caching"
menuTitle = "Application performance"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++

1. Apply the bellow configuration. We are telling Nginx to create a caching entity that will be used by our Ingress.  

```
cat << EOF | kubectl apply -f -
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-config
  namespace: nginx-ingress
data:
  proxy-protocol: "True"
  real-ip-header: "proxy_protocol"
  set-real-ip-from: "0.0.0.0/0"
  http-snippets  : |
    proxy_cache_path /var/tmp/a levels=1:2 keys_zone=my_cache:10m max_size=100m inactive=60m use_temp_path=off;
EOF
```

2. Configure the Nginx Ingress to start using it and start caching.  
  
```
cat << EOF | kubectl apply -f -
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: arcadia
  annotations:
    nginx.com/health-checks: "true"
    ingress.kubernetes.io/ssl-redirect: "true"
    nginx.org/server-snippets: |
      proxy_ignore_headers X-Accel-Expires Expires Cache-Control;
      proxy_cache_valid any 30s;
    nginx.org/location-snippets: |      
      proxy_cache my_cache;
      add_header X-Cache-Status \$upstream_cache_status;

spec:
  tls:
  - hosts:
    - $nginx_ingress
    secretName: arcadia-tls
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



3. We have two simple indicators to check that all is working:  
- First if we open the browser developer tools we can see a new http header in the response called "X-Cache-Status".  
If the response was taken from the cache it will have a value of "HIT" otherwise if it was server by the server the value will be "MISS"
- The second options is to look at the Nginx Dashboard -> Caches and observe the HIT ration and traffic served