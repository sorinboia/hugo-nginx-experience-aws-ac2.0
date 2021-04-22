+++
title = "Side Car Injection"
date = 2020-07-08T14:37:59+03:00
weight = 30
+++



1. Inject the sidecar into the application pods

```
kubectl get deployment arcadia-frontend -oyaml | nsm inject | kubectl apply -f -
kubectl get deployment arcadia-login -oyaml | nsm inject | kubectl apply -f -
kubectl get deployment arcadia-stock-transaction -oyaml | nsm inject | kubectl apply -f -
kubectl get deployment arcadia-stocks -oyaml | nsm inject | kubectl apply -f -
kubectl get deployment arcadia-users -oyaml | nsm inject | kubectl apply -f -
```




