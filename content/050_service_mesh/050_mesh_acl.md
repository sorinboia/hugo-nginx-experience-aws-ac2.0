+++
title = "Mesh Access List"
date = 2020-07-08T14:37:59+03:00
weight = 50
+++


The Kubernetes networking environment by default is an open network without restrictions or limitations.  
This means that any pod can access any other pod or service. This introduces potential risks.  
Our `arcadia-users` exposes an internal API `/v1/user_i/` which is used by the `arcadia-login` service only. This means that no other service should have access to it. 
Lets see what security implications this has:

1. Access the `arcadia-stocks` pod. This pod is used only to get current crypto prices and nothing else. It should not be able to contact any other pod.

```
export arcadia_stocks_pod=$(kubectl get pods --selector=app=arcadia-stocks | grep arcadia-stocks -m 1 |  cut -d' ' -f1)
kubectl exec -it $arcadia_stocks_pod  -- bash
```

{{< output >}}
Defaulting container name to arcadia-stocks.
Use 'kubectl describe pod/arcadia-stocks-7bd6ff78c8-pb6bn -n default' to see all of the containers in this pod.
root@arcadia-stocks-7bd6ff78c8-pb6bn:/usr/src/app#
{{< /output >}}


2. You are now in the `arcadia-stocks` container, the bellow command will access the `arcadia-users` internal API and get specific user information.  
This should not be allowed. This call must be allowed only from the `arcadia-login` pods. 

```
curl http://arcadia-users/v1/user_i/c29yaW5AbmdpbnguY29t
```

{{< output >}}
{"_id":"6072b09ce6915d87d36e66af","accountId":"47808892","name":"Sorin Boiangiu","email":"sorin@nginx.com","cash":121973.826,"password":"nginx","stocks":{"btc":1.1989999999999994,"eth":3.5,"ltc":40.1},"picture":"default"}
{{< /output >}}

`exit` the container bash.

3. Improve you application security by introducing these checks and allowing access only from the `arcadia-login` pods

```
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: arcadia-users-sa
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: arcadia-login-sa
---
apiVersion: specs.smi-spec.io/v1alpha3
kind: HTTPRouteGroup
metadata:
  name: route-group
spec:
  matches:
    - name: destination-traffic
      methods:
        - GET

---
apiVersion: access.smi-spec.io/v1alpha2
kind: TrafficTarget
metadata:
  name: traffic-target
spec:
  destination:
    kind: ServiceAccount
    name: arcadia-users-sa
  rules:
    - kind: HTTPRouteGroup
      name: route-group
      matches:
        - destination-traffic
  sources:
    - kind: ServiceAccount
      name: arcadia-login-sa
EOF
```

4. Attach the service account to the pods
```
kubectl set serviceaccount deployments/arcadia-users arcadia-users-sa
kubectl set serviceaccount deployments/arcadia-login arcadia-login-sa
```

5. Access the `arcadia-stocks` pod and retry getting user information from the `arcadia-users` service, this time it will be blocked.

```
export arcadia_stocks_pod=$(kubectl get pods --selector=app=arcadia-stocks | grep arcadia-stocks -m 1 |  cut -d' ' -f1)
kubectl exec -it $arcadia_stocks_pod  -- bash
```

```
curl http://arcadia-users/v1/user_i/c29yaW5AbmdpbnguY29t
```

{{< output >}}
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.19.5</center>
</body>
</html>
{{< /output >}}

`exit` the container bash.

6. Access the `arcadia-login` pod and try getting user information from the `arcadia-users` service, only this service should be allowed.

```
export arcadia_login_pod=$(kubectl get pods --selector=app=arcadia-login | grep arcadia-login -m 1 |  cut -d' ' -f1)
kubectl exec -it $arcadia_login_pod  -- bash
```

```
curl http://arcadia-users/v1/user_i/c29yaW5AbmdpbnguY29t
```

{{< output >}}
{"_id":"6072b09ce6915d87d36e66af","accountId":"47808892","name":"Sorin Boiangiu","email":"sorin@nginx.com","cash":121973.826,"password":"nginx","stocks":{"btc":1.1989999999999994,"eth":3.5,"ltc":40.1},"picture":"default"}
{{< /output >}}

`exit` the container bash.