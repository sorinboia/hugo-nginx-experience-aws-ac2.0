+++
title = "Side Car Injection"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++



1. Inject the sidecar into the application pods

```
kubectl get deployment arcadia-frontend -oyaml | nginx-meshctl inject | kubectl apply -f -
kubectl get deployment arcadia-login -oyaml | nginx-meshctl inject | kubectl apply -f -
kubectl get deployment arcadia-stock-transaction -oyaml | nginx-meshctl inject | kubectl apply -f -
kubectl get deployment arcadia-stocks -oyaml | nginx-meshctl inject | kubectl apply -f -
kubectl get deployment arcadia-users -oyaml | nginx-meshctl inject | kubectl apply -f -
```

2. Wait for the sidecar to be injected in all pods and verify that the pods have 2 containers

```
kubectl get pods
```

{{< output >}}
NAME                                         READY   STATUS    RESTARTS   AGE
arcadia-db-6cb9cfcb44-l2v26                  1/1     Running   0          85m
arcadia-frontend-59b7c8d784-4xxvf            2/2     Running   0          6m25s
arcadia-frontend-59b7c8d784-nzz5b            2/2     Running   0          6m11s
arcadia-login-8599ff74b7-hg4mc               2/2     Running   0          6m11s
arcadia-login-8599ff74b7-sf4p4               2/2     Running   0          6m24s
arcadia-stock-transaction-684f86d649-5mzbf   2/2     Running   1          6m22s
arcadia-stock-transaction-684f86d649-nqk4s   2/2     Running   1          6m7s
arcadia-stocks-56ccc548d5-c48mf              2/2     Running   0          6m21s
arcadia-stocks-56ccc548d5-jzq64              2/2     Running   0          6m11s
arcadia-users-6df4f9fb7c-42dsr               2/2     Running   1          6m19s
arcadia-users-6df4f9fb7c-8jv8h               2/2     Running   1          6m8s
{{< /output >}}

3. Propagate the opentracing headers so we can get full visibility within the mesh

```
cat << EOF | kubectl apply -f -
apiVersion: k8s.nginx.org/v1
kind: VirtualServer
metadata:
  name: arcadia
spec:
  host: $nginx_ingress  
  tls:
    secret: arcadia-wildcard # Represents the server certificate
    redirect:
      enable: true # Always redirect to https if incoming request is http
  upstreams:
    - name: arcadia-users
      service: arcadia-users
      port: 80
      healthCheck: # This is the most basic healthcheck config for more info follow this link https://docs.nginx.com/nginx-ingress-controller/configuration/virtualserver-and-virtualserverroute-resources/#upstream-healthcheck
        enable: true
        path: /healthz
    - name: arcadia-login
      service: arcadia-login
      port: 80
      healthCheck:
        enable: true
        path: /healthz
    - name: arcadia-stocks
      service: arcadia-stocks
      port: 80
      healthCheck:
        enable: true
        path: /healthz
    - name: arcadia-stock-transaction
      service: arcadia-stock-transaction
      port: 80
      healthCheck: 
        enable: true
        path: /healthz
    - name: arcadia-frontend
      service: arcadia-frontend
      port: 80
      healthCheck:
        enable: true
        path: /healthz
  routes:
    - path: /v1/user      
      location-snippets: |
        opentracing_propagate_context;
        opentracing_operation_name "nginx-ingress";       
      policies:
      - name: jwt-policy
      action:
        proxy:
          upstream: arcadia-users
          requestHeaders:
            set:
            - name: okta-user
              value: \${jwt_claim_email}
    - path: /v1/login
      location-snippets: |
        opentracing_propagate_context;
        opentracing_operation_name "nginx-ingress";      
      action:
        pass: arcadia-login
    - path: /v1/stock
      location-snippets: |
        opentracing_propagate_context;
        opentracing_operation_name "nginx-ingress";      
      action:
        pass: arcadia-stocks
    - path: /v1/stockt
      location-snippets: |
        opentracing_propagate_context;
        opentracing_operation_name "nginx-ingress";      
      policies:
      - name: jwt-policy
      action:
        proxy:
          upstream: arcadia-stock-transaction
          requestHeaders:
            set:
            - name: okta-user
              value: \${jwt_claim_email}
    - path: /
      location-snippets: |
        opentracing_propagate_context;
        opentracing_operation_name "nginx-ingress";      
      action:
        pass: arcadia-frontend
EOF
```

